---
title: Azure-Ressourcen-Manager-Vorlagen
description: Es wird beschrieben, wie Sie Azure Resource Manager-Vorlagen für die Bereitstellung von Ressourcen verwenden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: tomfitz
ms.openlocfilehash: 96f140cfa5e6151ad53ca242d1fc87ba3397316e
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "71300153"
---
# <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Mit dem Wechsel zur Cloud haben viele Teams agile Entwicklungsmethoden eingeführt. Diese Teams führen schnelle Iterationen durch. Sie müssen ihre Lösungen wiederholt in der Cloud bereitstellen und sicher sein, dass sich ihre Infrastruktur in einem zuverlässigen Zustand befindet. Da die Infrastruktur Teil des iterativen Prozesses geworden ist, gibt es keine Trennung mehr zwischen Betrieb und Entwicklung. Die Teams müssen Infrastruktur und Anwendungscode in einem einheitlichen Prozess verwalten.

Um diese Herausforderungen zu erfüllen, können Bereitstellungen automatisiert und die Infrastructure-as-Code-Methode verwendet werden. Im Code definieren Sie die Infrastruktur, die bereitgestellt werden muss. Der Infrastrukturcode wird Teil des Projekts. Genau wie Anwendungscode speichern Sie den Infrastrukturcode in einem Quellrepository und versionieren ihn. Jeder in Ihrem Team kann den Code ausführen und ähnliche Umgebungen bereitstellen.

Verwenden Sie Azure Resource Manager-Vorlagen, um Infrastructure-as-Code für Ihre Azure-Lösungen zu implementieren. Die Vorlage ist eine JSON-Datei (JavaScript Object Notation), in der die Infrastruktur und die Konfiguration für Ihr Projekt definiert sind. Für die Vorlage wird deklarative Syntax verwendet. Hiermit können Sie angeben, was Sie bereitstellen möchten, ohne dass Sie die Folge der Programmierbefehle für die Erstellung schreiben müssen. In der Vorlage geben Sie die bereitzustellenden Ressourcen und die Eigenschaften für diese Ressourcen an.

## <a name="why-choose-resource-manager-templates"></a>Warum sollten Sie Resource Manager-Vorlagen wählen?

Bei der Entscheidung zwischen Resource Manager-Vorlagen und einem der anderen Infrastructure-as-Code-Dienste sollten Sie die folgenden Vorteile von Vorlagen berücksichtigen:

* **Deklarative Syntax**: Mit Resource Manager-Vorlagen können Sie eine gesamte Azure-Infrastruktur deklarativ erstellen und bereitstellen. Sie können beispielsweise nicht nur virtuelle Computer bereitstellen, sondern auch die Netzwerkinfrastruktur, die Speichersysteme und alle anderen Ressourcen, die Sie möglicherweise benötigen.

* **Wiederholbare Ergebnisse**: Wiederholtes Bereitstellen Ihrer Infrastruktur während des gesamten Entwicklungslebenszyklus und Gewährleistung, dass Ihre Ressourcen einheitlich bereitgestellt werden. Vorlagen sind idempotent, d. h. Sie können dieselbe Vorlage mehrmals bereitstellen und erhalten immer dieselben Ressourcentypen im selben Zustand. Sie können eine Vorlage entwickeln, die den gewünschten Zustand darstellt, anstatt viele separate Vorlagen zur Darstellung von Aktualisierungen zu entwickeln.

* **Orchestrierung:** Sie müssen sich keine Gedanken über die Komplexität von Bestellvorgängen machen. Resource Manager orchestriert die Bereitstellung voneinander abhängiger Ressourcen, sodass diese in der richtigen Reihenfolge erstellt werden. Wenn möglich, stellt der Resource Manager Ressourcen parallel bereit, sodass Ihre Bereitstellungen schneller abgeschlossen werden als serielle Bereitstellungen. Sie stellen die Vorlage über einen Befehl statt über mehrere imperativer Befehle bereit.

   ![Vergleich von Vorlagenbereitstellungen](./media/template-deployment-overview/template-processing.png)

* **Integrierte Überprüfung**: Ihre Vorlage wird erst bereitgestellt, wenn sie die Überprüfung bestanden hat. Resource Manager überprüft die Vorlage vor Beginn der Bereitstellung, um sicherzustellen, dass die Bereitstellung erfolgreich sein wird. Es ist weniger wahrscheinlich, dass die Bereitstellung in einem halbfertigen Zustand beendet wird.

