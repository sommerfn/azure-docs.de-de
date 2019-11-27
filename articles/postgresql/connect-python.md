---
title: Verwenden von Python zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver
description: Diese Schnellstartanleitung enthält Python-Codebeispiele, die Sie verwenden können, um eine Verbindung mit einem Azure Database for PostgreSQL-Einzelserver herzustellen und Daten daraus abzufragen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc, devcenter
ms.devlang: python
ms.topic: quickstart
ms.date: 11/07/2019
ms.openlocfilehash: 441ff1ebeffde36d1940520404050f6cc29ea53e
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74066293"
---
# <a name="use-python-to-connect-and-query-data-in-azure-database-for-postgresql---single-server"></a>Verwenden von Python zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver sowie zum Abfragen von Daten
In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python unter macOS, Ubuntu Linux oder Windows mit Azure Database for PostgreSQL arbeiten. Die Schnellstartanleitung zeigt, wie Sie eine Verbindung mit der Datenbank herstellen und SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit Python vertraut sind, aber noch keine Erfahrung mit Azure Database for PostgreSQL haben.

## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure Database for PostgreSQL-Einzelserver, der mithilfe der Schritte unter [Schnellstart: Erstellen eines Azure Database for PostgreSQL-Servers im Azure-Portal](quickstart-create-server-database-portal.md) oder [Schnellstart: Erstellen eines Azure Database for PostgreSQL-Einzelservers mit der Azure CLI](quickstart-create-server-database-azure-cli.md) erstellt wurde. 
  
- [Python](https://www.python.org/downloads/) 2.7.9+ oder 3.4+.
  
- Das neueste Update des Paketinstallationsprogramms [pip](https://pip.pypa.io/en/stable/installing/) (installiert mit `pip install -U pip`). 

## <a name="install-the-python-libraries-for-postgresql"></a>Installieren der Python-Bibliotheken für PostgreSQL
Das Modul [psycopg2](https://pypi.python.org/pypi/psycopg2/) ermöglicht das Herstellen einer Verbindung mit einer PostgreSQL-Datenbank sowie das Abfragen von Daten aus dieser Datenbank und ist als [wheel-Paket](https://pythonwheels.com/) für Linux, macOS oder Windows verfügbar. Installieren Sie die Binärversion des Moduls mit allen Abhängigkeiten. Weitere Informationen zur Installation von `psycopg2` sowie zu den Anforderungen finden Sie unter [Installation](http://initd.org/psycopg/docs/install.html). 

Öffnen Sie zum Installieren von `psycopg2` ein Terminal oder eine Eingabeaufforderung, und führen Sie den Befehl `pip install psycopg2` aus.

## <a name="get-database-connection-information"></a>Abrufen von Datenbankverbindungsinformationen
Für die Verbindungsherstellung mit einer Azure Database for PostgreSQL-Datenbank sind der vollqualifizierte Servername und Anmeldeinformationen erforderlich. Diese Informationen finden Sie im Azure-Portal.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach dem Namen Ihres Azure Database for PostgreSQL-Servers, und wählen Sie ihn aus. 
1. Kopieren Sie auf der **Übersichtsseite** des Servers unter **Servername** den vollqualifizierten Servernamen und unter **Administratorbenutzername** den Administratorbenutzernamen. Der vollqualifizierte **Servername** hat immer das Format *\<Servername>.postgres.database.azure.com*. Der **Administratorbenutzername** hat immer das Format *\<Administratorbenutzername>@\<Servername>* . 
   
   Darüber hinaus benötigen Sie Ihr Administratorkennwort. Sollten Sie es vergessen, können Sie es auf dieser Seite zurücksetzen. 
   
   ![Name des Azure Database for PostgreSQL-Servers](./media/connect-python/1-connection-string.png)

## <a name="how-to-run-the-python-examples"></a>Ausführen der Python-Beispiele

Führen Sie für jedes Codebeispiel in diesem Artikel die folgenden Schritte aus:

1. Erstellen Sie eine neue Datei in einem Text-Editor. 
   
1. Fügen Sie der Datei das Codebeispiel hinzu. Ersetzen Sie im Code Folgendes:
   - `<server-name>` und `<admin-username>` durch die Werte, die Sie aus dem Azure-Portal kopiert haben.
   - `<admin-password>` durch Ihr Serverkennwort.
   - `<database-name>` durch den Namen Ihrer Azure Database for PostgreSQL-Datenbank. Bei der Servererstellung wurde automatisch eine Standarddatenbank namens *postgres* erstellt. Sie können diese Datenbank umbenennen oder mithilfe von SQL-Befehlen eine neue Datenbank erstellen. 
   
1. Speichern Sie die Datei mit der Erweiterung *.py* in Ihrem Projektordner (Beispiel: *postgres-insert.py*). Achten Sie unter Windows darauf, dass beim Speichern die UTF-8-Codierung ausgewählt ist. 
   
1. Wechseln Sie zum Ausführen der Datei mithilfe einer Befehlszeilenschnittstelle zu Ihrem Projektordner, und geben Sie `python` sowie den Dateinamen ein (Beispiel: `python postgres-insert.py`).

## <a name="create-a-table-and-insert-data"></a>Erstellen einer Tabelle und Einfügen von Daten
Im folgenden Codebeispiel wird mithilfe der Funktion [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) eine Verbindung mit Ihrer Azure Database for PostgreSQL-Datenbank hergestellt, und es werden Daten mithilfe einer SQL-Anweisung vom Typ **INSERT** geladen. Die Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) führt die SQL-Abfrage für die Datenbank aus. 

```Python
import psycopg2

# Update connection string information 
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print("Finished dropping table (if existed)")

# Create a table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print("Finished creating table")

# Insert some data into the table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print("Inserted 3 rows of data")

# Clean up
conn.commit()
cursor.close()
conn.close()
```

Nach erfolgreicher Ausführung generiert der Code die folgende Ausgabe:

![Befehlszeilenausgabe](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Lesen von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL-Datenbank hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **SELECT** verwendet, um Daten zu lesen. Diese Funktion akzeptiert eine Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) durchlaufen werden kann. 

```Python
import psycopg2

# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Aktualisieren von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL-Datenbank hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **UPDATE** verwendet, um Daten zu aktualisieren. 

```Python
import psycopg2

# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print("Updated 1 row of data")

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Löschen von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL-Datenbank hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **DELETE** verwendet, um ein zuvor eingefügtes Bestandselement zu löschen. 

```Python
import psycopg2

# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print("Deleted 1 row of data")

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
