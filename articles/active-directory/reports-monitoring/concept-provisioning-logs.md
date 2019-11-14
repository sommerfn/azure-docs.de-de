---
title: Bereitstellungsprotokolle im Azure Active Directory-Portal (Vorschau) | Microsoft-Dokumentation
description: Einführung in Bereitstellungsaktivitätsberichte im Azure Active Directory-Portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/04/2019
ms.author: markvi
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6e0c697f9ab9796feade9b4d5c2a64794f3980b
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612797"
---
# <a name="provisioning-reports-in-the-azure-active-directory-portal-preview"></a>Bereitstellungsberichte im Azure Active Directory-Portal (Vorschau)

Die Architektur für die Berichterstellung in Azure Active Directory (Azure AD) umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldungen**: Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung.
    - **Überwachungsprotokolle** - [Überwachungsprotokolle](concept-audit-logs.md) stellen Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung, zu verwalteten Anwendungen und zu Verzeichnisaktivitäten bereit.
    - **Bereitstellungsprotokolle:** enthalten Systemaktivitäten zu Benutzern, Gruppen und Rollen, die mit dem Azure AD-Bereitstellungsdienst bereitgestellt werden. 

- **Sicherheit** 
    - **Riskante Anmeldungen**: Eine [riskante Anmeldung](concept-risky-sign-ins.md) ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist.
    - **Benutzer mit Risikomarkierung**: Ein [Benutzer mit Risikomarkierung](concept-user-at-risk.md) ist ein Indikator für ein ggf. kompromittiertes Benutzerkonto.

In diesem Thema erhalten Sie einen Überblick über den Bereitstellungsbericht.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“, „Sicherheitsleseberechtigter“, „Berichtsleser“, „Anwendungsadministrator“ und „Cloudanwendungsadministrator“
* Globale Administratoren


### <a name="what-azure-ad-license-do-you-need-to-access-provisioning-activities"></a>Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf Bereitstellungsaktivitäten?

Ihrem Mandanten muss eine Azure AD Premium-Lizenz zugewiesen sein, damit der Gesamtbericht für Bereitstellungsaktivitäten angezeigt werden kann. Unter [Erste Schritte mit Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) erfahren Sie, wie Sie ein Upgrade für Ihre Azure Active Directory-Edition durchführen. 

## <a name="provisioning-logs"></a>Bereitstellungsprotokolle

Die Bereitstellungsprotokolle bieten Antworten auf die folgenden Fragen:

* Welche Gruppen wurden erfolgreich in ServiceNow erstellt?
* Wie wurden Rollen aus Amazon Web Services importiert?
* Welche Benutzer konnten in Dropbox nicht erstellt werden?

Sie können auf die Bereitstellungsprotokolle zugreifen, indem Sie im [Azure-Portal](https://portal.azure.com) auf dem Blatt **Azure Active Directory** im Abschnitt **Überwachung** die Option **Bereitstellungsprotokolle** auswählen. Bei einigen Bereitstellungsdatensätzen kann es bis zu zwei Stunden dauern, bis sie im Portal angezeigt werden.

![Bereitstellungsprotokolle](./media/concept-provisioning-logs/access-provisioning-logs.png "Bereitstellungsprotokolle")


Ein Bereitstellungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Identität
- Aktion
- Quellsystem
- Zielsystem
- Status
- Datum


![Standardspalten](./media/concept-provisioning-logs/default-columns.png "Standardspalten")

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Spaltenauswahl](./media/concept-provisioning-logs/column-chooser.png "Spaltenauswahl")

Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

![Verfügbare Spalten](./media/concept-provisioning-logs/available-columns.png "Verfügbare Spalten")

Wählen Sie in der Listenansicht ein Element aus, um ausführlichere Informationen zu erhalten.

![Ausführliche Informationen](./media/concept-provisioning-logs/steps.png "Filter")


## <a name="filter-provisioning-activities"></a>Filtern von Bereitstellungsaktivitäten

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Bereitstellungsdaten anhand der folgenden Standardfelder filtern. Beachten Sie, dass die Werte in den Filtern basierend auf Ihrem Mandanten dynamisch gefüllt werden. Wenn im Mandanten beispielsweise keine Erstellungsereignisse vorhanden sind, gibt es keine Filteroptionen für „Erstellen“.

