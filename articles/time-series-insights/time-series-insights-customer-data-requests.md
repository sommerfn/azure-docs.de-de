---
title: Features für Kundendatenanforderungen in Azure Time Series Insights | Microsoft-Dokumentation
description: Zusammenfassung der Funktionen für Kundendatenanforderungen in Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.date: 08/09/2019
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.custom: seodec18
ms.openlocfilehash: fb464d2c75e271bf2f7f1684ed991d01691d70e4
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330936"
---
# <a name="summary-of-customer-data-request-features"></a>Zusammenfassung der Features für Kundendatenanforderungen

Azure Time Series Insights ist ein verwalteter Clouddienst mit Speicher-, Analyse- und Visualisierungskomponenten zur einfachen gleichzeitigen Erfassung, Speicherung, Untersuchung und Analyse unzähliger Ereignisse.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Um personenbezogene Daten, die möglicherweise Gegenstand eines Antrags von betroffenen Personen sind, anzuzeigen, zu exportieren und zu löschen, kann ein Azure Time Series Insights-Mandantenadministrator das Azure-Portal oder die REST-APIs verwenden. Die Verwendung des Azure-Portals für Anträge von betroffenen Personen gestaltet sich für diese Vorgänge weniger komplex und wird von den meisten Benutzern vorgezogen.

## <a name="identifying-customer-data"></a>Identifizieren von Kundendaten

In Azure Time Series Insights gelten Daten, die Administratoren und Benutzern von Time Series Insights zugeordnet sind, als personenbezogene Daten. In Time Series Insights wird die Azure Active Directory-Objekt-ID von Benutzern mit Zugriff auf die Umgebung gespeichert. Im Azure-Portal werden E-Mail-Adressen von Benutzern angezeigt. Diese E-Mail-Adressen werden jedoch in Time Series Insights nicht gespeichert, sie werden über die Azure Active Directory-Objekt-ID in Azure Active Directory dynamisch gesucht.

## <a name="deleting-customer-data"></a>Löschen von Kundendaten

Ein Mandantenadministrator kann Kundendaten über das Azure-Portal löschen.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Bevor Sie jedoch Kundendaten über das Portal löschen, sollten Sie die Zugriffsrichtlinien des Benutzers aus der Time Series Insights-Umgebung im Azure-Portal entfernen. Weitere Informationen finden Sie unter [Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal](time-series-insights-data-access.md).

Sie können Löschvorgänge für Zugriffsrichtlinien auch über die REST-API durchführen. Weitere Informationen finden Sie unter [Access Policies – Delete](https://docs.microsoft.com/rest/api/time-series-insights/management/accesspolicies/delete) (Zugriffsrichtlinien – Delete).

Time Series Insights ist in das Blatt „Policy“ im Azure-Portal integriert. In Time Series Insights sowie auf dem Blatt „Policy“ können Sie die innerhalb des Diensts gespeicherten Benutzerdaten anzeigen, exportieren und löschen. Jeder Löschvorgang, der auf dem Blatt „Policy“ des Azure-Portals durchgeführt wird, zieht die Löschung von Benutzerdaten in Time Series Insights nach sich. Wenn für einen Benutzer beispielsweise eine personenbezogene Abfrage gespeichert wurde, wird diese Abfrage endgültig aus dem Time Series Insights-Explorer gelöscht. Wenn für den Benutzer eine freigegebene Abfrage gespeichert wurde, wird die Abfrage beibehalten, die Benutzerinformationen werden jedoch endgültig gelöscht. Der folgende Hinweis enthält Anweisungen zum Durchführen dieser Aufgaben.

## <a name="exporting-customer-data"></a>Exportieren von Kundendaten

Ähnlich wie beim Löschen von Daten kann ein Mandantenadministrator die in Time Series Insights gespeicherten Daten über das Blatt „Policy“ im Azure-Portal anzeigen und exportieren.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Als Mandantenadministrator können Sie Datenzugriffsrichtlinien innerhalb der Time Series Insights-Umgebung im Azure-Portal anzeigen. Weitere Informationen finden Sie unter [Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal](time-series-insights-data-access.md).

Es ist zudem möglich, Exportvorgänge für Zugriffsrichtlinien über den Vorgang „List By Environment“ in der bereitgestellten REST-API durchzuführen. Weitere Informationen finden Sie unter [Access Policies – List By Environment](https://docs.microsoft.com/rest/api/time-series-insights/management/accesspolicies/listbyenvironment) (Zugriffsrichtlinien – List By Environment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>So löschen Sie in Time Series Insights gespeicherte Daten

Möglicherweise gelangen personenbezogene Daten in den Time Series Insights-Speicher: ein anderes Szenario als bei Benutzer- und Administratordaten. Wenn Sie die in Time Series Insights gespeicherten Daten als personenbezogene Daten einstufen, können Sie diese Daten mit den folgenden Schritten exportieren und löschen:

**Anzeigen und Exportieren von Daten**

Zum Anzeigen und Exportieren von in Time Series Insights gespeicherten Daten müssen Sie die entsprechenden Daten suchen. Sie können Daten über den Time Series Insights-Explorer oder die Time Series Insights-Abfrage-APIs anzeigen und exportieren. Um Daten über den Time Series Insights-Explorer anzuzeigen und zu exportieren, suchen Sie zunächst die betreffenden Benutzerdaten. Klicken Sie nach der Suche mit der rechten Maustaste auf das Diagramm, und wählen Sie **Ereignisse untersuchen** aus. Im dann angezeigten Raster mit den Ereignissen stehen Optionen zum Exportieren der Daten als CSV- und JSON-Datei zur Verfügung.

Weitere Informationen finden Sie unter [Azure Time Series Insights-Explorer](time-series-insights-explorer.md).

**Löschen von Daten**

Derzeit ist in Time Series Insights die differenzierte Löschung von Daten nicht möglich. Time Series Insights bietet jedoch die Möglichkeit, die in Time Series Insights gespeicherten Kundendaten durch Konfigurieren von Aufbewahrungsrichtlinien zu entfernen. Sie können die Aufbewahrungsdauer für die gesamte Time Series Insights-Umgebung in die Anzahl von Tagen anpassen, die Ihren Anforderungen für Löschvorgänge entspricht.

Weitere Informationen finden Sie unter [Konfigurieren der Vermerkdauer in Time Series Insights](time-series-insights-how-to-configure-retention.md).

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie weitere Informationen zum [Gewähren von Datenzugriff auf Ihre Azure Time Series Insights-Umgebung](./time-series-insights-data-access.md).

* Anzeigen des [Azure Time Series Insights-Explorers](time-series-insights-explorer.md).

* Informationen zum [Konfigurieren der Vermerkdauer in Time Series Insights](time-series-insights-how-to-configure-retention.md).