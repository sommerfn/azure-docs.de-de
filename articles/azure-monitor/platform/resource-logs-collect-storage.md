---
title: Archivieren von Azure-Ressourcenprotokollen in einem Speicherkonto | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure-Ressourcenprotokolle zur langfristigen Aufbewahrung in einem Speicherkonto archivieren.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 8a1802f0f24ba5ccad3ec1c45f3baa29dfe6909f
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263037"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>Archivieren von Azure-Ressourcenprotokollen in einem Speicherkonto
[Ressourcenprotokolle](resource-logs-overview.md) in Azure liefern regelmäßig umfassende Daten zum internen Betrieb einer Azure-Ressource. In diesem Artikel wird beschrieben, wie Ressourcenprotokolle in einem Azure-Speicherkonto erfasst werden, um Daten für die Archivierung beizubehalten.

## <a name="prerequisites"></a>Voraussetzungen
Falls Sie noch nicht über ein Konto verfügen, müssen Sie [ein Azure-Speicherkonto erstellen](../../storage/common/storage-quickstart-create-account.md). Das Speicherkonto muss sich nicht unter demselben Abonnement befinden wie die Ressource, die Protokolle sendet, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements besitzt.

Um den Zugriff auf Überwachungsdaten besser steuern zu können, sollten Sie kein bereits vorhandenes Speicherkonto mit anderen, nicht überwachungsbezogenen Daten verwenden. Wenn Sie auch das [Aktivitätsprotokoll](activity-logs-overview.md) in einem Speicherkonto archivieren, können Sie dasselbe Speicherkonto verwenden, damit sich alle Überwachungsdaten an einem zentralen Ort befinden.

## <a name="create-a-diagnostic-setting"></a>Erstellen einer Diagnoseeinstellung
Ressourcenprotokolle werden standardmäßig nicht erfasst. Erfassen Sie diese in einem Azure-Speicherkonto und anderen Zielen, indem Sie eine Diagnoseeinstellung für eine Azure-Ressource erstellen. Weitere Informationen finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).


## <a name="data-retention"></a>Beibehaltung von Daten
Die Aufbewahrungsrichtlinie definiert die Anzahl von Tagen für die Aufbewahrung von Daten aus jeder Protokollkategorie und der Metrikdaten, die in einem Speicherkonto gespeichert werden. Für eine Aufbewahrungsrichtlinie kann eine beliebige Anzahl von Tagen zwischen 0 und 365 festgelegt werden. Eine auf Null festgelegte Aufbewahrungsrichtlinie gibt an, dass Ereignisse für diese Protokollkategorie unbegrenzt gespeichert werden.

Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht. Der Löschvorgang beginnt um Mitternacht (UTC), jedoch kann es bis zu 24 Stunden dauern, bis die Protokolle aus Ihrem Speicherkonto gelöscht werden. 


## <a name="schema-of-resource-logs-in-storage-account"></a>Schema der Ressourcenprotokolle im Speicherkonto

Nach dem Einrichten der Diagnoseeinstellung wird im Speicherkonto ein Speichercontainer erstellt, sobald ein Ereignis in einer der aktivierten Protokollkategorien auftritt. Die Blobs im Container verwenden die folgende Benennungskonvention:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Beispielsweise könnte das Blob für eine Netzwerksicherheitsgruppe einen Namen aufweisen, der dem folgenden Beispiel ähnelt:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Jedes Blob vom Typ „PT1H.json“ enthält ein JSON-Blob mit Ereignissen, die innerhalb der in der Blob-URL angegebenen Stunde (Beispiel: h=12) aufgetreten sind. Während der aktuellen Stunde werden Ereignisse an die Datei „PT1H.json“ angefügt, sobald sie auftreten. Der Minutenwert (m=00) ist immer „00“, da Ressourcenprotokollereignisse stundenweise in einzelne Blobs unterteilt werden.

Innerhalb der Datei „PT1H.json“ wird jedes Ereignis im folgenden Format gespeichert. Hierbei wird ein gemeinsames Schema der obersten Ebene verwendet. Es ist aber für jeden Azure-Dienst eindeutig, wie unter [Ressourcenprotokollschema](resource-logs-overview.md#resource-logs-schema) beschrieben.

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Plattformprotokolle werden mit [JSON Lines](http://jsonlines.org/) in den Blobspeicher geschrieben. Dies bedeutet, dass jedes Ereignis in einer Zeile enthalten ist und mit dem Zeilenumbruchzeichen ein neues Ereignis angegeben wird. Dieses Format wurde im November 2018 implementiert. Vor diesem Datum wurden Protokolle in den Blobspeicher als JSON-Array von Datensätzen geschrieben, wie unter [Vorbereiten auf die Formatänderung für Azure Monitor-Plattformprotokolle, die in einem Speicherkonto archiviert werden](resource-logs-blob-format.md) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* [Herunterladen von Blobs für die Analyse](../../storage/blobs/storage-quickstart-blobs-dotnet.md).
* [Archivieren von Azure Active Directory-Protokollen mit Azure Monitor](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md).
* [Weitere Informationen zu Ressourcenprotokollen](../../azure-monitor/platform/resource-logs-overview.md).

