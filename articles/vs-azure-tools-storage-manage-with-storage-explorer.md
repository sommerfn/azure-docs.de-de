---
title: Erste Schritte mit dem Storage-Explorer | Microsoft-Dokumentation
description: Verwalten von Azure Storage-Ressourcen mit dem Storage-Explorer
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2019
ms.author: cawa
ms.openlocfilehash: f7dd6d3d30f34ba2c69b40111bb28d484ce572e7
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508741"
---
# <a name="get-started-with-storage-explorer"></a>Erste Schritte mit dem Storage-Explorer

## <a name="overview"></a>Übersicht

Beim Azure Storage-Explorer handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Storage-Daten arbeiten können. In diesem Artikel werden mehrere Möglichkeiten zum Verbinden und Verwalten Ihrer Azure-Speicherkonten beschrieben.

![Microsoft Azure Storage-Explorer][0]

## <a name="prerequisites"></a>Voraussetzungen

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Der Azure Storage-Explorer wird in folgenden Windows-Versionen unterstützt:

* Windows 10 (empfohlen)
* Windows 8
* Windows 7

Für alle Versionen von Windows ist .NET Framework 4.6.2 oder höher erforderlich.

[Herunterladen und Installieren des Storage-Explorers](https://www.storageexplorer.com)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Der Azure Storage-Explorer wird in folgenden macOS-Versionen unterstützt:

* macOS 10.12 „Sierra“ und höher

[Herunterladen und Installieren des Storage-Explorers](https://www.storageexplorer.com)

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Der Azure Storage-Explorer wird in folgenden Linux-Verteilungen unterstützt:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

Der Azure Storage-Explorer funktioniert möglicherweise mit anderen Verteilungen, es werden jedoch nur die oben aufgeführten Versionen offiziell unterstützt.

Weitere Informationen zum Installieren von Storage-Explorer unter Linux finden Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

Die [Versionshinweise](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) des Azure Storage-Explorers enthalten für einige Verteilungen spezielle Schritte.

[Herunterladen und Installieren des Storage-Explorers](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Herstellen der Verbindung mit einem Speicherkonto oder Dienst

Es gibt verschiedene Möglichkeiten, wie Sie den Storage-Explorer mit Speicherkonten verbinden können. Generell haben Sie folgende Möglichkeiten:

* [Sie können sich bei Azure anmelden, um auf Ihre Abonnements und deren Ressourcen zuzugreifen](#sign-in-to-azure)
* [Sie können eine bestimmte Azure Storage oder CosmosDB-Ressource anhängen](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

> [!NOTE]
> Um nach der Anmeldung vollständig auf Ressourcen zugreifen zu können, benötigt der Storage-Explorer Berechtigungen für sowohl die Verwaltung (ARM) als auch Datenebenen. Das bedeutet, dass Sie Azure AD-Berechtigungen benötigen. Diese geben Ihnen Zugriff auf Ihr Speicherkonto, auf die Container in dem Konto und die Daten in den Containern. Wenn Sie die Berechtigungen nur für die Datenebene haben, dann ziehen Sie die Nutzung von [„Verbinden mit Azure AD“](#add-a-resource-via-azure-ad) in Betracht. Weitere Informationen über die genauen Berechtigung, die für Storage-Explorer notwendig sind, finden Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).
>
>

1. Wählen Sie im Storage-Explorer **Konten verwalten** aus, um zum **Bereich für die Kontoverwaltung** zu wechseln.

    ![Konten verwalten][1]

2. Im linken Bereich werden jetzt alle Azure-Konten angezeigt, bei denen Sie angemeldet sind. Wählen Sie zum Herstellen der Verbindung mit einem anderen Konto die Option **Konto hinzufügen**.

3. Wenn Sie sich bei einer nationalen Cloud oder Azure Stack anmelden möchten, klicken Sie auf die Dropdownliste **Azure-Umgebung**, um die gewünschte Azure-Cloud auszuwählen. Nachdem Sie Ihre Umgebung gewählt haben, klicken Sie auf die Schaltfläche **Anmelden...** . Weitere Informationen zur Anmeldung bei Azure Stack finden Sie unter [Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement](/azure-stack/user/azure-stack-storage-connect-se).

    ![Anmeldeoption][2]

4. Nach erfolgreicher Anmeldung mit einem Azure-Konto werden im linken Bereich das Konto und die mit diesem Konto verknüpften Azure Stack-Abonnements angezeigt. Wählen Sie die Azure-Abonnements, mit denen Sie arbeiten möchten, und klicken Sie dann auf **Anwenden** (durch Auswahl von **Alle Abonnements** werden alle bzw. keine der aufgelisteten Azure-Abonnements ausgewählt).

    ![Azure-Abonnements auswählen][3]

    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure-Abonnements verknüpft sind.

    ![Ausgewählte Azure-Abonnements][4]

### <a name="attach-a-specific-resource"></a>Verbinden einer bestimmten Ressourcengruppe
    
Dies sind einige Optionen zum Verbinden einer Ressource mit Storage-Explorer. Ihre Möglichkeiten:

* [Hinzufügen einer Ressource über Azure AD](#add-a-resource-via-azure-ad): Wenn Sie nur Berechtigungen in der Datenebene haben, dann können Sie mit dieser Option einen Blobcontainer oder einen ADLS Gen2-Blobcontainer hinzufügen.
* [Verwendung einer Verbindungszeichenfolge](#use-a-connection-string): Wenn Sie eine Verbindungszeichenfolge für ein Speicherkonto haben. Storage-Explorer unterstützt sowohl Schlüssel- als auch [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md)-Verbindungszeichenfolgen.
* [Verwendung einer SAS URI](#use-a-sas-uri): Wenn Sie eine [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) URI für einen Blobcontainer, eine Dateifreigabe, eine Warteschlange oder eine Tabelle haben. Sie können [Storage-Explorer](#generate-a-sas-in-storage-explorer) oder das [Azure-Portal](https://portal.azure.com) verwenden, um eine SAS URI zu erhalten.
* [Verwenden eines Namens und eines Schlüssels](#use-a-name-and-key): Wenn Sie einen Kontoschlüssel für Ihr Speicherkonto kennen, dann können Sie sich mit dieser Option schneller verbinden. Die Schlüssel für Ihr Speicherkonto befinden sich auf dem Blatt **„Zugriffsschlüssel“** des Speicherkontos im [Azure-Portal](https://portal.azure.com).
* [Verbinden mit einem lokalen Emulator](#attach-to-a-local-emulator): Wenn Sie einen der verfügbaren Azure Storage-Emulatoren verwenden, dann können Sie diese Option nutzen, um sich leichter mit Ihrem Emulator zu verbinden.
* [Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto mithilfe einer Verbindungszeichenfolge](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Wenn Sie eine Verbindungszeichenfolge für eine CosmosDB-Instanz haben.
* [Herstellen einer Verbindung mit Azure Data Lake Store über den URI](#connect-to-azure-data-lake-store-by-uri): Wenn Sie eine URI für ein Azure Data Lake Storage haben.

#### <a name="add-a-resource-via-azure-ad"></a>Hinzufügen einer Ressource über Azure AD

1. Öffnen Sie das **Dialogfeld „Verbinden“** , indem Sie auf die **Verbindungsschaltfläche** in der linken, vertikalen Symbolleiste klicken.

    ![Option „Mit Azure Storage verbinden“][9]

2. Falls Sie es noch nicht getan haben, verwenden Sie die Option **„Azure-Konto hinzufügen“** , um sich bei dem Azure-Konto anzumelden, das Zugriff auf die Ressource hat. Kehren Sie nach der Anmeldung zurück zum **Dialogfeld „Verbinden“** .

3. Wählen Sie die Option **„Hinzufügen einer Ressource über Azure Active Directory (Azure AD)“** und klicken Sie auf **„Weiter“** .

4. Wählen Sie das Azure-Konto aus, das Zugriff auf die Speicherressource hat, die Sie verbinden wollen, sowie das Abonnement, in dem sich die Ressource befindet. Klicken Sie dann auf **„Weiter“** .

5. Wählen Sie den Ressourcentyp, den Sie verbinden wollen, und geben Sie dann die Verbindungsinformationen ein. Die Eingaben auf dieser Seite ändern sich, je nach hinzugefügtem Ressourcentyp. Achten Sie darauf, dass Sie den richtigen Ressourcentyp wählen. Sobald Sie die erforderlichen Informationen eingegeben haben, können Sie auf **„Weiter“** klicken.

6. Überprüfen Sie die Verbindungszusammenfassung und stellen Sie sicher, dass alle Informationen stimmen. Wenn alle Informationen richtig aussehen, können Sie auf **„Verbinden“** . Wenn nicht, kehren Sie mit **„Zurück**“ zu den vorherigen Seiten zurück und korrigieren Sie die Informationen.

Sobald Sie die Verbindung erfolgreich hinzugefügt haben, wird die Ressourcenstruktur automatisch zu dem Knoten navigieren, der für die Verbindung steht. Sollte sie das nicht tun, überprüfen Sie **Local & Attached** → **Speicherkonten** → **(Verbundene Container)** → **Blobcontainer**. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte, oder Sie nicht auf Ihre Daten nach erfolgreichem Hinzufügen der Verbindung zugreifen können, dann sehen Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-connection-string"></a>Verwendung einer Verbindungszeichenfolge

1. Öffnen Sie das **Dialogfeld „Verbinden“** , indem Sie auf die **Verbindungsschaltfläche** in der linken, vertikalen Symbolleiste klicken.

    ![Option „Mit Azure Storage verbinden“][9]

2. Wählen Sie **„Verwendung einer Verbindungszeichenfolge“** und klicken Sie dann auf **„Weiter“** .

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung und geben Sie Ihre Verbindungszeichenfolge ein. Klicken Sie auf **Weiter**.

4. Überprüfen Sie die Verbindungszusammenfassung und stellen Sie sicher, dass alle Informationen stimmen. Wenn alle Informationen richtig aussehen, können Sie auf **„Verbinden“** . Wenn nicht, kehren Sie mit **„Zurück**“ zu den vorherigen Seiten zurück und korrigieren Sie die Informationen.

Sobald Sie die Verbindung erfolgreich hinzugefügt haben, wird die Ressourcenstruktur automatisch zu dem Knoten navigieren, der für die Verbindung steht. Sollte sie das nicht tun, überprüfen Sie **Local & Attached** → **Speicherkonten**. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte, oder Sie nicht auf Ihre Daten nach erfolgreichem Hinzufügen der Verbindung zugreifen können, dann sehen Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-sas-uri"></a>Verwendung einer SAS URI

1. Öffnen Sie das **Dialogfeld „Verbinden“** , indem Sie auf die **Verbindungsschaltfläche** in der linken, vertikalen Symbolleiste klicken.

    ![Option „Mit Azure Storage verbinden“][9]

2. Wählen Sie die Option **„Verwenden einer Shared Access Signature (SAS) URI“** und klicken Sie dann auf **„Weiter“** .

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung und geben Sie Ihre SAS URI ein. Der Dienstendpunkt für den Ressourcentyp, den Sie verbinden, sollte automatisch eingetragen werden. Wenn Sie einen benutzerdefinierten Endpunkt verwenden, kann es sein, dass dies nicht geschieht. Klicken Sie auf **Weiter**.

4. Überprüfen Sie die Verbindungszusammenfassung und stellen Sie sicher, dass alle Informationen stimmen. Wenn alle Informationen richtig aussehen, können Sie auf **„Verbinden“** . Wenn nicht, kehren Sie mit **„Zurück**“ zu den vorherigen Seiten zurück und korrigieren Sie die Informationen.

Sobald Sie die Verbindung erfolgreich hinzugefügt haben, wird die Ressourcenstruktur automatisch zu dem Knoten navigieren, der für die Verbindung steht. Sollte sie das nicht tun, überprüfen Sie **Local & Attached** → **Speicherkonten** → **(Verbundene Container)** → **der Dienstknoten für den Containertyp, den Sie verbunden haben**. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte, oder Sie nicht auf Ihre Daten nach erfolgreichem Hinzufügen der Verbindung zugreifen können, dann sehen Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-name-and-key"></a>Verwenden eines Namens und eines Schlüssels

1. Öffnen Sie das **Dialogfeld „Verbinden“** , indem Sie auf die **Verbindungsschaltfläche** in der linken, vertikalen Symbolleiste klicken.

    ![Option „Mit Azure Storage verbinden“][9]

2. Wählen Sie die Option **„Verwenden eines Speicherkontonamens und -schlüssels“** und klicken Sie dann auf **„Weiter“** .

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung.

4. Geben Sie Ihren Speicherkontonamen und einen der Zugriffsschlüssel ein.

5. Wählen Sie die **Speicherdomäne**, die Sie benutzen wollen, und klicken Sie dann auf **„Weiter“** .

4. Überprüfen Sie die Verbindungszusammenfassung und stellen Sie sicher, dass alle Informationen stimmen. Wenn alle Informationen richtig aussehen, können Sie auf **„Verbinden“** . Wenn nicht, kehren Sie mit **„Zurück**“ zu den vorherigen Seiten zurück und korrigieren Sie die Informationen.

Sobald Sie die Verbindung erfolgreich hinzugefügt haben, wird die Ressourcenstruktur automatisch zu dem Knoten navigieren, der für die Verbindung steht. Sollte sie das nicht tun, überprüfen Sie **Local & Attached** → **Speicherkonten**. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte, oder Sie nicht auf Ihre Daten nach erfolgreichem Hinzufügen der Verbindung zugreifen können, dann sehen Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="attach-to-a-local-emulator"></a>Verbinden mit einem lokalen Emulator

Storage-Explorer unterstützt Emulatoren auf allen Plattformen. Die zwei offiziell verfügbaren Emulator sind:
* [Azure Storage Emulator](storage/common/storage-use-emulator.md) (nur für Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS oder Linux)

Wenn Ihr Emulator auf den Standardports ausgeführt wird, dann können Sie den Knoten **„Emulator – Standardports“** verwenden. Diesen finden Sie immer unter **„Local & Attached“** → **Speicherkonten**. Damit können Sie schnell auf Ihren Emulator zugreifen. Wenn Sie einen anderen Namen für Ihre Verbindung verwenden möchten oder Ihr Emulator nicht auf den Standardports läuft, dann führen Sie diese Schritte durch.

1. Starten Sie Ihren Emulator. Merken Sie sich, welche Ports der Emulator für jeden Diensttyp aufführt. Sie benötigen diese Informationen später.

   > [!IMPORTANT]
   > Storage-Explorer startet Ihren Emulator nicht automatisch. Sie müssen ihn selbst starten.

2. Öffnen Sie das **Dialogfeld „Verbinden“** , indem Sie auf die **Verbindungsschaltfläche** in der linken, vertikalen Symbolleiste klicken.

    ![Option „Mit Azure Storage verbinden“][9]

3. Wählen Sie **„Attach to a local emulator“** (An einen lokalen Emulator anfügen), und klicken Sie dann auf **„Weiter“** .

4. Wählen Sie einen Anzeigenamen für Ihre Verbindung und geben Sie die Ports ein, die Ihr Eingeben für jeden Diensttyp aufgeführt hat. Standardmäßig enthalten die Textfelder die Werte für die Standardports der meisten Emulatoren. Der **Files-Port** bleibt standardmäßig leer, da keiner der offiziellen Emulatoren derzeit den Azure Files-Dienst unterstützt. Wenn Ihr Emulator ihn jedoch unterstützt, können Sie den verwendeten Port eingeben. Klicken Sie auf **Weiter**.

5. Überprüfen Sie die Verbindungszusammenfassung und stellen Sie sicher, dass alle Informationen stimmen. Wenn alle Informationen richtig aussehen, können Sie auf **„Verbinden“** . Wenn nicht, kehren Sie mit **„Zurück**“ zu den vorherigen Seiten zurück und korrigieren Sie die Informationen.

Sobald Sie die Verbindung erfolgreich hinzugefügt haben, wird die Ressourcenstruktur automatisch zu dem Knoten navigieren, der für die Verbindung steht. Sollte sie das nicht tun, überprüfen Sie **Local & Attached** → **Speicherkonten**. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte, oder Sie nicht auf Ihre Daten nach erfolgreichem Hinzufügen der Verbindung zugreifen können, dann sehen Sie im [Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto mithilfe einer Verbindungszeichenfolge

Neben der Verwaltung von Azure Cosmos DB-Konten über das Azure-Abonnement kann eine Verbindung mit einer Azure Cosmos DB alternativ auch mit einer Verbindungszeichenfolge hergestellt werden. Gehen Sie folgendermaßen vor, um eine Verbindung mithilfe einer Verbindungszeichenfolge herzustellen.

1. Suchen Sie in der linken Struktur nach **Lokal und angefügt**, klicken Sie mit der rechten Maustaste auf **Azure Cosmos DB-Konten**, und wählen Sie **Mit Azure-Cosmos-Datenbank verbinden** aus.

    ![Herstellen einer Verbindung mit Azure Cosmos DB mithilfe einer Verbindungszeichenfolge][21]

2. Wählen Sie die Azure Cosmos DB-API aus, fügen Sie Ihre **Verbindungszeichenfolge** ein, und klicken Sie dann auf **OK**, um eine Verbindung mit dem Azure Cosmos DB-Konto herzustellen. Informationen zum Abrufen der Verbindungszeichenfolge finden Sie unter [Abrufen der Verbindungszeichenfolge](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![connection-string][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Herstellen einer Verbindung mit Azure Data Lake Store über den URI

Wenn Sie auf Ressourcen zugreifen möchten, die in Ihrem Abonnement nicht vorhanden sind, müssen andere Personen Ihnen die Berechtigung zum Abrufen des URI für die Ressourcen gewähren. In diesem Fall können Sie eine Verbindung mit der Data Lake Store-Instanz herstellen, indem Sie nach dem Anmelden den URI verwenden. Führen Sie die folgenden Schritte aus.

1. Öffnen Sie den Storage-Explorer.
2. Erweitern Sie im linken Bereich die Option **Local and Attached** (Lokal und angefügt).
3. Klicken Sie mit der rechten Maustaste auf **Data Lake Store**, und wählen Sie im Kontextmenü die Option **Connect to Data Lake Store...** (Verbindung mit Data Lake Store herstellen...).

    ![Kontextmenü „Connect to Data Lake Store...“ (Verbindung mit Data Lake Store herstellen...)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Geben Sie den URI ein. Das Tool navigiert zum gerade eingegebenen URL-Speicherort.

    ![Dialogfeld „Connect to Data Lake Store...“ (Verbindung mit Data Lake Store herstellen...)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Verbindung mit Data Lake Store herstellen – Ergebnis](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Generieren Sie eine SAS in Storage-Explorer

### <a name="account-level-sas"></a>Kontoebenen-SAS

1. Klicken Sie mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie **„Shared Access Signature abrufen...“** aus.

    ![Kontextmenüoption „SAS abrufen“][14]

2. Geben Sie im Dialogfeld **Shared Access Signature generieren** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und klicken Sie anschließend auf die Schaltfläche **Erstellen**.

    ![Dialogfeld „SAS abrufen“][15]

3. Sie können jetzt entweder die **Verbindungszeichenfolge** oder die **Rohabfragezeichenfolge** in Ihre Zwischenablage kopieren.

### <a name="service-level-sas"></a>Servicelevel-SAS

[So erhalten Sie eine SAS für einen Blobcontainer in Storage-Explorer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Suchen nach Speicherkonten

Wenn Sie eine Speicherressource suchen müssen und nicht wissen, wo sie sich befindet, können Sie hierfür das Suchfeld oben im linken Bereich verwenden.

Sobald Sie Text in das Suchfeld eingeben, werden im linken Bereich alle Ressourcen angezeigt, die dem bislang eingegebenen Suchwert entsprechen. Im folgenden Screenshot wird zum Beispiel die Suche nach dem Begriff **Endpunkte** gezeigt:

![Speicherkontosuche][23]

> [!NOTE]
> Deaktivieren Sie über das **Panel für Kontoverwaltung** alle Abonnements, die das gesuchte Element nicht enthalten, um die Ausführungsdauer Ihres Suchvorgangs zu verkürzen. Alternativ können Sie mit der rechten Maustaste auf einen Knoten klicken und **Ab hier suchen** wählen, um die Suche von einem bestimmten Knoten aus zu starten.
>
>

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Azure Blob Storage-Ressourcen mit dem Storage-Explorer](vs-azure-tools-storage-explorer-blobs.md)
* [Verwalten von Azure Cosmos DB in Azure Storage-Explorer (Vorschau)](./cosmos-db/storage-explorer.md)
* [Verwalten von Azure Data Lake Store-Ressourcen mit Storage-Explorer](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