- Identity
- Aktion
- Quellsystem
- Zielsystem
- Status
- Date


![Filter](./media/concept-provisioning-logs/filter.png "Filter")

Mit dem Filter **Identität** können Sie den Namen oder die Identität angeben, der bzw. die für Sie relevant ist. Diese Identität kann ein Benutzer, eine Gruppe, eine Rolle oder ein anderes Objekt sein. Sie können nach dem Namen oder der ID des Objekts suchen. Die ID variiert je nach Szenario. Bei der Bereitstellung eines Objekts aus Azure AD in Salesforce ist die Quell-ID beispielsweise die Objekt-ID des Benutzers in Azure AD und die Ziel-ID die ID des Benutzers in Salesforce. Wenn die Bereitstellung aus Workday in Active Directory erfolgt, ist die Quell-ID die Mitarbeiter-ID des Workday-Mitarbeiters. Beachten Sie, dass der Name des Benutzers möglicherweise nicht immer in der Spalte „Identität“ enthalten ist. Es gibt jedoch immer eine ID. 

Mit dem Filter **Quellsystem** können Sie angeben, woher die Identität bereitgestellt wird. Wenn Sie z. B. ein Objekt aus Azure AD in ServiceNow bereitstellen, ist Azure AD das Quellsystem. 

Mit dem Filter **Zielsystem** können Sie angeben, wo die Identität bereitgestellt wird. Wenn Sie z. B. ein Objekt aus Azure AD in ServiceNow bereitstellen, ist ServiceNow das Zielsystem. 

Für den Filter **Zustand** können Sie eine der folgenden Optionen auswählen:

- Alle
- Erfolgreich
- Fehler
- Übersprungen

Mit dem Filter **Aktion** können Sie Folgendes filtern:

- Erstellen 
- Aktualisieren
- Löschen
- Disable
- Andere

Mit dem Filter **Datum** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 1 Monat
- 7 Tage
- 30 Tage
- 24 Stunden
- Benutzerdefiniertes Zeitintervall

Beim Auswählen eines benutzerdefinierten Zeitraums können Sie ein Startdatum und ein Enddatum konfigurieren.


Zusätzlich zu den Standardfeldern können Sie bei Auswahl auch die folgenden Felder in den Filter aufnehmen:

- **Auftrags-ID:** Jeder Anwendung, für die Sie die Bereitstellung aktiviert haben, ist eine eindeutige Auftrags-ID zugeordnet.   

- **Zyklus-ID:** eindeutige Identifizierung des Bereitstellungszyklus. Sie können diese ID freigeben, damit nach dem Zyklus gesucht werden kann, in dem das Ereignis aufgetreten ist.

- **Änderungs-ID:** eindeutiger Bezeichner für das Bereitstellungsereignis. Sie können diese ID freigeben, damit nach dem Bereitstellungsereignis gesucht werden kann.   



  

## <a name="provisioning-details"></a>Bereitstellungsdetails 

Wenn Sie ein Element in der Bereitstellungslistenansicht auswählen, erhalten Sie weitere Informationen zu diesem Element.
Die Details werden basierend auf den folgenden Kategorien gruppiert:

- Schritte

- Problembehandlung und Empfehlungen

- Geänderte Eigenschaften

- Zusammenfassung


![Filter](./media/concept-provisioning-logs/provisioning-tabs.png "Registerkarten")



### <a name="steps"></a>Schritte

Auf der Registerkarte **Schritte** sind die Schritte zum Bereitstellen eines Objekts aufgeführt. Die Bereitstellung eines Objekts kann aus vier Schritten bestehen: 

- Importieren des Objekts
- Ermitteln, ob das Objekt sich im Bereich befindet
- Abgleichen des Objekts zwischen Quelle und Ziel
- Bereitstellen des Objekts (Ausführen einer Aktion, z. B. Erstellen, Aktualisieren, Löschen oder Deaktivieren)



![Filter](./media/concept-provisioning-logs/steps.png "Filter")


### <a name="troubleshoot-and-recommendations"></a>Problembehandlung und Empfehlungen


