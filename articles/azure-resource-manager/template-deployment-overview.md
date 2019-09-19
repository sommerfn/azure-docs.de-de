---
title: Azure Resource Manager-Vorlagen
description: Es wird beschrieben, wie Sie Azure Resource Manager-Vorlagen für die Bereitstellung von Ressourcen verwenden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/07/2019
ms.author: tomfitz
ms.openlocfilehash: 61e9bbabee969280c07521edb05d67ba68c0c58e
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802022"
---
# <a name="azure-resource-manager-templates"></a>Azure Resource Manager-Vorlagen

Mit dem Wechsel zur Cloud haben viele Teams agile Entwicklungsmethoden eingeführt. Diese Teams führen schnelle Iterationen durch. Sie müssen ihre Lösungen wiederholt in der Cloud bereitstellen und sicher sein, dass sich ihre Infrastruktur in einem zuverlässigen Zustand befindet. Da die Infrastruktur Teil des iterativen Prozesses geworden ist, gibt es keine Trennung mehr zwischen Betrieb und Entwicklung. Die Teams müssen Infrastruktur und Anwendungscode in einem einheitlichen Prozess verwalten.

Um diese Herausforderungen zu erfüllen, können Bereitstellungen automatisiert und die Infrastructure-as-Code-Methode verwendet werden. Im Code definieren Sie die Infrastruktur, die bereitgestellt werden muss. Der Infrastrukturcode wird Teil des Projekts. Genau wie Anwendungscode speichern Sie den Infrastrukturcode in einem Quellrepository und versionieren ihn. Jeder in Ihrem Team kann den Code ausführen und ähnliche Umgebungen bereitstellen.

Verwenden Sie Azure Resource Manager-Vorlagen, um Infrastructure-as-Code für Ihre Azure-Lösungen zu implementieren. Die Vorlage ist eine JSON-Datei (JavaScript Object Notation), in der die Infrastruktur und die Konfiguration für Ihr Projekt definiert sind. Für die Vorlage wird deklarative Syntax verwendet. Hiermit können Sie angeben, was Sie bereitstellen möchten, ohne dass Sie die Folge der Programmierbefehle für die Erstellung schreiben müssen. In der Vorlage geben Sie die bereitzustellenden Ressourcen und die Eigenschaften für diese Ressourcen an.

## <a name="benefits-of-resource-manager-templates"></a>Vorteile von Resource Manager-Vorlagen

Resource Manager-Vorlagen bieten die folgenden Vorteile:

* Bereitstellen, Verwalten und Überwachen aller Ressourcen für Ihre Lösung als Gruppe, anstatt diese Ressourcen einzeln zu behandeln

* Wiederholtes Bereitstellen Ihrer Lösung während des gesamten Entwicklungslebenszyklus und Gewährleistung, dass Ihre Ressourcen einheitlich bereitgestellt werden

* Verwalten Ihrer Infrastruktur über deklarative Vorlagen (anstelle von Skripts)

Bei der Entscheidung zwischen Resource Manager-Vorlagen und einem der anderen Infrastructure-as-Code-Dienste sollten Sie die folgenden Vorteile von Vorlagen gegenüber diesen Diensten berücksichtigen:

* Neue Azure-Dienste und -Features sind in Vorlagen sofort verfügbar. Sobald ein Ressourcenanbieter neue Ressourcen einführt, können Sie diese Ressourcen mithilfe von Vorlagen bereitstellen. Bei anderen Infrastructure-as-Code-Diensten müssen Sie darauf warten, dass Drittanbieter Schnittstellen für die neuen Ressourcen implementieren.

* Vorlagenbereitstellungen werden durch eine einzelne Übermittlung der Vorlage und nicht über mehrere imperative Befehle vorgenommen. Resource Manager orchestriert die Bereitstellung voneinander abhängiger Ressourcen, sodass diese in der richtigen Reihenfolge erstellt werden. Er analysiert die Vorlage und bestimmt die richtige Reihenfolge für die Bereitstellung auf der Grundlage von Verweisen zwischen Ressourcen.

   ![Vergleich von Vorlagenbereitstellungen](./media/template-deployment-overview/template-processing.png)

