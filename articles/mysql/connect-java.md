---
title: Herstellen einer Verbindung mit Azure Database for MySQL mithilfe von Java
description: Dieser Schnellstart enthält ein Java-Codebeispiel, das Sie nutzen können, um mit den Daten von Azure Database for MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devcenter, seo-java-july2019
ms.topic: quickstart
ms.devlang: java
ms.date: 08/08/2019
ms.openlocfilehash: 5dbac4cee4df43dfc2f0ebf9fdb5e2e7b7d3192c
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931598"
---
# <a name="azure-database-for-mysql-use-java-to-connect-and-query-data"></a>Azure Database for MySQL: Verwenden von Java zum Herstellen einer Verbindung und Abfragen von Daten
Diese Schnellstartanleitung veranschaulicht, wie Sie unter Verwendung einer Java-Anwendung und des JDBC-Treibers [MariaDB Connector/J](https://mariadb.com/kb/en/library/mariadb-connector-j/) eine Verbindung mit einer Azure Database for MySQL-Instanz herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit der Java-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
1. In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
   - [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
   - [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

2. Stellen Sie sicher, dass die Sicherheit Ihrer Azure-Datenbank für MySQL-Verbindung so konfiguriert ist, dass die Firewall geöffnet ist und SSL-Einstellungen für Ihre Anwendung angepasst wurden, damit die Verbindungsherstellung erfolgreich ist.

3. Rufen Sie den Connector „MariaDB Connector/J“ mit einer der folgenden Methoden ab:
   - Verwenden Sie das Maven-Paket [mariadb-java-client](https://search.maven.org/search?q=a:mariadb-java-client), um die [mariadb-java-client-Abhängigkeit](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client) in die POM-Datei für Ihr Projekt aufzunehmen.
   - Laden Sie den JDBC-Treiber [MariaDB Connector/J](https://downloads.mariadb.org/connector-java/) herunter, und nehmen Sie die JDBC-JAR-Datei (beispielsweise „mariadb-java-client-2.4.3.jar“) in den Klassenpfad Ihrer Anwendung auf. Ausführlichere Informationen zu Klassenpfaden finden Sie bei Bedarf in der Dokumentation der Umgebung (beispielsweise [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) oder [Java SE](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)).

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie dann nach dem soeben erstellten Server, z.B. **mydemoserver**.
3. Klicken Sie auf den Servernamen.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-java/1_server-overview-name-login.png)

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um die Verbindung herzustellen und die Daten zu laden, indem Sie die Funktion mit einer **INSERT**-SQL-Anweisung nutzen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Mit den Methoden [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) und „execute()“ wird die Tabelle verworfen und erstellt. Mit dem „prepareStatement“-Objekt werden die Einfügebefehle erstellt, und „setString()“ und „setInt()“ werden zum Binden der Parameterwerte genutzt. Mit der „executeUpdate()“-Methode wird der Befehl für jeden Parametersatz ausgeführt, um die Werte einzufügen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
                
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu lesen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Die Methoden [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) und „executeQuery()“ werden verwendet, um die SELECT-Anweisung zu verbinden und auszuführen. Die Ergebnisse werden mit einem ResultSet-Objekt verarbeitet. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu ändern. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Die Methoden [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) und „executeUpdate()“ werden verwendet, um die Verbindung für die UPDATE-Anweisung herzustellen und sie vorzubereiten und auszuführen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um Daten mit einer **DELETE**-SQL-Anweisung zu entfernen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen.  Die Methoden [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) und „executeUpdate()“ werden verwendet, um DELETE-Anweisung vorzubereiten und auszuführen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";
        
        // check that the driver is installed
        try
        {
            Class.forName("org.mariadb.jdbc");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MariaDB JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MariaDB JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)
