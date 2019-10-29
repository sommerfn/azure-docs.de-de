---
title: Azure-Konfigurationen nach der Bereitstellung mithilfe von Erweiterungen
description: Hier erfahren Sie, wie Sie die Azure Resource Manager-Vorlagenerweiterungen verwenden, um Konfigurationen nach der Bereitstellung bereitzustellen.
services: azure-resource-manager
documentationcenter: na
author: mumian
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: ccac91553d43a98bdef20abe451e7abec139dbaf
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533733"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Bereitstellen von Konfigurationen nach der Bereitstellung mithilfe von Erweiterungen

Vorlagenerweiterungen sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf Azure-Ressourcen nach der Bereitstellung ermöglichen. Die beliebtesten sind VM-Erweiterungen. Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer (Windows)](../virtual-machines/extensions/features-windows.md) sowie unter [Erweiterungen und Features für virtuelle Computer (Linux)](../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Erweiterungen

Die vorhandenen Erweiterungen sind:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters/extensions)
- [Microsoft.Sql servers/databases/extensions](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Um die verfügbaren Erweiterungen herauszufinden, navigieren Sie zur [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/). Geben Sie in **Nach Titel filtern** die **Erweiterung** ein.

Informationen zur Verwendung dieser Erweiterungen finden Sie unter:

- [Tutorial: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-vm-extensions.md).
- [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-vm-extensions.md)
