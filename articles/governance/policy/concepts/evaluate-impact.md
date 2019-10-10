---
title: Auswerten der Auswirkung einer neuen Azure-Richtlinie
description: Informieren Sie sich über den Prozess, der bei der Einführung einer neuen Richtlinie in Ihre Azure-Umgebung zu befolgen ist.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 2aee31d3e874f709d5ec30ed4ddc0ba0445d4130
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980764"
---
# <a name="evaluate-the-impact-of-a-new-azure-policy"></a>Auswerten der Auswirkung einer neuen Azure-Richtlinie

Azure Policy ist ein leistungsstarkes Tool zur Verwaltung Ihrer Azure-Ressourcen nach geschäftsspezifischen Standards und zur Erfüllung von Complianceanforderungen. Wenn Personen, Prozesse oder Pipelines Ressourcen erstellen oder aktualisieren, überprüft Azure Policy die Anforderung. Wenn die Auswirkung der Richtliniendefinition [Append](./effects.md#deny) oder [DeployIfNotExists](./effects.md#deployifnotexists) lautet, ändert Azure Policy die Anforderung oder ergänzt sie. Wenn die Auswirkung der Richtliniendefinition [Audit](./effects.md#audit) oder [AuditIfNotExists](./effects.md#auditifnotexists) lautet, veranlasst Azure Policy die Erstellung eines Aktivitätsprotokolleintrags. Und wenn die Auswirkung der Richtliniendefinition [Deny](./effects.md#deny) lautet, stoppt Azure Policy die Erstellung oder Änderung der Anforderung.

Wenn Sie wissen, dass die Richtlinie ordnungsgemäß definiert ist, entsprechen diese Ergebnisse genau den Erwartungen. Es ist jedoch wichtig zu überprüfen, dass eine neue Richtlinie ordnungsgemäß funktioniert, bevor ihr gestattet wird, die Arbeit zu ändern oder zu blockieren. Die Überprüfung muss sicherstellen, dass nur die vorgesehenen Ressourcen als nicht konform eingestuft und keine konformen Ressourcen fälschlicherweise in die Ergebnisse einbezogen werden (bekannt als _False Positive_).

Der empfohlene Ansatz für die Überprüfung einer neuen Richtliniendefinition besteht darin, die folgenden Schritte auszuführen:

- Präzises Definieren Ihrer Richtlinie
- Überprüfen Ihrer vorhandenen Ressourcen
- Überprüfen von neuen oder aktualisierten Ressourcenanforderungen
- Bereitstellen Ihrer Richtlinie für Ressourcen
- Kontinuierliche Überwachung

## <a name="tightly-define-your-policy"></a>Präzises Definieren Ihrer Richtlinie

Es ist wichtig zu verstehen, wie die Geschäftsrichtlinie als Richtliniendefinition und die Beziehung von Azure-Ressourcen zu anderen Azure-Diensten implementiert wird. Dieser Schritt wird durch [Identifizieren der Anforderungen](../tutorials/create-custom-policy-definition.md#identify-requirements) und [Bestimmen der Ressourceneigenschaften](../tutorials/create-custom-policy-definition.md#determine-resource-properties) durchgeführt.
Aber es ist auch wichtig, über die knappe Definition Ihrer Geschäftsrichtlinie hinauszuschauen. Steht in Ihrer Richtlinie z. B. „Alle virtuellen Computer müssen...“? Was ist mit anderen Azure-Diensten, die virtuelle Computer nutzen, z. B. HDInsight oder AKS? Bei der Definition einer Richtlinie müssen wir berücksichtigen, wie sich diese Richtlinie auf Ressourcen auswirkt, die von anderen Diensten genutzt werden.

Aus diesem Grund sollten Ihre Richtliniendefinitionen so präzise wie möglich definiert sein und sich auf die Ressourcen und Eigenschaften konzentrieren, die Sie für die Compliance auswerten müssen.

## <a name="audit-existing-resources"></a>Überwachen vorhandener Ressourcen

Bevor Sie mit Ihrer neuen Richtliniendefinition neue oder aktualisierte Ressourcen verwalten, ist es am besten zu prüfen, wie sie eine begrenzte Teilmenge vorhandener Ressourcen, z. B. eine Testressourcengruppe, auswertet. Verwenden Sie den [Erzwingungsmodus](./assignment-structure.md#enforcement-mode)
_Deaktiviert_ (DoNotEnforce) bei Ihrer Richtlinienzuweisung, um zu verhindern, dass die [Auswirkung](./effects.md) ausgelöst oder Einträge im Aktivitätsprotokoll erstellt werden.

Dieser Schritt bietet Ihnen die Möglichkeit, die Complianceergebnisse der neuen Richtlinie für vorhandene Ressourcen auszuwerten, ohne den Workflow zu beeinträchtigen. Überprüfen Sie, dass keine konformen Ressourcen als nicht konform gekennzeichnet sind (_False Positive_) und dass alle Ressourcen, von denen Sie erwarten, dass sie nicht konform sind, korrekt gekennzeichnet sind.
Nachdem die anfängliche Teilmenge der Ressourcen wie erwartet überprüft wurde, erweitern Sie die Auswertung langsam auf alle vorhandenen Ressourcen.

Die auf diese Weise durchgeführte Auswertung der vorhandenen Ressourcen bietet auch die Möglichkeit, nicht konforme Ressourcen vor der vollständigen Implementierung der neuen Richtlinie zu bereinigen. Diese Bereinigung kann manuell oder über einen [Wartungstask](../how-to/remediate-resources.md) erfolgen, wenn die Auswirkung der Richtliniendefinition _DeployIfNotExists_ lautet.

## <a name="audit-new-or-updated-resources"></a>Überprüfen von neuen oder aktualisierten Ressourcen

Nachdem Sie überprüft haben, dass Ihre neue Richtliniendefinition ordnungsgemäß über vorhandene Ressourcen berichtet, ist es an der Zeit, die Auswirkungen der Richtlinie zu untersuchen, wenn Ressourcen erstellt oder aktualisiert werden. Wenn die Richtliniendefinition die Parametrisierung von Auswirkungen unterstützt, verwenden Sie [Überwachen](./effects.md#audit). Mit dieser Konfiguration können Sie die Erstellung und Aktualisierung von Ressourcen überwachen, um festzustellen, ob die neue Richtliniendefinition einen Eintrag im Azure-Aktivitätsprotokoll für eine nicht konforme Ressource auslöst, ohne die vorhandene Arbeit oder Anforderungen zu beeinträchtigen.

Es wird empfohlen, neue Ressourcen zu aktualisieren und zu erstellen, die Ihrer Richtliniendefinition entsprechen, um sicherzustellen, dass die Auswirkung von _Überwachen_ ordnungs- und erwartungsgemäß ausgelöst werden. Achten Sie auf Ressourcenanforderungen, die nicht von der neuen Richtliniendefinition betroffen sein sollten, die die Auswirkung von _Überwachen_ auslöst.
Diese betroffenen Ressourcen sind ein weiteres Beispiel für _False Positive_ und müssen vor der vollständigen Implementierung in der Richtliniendefinition festgelegt werden.

Für den Fall, dass die Richtliniendefinition in dieser Phase der Tests geändert wird, wird empfohlen, den Prüfungsprozess mit der Überwachung vorhandener Ressourcen zu beginnen. Eine Änderung der Richtliniendefinition für ein _False Positive_ bei neuen oder aktualisierten Ressourcen wird wahrscheinlich auch Auswirkungen auf vorhandene Ressourcen haben.

## <a name="deploy-your-policy-to-resources"></a>Bereitstellen Ihrer Richtlinie für Ressourcen

Nachdem Sie die Überprüfung Ihrer neuen Richtliniendefinition sowohl mit vorhandenen Ressourcen als auch mit neuen oder aktualisierten Ressourcenanforderungen abgeschlossen haben, beginnen Sie mit der Implementierung der Richtlinie. Es wird empfohlen, die Richtlinienzuweisung für die neue Richtliniendefinition zunächst für eine Teilmenge aller Ressourcen zu erstellen, z. B. für eine Ressourcengruppe. Erweitern Sie nach der Überprüfung der anfänglichen Bereitstellung den Geltungsbereich der Richtlinie auf immer umfassendere Ebenen, wie Abonnements und Verwaltungsgruppen. Diese Erweiterung wird erreicht, indem die Zuweisung entfernt und eine neue in den Zielbereichen erstellt wird, bis sie dem gesamten Ressourcenbereich zugewiesen wird, der von Ihrer neuen Richtliniendefinition abgedeckt werden soll.

Wenn während des Rollouts Ressourcen gefunden werden, die von Ihrer neuen Richtliniendefinition ausgenommen sein sollten, gehen Sie wie folgt vor:

- Aktualisieren Sie die Definition der Richtlinie, um sie expliziter zu formulieren, damit unbeabsichtigte Auswirkungen verringert werden.
- Ändern des Bereichs der Richtlinienzuweisung (durch Entfernen und Erstellen einer neuen Zuweisung)
- Hinzufügen der Gruppe von Ressourcen zur Ausschlussliste für die Richtlinienzuweisung

Alle Änderungen am Bereich (Ebene oder Ausschlüsse) sollten vollständig überprüft und mit Ihren Sicherheits- und Complianceorganisationen kommuniziert werden, um sicherzustellen, dass es keine Lücken bei der Abdeckung gibt.

## <a name="monitor-your-policy-and-compliance"></a>Überwachen Ihrer Richtlinie und Compliance

Die Implementierung und Zuweisung Ihrer Richtliniendefinition ist nicht der letzte Schritt. Überwachen Sie kontinuierlich die [Compliance](../how-to/get-compliance-data.md) der Ressourcen gemäß Ihrer neuen Richtliniendefinition, und richten Sie geeignete [Azure Monitor-Warnungen und -Benachrichtigungen](../../../azure-monitor/platform/alerts-overview.md) ein, wenn nicht konforme Geräte identifiziert werden. Es wird außerdem empfohlen, die Richtliniendefinition und die damit verbundenen Zuweisungen planmäßig auszuwerten, um zu überprüfen, ob die Richtliniendefinition den Anforderungen der Geschäftsrichtlinie und der Compliance entspricht. Richtlinien sollten entfernt werden, wenn sie nicht mehr benötigt werden. Richtlinien müssen auch von Zeit zu Zeit aktualisiert werden, wenn sich die zugrunde liegenden Azure-Ressourcen weiterentwickeln und neue Eigenschaften und Funktionen hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Struktur von Richtliniendefinitionen](./definition-structure.md).
- Erfahren Sie mehr über die [Struktur von Richtlinienzuweisungen](./assignment-structure.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).