---
title: Importieren und Exportieren von Blaupausendefinitionen mit PowerShell
description: Erfahren Sie, wie Sie mit Blaupausendefinitionen als Code arbeiten. Verwenden Sie die Export- und Importbefehle für Freigabe, Quellcodeverwaltung und Verwaltung.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/03/2019
ms.topic: conceptual
ms.service: blueprints
ms.openlocfilehash: 30e734c99a87364acfba9a58d83fe9a377958607
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71978439"
---
# <a name="import-and-export-blueprint-definitions-with-powershell"></a>Importieren und Exportieren von Blaupausendefinitionen mit PowerShell

Azure Blueprints kann vollständig über das Azure-Portal verwaltet werden. Mit dem fortschreitenden Einsatz von Azure Blueprints sollten Organisationen damit beginnen, Blaupausendefinitionen als verwalteten Code zu betrachten. Dieses Konzept wird häufig als Infrastructure-as-Code (IaC) bezeichnet. Das Behandeln von Blaupausendefinitionen als Code bietet Vorteile, die über die Möglichkeiten im Azure-Portal hinausgehen. Zu diesen Vorteilen zählen:

- Freigeben von Blaupausendefinitionen
- Sichern von Blaupausendefinitionen
- Wiederverwenden von Blaupausendefinitionen für verschiedene Mandanten oder Abonnements
- Übernehmen von Blaupausendefinitionen in die Quellcodeverwaltung
  - Automatisches Testen von Blaupausendefinitionen in Testumgebungen
  - Unterstützung von Continuous Integration- und Continuous Deployment-Pipelines (CI/CD)

