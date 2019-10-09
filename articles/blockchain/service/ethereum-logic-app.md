---
title: Verwenden des Ethereum-Blockchain-Connectors mit Azure Logic Apps
description: Verwenden des Ethereum-Blockchain-Connectors mit Azure Logic Apps zum Auslösen von Smart Contract-Funktionen und zum Reagieren auf Smart Contract-Ereignisse
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 09/26/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
manager: femila
ms.openlocfilehash: 48f862bea6df50480ce4f9d21993693da34cfbc9
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720673"
---
# <a name="how-to-use-ethereum-blockchain-connector-with-azure-logic-apps"></a>Verwenden des Ethereum-Blockchain-Connectors mit Azure Logic Apps

Verwenden Sie den [Ethereum-Blockchain-Connector](https://docs.microsoft.com/connectors/blockchainethereum/) mit [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) zum Ausführen von Smart Contract-Funktionen sowie zum Reagieren auf Smart Contract-Ereignisse. Beispiel: Sie möchten einen REST-basierten Microservice erstellen, der Informationen von einem Blockchainledger zurückgibt. Mithilfe einer Logik-App können Sie HTTP-Anforderungen akzeptieren, die in einem Blockchainledger gespeicherte Informationen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie den optionalen [Schnellstart: Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe von Visual Studio Code](connect-vscode.md) aus. Der Schnellstart führt Sie durch die Installation des [Azure Blockchain Development Kits für Ethereum](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) und das Einrichten Ihrer Blockchain-Entwicklungsumgebung.

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Azure Logic Apps hilft Ihnen beim Planen und Automatisieren von Geschäftsprozessen und Workflows, wenn Sie Systeme und Dienste integrieren müssen. Erstellen Sie zunächst eine Logik, die den Ethereum-Blockchain-Connector verwendet.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen > Integration > Logik-App** aus.
1. Geben Sie unter **Logik-App erstellen** an, wo die Logik-App erstellt werden soll. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

    Weitere Informationen zum Erstellen von Azure Logic Apps finden Sie unter [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Nachdem Azure Ihre App bereitgestellt hat, wählen Sie Ihre Logik-App-Ressource aus.
1. Wählen Sie im Designer für Logik-Apps unter **Vorlagen** die Option **Leere Logik-App** aus.

Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt die Logic Apps-Engine eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird.

Der Ethereum-Blockchain-Connector verfügt über einen Trigger und mehrere Aktionen. Welche Trigger oder Aktionen Sie verwenden, hängt von Ihrem Szenario ab.

Wählen Sie den auf Ihren Workflow zutreffenden Abschnitt aus:

* Wenn Ihr Workflow beim Auftreten eines Ereignisses in der Blockchain ausgelöst wird, [verwenden Sie den Ereignisauslöser](#use-the-event-trigger).
* Wenn Ihr Workflow einen Smart Contract bereitstellt, [verwenden Sie Aktionen](#use-actions).
* Wenn Ihr Workflow einem häufigen Szenario folgt, [erstellen Sie einen Workflow mithilfe des Entwickler-Kits](#generate-a-workflow).

## <a name="use-the-event-trigger"></a>Verwenden des Ereignisauslösers

Verwenden Sie Ethereum-Blockchain-Ereignisauslöser, wenn Sie möchten, dass eine Logik-App ausgeführt wird, nachdem ein Smart Contract-Ereignis aufgetreten ist. Beispiel: Nach dem Aufrufen einer Smart Contract-Funktion soll eine E-Mail versendet werden.

1. Wählen Sie im Logik-App-Designer den Ethereum-Blockchain-Connector aus.
1. Wählen Sie auf der Registerkarte **Trigger** die Option **When a smart contract event occurs** (Wenn ein Smart Contract-Ereignis auftritt) aus.
1. Ändern oder [erstellen Sie eine API-Verbindung](#create-an-api-connection) in Azure Blockchain Service.
1. Geben Sie die Details zum Smart Contract ein, den Sie auf Ereignisse überprüfen möchten.

    ![Eigenschaften von Ereignisauslösern](./media/ethereum-logic-app/event-properties.png)

    | Eigenschaft | BESCHREIBUNG |
    |----------|-------------|
    | **Contract ABI** (Vertrags-ABI) | Die binäre Anwendungsschnittstelle (Application Binary Interface, ABI) des Vertrags definiert die Smart Contract-Schnittstellen. Vorgehensweise zum [Abrufen der Vertrags-ABI](#get-contract-abi). |
    | **Smart contract address** (Smart Contract-Adresse) | Die Vertragsadresse ist die Smart Contract-Zieladresse in der Ethereum-Blockchain. Vorgehensweise zum [Abrufen der Vertragsadresse](#get-contract-address). |
    | **Event name** (Ereignisname) | Wählen Sie ein Smart Contract-Ereignis für die Überprüfung aus. Das Ereignis löst die Logik-App aus. |
    | **Interval** (Intervall) und **Frequency** (Häufigkeit) | Wählen Sie aus, wie häufig auf das Ereignis geprüft werden soll. |

1. Wählen Sie **Speichern** aus.

Zum Fertigstellen Ihrer Logik-App können Sie einen neuen Schritt hinzufügen, der eine Aktion ausführt, die auf dem Ethereum-Blockchein-Ereignisauslöser basiert. Beispiel: Senden einer E-Mail.

## <a name="use-actions"></a>Verwenden von Aktionen

Verwenden Sie Ethereum-Blockchain-Aktionen, wenn eine Logik-App eine Aktion im Blockchainledger ausführen soll. Beispiel: Sie möchten einen REST-basierten Microservice erstellen, der eine Smart Contract-Funktion aufruft, wenn eine HTTP-Anforderung an eine Logik-App gesendet wird.

Connector-Aktionen erfordern einen Trigger. Sie können eine Ethereum-Blockchain-Connectoraktion als nächsten Schritt nach einem Trigger verwenden, etwa einen HTTP-Anforderungstrigger für einen Microservice.

1. Wählen Sie im Logik-App-Designer nach einem Trigger die Option **Neuer Schritt** aus.
1. Wählen Sie den Ethereum-Blockchain-Connector aus.
1. Wählen Sie auf der Registerkarte **Aktionen** eine der verfügbaren Aktionen aus.

    ![Aktionseigenschaften](./media/ethereum-logic-app/action-properties.png)

1. Ändern oder [erstellen Sie eine API-Verbindung](#create-an-api-connection) in Azure Blockchain Service.
1. Geben Sie je nach der gewählten Aktion die folgenden Details zu Ihrer Smart Contract-Funktion an:

    | Eigenschaft | BESCHREIBUNG |
    |----------|-------------|
    | **Contract ABI** (Vertrags-ABI) | Die binäre Anwendungsschnittstelle (Application Binary Interface, ABI) des Vertrags definiert die Smart Contract-Schnittstellen. Vorgehensweise zum [Abrufen der Vertrags-ABI](#get-contract-abi). |
    | **Contract bytecode** (Vertragsbytecode) | Der kompilierte Smart Contract-Bytecode. Vorgehensweise zum [Abrufen des Vertragsbytecodes](#get-contract-bytecode) |
    | **Smart contract address** (Smart Contract-Adresse) | Die Vertragsadresse ist die Smart Contract-Zieladresse in der Ethereum-Blockchain. Vorgehensweise zum [Abrufen der Vertragsadresse](#get-contract-address) |
    | **Smart contract function name** (Smart Contract-Funktionsname) | Wählen Sie den Smart Contract-Funktionsnamen für die Aktion aus. Die Liste wird mit den Details in der Vertrags-ABI ausgefüllt. |

    Nachdem Sie einen Smart Contract-Funktionsnamen ausgewählt haben, werden unter Umständen die erforderlichen Felder für Funktionsparameter angezeigt. Geben Sie die Werte oder dynamischen Inhalte ein, die für Ihr Szenario erforderlich sind.

Nun können Sie Ihre Logik-App verwenden. Wenn das Logik-App-Ereignis ausgelöst wird, wird die Ethereum-Blockchain-Aktion ausgeführt. Beispiel: Ein HTTP-Anforderungstrigger führt eine Ethereum-Blockchain-Aktion aus, um einen Smart Contract-Zustandswert abzufragen, der zu einer HTTP-Antwort führt, welche den Wert zurückgibt.

## <a name="generate-a-workflow"></a>Generieren eines Workflows

Das Azure Blockchain Development Kit für die Ethereum-Erweiterung in Visual Studio Code kann Logik-App-Workflows für gängige Szenarien generieren. Vier Szenarien stehen zur Verfügung:

* Veröffentlichen von Daten in Azure SQL-Datenbank
* Veröffentlichung von Ereignissen in Azure Event Grid oder Azure Service Bus
* Veröffentlichung von Berichten
* REST-basierter Microservice

 Das Azure Blockchain Development Kit verwendet Truffle, um die Blockchainentwicklung zu vereinfachen. Wenn Sie ausgehend von einem Smart Contract eine Logik-App generieren möchten, benötigen Sie eine Truffle-Lösung für den Smart Contract. Außerdem benötigen Sie eine Verbindung mit Ihrem Azure Blockchain Service-Konsortiumsnetzwerk. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe von Visual Studio Code](connect-vscode.md).

Die folgenden Schritte generieren beispielsweise eine REST-basierte Microservice-Logik-App basierend auf dem Schnellstart-Smart Contract **HelloBlockchain**:

1. Erweitern Sie auf der Seitenleiste des VS Code-Explorers den Ordner **contracts** in Ihrer Lösung.
1. Klicken Sie mit der rechten Maustaste auf **HelloBlockchain.sol**, und wählen Sie im Menü die Option **Generate microservices for smart contracts** (Microservices für Smart Contracts generieren) aus.

    ![Generieren einer Logik-App](./media/ethereum-logic-app/generate-logic-app.png)

1. Wählen Sie in der Befehlspalette **Logik-App** aus.
1. Geben Sie die **Vertragsadresse** ein. Weitere Informationen finden Sie unter [Abrufen der Vertragsadresse](#get-contract-address).
1. Wählen Sie das Azure-Abonnement und die Ressourcengruppe für die Logik-App aus.

    Die Logik-App-Konfiguration und die Codedateien werden im Verzeichnis **generatedLogicApp** generiert.

1. Zeigen Sie das Verzeichnis **generatedLogicApp/HelloBlockchain** an. Für jede Smart Contract-Funktion, jedes Ereignis und jede Eigenschaft gibt es eine Logik-App-JSON-Datei.
1. Öffnen Sie die Datei **generatedLogicApp/HelloBlockchain/Service/property.RequestMessage.logicapp.json**, und kopieren Sie ihren Inhalt.

    ![JSON für die RequestMessage-Eigenschaft](./media/ethereum-logic-app/requestmessage.png)

1. Wählen Sie in Ihrer Logik-App die Option **Logik-App-Codeansicht** aus. Ersetzen Sie den vorhandenen JSON-Code durch den generierten JSON-Code der Logik-App.

    ![Ersetzen der Logik-App-Konfiguration in der Codeansicht](./media/ethereum-logic-app/code-view.png)

1. Wählen Sie **Designer** aus, um zur Designeransicht zu wechseln.
1. Die Logik-App enthält die grundlegenden Schritte für das Szenario. Allerdings müssen Sie die Konfigurationsdetails für den Ethereum-Blockchain-Connector aktualisieren.
1. Wählen Sie den Schritt **Verbindungen** aus, und ändern oder [erstellen Sie eine API-Verbindung](#create-an-api-connection) in Azure Blockchain Service.

    ![Microservice-Logik-App](./media/ethereum-logic-app/microservice-logic-app.png)

1. Sie können Ihre Logik-App jetzt verwenden. Um den REST-basierten Microservice zu testen, geben Sie eine HTTP-POST-Anforderung an die Logik-App-Anforderungs-URL aus. Kopieren Sie die **HTTP-POST-URL** aus dem Schritt **Beim Empfang einer HTTP-Anforderung**.

    ![HTTP-POST-URL](./media/ethereum-logic-app/post-url.png)

1. Verwenden Sie cURL zum Erstellen einer HTTP-POST-Anforderung. Ersetzen Sie den Platzhaltertext **\<HTTP POST URL\>** durch die URL aus dem vorherigen Schritt.

    ``` bash
    curl -d "{}" -H "Content-Type: application/json" -X POST "<HTTP POST URL>"
    ```

    Der cURL-Befehl gibt eine Antwort von der Logik-App zurück. In diesem Fall ist dies die Ausgabe der Smart Contract-Funktion **RequestMessage**.

    ![RequestMessage-Eigenschaftenausgabe](./media/ethereum-logic-app/curl.png)

Weitere Informationen zur Verwendung des Development Kits finden Sie im [Wiki für das Azure Blockchain Development Kit für Ethereum](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki).

## <a name="create-an-api-connection"></a>Erstellen einer API-Verbindung

Für den Ethereum-Blockchain-Connector ist eine API-Verbindung mit einer Blockchain erforderlich. Sie können den API-Connector für mehrere Logik-Apps verwenden. Einige Eigenschaften sind erforderlich, während andere von Ihrem Szenario abhängig sind.

> [!IMPORTANT]
> Zum Erstellen von Transaktionen in einer Blockchain ist ein privater Schlüssel oder eine Kontoadresse mit einem Kennwort erforderlich. Es ist nur eine Art der Authentifizierung erforderlich. Sie müssen nicht sowohl den privaten Schlüssel als auch die Kontodetails angeben. Zum Abfragen von Verträgen ist keine Transaktion erforderlich. Wenn Sie Aktionen verwenden, die den Vertragsstatus abfragen, sind der private Schlüssel oder die Kontoadresse und das Kennwort nicht erforderlich.

Zum Einrichten einer Verbindung mit einem Azure Blockchain Service-Mitglied finden Sie in der folgenden Liste mögliche Eigenschaften, die je nach Szenario erforderlich sein könnten:

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
|**Verbindungsname** | Der Name der API-Verbindung. Erforderlich. |
|**Ethereum-RPC-Endpunkt** | HTTP-Adresse des Azure Blockchain Service-Transaktionsknoten. Erforderlich. Vorgehensweise zum [Abrufen des RPC-Endpunkts](#get-rpc-endpoint). |
|**Privater Schlüssel** | Privater Schlüssel des Ethereum-Kontos. Der private Schlüssel oder die Kontoadresse und das Kennwort sind für Transaktionen erforderlich. Vorgehensweise zum [Abrufen des privaten Schlüssels](#get-private-key) |
|**Kontoadresse** | Die Kontoadresse des Azure Blockchain-Mitglieds. Der private Schlüssel oder die Kontoadresse und das Kennwort sind für Transaktionen erforderlich. Vorgehensweise zum [Abrufen der Kontoadresse](#get-account-address) |
|**Kontokennwort** | Das Kontokennwort wird festgelegt, wenn Sie das Mitglied erstellen. Weitere Informationen zum Zurücksetzen des Kennworts finden Sie unter [Ethereum-Konto](consortium.md#ethereum-account).|

## <a name="get-rpc-endpoint"></a>Abrufen des RPC-Endpunkts

Die Azure Blockchain Service-RPC-Endpunktadresse ist erforderlich, um eine Verbindung mit dem Blockchainnetzwerk herzustellen. Sie können die Endpunktadresse mit dem Azure Blockchain Development Kit für Ethereum oder dem Azure-Portal abrufen.

**Mit dem Development Kit:**

1. Klicken Sie in Visual Studio Code unter **Azure Blockchain Service** mit der rechten Maustaste auf das Konsortium.
1. Wählen Sie **Copy RPC endpoint** (RPC-Endpunkt kopieren) aus.

    ![Kopieren des RPC-Endpunkts](./media/ethereum-logic-app/devkit-rpc.png)

    Der RPC-Endpunkt wird in die Zwischenablage kopiert.

**Mit dem Azure-Portal:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrem Azure Blockchain-Mitglied. Klicken Sie auf **Transaktionsknoten** und dann auf den Link „default transaction node“ (Standardtransaktionsknoten).

    ![Standardtransaktionsknoten auswählen](./media/ethereum-logic-app/transaction-nodes.png)

1. Klicken Sie auf **Verbindungszeichenfolgen > Zugriffsschlüssel**.
1. Kopieren Sie die Endpunktadresse von **HTTP (Zugriffsschlüssel 1)** oder Zugriffsschlüssel 2.

    ![Verbindungszeichenfolge](./media/ethereum-logic-app/connection-string.png)

    Der RPC-Endpunkt ist die HTTPS-URL, einschließlich der Adresse und des Zugriffsschlüssels Ihres Azure Blockchain Service-Mitgliedstransaktionsknotens.

## <a name="get-private-key"></a>Abrufen des privaten Schlüssels

Der private Schlüssel des Ethereum-Kontos kann zum Authentifizieren verwendet werden, wenn eine Transaktion an die Blockchain gesendet wird. Der öffentliche und der private Schlüssel Ihres Ethereum-Kontos werden aus einem mnemonischen Zeichen aus zwölf Wörtern generiert. Das Azure Blockchain Development Kit für Ethereum generiert ein mnemonisches Zeichen, wenn Sie eine Verbindung mit einem Azure Blockchain Service-Konsortiumsmitglied herstellen. Sie können die Endpunktadresse mithilfe der Development Kit-Erweiterung abrufen.

1. Öffnen Sie in Visual Studio Code die Befehlspalette (F1).
1. Wählen Sie **Azure Blockchain: Retrieve private key** (Azure Blockchain: privaten Schlüssel abrufen) aus.
1. Wählen Sie das mnemonische Zeichen aus, das Sie beim Herstellen einer Verbindung mit dem Konsortiummitglied gespeichert haben.

    ![Mnemonisches Zeichen auswählen](./media/ethereum-logic-app/private-key.png)

    Der private Schlüssel wird in die Zwischenablage kopiert.

## <a name="get-account-address"></a>Abrufen der Kontoadresse

Das Mitgliedskonto und das Kennwort können zum Authentifizieren verwendet werden, wenn eine Transaktion an die Blockchain gesendet wird. Das Kennwort wird festgelegt, wenn Sie das Mitglied erstellen.

1. Navigieren Sie im Azure-Portal zur Azure Blockchain Service-Übersichtsseite.
1. Kopieren Sie die Adresse des **Mitgliedskontos**.

    ![Kopieren des Mitgliedskontos](./media/ethereum-logic-app/member-account.png)

Weitere Informationen zur Kontoadresse und zum Kennwort finden Sie unter [Ethereum-Konto](consortium.md#ethereum-account).

## <a name="get-contract-abi"></a>Abrufen der Vertrags-ABI

Die binäre Anwendungsschnittstelle (Application Binary Interface, ABI) des Vertrags definiert die Smart Contract-Schnittstellen. Sie beschreibt, wie mit dem Smart Contract interagiert wird. Sie erhalten die Vertrags-ABI mithilfe des Azure Blockchain Development Kits für Ethereum oder aus der Metadatendatei des Solidity-Compilervertrags.

**Mit dem Development Kit:**

Wenn Sie Ihren Smart Contract mit dem Development Kit oder Truffle erstellt haben, können Sie die-Erweiterung verwenden, um die Vertrags-ABI in die Zwischenablage zu kopieren.

1. Erweitern Sie im Explorer-Bereich von Visual Studio Code den Ordner **build/contracts** Ihres Solidity-Projekts.
1. Klicken Sie mit der rechten Maustaste auf die Vertragsmetadaten-JSON-Datei. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Wählen Sie **Copy contract ABI** (Vertrags-ABI kopieren) aus.

    ![Kopieren der Vertrags-ABI mithilfe von DevKit](./media/ethereum-logic-app/abi-devkit.png)

    Die Vertrags-ABI wird in die Zwischenablage kopiert.

**Mit der Vertragsmetadatendatei:**

1. Öffnen Sie die Vertragsmetadatendatei, die im Ordner **build/contracts** Ihres Solidity-Projekts enthalten ist. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Suchen Sie den Abschnitt **abi** in der JSON-Datei.
1. Kopieren Sie das JSON-Array **abi**.

    ![Abschnitt mit der Vertrags-ABI in den Metadaten](./media/ethereum-logic-app/abi-metadata.png)

## <a name="get-contract-bytecode"></a>Abrufen des Vertragsbytecodes

Der Vertragsbytecode ist der kompilierte Smart Contract, der vom virtuellen Ethereum-Computer ausgeführt wird. Sie erhalten den Vertragsbytecode mithilfe des Azure Blockchain Development Kits für Ethereum oder aus dem Solidity-Compiler.

**Mit dem Development Kit:**

Wenn Sie Ihren Smart Contract mit dem Development Kit oder Truffle erstellt haben, können Sie die-Erweiterung verwenden, um den Vertragsbytecode in die Zwischenablage zu kopieren.

1. Erweitern Sie im Explorer-Bereich von Visual Studio Code den Ordner **build/contracts** Ihres Solidity-Projekts.
1. Klicken Sie mit der rechten Maustaste auf die Vertragsmetadaten-JSON-Datei. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Wählen Sie **Copy contract bytecode** (Vertragsbytecode kopieren) aus.

    ![Kopieren des Vertragsbytecodes mithilfe von DevKit](./media/ethereum-logic-app/bytecode-devkit.png)

    Der Vertragsbytecode wird in die Zwischenablage kopiert.

**Mit der Vertragsmetadatendatei:**

1. Öffnen Sie die Vertragsmetadatendatei, die im Ordner **build/contracts** Ihres Solidity-Projekts enthalten ist. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Suchen Sie das Element **bytecode** in der JSON-Datei.
1. Kopieren Sie den Wert **bytecode**.

    ![Kopieren des Bytecodes mithilfe von Metadaten](./media/ethereum-logic-app/bytecode-metadata.png)

**Mit dem Solidity-Compiler:**

Verwenden Sie den Befehl `solc --bin <smart contract>.sol`, um den Vertragsbytecode zu generieren.

## <a name="get-contract-address"></a>Abrufen der Vertragsadresse

Die Vertragsadresse ist die Smart Contract-Zieladresse in der Ethereum-Blockchain. Diese Adresse wird zum Senden einer Transaktion oder eines Smart Contract-Abfragezustands verwendet. Sie können die Vertragsadresse aus der Truffle-Migrationsausgabe oder der Vertragsmetadatendatei abrufen.

**Mit der Truffle-Migrationsausgabe:**

Truffle zeigt die Vertragsadresse an, nachdem der Smart Contract bereitgestellt wurde. Kopieren Sie die **Vertragsadresse** aus der Ausgabe.

![Vertragsadresse aus Truffle-Ausgabe](./media/ethereum-logic-app/contract-address-truffle.png)

**Mit der Vertragsmetadatendatei:**

1. Öffnen Sie die Vertragsmetadatendatei, die im Ordner **build/contracts** Ihres Solidity-Projekts enthalten ist. Der Dateiname ist der Smart Contract-Name gefolgt von der Erweiterung **.json**.
1. Suchen Sie den Abschnitt **networks** in der JSON-Datei.
1. Private Netzwerke werden durch eine ganzzahlige Netzwerk-ID identifiziert. Suchen Sie den Adresswert im Netzwerkabschnitt.
1. Kopieren Sie den Wert **address**.

![Vertragsadresse aus Metadaten](./media/ethereum-logic-app/contract-address-metadata.png)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich [Häufige Szenarien beim Verbinden von Blockchain mithilfe von Azure Logic Apps](https://channel9.msdn.com/Shows/Blocktalk/Doing-more-with-Logic-Apps?term=logic%20apps%20blockchain&lang-en=true) an.
