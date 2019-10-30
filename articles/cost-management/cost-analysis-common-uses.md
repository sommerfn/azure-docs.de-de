---
title: Häufige Anwendungsbereiche der Kostenanalyse in Azure Cost Management
description: In diesem Artikel wird erläutert, wie Sie in Azure Cost Management Ergebnisse für häufige Kostenanalyseaufgaben erhalten.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/16/2019
ms.topic: conceptual
ms.service: cost-management
manager: adwise
ms.custom: ''
ms.openlocfilehash: cc133a75506dcbe552d380e2a6de219a47aca3d8
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72599880"
---
# <a name="common-cost-analysis-uses"></a>Häufige Anwendungsbereiche der Kostenanalyse

Benutzer von Azure Cost Management wünschen sich häufig Antworten auf Fragen, die auch viele andere stellen. In diesem Artikel wird Schritt für Schritt erläutert, wie Sie in Azure Cost Management Ergebnisse für häufige Kostenanalyseaufgaben erhalten.

## <a name="view-cost-breakdown-by-azure-service"></a>Anzeigen der Kostenaufschlüsselung nach Azure-Dienst

Die Kostenanzeige nach Azure-Dienst kann Ihnen helfen, die Teile Ihrer Infrastruktur besser zu verstehen, die die höchsten Kosten verursachen. Beispielsweise können die VM-Computekosten niedrig sein. Doch können aufgrund der Informationsmenge, die von den virtuellen Computern ausgegeben wird, erhebliche Netzwerkkosten entstehen. Es ist von entscheidender Bedeutung, die primären Kostenverursacher bei Ihren Azure-Diensten zu kennen, damit Sie bei Bedarf die Dienstnutzung anpassen können.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie **Kosten nach Dienst** aus, und gruppieren Sie dann nach **Dienstebene**.
1. Ändern Sie die Ansicht in **Tabelle**.

![Kostenaufschlüsselung nach Azure-Dienst](./media/cost-analysis-common-uses/breakdown-by-service.png)

## <a name="view-cost-breakdown-by-azure-resource"></a>Anzeigen der Kostenaufschlüsselung nach Azure-Ressource

Ihre Dienste werden mit Azure-Ressourcen erstellt. Die Überprüfung der Kosten nach Ressourcen kann Ihnen helfen, schnell die Hauptkostenverursacher zu identifizieren. Wenn ein Dienst Ressourcen nutzt, die zu teuer sind, sollten Sie in Erwägung ziehen, zur Kostensenkung Änderungen vorzunehmen.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie **Kosten nach Ressource** aus.
1. Ändern Sie die Ansicht in **Tabelle**.

![Anzeigen der Kostenaufschlüsselung nach Azure-Ressource](./media/cost-analysis-common-uses/cost-by-resource.png)

## <a name="view-cost-breakdown-by-selected-dimensions"></a>Anzeigen der Kostenaufschlüsselung nach ausgewählten Dimensionen

Mithilfe von Dimensionen können Sie Ihre Kosten anhand verschiedener Metadatenwerte organisieren, die in ihren Gebühren angezeigt werden. Beispielsweise könnten Sie Ihre Kosten nach Standort gruppieren.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie den Filter **Gruppieren nach** aus.  
    ![Auswählen einer Gruppe nach Position](./media/cost-analysis-common-uses/group-by.png)
1. Optional können Sie die Ansicht zur späteren Verwendung speichern.
1. Klicken Sie unterhalb des Graphen auf ein Kreisdiagramm, um ausführlichere Daten anzuzeigen.  
    ![Anzeigen der Kostenaufschlüsselung nach ausgewählten Dimensionen](./media/cost-analysis-common-uses/drill-down.png)

## <a name="view-costs-per-day-or-by-month"></a>Anzeigen der Kosten pro Tag oder Monat

Durch die Betrachtung der täglichen und monatlichen Kosten können Sie besser verstehen, ob es eine Woche oder ein Jahr gibt, in der bzw. dem Ihre Kosten höher liegen. Verursacht ein eventuell höherer Datenverkehr von Kunden an Feiertagen einen entsprechenden Anstieg ihrer Azure-Kosten? Ist der Freitag ein kostenträchtigerer Tag als Montag?

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Legen Sie die **Granularität** auf **Monatlich** oder **Täglich** fest.

![Anzeigen der Kosten pro Tag](./media/cost-analysis-common-uses/daily-granularity.png)

## <a name="view-costs-for-a-specific-tag"></a>Anzeigen der Kosten für ein bestimmtes Tag

