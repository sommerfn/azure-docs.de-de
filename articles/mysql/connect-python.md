---
title: 'Schnellstart: Herstellen einer Verbindung mit Azure Database for MySQL per Python'
description: Diese Schnellstartanleitung enthält mehrere Python-Codebeispiele, mit deren Hilfe Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen und Daten daraus abfragen können.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: seo-python-october2019
ms.devlang: python
ms.topic: quickstart
ms.date: 08/08/2019
ms.openlocfilehash: 9f9d432bc2f7d70396bca5d140f1a66a75fd7231
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72434309"
---
# <a name="quickstart-use-python-to-connect-and-query-data-with-azure-database-for-mysql"></a>Schnellstart: Herstellen einer Verbindung und Abfragen von Daten mithilfe von Python mit Azure Database for MySQL
In diesem Artikel erfahren Sie, wie Sie mithilfe von [Python](https://python.org) eine Verbindung mit einer Azure-Datenbank für MySQL herstellen. Dabei werden SQL-Anweisungen verwendet, um Daten in der Datenbank über Mac OS, Ubuntu Linux und Windows-Plattformen abzufragen, einzufügen, zu aktualisieren und zu löschen. 

Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der Python-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-the-mysql-connector"></a>Installieren von Python und dem MySQL-Connector
Installieren Sie [Python](https://www.python.org/downloads/) und den [MySQL-Connector für Python](https://dev.mysql.com/downloads/connector/python/) auf Ihrem eigenen Computer. Führen Sie je nach Plattform die folgenden Schritte im entsprechenden Abschnitt unten aus. 

> [!NOTE]
> In dieser Schnellstartanleitung wird eine unformatierte SQL-Abfrage verwendet, um zum Ausführen von Abfragen eine Verbindung mit MySQL herzustellen. Nutzen Sie bei Verwendung eines Webframeworks den empfohlenen Connector für diese Frameworks. Für die Nutzung mit Django wird beispielsweise [mysqlclient](https://pypi.org/project/mysqlclient/) empfohlen.
>

### <a name="windows"></a>Windows
1. Laden Sie Python 3.7 von [python.org](https://www.python.org/downloads/windows/) herunter, und installieren Sie die Software. 
2. Starten Sie die Eingabeaufforderung, um die Python-Installation zu überprüfen. Führen Sie den Befehl `C:\python37\python.exe -V` mit dem Switch „V“ (groß geschrieben) aus, um die Versionsnummer anzuzeigen.
3. Installieren Sie den für Ihre Python-Version passenden Python-Connector für MySQL (zu finden unter [mysql.com](https://dev.mysql.com/downloads/connector/python/)).

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Unter Linux (Ubuntu) wird Python in der Regel als Teil der Standardinstallation installiert.
2. Starten Sie zum Überprüfen der Python-Installation die Bash-Shell. Führen Sie den Befehl `python -V` mit dem Switch „V“ (groß geschrieben) aus, um die Versionsnummer anzuzeigen.
3. Führen Sie zum Überprüfen der PIP-Installation den `pip show pip -V` Befehl aus, um die Versionsnummer anzuzeigen. 
4. PIP ist unter Umständen in einigen Versionen von Python enthalten. Sollte PIP nicht installiert sein, können Sie das [PIP-Paket](https://pip.pypa.io/en/stable/installing/) durch Ausführen des Befehls `sudo apt-get install python-pip` installieren.
5. Führen Sie den Befehl `pip install -U pip` aus, um PIP auf die neueste Version zu aktualisieren.
6. Verwenden Sie den folgenden PIP-Befehl, um den MySQL-Connector für Python und die dazugehörigen Abhängigkeiten zu installieren:

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a>macOS
1. Unter Mac OS wird Python in der Regel im Rahmen der Standardinstallation des Betriebssystems installiert.
2. Starten Sie zum Überprüfen der Python-Installation die Bash-Shell. Führen Sie den Befehl `python -V` mit dem Switch „V“ (groß geschrieben) aus, um die Versionsnummer anzuzeigen.
3. Führen Sie zum Überprüfen der PIP-Installation den `pip show pip -V` Befehl aus, um die Versionsnummer anzuzeigen.
4. PIP ist unter Umständen in einigen Versionen von Python enthalten. Sollte PIP nicht installiert sein, können Sie das [PIP-Paket](https://pip.pypa.io/en/stable/installing/) installieren.
5. Führen Sie den Befehl `pip install -U pip` aus, um PIP auf die neueste Version zu aktualisieren.
6. Verwenden Sie den folgenden PIP-Befehl, um den MySQL-Connector für Python und die dazugehörigen Abhängigkeiten zu installieren:

   ```bash
   pip install mysql-connector-python-rf
   ``` 

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im linken Menü **Alle Ressourcen** aus, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **mydemoserver**).
3. Wählen Sie den Servernamen aus.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-python/azure-database-for-mysql-server-overview-name-login.png)

## <a name="run-python-code"></a>Ausführen des Python-Codes
- Fügen Sie den Code in eine Textdatei ein, und speichern Sie dann die Datei in einem Projektordner mit der Dateierweiterung „.py“ (Beispiele: „C:\pythonmysql\createtable.py“ oder „/home/username/pythonmysql/createtable.py“).
- Starten Sie zum Ausführen des Codes die Eingabeaufforderung oder die Bash-Shell. Wechseln Sie in das Verzeichnis Ihres Projektordners `cd pythonmysql`. Geben Sie anschließend den Python-Befehl gefolgt vom Dateinamen `python createtable.py` ein, um die Anwendung auszuführen. Sollte „python.exe“ unter Windows nicht gefunden werden, müssen Sie ggf. den vollständigen Pfad zur ausführbaren Datei angeben oder den Python-Pfad in die Umgebungsvariable „path“ einschließen. `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung mit dem Server herzustellen, eine Tabelle zu erstellen und die Daten unter Verwendung einer SQL-Anweisung des Typs **INSERT** zu laden. 

Im Code wird die Bibliothek „mysql.connector“ importiert. Die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wird verwendet, um unter Verwendung der [Verbindungsargumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung eine Verbindung mit Azure-Datenbank für MySQL herzustellen. Im Code wird ein Cursor für die Verbindung verwendet, und die [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wendet die SQL-Abfrage auf die MySQL-Datenbank an. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'mydemoserver.mysql.database.azure.com',
  'user':'myadmin@mydemoserver',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **SELECT** zu verbinden und zu lesen. 

Im Code wird die Bibliothek „mysql.connector“ importiert. Die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wird verwendet, um unter Verwendung der [Verbindungsargumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung eine Verbindung mit Azure-Datenbank für MySQL herzustellen. Im Code wird ein Cursor für die Verbindung verwendet, und die [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wendet die SQL-Anweisung auf die MySQL-Datenbank an. Die Datenzeilen werden unter Verwendung der Methode [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) gelesen. Das Resultset wird in einer Auflistungszeile gespeichert, und die Zeilen werden mithilfe eines for-Iterators durchlaufen.

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'mydemoserver.mysql.database.azure.com',
  'user':'myadmin@mydemoserver',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **UPDATE** zu verbinden und zu aktualisieren. 

Im Code wird die Bibliothek „mysql.connector“ importiert.  Die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wird verwendet, um unter Verwendung der [Verbindungsargumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung eine Verbindung mit Azure-Datenbank für MySQL herzustellen. Im Code wird ein Cursor für die Verbindung verwendet, und die [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wendet die SQL-Anweisung auf die MySQL-Datenbank an. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'mydemoserver.mysql.database.azure.com',
  'user':'myadmin@mydemoserver',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten mithilfe einer SQL-Anweisung des Typs **DELETE** zu entfernen. 

Im Code wird die Bibliothek „mysql.connector“ importiert.  Die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wird verwendet, um unter Verwendung der [Verbindungsargumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung eine Verbindung mit Azure-Datenbank für MySQL herzustellen. Im Code wird ein Cursor für die Verbindung verwendet, und die [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wendet die SQL-Abfrage auf die MySQL-Datenbank an. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'mydemoserver.mysql.database.azure.com',
  'user':'myadmin@mydemoserver',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./concepts-migrate-import-export.md)
