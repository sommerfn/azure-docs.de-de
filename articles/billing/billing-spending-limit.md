---
title: Azure-Ausgabenlimit | Microsoft-Dokumentation
description: In diesem Artikel werden die Funktionsweise und das Entfernen des Azure-Ausgabenlimits beschrieben.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: banders
ms.openlocfilehash: 1324b60de05805cd409975358ff78e5b2b27eef1
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114451"
---
# <a name="azure-spending-limit"></a>Azure-Ausgabenlimit

Mit dem Ausgabenlimit in Azure wird verhindert, dass Sie Ihr Guthaben überschreiten. Für alle neuen Kunden, die sich für eine Azure-Testversion oder -Angebote registrieren, die Guthaben für mehrere Monate vorsehen, ist das Ausgabenlimit standardmäßig aktiviert. Das Ausgabenlimit beträgt 0 USD und kann nicht geändert werden. Beispielsweise kann das Ausgabenlimit nicht in 100 USD geändert werden. Sie können das Ausgabenlimit jedoch entfernen. Sie haben also entweder keine Beschränkung, oder Sie haben ein Limit von null, das Sie an den meisten Ausgaben hindert. Für Abonnements in Plänen mit Prepaidtarifen oder nutzungsbasierter Bezahlung ist das Ausgabenlimit nicht verfügbar. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Erreichen eines Ausgabenlimits

Wenn durch Ihre Nutzung Gebühren anfallen, die das monatliche Guthaben Ihres Azure-Abonnements überschreiten, werden die von Ihnen bereitgestellten Dienste für die verbleibende Laufzeit des jeweiligen Abrechnungszeitraums deaktiviert.

Wenn Sie beispielsweise das gesamte in Ihrem Abonnement enthaltene Guthaben verbrauchen, werden die von Ihnen bereitgestellten Azure-Ressourcen aus der Produktion entfernt sowie Ihre virtuellen Azure-Computer angehalten und die entsprechende Zuordnung aufgehoben. Die Daten in Ihren Speicherkonten sind in diesem Fall nur schreibgeschützt verfügbar.

Wenn Ihr Abonnementangebot Guthaben für mehrere Monate umfasst, wird Ihr Abonnement zu Beginn des nächsten Abrechnungszeitraums automatisch reaktiviert. Sie können dann Ihre Azure-Ressourcen erneut bereitstellen und haben Vollzugriff auf Ihre Speicherkonten und Datenbanken.

Wenn Sie das Ausgabenlimit Ihres Abonnements erreichen, sendet Azure entsprechende E-Mail-Benachrichtigungen. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an, um Benachrichtigungen zu Abonnements einzusehen, deren Ausgabenlimit erreicht wurde.

Wenn Sie über eine kostenlose Testversion verfügen und das Ausgabenlimit erreichen, können Sie auf einen Plan mit [nutzungsbasierter Bezahlung](billing-upgrade-azure-subscription.md) umsteigen, damit das Ausgabenlimit entfernt und das Abonnement automatisch aktiviert wird.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Entfernen des Ausgabenlimits im Kontocenter

Wenn für Ihr Abonnement eine gültige Zahlungsmethode festgelegt wurde, können Sie das Ausgabenlimit jederzeit entfernen. Bei Angeboten mit Guthaben für mehrere Monate können Sie das Ausgabenlimit darüber hinaus zu Beginn des nächsten Abrechnungszeitraums aktivieren.

Führen Sie die folgenden Schritte aus, um das Ausgabenlimit zu entfernen:

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an.
1. Wählen Sie ein Abonnement aus. Wenn das Abonnement deaktiviert ist, weil das Ausgabenlimit erreicht wurde, klicken Sie auf die Benachrichtigung: **Das Abonnement wurde deaktiviert, um zu verhindern, dass Gebühren anfallen.** Klicken Sie andernfalls im Bereich **ABONNEMENTSTATUS** auf **Ausgabenlimit entfernen**.
1. Wählen Sie die gewünschte Option aus.

![Auswählen einer Option zum Entfernen des Ausgabenlimits](./media/billing-spending-limit/remove-spending-limit.PNG)

| Option | Wirkung |
| --- | --- |
| Ausgabenlimit auf unbestimmte Zeit deaktivieren | Entfernt das Ausgabenlimit, ohne es automatisch am Anfang des nächsten Abrechnungszeitraums wieder zu aktivieren. |
| Ausgabenlimit für den aktuellen Abrechnungszeitraum deaktivieren | Entfernt das Ausgabenlimit, sodass es automatisch am Anfang des nächsten Abrechnungszeitraums wieder aktiviert wird. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Warum sollten Sie das Ausgabenlimit entfernen?

Das Ausgabenlimit könnte verhindern, dass Sie bestimmte Drittanbieterdienste und Microsoft-Dienste bereitstellen und verwenden können. Im Folgenden sind Situationen aufgeführt, in denen Sie das Ausgabenlimit für Ihr Abonnement entfernen sollten.

-  Sie planen, Images von Erstanbietern wie Oracle und Dienste wie Azure DevOps Services bereitzustellen. In dieser Situation überschreiten Sie praktisch umgehend das Ausgabenlimit, sodass Ihr Abonnement deaktiviert wird.
- Sie haben Dienste, die nicht unterbrochen werden sollen.
- Sie verfügen über Dienste und Ressourcen mit Einstellungen wie virtuellen IP-Adressen, die nicht verloren gehen sollen. Wenn die Zuordnung der Dienste und Ressourcen aufgehoben wird, gehen diese Einstellungen verloren.

## <a name="turn-on-the-spending-limit-after-removing"></a>Aktivieren des Ausgabenlimits nach dem Entfernen

Dieses Feature ist nur verfügbar, wenn das Ausgabenlimit auf unbestimmte Zeit entfernt wurde. Aktivieren Sie es, damit es zu Beginn des nächsten Abrechnungszeitraums automatisch gestartet wird.

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an.
1. Klicken Sie auf das gelbe Banner, um die Option „Ausgabenlimit“ zu ändern.
1. Wählen Sie **Ausgabenlimit im nächsten Abrechnungszeitraum aktivieren \<Startdatum des Abrechnungszeitraum\>** .

## <a name="custom-spending-limit"></a>Benutzerdefiniertes Ausgabenlimit

Ausgabenlimits von Kunden sind nicht verfügbar.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Ein Ausgabenlimit verhindert nicht alle Gebühren

[Einige im Azure Marketplace veröffentlichte externe Dienste](billing-understand-your-azure-marketplace-charges.md) können nicht mit Ihrem Abonnementguthaben verwendet werden und separate Gebühren verursachen, selbst wenn ein Ausgabenlimit festgelegt ist. Beispiele sind Visual Studio-Lizenzen, Azure Active Directory Premium, Supportpläne und die meisten Drittanbieterdienste. Wenn Sie einen neuen externen Dienst bereitstellen, wird eine Warnung angezeigt, um darauf hinzuweisen, dass die Dienste separat in Rechnung gestellt werden:

![Warnung beim Erwerb von Marketplace-Angeboten](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- Steigen Sie auf einen Plan mit [nutzungsbasierter Bezahlung](billing-upgrade-azure-subscription.md) um.
