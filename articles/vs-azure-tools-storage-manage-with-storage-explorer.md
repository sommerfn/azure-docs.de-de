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
ms.openlocfilehash: 2b923a381be9d6cdb1a83f582a541c9e17e117a4
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086626"
---
# <a name="get-started-with-storage-explorer"></a>Erste Schritte mit dem Storage-Explorer

## <a name="overview"></a>Übersicht

Microsoft Azure Storage-Explorer ist eine eigenständige App, mit der Sie unter Windows, macOS und Linux komfortabel mit Azure Storage-Daten arbeiten können. In diesem Artikel werden mehrere Möglichkeiten zum Herstellen einer Verbindung mit und zum Verwalten Ihrer Azure-Speicherkonten beschrieben.

![Microsoft Azure Storage-Explorer][0]

## <a name="prerequisites"></a>Voraussetzungen

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Storage-Explorer wird von folgenden Windows-Versionen unterstützt:

* Windows 10 (empfohlen)
* Windows 8
* Windows 7

Für alle Windows-Versionen ist mindestens .NET Framework 4.6.2 erforderlich.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Storage-Explorer wird von den folgenden macOS-Versionen unterstützt:

* macOS 10.12 „Sierra“ und höher

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Storage-Explorer ist für die meisten gängigen Linux-Distributionen im [Snap Store](https://snapcraft.io/storage-explorer) verfügbar und wird als Installationsmethode empfohlen. Das Storage-Explorer-Snap installiert automatisch alle seine Abhängigkeiten und Updates, sobald neue Versionen im Snap Store veröffentlicht werden.

Eine Liste der unterstützten Distributionen finden Sie auf der Seite [Installing snapd](https://snapcraft.io/docs/installing-snapd).

Storage-Explorer erfordert den Einsatz eines Kennwort-Managers, mit dem Sie möglicherweise manuell eine Verbindung herstellen müssen, damit Storage-Explorer ordnungsgemäß funktioniert. Sie können Storage-Explorer mit dem Kennwort-Manager Ihres Systems verbinden, indem Sie den folgenden Befehl ausführen:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Storage-Explorer ist auch als „tar. gz“-Download verfügbar, wobei Sie allerdings Abhängigkeiten manuell installieren müssen. Der „tar. gz“-Installation wird für die folgenden Linux-Distributionen unterstützt:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

Die „.tar.gz“-Installation könnte auch bei anderen Distributionen funktionieren, wird aber nur für diese aufgeführten Distributionen offiziell unterstützt.

Weitere Informationen zum Installieren von Storage-Explorer unter Linux finden Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

---

## <a name="download-and-install"></a>Herunterladen und Installieren

[Herunterladen und Installieren des Storage-Explorers](https://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Herstellen der Verbindung mit einem Speicherkonto oder Dienst

Es gibt verschiedene Möglichkeiten, wie Sie den Storage-Explorer mit Speicherkonten verbinden können. Generell haben Sie folgende Möglichkeiten:

* [Sich bei Azure anmelden, um auf Ihre Abonnements und deren Ressourcen zuzugreifen](#sign-in-to-azure)
* [Eine bestimmte Azure Storage- oder CosmosDB-Ressource anfügen](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

> [!NOTE]
> Um nach der Anmeldung Vollzugriff auf Ressourcen zu haben, benötigt Storage-Explorer Berechtigungen für sowohl die Verwaltung (Azure Resource Manager) als auch Datenebenen. Das bedeutet, dass Sie Azure Active Directory-Berechtigungen (Azure AD) benötigen, die Ihnen Zugriff auf Ihr Speicherkonto, die Container im Konto und Daten in den Containern gewähren. Wenn Sie nur über Berechtigungen auf Datenebene verfügen, erwägen Sie das [Hinzufügen einer Ressource über Azure AD](#add-a-resource-via-azure-ad). Weitere Informationen zu den spezifischen Berechtigungen, die Storage-Explorer benötigt, finden Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).

1. Wählen Sie in Storage-Explorer **Konten verwalten** aus, um zum Panel „Kontoverwaltung“ zu wechseln.

    ![Konten verwalten][1]

2. Im linken Bereich werden jetzt alle Azure-Konten angezeigt, bei denen Sie angemeldet sind. Wählen Sie zum Herstellen der Verbindung mit einem anderen Konto **Konto hinzufügen** aus.

3. Wenn Sie sich bei einer nationalen Cloud oder Azure Stack anmelden möchten, wählen Sie die Dropdownliste **Azure-Umgebung** aus, um die gewünschte Azure-Cloud auszuwählen. Nachdem Sie Ihre Umgebung gewählt haben, klicken Sie auf die Schaltfläche **Anmelden**. Weitere Informationen finden Sie unter [Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement](/azure-stack/user/azure-stack-storage-connect-se).

    ![Anmeldeoption][2]

4. Nach erfolgreicher Anmeldung mit einem Azure-Konto werden im linken Bereich das Konto und die mit diesem Konto verknüpften Azure Stack-Abonnements angezeigt. Wählen Sie die Azure-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Auswählen von **Alle Abonnements** wird in der Liste zwischen allen bzw. keinen Azure-Abonnements umgeschaltet.)

    ![Azure-Abonnements auswählen][3]

    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure-Abonnements verknüpft sind.

    ![Ausgewählte Azure-Abonnements][4]

### <a name="attach-a-specific-resource"></a>Anfügen einer bestimmten Ressourcengruppe

Es gibt mehrere Möglichkeiten des Anfügens an eine Ressource in Storage-Explorer:

* [Ressource über Azure AD hinzufügen](#add-a-resource-via-azure-ad). Wenn Sie nur auf Datenebene über Berechtigungen verfügen, wählen Sie diese Option, um einen Blobcontainer oder einen Azure Data Lake Storage Gen2 Blob-Speichercontainer hinzuzufügen.
* [Verbindungszeichenfolge verwenden](#use-a-connection-string). Wählen Sie diese Option, wenn Sie eine Verbindungszeichenfolge für ein Speicherkonto haben. Storage-Explorer unterstützt sowohl Schlüssel- als auch [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md)-Verbindungszeichenfolgen.
* [SAS-URI verwenden](#use-a-sas-uri). Wenn Sie einen [SAS-URI](storage/common/storage-dotnet-shared-access-signature-part-1.md) für einen Blobcontainer, eine Dateifreigabe, eine Warteschlange oder eine Tabelle haben, führen Sie damit das Anfügen an die Ressource durch. Sie können [Storage-Explorer](#generate-a-sas-in-storage-explorer) oder das [Azure-Portal](https://portal.azure.com) verwenden, um einen SAS URI zu erhalten.
* [Namen und Schlüssel verwenden](#use-a-name-and-key). Wenn Sie einen der Kontoschlüssel für Ihr Speicherkonto kennen, können Sie sich damit schneller verbinden. Die Schlüssel für Ihr Speicherkonto befinden sich im [Azure-Portal](https://portal.azure.com) im Panel **Zugriffsschlüssel** des Speicherkontos.
* [An lokalen Emulator anfügen](#attach-to-a-local-emulator). Wenn Sie einen der verfügbaren Azure Storage-Emulatoren verwenden, können Sie diese Option nutzen, um sich leichter mit Ihrem Emulator zu verbinden.
* [Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto mithilfe einer Verbindungszeichenfolge](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string). Wählen Sie diese Option, wenn Sie eine Verbindungszeichenfolge für eine CosmosDB-Instanz haben.
* [Herstellen einer Verbindung mit Azure Data Lake Store über den URI](#connect-to-azure-data-lake-store-by-uri). Wählen Sie diese Option, wenn Sie einen URI für Azure Data Lake Store haben.

#### <a name="add-a-resource-via-azure-ad"></a>Hinzufügen einer Ressource über Azure AD

1. Öffnen Sie das Dialogfeld **Verbinden**, indem Sie links auf der vertikalen Symbolleiste auf die Schaltfläche **Verbinden** klicken:

    ![Option „Mit Azure Storage verbinden“][9]

2. Falls noch nicht erfolgt, verwenden Sie die Option **Azure-Konto hinzufügen**, um sich beim Azure-Konto anzumelden, das Zugriff auf die Ressource hat. Kehren Sie nach der Anmeldung zum Dialogfeld **Verbinden** zurück.

3. Wählen Sie die Option **Ressource über Azure Active Directory (Azure AD) hinzufügen** und klicken Sie auf **Weiter**.

4. Wählen Sie das Azure-Konto und den Mandanten mit Zugriff auf die Azure Storage-Ressource aus, an die das Anfügen erfolgen soll. Klicken Sie auf **Weiter**.

5. Wählen Sie den Ressourcentyp, an den das Anfügen erfolgen soll, und geben Sie dann die Verbindungsinformationen ein. Die Informationen, die Sie auf dieser Seite eingeben, hängen vom hinzugefügten Ressourcentyp ab. Achten Sie darauf, dass Sie den richtigen Ressourcentyp wählen. Klicken Sie auf **Weiter**, nachdem Sie alle erforderlichen Informationen eingegeben haben.

6. Überprüfen Sie in der Verbindungszusammenfassung alle Informationen auf Richtigkeit. Wenn alles stimmt, wählen Sie **Verbinden** aus. Klicken Sie andernfalls auf die Schaltfläche **Zurück**, um zu den vorherigen Seiten zurückzukehren und falsche Informationen zu korrigieren.

Nachdem die Verbindung erfolgreich hinzugefügt wurde, wechselt die Ressourcenstruktur automatisch zum Knoten, der die Verbindung darstellt. Wenn dies nicht erfolgt, sehen Sie unter **Lokal und angefügt** > **Speicherkonten** >  **(Angefügte Container)**  > **Blobcontainer** nach. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte oder Sie nach erfolgreichem Hinzufügen der Verbindung nicht auf Ihre Daten zugreifen können, sehen Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-connection-string"></a>Verwenden einer Verbindungszeichenfolge

1. Öffnen Sie das Dialogfeld **Verbinden**, indem Sie links auf der vertikalen Symbolleiste auf die Schaltfläche **Verbinden** klicken:

    ![Option „Mit Azure Storage verbinden“][9]

2. Wählen Sie **Verbindungszeichenfolge verwenden** aus, und klicken Sie dann auf **Weiter**.

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung, und geben Sie Ihre Verbindungszeichenfolge ein. Klicken Sie anschließend auf **Weiter**.

4. Überprüfen Sie in der Verbindungszusammenfassung alle Informationen auf Richtigkeit. Wenn alles stimmt, wählen Sie **Verbinden** aus. Klicken Sie andernfalls auf die Schaltfläche **Zurück**, um zu den vorherigen Seiten zurückzukehren und falsche Informationen zu korrigieren.

Nachdem die Verbindung erfolgreich hinzugefügt wurde, wechselt die Ressourcenstruktur automatisch zum Knoten, der die Verbindung darstellt. Wenn dies nicht erfolgt, sehen Sie unter **Lokal und angefügt** > **Speicherkonten** nach. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte oder Sie nach erfolgreichem Hinzufügen der Verbindung nicht auf Ihre Daten zugreifen können, sehen Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-sas-uri"></a>Verwenden eines SAS-URI

1. Öffnen Sie das Dialogfeld **Verbinden**, indem Sie links auf der vertikalen Symbolleiste auf die Schaltfläche **Verbinden** klicken:

    ![Option „Mit Azure Storage verbinden“][9]

2. Wählen Sie **SAS-URI (Shared Access Signature) verwenden** aus, und klicken Sie auf **Weiter**.

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung, und geben Sie Ihren SAS-URI ein. Der Dienstendpunkt für den Ressourcentyp, den Sie anfügen, sollte automatisch eingetragen werden. Wenn Sie einen benutzerdefinierten Endpunkt verwenden, erfolgt dies möglicherweise nicht. Klicken Sie auf **Weiter**.

4. Überprüfen Sie in der Verbindungszusammenfassung alle Informationen auf Richtigkeit. Wenn alles stimmt, wählen Sie **Verbinden** aus. Klicken Sie andernfalls auf die Schaltfläche **Zurück**, um zu den vorherigen Seiten zurückzukehren und falsche Informationen zu korrigieren.

Nachdem die Verbindung erfolgreich hinzugefügt wurde, wechselt die Ressourcenstruktur automatisch zum Knoten, der die Verbindung darstellt. Falls nicht, überprüfen Sie unter **Lokal und angefügt** > **Speicherkonten** >  **(Angefügte Container)**  >  *den Dienstknoten des Containertyps, den Sie angefügt haben*. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte oder Sie nach erfolgreichem Hinzufügen der Verbindung nicht auf Ihre Daten zugreifen können, sehen Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="use-a-name-and-key"></a>Verwenden eines Namens und Schlüssels

1. Öffnen Sie das Dialogfeld **Verbinden**, indem Sie links auf der vertikalen Symbolleiste auf die Schaltfläche **Verbinden** klicken:

    ![Option „Mit Azure Storage verbinden“][9]

2. Aktivieren Sie **Einen Speicherkontonamen und -schlüssel verwenden**, und klicken Sie dann auf **Weiter**.

3. Wählen Sie einen Anzeigenamen für Ihre Verbindung.

4. Geben Sie Ihren Speicherkontonamen und einen seiner Zugriffsschlüssel ein.

5. Wählen Sie die **Speicherdomäne**, die Sie nutzen möchten, und klicken Sie dann auf **Weiter**.

6. Überprüfen Sie in der Verbindungszusammenfassung alle Informationen auf Richtigkeit. Wenn alles stimmt, wählen Sie **Verbinden** aus. Klicken Sie andernfalls auf die Schaltfläche **Zurück**, um zu den vorherigen Seiten zurückzukehren und falsche Informationen zu korrigieren.

Nachdem die Verbindung erfolgreich hinzugefügt wurde, wechselt die Ressourcenstruktur automatisch zum Knoten, der die Verbindung darstellt. Wenn dies nicht erfolgt, sehen Sie unter **Lokal und angefügt** > **Speicherkonten** nach. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte oder Sie nach erfolgreichem Hinzufügen der Verbindung nicht auf Ihre Daten zugreifen können, sehen Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="attach-to-a-local-emulator"></a>Anfügen an einen lokalen Emulator

Storage-Explorer unterstützt derzeit zwei offizielle Speicheremulatoren:
* [Azure Storage-Emulator](storage/common/storage-use-emulator.md) (nur Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS oder Linux)

Wenn Ihr Emulator an den Standardports lauscht wird, können Sie den Knoten **Emulator – Standardports** verwenden, den Sie unter **Lokal und angefügt** > **Speicherkonten** finden, um schnell auf Ihren Emulator zuzugreifen.

Wenn Sie einen anderen Namen für Ihre Verbindung verwenden möchten oder Ihr Emulator nicht auf den Standardports ausgeführt wird, führen Sie diese Schritte durch:

1. Starten Sie Ihren Emulator. Notieren Sie sich in diesem Fall die Ports, an denen der Emulator auf die einzelnen Diensttypen lauscht.

   > [!IMPORTANT]
   > Storage-Explorer startet Ihren Emulator nicht automatisch. Sie müssen ihn manuell starten.

2. Öffnen Sie das Dialogfeld **Verbinden**, indem Sie links auf der vertikalen Symbolleiste auf die Schaltfläche **Verbinden** klicken:

    ![Option „Mit Azure Storage verbinden“][9]

3. Wählen Sie **An einen lokalen Emulator anfügen** aus, und klicken Sie dann auf **Weiter**.

4. Wählen Sie einen Anzeigenamen für Ihre Verbindung, und geben Sie die Ports ein, an denen Ihr Emulator auf die einzelnen Diensttypen lauscht. Die Textfelder beginnen bei den meisten Emulatoren mit den Standardwerten für die Ports. Das Feld **Files-Port** bleibt leer, da keiner der offiziellen Emulatoren derzeit den Azure Files-Dienst unterstützt. Wenn der von Ihnen verwendete Emulator Azure Files unterstützt, können Sie den verwendeten Port eingeben. Klicken Sie anschließend auf **Weiter**.

5. Überprüfen Sie in der Verbindungszusammenfassung alle Informationen auf Richtigkeit. Wenn alles stimmt, wählen Sie **Verbinden** aus. Klicken Sie andernfalls auf die Schaltfläche **Zurück**, um zu den vorherigen Seiten zurückzukehren und falsche Informationen zu korrigieren.

Nachdem die Verbindung erfolgreich hinzugefügt wurde, wechselt die Ressourcenstruktur automatisch zum Knoten, der die Verbindung darstellt. Wenn dies nicht erfolgt, sehen Sie unter **Lokal und angefügt** > **Speicherkonten** nach. Wenn Storage-Explorer Ihre Verbindung nicht hinzufügen konnte oder Sie nach erfolgreichem Hinzufügen der Verbindung nicht auf Ihre Daten zugreifen können, sehen Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) nach.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto mithilfe einer Verbindungszeichenfolge

Als Alternative zur Verwaltung von Azure Cosmos DB-Konten über ein Azure-Abonnement können Sie sich auch über eine Verbindungszeichenfolge mit Azure Cosmos DB verbinden. Gehen Sie dazu folgendermaßen vor:

1. Suchen Sie links in der Ressourcenstruktur nach **Lokal und angefügt**. Klicken Sie mit der rechten Maustaste auf **Azure Cosmos DB-Konten**, und wählen Sie **Mit Azure-Cosmos-Datenbank verbinden** aus.

    ![Herstellen einer Verbindung mit Azure Cosmos DB mithilfe einer Verbindungszeichenfolge][21]

2. Wählen Sie die Azure Cosmos DB-API aus, fügen Sie Ihre **Verbindungszeichenfolge** ein, und klicken Sie dann auf **OK**, um eine Verbindung mit dem Azure Cosmos DB-Konto herzustellen. Informationen zum Abrufen der Verbindungszeichenfolge finden Sie unter [Abrufen der Verbindungszeichenfolge](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![Verbindungszeichenfolge][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Herstellen einer Verbindung mit Azure Data Lake Store über den URI

Wenn Sie auf eine Ressource zugreifen möchten, die nicht in Ihrem Abonnement enthalten ist, benötigen Sie jemanden, der auf diese Ressource zugreifen kann, um Ihnen den Ressourcen-URI mitzuteilen. Nach der Anmeldung können Sie sich über den URI mit Data Lake Store verbinden. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie den Storage-Explorer.
2. Erweitern Sie im linken Bereich die Option **Local and Attached** (Lokal und angefügt).
3. Klicken Sie mit der rechten Maustaste auf **Data Lake Store**. Klicken Sie im Kontextmenü **Mit Data Lake Store verbinden** aus.

    ![Kontextmenü „Mit Data Lake Store verbinden“](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Geben Sie den URI ein. Das Tool navigiert zum Speicherort der URL, den Sie gerade eingegeben haben.

    ![Dialogfeld „Mit Data Lake Store verbinden“](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Mit Data Lake Store verbinden: Ergebnis](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Generieren einer SAS in Storage-Explorer

### <a name="account-level-sas"></a>SAS auf Kontoebene

1. Klicken Sie mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie **Shared Access Signature abrufen** aus.

    ![Kontextmenüoption „SAS abrufen“][14]

2. Geben Sie im Dialogfeld **Shared Access Signature generieren** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und wählen Sie anschließend **Erstellen** aus.

    ![Dialogfeld „SAS abrufen“][15]

3. Sie können jetzt entweder die **Verbindungszeichenfolge** oder die unformatierte **Abfragezeichenfolge** in die Zwischenablage kopieren.

### <a name="service-level-sas"></a>SAS auf Dienstebene

[So erhalten Sie eine SAS für einen Blobcontainer in Storage-Explorer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Suchen nach Speicherkonten

Wenn Sie eine Speicherressource benötigen und nicht wissen, wo sie sich befindet, können Sie hierfür das Suchfeld oben im linken Bereich verwenden.

Sobald Sie Text in das Suchfeld eingeben, werden im linken Bereich alle Ressourcen angezeigt, die dem bislang eingegebenen Suchwert entsprechen. Im folgenden Screenshot wird zum Beispiel die Suche nach dem Begriff **Endpunkte** gezeigt:

![Speicherkontosuche][23]

> [!NOTE]
> Deaktivieren Sie zum Beschleunigen Ihrer Suche im Panel „Kontoverwaltung“ alle Abonnements, die das gesuchte Element nicht enthalten. Sie können auch mit der rechten Maustaste auf einen Knoten klicken, und **Ab hier suchen** wählen, um die Suche ab einem bestimmten Knoten zu starten.
>
>

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Azure Blob Storage-Ressourcen mit Storage-Explorer](vs-azure-tools-storage-explorer-blobs.md)
* [Verwalten von Azure Cosmos DB in Storage-Explorer (Vorschau)](./cosmos-db/storage-explorer.md)
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