* Vorlagenbereitstellungen werden im Azure-Portal nachverfolgt. Sie können den Bereitstellungsverlauf überprüfen und Informationen zur Vorlagenbereitstellung abrufen. Sie können die bereitgestellte Vorlage, die übergebenen Parameterwerte und die Ausgabewerte anzeigen. Andere Infrastructure-as-Code-Dienste werden nicht über das Portal nachverfolgt.

   ![Bereitstellungsverlauf](./media/template-deployment-overview/deployment-history.png)

* Vorlagenbereitstellungen durchlaufen eine Preflight-Überprüfung. Resource Manager überprüft die Vorlage vor Beginn der Bereitstellung, um sicherzustellen, dass die Bereitstellung erfolgreich sein wird. Es ist weniger wahrscheinlich, dass die Bereitstellung in einem halbfertigen Zustand beendet wird.

* Wenn Sie [Azure-Richtlinien](../governance/policy/overview.md) verwenden, erfolgt die Richtlinienwartung für nicht kompatible Ressourcen bei der Bereitstellung über Vorlagen.

* Microsoft stellt [Blaupausen](../governance/blueprints/overview.md) für Bereitstellungen bereit, damit gesetzlichen und Compliancestandards entsprochen wird. Diese Blaupausen umfassen vorgefertigte Vorlagen für verschiedene Architekturen.

## <a name="idempotent"></a>Idempotent

Idempotent bedeutet einfach, dass Sie die gleichen Vorgänge mehrmals ausführen können und immer dasselbe Ergebnis erzielen. Das Bereitstellen einer Resource Manager-Vorlage ist idempotent. Sie können dieselbe Vorlage mehrmals bereitstellen und erhalten immer dieselben Ressourcentypen im selben Zustand. Dieses Konzept ist wichtig, da Sie dadurch konsistente Ergebnisse erhalten, und zwar unabhängig davon, ob Sie eine Vorlage erneut für eine vorhandene Ressourcengruppe bereitstellen oder eine Vorlage für eine neue Ressourcengruppe bereitstellen.

Angenommen, Sie haben drei Ressourcen für eine Ressourcengruppe bereitgestellt und entscheiden dann, dass Sie eine vierte Ressource hinzufügen müssen. Anstatt eine neue Vorlage zu erstellen, die nur die neue Ressource enthält, können Sie die vierte Ressource der vorhandenen Vorlage hinzufügen. Wenn Sie die neue Vorlage für die Ressourcengruppe bereitstellen, die bereits drei Ressourcen enthält, ermittelt Resource Manager, welche Aktionen durchzuführen sind.

Wenn die Ressource in der Ressourcengruppe vorhanden ist und die Anforderung keine Updates für Eigenschaften enthält, wird keine Aktion ausgeführt. Wenn die Ressource vorhanden ist, aber Eigenschaften geändert wurden, wird die vorhandene Ressource aktualisiert. Wenn die Ressource nicht vorhanden ist, wird die neue Ressource erstellt.

Wenn die Bereitstellung abgeschlossen ist, haben Sie die Gewissheit, dass sich die Ressourcen immer im erwarteten Zustand befinden.

## <a name="template-file"></a>Vorlagendatei

Innerhalb Ihrer Vorlage können Sie [Vorlagenausdrücke](template-expressions.md) schreiben, mit denen die Funktionen von JSON erweitert werden. Diese Ausdrücke verwenden die von Resource Manager bereitgestellten [Funktionen](resource-group-template-functions.md).

Die Vorlage hat die folgenden Abschnitte:

* [Parameter](template-parameters.md): Angeben von Werten während der Bereitstellung, mit denen dieselbe Vorlage für verschiedene Umgebungen verwendet werden kann.

* [Variablen](template-variables.md): Definieren von Werten, die in Ihren Vorlagen wiederverwendet werden. Diese können aus Parameterwerten erstellt werden.

* [Benutzerdefinierte Funktionen](template-user-defined-functions.md): Erstellen angepasster Funktionen zum Vereinfachen Ihrer Vorlage.

* [Ressourcen](resource-group-authoring-templates.md#resources): Angeben der bereitzustellenden Ressourcen.

* [Ausgaben](template-outputs.md): Zurückgeben von Werte aus den bereitgestellten Ressourcen.

## <a name="template-features"></a>Vorlagenfunktionen

Resource Manager analysiert Abhängigkeiten, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge erstellt werden. Die meisten Abhängigkeiten werden implizit bestimmt. Sie können jedoch eine Abhängigkeit explizit festlegen, um sicherzustellen, dass eine Ressource vor einer anderen Ressource bereitgestellt wird. Weitere Informationen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md).