* **Modulare Dateien**: Sie können Ihre Vorlagen in kleinere, wiederverwendbare Komponenten unterteilen und sie zum Zeitpunkt der Bereitstellung miteinander verknüpfen. Sie können eine Vorlage auch in anderen Vorlagen schachteln.

* **Erstellen jeder Azure-Ressource**: Sie können neue Azure-Dienste und -Features sofort in Vorlagen verwenden. Sobald ein Ressourcenanbieter neue Ressourcen einführt, können Sie diese Ressourcen mithilfe von Vorlagen bereitstellen. Sie müssen nicht warten, bis Tools oder Module aktualisiert werden, bevor Sie die neuen Dienste verwenden.

* **Nachverfolgte Bereitstellungen**: Im Azure-Portal können Sie den Bereitstellungsverlauf überprüfen und Informationen zur Vorlagenbereitstellung abrufen. Sie können die bereitgestellte Vorlage, die übergebenen Parameterwerte und die Ausgabewerte anzeigen. Andere Infrastructure-as-Code-Dienste werden nicht über das Portal nachverfolgt.

   ![Bereitstellungsverlauf](./media/template-deployment-overview/deployment-history.png)

* **Richtlinie als Code**: [Azure Policy](../governance/policy/overview.md) ist eine Richtlinie in Form eines Codeframeworks zum Automatisieren der Governance. Wenn Sie Azure-Richtlinien verwenden, erfolgt die Richtlinienwartung für nicht kompatible Ressourcen bei der Bereitstellung über Vorlagen.

* **Bereitstellungsblaupausen**: Sie können von Microsoft bereitgestellte [Blaupausen](../governance/blueprints/overview.md) nutzen, um gesetzliche Vorgaben und Konformitätsstandards einzuhalten. Diese Blaupausen umfassen vorgefertigte Vorlagen für verschiedene Architekturen.

* **CI/CD-Integration**: Sie können Vorlagen in ihre Tools für Continuous Integration und Continuous Deployment (CI/CD) integrieren, die Ihre Releasepipelines für schnelle und zuverlässige Updates von Anwendungen und Infrastrukturen automatisieren können. Wenn Sie die Azure DevOps- und Resource Manager-Vorlagenaufgabe verwenden, können Sie Azure-Pipelines zum kontinuierlichen Erstellen und Bereitstellen von Azure Resource Manager-Vorlagenprojekten verwenden. Weitere Informationen finden Sie unter [VS-Projekte mit Pipelines](./vs-resource-groups-project-devops-pipelines.md) und [Continuous Integration mit Azure Pipelines](./resource-manager-tutorial-use-azure-pipelines.md).

* **Exportierbarer Code**: Sie können eine Vorlage für eine vorhandene Ressourcengruppe abrufen, indem Sie entweder den aktuellen Zustand der Ressourcengruppe exportieren oder die Vorlage anzeigen, die für eine bestimmte Bereitstellung verwendet wurde. Das Anzeigen der [exportierten Vorlage](export-template-portal.md) ist hilfreich, um sich über die Vorlagensyntax zu informieren.

* **Dokumenterstellungstools**: Sie können Vorlagen mit [Visual Studio Code](resource-manager-tools-vs-code.md) und der Vorlagentoolerweiterung erstellen. IntelliSense, Syntaxhervorhebung, Inlinehilfe und viele andere Sprachfunktionen stehen Ihnen zur Verfügung. Zusätzlich zu Visual Studio Code können Sie auch [Visual Studio](./vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) verwenden.

## <a name="template-file"></a>Vorlagendatei

Innerhalb Ihrer Vorlage können Sie [Vorlagenausdrücke](template-expressions.md) schreiben, mit denen die Funktionen von JSON erweitert werden. Diese Ausdrücke verwenden die von Resource Manager bereitgestellten [Funktionen](resource-group-template-functions.md).

Die Vorlage hat die folgenden Abschnitte:

* [Parameter](template-parameters.md): Angeben von Werten während der Bereitstellung, mit denen dieselbe Vorlage für verschiedene Umgebungen verwendet werden kann.

* [Variablen](template-variables.md): Definieren von Werten, die in Ihren Vorlagen wiederverwendet werden. Diese können aus Parameterwerten erstellt werden.

* [Benutzerdefinierte Funktionen](template-user-defined-functions.md): Erstellen angepasster Funktionen zum Vereinfachen Ihrer Vorlage.

* [Ressourcen](resource-group-authoring-templates.md#resources): Angeben der bereitzustellenden Ressourcen.

* [Ausgaben](template-outputs.md): Zurückgeben von Werte aus den bereitgestellten Ressourcen.

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
* Weitere Informationen zum Exportieren von Vorlagen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).
