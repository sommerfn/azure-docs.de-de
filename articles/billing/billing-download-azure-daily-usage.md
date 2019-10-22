---
title: Anzeigen und Herunterladen der Azure-Nutzung und -Gebühren
description: Hier wird beschrieben, wie Sie Azure-Daten zur täglichen Nutzung und zu den Gebühren herunterladen oder anzeigen.
keywords: Nutzungsabrechnung, Nutzungsgebühren, Nutzung herunterladen, Nutzung anzeigen, Azure-Rechnung, Azure-Nutzung
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: e7d1947b2194c04bb5269887b73e2f4fa13df6e7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375731"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Anzeigen und Herunterladen der Azure-Nutzung und -Gebühren

Sie können eine tägliche Aufschlüsselung Ihrer Azure-Nutzung und -Gebühren vom Azure-Portal herunterladen. Nur bestimmte Rollen (z. B. „Kontoadministrator“ oder „Unternehmensadministrator“) verfügen über die Berechtigung zum Abrufen von Azure-Nutzungsinformationen. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](billing-manage-access.md).

Wenn Sie über eine Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) verfügen, müssen Sie über die Rolle „Besitzer des Abrechnungsprofils“, „Mitwirkender am Abrechnungsprofil“, „Benutzer mit Leseberechtigung für das Abrechnungsprofil“ oder „Rechnungs-Manager“ verfügen, um Ihre Azure-Nutzung und -Gebühren anzeigen zu können.  Bei einer Microsoft-Partnervereinbarung (Microsoft Partner Agreement, MPA) können nur der globale Administrator und Administrator-Agent in der Partnerorganisation Azure-Rechnungen anzeigen und herunterladen. [Überprüfen des Abrechnungskontotyps im Azure-Portal](#check-your-billing-account-type).

## <a name="download-usage-from-the-azure-portal-csv"></a>Herunterladen der Nutzung aus dem Azure-Portal (CSV-Datei)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto oder ein Abrechnungsprofil auswählen.
1. Wählen Sie im Menü auf der linken Seite die Option **Rechnungen** unter **Abrechnung** aus.
1. Suchen Sie im Rechnungsraster nach der Zeile des Abrechnungszeitraums, der der Nutzung entspricht, die Sie herunterladen möchten.
1. Klicken Sie rechts auf das Downloadsymbol oder die Auslassungspunkte (`...`).
1. Wählen Sie im Downloadmenü die Option **Azure-Nutzung und -Gebühren herunterladen** aus.

## <a name="download-usage-for-ea-customers"></a>Herunterladen von Nutzungsdaten für EA-Kunden

Sie müssen Unternehmensadministrator, Kontobesitzer oder Abteilungsadministrator sein, um Nutzungsdaten als EA-Kunde anzeigen und herunterladen zu können. Außerdem muss die Richtlinie zum Anzeigen von Gebühren aktiviert sein.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. Wählen Sie **Verbrauch + Gebühren** aus.
1. Wählen Sie für den Monat, den Sie herunterladen möchten, **Herunterladen** aus.

## <a name="download-usage-for-pending-charges"></a>Herunterladen der ausstehenden Nutzungsgebühren

Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, können Sie die Nutzung für den bisherigen Kalendermonat für den aktuellen Abrechnungszeitraum herunterladen. Diese Nutzungsgebühren wurden noch nicht in Rechnung gestellt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.
3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise zuerst ein Abrechnungskonto auswählen.
4. Suchen Sie im Bereich **Übersicht** nach den Downloadlinks unter den Gebühren für den bisherigen Kalendermonat.
5. Wählen Sie **Azure-Nutzung und -Gebühren** aus.

    ![Screenshot des Blatts „Übersicht“ mit dem Downloadlink](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-billing-account-type"></a>Überprüfen des Abrechnungskontotyps
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und Nutzungsgebühren zu erfahren:

- [Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](billing-understand-your-usage.md)
- [Informationen zu Ihrer Rechnung für Microsoft Azure](billing-understand-your-bill.md)
- [Anzeigen und Herunterladen der Microsoft Azure-Rechnung](billing-download-azure-invoice.md)
- [Anzeigen und Herunterladen der Azure-Preise für Ihre Organisation](billing-ea-pricing.md)

Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, lesen Sie die folgenden Artikel:

- [Grundlegendes zu den Begriffen in der CSV-Datei zu Azure-Nutzung und -Gebühren für eine Microsoft-Kundenvereinbarung](billing-mca-understand-your-usage.md)
- [Grundlegendes zu den Gebühren auf der Rechnung für eine Microsoft-Kundenvereinbarung](billing-mca-understand-your-bill.md)
- [Anzeigen und Herunterladen der Microsoft Azure-Rechnung](billing-download-azure-invoice.md)
- [Anzeigen und Herunterladen von Steuerdokumenten für Ihre Microsoft-Kundenvereinbarung](billing-mca-download-tax-document.md)
- [Anzeigen und Herunterladen der Azure-Preise für Ihre Organisation](billing-ea-pricing.md)