Sie können Ihrer Vorlage eine Ressource hinzufügen und diese optional bereitstellen. In der Regel übergeben Sie einen Parameterwert, der angibt, ob die Ressource bereitgestellt werden muss. Weitere Informationen finden Sie unter [Bedingte Bereitstellung in Resource Manager-Vorlagen](conditional-resource-deployment.md).

Anstatt JSON-Blöcke mehrmals in der Vorlage zu wiederholen, können Sie ein copy-Element verwenden, um mehrere Instanzen einer Variablen, Eigenschaft oder Ressource anzugeben. Weitere Informationen finden Sie unter [Iteration von Ressourcen, Eigenschaften oder Variablen in Azure Resource Manager-Vorlagen](resource-group-create-multiple.md).

## <a name="export-templates"></a>Exportieren von Vorlagen

Sie können eine Vorlage für eine vorhandene Ressourcengruppe abrufen, indem Sie entweder den aktuellen Zustand der Ressourcengruppe exportieren oder die Vorlage anzeigen, die für eine bestimmte Bereitstellung verwendet wurde. Das Anzeigen der [exportierten Vorlage](export-template-portal.md) ist hilfreich, um sich über die Vorlagensyntax zu informieren.

Wenn Sie eine Lösung über das Portal erstellen, enthält sie automatisch eine Bereitstellungsvorlage. Sie müssen die Vorlage nicht völlig neu erstellen, da Sie mit der Vorlage für Ihre Lösung beginnen und diese dann an die speziellen Anforderungen anpassen können. Ein Beispiel finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).

## <a name="template-deployment-process"></a>Prozess für die Vorlagenbereitstellung

Wenn Sie eine Vorlage bereitstellen, konvertiert Resource Manager die Vorlage in REST-API-Vorgänge. Beispielsweise kann der Resource Manager eine Vorlage mit der folgenden Ressourcendefinition empfangen:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Er konvertiert die Definition in den folgenden REST-API-Vorgang, der an den Microsoft.Storage-Ressourcenanbieter gesendet wird:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

## <a name="template-design"></a>Vorlagenentwurf

Sie können völlig frei entscheiden, wie Sie Vorlagen und Ressourcengruppen definieren und Ihre Lösung verwalten möchten. Beispielsweise können Sie Ihre Anwendung mit drei Ebenen über eine einzelne Vorlage für eine einzelne Ressourcengruppe bereitstellen.

![Vorlage mit drei Ebenen](./media/template-deployment-overview/3-tier-template.png)

Sie müssen aber nicht die gesamte Infrastruktur in einer einzelnen Vorlage definieren. Oftmals ist es sinnvoll, die Bereitstellungsanforderungen in eine Gruppe von spezifischen, zweckgebundenen Vorlagen zu unterteilen. Sie können diese Vorlagen mühelos für verschiedene Lösungen erneut verwenden. Um eine bestimmte Lösung bereitzustellen, erstellen Sie eine Mastervorlage, die alle erforderlichen Vorlagen verknüpft. Die folgende Abbildung zeigt, wie Sie eine Lösung mit drei Ebenen über eine übergeordnete Vorlage bereitstellen, die drei geschachtelte Vorlagen enthält.

![Vorlage mit geschachtelten Ebenen](./media/template-deployment-overview/nested-tiers-template.png)

Wenn Ihre Ebenen über separate Lebenszyklen verfügen sollen, können Sie die drei Ebenen in separaten Ressourcengruppen bereitstellen. Beachten Sie, dass die Ressourcen weiterhin mit Ressourcen in anderen Ressourcengruppen verknüpft werden können.

![Ebenenvorlage](./media/template-deployment-overview/tier-templates.png)

Informationen zu geschachtelten Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den Eigenschaften in Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Einstieg in die Entwicklung von Vorlagen finden Sie unter [Verwenden von Visual Studio Code für die Erstellung von Azure Resource Manager-Vorlagen](resource-manager-tools-vs-code.md).
* Eine Einführung in den Resource Manager-Dienst, einschließlich der zugehörigen Verwaltungsfunktionen, finden Sie in der [Übersicht über Azure Resource Manager](resource-group-overview.md).

