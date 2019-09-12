---
title: Überwachen und Nachverfolgen der Nutzung von kostenlosen Azure-Diensten
description: Es wird beschrieben, wie Sie die Nutzung von kostenlosen Diensten im Azure-Portal und in der CSV-Nutzungsdatendatei überprüfen.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 3543bed7f699fd149ca7f2a6f61e9eb5aad5f1a3
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "67491426"
---
# <a name="check-free-service-usage-included-with-your-azure-free-account"></a>Überprüfen der Nutzung kostenloser Dienste, die in Ihrem kostenlosen Azure-Konto enthalten sind

Ihnen werden keine Gebühren für kostenlose Dienste, die in einem kostenlosen Azure-Konto enthalten sind, in Rechnung gestellt, solange Sie die Grenzwerte der Dienste nicht überschreiten. Um die Grenzwerte einzuhalten, können Sie das Azure-Portal oder die Nutzungsdatendatei zum Überwachen und Nachverfolgen der Nutzung kostenloser Dienste verwenden.

## <a name="check-usage-in-the-azure-portal"></a>Überprüfen der Nutzung im Azure-Portal

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2.  Wählen Sie im Navigationsbereich links die Option **Alle Dienste**.

3.  Wählen Sie **Abonnements**.

4.  Wählen Sie das Abonnement aus, das Sie erstellt haben, als Sie sich für das kostenlose Konto registriert haben.

    ![Screenshot mit allen Abonnements](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  In der Übersicht werden wichtige Informationen zu Ihrem Abonnement angezeigt. Beispiele hierfür sind Abonnement-ID, Angebotstyp und Abonnementname. Sie erhalten auch Informationen zum Ablauf des Guthabens für Ihr kostenloses Konto.

    ![Screenshot mit grundlegenden Abonnementinformationen](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Scrollen Sie nach unten, um Informationen zu Ihren aktuellen und prognostizierten Kosten anzuzeigen. Die Kosten beinhalten die Nutzung der Dienste, die nicht in Ihrem kostenlosen Konto enthalten sind, sowie die Nutzung, die die Grenzwerte der kostenlosen Dienste überschreitet.

    ![Screenshot mit Abonnementkosteninformationen](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Der letzte Teil des Übersichtsabschnitts enthält eine Tabelle mit Informationen zur Nutzung der kostenlosen Dienste.

    ![Screenshot mit Verwendung der kostenlosen Dienste](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Die Tabelle enthält folgende Spalten:

* **Name der Verbrauchseinheit:** Gibt die Maßeinheit für die genutzte Verbrauchseinheit an. Informationen zur Zuordnung von Diensten zu Verbrauchseinheiten finden Sie unter [Grundlegendes zur Zuordnung kostenloser Dienste zu Verbrauchseinheiten](billing-understand-free-service-meter-mapping.md).
* **Nutzung/Limit:** Nutzung und Grenzwert für die Verbrauchseinheit für den aktuellen Monat. Diese Informationen finden Sie auch in der Statusleiste.
* **Status:** Verwendungsstatus der Verbrauchseinheit. Basierend auf Ihrem Nutzungsmuster kann einer der folgenden Status gelten:
  * **Nicht in Verwendung:** Sie haben diese Verbrauchseinheit nicht verwendet, oder die Verwendung der Verbrauchseinheit hat das Abrechnungssystem nicht erreicht.
  * **Überschritten am \<Datum>:** Sie haben den Grenzwert für die Verbrauchseinheit am \<Datum> überschritten.
  * **Überschreitung unwahrscheinlich:** Es ist unwahrscheinlich, dass Sie den Grenzwert für diese Verbrauchseinheit überschreiten.
  * **Überschreitung am \<Datum>:** Sie werden den Grenzwert für die Verbrauchseinheit voraussichtlich am \<Datum> überschreiten.

## <a name="check-usage-with-the-usage-file"></a>Überprüfen der Nutzung anhand der Nutzungsdatendatei

Ihre Nutzungsdatendatei enthält detaillierte Informationen über Ihr Azure-Abonnement. Sie können Ihre monatliche und tägliche Nutzungsdatendatei über das Azure-Kontocenter herunterladen. Informationen zum Herunterladen der Nutzungsdatendatei und erforderlichen Zugriffsrechten finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md). Weitere Informationen zu Spalten in der Nutzungsdatendatei finden Sie unter [Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](billing-understand-your-usage.md).

Die Nutzungsdatendatei enthält Nutzungsinformationen zu kostenlosen und kostenpflichtigen Diensten. An das Ende des Namens kostenloser Dienstverbrauchseinheiten ist **Kostenlos** angefügt. Um nach kostenlosen Verbrauchseinheiten zu suchen, öffnen Sie die Datei in Excel und filtern die Spalte **Kategorie der Verbrauchseinheit** nach Zellen, die den Text **– Kostenlos** enthalten („Textfilter“ &rarr; Filter „Enthält“).


![Screenshot mit Verwendung der kostenlosen Dienste](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- [Upgraden Sie Ihr Abonnement](billing-upgrade-azure-subscription.md)
