---
title: Konfigurieren eines Katalogs mit freigegebenen Images in Azure DevTest Labs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Katalog mit freigegebenen Images in Azure DevTest Labs konfigurieren.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2019
ms.author: spelluru
ms.openlocfilehash: 80610168e0d293b65626da71ee349f25e456576b
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774566"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Konfigurieren eines gemeinsamen Image-Katalogs in Azure DevTest Labs
DevTest Labs unterstützt jetzt die Funktion [Katalog mit freigegebenen Images](../virtual-machines/windows/shared-image-galleries.md). Diese Funktion ermöglicht es Labbenutzern, beim Erstellen von Labressourcen auf Images an einem freigegebenen Speicherort zuzugreifen. Außerdem hilft sie Ihnen, Ihre benutzerdefinierten verwalteten VM-Images zu strukturieren und zu organisieren. Die Funktion „Katalog mit freigegebenen Images“ unterstützt Folgendes:

- Verwaltete globale Replikation von Images.
- Versionsverwaltung und Gruppierung von Images zur einfacheren Verwaltung.
- Hochverfügbarkeit für Images mithilfe von ZRS-Konten (Zone Redundant Storage, zonenredundanter Speicher) in Regionen, die Verfügbarkeitszonen unterstützen. ZRS bietet bessere Ausfallsicherheit bei zonenbezogenen Fehlern.
- Abonnement- und sogar mandantenübergreifende Freigabe von Images mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC).

Weitere Informationen finden Sie in der [Dokumentation zum Katalog mit freigegebenen Images](../virtual-machines/windows/shared-image-galleries.md). 
 
Wenn Sie eine große Anzahl verwalteter Images haben, die Sie pflegen müssen und im gesamten Unternehmen zur Verfügung stellen möchten, können Sie einen Katalog mit geteilten Images als Repository verwenden, mit dem sie Ihre Images ganz einfach aktualisieren und teilen können. Als Labbesitzer können Sie einen vorhandenen Katalog mit freigegebenen Images an Ihr Lab anfügen. Nachdem der Katalog angefügt wurde, können Labbenutzer mit diesen aktuellen Images VMs erstellen. Ein Hauptvorteil dieser Funktion ist, dass Azure DevTest Labs Images jetzt lab-, abonnement- und regionsübergreifend freigeben kann. 

## <a name="considerations"></a>Überlegungen
- Sie können jeweils nur einen Katalog mit freigegebenen Images an ein Lab anfügen. Wenn Sie einen anderen Katalog anfügen möchten, müssen Sie zuvor den vorhandenen Katalog trennen. 
- DevTest Labs bietet zurzeit keine Möglichkeit, Images über das Lab in den Katalog hochzuladen. 
- Wenn Sie eine VM mit einem Image aus dem Katalog mit freigegebenen Images erstellen, verwendet DevTest Labs immer die aktuelle veröffentlichte Version des Images.
- DevTest Labs versucht sicherzustellen, dass der Katalog mit freigegebenen Images die Images automatisch in die Region des Labs repliziert, dies ist jedoch nicht immer möglich. Stellen Sie daher sicher, dass die Images bereits in die Region des Labs repliziert wurden, damit es nicht zu Problemen kommt, wenn Benutzer diese Images zum Erstellen von VMs verwenden.

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsmenü die Option **Alle Dienste** aus.
1. Wählen Sie in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste mit den Labs Ihr **Lab** aus.
1. Wählen Sie im Abschnitt **Einstellungen** die Option **Konfiguration und Richtlinien** aus.
1. Wählen Sie im linken Menü unter **Basen virtueller Computer** die Option **Kataloge mit freigegebenen Images** aus.

    ![Menüoption „Kataloge mit freigegebenen Images“](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Fügen Sie einen vorhandenen Katalog mit freigegebenen Images an Ihr Lab an, indem Sie auf die Schaltfläche **Anfügen** klicken und in der Dropdownliste Ihren Katalog auswählen.

    ![Attach](./media/configure-shared-image-gallery/attach-options.png)
1. Wechseln Sie zu Ihrem angefügten Katalog, und konfigurieren Sie ihn, um freigegebene Images für die Erstellung von VMs zu **aktivieren oder deaktivieren**. Wählen Sie einen Imagekatalog aus der Liste aus, um ihn zu konfigurieren. 

    **Die Verwendung aller Images als Basis für virtuelle Computer zulassen** ist standardmäßig auf **Ja** festgelegt. Das bedeutet, dass alle Images aus dem angefügten Katalog mit freigegebenen Images für einen Labbenutzer zur Verfügung stehen, wenn dieser einen neuen virtuellen Labcomputer erstellt. Wenn der Zugriff auf bestimmte Images eingeschränkt werden muss, ändern Sie die Option **Die Verwendung aller Images als Basis für virtuelle Computer zulassen** in **Nein**, wählen Sie die Images aus, die Sie bei der Erstellung virtueller Computer zulassen möchten, und wählen Sie anschließend die Schaltfläche **Speichern** aus.

    ![Aktivieren oder Deaktivieren](./media/configure-shared-image-gallery/enable-disable.png)
1. Labbenutzer können dann eine VM mit den aktivierten Images erstellen, indem sie auf **+ Hinzufügen** klicken und auf der Seite **Basis auswählen** nach dem Image suchen.

    ![Labbenutzer](./media/configure-shared-image-gallery/lab-users.png)
## <a name="use-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Anfügen eines Katalogs mit freigegebenen Images an Ihr Lab
Wenn Sie eine Azure Resource Manager-Vorlage verwenden, um einen Katalog mit freigegebenen Images an Ihr Lab anzufügen, müssen Sie ihn wie im folgenden Beispiel gezeigt im Ressourcenabschnitt Ihrer Resource Manager-Vorlage hinzufügen:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "mylab",
    "location": "eastus",
    "resources": [
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/mySharedGalleryRg/providers/Microsoft.Compute/galleries/mySharedGallery",
            "allowAllImages": "Enabled"
        }
    }
    ]
}
```

Ein vollständiges Beispiel für eine Resource Manager-Vorlage finden Sie in den Resource Manager-Vorlagenbeispielen in unserem öffentlichen GitHub-Repository: [Konfigurieren eines Katalogs mit freigegebenen Images beim Erstellen eines Labs](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured)

## <a name="use-api"></a>Verwenden der API

### <a name="shared-image-galleries---create-or-update"></a>Kataloge mit freigegebenen Images: Erstellen oder Aktualisieren

```rest
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}?api-version= 2018-10-15-preview
Body: 
{
    "properties":{
        "galleryId": "[Shared Image Gallery resource Id]",
        "allowAllImages": "Enabled"
    }
}

```

### <a name="shared-image-galleries-images---list"></a>Kataloge mit freigegebenen Images: Imageliste 

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
```




## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie einen virtuellen Computer auf der Grundlage eines Images aus dem angefügten Katalog mit freigegebenen Images erstellen: [Hinzufügen einer VM mit einem Image aus dem angefügten Katalog mit freigegebenen Images](add-vm-use-shared-image.md)
