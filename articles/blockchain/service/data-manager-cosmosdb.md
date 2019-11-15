---
title: Verwenden von Blockchain Data Manager zum Aktualisieren von Azure Cosmos DB
description: Verwenden von Blockchain Data Manager zum Senden von Blockchaindaten an Azure Cosmos DB
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chroyal
ms.openlocfilehash: 3f2d0df2c094d8455aa29e79ad3c6acc0aa52dd4
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585706"
---
# <a name="tutorial-use-blockchain-data-manager-to-send-data-to-azure-cosmos-db"></a>Tutorial: Verwenden von Blockchain Data Manager zum Senden von Daten an Azure Cosmos DB

In diesem Tutorial verwenden Sie den Blockchain Data Manager for Azure Blockchain Service, um Blockchaintransaktionsdaten in Azure Cosmos DB aufzuzeichnen. Blockchain Data Manager erfasst, transformiert und stellt Blockchainledgerdaten für Azure Event Grid-Themen bereit. Von Azure Event Grid aus verwenden Sie einen Azure Logic App-Connector, um Dokumente in einer Azure Cosmos DB-Datenbank zu erstellen. Wenn Sie mit dem Tutorial fertig sind, können Sie die Blockchaintransaktionsdaten im Azure Cosmos DB-Daten-Explorer untersuchen.

[![Details zur Blockchaintransaktion](./media/data-manager-cosmosdb/raw-msg.png)](./media/data-manager-cosmosdb/raw-msg.png#lightbox)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer Blockchain Data Manager-Instanz
> * Hinzufügen einer Blockchainanwendung zum Decodieren von Transaktionseigenschaften und Ereignissen
> * Erstellen eines Azure Cosmos DB-Kontos und einer -Datenbank zum Speichern von Transaktionsdaten
> * Erstellen einer Azure-Logik-App zum Verbinden eines Azure Event Grid-Themas mit Azure Cosmos DB
> * Senden einer Transaktion an einen Blockchainledger
> * Anzeigen der decodierten Transaktionsdaten in Azure Cosmos DB

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md)
* [Quickstart: Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe von Visual Studio Code](connect-vscode.md) durch. Im Schnellstart werden Schritt für Schritt die Installation des [Azure Blockchain Development Kit für Ethereum](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) und die Einrichtung Ihrer Blockchain-Entwicklungsumgebung beschrieben.
* Abschließen von [Tutorial: Erstellen und Bereitstellen von Smart Contracts mithilfe von Visual Studio Code](send-transaction.md). Das Tutorial führt Sie durch die Erstellung eines Beispiels für Smart Contract.
* Erstellen eines [Event Grid-Themas](../../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic)
* Weitere Informationen zu [Ereignishandlern in Azure Event Grid](../../event-grid/event-handlers.md)

## <a name="create-instance"></a>Erstellen einer Instanz

Eine Blockchain Data Manager-Instanz verbindet und überwacht einen Azure Blockchain Service-Transaktionsknoten. Eine Instanz erfasst alle Block- und Transaktionsrohdaten aus dem Transaktionsknoten. Eine ausgehende Verbindung sendet Blockchaindaten an Azure Event Grid. Beim Erstellen der Instanz konfigurieren Sie eine einzelne ausgehende Verbindung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu dem Azure Blockchain Service-Mitglied, das Sie in der Voraussetzung [Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal](create-member.md) erstellt haben. Wählen Sie **Blockchain Data Manager** aus.
1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen von Blockchain Data Manager](./media/data-manager-cosmosdb/add-instance.png)

    Geben Sie die folgenden Details ein:

    Einstellung | Beispiel | BESCHREIBUNG
    --------|---------|------------
    NAME | mywatcher | Geben Sie einen eindeutigen Namen für einen verbundenen Blockchain Data Manager ein.
    Transaktionsknoten | myblockchainmember | Wählen Sie den Standardtransaktionsknoten des Azure Blockchain Service-Mitglieds aus, das Sie in der Voraussetzung erstellt haben.
    Verbindungsname | cosmosdb | Geben Sie einen eindeutigen Namen für die ausgehende Verbindung ein, an die Blockchaintransaktionsdaten gesendet werden.
    Event Grid-Endpunkt | myTopic | Wählen Sie ein Event Grid-Thema aus, das Sie in der Voraussetzung erstellt haben. Hinweis: Die Blockchain Data Manager-Instanz und das Event Grid-Thema müssen sich in demselben Abonnement befinden.