Viele Azure-Benutzer wenden Tags auf Ihre Ressourcen an, z.B. für eine Kostenstelle oder Entwicklungsumgebung (Produktion und Test), um Kosten besser zu kategorisieren. Tags werden in der Kostenanalyse als Dimension angezeigt. Mit der Dimension können Sie Einblicke in Ihre Kategorisierung mit benutzerdefinierten Tags erhalten.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie **Gruppieren nach** für das Tag aus.

![Anzeigen der Kosten für ein bestimmtes Tag](./media/cost-analysis-common-uses/tag.png)

## <a name="download-your-usage-details"></a>Herunterladen Ihrer Nutzungsdetails

Die Berichtsdatei mit den Nutzungsdetails im CSV-Format enthält eine Aufschlüsselung aller Gebühren, die für eine Rechnung anfallen. Sie können den Bericht mit Ihrer Rechnung vergleichen, die dadurch besser verständlich wird. Jede in Rechnung gestellte Gebühr entspricht den im Nutzungsbericht aufgeschlüsselten Kosten.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Nutzung und Gebühren** für ein Abrechnungskonto oder Abonnement. Beispiel:  **Kostenverwaltung + Abrechnungs** > **Abrechnung** > **Nutzung + Gebühren**.
1. Markieren Sie die Position, die Sie herunterladen möchten, und klicken Sie dann auf das Symbol zum Herunterladen.  
    ![Herunterladen von Nutzungsdaten und Gebühren](./media/cost-analysis-common-uses/download1.png)
1.  Wählen Sie die herunterzuladende Nutzungsdatei aus.  
    ![Auswählen der herunterzuladenden Nutzungsdatei](./media/cost-analysis-common-uses/download2.png)

## <a name="view-monthly-ea-cost-breakdown"></a>Anzeigen der Aufschlüsselung der EA-Kosten pro Monat

Durch die Registrierung Ihres Enterprise Agreements (EA) entstehen Kosten für Ihre gesamte Organisation. Wenn Sie verstehen, wie Kosten anfallen und im Laufe der Zeit in Rechnung gestellt werden, können Sie die entsprechenden Beteiligten besser einbeziehen, um sicherzustellen, dass die Kosten verantwortungsbewusst verwaltet werden.

1. Navigieren Sie im Azure-Portal zu **Kostenverwaltung + Abrechnung** > **Übersicht**.
1. Klicken Sie für den aktuellen Monat auf **Aufschlüsselung**, und zeigen Sie die Entstehung der Zahlungsverpflichtungen an.  
    ![Übersicht über EA-Kosten – Aufschlüsselung in der Übersicht](./media/cost-analysis-common-uses/breakdown1.png)
1.  Klicken Sie auf die Registerkarte **Nutzung + Gebühren**, und zeigen Sie die Aufschlüsselung des Vormonats im ausgewählten Zeitraum an.  
    ![Registerkarte „Nutzung + Gebühren“](./media/cost-analysis-common-uses/breakdown2.png)

## <a name="view-enrollment-monthly-cost-by-term"></a>Anzeigen der monatlichen Kosten der Registrierung nach Laufzeit

Verwenden Sie eine grafische Ansicht der monatlichen Kosten für Ihre Registrierung, um die Kostentrends und die in Rechnung gestellten Beträge für einen bestimmten Zeitraum zu verstehen.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie Ihre Registrierung aus, und legen Sie die Laufzeit der Registrierung fest.
1. Legen Sie die Granularität auf „Monatlich“ und dann die Ansicht auf **Säule (gestapelt)** fest.

Zugunsten einer detaillierteren Analyse können Sie Ihre Daten gruppieren und filtern.

![Monatliche Registrierungskosten nach Laufzeit](./media/cost-analysis-common-uses/enrollment-term1.png)

## <a name="view-ea-enrollment-accumulated-costs"></a>Anzeigen der kumulierten Kosten der EA-Registrierung

Zeigen Sie die im Laufe der Zeit angefallenen Gebühren als kumulierten Nettobetrag an, um die Gesamtausgaben für Ihre Organisation für einen bestimmten Zeitraum zu verstehen.

1. Navigieren Sie im Azure-Portal zur Kostenanalyse für Ihren Bereich. Beispiel:  **Kostenverwaltung + Abrechnung** > **Kostenverwaltung** > **Kostenanalyse**.
1. Wählen Sie Ihre Registrierung aus, und zeigen Sie dann die aktuellen kumulierten Kosten an.

![Kumulierte Kosten der Registrierung](./media/cost-analysis-common-uses/cost-analysis-enrollment.png)

## <a name="next-steps"></a>Nächste Schritte
- Falls Sie den ersten Schnellstart für Cost Management noch nicht abgeschlossen haben, lesen Sie ihn unter [Erste Schritte in der Analyse von Kosten](quick-acm-cost-analysis.md).
- Lesen Sie die [Dokumentation zur Kostenverwaltung](index.yml).
