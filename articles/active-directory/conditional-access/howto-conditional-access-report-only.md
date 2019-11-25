---
title: Konfigurieren einer Richtlinie für bedingten Zugriff im reinen Berichtsmodus – Azure Active Directory
description: Verwenden des reinen Berichtsmodus des bedingten Zugriffs zur Unterstützung der Einführung
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3cd57c09c08ab4c86feeca27915639123b439d0
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73180038"
---
# <a name="configure-a-conditional-access-policy-in-report-only-mode-preview"></a>Konfigurieren einer Richtlinie für bedingten Zugriff im reinen Berichtsmodus (Vorschauversion)

Gehen Sie wie folgt vor, um eine Richtlinie für bedingten Zugriff im reinen Berichtsmodus zu konfigurieren:

1. Melden Sie sich als Administrator für bedingten Zugriff, Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Konfigurieren Sie die Richtlinienbedingungen und die erforderlichen Steuerelemente zur Rechteerteilung nach Bedarf.
1. Legen Sie unter **Richtlinie aktivieren** die Umschaltfläche auf den Modus **Nur Bericht** fest.
1. Wählen Sie **Speichern** aus.

> [!TIP]
> Sie können den Status **Richtlinie aktivieren** einer vorhandenen Richtlinie von **Ein** in **Nur Bericht** ändern, aber dadurch wird die Richtlinienerzwingung deaktiviert. 

Sie können das Ergebnis von „Nur Bericht“ in Azure AD-Anmeldeprotokollen anzeigen.

Gehen Sie wie folgt vor, um das Ergebnis einer Richtlinie im reinen Berichtsmodus für eine bestimmte Anmeldung anzuzeigen:

1. Melden Sie sich als Berichtleseberechtigter, Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Anmeldungen**.
1. Wählen Sie eine Anmeldung aus, oder fügen Sie Filter hinzu, um die Ergebnisse einzugrenzen.
1. Wählen Sie im Bereich **Details** die Registerkarte **Nur Bericht (Vorschau)** aus, um die bei der Anmeldung ausgewerteten Richtlinien anzuzeigen.

## <a name="set-up-azure-monitor-integration-with-azure-ad"></a>Einrichten der Integration von Azure Monitor in Azure AD

Damit Sie die aggregierten Auswirkungen von Richtlinien für bedingten Zugriff mithilfe der neuen Arbeitsmappe für Erkenntnisse zum bedingten Zugriff anzeigen können, müssen Sie Azure Monitor in Azure AD integrieren und die Anmeldeprotokolle exportieren. Zur Einrichtung dieser Integration müssen Sie zwei Schritte ausführen: 

1. [Registrieren Sie sich für ein Azure Monitor-Abonnement, und erstellen Sie einen Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).
1. [Exportieren Sie die Anmeldeprotokolle aus Azure AD in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics).

