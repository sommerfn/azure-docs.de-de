---
title: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen | Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Lösungsvorlagen im Azure Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 9/25/2019
ms.author: ellacroi
ms.openlocfilehash: 934a5e050e190c9a1f90bb3a22c2d1323a3ccecf
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73808296"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Azure-Anwendungen: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten

Lösungsvorlagen zählen zu den wichtigsten Optionen für die Veröffentlichung von Lösungen im Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Verwenden Sie eine Lösungsvorlage für Azure-Apps, wenn für eine Lösung neben einem einzelnen virtuellen Computer noch eine zusätzliche Bereitstellung und Konfigurationsautomatisierung erforderlich ist. Sie können die Bereitstellung einer oder mehrerer VMs mithilfe von Azure-Apps automatisieren: Lösungsvorlagen. Sie können auch Ressourcen für Netzwerke und Speicher zur Verfügung stellen. Azure-Apps: Angebotstyp der Lösungsvorlagen bietet Automatisierungsvorteile für Einzel-VMs und gesamte IaaS-basierte Lösungen.

Diese Lösungsvorlagen sind keine Transaktionsangebote, sondern können zur Bereitstellung bezahlter VM-Angebote verwendet werden, die über den kommerziellen Marketplace von Microsoft abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.


## <a name="requirements-for-solution-templates"></a>Anforderungen für Lösungsvorlagen

| **Anforderungen** | **Details**  |
| ---------------  | -----------  |
|Abrechnung und Messung    |  Die Ressourcen werden im Azure-Abonnement des Kunden bereitgestellt. Für virtuelle Computer mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYGO) wird die Transaktion mit dem Kunden über Microsoft und die Abrechnung über das Azure-Abonnement des Kunden (PAYGO) abgewickelt.  <br/> Im Falle von Bring-Your-Own-License (BYOL) rechnet Microsoft die im Kundenabonnement angefallenen Infrastrukturkosten ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.   |
|Azure-kompatible virtuelle Festplatte (VHD)  |   VMs müssen unter Windows oder Linux erstellt werden.  Weitere Informationen finden Sie unter [Erstellen einer Azure-kompatiblen VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md). |
| Zuordnen der Nutzung durch Kunden | Die Zuordnung der Nutzung durch Kunden muss für alle Lösungsvorlagen aktiviert werden, die über den Azure Marketplace veröffentlicht werden. Weitere Informationen zur Zuordnung der Nutzung durch Kunden sowie zu deren Aktivierung finden Sie unter [Zuordnen der Nutzung durch Kunden von Azure-Partnern](./azure-partner-customer-usage-attribution.md).  |
| Verwenden von verwalteten Datenträgern | Die Standardoption für persistierte Datenträger virtueller IaaS-Computer in Azure lautet [Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview). In Lösungsvorlagen muss Managed Disks verwendet werden. <br> <br> 1. Halten Sie sich an die [Anweisungen](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) und [Beispiele](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) für die Verwendung von Managed Disks in den Azure ARM-Vorlagen, um Ihre Lösungsvorlagen zu aktualisieren. <br> <br> 2. Befolgen Sie die nachfolgenden Anweisungen, um die zugrunde liegende VHD von Managed Disks in ein Storage-Konto zu importieren und die VHD als Image im Marketplace zu veröffentlichen: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Nächste Schritte
[Registrieren Sie sich](https://azuremarketplace.microsoft.com/sell) im Marketplace, sofern noch nicht geschehen.

Wenn Sie bereits registriert sind und ein neues Angebot erstellen oder ein bereits vorhandenes Angebot bearbeiten möchten, melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com) an, um Ihr Angebot zu erstellen oder fertigzustellen.
