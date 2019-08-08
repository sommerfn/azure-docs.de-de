---
title: Sichern von Clustern in Azure Data Explorer
description: In diesem Artikel wird beschrieben, wie Sie Cluster in Azure Data Explorer im Azure-Portal sichern.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: 86350f21f5c530a00560c92cc0ae2fd58c9a2c57
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780053"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Sichern von Clustern in Azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. Der Dienst bietet Volumeverschlüsselung für das Betriebssystem und die Datenträger Ihrer virtuellen Clustercomputer. Azure Disk Encryption ist außerdem mit [Azure Key Vault](/azure/key-vault/) integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die Geheimnisse steuern und verwalten können. So wird sichergestellt, dass alle ruhenden Daten auf den Datenträgern von virtuellen Computern in Azure Storage verschlüsselt sind. Sie können die Datenträgerverschlüsselung in Ihrem Cluster über die Einstellungen für die Clustersicherheit aktivieren.
  
## <a name="enable-encryption-at-rest"></a>Aktivieren der Verschlüsselung ruhender Daten
  
Die [Verschlüsselung ruhender Daten](/azure/security/fundamentals/encryption-atrest) in Ihrem Cluster bietet Schutz für (ruhende) gespeicherte Daten. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Data Explorer-Clusterressource. Wählen Sie unter der Überschrift **Einstellungen** die Option **Sicherheit** aus. 

    ![Verschlüsselung ruhender Schlüssel aktivieren](media/manage-cluster-security/security-encryption-at-rest.png)

1. Klicken Sie im Fenster **Sicherheit** neben der Sicherheitseinstellung **Datenträgerverschlüsselung** auf **Ein**. 

1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Überprüfen der Clusterintegrität](/azure/data-explorer/check-cluster-health)