Ungeachtet Ihrer Gründe bietet die Verwaltung von Blaupausendefinitionen als Code stets Vorteile. In diesem Artikel wird gezeigt, wie Sie im Modul [AZ.Blueprint](https://powershellgallery.com/packages/Az.Blueprint/) die Befehle `Import-AzBlueprintWithArtifact` und `Export-AzBlueprintWithArtifact` verwenden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über angemessene Grundkenntnisse von Azure Blueprints verfügen. Arbeiten Sie, falls noch nicht geschehen, folgende Artikel durch:

- [Erstellen einer Blaupause im Portal](../create-blueprint-portal.md)
- Lesen Sie die Informationen zu den [Bereitstellungsstufen](../concepts/deployment-stages.md) und zum [Lebenszyklus von Blaupausen](../concepts/lifecycle.md)
- [Erstellen](../create-blueprint-powershell.md) und [Verwalten](./manage-assignments-ps.md) von Blaupausendefinitionen und Zuweisungen mit PowerShell

Befolgen Sie, falls das Modul **AZ.Blueprint** nicht bereits installiert ist, die Anweisungen unter [Hinzufügen des Moduls „AZ.Blueprint“](./manage-assignments-ps.md#add-the-azblueprint-module), um das Modul aus dem PowerShell-Katalog zu installieren und zu überprüfen.

## <a name="folder-structure-of-a-blueprint-definition"></a>Ordnerstruktur einer Blaupausendefinition

Bevor wir uns mit dem Exportieren und Importieren von Blaupausen befassen, sehen wir uns an, wie die Dateien strukturiert sind, aus denen die Blaupausendefinition besteht. Eine Blaupausendefinition sollte in einem eigenen Ordner gespeichert werden.

> [!IMPORTANT]
> Wird an den Parameter **Name** des Cmdlets `Import-AzBlueprintWithArtifact` kein Wert übergeben, wird der Name des Ordners verwendet, in dem die Blaupausendefinition gespeichert ist.

Neben der Blaupausendefinition, die den Namen `blueprint.json` erhalten muss, gibt es die Artefakte, aus denen sich die Blaupausendefinition zusammensetzt. Jedes Artefakt muss sich im Unterordner namens `artifacts` befinden.
Insgesamt sollte die Struktur Ihrer Blaupausendefinition als JSON-Dateien in Ordnern wie folgt aussehen:

```text
.
|
|- MyBlueprint/  _______________ # Root folder name becomes default name of blueprint definition
|  |- blueprint.json  __________ # The blueprint definition. Fixed name.
|
|  |- artifacts/  ______________ # Subfolder for all blueprint artifacts. Fixed name.
|     |- artifact.json  ________ # Blueprint artifact as JSON file. Artifact named from file.
|     |- ...
|     |- more-artifacts.json

```

## <a name="export-your-blueprint-definition"></a>Exportieren der Blaupausendefinition

Die Schritte zum Exportieren Ihrer Blaupausendefinition sind ganz einfach. Das Exportieren von Blaupausendefinitionen kann nützlich sein, um diese freizugeben, zu sichern oder in die Quellcodeverwaltung zu übernehmen.

- **Blaupause** [erforderlich]
  - Gibt die Blaupausendefinition an.
  - Verwenden Sie `Get-AzBlueprint`, um das Verweisobjekt abzurufen.
- **OutputPath** [erforderlich]
  - Gibt den Pfad an, in dem die JSON-Dateien der Blaupausendefinition gespeichert werden sollen.
  - Die Ausgabedateien befinden sich in einem Unterordner mit dem Namen der Blaupausendefinition.
- **Version** (optional)
  - Gibt die Version an, die ausgegeben werden soll, wenn das Verweisobjekt **Blueprint** Verweise auf mehr als eine Version enthält.

1. Rufen Sie einen Verweis auf die Blaupausendefinition ab, die aus dem als `{subId}` dargestellten Abonnement exportiert werden soll:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   # Get version '1.1' of the blueprint definition in the specified subscription
   $bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'MyBlueprint' -Version '1.1'
   ```

1. Verwenden Sie das Cmdlet `Export-AzBlueprintWithArtifact`, um die angegebene Blaupausendefinition zu exportieren:

   ```azurepowershell-interactive
   Export-AzBlueprintWithArtifact -Blueprint $bpDefinition -OutputPath 'C:\Blueprints'
   ```

## <a name="import-your-blueprint-definition"></a>Importieren der Blaupausendefinition

Wenn Sie entweder [eine exportierte Blaupausendefinition](#export-your-blueprint-definition) oder eine manuell erstellte Blaupausendefinition in der [erforderlichen Ordnerstruktur](#folder-structure-of-a-blueprint-definition) haben, können Sie diese Blaupausendefinition in eine andere Verwaltungsgruppe oder ein anderes Abonnement importieren.

Beispiele für integrierte Blaupausendefinitionen finden Sie im [GitHub-Repository „Azure Blueprints“](https://github.com/Azure/azure-blueprints/tree/master/samples/builtins).

- **Name** [erforderlich]
  - Gibt den Namen für die neue Blaupausendefinition an.
- **InputPath** [erforderlich]
  - Gibt den Pfad an, aus dem die Blaupausendefinition erstellt werden soll.
  - Dieser muss der [erforderlichen Ordnerstruktur](#folder-structure-of-a-blueprint-definition) entsprechen.
- **ManagementGroupId** (optional)
  - Die Verwaltungsgruppen-ID, unter der die Blaupausendefinition gespeichert werden soll, wenn nicht der Standardwert für den aktuellen Kontext verwendet wird.
  - Angegeben werden muss entweder **ManagementGroupId** oder **SubscriptionID**.
- **SubscriptionId** (optional)
  - Die ID des Abonnements, in dem die Blaupausendefinition gespeichert werden soll, wenn nicht der Standardwert für den aktuellen Kontext verwendet wird.
  - Angegeben werden muss entweder **ManagementGroupId** oder **SubscriptionID**.

1. Verwenden Sie das Cmdlet `Import-AzBlueprintWithArtifact`, um die angegebene Blaupausendefinition zu importieren:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   Import-AzBlueprintWithArtifact -Name 'MyBlueprint' -ManagementGroupId 'DevMG' -InputPath 'C:\Blueprints\MyBlueprint'
   ```

Nach dem Import der Blaupausendefinition [weisen Sie sie mit PowerShell zu](./manage-assignments-ps.md#create-blueprint-assignments).

Informationen zum Erstellen erweiterter Blaupausendefinitionen finden Sie in den folgenden Artikeln:

- Verwenden von [statischen und dynamischen Parametern](../concepts/parameters.md)
- Anpassen der [Abfolge von Blaupausen](../concepts/sequencing-order.md)
- Schützen von Bereitstellungen mit der [Blaupausen-Ressourcensperre](../concepts/resource-locking.md)
- [Verwalten von Blaupausen als Code](https://github.com/Azure/azure-blueprints/blob/master/README.md)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../concepts/resource-locking.md) können.
- Beheben Sie Probleme bei der Blaupausenzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).