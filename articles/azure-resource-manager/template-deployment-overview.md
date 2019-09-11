---
title: Azure-Ressourcen-Manager-Vorlagen
description: Es wird beschrieben, wie Sie Azure Resource Manager-Vorlagen für die Bereitstellung von Ressourcen verwenden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: tomfitz
ms.openlocfilehash: 95c127b3a7c9c47c96b292066bf1597a02896806
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70166435"
---
# <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Mit Azure Resource Manager können Sie Vorlagen erstellen, mit denen definiert wird, was Sie in Azure bereitstellen möchten. Die Vorlage ist eine JSON-Datei (JavaScript Object Notation), die die Infrastruktur und Konfiguration Ihrer Azure-Lösung enthält. Mit einer Vorlage können Sie die Lösung während ihres Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

Für die Vorlage wird deklarative Syntax verwendet. Hiermit können Sie angeben, was Sie bereitstellen möchten, ohne dass Sie die Folge der Programmierbefehle für die Erstellung schreiben müssen. In der Vorlage geben Sie die bereitzustellenden Ressourcen und die Eigenschaften für diese Ressourcen an.

## <a name="benefits-of-resource-manager-templates"></a>Vorteile von Resource Manager-Vorlagen

Resource Manager-Vorlagen haben mehrere Vorteile:

* Sie können alle Ressourcen für Ihre Lösung als Gruppe bereitstellen, verwalten und überwachen, anstatt diese Ressourcen einzeln zu verarbeiten.

* Sie können die Lösung während des gesamten Entwicklungslebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

* Sie können zum Verwalten Ihrer Infrastruktur anstelle von Skripts auch deklarative Vorlagen verwenden.

* Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.

Die folgenden Vorschläge helfen Ihnen, bei der Arbeit mit Lösungen die Vorteile von Resource Manager zu nutzen.

* Definieren Sie die Infrastruktur mithilfe der deklarativen Syntax in Ressourcen-Manager-Vorlagen anstatt mit imperativen Befehlen, und stellen Sie sie so bereit.

* Definieren Sie alle Bereitstellungs- und Konfigurationsschritte in der Vorlage. Sie sollten keine manuellen Schritte zum Einrichten der Lösung durchführen müssen.

* Führen Sie imperative Befehle aus, um Ihre Ressourcen zu verwalten, z. B. Starten oder Beenden einer App oder des Computers.

* Halten Sie sich an die Hinweise unter [Bewährte Methoden für Azure Resource Manager-Vorlagen](template-best-practices.md).

## <a name="template-deployment-process"></a>Prozess für die Vorlagenbereitstellung

Wenn Sie eine Vorlage bereitstellen, analysiert Resource Manager die Vorlage und konvertiert die darin enthaltene Syntax in REST-API-Vorgänge. Diese Vorgänge werden an die entsprechenden Ressourcenanbieter gesendet. Beispielsweise kann der Resource Manager eine Vorlage mit der folgenden Ressourcendefinition empfangen:

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

## <a name="template-structure"></a>Vorlagenstruktur

Sie können völlig frei entscheiden, wie Sie Vorlagen und Ressourcengruppen definieren und Ihre Lösung verwalten möchten. Beispielsweise können Sie Ihre Anwendung mit drei Ebenen über eine einzelne Vorlage für eine einzelne Ressourcengruppe bereitstellen.

![Vorlage mit drei Ebenen](./media/resource-group-overview/3-tier-template.png)

Sie müssen aber nicht die gesamte Infrastruktur in einer einzelnen Vorlage definieren. Oftmals ist es sinnvoll, die Bereitstellungsanforderungen in eine Gruppe von spezifischen, zweckgebundenen Vorlagen zu unterteilen. Sie können diese Vorlagen mühelos für verschiedene Lösungen erneut verwenden. Um eine bestimmte Lösung bereitzustellen, erstellen Sie eine Mastervorlage, die alle erforderlichen Vorlagen verknüpft. Die folgende Abbildung zeigt, wie Sie eine Lösung mit drei Ebenen über eine übergeordnete Vorlage bereitstellen, die drei geschachtelte Vorlagen enthält.

![Vorlage mit geschachtelten Ebenen](./media/resource-group-overview/nested-tiers-template.png)

Wenn Ihre Ebenen über separate Lebenszyklen verfügen sollen, können Sie die drei Ebenen in separaten Ressourcengruppen bereitstellen. Beachten Sie, dass die Ressourcen weiterhin mit Ressourcen in anderen Ressourcengruppen verknüpft werden können.

![Ebenenvorlage](./media/resource-group-overview/tier-templates.png)

Informationen zu geschachtelten Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

Azure Resource Manager analysiert Abhängigkeiten, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge erstellt werden. Wenn eine Ressource von einem Wert einer anderen Ressource abhängig ist (z.B. ein virtueller Computer, der ein Speicherkonto für Datenträger benötigt), legen Sie eine Abhängigkeit fest. Weitere Informationen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md).

Sie können die Vorlage auch für Aktualisierungen der Infrastruktur verwenden. Beispielsweise können Sie Ihrer Lösung eine Ressource sowie Konfigurationsregeln für die Ressourcen hinzufügen, die bereits bereitgestellt wurden. Wenn die Vorlage eine bereits vorhandene Ressource definiert, erstellt Resource Manager keine neue Ressource, sondern aktualisiert die vorhandene Ressource.

Der Resource Manager bietet Erweiterungen für Szenarien, in denen zusätzliche Vorgänge erforderlich sind – beispielsweise das Installieren bestimmter Software, die nicht Teil des Setups ist. Wenn Sie bereits einen Konfigurationsverwaltungsdienst verwenden (etwa DSC, Chef oder Puppet), können Sie ihn mithilfe von Erweiterungen auch weiter nutzen. Informationen zu Erweiterungen für virtuelle Computer finden Sie unter [Informationen zu Erweiterungen und Features für virtuelle Computer](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Wenn Sie eine Lösung über das Portal erstellen, enthält sie automatisch eine Bereitstellungsvorlage. Sie müssen die Vorlage nicht völlig neu erstellen, da Sie mit der Vorlage für Ihre Lösung beginnen und diese dann an die speziellen Anforderungen anpassen können. Ein Beispiel finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). Sie können auch eine Vorlage für eine vorhandene Ressourcengruppe abrufen, indem Sie entweder den aktuellen Zustand der Ressourcengruppe exportieren oder die Vorlage anzeigen, die für eine bestimmte Bereitstellung verwendet wurde. Das Anzeigen der [exportierten Vorlage](./manage-resource-groups-portal.md#export-resource-groups-to-templates) ist hilfreich, um sich über die Vorlagensyntax zu informieren.

Die Vorlage wird schließlich zu einem Teil des Quellcodes für Ihre App. Sie können sie in das Quellcoderepository einchecken und im Verlauf der App-Entwicklung aktualisieren. Sie können die Vorlage mit Visual Studio bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Vorlagendateien finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

Nach dem Definieren der Vorlage können Sie die Ressourcen in Azure bereitstellen. Informationen zur Bereitstellung finden Sie hier:

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](resource-group-template-deploy-cli.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](resource-group-template-deploy-portal.md)
* [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](resource-group-template-deploy-rest.md)

