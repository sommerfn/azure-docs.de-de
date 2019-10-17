---
title: Problembehandlung für häufige Fehler
description: Informationen zum Beheben von Problemen beim Abfragen von Azure-Ressourcen mit Azure Resource Graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/18/2019
ms.topic: troubleshooting
ms.service: resource-graph
ms.openlocfilehash: 030fe26a0aa8fc4ed855fb7744e576366f4fd2e2
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389702"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>Problembehandlung mit Azure Resource Graph

Beim Abfragen von Azure-Ressourcen mit Azure Resource Graph können Fehler auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="finding-error-details"></a>Ermitteln von Fehlerdetails

Die meisten Fehler werden durch Probleme beim Ausführen einer Abfrage mit Azure Resource Graph ausgelöst. Wenn eine Abfrage fehlschlägt, stellt das SDK Details zu dem Problem bereit. Diese Informationen enthalten Hinweise zum Problem, damit Sie es beheben können und die nächste Abfrage erfolgreich verläuft.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="toomanysubscription"></a>Szenario: Zu viele Abonnements

#### <a name="issue"></a>Problem

Kunden mit Zugriff auf mehr als 1000 Abonnements, einschließlich mandantenübergreifender Abonnements über [Azure Lighthouse](../../../lighthouse/overview.md), können nicht über einen einzigen Aufruf von Azure Resource Graph Daten zu allen Abonnements abrufen.

#### <a name="cause"></a>Ursache

Die Azure CLI und PowerShell leiten nur die ersten 1000 Abonnements an Azure Resource Graph weiter. Die Rest-API für Azure Resource Graph akzeptiert eine maximale Anzahl von Abonnements, für die die Abfrage durchgeführt werden soll.

#### <a name="resolution"></a>Lösung

Verwenden Sie Batchanforderungen für die Abfrage, die nur aus einem Teil der Abonnements bestehen, um unter der Grenze von 1000 Abonnements zu bleiben. Verwenden Sie den Parameter **Subscription** (Abonnement) in PowerShell, um das Problem zu lösen.

```azurepowershell-interactive
# Replace this query with your own
$query = 'Resources | project type'

# Fetch the full array of subscription IDs
$subscriptions = Get-AzSubscription
$subscriptionIds = $subscriptions.Id

# Create a counter, set the batch size, and prepare a variable for the results
$counter = [PSCustomObject] @{ Value = 0 }
$batchSize = 1000
$response = @()

# Group the subscriptions into batches
$subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

# Run the query for each batch
foreach ($batch in $subscriptionsBatch){ $response += Search-AzGraph -Query $query -Subscription $batch.Group }

# View the completed results of the query on all subscriptions
$response
```

### <a name="rest-contenttype"></a>Szenario: Nicht unterstützter REST-Header „Content-Type“

#### <a name="issue"></a>Problem

Kunden, die die Azure Resource Graph-REST-API abfragen, erhalten die Antwort _500_ (Interner Serverfehler).

#### <a name="cause"></a>Ursache

Die Azure Resource Graph-REST-API unterstützt nur **application/json** als `Content-Type`. Einige REST-Tools oder -Agents legen standardmäßig **text/plain** fest. Dies wird in der REST-API nicht unterstützt.

#### <a name="resolution"></a>Lösung

Überprüfen Sie, ob in dem Tool oder Agent, das oder den Sie zum Abfragen von Azure Resource Graph verwenden, der REST-API-Header `Content-Type` für **application/json** konfiguriert ist.

### <a name="rest-403"></a>Szenario: Keine Leseberechtigung für alle Abonnements in der Liste

#### <a name="issue"></a>Problem

Kunden, die mit einer Azure Resource Graph-Abfrage explizit eine Liste mit Abonnements übergeben, erhalten die Antwort _403_ (Verboten).

#### <a name="cause"></a>Ursache

Wenn der Kunde nicht über die Leseberechtigung für alle angegebenen Abonnements verfügt, wird die Anforderung aufgrund fehlender entsprechender Sicherheitsrechte abgelehnt.

#### <a name="resolution"></a>Lösung

Fügen Sie mindestens ein Abonnement in die Abonnementliste ein, für das der Kunde, der die Abfrage ausführt, mindestens über Lesezugriff verfügt. Weitere Informationen finden Sie unter [Berechtigungen in Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
- Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
- Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.