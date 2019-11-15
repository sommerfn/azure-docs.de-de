---
title: Azure-Tabelle im kommerziellen Marketplace-Programm | Azure Marketplace
description: Konfigurieren der Leadverwaltung für Azure Blob
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 7/30/2019
ms.author: evansma
ms.openlocfilehash: 5da4e0ab315b3f66a477b816f6fc5d27de7aa339
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812367"
---
# <a name="lead-management-instructions-for-azure-blob"></a>Anweisungen für die Leadverwaltung für Azure Blob

>[!Caution]
>Die Azure Blob-Option zum Verarbeiten von Leads aus Ihrem Marketplace-Angebot ist veraltet. Wenn Sie derzeit über ein Angebot verfügen, das mit der Leadverwaltungskonfiguration für Azure Blob veröffentlicht wurde, erhalten Sie keine Kundenleads mehr. Aktualisieren Sie die Leadverwaltungskonfiguration, und legen Sie eine andere Leadverwaltungsoption fest. Weitere Informationen zu den anderen Optionen erhalten Sie auf der [Landing Page für die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Wenn Ihr CRM-System (Customer Relationship Management) im Partner Center nicht explizit für den Empfang von Azure Marketplace- und AppSource-Leads unterstützt wird, können Sie zum Behandeln dieser Leads einen Azure Blob verwenden. Sie können dann die Daten exportieren und in Ihr CRM-System importieren. Die Anweisungen in diesem Artikel führen Sie durch die Schritte zum Erstellen eines Azure Storage-Kontos und eines Azure Blob unter diesem Konto. Außerdem können Sie mit Microsoft Flow einen neuen Flow erstellen, um eine E-Mail-Benachrichtigung zu senden, wenn Ihr Angebot einen Lead erhält.


## <a name="how-to-configure-azure-blob"></a>Konfigurieren eines Azure Blob

1. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie ein [kostenloses Testkonto erstellen](https://azure.microsoft.com/pricing/free-trial/).
1. Nachdem Ihr Azure-Konto aktiviert wurde, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Gehen Sie folgendermaßen vor, um im Azure-Portal ein Speicherkonto zu erstellen.  
    1. Wählen Sie im Menü auf der linken Seite **+ Ressource erstellen** aus.  Auf der rechten Seite wird der Bereich (das Blatt) **Neu** angezeigt.
    2. Wählen Sie im Bereich **Neu** die Option **Speicher** aus.  Auf der rechten Seite wird eine Liste **Empfohlen** angezeigt.
    3. Wählen Sie das **Speicherkonto** aus, um mit der Kontoerstellung zu beginnen.  Befolgen Sie die Anweisungen im Artikel [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    ![Schritte zum Erstellen eines Azure Storage-Kontos](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-create.png)

    Weitere Informationen zu Speicherkonten finden Sie im [Schnellstarttutorial](https://docs.microsoft.com/azure/storage/).  Weitere Informationen zu den Preisen für Storage finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Warten Sie, bis Ihr Speicherkonto bereitgestellt wird. Dieser Prozess nimmt in der Regel einige Minuten in Anspruch.  Greifen Sie über die **Homepage** des Azure-Portals auf Ihr Speicherkonto zu, indem Sie **All Ihre Ressourcen anzeigen** oder im linken Navigationsbereich des Azure-Portals **Alle Ressourcen** auswählen.

    ![Zugriff auf Ihr Azure-Speicherkonto](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-access.png)

5. Wählen Sie im Speicherkontobereich **Zugriffsschlüssel** aus, und kopieren Sie den Wert von *Verbindungszeichenfolge* für den Schlüssel. Speichern Sie diesen Wert, da dies der Wert von *Verbindungszeichenfolge für Speicherkonto* ist, den Sie im Veröffentlichungsportal angeben müssen, um Leads für Ihr Marketplace-Angebot zu erhalten.

     Beispiel für eine Verbindungszeichenfolge:

     ```sql
     DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
     ```

    ![Azure-Speicherschlüssel](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-keys-2.png)

6. Klicken Sie auf Ihrer Speicherkontoseite auf **BLOBs**.

   ![Azure-Speicherschlüssel](./media/commercial-marketplace-lead-management-instructions-azure-blob/select-blobs.png)

7. Klicken Sie auf der BLOBs-Seite auf die Schaltfläche **+ Container**.

8. Geben Sie einen **Namen** für den neuen Container ein. Der Containername muss klein geschrieben werden, mit einem Buchstaben oder einer Zahl beginnen und darf nur Buchstaben, Zahlen und Bindestriche (-) enthalten. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

    Speichern Sie diesen Wert, da dies der *Containername* ist, den Sie im Veröffentlichungsportal angeben müssen, um Leads für Ihr Marketplace-Angebot zu erhalten.

9. Legen Sie die öffentliche Zugriffsebene für den Container auf **Privat (kein anonymer Zugriff)** fest.

10. Wählen Sie **OK** aus, um den Container zu erstellen.

    ![Neuer Container](./media/commercial-marketplace-lead-management-instructions-azure-blob/new-container.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-blob"></a>Konfigurieren des Angebots, sodass Leads an den Azure Blob gesendet werden

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren:

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
2. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus.

    ![„Verbinden“ für Angebot](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-offer.png)

3. Wählen Sie im Popupfenster „Verbindungsdetails“ **Azure Blob** als Leadziel aus.

    ![Verbindungsdetails](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-details.png) 

4. Geben Sie **Containername** und **Verbindungszeichenfolge für Speicherkonto** an, die Sie durch Befolgen dieser Anweisungen erhalten haben.

    * Beispiel für Containername: `marketplaceleadcontainer`
    * Beispiel für Verbindungszeichenfolge für Speicherkonto: `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net` ![Verbindungsdetails](./media/commercial-marketplace-lead-management-instructions-azure-blob/connection-details.png) 

5. Wählen Sie **Speichern** aus.

    > [!NOTE]
    > Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten.


