---
title: Verwenden von Node.js zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver
description: Dieser Schnellstart enthält ein Node.js-Codebeispiel, das Sie nutzen können, um eine Verbindung mit einem Azure Database for PostgreSQL-Einzelserver herzustellen und Daten daraus abzufragen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 5/6/2019
ms.openlocfilehash: 44d99a9420fc33bdd01c05fdb04d94671b7c815b
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592340"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-in-azure-database-for-postgresql---single-server"></a>Schnellstart: Verwenden von Node.js zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver und zum Abfragen von Daten
Dieser Schnellstart zeigt, wie Sie mit einer [Node.j](https://nodejs.org/)s-Anwendung eine Verbindung mit Azure Database for PostgreSQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for PostgreSQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – CLI](quickstart-create-server-database-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren von [Node.js](https://nodejs.org)

## <a name="install-pg-client"></a>Installieren des pg-Clients
Installieren Sie [pg](https://www.npmjs.com/package/pg), den PostgreSQL-Client für Node.js.

Führen Sie hierzu den Knoten-Paket-Manager (npm) für JavaScript an der Befehlszeile aus, um den pg-Client zu installieren.
```bash
npm install pg
```

Überprüfen Sie die Installation, indem Sie die installierten Pakete auflisten.
```bash
npm list
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im linken Menü **Alle Ressourcen** aus, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **mydemoserver**).
3. Wählen Sie den Servernamen aus.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Azure Database for PostgreSQL-Verbindungszeichenfolge](./media/connect-nodejs/server-details-azure-database-postgresql.png)

## <a name="running-the-javascript-code-in-nodejs"></a>Ausführen des JavaScript-Codes in Node.js
Sie können Node.js über die Bash-Shell, das Terminal oder eine Windows-Eingabeaufforderung starten, indem Sie `node` eingeben und anschließend den JavaScript-Beispielcode interaktiv ausführen. Kopieren Sie ihn zu diesem Zweck in die Eingabeaufforderung. Alternativ können Sie den JavaScript-Code in einer Textdatei speichern und `node filename.js` mit dem Dateinamen als Parameter für die Ausführung starten.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden.
Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        UPDATE inventory 
        SET quantity= 1000 WHERE name='banana';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Update completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) {
        throw err;
    } else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DELETE FROM inventory 
        WHERE name = 'apple';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Delete completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
