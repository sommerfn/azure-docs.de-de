---
title: Untersuchen von Risiken in Azure Active Directory Identity Protection (aktualisiert) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie riskante Benutzer, Erkennungen und Anmeldungen in Azure Active Directory Identity Protection untersuchen (aktualisiert).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6e9715b63ccfbf1da3bdaedf947ea4cd65109b7
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129486"
---
# <a name="how-to-investigate-risk"></a>Anleitung: Untersuchen eines Risikos

Anhand der Berichte „Riskante Anmeldungen“, „Riskante Benutzer“ und „Risikoerkennung“ können Sie einen Einblick in die Risiken in Ihrer Umgebung gewinnen und die Risiken genauer untersuchen. Durch die Möglichkeit, die riskanten Anmeldungen und Benutzer und die Erkennungen zu filtern und zu sortieren, können Sie potenzielle Angriffe auf Ihre Organisation besser einordnen. 

## <a name="risky-users-report"></a>Bericht „Riskante Benutzer“

Mit den Informationen im Bericht „Riskante Benutzer“ können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Welche Benutzer haben ein hohes Risiko?
- Bei welchen Benutzern wurde der Risikozustand bereinigt?

Der erste Einstiegspunkt für diesen Bericht ist der Abschnitt **Untersuchen** auf der Seite „Sicherheit“.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/01.png)

In der Standardansicht für den Bericht „Riskante Benutzer“ wird Folgendes anzeigt:

- NAME
- Risikozustand
- Risikostufe
- Risikodetail
- Letzte Aktualisierung des Risikos
- type
- Status

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/03.png)

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/04.png)

In den Spalten des Dialogfelds können Sie weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/05.png)

In der Detailansicht wird Folgendes angezeigt:

- Grundlegende Informationen
- Letzte riskante Anmeldungen
- Risikoerkennungen ohne Bezug zu einer Anmeldung
- Risikoverlauf

Außerdem können Sie Folgendes tun:

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/08.png)

- Zeigen Sie alle Anmeldungsverknüpfungen an, um den Anmeldungsbericht für den jeweiligen Benutzer anzuzeigen.
- Zeigen Sie alle riskanten Anmeldungen an, um alle Anmeldungen für den Benutzer anzuzeigen, die als riskant gekennzeichnet wurden.
- Setzen Sie das Kennwort eines Benutzers zurück, wenn Sie der Meinung sind, dass die Identität des Benutzers gefährdet ist.
- Verwerfen Sie ein Benutzerrisiko, wenn Sie denken, dass die aktiven Risikoerkennungen eines Benutzers falsch positive Meldungen sind. Weitere Informationen finden Sie im Artikel [Senden von Feedback zu Risikoerkennungen in Azure AD Identity Protection](howto-provide-risk-event-feedback.md).

### <a name="filter-risky-users"></a>Filtern von riskanten Benutzern

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Daten zu den riskanten Benutzern anhand der folgenden Standardfelder filtern:

- NAME
- Username
- Risikozustand
- Risikostufe
- type
- Status

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/06.png)

Für den Filter **Name** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Für den Filter **Risikozustand** können Sie eine der folgenden Optionen auswählen:

- Gefährdet
- Bereinigt
- Verworfen

Für den Filter **Risikostufe** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig

Für den Filter **Typ** können Sie eine der folgenden Optionen auswählen:

- Gast
- Member

Für den Filter **Zustand** können Sie eine der folgenden Optionen auswählen:

- Deleted
- Aktiv

### <a name="download-risky-users-data"></a>Herunterladen von Daten zu riskanten Benutzern

Sie können die Daten zu riskanten Benutzern herunterladen, wenn Sie außerhalb des Azure-Portals damit arbeiten möchten. Wenn Sie auf „Herunterladen“ klicken, wird eine CSV-Datei mit den neuesten 2.500 Datensätzen erstellt. 

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/07.png)

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf „Spalten“ klicken.
 
Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.
 
Wenn Sie weitere Informationen zu einem riskanten Benutzer haben möchten, klicken Sie zum Erweitern auf den Drawer „Details“.

## <a name="risky-sign-ins-report"></a>Bericht über riskante Anmeldungen

Mit den Informationen im Bericht „Riskante Anmeldungen“ können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wie viele erfolgreiche Anmeldungen gab es in der letzten Woche, für die aufgrund einer anonymen IP-Adresse eine Risikoerkennung gemeldet wurde?
- Bei welchen Benutzern wurde im letzten Monat eine Gefährdung bestätigt?
- Bei welchen Benutzern gab es riskante Anmeldungen im Office 365-Portal?

Der erste Einstiegspunkt für diesen Bericht ist der Abschnitt **Untersuchen** auf der Seite „Sicherheit“.

![Bericht über riskante Anmeldungen](./media/howto-investigate-risky-users-signins/02.png)

In der Standardansicht für den Bericht „Riskante Anmeldungen“ wird Folgendes anzeigt:

- Date
- Benutzer
- Anwendung
- Anmeldestatus
- Risikozustand
- Risikostufe (aggregiert)
- Risikostufe (Echtzeit)
- Bedingter Zugriff
- MFA erforderlich  

![Bericht über riskante Anmeldungen](./media/howto-investigate-risky-users-signins/09.png)

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/11.png)

In den Spalten des Dialogfelds können Sie weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/12.png)

In der Detailansicht wird Folgendes angezeigt:

- Grundlegende Informationen
- Geräteinformationen
- Informationen zum Risiko
- MFA-Informationen
- Bedingter Zugriff

Außerdem können Sie Folgendes tun:

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/13.png)

- Gefährdung bestätigen 
- Sicherheit bestätigen

