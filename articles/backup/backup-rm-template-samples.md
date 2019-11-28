---
title: Azure-Ressourcen-Manager-Vorlagen
description: Azure Resource Manager-Vorlagen für die Verwendung mit Recovery Services-Tresoren und Azure Backup-Features
ms.topic: sample
ms.date: 01/31/2019
ms.custom: mvc
ms.openlocfilehash: 3c05f345c6c5df5d0350c75e52381c24b0609501
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172180"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Resource Manager-Vorlagen für Azure Backup

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für die Verwendung mit Recovery Services-Tresoren und Azure Backup-Features. Weitere Informationen zur JSON-Syntax und zu Eigenschaften finden Sie unter [Microsoft.RecoveryServices resource types](/azure/templates/microsoft.recoveryservices/allversions) (Microsoft.RecoveryServices-Ressourcentypen).

|   |   |
|---|---|
|**Recovery Services-Tresor** | |
| [Erstellen eines Recovery Services-Tresors](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Erstellen Sie einen Recovery Services-Tresor. Der Tresor kann für Azure Backup und für Azure Site Recovery verwendet werden. |
|**Sichern virtueller Computer**| |
| [Sichern von Resource Manager-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Verwenden Sie den vorhandenen Recovery Services-Tresor und die vorhandene Sicherungsrichtlinie, um Resource Manager-VMs in der gleichen Ressourcengruppe zu sichern.|
| [Sichern von IaaS-VMs im Recovery Services-Tresor](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Vorlage zum Sichern klassischer und Resource Manager-basierter virtueller Computer. |
| [Erstellen einer wöchentlichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer wöchentlichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer|
| [Erstellen einer täglichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer täglichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer|
| [Bereitstellen eines virtuellen Windows Server-Computers mit aktivierter Sicherung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Vorlage zum Erstellen eines virtuellen Windows Server-Computers und eines Recovery Services-Tresors mit aktivierter Standardsicherungsrichtlinie.|
|**Überwachen von Sicherungsaufträgen** |  |
| [Verwenden von Azure Monitor-Protokollen mit Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Vorlage zum Bereitstellen von Azure Monitor-Protokollen für Azure Backup zur Überwachung von Sicherungs- und Wiederherstellungsaufträgen, Sicherungswarnungen sowie zur Überwachung des Cloudspeichers, der in Ihren Recovery Services-Tresoren verwendet wird|  
|**Sichern von SQL Server auf einem virtuellen Azure-Computer** |  |
| [Sichern von SQL Server auf einem virtuellen Azure-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer Workload-spezifischen Sicherungsrichtlinie. Der virtuelle Computer wird beim Azure Backup-Dienst registriert, und der Schutz wird auf diesem virtuellen Computer konfiguriert. Gegenwärtig können nur SQL-Katalogimages verwendet werden. |
|   |   |
