---
title: Aktivieren von Azure Disk Encryption für VM-Skalierungsgruppen
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Azure Disk Encryption für VM-Skalierungsgruppen aktivieren.
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: article
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 0097d0e1d5ea7de092da14683d4bab3d673b2219
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73177782"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Azure Disk Encryption für VM-Skalierungsgruppen

Azure Disk Encryption bietet Volumeverschlüsselung für Betriebssystemdatenträger und reguläre Datenträger Ihrer virtuellen Computer und trägt so zum Schutz Ihrer Daten bei, um die Sicherheits- und Compliancevorgaben der Organisation zu erfüllen. Weitere Informationen finden Sie unter [Azure Disk Encryption für Linux-VMs](../virtual-machines/linux/disk-encryption-overview.md) bzw. unter [Azure Disk Encryption für virtuelle Windows-Computer](../virtual-machines/windows/disk-encryption-overview.md).  

In folgenden Fällen kann Azure Disk Encryption auch auf VM-Skalierungsgruppen unter Windows und Linux angewendet werden:
- Skalierungsgruppen mit verwalteten Datenträgern. Für Skalierungsgruppen mit nativen (oder nicht verwalteten) Datenträgern wird Azure Disk Encryption nicht unterstützt.
- Betriebssystem- und Datenvolumes in Windows-Skalierungsgruppen
- Datenvolumes in Linux-Skalierungsgruppen. Betriebssystem-Datenträgerverschlüsselung wird zurzeit für Linux-Skalierungsgruppen NICHT unterstützt.

Anhand der Tutorials [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI](disk-encryption-cli.md) und [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell](disk-encryption-powershell.md) können Sie sich in wenigen Minuten mit den Grundlagen von Azure Disk Encryption für VM-Skalierungsgruppen vertraut machen.

## <a name="next-steps"></a>Nächste Schritte

- [Verschlüsseln von VM-Skalierungsgruppen mit Azure Resource Manager](disk-encryption-azure-resource-manager.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md)
- [Verwenden von Azure Disk Encryption mit Erweiterungssequenzierung der VM-Skalierungsgruppe](disk-encryption-extension-sequencing.md)
