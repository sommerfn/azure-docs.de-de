---
title: Konfigurieren von Blockchain Data Manager – Azure-Portal
description: Informationen zum Erstellen und Verwalten von Blockchain Data Manager über das Azure-Portal
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: chroyal
ms.openlocfilehash: 484322fb0486eeb4ab67366d32350c69a18da743
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605918"
---
# <a name="configure-blockchain-data-manager-using-the-azure-portal"></a>Konfigurieren von Blockchain Data Manager über das Azure-Portal

Konfigurieren Sie Blockchain Data Manager für den Azure Blockchain Service, um Blockchaindaten zu erfassen und an ein Azure Event Grid-Thema zu senden.

So konfigurieren Sie eine Blockchain Data Manager-Instanz

* Erstellen Sie eine Blockchain Data Manager-Instanz für einen Azure Blockchain Service-Transaktionsknoten.
* Fügen Sie Blockchainanwendungen hinzu.

## <a name="prerequisites"></a>Voraussetzungen

* [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md)
* Erstellen eines [Event Grid-Themas](../../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic)
* Weitere Informationen zu [Ereignishandlern in Azure Event Grid](../../event-grid/event-handlers.md)

## <a name="create-instance"></a>Erstellen einer Instanz

Eine Blockchain Data Manager-Instanz verbindet und überwacht einen Azure Blockchain Service-Transaktionsknoten. Nur Benutzer, die Zugriff auf den Transaktionsknoten haben, können eine Verbindung herstellen. Eine Instanz erfasst alle Block- und Transaktionsrohdaten aus dem Transaktionsknoten.

Eine ausgehende Verbindung sendet Blockchaindaten an Azure Event Grid. Beim Erstellen der Instanz konfigurieren Sie eine einzelne ausgehende Verbindung. Blockchain Data Manager unterstützt ausgehende Verbindungen mit mehreren Event Grid-Themen für eine beliebige Blockchain Data Manager-Instanz. Sie können Blockchaindaten an ein einzelnes Ziel oder an mehrere Ziele senden. Fügen Sie der Instanz einfach zusätzliche ausgehende Verbindungen hinzu, um ein weiteres Ziel hinzuzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zum Azure Blockchain Service-Mitglied, das Sie mit Blockchain Data Manager verbinden möchten. Wählen Sie **Blockchain Data Manager** aus.
1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen von Blockchain Data Manager](./media/data-manager-portal/add-instance.png)

    Geben Sie die folgenden Details ein:

    Einstellung | BESCHREIBUNG
    --------|------------
    NAME | Geben Sie einen eindeutigen Namen für einen verbundenen Blockchain Data Manager ein. Der Blockchain Data Manager-Name darf nur Kleinbuchstaben und Ziffern enthalten und eine maximale Länge von 20 Zeichen aufweisen.
    Transaktionsknoten | Wählen Sie einen Transaktionsknoten aus. Es werden nur Transaktionsknoten aufgelistet, auf die Sie Lesezugriff haben.
    Verbindungsname | Geben Sie einen eindeutigen Namen für die ausgehende Verbindung ein, an die Blockchaintransaktionsdaten gesendet werden.
    Event Grid-Endpunkt | Wählen Sie ein Event Grid-Thema im gleichen Abonnement aus,unter dem auch die Blockchain Data Manager-Instanz verwendet wird.

1. Klicken Sie auf **OK**.

    Die Erstellung einer Blockchain Data Manager-Instanz dauert weniger als eine Minute. Nachdem die Instanz bereitgestellt wurde, wird sie automatisch gestartet. In einer laufenden Blockchain Data Manager-Instanz werden Blockchainereignisse aus dem Transaktionsknoten erfasst und Daten an ausgehende Verbindungen gesendet.

    Die neue Instanz wird in der Liste der Blockchain Data Manager-Instanzen für das Azure Blockchain Service-Mitglied angezeigt.

    ![Liste der Blockchain Data Member-Instanzen](./media/data-manager-portal/instance-list.png)

## <a name="add-blockchain-application"></a>Hinzufügen einer Blockchainanwendung

