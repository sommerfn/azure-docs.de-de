---
title: Azure PowerShell-Skriptbeispiel – Durchführen eines Upgrades für eine Service Fabric-Anwendung | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Durchführen eines Upgrades für eine Service Fabric-Anwendung.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 45186f497371b533451ff374e68b38f9a7eebe51
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035493"
---
# <a name="upgrade-a-service-fabric-application"></a>Durchführen eines Upgrades für eine Service Fabric-Anwendung

Mit diesem Beispielskript wird ein Upgrade für eine ausgeführte Service Fabric-Anwendungsinstanz auf Version 1.3.0 durchgeführt. Das Skript kopiert das neue Anwendungspaket in den Clusterimagespeicher, registriert den Anwendungstyp und entfernt das nicht benötigte Anwendungspaket.  Das Skript startet ein überwachtes Upgrade und prüft kontinuierlich den Upgradestatus, bis das Upgrade abgeschlossen ist oder ein Rollback ausgeführt wird. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Ruft alle Anwendungen im Service Fabric-Cluster oder in einer bestimmten Anwendung ab.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Ruft den Status eines Service Fabric-Anwendungsupgrades ab. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Ruft die für den Service Fabric-Cluster registrierten Service Fabric-Anwendungstypen ab. |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Hebt die Registrierung eines Service Fabric-Anwendungstyps auf.  |
| [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopiert ein Service Fabric-Anwendungspaket in den Imagespeicher.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Registriert einen Service Fabric-Anwendungstyp. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Führt ein Upgrade für eine Service Fabric-Anwendung auf die angegebene Anwendungstypversion durch. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Entfernt ein Service Fabric-Anwendungspakets aus dem Imagespeicher|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/?view=azureservicefabricps).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
