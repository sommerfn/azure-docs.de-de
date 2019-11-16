---
title: Ausführen Ihrer ersten Abfrage mit der Azure CLI
description: Dieser Artikel führt Sie durch die Schritte zum Aktivieren der Resource Graph-Erweiterung für die Azure-Befehlszeilenschnittstelle und Ausführen Ihrer ersten Abfrage.
ms.date: 10/18/2019
ms.topic: quickstart
ms.openlocfilehash: b74e4314a7755bb067831f955ca02d62485abe4f
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73958967"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-cli"></a>Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe der Azure CLI

Der erste Schritt der Verwendung von Azure Resource Graph ist, zu überprüfen, ob die Erweiterung für die [Azure-Befehlszeilenschnittstelle](/cli/azure/) installiert ist. Dieser Schnellstart führt Sie durch das Hinzufügen der Erweiterung zu Ihrer Azure-Befehlszeilenschnittstelle. Sie können die Erweiterung mit lokal installierter Azure CLI oder über die [Azure Cloud Shell](https://shell.azure.com) verwenden.

Am Ende dieses Prozesses haben Sie die Erweiterung der Installation der Azure-Befehlszeilenschnittstelle Ihrer Wahl hinzugefügt und Ihre erste Resource Graph-Abfrage durchgeführt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="add-the-resource-graph-extension"></a>Hinzufügen der Resource Graph-Erweiterung

Um die Azure-Befehlszeilenschnittstelle für die Abfrage von Azure Resource Graph zu aktivieren, muss die Erweiterung hinzugefügt werden. Diese Erweiterung funktioniert überall, wo die Azure-Befehlszeilenschnittstelle verwendet werden kann, einschließlich [bash unter Windows 10](/windows/wsl/install-win10), [Cloud Shell](https://shell.azure.com) (sowohl eigenständig als auch innerhalb des Portals), [Azure CLI-Docker-Image](https://hub.docker.com/r/microsoft/azure-cli/) oder lokaler Installation.

1. Vergewissern Sie sich, dass die neueste Azure CLI (mindestens **2.0.45**) installiert ist. Falls es noch nicht installiert ist, befolgen Sie [diese Anweisungen](/cli/azure/install-azure-cli-windows?view=azure-cli-latest).

1. Importieren Sie sie mit folgendem Befehl in die Azure CLI-Umgebung Ihrer Wahl:

   ```azurecli-interactive
   # Add the Resource Graph extension to the Azure CLI environment
   az extension add --name resource-graph
   ```

1. Überprüfen Sie, ob die Erweiterung installiert wurde und in der erwarteten Version vorliegt (mindestens **1.0.0**):

   ```azurecli-interactive
   # Check the extension list (note that you may have other extensions installed)
   az extension list

   # Run help for graph query options
   az graph query -h
   ```

## <a name="run-your-first-resource-graph-query"></a>Ausführen Ihrer ersten Resource Graph-Abfrage

Nachdem die Azure CLI-Erweiterung der gewünschten Umgebung hinzugefügt wurde, können Sie eine einfache Resource Graph-Abfrage ausprobieren. Die Abfrage gibt die ersten fünf Azure-Ressourcen mit **Namen** und **Ressourcentyp** der einzelnen Ressourcen zurück.

1. Führen Sie Ihre erste Azure Resource Graph-Abfrage mit der `graph`-Erweiterung und dem `query`-Befehl aus:

   ```azurecli-interactive
   # Login first with az login if not using Cloud Shell

   # Run Azure Resource Graph query
   az graph query -q 'Resources | project name, type | limit 5'
   ```

   > [!NOTE]
   > Da dieses Abfragebeispiel keinen Sortierungsmodifizierer wie z.B. `order by` umfasst, ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen.

1. Aktualisieren Sie die Abfrage auf `order by` der Eigenschaft **Name**:

   ```azurecli-interactive
   # Run Azure Resource Graph query with 'order by'
   az graph query -q 'Resources | project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > Genau wie bei der ersten Abfrage ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen. Die Reihenfolge der Abfragebefehle ist wichtig. In diesem Beispiel kommt `order by` nach `limit`. Dies beschränkt zunächst die Ergebnisse der Abfrage und sortiert sie dann.

1. Aktualisieren Sie die Abfrage, sodass zuerst eine Sortierung (`order by`) nach der Eigenschaft **Name** vorgenommen wird, und begrenzen Sie die Ergebnisse mithilfe von `limit` auf die fünf relevantesten Ergebnisse:

   ```azurecli-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   az graph query -q 'Resources | project name, type | order by name asc | limit 5'
   ```

Wenn die letzte Abfrage mehrmals ausgeführt wird, sind die zurückgegebenen Ergebnisse – vorausgesetzt, dass sich in Ihrer Umgebung nichts ändert – konsistent und wie erwartet: sortiert nach der Eigenschaft **Name**, aber immer noch auf die fünf relevantesten Ergebnisse begrenzt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Resource Graph-Erweiterung aus der Azure CLI-Umgebung entfernen möchten, verwenden Sie hierfür folgenden Befehl:

```azurecli-interactive
# Remove the Resource Graph extension from the Azure CLI environment
az extension remove -n resource-graph
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abfragesprache](./concepts/query-language.md).
- Erfahren Sie mehr über das [Erkunden von Ressourcen](./concepts/explore-resources.md).
- Führen Sie Ihre erste Abfrage über das [Azure-Portal](first-query-portal.md) aus.
- Führen Sie Ihre erste Abfrage mit [Azure PowerShell](first-query-powershell.md) aus.
- Sehen Sie sich Beispiele für [einfache Abfragen](./samples/starter.md) an.
- Sehen Sie sich Beispiele für [erweiterte Abfragen](./samples/advanced.md) an.
- Geben Sie über [UserVoice](https://feedback.azure.com/forums/915958-azure-governance) Feedback ab.