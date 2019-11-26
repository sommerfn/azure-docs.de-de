---
title: 'Tutorial: Überwachen von Key Vault mit Azure Event Grid'
description: 'Tutorial: Verwenden von Azure Event Grid zum Abonnieren von Key Vault-Ereignissen'
services: media-services
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 5771af365b763d2152eea4ef4f662e08769b378c
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133351"
---
# <a name="tutorial-monitoring-key-vault-with-azure-event-grid-preview"></a>Tutorial: Überwachen von Key Vault mit Azure Event Grid (Vorschau)

Die Key Vault-Integration in Event Grid befindet sich derzeit in der Vorschauphase. Sie ermöglicht es Benutzern, benachrichtigt zu werden, wenn sich der Status eines geheimen Schlüssels, der im Schlüsseltresor (Key Vault) gespeichert ist, geändert hat. Eine Statusänderung ist als Geheimnis, das demnächst abläuft (innerhalb von 30 Tagen bis Ablauf), als Geheimnis, das abgelaufen ist, oder als Geheimnis definiert ist, für das eine neue Version verfügbar ist. Es werden Benachrichtigungen für alle drei Geheimnistypen (Schlüssel, Zertifikat und Geheimnis) unterstützt.

Anwendungen können auf diese Ereignisse mit modernen serverlosen Architekturen reagieren, ohne dass komplizierter Code oder teure und ineffiziente Abrufdienste erforderlich sind. Ereignisse werden per Push über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Ereignishandler, etwa [Azure Functions](https://azure.microsoft.com/services/functions/) und [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), oder sogar an Ihren eigenen Webhook übertragen, und Sie zahlen nur für das, was Sie tatsächlich nutzen. Informationen zu den Preisen finden Sie unter [Event Grid – Preise](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="key-vault-events-and-schemas"></a>Key Vault- Ereignisse und -Schemas

Event Grid verwendet [Ereignisabonnements](../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten. Key Vault-Ereignisse enthalten alle Informationen, die Sie benötigen, um auf Änderungen in Ihren Daten zu reagieren. Sie erkennen ein Key Vault-Ereignis daran, dass die eventType-Eigenschaft mit „Microsoft.KeyVault“ beginnt.

Weitere Informationen finden Sie unter [Azure Event Grid-Ereignisschema für Azure Key Vault](../event-grid/event-schema-key-vault.md).

> [!NOTE]
> Ereignisse werden nur für Geheimnisversionen (alle drei Typen) ausgelöst, die nach dem Festlegen des Abonnements erstellt wurden.
>
> Für vorhandene Geheimnisse müssen Sie neue Versionen generieren.

## <a name="practices-for-consuming-events"></a>Methoden zum Nutzen von Ereignissen

Anwendungen, die Key Vault-Ereignisse verarbeiten, sollten einige bewährte Methoden nutzen:

* Es können mehrere Abonnements konfiguriert werden, um Ereignisse an den gleichen Ereignishandler zu routen. Sie sollten keinesfalls davon ausgehen, dass Ereignisse aus einer bestimmten Quelle stammen, sondern Sie sollten das Thema der Nachricht überprüfen, um sich zu vergewissern, dass das jeweilige Ereignis aus dem Schlüsseltresor stammt, den Sie erwartet haben.
* Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
* Ignorieren Sie Felder, die Sie nicht verstehen.  So müssen Sie sich nicht mit neuen Features auseinandersetzen, die in der Zukunft hinzugefügt werden könnten.
* Verwenden Sie Präfix- und Suffixübereinstimmungen für „subject“, um Ereignisse auf ein bestimmtes Ereignis zu beschränken.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)
- [Was ist Azure Event Grid?](../event-grid/overview.md)
- Gewusst wie: [Empfangen von und Reagieren auf Key Vault-Benachrichtigungen mit Azure Event Grid (Vorschau)](event-grid-tutorial.md)
- Gewusst wie: [Verwenden von Logic Apps zum Empfangen einer E-Mail bei Statusänderungen von Key Vault-Geheimnissen](event-grid-logicapps.md)
- [Azure Event Grid-Ereignisschema für Azure Key Vault (Vorschau)](../event-grid/event-schema-key-vault.md)
- [Azure Automation – Übersicht](../automation/index.yml)
