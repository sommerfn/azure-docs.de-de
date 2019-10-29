---
title: Verschieben von Azure-Ressourcen mit klassischer Bereitstellung
description: Verwenden Sie Azure Resource Manager, um Ressourcen mit klassischer Bereitstellung in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 783fcdca7637f3f67cf146bb827760cb4cdd7cbe
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533480"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Anleitung zum Verschieben von Ressourcen mit dem klassischen Bereitstellungsmodell

Die Schritte zum Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, variieren abhängig davon, ob Sie die Ressourcen innerhalb eines Abonnements oder in ein neues Abonnement verschieben.

## <a name="move-in-the-same-subscription"></a>Verschieben im gleichen Abonnement

Beim Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere innerhalb des gleichen Abonnements gelten die folgenden Einschränkungen:

* Virtuelle Netzwerke (klassisch) können nicht verschoben werden.
* Virtuelle Computer (klassisch) müssen mit dem Clouddienst verschoben werden.
* Der Clouddienst kann nur verschoben werden, wenn der Verschiebevorgang alle dazugehörigen virtuellen Computer umfasst.
* Es kann immer nur ein Clouddienst gleichzeitig verschoben werden.
* Es kann immer nur ein Speicherkonto (klassisch) gleichzeitig verschoben werden.
* Ein Speicherkonto (klassisch) kann nicht im selben Vorgang mit einem virtuellen Computer oder Clouddienst verschoben werden.

Um klassische Ressourcen in eine neue Ressourcengruppe innerhalb des gleichen Abonnements zu verschieben, verwenden Sie die [standardmäßigen Verschiebevorgänge](../resource-group-move-resources.md) im Portal, in Azure PowerShell, in der Azure-Befehlszeilenschnittstelle (CLI) oder in der REST-API. Sie verwenden hierbei die gleichen Vorgänge wie beim Verschieben von Resource Manager-Ressourcen.

## <a name="move-across-subscriptions"></a>Verschieben zwischen Abonnements

Beim Verschieben von Ressourcen in ein neues Abonnement gelten die folgenden Einschränkungen:

* Alle klassische Ressourcen im Abonnement müssen im selben Vorgang verschoben werden.
* Das Zielabonnement darf keine anderen klassischen Ressourcen enthalten.
* Der Verschiebevorgang kann nur über eine separate REST-API für klassische Verschiebevorgänge angefordert werden. Die Resource Manager-Standardbefehle für das Verschieben funktionieren beim Verschieben klassischer Ressourcen in ein neues Abonnement nicht.

Verwenden Sie zum Verschieben von klassischen Ressourcen in ein neues Abonnement die REST-Vorgänge, die für klassische Ressourcen spezifisch sind. Führen Sie zum Verwenden von REST die folgenden Schritte aus:

1. Überprüfen Sie, ob das Quellabonnement an einem abonnementübergreifenden Verschiebevorgang teilnehmen kann. Gehen Sie folgendermaßen vor:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     Fügen Sie Folgendes in den Anforderungstext ein:

   ```json
   {
    "role": "source"
   }
   ```

     Die Antwort für den Überprüfungsvorgang erfolgt in folgendem Format:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Überprüfen Sie, ob das Zielabonnement an einem abonnementübergreifenden Verschiebevorgang teilnehmen kann. Gehen Sie folgendermaßen vor:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     Fügen Sie Folgendes in den Anforderungstext ein:

   ```json
   {
    "role": "target"
   }
   ```

     Die Antwort liegt im gleichen Format vor wie bei der Überprüfung des Quellabonnements.
1. Wenn beide Abonnements die Überprüfung bestehen, verschieben Sie alle klassischen Ressourcen mithilfe des folgenden Vorgangs aus einem Abonnement in ein anderes:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    Fügen Sie Folgendes in den Anforderungstext ein:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

Dieser Vorgang kann einige Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Verschieben klassischer Ressourcen Probleme auftreten, wenden Sie sich an den [Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).
