---
title: Übersicht über Azure Resource Manager | Microsoft Docs
description: Es wird beschrieben, wie Sie den Azure-Ressourcen-Manager für die Bereitstellung, Verwaltung und Zugriffssteuerung von Ressourcen unter Azure verwenden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: overview
ms.date: 08/29/2019
ms.author: tomfitz
ms.openlocfilehash: 525271fdc94e0fd1b2cafe4a5a222ce9100156ef
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888474"
---
# <a name="azure-resource-manager-overview"></a>Übersicht über den Azure-Ressourcen-Manager

Azure Resource Manager ist der Bereitstellungs- und Verwaltungsdienst für Azure. Er bietet eine Verwaltungsebene, die das Erstellen, Aktualisieren und Löschen von Ressourcen in Ihrem Azure-Abonnement ermöglicht. Mithilfe von Verwaltungsfeatures wie Zugriffssteuerung, Sperren und Tags können Sie Ihre Ressourcen nach der Bereitstellung schützen und organisieren.

Informationen zu Azure Resource Manager-Vorlagen finden Sie in der [Übersicht über die Vorlagenbereitstellung](template-deployment-overview.md).

## <a name="consistent-management-layer"></a>Einheitliche Verwaltungsebene

Wenn ein Benutzer eine Anforderung über eines der Azure-Tools, über eine der APIs oder über eines der SDKs sendet, wird diese von Resource Manager empfangen. Die Anforderung wird authentifiziert und autorisiert. Anschließend sendet Resource Manager die Anforderung an den Azure-Dienst, der daraufhin die angeforderte Aktion ausführt. Da alle Anforderungen von der gleichen API verarbeitet werden, stehen in allen Tools konsistente Ergebnisse und Funktionen zur Verfügung.

Die folgende Abbildung veranschaulicht die Rolle von Azure Resource Manager bei der Behandlung von Azure-Anforderungen. 

![Resource Manager-Anforderungsmodell](./media/resource-group-overview/consistent-management-layer.png)

Alle Funktionen, die über das Azure-Portal verfügbar sind, stehen auch über PowerShell, die Azure CLI, die REST-APIs und über Client-SDKs zur Verfügung. Funktionen, die ursprünglich über APIs veröffentlicht wurden, werden innerhalb von 180 Tagen ab der ursprünglichen Veröffentlichung in das Portal aufgenommen.

## <a name="terminology"></a>Begriff

Wenn Sie mit dem Azure Resource Manager noch nicht vertraut sind, kennen Sie unter Umständen einige Begriffe noch nicht.

* **Ressource** : Ein verwaltbares Element, das über Azure verfügbar ist. Virtuelle Computer, Speicherkonten, Web-Apps, Datenbanken und virtuelle Netzwerke sind Beispiele für Ressourcen.
* **Ressourcengruppe** : Ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Die Ressourcengruppe enthält die Ressourcen, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, welche Ressourcen in eine Ressourcengruppen gehören. Weitere Informationen finden Sie unter [Ressourcengruppen](#resource-groups).
* **Ressourcenanbieter:** Ein Dienst, der Azure-Ressourcen bereitstellt. Ein allgemeiner Ressourcenanbieter ist beispielsweise „Microsoft.Compute“, der die VM-Ressource bereitstellt. „Microsoft.Storage“ ist ein weiterer allgemeiner Ressourcenanbieter. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](resource-manager-supported-services.md).
* **Resource Manager-Vorlage:** Eine JSON-Datei (JavaScript Object Notation), mit der eine oder mehrere Ressourcen zum Bereitstellen einer Ressourcengruppe oder eines Abonnements definiert werden. Die Vorlage kann zum konsistenten und wiederholten Bereitstellen der Ressourcen verwendet werden. Weitere Informationen finden Sie in der [Übersicht über die Vorlagenbereitstellung](template-deployment-overview.md).
* **Deklarative Syntax** : Bei dieser Syntax können Sie beispielsweise „Here is what I intend to create“ (Dies möchte ich erstellen) eingeben, ohne dafür die Folge der Programmierbefehle für die Erstellung schreiben zu müssen. Die Resource Manager-Vorlage ist ein Beispiel für die deklarative Syntax. In der Datei definieren Sie die Eigenschaften für die Infrastruktur zum Bereitstellen für Azure.  Weitere Informationen finden Sie in der [Übersicht über die Vorlagenbereitstellung](template-deployment-overview.md).

## <a name="the-benefits-of-using-resource-manager"></a>Vorteile der Verwendung des Ressourcen-Managers

Resource Manager ermöglicht Folgendes:

* Verwalten Ihrer Infrastruktur über deklarative Vorlagen (anstelle von Skripts)

* Bereitstellen, Verwalten und Überwachen aller Ressourcen für Ihre Lösung als Gruppe, anstatt diese Ressourcen einzeln zu behandeln

* Wiederholtes Bereitstellen Ihrer Lösung während des gesamten Entwicklungslebenszyklus mit der Gewissheit, dass Ihre Ressourcen konsistent bereitgestellt werden

* Definieren der Abhängigkeiten zwischen Ressourcen, sodass diese in der korrekten Reihenfolge bereitgestellt werden

* Anwenden der Zugriffssteuerung auf alle Dienste in der Ressourcengruppe, da die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) standardmäßig in die Verwaltungsplattform integriert ist

* Anwenden von Tags auf Ressourcen, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren

* Anzeigen der Kosten für eine Gruppe von Ressourcen mit dem gleichen Tag, um die Abrechnungstransparenz Ihrer Organisation zu verbessern