Auf der Registerkarte **Problembehandlung und Empfehlungen** werden der Fehlercode und der entsprechende Grund angezeigt. Die Fehlerinformationen sind nur im Fall eines Fehlers verfügbar. 


### <a name="modified-properties"></a>Geänderte Eigenschaften

Auf der Registerkarte **Geänderte Eigenschaften** werden der alte und der neue Wert angezeigt. Wenn kein alter Wert vorhanden ist, bleibt die entsprechende Spalte leer. 


### <a name="summary"></a>Zusammenfassung

Die Registerkarte **Zusammenfassung** bietet eine Übersicht über die Vorgänge und die Bezeichner für das Objekt im Quell- und Zielsystem. 

## <a name="what-you-should-know"></a>Wichtige Informationen

- Gemeldete Bereitstellungsdaten werden bei Verwendung einer Premium Edition 30 Tage und bei einer kostenlosen Edition 7 Tage im Azure-Portal gespeichert.

- Sie können das Attribut „Änderungs-ID“ als eindeutigen Bezeichner verwenden. Dies ist beispielsweise bei der Kommunikation mit dem Produktsupport hilfreich.

- Derzeit gibt es keine Option zum Herunterladen von Bereitstellungsdaten.

- Die Protokollanalyse wird derzeit nicht unterstützt.

- Wenn Sie über den Kontext einer App auf die Bereitstellungsprotokolle zugreifen, werden Ereignisse nicht automatisch für die jeweilige App gefiltert, wie das bei Überwachungsprotokollen der Fall ist.

## <a name="error-codes"></a>Fehlercodes

Anhand der folgenden Tabelle können Sie besser verstehen, wie Sie mögliche Fehler in Bereitstellungsprotokollen beheben. Stellen Sie mithilfe des Links unten auf dieser Seite für alle Fehlercodes, die nicht aufgeführt sind, Feedback bereit. 

