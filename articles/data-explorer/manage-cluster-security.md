---
title: Sichern von Clustern in Azure Data Explorer
description: In diesem Artikel wird beschrieben, wie Sie Cluster in Azure Data Explorer im Azure-Portal sichern.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: c6f7e921886a6acdaa31d0f69f57119c339c0b8b
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70172589"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Sichern von Clustern in Azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. Der Dienst bietet Volumeverschlüsselung für das Betriebssystem und die Datenträger Ihrer virtuellen Clustercomputer. Azure Disk Encryption ist außerdem mit [Azure Key Vault](/azure/key-vault/) integriert, um die Schlüssel und Geheimnisse für die Datenträgerverschlüsselung steuern und verwalten zu können. So wird sichergestellt, dass alle ruhenden Daten auf den Datenträgern von virtuellen Computern in Azure Storage verschlüsselt sind. 

Sie können die Datenträgerverschlüsselung in Ihrem Cluster über die Einstellungen für die Clustersicherheit aktivieren.
  
## <a name="enable-encryption-at-rest"></a>Aktivieren der Verschlüsselung ruhender Daten
  
Die [Verschlüsselung ruhender Daten](/azure/security/fundamentals/encryption-atrest) in Ihrem Cluster bietet Schutz für (ruhende) gespeicherte Daten. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Data Explorer-Clusterressource. Wählen Sie unter der Überschrift **Einstellungen** die Option **Sicherheit** aus. 

    ![Verschlüsselung ruhender Schlüssel aktivieren](media/manage-cluster-security/security-encryption-at-rest.png)

1. Klicken Sie im Fenster **Sicherheit** neben der Sicherheitseinstellung **Datenträgerverschlüsselung** auf **Ein**. 

1. Wählen Sie **Speichern** aus.
 
> [!NOTE]
> Wählen Sie **Aus** aus, um die Verschlüsselung zu deaktivieren, nachdem sie aktiviert wurde.

## <a name="next-steps"></a>Nächste Schritte

[Überprüfen der Clusterintegrität](/azure/data-explorer/check-cluster-health)