Weitere Informationen finden Sie im Artikel [Senden von Feedback zu Risikoerkennungen in Azure AD Identity Protection](howto-provide-risk-event-feedback.md).

### <a name="filter-risky-sign-ins"></a>Filtern von riskanten Anmeldungen

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Daten zu den riskanten Anmeldungen anhand der folgenden Standardfelder filtern:

- Benutzer
- Anwendung
- Anmeldestatus
- Risikozustand
- Risikostufe (aggregiert)
- Risikostufe (Echtzeit)
- Bedingter Zugriff
- Date
- Art der Risikostufe

![Bericht über riskante Anmeldungen](./media/howto-investigate-risky-users-signins/14.png)

Für den Filter **Name** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Für den Filter **Anwendung** können Sie die Cloud-App angeben, auf die der Benutzer zugreifen wollte.

Für den Filter **Anmeldestatus** können Sie eine der folgenden Optionen auswählen:

- Alle
- Erfolgreich
- Fehler

Für den Filter **Risikozustand** können Sie eine der folgenden Optionen auswählen:

- Gefährdet
- Als gefährdet bestätigt
- Als sicher bestätigt
- Verworfen
- Bereinigt

Für den Filter **Risikostufe (aggregiert)** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig

Für den Filter **Risikostufe (Echtzeit)** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig

Für den Filter **Bedingter Zugriff** können Sie eine der folgenden Optionen auswählen:

- Alle
- Nicht angewendet
- Erfolgreich
- Fehler

Mit dem Filter **Datum** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.
Mögliche Werte:

- Letzter Monat
- Letzte 7 Tage
- Letzte 24 Stunden
- Benutzerdefiniertes Zeitintervall

### <a name="download-risky-sign-ins-data"></a>Herunterladen von Daten zu riskanten Anmeldungen

Sie können die Daten zu riskanten Anmeldungen herunterladen, wenn Sie außerhalb des Azure-Portals damit arbeiten möchten. Wenn Sie auf „Herunterladen“ klicken, wird eine CSV-Datei mit den neuesten 2.500 Datensätzen erstellt. 

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/15.png)


## <a name="risk-detections-report"></a>Bericht „Risikoerkennung“

Der Bericht „Risikoerkennung“ enthält Informationen zu allen Identity Protection-Risikoerkennungen in Ihrem Mandanten.


In der Standardansicht für den Bericht „Risikoerkennung“ wird Folgendes anzeigt:

- Date

- Benutzer

- IP-Adresse

- Location

- Erkennungstyp

- Risikozustand

- Risikostufe

- Anfrage-ID
 

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken. In den Spalten des Dialogfelds können Sie weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.


In der Detailansicht werden zusätzliche Informationen zu den Risikoerkennungen angezeigt.

Die Detailansicht enthält außerdem Links zum

- Anzeigen des Risikoberichts des Benutzers
- Anzeigen der Anmeldungen des Benutzers
- Anzeigen der riskanten Anmeldungen des Benutzers
- Anzeigen einer verknüpften riskanten Anmeldung
- Anzeigen der Risikoerkennungen des Benutzers


### <a name="filter-risk-detections"></a>Filtern von Risikoerkennungen

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Risikoerkennungsdaten anhand der folgenden Standardfelder filtern:

- Zeitpunkt der Erkennung
- Erkennungstyp
- Risikozustand
- Risikostufe
- Benutzer
- Username
- IP-Adresse
- Location
- Zeitpunkt der Erkennung
- Aktivität 
- `Source`
- Typ des Tokenausstellers
- Anfrage-ID
- Korrelations-ID


Mit dem Filter **Zeitpunkt der Erkennung** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen. Für diesen Filter können Sie eine der folgenden Optionen auswählen:
- Letzte 90 Tage
- Letzte 30 Tage
- Letzte 7 Tage
- Letzte 24 Stunden
- Benutzerdefiniertes Zeitintervall

Mit dem Filter **Erkennungstyp** können Sie den Typ der Risikoerkennung angeben (z. B. ungewöhnliche Anmeldeeigenschaften).

Für den Filter **Risikozustand** können Sie eine der folgenden Optionen auswählen:

- Gefährdet
- Als gefährdet bestätigt
- Als sicher bestätigt
- Verworfen
- Bereinigt

Für den Filter **Risikostufe** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig

Mit dem Filter **Benutzer** können Sie den Namen oder die Objekt-ID des gewünschten Benutzers angeben.

Mit dem Filter **Benutzername** können Sie den Benutzerprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Mit dem Filter **Zeitpunkt der Erkennung** können Sie angeben, ob die Erkennung in Echtzeit oder offline erfolgt ist. Hinweis: Sie können Anmeldungen mit Echtzeiterkennungen mithilfe der Anmelderisiko-Richtlinie auswerten. 

Mit dem Filter **Aktivität** können Sie angeben, ob die Erkennung mit einer Anmeldung (z. B. anonyme IP-Adresse) oder einem Benutzer (z. B. kompromittierte Anmeldeinformationen) verknüpft wurde.

Mit dem Filter **Quelle** können Sie die Quelle der Risikoerkennung angeben (z. B. Azure AD oder Microsoft Cloud App Security). 

Mit dem Filter **Typ des Tokenausstellers** können Sie angeben, wo das Token ausgestellt wurde (z. B. Azure AD oder Active Directory-Verbunddienste (AD FS)).


### <a name="download-risk-detections-data"></a>Herunterladen der Risikoerkennungsdaten

Sie können die Risikoerkennungsdaten herunterladen, wenn Sie sie außerhalb des Azure-Portals verwenden möchten. Wenn Sie auf „Herunterladen“ klicken, wird eine CSV-Datei mit den neuesten 5.000 Datensätzen erstellt. 

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview-v2.md).
