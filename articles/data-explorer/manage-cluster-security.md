---
title: Sichern von Clustern in Azure Data Explorer
description: In diesem Artikel wird beschrieben, wie Sie Cluster in Azure Data Explorer im Azure-Portal sichern.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: ad08bb19f96aadad42e973eebb8adce6875e07b1
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876592"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Sichern von Clustern in Azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. Der Dienst bietet Volumeverschlüsselung für das Betriebssystem und die Datenträger Ihrer virtuellen Clustercomputer. Azure Disk Encryption ist außerdem mit [Azure Key Vault](/azure/key-vault/) integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die Geheimnisse steuern und verwalten können. So wird sichergestellt, dass alle ruhenden Daten auf den Datenträgern von virtuellen Computern in Azure Storage verschlüsselt sind. Sie können die Datenträgerverschlüsselung in Ihrem Cluster über die Einstellungen für die Clustersicherheit aktivieren.
  
## <a name="enable-encryption-at-rest"></a>Aktivieren der Verschlüsselung ruhender Daten
  
Die [Verschlüsselung ruhender Daten](/azure/security/fundamentals/encryption-atrest) in Ihrem Cluster bietet Schutz für (ruhende) gespeicherte Daten. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Data Explorer-Clusterressource. Wählen Sie unter der Überschrift **Einstellungen** die Option **Sicherheit** aus. 

    ![Verschlüsselung ruhender Schlüssel aktivieren](media/manage-cluster-security/security-encryption-at-rest.png)

1. Klicken Sie im Fenster **Sicherheit** neben der Sicherheitseinstellung **Datenträgerverschlüsselung** auf **Ein**. 

1. Wählen Sie **Speichern** aus.
 
> [!NOTE]
> Die Verschlüsselung kann nach der Aktivierung wieder deaktiviert werden.

## <a name="next-steps"></a>Nächste Schritte

[Überprüfen der Clusterintegrität](/azure/data-explorer/check-cluster-health)
