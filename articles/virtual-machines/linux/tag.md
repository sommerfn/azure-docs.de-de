---
title: Markieren eines virtuellen Linux-Computers in Azure | Microsoft-Dokumentation
description: Informieren Sie sich über das Markieren eines virtuellen Linux-Computers unter Azure mit Tags, der in Azure mithilfe des Resource Manager-Bereitstellungsmodells erstellt wurde.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 290105b4e5e3ac3337b0be1b7d437601223bdf68
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708747"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Gewusst wie: Markieren eines virtuellen Linux-Computers in Azure
In diesem Artikel werden verschiedene Methoden zum Markieren eines virtuellen Linux-Computers in Azure mithilfe des Resource Manager-Bereitstellungsmodells beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Tags nur für Ressourcen unterstützt werden, die über das Resource Manager-Bereitstellungsmodell erstellt wurden.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Erstellen von Tags mit der Azure-Befehlszeilenschnittstelle

Die neueste Version der [Azure CLI](/cli/azure/install-azure-cli) muss installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/reference-index#az-login) bei einem Azure-Konto angemeldet sein.

Sie können alle Eigenschaften für einen bestimmten virtuellen Computer einschließlich der Tags anzeigen, indem Sie den folgenden Befehl verwenden:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Zum Hinzufügen eines neuen VM-Tags über die Azure-Befehlszeilenschnittstelle können Sie den `azure vm update` -Befehl zusammen mit dem Tag-Parameter **--set**verwenden:

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Um Tags zu entfernen, können Sie den **--remove**-Parameter im `azure vm update`-Befehl verwenden.

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Nun, da wir unseren Ressourcen über die Azure-Befehlszeilenschnittstelle und das Portal Tags zugewiesen haben, werfen wir einen Blick auf die Nutzungsdetails, um die Tags im Abrechnungsportal anzuzeigen.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Taggen Ihrer Azure-Ressourcen finden Sie in der [Übersicht über den Azure-Ressourcen-Manager][Azure Resource Manager Overview] sowie unter „Verwenden von Tags zum Organisieren von Azure-Ressourcen“. and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources]
* Unter [Grundlegendes zu Ihrer Microsoft Azure-Rechnung][Understanding your Azure Bill] und „Verwenden der Azure-Abrechnungs-APIs, um programmgesteuerte Einblicke in die Nutzung Ihrer Azure-Ressourcen zu erlangen“ erfahren Sie, wie Tags Sie bei der Verwaltung Ihrer Azure-Ressourcenverwendung unterstützen können. and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption]

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