## <a name="understand-scope"></a>Der Bereich

Azure bietet vier Bereichsebenen: [Verwaltungsgruppen](../governance/management-groups/overview.md), Abonnements, [Ressourcengruppen](#resource-groups) und Ressourcen. Die folgende Abbildung zeigt ein Beispiel dieser Ebenen.

![`Scope`](./media/resource-group-overview/scope-levels.png)

Sie wenden die Verwaltungseinstellungen auf einer dieser Bereichsebenen an. Die von Ihnen ausgewählte Ebene bestimmt, wie umfassend die Einstellung angewendet wird. Niedrigere Ebenen erben die Einstellungen von höheren Ebenen. Wenn Sie also beispielsweise eine [Richtlinie](../governance/policy/overview.md) auf das Abonnement anwenden, gilt diese für alle Ressourcengruppen und Ressourcen in Ihrem Abonnement. Wenn Sie eine Richtlinie auf die Ressourcengruppe anwenden, gilt diese für die Ressourcengruppe und alle dazugehörigen Ressourcen. Andere Ressourcengruppen sind von der Richtlinienzuweisung dagegen nicht betroffen.

Sie können Vorlagen für Verwaltungsgruppen, Abonnements oder Ressourcengruppen bereitstellen.

## <a name="resource-groups"></a>Ressourcengruppen

Beim Definieren der Ressourcengruppe sind einige wichtige Faktoren zu beachten:

* Alle Ressourcen einer Gruppe sollten über den gleichen Lebenszyklus verfügen. Sie werden von Ihnen zusammen bereitgestellt, aktualisiert und gelöscht. Falls eine Ressource, z. B. ein Datenbankserver, in einem anderen Entwicklungszyklus vorhanden sein muss, sollte er in einer anderen Ressourcengruppe enthalten sein.

* Jede Ressource kann nur in einer Ressourcengruppe vorhanden sein.

* Sie können eine Ressource einer Ressourcengruppe jederzeit hinzufügen bzw. die Ressource daraus entfernen.

* Sie können eine Ressource aus einer Ressourcengruppe in eine andere Gruppe verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

* Eine Ressourcengruppe kann Ressourcen enthalten, die sich in unterschiedlichen Regionen befinden.

* Eine Ressourcengruppe kann zum Festlegen der Zugriffssteuerung für administrative Aktionen verwendet werden.

* Eine Ressource kann mit Ressourcen in anderen Ressourcengruppen interagieren. Zu dieser Interaktion kommt es häufig, wenn die beiden Ressourcen zusammengehören, aber nicht den gleichen Lebenszyklus aufweisen (beispielsweise Web-Apps, die eine Verbindung mit einer Datenbank herstellen).

Beim Erstellen einer Ressourcengruppe müssen Sie einen Standort für die Ressourcengruppe angeben. Vielleicht stellen Sie sich hierbei die folgende Frage: „Warum wird für eine Ressourcengruppe ein Standort benötigt? Und wenn die Ressourcen andere Standorte als die Ressourcengruppe aufweisen können, warum ist der Standort der Ressourcengruppe dann überhaupt wichtig?“ In der Ressourcengruppe werden Metadaten zu den Ressourcen gespeichert. Wenn Sie einen Standort für die Ressourcengruppe angeben, legen Sie fest, wo die Metadaten gespeichert werden. Aus Compliance-Gründen müssen Sie unter Umständen sicherstellen, dass Ihre Daten in einer bestimmten Region gespeichert werden.

Ist die Region der Ressourcengruppe vorübergehend nicht verfügbar, können Sie keine Ressourcen in der Ressourcengruppe aktualisieren, da die Metadaten nicht verfügbar sind. Die Ressourcen in anderen Regionen funktionieren weiterhin wie erwartet, doch können Sie diese nicht aktualisieren. Weitere Informationen zum Entwerfen zuverlässiger Anwendungen finden Sie unter [Entwerfen zuverlässiger Azure-Anwendungen](/azure/architecture/checklist/resiliency-per-service).

## <a name="resiliency-of-azure-resource-manager"></a>Resilienz von Azure Resource Manager

Der Azure Resource Manager-Dienst ist auf Resilienz und fortlaufende Verfügbarkeit ausgelegt. Resource Manager-Vorgänge und Vorgänge auf Steuerungsebene (an management.azure.com gesendete Anforderungen) in der REST-API haben folgende Eigenschaften:

* Sie sind regionsübergreifend verteilt. Einige Dienste sind regional.

* Sie sind an Standorten mit mehreren Verfügbarkeitszonen über Verfügbarkeitszonen (sowie Regionen) verteilt.

* Sie sind nicht von einem einzelnen logischen Rechenzentrum abhängig.

* Sie werden nie für Wartungsaktivitäten außer Betrieb genommen.

Diese Resilienz gilt für Dienste, die Anforderungen über Resource Manager empfangen. Key Vault profitiert beispielsweise von dieser Resilienz.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu allen von Ressourcenanbietern angebotenen Vorgängen finden Sie [Azure REST API Reference](/rest/api/azure/) (Referenz zu Azure-REST-APIs).

* Informationen zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

* Informationen zum Taggen von Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).

* Informationen zum Sperren von Ressourcen finden Sie unter [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](resource-group-lock-resources.md).

* Informationen zum Erstellen von Vorlagen für Bereitstellungen finden Sie in der [Übersicht über die Vorlagenbereitstellung](template-deployment-overview.md).