1. Klicken Sie auf **OK**.

    Die Erstellung einer Blockchain Data Manager-Instanz dauert weniger als eine Minute. Nachdem die Instanz bereitgestellt wurde, wird sie automatisch gestartet. In einer laufenden Blockchain Data Manager-Instanz werden Blockchainereignisse aus dem Transaktionsknoten erfasst und Daten an Event Grid gesendet.

## <a name="add-application"></a>Anwendung hinzufügen

Fügen Sie die Blockchainanwendung **helloblockchain** hinzu, damit der Blockchain Data Manager den Ereignis- und Eigenschaftsstatus decodiert. Blockchain Data Manager erfordert die Smart Contract ABI und Bytecodedatei, um die Anwendung hinzuzufügen.

### <a name="get-contract-abi-and-bytecode"></a>Abrufen von Vertrags-ABI und Bytecode

Mit der Vertrags-ABI werden die Smart Contract-Schnittstellen definiert. Sie beschreibt, wie mit dem Smart Contract interagiert wird. Sie können das [Azure Blockchain Development Kit für Ethereum-Erweiterung](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) verwenden, um die Vertrags-ABI in die Zwischenablage zu kopieren.

1. Erweitern Sie im Explorer-Bereich von Visual Studio Code den Ordner **build/contracts** Ihres Solidity-Projekts **helloblockchain**, das Sie in der Voraussetzung [Tutorial: Erstellen und Bereitstellen von Smart Contracts mithilfe von Visual Studio Code](send-transaction.md) erstellt haben.
1. Klicken Sie mit der rechten Maustaste auf die Vertragsmetadaten-JSON-Datei. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Wählen Sie **Copy Contract ABI** (Vertrags-ABI kopieren) aus.

    ![Visual Studio Code-Bereich mit Auswahl von „Copy Contract ABI“ (Vertrags-ABI kopieren)](./media/data-manager-cosmosdb/abi-devkit.png)

    Die Vertrags-ABI wird in die Zwischenablage kopiert.

1. Speichern Sie das **abi**-Array als JSON-Datei. Beispiel: *abi.json*. Sie verwenden die Datei in einem späteren Schritt.

Blockchain Data Manager benötigt den bereitgestellten Bytecode für den Smart Contract. Der bereitgestellte Bytecode unterscheidet sich von Smart Contract-Bytecode. Sie können den bereitgestellten Bytecode aus der kompilierten Vertragsmetadatendatei abrufen.

1. Öffnen Sie die Vertragsmetadatendatei, die im Ordner **build/contracts** Ihres Solidity-Projekts enthalten ist. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Suchen Sie in der JSON-Datei nach dem Element **deployedBytecode**.
1. Kopieren Sie den Hexadezimalwert ohne die Anführungszeichen.

    ![Visual Studio Code-Bereich mit Bytecode in den Metadaten](./media/data-manager-portal/bytecode-metadata.png)

1. Speichern Sie den **bytecode**-Wert als JSON-Datei. Beispiel: *bytecode.json*. Sie verwenden die Datei in einem späteren Schritt.

Das folgende Beispiel zeigt die im VC Code-Editor geöffneten Dateien *abi.json* und *bytecode.json*. Ihre Dateien sollten ähnlich aussehen.

![Beispiel der Dateien „abi.json“ und „bytecode.json“](./media/data-manager-cosmosdb/contract-files.png)

### <a name="create-contract-abi-and-bytecode-url"></a>Erstellen von Vertrags-ABI- und Bytecode-URLs

Blockchain Data Manager erfordert, dass die Contract ABI- und Bytecodedateien beim Hinzufügen einer Anwendung über eine URL abrufbar sind. Sie können ein Azure Storage-Konto verwenden, um eine privat zugängliche URL bereitzustellen.

#### <a name="create-storage-account"></a>Speicherkonto erstellen

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

#### <a name="upload-contract-files"></a>Hochladen von Vertragsdateien

1. Erstellen Sie einen neuen Container für das Speicherkonto. Wählen Sie **Container > Container** aus.

    ![Erstellen eines Speicherkontocontainers](./media/data-manager-cosmosdb/create-container.png)

    | Einstellung | BESCHREIBUNG |
    |---------|-------------|
    | NAME  | Geben Sie dem Container einen Namen. Beispiel: *smartcontract* |
    | Öffentliche Zugriffsebene | Wählen Sie *Privat (kein anonymer Zugriff)* aus. |

