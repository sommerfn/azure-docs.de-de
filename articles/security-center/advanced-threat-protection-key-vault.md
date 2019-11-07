---
title: Einrichten von Advanced Threat Protection für Azure Key Vault | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Advanced Threat Protection für Azure Key Vault in Azure Security Center einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: b28f03f0997b6bb2c494c35cee9695a478906c47
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521456"
---
# <a name="how-to-set-up-advanced-threat-protection-for-azure-key-vault-preview"></a>Einrichten von Advanced Threat Protection für Azure Key Vault (Vorschau)

Advanced Threat Protection für Azure Key Vault bietet eine zusätzliche Ebene der Sicherheitsintelligenz. Dieses Tool erkennt potenziell schädliche Versuche, auf Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen. Mithilfe des nativen Advanced Threat Protection-Diensts von Security Center können Sie Bedrohungen beheben, ohne ein Sicherheitsexperte zu sein und ohne weitere Sicherheitsüberwachungssysteme erlernen zu müssen.

Wenn Security Center anomale Aktivitäten erkennt, werden Warnungen angezeigt. Außerdem wird der Administrator des Abonnements per E-Mail über die Details der verdächtigen Aktivität informiert und erhält Empfehlungen, wie er die identifizierten Bedrohungen untersuchen und beheben kann. 

> [!NOTE]
> Advanced Threat Protection für Azure Key Vault ist zurzeit nur in Regionen von Nordamerika verfügbar.

## <a name="to-set-up-advanced-threat-protection-from-azure-security-center"></a>So richten Sie Warnungen von Advanced Threat Protection im Azure Security Center ein

Standardmäßig ist Advanced Threat Protection für alle Ihre Key Vault-Konten aktiviert, wenn Sie den Tarif „Standard“ von Security Center abonniert haben (siehe [Preise](security-center-pricing.md)). 

So aktivieren oder deaktivieren Sie den Schutz für ein bestimmtes Abonnement:

1. Klicken Sie auf der Security Center-Randleiste auf **Preise & Einstellungen**.
1. Wählen Sie das Abonnement mit den Speicherkonten aus, für die Sie die Bedrohungserkennung aktivieren oder deaktivieren möchten.
1. Klicken Sie auf **Tarif**.
1. Suchen Sie in der Gruppe **Tarif nach Ressourcentyp auswählen** die Zeile „Key Vaults“, und klicken Sie auf **Aktiviert** oder **Deaktiviert**.
    [![Aktivieren oder Deaktivieren von Advanced Threat Protection für Key Vault in Azure Security Center](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)
1. Klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Advanced Threat Protection für Azure Key Vault aktivieren und deaktivieren. 

Weitere verwandte Informationen finden Sie im folgenden Artikel:

- [Bedrohungserkennung für die Azure Services-Ebenen der Azure-Dienste in Security Center](security-center-alerts-service-layer.md): In diesem Artikel werden die Warnungen im Zusammenhang mit Advanced Threat Protection für Azure Key Vault beschrieben.