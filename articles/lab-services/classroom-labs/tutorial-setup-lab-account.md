---
title: Einrichten eines Lab-Kontos mit Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie ein Lab-Konto mit Azure Lab Services ein.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/24/2019
ms.author: spelluru
ms.openlocfilehash: 0977c4537e409b59be7f9031c488b3317f9f2f0f
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415792"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Tutorial: Einrichten eines Labkontos mit Azure Lab Services
In Azure Lab Services fungiert ein Lab-Konto als zentrales Konto, unter dem die Labs Ihrer Organisation verwaltet werden. In Ihrem Lab-Konto können Sie anderen Benutzern die Berechtigung zum Erstellen von Labs erteilen und Richtlinien festlegen, die für alle Labs unter dem Lab-Konto gelten. In diesem Tutorial erfahren Sie, wie Sie als Lab-Administrator ein Lab-Konto erstellen. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Lab-Kontos
> * Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
> * Angeben eines für Lab-Besitzer verfügbaren Marketplace-Images

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
Die folgenden Schritte veranschaulichen, wie Sie Azure-Portal verwenden, um ein Lab-Konto in Azure Lab Services zu erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus. Wählen Sie im Abschnitt **DEVOPS** die Option **Lab-Dienste** aus. Wenn Sie das Sternchen (`*`) neben **Lab-Dienste** auswählen, wird die Option im linken Menü dem Abschnitt **FAVORITEN** hinzugefügt. Beim nächsten Mal wählen Sie **Lab-Dienste** unter **FAVORITEN** aus.

    ![Alle Dienste -> Lab-Dienste](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Wählen Sie auf der Seite **Lab-Dienste** auf der Symbolleiste **Hinzufügen** aus. 

    ![Auswählen von „Hinzufügen“ auf der Seite „Labkonten“](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Führen Sie auf der Seite **Labkonto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Wählen Sie einen vorhandenen **Katalog mit freigegebenen Images** aus, oder erstellen Sie einen solchen. Sie können die Vorlagen-VM im Katalog mit den freigegebenen Images speichern, damit sie von anderen Benutzern wiederverwendet werden kann. Ausführliche Informationen zu Katalogen mit freigegebenen Images finden Sie unter [Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services](how-to-use-shared-image-gallery.md). 
    6. Wählen Sie als **Virtuelles Peernetzwerk** ein virtuelles Peernetzwerk (VNET) für das Lab-Netzwerk aus. In diesem Konto erstellte Labs sind mit dem ausgewählten VNET verbunden und haben Zugriff auf die Ressourcen im ausgewählten VNET. 
    7. Geben Sie einen **Adressbereich** für VMs im Lab an. Der Adressbereich muss in der CIDR-Notation (Classless Inter-Domain Routing) angegeben werden (Beispiel: 10.20.0.0/23). Virtuelle Computer im Lab werden in diesem Adressbereich erstellt. Weitere Informationen finden Sie unter [Angeben eines Adressbereichs für VMs im Lab](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).
    8. Geben Sie im Feld **Auswahl des Labstandorts** an, ob Sie es Laberstellern ermöglichen möchten, einen Standort für das Lab auszuwählen. Standardmäßig ist die Option deaktiviert. Wenn die Option deaktiviert ist, können Labersteller keinen Standort für das Lab angeben, das sie erstellen. Die Labs werden am nächstgelegenen geografischen Standort (relativ zum Labkonto) erstellt. Wenn die Option aktiviert ist, kann ein Labersteller bei der Laberstellung einen Standort auswählen. 
    9. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Wählen Sie auf der Symbolleiste das **Glockensymbol** (**Benachrichtigungen**), überprüfen Sie, ob die Bereitstellung erfolgreich war, und wählen Sie dann **Zu Ressource wechseln**. 

    Wählen Sie alternativ auf der Seite **Labkonten** die Option **Aktualisieren**, und wählen Sie anschließend das von Ihnen erstellte Labkonto aus. 

    ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, können Sie diesen Schritt überspringen. Führen Sie die folgenden Schritte aus, um zum Erstellen eines Classroom-Labs ein anderes Benutzerkonto zu verwenden: 

Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle **Lab-Ersteller** hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und wählen Sie auf der Symbolleiste **+Hinzufügen** und dann **+ Rollenzuweisung hinzufügen** aus. 

    ![Zugriffssteuerung > Schaltfläche „Rollenzuweisung hinzufügen“](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** als **Rolle** die Option **Ersteller des Labs** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Ersteller des Labs“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen des Lab-Erstellers](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>Angeben von für Lab-Ersteller verfügbare Marketplace-Images
Als Lab-Kontobesitzer können Sie die Marketplace-Images festlegen, die Lab-Ersteller zum Erstellen von Labs im Lab-Konto verwenden können. 

1. Wählen Sie links im Menü die Option **Marketplace-Images** aus. Standardmäßig wird die vollständige Liste der Images (sowohl aktivierte als auch deaktivierte) angezeigt. Sie können die Liste filtern, um nur aktivierte bzw. deaktivierte Images anzuzeigen. Wählen Sie dazu in der Dropdownliste oben die Option **Enabled only**/**Disabled only** (Nur aktivierte/Nur deaktivierte). 
    
    ![Seite mit Marketplace-Images](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Die Marketplace-Images, die in der Liste angezeigt werden, sind die einzigen, die die folgenden Bedingungen erfüllen:
        
    - Erstellt einen einzelnen virtuellen Computer
    - Verwendet den Azure Resource Manager zum Bereitstellen von virtuellen Computern
    - Erfordert nicht den Erwerb eines zusätzlichen Lizenzplans
2. Um ein aktiviertes Marketplace-Image zu **deaktivieren**, führen Sie eine der folgenden Aktionen aus: 
    1. Klicken Sie in der letzten Spalte auf die Ellipse (**...**) und dann auf **Disable image** (Image deaktivieren). 

        ![Deaktivieren eines Images](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Wählen Sie mindestens ein Image in der Liste aus, indem Sie die Kontrollkästchen vor den Imagenamen in der Liste aktivieren, und wählen Sie **Ausgewählte Images deaktivieren** aus. 

        ![Deaktivieren mehrerer Images](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Ebenso führen Sie zum **Aktivieren** eines Marketplace-Images eine der folgenden Aktionen aus: 
    1. Klicken Sie in der letzten Spalte auf die Ellipse (**...**) und dann auf **Enable image** (Image aktivieren). 
    2. Wählen Sie mindestens ein Image in der Liste aus, indem Sie die Kontrollkästchen vor den Imagenamen in der Liste aktivieren, und wählen Sie **Ausgewählte Images aktivieren** aus. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab-Konto erstellt. Um zu erfahren, wie Sie als beruflicher Benutzer ein Classroom-Lab erstellen, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)

