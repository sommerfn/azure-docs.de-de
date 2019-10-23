---
title: Erstellen einer Konsolen-App mit der API für MongoDB von Azure Cosmos DB und dem Golang SDK
description: Stellt ein Golang-Codebeispiel vor, mit dem Sie eine Verbindung herstellen und Abfragen über die API für MongoDB von Azure Cosmos DB durchführen können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: quickstart
ms.date: 12/26/2018
ms.openlocfilehash: e16b9b7e591fcc089d74794c98ddfc951cbdced9
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755114"
---
# <a name="quickstart-build-a-console-app-using-azure-cosmos-dbs-api-for-mongodb-and-golang-sdk"></a>Schnellstart: Erstellen einer Konsolen-App mit der API für MongoDB von Azure Cosmos DB und dem Golang SDK

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Cosmos DB zugrunde liegen.

Dieser Schnellstart zeigt, wie Sie eine vorhandene MongoDB-App, die in [Golang](https://golang.org/) geschrieben wurde, mit Ihrer Cosmos-Datenbank über die API für MongoDB von Azure Cosmos DB verbinden.

Anders ausgedrückt: Die Golang-Anwendung verfügt nur über Informationen darüber, dass sie mithilfe eines MongoDB-Clients eine Verbindung herstellt. Für die Anwendung ist es ersichtlich, dass die Daten in einer Cosmos-Datenbank gespeichert sind.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen. 

  [!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

- [Go](https://golang.org/dl/) und Grundkenntnisse der Sprache [Go](https://golang.org/).
- Eine integrierte Entwicklungsumgebung (IDE): [GoLand](https://www.jetbrains.com/go/) von Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) von Microsoft oder [Atom](https://atom.io/). In diesem Tutorial wird GoLand verwendet.

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie die Beispielanwendung, und installieren Sie die erforderlichen Pakete.

1. Erstellen Sie einen Ordner mit dem Namen „CosmosDBSample“ im Ordner „GOROOT\src“, der sich standardmäßig unter „C:\Go\“ befindet.
2. Führen Sie über ein Git-Terminalfenster, z.B. Git Bash, den folgenden Befehl aus, um das Beispielrepository im Ordner „CosmosDBSample“ zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  Führen Sie den unten angegebenen Befehl aus, um das mgo-Paket abzurufen. 

    ```
    go get gopkg.in/mgo.v2
    ```

Der [mgo](https://labix.org/mgo)-Treiber ist ein [MongoDB](https://www.mongodb.com/)-Treiber für die [Sprache Go](https://golang.org/), mit dem eine umfassende und gründlich getestete Auswahl von Funktionen unter einer sehr einfachen API auf Grundlage von Go-Standardbegriffen implementiert wird.

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im Navigationsmenü auf der linken Seite auf **Schnellstart** und dann auf **Andere**, um die Verbindungszeichenfolgen-Informationen anzuzeigen, die für die Go-Anwendung erforderlich sind.

2. Öffnen Sie in Goglang die Datei „main.go“ im Verzeichnis „GOROOT\CosmosDBSample“, und aktualisieren Sie die folgenden Codezeilen, indem Sie die Verbindungszeichenfolgen-Informationen aus dem Azure-Portal verwenden. Dies ist im folgenden Screenshot dargestellt. 

    Der Datenbankname ist das Präfix des Werts **Host** im Bereich des Azure-Portals mit der Verbindungszeichenfolge. Für das Konto, das in der Abbildung unten dargestellt ist, lautet der Datenbankname „golang-coach“.

    ```go
    Database: "The prefix of the Host value in the Azure portal",
    Username: "The Username in the Azure portal",
    Password: "The Password in the Azure portal",
    ```

    ![Schnellstart-Bereich: Registerkarte „Andere“ im Azure-Portal mit den Verbindungszeichenfolgen-Informationen](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. Speichern Sie die Datei „main.go“.

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Ausführen der App](#run-the-app) fortfahren. 

Die folgenden Codeausschnitte stammen alle aus der Datei „main.go“.

### <a name="connecting-the-go-app-to-cosmos-db"></a>Herstellen einer Verbindung der Go-App mit Cosmos DB

Die API für MongoDB von Azure Cosmos DB unterstützt die SSL-fähige Verbindung. Zum Herstellen einer Verbindung müssen Sie die Funktion **DialServer** in [mgo.DialInfo](https://godoc.org/gopkg.in/mgo.v2#DialInfo) definieren und die Funktion [tls.*Dial*](https://golang.org/pkg/crypto/tls#Dial) nutzen, um die Verbindung herzustellen.

Mit dem folgenden Golang-Codeausschnitt wird die Go-App mit der API für MongoDB von Azure Cosmos DB verbunden. Die *DialInfo*-Klasse enthält Optionen zum Einrichten einer Sitzung.

```go
// DialInfo holds options for establishing a session.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "database", // It can be anything
    Username: "username", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// to Cosmos database (using Azure Cosmos DB's API for MongoDB).
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes the session safety mode.
// If the safe parameter is nil, the session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. The unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

Die **mgo.Dial()** -Methode wird verwendet, wenn keine SSL-Verbindung vorhanden ist. Für eine SSL-Verbindung ist die **mgo.DialWithInfo()** -Methode erforderlich.

Eine Instanz des **DialWIthInfo{}** -Objekts wird zum Erstellen des Sitzungsobjekts verwendet. Nach dem Einrichten der Sitzung können Sie auf die Sammlung zugreifen, indem Sie den folgenden Codeausschnitt verwenden:

```go
collection := session.DB("database").C("package")
```

<a id="create-document"></a>

### <a name="create-a-document"></a>Erstellen eines Dokuments

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

### <a name="query-or-read-a-document"></a>Abfragen oder Lesen eines Dokuments

Cosmos DB unterstützt umfassende Abfragen der in jeder Sammlung gespeicherten Daten. Der folgende Beispielcode zeigt eine Abfrage, die Sie für die Dokumente in Ihrer Sammlung ausführen können.

```go
// Get a Document from the collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a>Aktualisieren eines Dokuments

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

### <a name="delete-a-document"></a>Löschen eines Dokuments

Cosmos DB unterstützt das Löschen von Dokumenten.

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-the-app"></a>Ausführen der App

1. Stellen Sie in Golang sicher, dass Ihr GOPATH (verfügbar unter **Datei**, **Einstellungen**, **Go**, **GOPATH**) den Speicherort enthält, an dem gopkg installiert wurde (standardmäßig unter „USERPROFILE\go“). 
2. Kommentieren Sie die Zeilen aus, mit denen das Dokument gelöscht wird (Zeilen 103 bis 107), damit Sie das Dokument nach dem Ausführen der App sehen können.
3. Klicken Sie in Golang auf **Run** (Ausführen) und anschließend auf **Run 'Build main.go and run'** (main.go erstellen und ausführen).

    Die App wird beendet, und die Beschreibung des Dokuments, das unter [Erstellen eines Dokuments](#create-document) erstellt wurde, wird angezeigt.
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Golang mit der Ausgabe der App](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a>Prüfen Ihres Dokuments im Daten-Explorer

Wechseln Sie zurück in das Azure-Portal, um Ihr Dokument im Daten-Explorer anzuzeigen.

1. Klicken Sie im Navigationsmenü auf der linken Seite auf **Daten-Explorer (Vorschau)** , erweitern Sie **golang-coach** und **package**, und klicken Sie dann auf **Dokumente**. Klicken Sie auf der Registerkarte **Dokumente** auf die „\_id“, um das Dokument im rechten Bereich anzuzeigen. 

    ![Daten-Explorer mit dem neu erstellten Dokument](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. Sie können dann inline mit dem Dokument arbeiten und auf **Aktualisieren** klicken, um es zu speichern. Außerdem können Sie das Dokument löschen oder neue Dokumente oder Abfragen erstellen.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein Cosmos-Konto erstellen und eine Golang-App ausführen. Jetzt können Sie zusätzliche Daten in Ihre Cosmos-Datenbank importieren. 

> [!div class="nextstepaction"]
> [Import MongoDB data into Azure Cosmos DB (Importieren von Daten aus MongoDB in Azure Cosmos DB)](mongodb-migrate.md)
