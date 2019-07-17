---
title: 'Tutorial: Freigeben von Daten für Kunden und Partner mit Azure Data Share (Vorschauversion)'
description: 'Tutorial: Freigeben von Daten für Kunden und Partner mit Azure Data Share (Vorschauversion)'
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: ebd60607f9a24074e0fa985973dfc35674f0d66c
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788452"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Tutorial: Freigeben von Daten mit Azure Data Share (Vorschauversion)

Im Tutorial wird beschrieben, wie Sie eine neue Azure Data Share-Instanz (Datenfreigabe) einrichten und mit dem Freigeben Ihrer Daten für Kunden und Partner außerhalb Ihrer Azure-Organisation beginnen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Datenfreigabe
> * Hinzufügen von Datasets zu Ihrer Datenfreigabe
> * Aktivieren eines Synchronisierungszeitplans für Ihre Datenfreigabe 
> * Hinzufügen von Empfängern zu Ihrer Datenfreigabe 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-data-share-account"></a>Erstellen eines Data Share-Kontos

Erstellen Sie eine Azure Data Share-Ressource in einer Azure-Ressourcengruppe.

1. Klicken Sie in der linken oberen Ecke des Portals auf die Schaltfläche **Ressource erstellen** (+).

1. Suchen Sie nach *Data Share*.

1. Wählen Sie „Data Share“ (Vorschauversion) und dann die Option **Erstellen** aus.

1. Geben Sie die folgenden Informationen als grundlegende Details Ihrer Azure Data Share-Ressource ein. 

     **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | NAME | *datashareacount* | Geben Sie einen Namen für Ihr Data Share-Konto an. |
    | Abonnement | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihr Data Share-Konto verwenden möchten.|
    | Ressourcengruppe | *test-resource-group* | Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. |
    | Location | *USA, Osten 2* | Wählen Sie eine Region für Ihr Data Share-Konto aus.
    | | |

1. Wählen Sie **Erstellen**, um Ihr Data Share-Konto bereitzustellen. Die Bereitstellung eines neuen Data Share-Kontos dauert normalerweise maximal ca. 2 Minuten. 

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.

## <a name="create-a-data-share"></a>Erstellen einer Datenfreigabe

1. Navigieren Sie zu Ihrer Seite mit der Data Share-Übersicht.

    ![Freigeben von Daten](./media/share-receive-data.png "Freigeben von Daten") 

1. Wählen Sie **Start sharing your data** (Mit Freigabe der Daten beginnen).

1. Klicken Sie auf **Erstellen**.   

1. Geben Sie die Details für Ihre Datenfreigabe ein. Geben Sie einen Namen, eine Beschreibung der Freigabeinhalte und Nutzungsbedingungen (optional) an. 

    ![EnterShareDetails](./media/enter-share-details.png "Eingeben von Details zur Freigabe") 

1. Wählen Sie **Weiter**.

1. Wählen Sie zum Hinzufügen von Datasets zu Ihrer Datenfreigabe die Option **Add Datasets** (Datasets hinzufügen). 

    ![Datasets](./media/datasets.png "Datasets")

1. Wählen Sie den gewünschten Datasettyp für das Hinzufügen aus. 

    ![AddDatasets](./media/add-datasets.png "Hinzufügen von Datasets")    

1. Navigieren Sie zum Objekt, das Sie freigeben möchten, und wählen Sie „Datasets hinzufügen“. 

    ![SelectDatasets](./media/select-datasets.png "Auswählen von Datasets")    

1. Geben Sie auf der Registerkarte „Empfänger“ die E-Mail-Adressen Ihrer Datenconsumer ein, indem Sie die Option „+ Empfänger hinzufügen“ wählen. 

    ![AddRecipients](./media/add-recipient.png "Hinzufügen von Empfängern") 

1. Wählen Sie **Weiter**.

1. Aktivieren Sie den Zeitplan für Momentaufnahmen, wenn Sie möchten, dass Ihre Datenconsumer inkrementelle Updates Ihrer Daten erhalten. 

    ![EnableSnapshots](./media/enable-snapshots.png "Aktivieren von Momentaufnahmen") 

1. Wählen Sie eine Startzeit und ein Wiederholungsintervall aus. 

1. Wählen Sie **Weiter**.

1. Überprüfen Sie auf der Registerkarte „Bewerten + erstellen“ die Angaben für Paketinhalt, Einstellungen, Empfänger und Synchronisierungseinstellungen. Klicken Sie auf **Erstellen**

Ihre Azure Data Share-Instanz wurde jetzt erstellt, und der Empfänger Ihrer Datenfreigabe kann Ihre Einladung nun akzeptieren. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie eine Azure Data Share-Instanz erstellen und Empfänger einladen. Fahren Sie mit dem Tutorial [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md) fort, um sich darüber zu informieren, wie ein Datenconsumer eine Datenfreigabe akzeptieren und empfangen kann. 