1. Wählen Sie **OK** aus, um den Container zu erstellen.
1. Wählen Sie den Container und dann **Hochladen** aus.
1. Wählen Sie beide JSON-Dateien aus, die Sie im Abschnitt [Abrufen von Vertrags-ABI und Bytecode](#get-contract-abi-and-bytecode) erstellt haben.

    ![Blob hochladen](./media/data-manager-cosmosdb/upload-blobs.png)

    Wählen Sie die Option **Hochladen**.

#### <a name="generate-url"></a>Generieren der URL

Generieren Sie für jedes Blob eine Shared Access Signature.

1. Wählen Sie das ABI JSON-Blob aus.
1. Wählen Sie **SAS generieren** aus.
1. Wählen Sie den gewünschten Access Signature-Ablauf und dann **Blob-SAS-Token und URL generieren** aus.

    ![Generieren des SAS-Tokens](./media/data-manager-cosmosdb/generate-sas.png)

1. Kopieren Sie die **Blob-SAS-URL**, und speichern Sie sie für den nächsten Abschnitt.
1. Wiederholen Sie die Schritte unter [URL generieren](#generate-url) für das Bytecode-JSON-Blob.

### <a name="add-helloblockchain-application-to-instance"></a>Hinzufügen der helloblockchain-Anwendung zur Instanz

1. Wählen Sie Ihre Blockchain Data Manager-Instanz aus der Instanzliste aus.
1. Wählen Sie **Blockchainanwendungen** aus.
1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen einer Blockchainanwendung](./media/data-manager-cosmosdb/add-application.png)

    Geben Sie den Namen der Blockchainanwendung und die Smart Contract ABI- und Bytecode-URLs ein.

    Einstellung | BESCHREIBUNG
    --------|------------
    NAME | Geben Sie einen eindeutigen Namen zur Verfolgung der Blockchainanwendung ein.
    Vertrags-ABI | Der URL-Pfad zur Datei mit der Vertrags-ABI. Weitere Informationen finden Sie unter [Erstellen von Vertrags-ABI- und Bytecode-URLs](#create-contract-abi-and-bytecode-url).
    Vertragsbytecode | Der URL-Pfad zur Bytecodedatei. Weitere Informationen finden Sie unter [Erstellen von Vertrags-ABI- und Bytecode-URLs](#create-contract-abi-and-bytecode-url).

1. Klicken Sie auf **OK**.

    Nachdem die Anwendung erstellt wurde, wird sie in der Liste der Blockchainanwendungen angezeigt.

    ![Liste der Blockchainanwendungen](./media/data-manager-cosmosdb/artifact-list.png)

Sie können das Azure Storage-Konto löschen oder zur Konfiguration weiterer Blockchainanwendungen verwenden. Wenn Sie das Azure Storage-Konto löschen möchten, können Sie die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden sowohl das zugeordnete Speicherkonto als auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

## <a name="create-azure-cosmos-db"></a>Erstellen von Azure Cosmos DB

[!INCLUDE [cosmos-db-create-storage-account](../../../includes/cosmos-db-create-dbaccount.md)]

### <a name="add-a-database-and-container"></a>Hinzufügen einer Datenbank und eines Containers

Sie können mit dem Daten-Explorer im Azure-Portal eine Datenbank und einen Container erstellen.

1. Wählen Sie auf der Azure Cosmos DB-Kontoseite im linken Navigationsbereich die Option **Daten-Explorer** und dann **Neuer Container** aus.
1. Geben Sie im Bereich **Container hinzufügen** die Einstellungen für den neuen Container ein.

    ![Hinzufügen von Containereinstellungen](./media/data-manager-cosmosdb/add-container.png)

    | Einstellung | BESCHREIBUNG
    |---------|-------------|
    | Datenbank-ID | Geben Sie **blockchain-data** als Namen für die neue Datenbank ein. |
    | Throughput | Belassen Sie den Durchsatz bei **400** Anforderungseinheiten pro Sekunde (RU/s). Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten reduzieren möchten.|
    | Container-ID | Geben Sie **Messages** als Namen für den neuen Container ein. |
    | Partitionsschlüssel | Verwenden Sie **/MessageType** als Partitionsschlüssel. |

1. Klicken Sie auf **OK**. Im Daten-Explorer werden die neue Datenbank und der Container angezeigt, die Sie erstellt haben.

## <a name="create-logic-app"></a>Erstellen einer Logik-App

Azure Logic Apps hilft Ihnen beim Planen und Automatisieren von Geschäftsprozessen und Workflows, wenn Sie Systeme und Dienste integrieren müssen. Sie können eine Logik-App verwenden, um Event Grid mit Azure Cosmos DB zu verbinden.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** > **Integration** > **Logik-App** aus.
1. Geben Sie an, wo die Logik-App erstellt werden soll. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

    Weitere Informationen zum Erstellen von Logik-Apps finden Sie unter [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Nachdem Azure Ihre App bereitgestellt hat, wählen Sie Ihre Logik-App-Ressource aus.
1. Wählen Sie im Designer für Logik-Apps unter **Vorlagen** die Option **Leere Logik-App** aus.

### <a name="add-event-grid-trigger"></a>Hinzufügen eines Event Grid-Triggers

Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt die Logic Apps-Engine eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird. Verwenden Sie einen Azure Event Grid-Trigger, um Blockchaintransaktionsdaten von Event Grid an Cosmos DB zu senden.

1. Suchen Sie im Designer für Logik-Apps nach dem **Azure Event Grid**-Connector und wählen Sie ihn aus.
1. Wählen Sie auf der Registerkarte **Trigger** die Option **When a resource event occurs** (Bei Eintritt eines Ressourcenereignisses) aus.
1. Erstellen Sie eine API-Verbindung mit Ihrem Event Grid-Thema.

    ![Einstellungen für den Event Grid-Trigger](./media/data-manager-cosmosdb/event-grid-trigger.png)

    | Einstellung | BESCHREIBUNG
    |---------|-------------|
    | Subscription | Wählen Sie das Abonnement aus, das das Event Grid-Thema enthält. |
    | Ressourcentyp | Wählen Sie **Microsoft.EventGrid.Topics** aus. |
    | Ressourcenname | Wählen Sie den Namen des Event Grid-Themas aus, in dem Blockchain Data Manager Transaktionsdatennachrichten sendet. |

### <a name="add-cosmos-db-action"></a>Hinzufügen einer Cosmos DB-Aktion

Fügen Sie eine Aktion hinzu, um für jede Transaktion ein Dokument in Cosmos DB zu erstellen. Verwenden Sie den Transaktionsnachrichtentyp als Partitionsschlüssel, um die Nachrichten zu kategorisieren.

1. Wählen Sie **Neuer Schritt** aus.
1. Suchen Sie unter **Aktion auswählen** nach **Azure Cosmos DB**.
1. Wählen Sie **Azure Cosmos DB > Aktionen > Dokument erstellen oder aktualisieren** aus.
1. Erstellen Sie eine API-Verbindung mit Ihrer Cosmos DB-Datenbank.

    ![Cosmos DB-Verbindungseinstellungen](./media/data-manager-cosmosdb/cosmosdb-connection.png)

    | Einstellung | BESCHREIBUNG
    |---------|-------------|
    | Verbindungsname | Wählen Sie das Abonnement aus, das das Event Grid-Thema enthält. |
    | DocumentDB-Konto | Wählen Sie das DocumentDB-Konto aus, das Sie im Abschnitt [Azure Cosmos DB-Konto erstellen](#create-azure-cosmos-db) erstellt haben. |

1. Geben Sie die **Datenbank-ID** und **Sammlungs-ID** für Ihre Azure Cosmos DB ein, die Sie zuvor im Abschnitt [Hinzufügen einer Datenbank und eines Containers](#add-a-database-and-container) erstellt haben.

1. Wählen Sie die Einstellung **Dokument** aus. Wählen Sie im Popoutbereich *Dynamischen Inhalt hinzufügen* die Option **Ausdruck** aus, und fügen Sie den folgenden Ausdruck ein, nachdem Sie ihn kopiert haben:

    ```
    addProperty(triggerBody()?['data'], 'id', utcNow())
    ```

    Der Ausdruck ruft den Datenteil der Nachricht ab und legt die ID auf einen Zeitstempelwert fest.

1. Wählen Sie **Neuen Parameter hinzufügen** und anschließend **Partitionsschlüsselwert** aus.
1. Legen Sie den **Partitionsschlüsselwert** auf `"@{triggerBody()['data']['MessageType']}"` fest. Der Wert muss in doppelte Anführungszeichen eingeschlossen werden.

    ![Designer für Logik-Apps mit Cosmos DB-Einstellungen](./media/data-manager-cosmosdb/create-action.png)

    Der Wert legt den Partitionsschlüssel auf den Transaktionsnachrichtentyp fest.

1. Wählen Sie **Speichern** aus.

Die Logik-App überwacht das Event Grid-Thema. Wenn eine neue Transaktionsnachricht vom Blockchain Data Manager gesendet wird, erstellt die Logik-App ein Dokument in Cosmos DB.

## <a name="send-a-transaction"></a>Senden einer Transaktion

Anschließend senden Sie eine Transaktion an den Blockchainledger, um zu testen, was Sie erstellt haben. Verwenden Sie das Skript **sendrequest.js**, das Sie in der Voraussetzung [Tutorial: Erstellen und Bereitstellen von Smart Contracts mithilfe von Visual Studio Code](send-transaction.md) erstellt haben.

Verwenden Sie Truffle im Terminalbereich von VS Code, um das Skript in Ihrem Blockchainnetzwerk für Konsortien auszuführen. Wählen Sie in der Menüleiste des Terminalbereichs die Registerkarte **Terminal** und in der Dropdownliste die Option **PowerShell**.

``` PowerShell
truffle exec sendrequest.js --network <blockchain network>
```

Ersetzen Sie \<blockchain network\> durch den Namen des Blockchainnetzwerks, das in der Datei **truffle-config.js** definiert ist.

![Senden einer Transaktion](./media/data-manager-cosmosdb/send-request.png)

## <a name="view-transaction-data"></a>Anzeigen von Transaktionsdaten

Nachdem Sie Ihren Blockchain Data Manager mit Azure Cosmos DB verbunden haben, können Sie die Nachrichten der Blockchaintransaktion im Cosmos DB-Daten-Explorer anzeigen.

1. Wechseln Sie zur Ansicht des Cosmos DB-Daten-Explorers. Beispiel: **cosmosdb-blockchain > Daten-Explorer > blockchain-data > Nachrichten > Elemente**.

    ![Cosmos DB-Daten-Explorer](./media/data-manager-cosmosdb/data-explorer.png)

    Der Daten-Explorer listet die Blockchaindatennachrichten auf, die in der Cosmos DB-Datenbank erstellt wurden.

1. Durchsuchen Sie die Nachrichten, indem Sie die Element-ID auswählen und die Nachricht mit dem passenden Transaktionshash finden.

    [![Details zur Blockchaintransaktion](./media/data-manager-cosmosdb/raw-msg.png)](./media/data-manager-cosmosdb/raw-msg.png#lightbox)

    Die unformatierte Transaktionsnachricht enthält Details über die Transaktion. Die Eigenschaftsinformationen sind jedoch verschlüsselt.

    Da Sie den HelloBlockchain Smart Contract zur Blockchain Data Manager-Instanz hinzugefügt haben, wird auch ein **ContractProperties**-Nachrichtentyp gesendet, der decodierte Eigenschaftsinformationen enthält.

1. Suchen Sie die **ContractProperties**-Nachricht für die Transaktion. Es sollte die nächste Nachricht in der Liste sein.

    [![Details zur Blockchaintransaktion](./media/data-manager-cosmosdb/properties-msg.png)](./media/data-manager-cosmosdb/properties-msg.png#lightbox)

    Das Array **DecodedProperties** enthält die Eigenschaften der Transaktion.

Glückwunsch! Sie haben mit Blockchain Data Manager und Azure Cosmos DB erfolgreich einen Explorer für Transaktionsnachrichten erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen und Ressourcengruppen, die Sie für dieses Tutorial verwendet haben, nicht mehr benötigen, können Sie sie löschen. So löschen Sie eine Ressourcengruppe:

1. Navigieren Sie im Azure-Portal im linken Navigationsbereich zu **Ressourcengruppe**, und wählen Sie die Ressourcengruppe aus, die gelöscht werden soll.
1. Wählen Sie die Option **Ressourcengruppe löschen**. Überprüfen Sie den Löschvorgang, indem Sie den Ressourcengruppennamen eingeben und auf **Löschen** klicken.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Integration mit Blockchainledgern.

> [!div class="nextstepaction"]
> [Verwenden des Ethereum-Blockchain-Connectors mit Azure Logic Apps](ethereum-logic-app.md)