Wenn Sie eine Blockchainanwendung hinzufügen, werden Ereignis- und Eigenschaftsstatus der Anwendung von Blockchain Data Manager decodiert. Andernfalls werden nur Block- und Transaktionsrohdaten gesendet. Blockchain Data Manager ermittelt auch Vertragsadressen, wenn der Vertrag bereitgestellt wird. Sie können einer Blockchain Data Manager-Instanz mehrere Blockchainanwendungen hinzufügen.

> [!IMPORTANT]
> Derzeit werden Blockchainanwendungen, die [Arraytypen](https://solidity.readthedocs.io/en/v0.5.12/types.html#arrays) oder [Zuordnungstypen](https://solidity.readthedocs.io/en/v0.5.12/types.html#mapping-types) von Solidity deklarieren, nicht vollständig unterstützt. Als Array- oder Zuordnungstypen deklarierte Eigenschaften werden in Meldungen vom Typ *ContractPropertiesMsg* oder *DecodedContractEventsMsg* nicht decodiert.

Blockchain Data Manager erfordert eine Smart Contract ABI und eine bereitgestellte Bytecodedatei, um die Anwendung hinzuzufügen.

### <a name="get-contract-abi-and-bytecode"></a>Abrufen von Vertrags-ABI und Bytecode

Mit der Vertrags-ABI werden die Smart Contract-Schnittstellen definiert. Sie beschreibt, wie mit dem Smart Contract interagiert wird. Sie können das [Azure Blockchain Development Kit für Ethereum-Erweiterung](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) verwenden, um die Vertrags-ABI in die Zwischenablage zu kopieren.

1. Erweitern Sie im Explorer-Bereich von Visual Studio Code den Ordner **build/contracts** Ihres Solidity-Projekts.
1. Klicken Sie mit der rechten Maustaste auf die Vertragsmetadaten-JSON-Datei. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Wählen Sie **Copy Contract ABI** (Vertrags-ABI kopieren) aus.

    ![Visual Studio Code-Bereich mit Auswahl von „Copy Contract ABI“ (Vertrags-ABI kopieren)](./media/data-manager-portal/abi-devkit.png)

    Die Vertrags-ABI wird in die Zwischenablage kopiert.

1. Speichern Sie das **abi**-Array als JSON-Datei. Beispiel: *abi.json*. Sie verwenden die Datei in einem späteren Schritt.

Blockchain Data Manager benötigt den bereitgestellten Bytecode für den Smart Contract. Der bereitgestellte Bytecode unterscheidet sich von Smart Contract-Bytecode. Sie können den bereitgestellten Bytecode aus der kompilierten Vertragsmetadatendatei abrufen.

1. Öffnen Sie die Vertragsmetadatendatei, die im Ordner **build/contracts** Ihres Solidity-Projekts enthalten ist. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Suchen Sie in der JSON-Datei nach dem Element **deployedBytecode**.
1. Kopieren Sie den Hexadezimalwert ohne die Anführungszeichen.

    ![Visual Studio Code-Bereich mit Bytecode in den Metadaten](./media/data-manager-portal/bytecode-metadata.png)

1. Speichern Sie den **bytecode**-Wert als JSON-Datei. Beispiel: *bytecode.json*. Sie verwenden die Datei in einem späteren Schritt.

Das folgende Beispiel zeigt die im VC Code-Editor geöffneten Dateien *abi.json* und *bytecode.json*. Ihre Dateien sollten ähnlich aussehen.

![Beispiel der Dateien „abi.json“ und „bytecode.json“](./media/data-manager-portal/contract-files.png)

### <a name="create-contract-abi-and-bytecode-url"></a>Erstellen von Vertrags-ABI- und Bytecode-URLs

Blockchain Data Manager erfordert, dass die Contract ABI- und Bytecodedateien beim Hinzufügen einer Anwendung über eine URL abrufbar sind. Sie können ein Azure Storage-Konto verwenden, um eine privat zugängliche URL bereitzustellen.

#### <a name="create-storage-account"></a>Speicherkonto erstellen

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

#### <a name="upload-contract-files"></a>Hochladen von Vertragsdateien

1. Erstellen Sie einen neuen Container für das Speicherkonto. Wählen Sie **Container > Container** aus.

    ![Erstellen eines Speicherkontocontainers](./media/data-manager-portal/create-container.png)

    | Feld | BESCHREIBUNG |
    |-------|-------------|
    | NAME  | Geben Sie dem Container einen Namen. Beispiel: *smartcontract* |
    | Öffentliche Zugriffsebene | Wählen Sie *Privat (kein anonymer Zugriff)* aus. |

1. Wählen Sie **OK** aus, um den Container zu erstellen.
1. Wählen Sie den Container und dann **Hochladen** aus.
1. Wählen Sie beide JSON-Dateien aus, die Sie im Abschnitt [Abrufen von Vertrags-ABI und Bytecode](#get-contract-abi-and-bytecode) erstellt haben.

    ![Blob hochladen](./media/data-manager-portal/upload-blobs.png)

    Wählen Sie die Option **Hochladen**.

#### <a name="generate-url"></a>Generieren der URL

Generieren Sie für jedes Blob eine Shared Access Signature.

1. Wählen Sie das ABI JSON-Blob aus.
1. Wählen Sie **SAS generieren** aus.
1. Wählen Sie den gewünschten Access Signature-Ablauf und dann **Blob-SAS-Token und URL generieren** aus.

    ![Generieren des SAS-Tokens](./media/data-manager-portal/generate-sas.png)

1. Kopieren Sie die **Blob-SAS-URL**, und speichern Sie sie für den nächsten Abschnitt.
1. Wiederholen Sie die Schritte unter [URL generieren](#generate-url) für das Bytecode-JSON-Blob.

### <a name="add-application-to-instance"></a>Hinzufügen einer Anwendung zur Instanz

1. Wählen Sie Ihre Blockchain Data Manager-Instanz aus der Instanzliste aus.
1. Wählen Sie **Blockchainanwendungen** aus.
1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen einer Blockchainanwendung](./media/data-manager-portal/add-application.png)

    Geben Sie den Namen der Blockchainanwendung und die Smart Contract ABI- und Bytecode-URLs ein.

    Einstellung | BESCHREIBUNG
    --------|------------
    NAME | Geben Sie einen eindeutigen Namen zur Verfolgung der Blockchainanwendung ein.
    Vertrags-ABI | Der URL-Pfad zur Datei mit der Vertrags-ABI. Weitere Informationen finden Sie unter [Erstellen von Vertrags-ABI- und Bytecode-URLs](#create-contract-abi-and-bytecode-url).
    Vertragsbytecode | Der URL-Pfad zur Bytecodedatei. Weitere Informationen finden Sie unter [Erstellen von Vertrags-ABI- und Bytecode-URLs](#create-contract-abi-and-bytecode-url).

1. Klicken Sie auf **OK**.

    Nachdem die Anwendung erstellt wurde, wird sie in der Liste der Blockchainanwendungen angezeigt.

    ![Liste der Blockchainanwendungen](./media/data-manager-portal/artifact-list.png)

Sie können das Azure Storage-Konto löschen oder zur Konfiguration weiterer Blockchainanwendungen verwenden. Wenn Sie das Azure Storage-Konto löschen möchten, können Sie die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden sowohl das zugeordnete Speicherkonto als auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

## <a name="stop-instance"></a>Beenden einer Instanz

Beenden Sie die Blockchain Manager-Instanz, wenn Sie das Erfassen von Blockchainereignissen und das Senden von Daten an ausgehende Verbindungen beenden möchten. Wenn die Instanz beendet wird, fallen keine Gebühren für Blockchain Data Manager an. Weitere Informationen finden Sie unter [Azure Data Lake Storage – Preise](https://azure.microsoft.com/pricing/details/blockchain-service).

1. Wechseln Sie zu **Übersicht**, und wählen Sie **Beenden** aus.

    ![Beenden einer Instanz](./media/data-manager-portal/stop-instance.png)

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, mit Blockchain Data Manager und Azure Cosmos DB einen Explorer für Blockchaintransaktionsnachrichten zu erstellen.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Blockchain Data Manager zum Senden von Daten an Azure Cosmos DB](data-manager-cosmosdb.md)