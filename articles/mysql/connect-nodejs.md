---
title: Herstellen einer Verbindung mit Azure Database for MySQL per Node.js
description: Diese Schnellstartanleitung enthält mehrere Node.js-Codebeispiele, mit deren Hilfe Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen und Daten daraus abfragen können.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/21/2018
ms.openlocfilehash: fb61a976e62c3ae5e29dfcc5e28b48f2ea4214c8
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529052"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-in-azure-database-for-mysql"></a>Schnellstart: Verwenden von Node.js zum Herstellen von Verbindungen mit Daten und Abfragen von Daten in Azure Database for MySQL
In diesem Schnellstart wird gezeigt, wie Sie eine Verbindung mit einer Azure-Datenbank für MySQL per [Node.js](https://nodejs.org/) auf Windows-, Ubuntu Linux- und Mac-Plattformen herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installation der [Node.js](https://nodejs.org)-Laufzeit
- Installation des [mysql](https://www.npmjs.com/package/mysql)-Pakets zum Herstellen der Verbindung mit MySQL für die Node.js-Anwendung 

## <a name="install-nodejs-and-the-mysql-connector"></a>Installation von Node.js und MySQL-Connector
Befolgen Sie für die Installation von Node.js je nach Plattform die Anweisungen im entsprechende Abschnitt. Verwenden Sie npm, um das mysql-Paket und die Abhängigkeiten in Ihrem Projektordner zu installieren.

### <a name="windows"></a>**Windows**
1. Besuchen Sie die [Node.js-Seite „Downloads“](https://nodejs.org/en/download/), und wählen Sie dann die gewünschte Windows Installer-Option aus.
2. Erstellen Sie einen lokalen Projektordner, z.B. `nodejsmysql`. 
3. Öffnen Sie die Eingabeaufforderung, und wechseln Sie zum Projektordner, z.B. mit `cd c:\nodejsmysql\`.
4. Führen Sie das NPM-Tool aus, um die mysql-Bibliothek im Projektordner zu installieren.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql
   "C:\Program Files\nodejs\npm" list
   ```

5. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von `npm list` ansehen. Die Versionsnummer kann hiervon abweichen, wenn neue Patches veröffentlicht werden.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Führen Sie die folgenden Befehle zum Installieren von **Node.js** und **npm** (Paket-Manager für Node.js) aus.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Führen Sie die folgenden Befehle aus, um den Projektordner `mysqlnodejs` zu erstellen und das mysql-Paket in diesem Ordner zu installieren.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```
3. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von „npm list“ ansehen. Die Versionsnummer kann hiervon abweichen, wenn neue Patches veröffentlicht werden.

### <a name="mac-os"></a>**Mac OS**
1. Geben Sie die folgenden Befehle zum Installieren von **brew** ein, einem einfach zu verwendenden Paket-Manager für Mac OS X und **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Führen Sie die folgenden Befehle aus, um den Projektordner `mysqlnodejs` zu erstellen und das mysql-Paket in diesem Ordner zu installieren.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```

3. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von `npm list` ansehen. Die Versionsnummer kann hiervon abweichen, wenn neue Patches veröffentlicht werden.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im linken Menü **Alle Ressourcen** aus, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **mydemoserver**).
3. Wählen Sie den Servernamen aus.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-nodejs/server-name-azure-database-mysql.png)

## <a name="running-the-javascript-code-in-nodejs"></a>Ausführen des JavaScript-Codes in Node.js
1. Fügen Sie den JavaScript-Code in Textdateien ein, und speichern Sie ihn dann in einem Projektordner mit der Dateierweiterung „.js“, z.B. „C:\nodejsmysql\createtable.js“ oder „/home/username/nodejsmysql/createtable.js“.
2. Öffnen Sie die Eingabeaufforderung oder die Bash-Shell, und wechseln Sie zu Ihrem Projektordner: `cd nodejsmysql`.
3. Um die Anwendung auszuführen, geben Sie den node-Befehl gefolgt vom Dateinamen ein, z.B. `node createtable.js`.
4. Unter Windows müssen Sie unter Umständen den vollständigen Pfad verwenden, um die Node-Anwendung zu starten, z.B. `"C:\Program Files\nodejs\node.exe" createtable.js`, wenn sich die Node-Anwendung nicht unter dem Pfad der Umgebungsvariablen befindet.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden.

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die Funktion [connect()](https://github.com/mysqljs/mysql#establishing-connections) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [query()](https://github.com/mysqljs/mysql#performing-queries) wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **SELECT** zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. Das results-Array wird für die Ergebnisse der Abfrage verwendet.

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **UPDATE** zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **DELETE** zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./concepts-migrate-import-export.md)
