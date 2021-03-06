---
title: 'Benutzerdefinierte Java-Funktion (UDF) mit Apache Hive in HDInsight: Azure'
description: Hier erfahren Sie, wie Sie eine Java-basierte benutzerdefinierte Funktion (UDF) zur Verwendung mit Apache Hive erstellen. Diese benutzerdefinierte Beispielfunktion (UDF) konvertiert eine Tabelle von Textzeichenfolgen in Kleinbuchstaben.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 94e9a70707472eb94109ebcc404fd7a1a3074135
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575745"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Verwenden einer benutzerdefinierten Java-Funktion mit Apache Hive in HDInsight

Hier erfahren Sie, wie Sie eine Java-basierte benutzerdefinierte Funktion (UDF) zur Verwendung mit Apache Hive erstellen. Die benutzerdefinierte Java-Funktion (UDF) in diesem Beispiel konvertiert eine Tabelle von Textzeichenfolgen in klein geschriebene Zeichen.

## <a name="requirements"></a>Requirements (Anforderungen)

* Ein HDInsight-Cluster 

    > [!IMPORTANT]
    > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

    Die meisten Schritte in diesem Dokument funktionieren sowohl in Windows- als auch in Linux-basierten Clustern. Die Schritte zum Hochladen der kompilierten UDF in den Cluster und ihrer Ausführung sind jedoch spezifisch für Linux-basierte Cluster. Links zu Informationen, die mit Windows-basierten Clustern verwendet werden können, sind angegeben.

* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 oder höher (oder eine entsprechende Anwendung wie OpenJDK)

* [Apache Maven](https://maven.apache.org/)

* Ein Text-Editor oder eine Java-IDE

    > [!IMPORTANT]
    > Wenn Sie die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden, der als Zeilenende LF verwendet. Wenn Sie nicht sicher sind, ob der Editor LF oder CRLF verwendet, finden Sie im Abschnitt „Problembehandlung“ Schritte für das Entfernen des CR-Zeichens.

## <a name="create-an-example-java-udf"></a>Erstellen Sie einer Beispiel-UDF in Java 

1. Geben Sie an der Befehlszeile Folgendes an, um ein neues Maven-Projekt zu erstellen:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Wenn Sie PowerShell verwenden, müssen Sie die Parameter in Anführungszeichen setzen. Beispiel: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Mit diesem Befehl wird ein Verzeichnis mit dem Namen **exampleudf** erstellt, das das Maven-Projekt enthält.

2. Nachdem das Projekt erstellt wurde, löschen Sie das Verzeichnis **exampleudf/src/test**, das als Teil des Projekts erstellt wurde.

3. Öffnen Sie die Datei **exampleudf/pom.xml**, und ersetzen Sie den vorhandenen Eintrag `<dependencies>` durch folgenden XML-Code:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    Diese Einträge geben die Version von Hadoop und Hive an, die in HDInsight 3.6 enthalten sind. Informationen zu den in HDInsight enthaltenen Versionen von Hadoop und Hive finden Sie im Dokument zu den [Versionen von HDInsight-Komponenten](../hdinsight-component-versioning.md) .

    Fügen Sie am Ende der Datei den Abschnitt `<build>` der Zeile `</project>` hinzu. Dieser Abschnitt sollte folgenden XML-Code enthalten:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.6  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    ```

    Diese Einträge definieren, wie das Projekt erstellt werden soll. Insbesondere die Version von Java, die das Projekt verwendet, und wie Sie ein „Uberjar“ für die Bereitstellung im Cluster zu erstellen.

    Speichern Sie die Datei, nachdem die Änderungen erfolgt sind.

4. Benennen Sie **exampleudf/src/main/java/com/microsoft/examples/App.java** in **ExampleUDF.java** um, und öffnen Sie dann die Datei im Editor.

5. Ersetzen Sie den Inhalt der Datei **ExampleUDF.java** durch Folgendes, und speichern Sie dann die Datei.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Mit diesem Code wird eine UDF implementiert, die einen Zeichenfolgenwert verwendet und die Zeichenfolge in Kleinbuchstaben zurückgibt.

## <a name="build-and-install-the-udf"></a>Erstellen und Installieren der UDF

1. Führen Sie den folgenden Befehl aus, um die UDF zu kompilieren und zu packen:

    ```bash
    mvn compile package
    ```

    Dieser Befehl erstellt und packt die benutzerdefinierte Funktion in der Datei `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar`.

2. Kopieren Sie die Datei mit dem Befehl `scp` in den HDInsight-Cluster.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `myuser` durch das SSH-Benutzerkonto für Ihren Cluster. Ersetzen Sie `mycluster` durch den Namen des Clusters. Wenn Sie zum Schutz des SSH-Kontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um die Datei mit dem privaten Schlüssel anzugeben.

3. Stellen Sie mithilfe von SSH eine Verbindung zum Cluster her.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Kopieren Sie in der SSH-Sitzung die JAR-Datei in HDInsight-Speicher.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>Verwenden der UDF in Hive

1. Verwenden Sie den folgenden Befehl, um den Beeline-Client in der SSH-Sitzung zu starten.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Dieser Befehl setzt voraus, dass Sie die Standardeinstellung **admin** für das Anmeldekonto Ihres Clusters verwendet haben.

2. Sobald Sie bei der Eingabeaufforderung `jdbc:hive2://localhost:10001/>` ankommen, geben Sie Folgendes ein, um die UDF zu Hive hinzuzufügen und sie als Funktion verfügbar zu machen.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > In diesem Beispiel wird davon ausgegangen, dass Azure Storage als Standardspeicher für den Cluster verwendet wird. Wenn im Cluster stattdessen Data Lake Storage Gen2 verwendet wird, ändern Sie den Wert `wasb:///` in `abfs:///`. Wenn im Cluster stattdessen Data Lake Storage Gen1 verwendet wird, ändern Sie den Wert `wasb:///` in `adl:///`.

3. Verwenden Sie die UDF, um aus einer Tabelle abgerufene Werte in Zeichenfolgen mit Kleinbuchstaben zu konvertieren.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Durch diese Abfrage wird die Geräteplattform (Android, Windows, iOS usw.) in der Tabelle ausgewählt, die Zeichenfolge in Kleinbuchstaben umgewandelt und dann angezeigt. Die Ausgabe sieht in etwa wie folgt aus:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Nächste Schritte

Andere Möglichkeiten zum Arbeiten mit Hive finden Sie unter [Verwenden von Apache Hive mit HDInsight](hdinsight-use-hive.md).

Weitere Informationen zu benutzerdefinierten Hive-Funktionen finden Sie im Abschnitt [Apache Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) (Apache Hive-Operatoren und benutzerdefinierte Funktionen) des Hive-Wikis auf „apache.org“.
