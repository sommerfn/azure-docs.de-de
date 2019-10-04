---
title: Hinzufügen einer VM über ein freigegebenes Image in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Computer (VM) mithilfe eines Images aus dem angefügten Katalog mit freigegebenen Images in Azure DevTest Labs hinzufügen.
services: devtest-lab,virtual-machines
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
ms.openlocfilehash: c13b2c3d00a0ab0af0f7785feae645d907ffdaa3
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775786"
---
# <a name="add-a-vm-using-an-image-from-the-attached-shared-image-gallery"></a>Hinzufügen einer VM mit einem Image aus dem angefügten Katalog mit freigegebenen Images
Mit Azure DevTest Labs können Sie einen Katalog mit freigegebenen Images an Ihr Lab anfügen und dann Images im Katalog als Basis für Ihre im Lab erstellten VMs verwenden. Informationen zum Anfügen eines Katalogs mit freigegebenen Images an Ihr Lab finden Sie unter [Konfigurieren eines Katalogs mit freigegebenen Images](configure-shared-image-gallery.md). In diesem Artikel wird gezeigt, wie Sie Ihrem Lab mithilfe eines Images aus dem angefügten Katalog mit freigegebenen Images als Basis einen virtuellen Computer hinzufügen. 

## <a name="azure-portal"></a>Azure-Portal
In diesem Abschnitt erfahren Sie, wie Sie mit dem Azure-Portal Ihrem Lab einen virtuellen Computer hinzufügen, der auf einem Image aus dem angefügten Katalog mit freigegebenen Images basiert. Dieser Abschnitt enthält keine ausführlichen Schrittanleitungen zum Erstellen eines virtuellen Computers über das Azure-Portal. Diese Informationen finden Sie unter [Erstellen eines virtuellen Computers – Azure-Portal](devtest-lab-add-vm.md). Es werden nur die Schritte genauer behandelt, mit denen Sie ein Image aus dem angefügten Katalog mit freigegebenen Images und dann eine Version des zu verwendenden Images auswählen. 

Beim Hinzufügen eines virtuellen Computers zu Ihrem Lab können Sie ein Image aus dem angefügten Katalog mit freigegebenen Images als Basisimage auswählen: 

![Auswählen eines freigegebenen Images als Basis](./media/add-vm-use-shared-image/select-shared-image-for-base.png)

Anschließend können Sie auf der Registerkarte **Erweiterte Einstellungen** der Seite **Labressource erstellen** die Version des Images auswählen, das Sie als Basisimage verwenden möchten:

![Auswählen einer Imageversion](./media/add-vm-use-shared-image/select-version-shared-image.png)

Nach dem Erstellen der VM können Sie zu einer anderen Version des Images wechseln. 

## <a name="resource-manager-template"></a>Resource Manager-Vorlage
Wenn Sie mithilfe einer Azure Resource Manager-Vorlage einen virtuellen Computer mit einem Image aus dem Katalog mit freigegebenen Images erstellen, geben Sie im Abschnitt **Eigenschaften** einen Wert für **sharedImageId** an. Siehe folgendes Beispiel: 

```json
"resources": [
{
    ...
    "properties": {
         "sharedImageId": "/subscriptions/111111111-1111-1111-1111-111111111111/resourcegroups/mydtlrg/providers/microsoft.devtestlab/labs/mydtllab/sharedgalleries/spsig/sharedimages/myimagefromgallery",
        "sharedImageVersion": "1.0.1",
        ...
    }
}
],
```

Ein Beispiel für eine vollständige Resource Manager-Vorlage finden Sie im Beispiel [Create a virtual machine using a shared image gallery image](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage) (Erstellen eines virtuellen Computers mit einem Image aus dem Katalog mit freigegebenen Images) in unserem GitHub-Repository. 

## <a name="rest-api"></a>REST-API

1. Zunächst müssen Sie die ID des Images im Katalog mit freigegebenen Images abrufen. Eine Möglichkeit besteht darin, mit dem folgenden GET-Befehl alle Images im angefügten Katalog mit freigegebenen Images aufzulisten. 

    ```rest
    GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
    ```
2. Rufen Sie die PUT-Methode auf virtuellen Computern auf, indem Sie die ID des freigegebenen Images übergeben, das Sie aus dem vorherigen Aufruf von `properties.SharedImageId` erhalten haben.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Anfügen eines Katalogs mit freigegebenen Images an ein Lab und zum Konfigurieren des Katalogs finden Sie unter [Konfigurieren eines Katalogs mit freigegebenen Images](configure-shared-image-gallery.md).