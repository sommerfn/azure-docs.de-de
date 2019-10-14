---
title: Details zur Richtlinienzuweisungsstruktur
description: Beschreibt die Definition der Richtlinienzuweisung, die von Azure Policy verwendet wird, um Richtliniendefinitionen und -parameter zur Bewertung mit Ressourcen in Beziehung zu setzen.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 5326e765701a42323ea62df8d35128c4117b2ed9
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981414"
---
# <a name="azure-policy-assignment-structure"></a>Azure Policy-Zuweisungsstruktur

Richtlinienzuweisungen werden von Azure Policy zur Definition verwendet, welche Ressourcen bei Richtlinien oder Initiativen zugewiesen werden. Die Richtlinienzuweisung kann die Werte von Parametern für diese Gruppe von Ressourcen zum Zeitpunkt der Zuweisung bestimmen, wodurch es möglich wird, Richtliniendefinitionen wiederzuverwenden, die dieselben Ressourceneigenschaften mit unterschiedlichen Anforderungen an die Compliance ansprechen.

Eine Richtlinienzuweisung wird mithilfe von JSON erstellt. Die Richtlinienzuweisung enthält Elemente für Folgendes:

- Anzeigename
- description
- metadata
- Erzwingungsmodus
- Richtliniendefinition
- parameters

Der folgende JSON-Code zeigt z. B. eine Richtlinienzuweisung im _DoNotEnforce_-Modus mit dynamischen Parametern an:

```json
{
    "properties": {
        "displayName": "Enforce resource naming rules",
        "description": "Force resource names to begin with DeptA and end with -LC",
        "metadata": {
            "assignedBy": "Cloud Center of Excellence"
        },
        "enforcementMode": "DoNotEnforce",
        "policyDefinitionId": "/subscriptions/{mySubscriptionID}/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming",
        "parameters": {
            "prefix": {
                "value": "DeptA"
            },
            "suffix": {
                "value": "-LC"
            }
        }
    }
}
```

Alle Azure Policy-Beispiele sind unter [Azure Policy-Beispiele](../samples/index.md) verfügbar.

## <a name="display-name-and-description"></a>Anzeigename und Beschreibung

Sie verwenden **displayName** und **description**, um die Richtlinienzuweisung zu identifizieren und den Kontext für ihre Verwendung mit einem bestimmten Satz von Ressourcen bereitzustellen. **displayName** hat eine maximale Länge von _128_ Zeichen, und **description** hat eine maximale Länge von _512_ Zeichen.

## <a name="enforcement-mode"></a>Erzwingungsmodus

Mit der **enforcementMode**-Eigenschaft können Kunden das Ergebnis einer Richtlinie für vorhandene Ressourcen testen, ohne dass die Richtlinienauswirkung initiiert oder Einträge im [Azure-Aktivitätsprotokoll](../../../azure-monitor/platform/activity-logs-overview.md) ausgelöst werden.
Dieses Szenario wird allgemein als „Was-wäre-wenn“ bezeichnet und richtet sich nach sicheren Bereitstellungsverfahren.

Diese Eigenschaft weist die folgenden Werte auf:

|Mode |JSON-Wert |type |Manuelle Behebung |Aktivitätsprotokolleintrag |BESCHREIBUNG |
|-|-|-|-|-|-|
|Enabled |Standard |Zeichenfolge |Ja |Ja |Die Richtlinienauswirkung wird während der Erstellung oder Aktualisierung von Ressourcen erzwungen. |
|Deaktiviert |DoNotEnforce |Zeichenfolge |Ja |Nein | Die Richtlinienauswirkung wird nicht während der Erstellung oder Aktualisierung von Ressourcen erzwungen. |

Wenn **enforcementMode** nicht in einer Richtlinie oder Initiativendefinition angegeben ist, wird der Wert _Default_ verwendet. [Aufgaben zur Bereinigung](../how-to/remediate-resources.md) können für [deployIfNotExists](./effects.md#deployifnotexists)-Richtlinien gestartet werden, auch wenn **enforcementMode** auf _DoNotEnforce_ festgelegt ist.

## <a name="policy-definition-id"></a>Richtliniendefinitions-ID

Dieses Feld muss der vollständige Pfadname einer Richtlinien- oder einer Initiativendefinition sein.
`policyDefinitionId` ist eine Zeichenfolge und kein Array. Es wird empfohlen, stattdessen eine [Initiative](./definition-structure.md#initiatives) zu verwenden, wenn häufig mehrere Richtlinien gleichzeitig zugewiesen werden.

## <a name="parameters"></a>Parameter

Dieses Segment der Richtlinienzuweisung stellt die Werte für die Parameter bereit, die in der [Richtliniendefinition oder Initiativendefinition](./definition-structure.md#parameters) definiert sind.
Dieses Design ermöglicht es, eine Richtlinien- oder Initiativendefinition mit unterschiedlichen Ressourcen wiederzuverwenden, aber auf unterschiedliche Geschäftswerte oder -ergebnisse zu prüfen.

```json
"parameters": {
    "prefix": {
        "value": "DeptA"
    },
    "suffix": {
        "value": "-LC"
    }
}
```

In diesem Beispiel sind die Parameter, die zuvor in der Richtliniendefinition definiert wurden, `prefix` und `suffix`. Diese spezielle Richtlinienzuweisung legt `prefix` auf **DeptA** und `suffix` auf **-LC** fest. Dieselbe Richtliniendefinition kann mit einem anderen Satz von Parametern für eine andere Abteilung wiederverwendet werden, wodurch die Duplizierung und Komplexität von Richtliniendefinitionen reduziert und gleichzeitig Flexibilität bereitgestellt wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Struktur von Richtliniendefinitionen](./definition-structure.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).