|Fehlercode|BESCHREIBUNG|
|---|---|
|Conflict, EntryConflict|Korrigieren Sie die in Konflikt stehenden Attributwerte entweder in Azure AD oder in der Anwendung, oder überprüfen Sie die Konfiguration der übereinstimmenden Attribute, wenn das in Konflikt stehende Benutzerkonto hätte abgeglichen und übernommen werden sollen. Weitere Informationen zum Konfigurieren von übereinstimmenden Attributen finden Sie in der folgenden [Dokumentation](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes).|
|TooManyRequests|Die Ziel-App hat diesen Versuch zum Aktualisieren des Benutzers abgelehnt, weil sie überlastet ist und zu viele Anforderungen empfängt. Sie brauchen nichts zu tun. Dieser Versuch wird automatisch wiederholt. Außerdem wurde Microsoft über dieses Problem informiert.|
|InternalServerError |Die Ziel-App hat einen unerwarteten Fehler zurückgegeben. Möglicherweise gibt es ein Dienstproblem mit der Zielanwendung, das ein ordnungsgemäßes Funktionieren verhindert. Dieser Versuch wird automatisch in 40 Minuten wiederholt.|
|InsufficientRights, MethodNotAllowed, NotPermitted, Unauthorized| Azure AD konnte sich zwar bei der Zielanwendung authentifizieren, war aber nicht zum Ausführen der Aktualisierung autorisiert. Überprüfen Sie alle Anweisungen, die von der Zielanwendung bereitgestellt werden, sowie das entsprechende [Tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) für die Anwendung.|
|UnprocessableEntity|Die Zielanwendung hat eine unerwartete Antwort zurückgegeben. Die Konfiguration der Zielanwendung ist möglicherweise nicht korrekt, oder es gibt ein Dienstproblem mit der Zielanwendung, das ein ordnungsgemäßes Funktionieren verhindert.|
|WebExceptionProtocolError |Beim Herstellen einer Verbindung mit der Zielanwendung ist ein HTTP-Protokollfehler aufgetreten. Sie brauchen nichts zu tun. Dieser Versuch wird automatisch in 40 Minuten wiederholt.|
|InvalidAnchor|Ein Benutzer, der zuvor vom Bereitstellungsdienst erstellt oder abgeglichen wurde, ist nicht mehr vorhanden. Stellen Sie sicher, dass der Benutzer vorhanden ist. Um einen erneuten Abgleich aller Benutzer zu erzwingen, verwenden Sie die Microsoft Graph-API, um [den Auftrag neu zu starten](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http). Beachten Sie, dass durch den Neustart der Bereitstellung ein Startzyklus auslöst wird, der einige Zeit dauern kann. Außerdem wird der Cache gelöscht, der vom Bereitstellungsdienst für den Betrieb verwendet wird. Dies bedeutet, dass alle Benutzer und Gruppen im Mandanten erneut ausgewertet werden müssen. Möglicherweise werden auch bestimmte Bereitstellungsereignisse gelöscht.|
|NotImplemented | Die Ziel-App hat eine unerwartete Antwort zurückgegeben. Die Konfiguration der App ist möglicherweise nicht korrekt, oder es gibt ein Dienstproblem mit der Ziel-App, das deren ordnungsgemäßes Funktionieren verhindert. Überprüfen Sie alle Anweisungen, die von der Zielanwendung bereitgestellt werden, sowie das entsprechende [Tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) für die Anwendung. |
|MandatoryFieldsMissing, MissingValues |Der Benutzer konnte nicht erstellt werden, da erforderliche Werte fehlten. Korrigieren Sie die fehlenden Attributwerte im Quelldatensatz, oder überprüfen Sie die Konfiguration der übereinstimmenden Attribute, um sicherzustellen, dass die erforderlichen Felder nicht ausgelassen wurden. [Erfahren Sie mehr](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) über das Konfigurieren von übereinstimmenden Attributen.|
|SchemaAttributeNotFound |Der Vorgang konnte nicht durchgeführt werden, da ein Attribut angegeben wurde, das in der Zielanwendung nicht vorhanden ist. In der [Dokumentation](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) finden Sie Informationen zum Anpassen von Attributen. Stellen Sie außerdem sicher, dass Ihre Konfiguration korrekt ist.|
|InternalError |Im Azure AD-Bereitstellungsdienst ist ein interner Dienstfehler aufgetreten. Sie brauchen nichts zu tun. Dieser Versuch wird automatisch in 40 Minuten wiederholt.|
|InvalidDomain |Der Vorgang konnte aufgrund eines Attributwerts mit einem ungültigen Domänennamen nicht ausgeführt werden. Aktualisieren Sie den Domänennamen für den Benutzer, oder fügen Sie ihn der Liste zulässiger Domänen in der Zielanwendung hinzu. |
|Timeout |Der Vorgang konnte nicht abgeschlossen werden, da die Antwort der Zielanwendung zu lange gedauert hat. Sie brauchen nichts zu tun. Dieser Versuch wird automatisch in 40 Minuten wiederholt.|
|LicenseLimitExceeded|Der Benutzer konnte in der Zielanwendung nicht erstellt werden, da für diesen Benutzer keine Lizenzen verfügbar sind. Erwerben Sie entweder zusätzliche Lizenzen für die Zielanwendung, oder überprüfen Sie die Konfiguration der Benutzerzuweisungen und Attributzuordnungen, um sicherzustellen, dass die richtigen Attribute den richtigen Benutzern zugewiesen sind.|
|DuplicateTargetEntries  |Der Vorgang konnte nicht abgeschlossen werden, da in der Zielanwendung mehr als ein Benutzer mit den konfigurierten übereinstimmenden Attributen gefunden wurde. Entfernen Sie entweder den doppelten Benutzer aus der Zielanwendung, oder konfigurieren Sie die Attributzuordnungen neu, wie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) beschrieben.|
|DuplicateSourceEntries | Der Vorgang konnte nicht abgeschlossen werden, da mehr als ein Benutzer mit den konfigurierten übereinstimmenden Attributen gefunden wurde. Entfernen Sie entweder den doppelten Benutzer, oder konfigurieren Sie die Attributzuordnungen neu, wie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) beschrieben.|

## <a name="next-steps"></a>Nächste Schritte

* [Überprüfen des Status der Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)
* [Problem beim Konfigurieren der Benutzerbereitstellung für eine Azure AD-Kataloganwendung](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)


