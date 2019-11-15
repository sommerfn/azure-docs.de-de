---
title: Verwenden von Java für Abfragen
description: Hier erfahren Sie, wie Sie unter Verwendung von Java ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von T-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-java-july2019. seo-java-august2019
ms.openlocfilehash: 6d4d9353e29a29b0cd6db7575e49a00a213355d3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827047"
---
# <a name="quickstart-use-java-to-connect-to-and-query-an-azure-sql-database"></a>Schnellstart: Herstellen einer Verbindung mit einer Azure SQL-Datenbank und Abfragen der Datenbank mit Java

In diesem Artikel erfahren Sie, wie Sie mithilfe von [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Sie können dann T-SQL-Anweisungen zum Abfragen von Daten verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Ausführen dieses Beispiels sicher, dass die folgenden erforderlichen Komponenten vorhanden sind:

- Eine Azure SQL-Datenbank. In den folgenden Schnellstartanleitungen erfahren Sie jeweils, wie Sie eine Datenbank in Azure SQL-Datenbank erstellen und anschließend konfigurieren:

  || Einzeldatenbank | Verwaltete Instanz |
  |:--- |:--- |:---|
  | Erstellen| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [BEFEHLSZEILENSCHNITTSTELLE (CLI)](scripts/sql-database-create-and-configure-database-cli.md) | [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Konfigurieren | [IP-Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md)| [Verbindung von einem virtuellen Computer](sql-database-managed-instance-configure-vm.md)|
  |||[Verbindung von einer lokalen Ressource](sql-database-managed-instance-configure-p2s.md)
  |Laden von Daten|Laden von Adventure Works gemäß Schnellstartanleitung|[Wiederherstellen von Wide World Importers](sql-database-managed-instance-get-started-restore.md)
  |||Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](sql-database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  
  > [!IMPORTANT]
  > Die Skripts in diesem Artikel sind für die Adventure Works-Datenbank geschrieben. Bei einer verwalteten Instanz müssen Sie entweder die Adventure Works-Datenbank in eine Instanzdatenbank importieren oder die Skripts in diesem Artikel zur Verwendung der Wide World Importers-Datenbank anpassen.

- Java-Software für Ihr Betriebssystem:

  - **MacOS:** Installieren Sie Homebrew und Java und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu:** Installieren Sie Java, das Java Development Kit und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3 und 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows:** Installieren Sie Java und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die Sie zum Herstellen einer Verbindung mit der Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **SQL-Datenbanken** aus, oder öffnen Sie die Seite **Verwaltete SQL-Instanzen**.

3. Sehen Sie sich auf der Seite **Übersicht** bei einer Einzeldatenbank den vollqualifizierten Servernamen neben **Servername** und bei einer verwalteten Instanz den vollqualifizierten Servernamen neben **Host** an. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus. 

## <a name="create-the-project"></a>Erstellen des Projekts

1. Erstellen Sie über die Eingabeaufforderung ein neues Maven-Projekt namens *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Wechseln Sie zum Ordner *sqltest*, und öffnen Sie *pom.xml* in Ihrem bevorzugten Text-Editor. Fügen Sie den Abhängigkeiten Ihres Projekts den **Microsoft JDBC-Treiber für SQL Server** hinzu. Verwenden Sie dazu den folgenden Code:

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Fügen Sie Ihrem Projekt in *pom.xml* auch die folgenden Eigenschaften hinzu. Sollte kein Abschnitt für Eigenschaften vorhanden sein, können Sie im Anschluss an die Abhängigkeiten einen entsprechenden Abschnitt hinzufügen.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Speichern und schließen Sie *pom.xml*.

## <a name="add-code-to-query-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

1. Ihr Maven-Projekt müsste bereits eine Datei namens *App.java* enthalten (unter

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*).

1. Öffnen Sie die Datei, und ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie anschließend die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

    ```java
    package com.sqldbsamples;

    import java.sql.Connection;
    import java.sql.Statement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.DriverManager;

    public class App {

        public static void main(String[] args) {

            // Connect to database
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
            Connection connection = null;

            try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName "
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";

                try (Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql)) {

                    // Print results from select statement
                    System.out.println("Top 20 categories:");
                    while (resultSet.next())
                    {
                        System.out.println(resultSet.getString(1) + " "
                            + resultSet.getString(2));
                    }
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > Im Codebeispiel wird die Beispieldatenbank **AdventureWorksLT** für Azure SQL verwendet.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die App an der Eingabeaufforderung aus.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)  

- [Microsoft JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)  
