---
title: 'Tutorial: Freigeben außerhalb Ihrer Organisation: Azure Data Share'
description: 'Tutorial: Freigeben von Daten für Kunden und Partner mit Azure Data Share'
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 4ef9256404b0d0d4d6379e4f5a76c0d41a38c7cd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499320"
---
# <a name="tutorial-share-data-using-azure-data-share"></a>Tutorial: Freigeben von Daten mithilfe von Azure Data Share  

In diesem Tutorial wird beschrieben, wie Sie eine neue Azure Data Share-Instanz einrichten und mit dem Freigeben Ihrer Daten für Kunden und Partner außerhalb Ihrer Azure-Organisation beginnen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Datenfreigabe
> * Hinzufügen von Datasets zu Ihrer Datenfreigabe
> * Aktivieren eines Synchronisierungszeitplans für Ihre Datenfreigabe 
> * Hinzufügen von Empfängern zu Ihrer Datenfreigabe 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* E-Mail-Adresse für die Azure-Anmeldung Ihrer Empfänger (Ein E-Mail-Alias funktioniert nicht.)

### <a name="share-from-a-storage-account"></a>Freigeben über ein Speicherkonto:

* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (in der Berechtigung *Microsoft.Authorization/role assignments/write* enthalten). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden. 

### <a name="share-from-a-sql-based-source"></a>Freigeben über eine SQL-basierte Quelle:

* Eine Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Instanz mit Tabellen und Ansichten, die Sie freigeben möchten
* Berechtigung der Datenfreigabe für den Zugriff auf Data Warehouse. Die Berechtigung kann mit folgenden Schritten gewährt werden: 
    1. Legen Sie sich als Azure Active Directory-Administrator für den Server fest.
    1. Stellen Sie mithilfe von Azure Active Directory eine Verbindung mit der Azure SQL-Datenbank-/Data Warehouse-Instanz her.
    1. Führen Sie mit dem Abfrage-Editor (Vorschau) das folgende Skript aus, um die Data Share-MSI als „db_owner“ hinzuzufügen. Sie müssen mithilfe von Active Directory und nicht über die SQL Server-Authentifizierung eine Verbindung herstellen. 
    
```sql
    create user <share_acct_name> from external provider;     
    exec sp_addrolemember db_owner, <share_acct_name>; 
```                   
Beachten Sie, dass *<share_acc_name>* der Name Ihres Data Share-Kontos ist. Wenn Sie noch kein Data Share-Konto erstellt haben, können Sie später zu dieser Voraussetzung zurückkehren.  

* Client-IP-SQL Server-Firewallzugriff: Die Berechtigung kann mit folgenden Schritten gewährt werden: 1. Navigieren Sie zu *Firewalls und virtuelle Netzwerke*. 2. Legen Sie die Umschaltfläche auf **Ein** fest, um den Zugriff auf Azure-Dienste zuzulassen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-data-share-account"></a>Erstellen eines Data Share-Kontos

Erstellen Sie eine Azure Data Share-Ressource in einer Azure-Ressourcengruppe.

1. Klicken Sie in der linken oberen Ecke des Portals auf die Schaltfläche **Ressource erstellen** (+).

1. Suchen Sie nach *Data Share*.

1. Wählen Sie „Data Share“ und dann die Option **Erstellen** aus.

1. Geben Sie die folgenden Informationen als grundlegende Details Ihrer Azure Data Share-Ressource ein. 

     **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**
    |---|---|---|
    | NAME | *datashareacount* | Geben Sie einen Namen für Ihr Data Share-Konto an. |
    | Subscription | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihr Data Share-Konto verwenden möchten.|
    | Resource group | *test-resource-group* | Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. |
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

    ![EnterShareDetails](./media/enter-share-details.png "Eingeben der Details zur Freigabe") 

1. Wählen Sie **Weiter**.

1. Wählen Sie zum Hinzufügen von Datasets zu Ihrer Datenfreigabe die Option **Add Datasets** (Datasets hinzufügen). 

    ![Datasets](./media/datasets.png "Datasets")

1. Wählen Sie den gewünschten Datasettyp für das Hinzufügen aus. 

    ![AddDatasets](./media/add-datasets.png "Hinzufügen von Datasets")    

1. Navigieren Sie zum Objekt, das Sie freigeben möchten, und wählen Sie „Datasets hinzufügen“. 

    ![SelectDatasets](./media/select-datasets.png "Auswählen der Datasets")    

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