Weitere Informationen zur Preisgestaltung für Azure Monitor finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/). Ressourcen zum Schätzen von Kosten, zum Festlegen eines Tageslimits oder zum Anpassen des Datenaufbewahrungszeitraums finden Sie im Artikel [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](../../azure-monitor/platform/manage-cost-storage.md#estimating-the-costs-to-manage-your-environment).

## <a name="view-conditional-access-insights-workbook"></a>Anzeigen der Arbeitsmappe für Erkenntnisse zum bedingten Zugriff

Nachdem Sie Ihre Azure AD-Protokolle in Azure Monitor integriert haben, können Sie die Auswirkungen von Richtlinien für bedingten Zugriff mithilfe der neuen Arbeitsmappen für Erkenntnisse zum bedingten Zugriff überwachen.

1. Melden Sie sich als Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Arbeitsmappen**.
1. Wählen Sie **Erkenntnisse zum bedingten Zugriff (Vorschau)** aus.
1. Wählen Sie in der Dropdownliste **Richtlinien für bedingten Zugriff** eine oder mehrere Richtlinien aus. Standardmäßig sind alle aktivierten Richtlinien ausgewählt.
1. Wählen Sie einen Zeitbereich aus (wenn der Zeitbereich das verfügbare Dataset überschreitet, werden im Bericht alle verfügbaren Daten angezeigt). Nachdem Sie die **Richtlinie für bedingten Zugriff** und die Parameter für den **Zeitbereich** festgelegt haben, wird der Bericht geladen.
   1. Suchen Sie optional nach einzelnen **Benutzern** oder **Apps**, um den Bereich des Berichts einzugrenzen.
1. Wählen Sie für die Anzeige der Daten im Zeitbereich zwischen der Anzahl von Benutzern oder der Anzahl von Anmeldungen aus.
1. Abhängig von der **Datenansicht** zeigt die **Zusammenfassung der Auswirkungen** die Anzahl von Benutzern oder Anmeldungen im Bereich der ausgewählten Parameter gruppiert nach Gesamtanzahl, **Erfolg**, **Fehler**, **Benutzeraktion erforderlich** und **Nicht angewendet** an. Wählen Sie eine Kachel aus, um die Anmeldungen eines bestimmten Ergebnistyps zu untersuchen. 
   1. Wenn Sie die Arbeitsmappenparameter geändert haben, können Sie eine Kopie zur späteren Verwendung speichern. Wählen Sie oben im Bericht das Symbol „Speichern“ aus, und geben Sie einen Namen und den Speicherort für die Speicherung an.
1. Scrollen Sie nach unten, um die Aufschlüsselung der Anmeldungen für jede Bedingung anzuzeigen.
1. Sehen Sie sich im unteren Bereich des Berichts die **Anmeldeinformationen** an, um einzelne, nach den oben aufgeführten Auswahlkriterien gefilterte Anmeldeereignisse zu untersuchen.

> [!TIP] 
> Müssen Sie einen Drilldown für eine bestimmte Abfrage ausführen oder die Anmeldeinformationen exportieren? Wählen Sie die Schaltfläche rechts neben einer beliebigen Abfrage aus, um die entsprechende Abfrage in Log Analytics zu öffnen. Wählen Sie „Exportieren“ aus, um die Daten in eine CSV-Datei oder nach Power BI zu exportieren.

## <a name="common-problems-and-solutions"></a>Häufige Probleme und Lösungen

### <a name="why-are-the-queries-in-the-workbook-failing"></a>Warum können die Abfragen in der Arbeitsmappe nicht ausgeführt werden?

Kunden haben festgestellt, dass Abfragen manchmal nicht ausgeführt werden, wenn der Arbeitsmappe falsche oder mehrere Arbeitsbereiche zugeordnet sind. Um dieses Problem zu beheben, klicken Sie oben in der Arbeitsmappe auf **Bearbeiten**, und klicken Sie dann auf das Zahnradsymbol „Einstellungen“. Wählen Sie nicht der Arbeitsmappe zugeordnete Arbeitsbereiche aus, und entfernen Sie diese Arbeitsbereiche. Jeder Arbeitsmappe sollte nur ein Arbeitsbereich zugeordnet sein.

### <a name="why-doesnt-the-conditional-access-policies-dropdown-parameter-contain-my-policies"></a>Warum sind meine Richtlinien nicht im Dropdownparameter „Richtlinien für bedingten Zugriff“ enthalten?

Die Dropdownliste „Richtlinien für bedingten Zugriff“ wird durch Abfragen der letzten Anmeldungen über einen Zeitraum von 4 Stunden aufgefüllt. Wenn bei einem Mandanten in den letzten vier Stunden keine Anmeldungen erfolgt sind, ist es möglich, dass die Dropdownliste leer ist. Wenn es sich bei dieser Verzögerung um ein dauerhaftes Problem handelt, beispielsweise bei kleinen Mandanten mit seltenen Anmeldungen, können Administratoren die Abfrage für die Dropdownliste „Richtlinien für bedingten Zugriff“ bearbeiten und den Zeitraum für die Abfrage auf eine Dauer von mehr als 4 Stunden verlängern.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

Weitere Informationen zu Azure AD-Arbeitsmappen finden Sie im Artikel [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](../reports-monitoring/howto-use-azure-monitor-workbooks.md).
