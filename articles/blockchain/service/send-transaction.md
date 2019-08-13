---
title: Verwenden von Smart Contracts für den Azure Blockchain Service
description: Tutorial zur Verwendung des Azure Blockchain Service zum Bereitstellen eines Smart Contracts und Ausführen einer Funktion per Transaktion.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 07/31/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
ms.openlocfilehash: 1843bd66e11a6686c9ae81fb8e30c7b030e889b7
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705132"
---
# <a name="tutorial-use-smart-contracts-on-azure-blockchain-service"></a>Tutorial: Verwenden von Smart Contracts für den Azure Blockchain Service

In diesem Tutorial verwenden Sie das Azure Blockchain Development Kit für Ethereum, um einen Smart Contract zu erstellen und bereitzustellen, und führen dann eine Smart Contract-Funktion per Transaktion in einem Blockchainnetzwerk für Konsortien aus.

Sie verwenden das Azure Blockchain Development Kit für Ethereum für folgende Zwecke:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit einem Konsortium-Blockchainmitglied per Azure Blockchain Service
> * Erstellen eines Smart Contracts
> * Bereitstellen eines Smart Contracts
> * Ausführen einer Smart Contract-Funktion per Transaktion
> * Abfragen des Contract-Status

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Azure Blockchain Development Kit für Ethereum-Erweiterung](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node.js](https://nodejs.org)
* [Git-Client](https://git-scm.com)
* [Python](https://www.python.org/downloads/release/python-2715/). Fügen Sie Ihrem Pfad „python. exe“ hinzu. Python ist in Ihrem Pfad für das Azure Blockchain Development Kit erforderlich.
* [Truffle](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI](https://github.com/trufflesuite/ganache-cli)

### <a name="verify-azure-blockchain-development-kit-environment"></a>Überprüfen der Azure Blockchain Development Kit-Umgebung

Mit dem Azure Blockchain Development Kit wird überprüft, ob die Voraussetzungen für Ihre Entwicklungsumgebung erfüllt wurden. Bereiten Sie Ihre Entwicklungsumgebung wie folgt vor:

Wählen Sie in der VS Code-Befehlspalette die Option **Azure Blockchain: Show Welcome Page** (Azure Blockchain: Willkommensseite anzeigen).

Das Azure Blockchain Development Kit führt ein Überprüfungsskript mit einer Dauer von ca. einer Minute aus. Sie können die Ausgabe anzeigen, indem Sie **Terminal > New Terminal** (Terminal > Neues Terminal) wählen. Wählen Sie in der Menüleiste des Terminals die Registerkarte **Output** (Ausgabe) und in der Dropdownliste dann **Azure Blockchain**. Eine erfolgreiche Überprüfung sieht in etwa wie in der folgenden Abbildung aus:

![Gültige Entwicklungsumgebung](./media/send-transaction/valid-environment.png)

 Falls ein benötigtes Tool fehlt, können Sie eine neue Registerkarte mit dem Namen **Azure Blockchain Development Kit – Preview** (Azure Blockchain Development Kit: Vorschauversion) verwenden. Darauf sind die erforderlichen Apps, die installiert werden müssen, und die Links zum Herunterladen der Tools aufgeführt.

![Development Kit: Erforderliche Apps](./media/send-transaction/required-apps.png)

## <a name="connect-to-consortium-member"></a>Herstellen einer Verbindung mit dem Konsortiumsmitglied

Sie können eine Verbindung mit Konsortiumsmitgliedern herstellen, indem Sie die VS Code-Erweiterung für das Azure Blockchain Development Kit verwenden. Nachdem die Verbindung mit einem Konsortium hergestellt wurde, können Sie Smart Contracts kompilieren, erstellen und für ein Azure Blockchain Service-Konsortiumsmitglied bereitstellen.

Falls Sie keinen Zugriff auf ein Azure Blockchain Service-Konsortiumsmitglied haben, müssen Sie als Voraussetzung [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md) durcharbeiten.

1. Erweitern Sie im VS Code-Explorer-Bereich (Visual Studio Code) die Erweiterung **Azure Blockchain**.
1. Wählen Sie die Option **Connect to Consortium** (Verbindung mit Konsortium herstellen).

   ![Herstellen einer Verbindung mit einem Konsortium](./media/send-transaction/connect-consortium.png)

    Befolgen Sie die Anweisungen zum Authentifizieren per Browser, wenn Sie zum Durchführen der Azure-Authentifizierung aufgefordert werden.
1. Wählen Sie in der Dropdownliste der Befehlspalette die Option **Connect to Azure Blockchain Service consortium** (Verbindung mit Azure Blockchain Service-Konsortium herstellen).
1. Wählen Sie das Abonnement und die Ressourcengruppe aus, das bzw. die Ihrem Azure Blockchain Service-Konsortiumsmitglied zugeordnet ist.
1. Wählen Sie Ihr Konsortium in der Liste aus.

Die Konsortiums- und Blockchainmitglieder sind im Visual Studio Explorer in der Seitenleiste aufgeführt.

![Anzeige des Konsortiums im Explorer](./media/send-transaction/consortium-node.png)

## <a name="create-a-smart-contract"></a>Erstellen eines Smart Contracts

Im Azure Blockchain Development Kit für Ethereum werden Projektvorlagen und Truffle-Tools verwendet, die als Hilfe beim Vorbereiten, Erstellen und Bereitstellen von Contracts dienen.

1. Wählen Sie in der VS Code-Befehlspalette die Option **Azure Blockchain: New Solidity Project** (Azure Blockchain: Neues Solidity-Projekt).
1. Wählen Sie **Create basic project** (Einfaches Projekt erstellen).
1. Erstellen Sie einen neuen Ordner mit dem Namen `HelloBlockchain`, und wählen Sie einen **neuen Projektpfad** aus.

Das Azure Blockchain Development Kit erstellt und initialisiert ein neues Solidity-Projekt für Sie. Das Basisprojekt enthält den Smart Contract **HelloBlockchain** als Beispiel sowie alle Dateien, die Sie für die Erstellung und die Bereitstellung für Ihr Konsortiumsmitglied in Azure Blockchain Service benötigen. Es kann einige Minuten dauern, bis das Projekt erstellt wurde. Sie können den Status im Terminalbereich von VS Code verfolgen, indem Sie die Ausgabe für Azure Blockchain auswählen.

Die Projektstruktur sieht wie im folgenden Beispiel aus:

   ![Solidity-Projekt](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Erstellen eines Smart Contracts

Smart Contracts befinden sich im Verzeichnis **contracts** des Projekts. Sie kompilieren Smart Contracts, bevor Sie diese für eine Blockchain bereitstellen. Verwenden Sie den Befehl **Build Contracts** (Contracts erstellen), um alle Smart Contracts in Ihrem Projekt zu kompilieren.

1. Erweitern Sie in der Seitenleiste des VS Code-Explorers den Ordner **contracts** Ihres Projekts.
1. Klicken Sie mit der rechten Maustaste auf **HelloBlockchain.sol**, und wählen Sie im Menü die Option **Build Contracts** (Contracts erstellen).

    ![Erstellen von Contracts](./media/send-transaction/build-contracts.png)

Vom Azure Blockchain Development Kit wird Truffle verwendet, um Smart Contracts zu kompilieren.

![Kompilieren der Ausgabe](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Bereitstellen eines Smart Contracts

Für Truffle werden Migrationsskripts verwendet, um Ihre Contracts in einem Ethereum-Netzwerk bereitzustellen. Migrationen sind JavaScript-Dateien, die im Verzeichnis **migrations** des Projekts enthalten sind.

1. Klicken Sie zum Bereitstellen Ihres Smart Contracts mit der rechten Maustaste auf **HelloBlockchain.sol**, und wählen Sie im Menü die Option **Deploy Contracts** (Contracts bereitstellen).
1. Wählen Sie Ihr Azure Blockchain-Konsortiumsnetzwerk unter **From truffle-config.js** (Aus truffle-config.js) aus. Das Blockchainnetzwerk für Konsortien wurde der Truffle-Konfigurationsdatei des Projekts hinzugefügt, als Sie das Projekt erstellt haben.
1. Wählen Sie **Generate mnemonic** (Mnemonisches Zeichen generieren). Wählen Sie einen Dateinamen aus, und speichern Sie die Datei im Projektordner. Beispiel: `myblockchainmember.env`. Die Datei mit dem mnemonischen Zeichen wird verwendet, um einen privaten Ethereum-Schlüssel für Ihr Blockchainmitglied zu generieren.

Für das Azure Blockchain Development Kit wird Truffle zum Ausführen des Migrationsskripts verwendet, um die Contracts für die Blockchain bereitzustellen.

![Erfolgreich bereitgestellter Contract](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Aufrufen einer Funktion des Contracts

Mit der Funktion **SendRequest** des Contracts **HelloBlockchain** wird die Zustandsvariable **RequestMessage** geändert. Die Änderung des Zustands eines Blockchainnetzwerks wird nicht per Transaktion durchgeführt. Sie können ein Skript erstellen, um die Funktion **SendRequest** per Transaktion auszuführen.

1. Erstellen Sie im Stammverzeichnis Ihres Truffle-Projekts eine neue Datei, und geben Sie ihr den Namen `sendrequest.js`. Fügen Sie der Datei den folgenden Web3-JavaScript-Code hinzu.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
        
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling SendRequest function for contract ", instance.address);
        return instance.SendRequest("Hello, blockchain!");
      }).then(function(result) {
        console.log("Transaction hash: ", result.tx);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Wenn mit dem Azure Blockchain Development Kit ein Projekt erstellt wird, wird die Truffle-Konfigurationsdatei mit den Endpunktdetails Ihres Blockchainnetzwerks für Konsortien generiert. Öffnen Sie in Ihrem Projekt die Datei **truffle-config.js**. In der Konfigurationsdatei sind zwei Netzwerke aufgeführt: ein benanntes Entwicklungsnetzwerk und ein Netzwerk mit demselben Namen wie das Konsortium.
1. Verwenden Sie Truffle im Terminalbereich von VS Code, um das Skript in Ihrem Blockchainnetzwerk für Konsortien auszuführen. Wählen Sie in der Menüleiste des Terminalbereichs die Registerkarte **Terminal** und in der Dropdownliste die Option **PowerShell**.

    ```PowerShell
    truffle exec sendrequest.js --network <blockchain network>
    ```

    Ersetzen Sie \<blockchain network\> durch den Namen des Blockchainnetzwerks, das in der Datei **truffle-config.js** definiert ist.

Truffle führt das Skript in Ihrem Blockchainnetzwerk aus.

![Skriptausgabe](./media/send-transaction/execute-transaction.png)

Wenn Sie die Funktion eines Contracts per Transaktion ausführen, wird die Transaktion erst verarbeitet, nachdem ein Block erstellt wurde. Bei Funktionen mit Ausführung per Transaktion wird anstelle eines Rückgabewerts eine Transaktions-ID zurückgegeben.

## <a name="query-contract-state"></a>Abfragen des Contract-Status

Smart Contract-Funktionen können den aktuellen Wert von Zustandsvariablen zurückgeben. Wir fügen eine Funktion hinzu, um den Wert einer Zustandsvariablen zurückzugeben.

1. Fügen Sie in der Datei **HelloBlockchain.sol** dem Smart Contract **HelloBlockchain** die Funktion **getMessage** hinzu.

    ``` solidity
    function getMessage() public view returns (string memory)
    {
        if (State == StateType.Request)
            return RequestMessage;
        else
            return ResponseMessage;
    }
    ```

    Die Funktion gibt die in einer Zustandsvariablen gespeicherte Nachricht basierend auf dem aktuellen Zustand des Contracts zurück.

1. Klicken Sie mit der rechten Maustaste auf **HelloBlockchain.sol**, und wählen Sie im Menü die Option **Build Contracts** (Contracts erstellen), um die Änderungen am Smart Contract zu kompilieren.
1. Klicken Sie zum Bereitstellen mit der rechten Maustaste auf **HelloBlockchain.sol**, und wählen Sie im Menü die Option **Deploy Contracts** (Contracts bereitstellen).
1. Erstellen Sie als Nächstes ein Skript, um die Funktion **getMessage** aufzurufen. Erstellen Sie im Stammverzeichnis Ihres Truffle-Projekts eine neue Datei, und geben Sie ihr den Namen `getmessage.js`. Fügen Sie der Datei den folgenden Web3-JavaScript-Code hinzu.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
    
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling getMessage function for contract ", instance.address);
        return instance.getMessage();
      }).then(function(result) {
        console.log("Request message value: ", result);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Verwenden Sie Truffle im Terminalbereich von VS Code, um das Skript in Ihrem Blockchainnetzwerk auszuführen. Wählen Sie in der Menüleiste des Terminalbereichs die Registerkarte **Terminal** und in der Dropdownliste die Option **PowerShell**.

    ```bash
    truffle exec getmessage.js --network <blockchain network>
    ```

    Ersetzen Sie \<blockchain network\> durch den Namen des Blockchainnetzwerks, das in der Datei **truffle-config.js** definiert ist.

Das Skript fragt den Smart Contract ab, indem die Funktion „getMessage“ aufgerufen wird. Der aktuelle Wert der Zustandsvariablen **RequestMessage** wird zurückgegeben.

![Skriptausgabe](./media/send-transaction/execute-get.png)

Beachten Sie, dass der Wert nicht **Hello, blockchain!** lautet. Stattdessen ist der zurückgegebene Wert ein Platzhalter. Wenn Sie den Contract ändern und bereitstellen, erhält er eine neue Adresse, und die Zustandsvariablen werden Werten im Smart Contract-Konstruktor zugewiesen. Im Truffle-Beispiel **2_deploy_contracts.js** für ein Migrationsskript wird der Smart Contract bereitgestellt und ein Platzhalterwert als Argument übergeben. Der Konstruktor legt die Zustandsvariable **RequestMessage** auf den Platzhalterwert fest, und der Wert wird zurückgegeben.

1. Führen Sie die Skripts **sendrequest.js** und **getmessage.js** erneut aus, um die Zustandsvariable **RequestMessage** festzulegen und den Wert abzufragen.

    ![Skriptausgabe](./media/send-transaction/execute-set-get.png)

    Mit **sendrequest.js** wird die Zustandsvariable **RequestMessage** auf **Hello, blockchain!** festgelegt. Mit **getmessage.js** wird der Contract nach dem Wert der Zustandsvariablen **RequestMessage** abgefragt, und **Hello, blockchain!** wird zurückgegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn sie nicht mehr benötigt werden, können Sie die Ressourcen löschen. Löschen Sie hierzu die Ressourcengruppe `myResourceGroup`, die Sie im Rahmen der Schnellstartanleitung unter *Erstellen eines Blockchainmitglieds* zur Vorbereitung erstellt haben.

So löschen Sie die Ressourcengruppe:

1. Navigieren Sie im Azure-Portal im linken Navigationsbereich zu **Ressourcengruppe**, und wählen Sie die Ressourcengruppe aus, die gelöscht werden soll.
1. Wählen Sie die Option **Ressourcengruppe löschen**. Überprüfen Sie den Löschvorgang, indem Sie den Ressourcengruppennamen eingeben und auf **Löschen** klicken.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mit dem Azure Blockchain Development Kit ein Solidity-Beispielprojekt erstellt. Sie haben einen Smart Contract erstellt und bereitgestellt und dann eine Funktion per Transaktion in einem Blockchain-Konsortiumsnetzwerk aufgerufen, das unter dem Azure Blockchain Service gehostet wird.

> [!div class="nextstepaction"]
> [Übersicht über die Entwicklung mit dem Azure Blockchain-Dienst](develop.md)
