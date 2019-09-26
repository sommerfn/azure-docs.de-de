---
title: 'Vordefinierter Domänenverweis: LUIS'
titleSuffix: Azure Cognitive Services
description: Referenz für die vordefinierten Domänen, die vordefinierte Sammlungen von Absichten und Entitäten aus LUIS (Language Understanding Intelligent Services) darstellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: f5810a813b6c54f190d95061e79914457f51d19c
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71067616"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Vordefinierter Domänenverweis für die LUIS-App
Diese Referenz enthält Informationen über die [vordefinierten Domänen](luis-how-to-use-prebuilt-domains.md), bei denen es sich um vordefinierte Sammlungen der von LUIS gebotenen Absichten und Entitäten handelt.

[Benutzerdefinierte Domänen](luis-how-to-start-new-app.md) weisen im Gegensatz dazu anfänglich keine Absichten und Modelle auf. Sie können einem benutzerdefinierten Modell beliebige Absichten und Entitäten aus einer vordefinierten Domäne hinzufügen.

# <a name="supported-domains-across-cultures"></a>Unterstützte Domänen in verschiedenen Kulturen

In der folgenden Tabelle sind die derzeit unterstützten Domänen zusammengefasst. Die Unterstützung für Englisch ist in der Regel vollständiger als für andere Sprachen. 

| Entitätstyp       | EN-US      | ZH-CN   | DE    | BV     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| [Calendar](#calendar)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Communication](#communication)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [E-Mail](#email)           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [HomeAutomation](#homeautomation)           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Hinweise](#notes)      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Places](#places)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [RestaurantReservation](#restaurantreservation)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [ToDo](#todo)     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Utilities](#utilities)          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Weather](#weather)        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Web](#web)    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Vordefinierte Domänen werden in den folgenden Sprachen **nicht unterstützt**:

* Französisch (Kanada)
* Hindi
* Spanisch (Mexiko)

# <a name="description-for-luis-prebuilt-domains"></a>Beschreibung für vordefinierte LUIS-Domänen
## <a name="calendar"></a>**Calendar**
Der Kalender enthält alle persönliche Besprechungen und Termine, aber KEINE öffentlichen Ereignisse (z. B. Zeitpläne für Weltmeisterschaften, Veranstaltungskalender für Seattle) oder generischen Kalender (z. B. den aktuellen Tag, den Herbstanfang oder den Tag der Arbeit).
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------------
 AcceptEventEntry | Akzeptieren eines Termins, einer Besprechung oder eines Ereignisses im Kalender | Akzeptieren eines Termins <br> Akzeptiere die Veranstaltung. <br> Akzeptiere die heutige Besprechung.
 Abbrechen | Abbrechen der laufenden Aktion durch den virtuellen Assistenten, z. B. die Erstellung einer Besprechung <br> _**Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Cancel“ im Calendar-Szenario. Wenn Sie einen allgemeinen Ausdruck für „Cancel“ benötigen, nutzen Sie die Absicht „Cancel“ in der Domäne **Utilities**._ | Es ist in Ordnung, sage die Veranstaltung einfach ab. <br> Nein, ich sage den Termin einfach ab.
 ChangeCalendarEntry | Ändern oder Neuplanen eines Kalendereintrags | Verlege den Termin morgen um 6:00 Uhr auf 14:00 Uhr. <br> Verlege den Arzttermin auf 17:00 Uhr. <br> Verlege das Mittagessen mit Jenny Olson auf Freitag. <br> Ändere die Veranstaltungszeit.
 CheckAvailability | Prüfen der Verfügbarkeit für einen Termin oder eine Besprechung im Kalender des Benutzers oder einer anderen Person | Wann ist Klaus für eine Besprechung verfügbar? <br> Zeige, wann Carol morgen verfügbar ist. <br> Ist Chris am Samstag frei?
 Confirm | Bestätigen eines Vorgangs, einer Aktion auf der Grundlage vorheriger Absichten <br> _**Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Confirm“ im Calendar-Szenario. Wenn Sie allgemeinere Ausdrücke für „Confirm“ benötigen, verwenden Sie die Absicht „Confirm“ in der Domäne **Utilities**._| Das ist richtig, erstelle das Meeting. <br> Ja, danke, stelle eine Verbindung mit der Besprechung her.
 ConnectToMeeting | Verbinden mit einer Besprechung | Verbinde mich mit dem 11:00-Uhr-Konferenzanruf mit Andy. <br> Akzeptieren des Aufrufs zur Budgetbesprechung
 ContactMeetingAttendees | Kontaktieren von Besprechungsteilnehmern | Benachrichtige die Besprechungsteilnehmer, dass ich etwas verspätet zum Meeting um 3:00 Uhr komme. <br> Benachrichtige die Kollegen, dass die 8-Uhr-Besprechungen auf 8:30 Uhr verschoben werden muss.
 CreateCalendarEntry | Hinzufügen eines neuen einmaligen Elements zum Kalender | Erstellen einer Besprechung zur Erörterung von Problemen <br> Erstelle eine Besprechung mit abc@microsoft.com.
 DeleteCalendarEntry | Anforderung der Löschung eines Kalendereintrags| Storniere meinen Termin mit Helga. <br> Lösche meine Besprechung um 9:00 Uhr. <br> Lösche meine Veranstaltung.
  FindCalendarEntry | Öffnen der Kalenderanwendung oder suchen eines Kalendereintrags | Suche den Termin für die Nachuntersuchung beim Zahnarzt. <br> Zeige meinen Kalender. <br> Was steht am Donnerstag in meinem Kalender?
 FindCalendarWhen | Überprüfen der Uhrzeit laut Zeitplan | Wann treffe ich mich mit Amber und Susan? <br> Wann ist der Brunch geplant? 
 FindCalendarWhere | Überprüfen des Orts für einen Termin | Wo finden meine Termine morgen statt? <br>Wo treffe ich mich mit Stacy und Michael morgen zum Essen?
  FindCalendarWho | Überprüfen der Teilnehmer am geplanten Termin | Ich möchte die bestätigten Teilnehmer an der Besprechung morgen um 2:00 Uhr wissen. <br> Wird Jim an der nächsten Besprechung für die Pflegekräfte teilnehmen?
 FindCalendarDetail | Überprüfen und Anzeigen von Details für einen Zeitplan | Ich benötige Details zur Besprechung, die ich mit meinem Kollegen Paul geplant habe.
 FindDuration | Überprüfen der Dauer | Wie viel Zeit habe ich, um Lebensmittel einzukaufen? <br> Wie viel Zeit habe ich für das Mittagessen?
 FindMeetingRoom | Suchen verfügbarer Besprechungsräume | Welche Besprechungsräume stehen mir zur Verfügung? <br> Finde einen neuen Besprechungsort.
 GoBack | Zurücknavigieren zum letzten Schritt oder Element  <br> _**Hinweis:** Weitere allgemeine Äußerungen zu „GoBack“ finden Sie in der Domäne **Utilities**._ | Letzter Eintrag <br> Zurück zur letzten E-Mail
 Reject | Ablehnen des Vorschlags des virtuellen Assistenten <br> _**Hinweis:** Weitere allgemeine Äußerungen zu „Reject“ finden Sie in der Domäne **Utilities**._ | Die Veranstaltung muss nicht eingetragen werden. <br> Zu diesem Zeitpunkt habe ich andere Dinge zu erledigen.
ShowNext | Überprüfen des nächsten Ereignisses <br> _**Hinweis:** Weitere allgemeine Äußerungen zu „ShowNext“ finden Sie in der Domäne **Utilities**._ | Zeige mit meine nächste Veranstaltung an. <br> Was steht als Nächstes in meinem Kalender?
 ShowPrevious | Überprüfen des letzten Ereignisses <br> _**Hinweis:** Weitere allgemeine Äußerungen zu „ShowPrevious“ finden Sie in der Domäne **Utilities**._ | Wie lautet der Zeitplan vorher?
 TimeRemaining | Überprüfen der verbleibenden Zeit bis zur nächsten Veranstaltung. | Zeige mir, wie viel Zeit ich vor meinen Besprechungen habe. <br> Zeige mir die Zeit, die ich vor Beginn der nächsten Besprechung habe.
 
### <a name="entities"></a>**Entitäten**
Entitätsname | Entitätstyp | BESCHREIBUNG | Beispiele | Slots
-------|-----------------------|-------------|---------|--------
ContactName | personName | Der Name eines Kontakts oder Besprechungsteilnehmers | Vereinbare ein Treffen mit **Betsy**. <br>  Trage ein Treffen mit **Aubrey** am 3. Juli um 19:00 Uhr ein. | Betsy <br> Aubrey <br> Amy 
DestinationCalendar | Einfach | Der Name des Zielkalenders | Mittagessen mit Mama, Dienstag um 12 **persönlich** <br> Verwende als Standardkalender meinen **Google**-Kalender. <br> Ändere den Yoga-Kurs auf Montag 15:00 Uhr und trage ihn in meinen **persönlichen** Kalender ein. | Google <br> Persönlich <br> business <br> Standard
Duration | datetime | Dauer einer Besprechung oder eines Termin oder die verbleibende Zeit | Füge meinem Arbeitskalender ein Meeting mit Gary hinzu. Ich möchte morgen um 11:00 Uhr für **20 Minuten** mit ihm die Details des Stipendiums besprechen. <br> Füge im Kalender ein Ereignis bei Subway am Freitag ein. Ich werde mit Thomas **eine Stunde** ab 21:00 Uhr essen. | eine Stunde <br> 2 Tage <br> 20 Minuten 
EndDate | datetime | Enddatum einer Besprechung oder eines Termins | Füge im Kalender ein Konzert in der Bass Hall vom 3. Mai bis **5. Mai** ein. | 5\. Mai  
EndTime | datetime | Beendigungszeit einer Besprechung oder eines Termins | Geht es von 14:30 Uhr bis **15:00** Uhr? | three 
Location | Einfach | Ort des Kalenderelements, der Besprechung oder des Termins  Adressen, Städte und Regionen sind gute Beispiele für Speicherorte. | Erstelle eine Besprechung in **Fremont** zum Tablet in Burma. <br> Pro-Bono-Meeting in **Edina** | Fitnesscenter Detmold <br> Pfannkuchenwelt <br> Garage 
MeetingRoom | Einfach | Raum einer Besprechung oder eines Termins | Füge im Arbeitskalender eine Besprechung mit Jake um 14:00 Uhr in seinem **Büro** an diesem Freitag ein. | sein Büro <br> Konferenzraum <br> Raum 2
MoveEarlierTimeSpan | datetime | Zeit, um die eine Besprechung oder ein Termin nach vorne verschoben werden soll | Verlege meinen Termin zum Mittagessen um **30 Minuten** nach vorne. | 30 Minuten <br> zwei Stunden 
MoveLaterTimeSpan |  datetime | Zeit, um die eine Besprechung oder ein Termin nach hinten verschoben werden soll | Verschiebe meine Besprechung mit Orchid Box um **4 Stunden**. | 4 Stunden <br> 15 Minuten 
OrderReference | Einfach | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt. | Was ist morgen mein nächster Termin? | Weiter
OriginalEndDate | datetime | Ursprüngliches Enddatum einer Besprechung oder eines Termins | Verlege mein Urlaubsende von **Freitag** auf Montag. | Freitag 
OriginalEndTime | datetime | Ursprüngliche Beendigungszeit einer Besprechung oder eines Termins | Verschiebe das Ende des Termins um **3** auf 4. | 3
OriginalStartDate | datetime | Ursprüngliches Startdatum einer Besprechung oder eines Termins | Verlege den Termin **morgen** um 10:00 Uhr auf Mittwoch um 9:00 Uhr.  | morgen 
OriginalStartTime | datetime | Ursprüngliche Startzeit einer Besprechung oder eines Termins | Verlege den Termin morgen von **10:00 Uhr** auf Mittwoch um 9:00 Uhr. | 10:00 Uhr
PositionReference | ordinal | Die absolute Position in einer Liste, die ein abzurufendes Element angibt | Das **zweite** | Sekunde <br> Nein. 3 <br> Nummer 5
RelationshipName | list | Der Beziehungsname eines Kontakts | Füge ein Mittagessen um 13:00 Uhr mit meiner **Frau** ein. | Frau <br> Ehemann <br> Schwester 
SlotAttribute | Einfach | Das Attribut, das abgefragt oder bearbeitet werden soll | Ändere den **Ort** der Veranstaltung. <br> Ändere die **Zeit** auf sieben Uhr. | location <br> time 
StartDate | datetime | Startdatum einer Besprechung oder eines Termins | Erstelle am **Mittwoch** um 16:00 Uhr eine Besprechung. | Wednesday 
StartTime | datetime | Startzeit einer Besprechung oder eines Termins | Erstelle am Mittwoch um **16:00 Uhr** eine Besprechung. | 16:00 Uhr
Subject | simple, pattern.Any | Der Betreff, z. B. der Titel einer Besprechung oder eines Termins | Zu welcher Uhrzeit ist die Besprechung zur **Partyvorbereitung**? | Zahnarzt <br> Mittagessen mit Jule 
`Message` | simple, pattern.Any | Die Nachricht, die an die Teilnehmer gesendet werden soll | Benachrichtige die Teilnehmer am Essen, dass **ich zu spät kommen werde**. | Ich verspäte mich.

## <a name="communication"></a>**Communication**
Zu diesem Entitätstyp gehören Anforderungen für Anrufe, das Senden von SMS- oder Sofortnachrichten, das Suchen und Hinzufügen von Kontakten sowie verschiedene andere kommunikationsbezogene Anforderungen (im Allgemeinen ausgehend). Äußerungen, die sich _ausschließlich auf den Kontaktnamen_ beziehen, gehören nicht in die Domäne „Communication“.

### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
AddContact | Hinzufügen eines neuen Kontakts zur Kontaktliste des Benutzers | Füge einen neuen Kontakt hinzu.  <br>   Speichere diese Nummer unter dem Namen „Jane“.
AddFlag | Hinzufügen eines Flags zu einer E-Mail | Markiere diese E-Mail. <br> Füge diese E-Mail ein Flag hinzu.
AddMore | Fügen Sie im Rahmen des schrittweisen Verfassens einer E-Mail oder eines Texts mehr hinzu. | Füge mehr Text ein.  <br>   Ergänze den E-Mail-Text.
Antwort | Beantworten eines eingehenden Telefonanrufs | Nimm den Anruf an.  <br>   Abheben
AssignContactNickname | Hinzufügen eines Spitznamens zu einem Kontakt | Ändere „Isaac“ in „Vati“.  <br>   Bearbeite Jims Spitznamen. <br>   Füge Patti Owens einen Spitznamen hinzu.
CallBack | Zurückrufen nach einem Telefonanruf | Rufe zurück.
CallVoiceMail | Verbinden mit der Voicemail des Benutzers | Verbinde mich mit meiner Voicemailbox. <br>Voicemail <br>   Rufe die Voicemail an.
Abbrechen | Abbrechen eines Vorgangs | Abbrechen <br>   Beenden
CheckIMStatus | Überprüfen des Status eines Kontakts im Sofortnachrichtendienst | Ist Klaus Onlinestatus auf „abwesend“ festgelegt? 
CheckMessages | Überprüfen auf neue Nachrichten oder E-Mails | Überprüfe mein Postfach. <br>  Habe ich neue E-Mail-Nachrichten?
Confirm | Bestätigen einer Aktion. | Ja, senden. <br> Ich bestätige, dass ich diese E-Mail senden möchte.
EndCall | Beenden eines Telefonanrufs | Beende den Anruf. <br> Auflegen
FindContact | Suchen von Kontaktinformationen anhand des Namens | Suche die Nummer meiner Mutter. <br>   Zeige mir Carols Nummer.
FinishTask | Beenden der aktuellen Aufgabe | Ich bin fertig. <br> Das ist alles.
TurnForwardingOff | Deaktivieren der Anrufweiterleitung | Meine Anrufe nicht mehr weiterleiten <br> Schalte die Anrufweiterleitung aus.
TurnForwardingOn | Einschalten der Anrufweiterleitung | Leite meine Anrufe an 3333 weiter. <br>  Schalte die Anrufweiterleitung an 3333 ein.
GetForwardingsStatus | Abrufen des aktuellen Status der Anrufweiterleitung | Ist meine Anrufweiterleitung aktiviert? <br>   Sage mir, ob meine Anrufweiterleitung an oder aus ist.
GetNotifications | Abrufen von Benachrichtigungen | Öffne meine Benachrichtigungen. <br>   Überprüfe die Facebook-Benachrichtigungen auf meinem Telefon.
GoBack | Zurück zur vorherigen Seite wechseln | Gehe zu Twitter zurück. <br>   Gehe einen Schritt zurück. <br>   Zurück navigieren
Ignorieren | Ignorieren eines eingehenden Anrufs | Nicht beantworten <br>   Anruf ignorieren
IgnoreWithMessage | Ignorieren eines eingehenden Anrufs, stattdessen mit Text antworten. | Beantworte diesen Anruf nicht, sende stattdessen eine Textnachricht. <br>   Ignorieren und Text zurückschicken
Dial | Tätigen eines Telefonanrufs. | Klaus anrufen <br>   Bitte wähle 311.
FindSpeedDial | Suchen der Kurzwahl, die für eine Telefonnummer festgelegt ist und umgekehrt. | Was ist meine Kurzwahl 5? <br>   Habe ich eine Kurzwahl festgelegt? <br>   Was ist die Kurzwahl für 94 15 55 53 33?
Weiter | E-Mail weiterleiten | Leite diese E-Mail an Greg weiter.
ReadAloud | Lautes Vorlesen einer Nachricht oder E-Mail für den Benutzer. | Lies Text. <br>   Was hat sie in der Nachricht gesagt?
PressKey | Drücken einer Taste oder Ziffer auf der Zehnertastatur. | Wähle die Sterntaste. <br>   Drücke 1 2 3.
QueryLastText | Abfragen der letzten SMS oder Nachricht | Wer hat mir getextet? <br>   Von wem habe ich kürzlich Nachrichten erhalten?
Redial | Erneut wählen oder Nummer noch einmal wählen | Erneut wählen <br>   Wähle meinen letzten Anrufpartner noch einmal an.
Reject | Abweisen eines eingehenden Anrufs. | Aufruf abweisen <br>   Ich kann jetzt nicht antworten. <br>   Ich bin im Moment nicht zu sprechen und komme später wieder.
Reply | Beantworten einer Nachricht | Antwort auf Lore Hound <br>   Beantworten durch Eingabe von „Ich bin unterwegs“
SearchMessages | Suchen von Nachrichten nach bestimmten Bedingungen | Durchsuche meine E-Mails nach Nachrichten von Jane.
SendEmail | Senden von E-Mail. Diese Absicht betrifft E-Mail, aber keine Textnachrichten. | Email to Mike Waters: Mike, that dinner last week was splendid (E-Mail an Mike Waters: Mike, das Essen letzte Woche war hervorragend) <br>   Sende eine E-Mail an Bob.
SendMessage | Senden einer Textnachricht oder Chatnachricht | Sende diesen Text an Chris und Helga. <br>   Facebook-Nachricht an Greg <br>   
SetSpeedDial | Festlegen einer Kurzwahl für die Telefonnummer eines Kontakts | Lege Kurzwahl eins für Helga fest. <br>   Richte Kurzwahl für Mami ein.
ShowCurrentNotification | Anzeigen der aktuellen Benachrichtigungen | Gibt es neue Benachrichtigungen? <br> Zeige mir eine Benachrichtigung.
ShowNext | Anzeigen des nächsten Elements beispielsweise in einer Liste von Textnachrichten oder E-Mails | Nächstes anzeigen <br>   Gehe zur nächsten Seite.
ShowPrevious | Anzeigen des vorherigen Elements, beispielsweise in einer Liste mit Textnachrichten oder E-Mails | Zeige den letzten an. <br>   Vorherige <br>   Gehe zum Vorherigen.
TurnSpeakerOff | Freisprecheinrichtung ausschalten | Lautsprecher ausschalten <br>   Schalte den Lautsprecher aus.
TurnSpeakerOn | Schalte den Lautsprecher ein. | Freisprechmodus <br>   Schalte den Lautsprecher ein.

### <a name="entities"></a>**Entitäten**
Entitätsname | Entitätstyp | BESCHREIBUNG | Beispiele | Slots
------|-------|----------|--------------|---------------
Attachment | Einfach | Die Anlage, die per Text oder E-Mail gesendet werden soll | Sende eine **Datei** per E-Mail aus OneNote. <br> Sende die **DOC**-Datei mit meinem Haushaltsplan an Katie. | file <br> DOC
AudioDeviceType | Einfach | Typ des Audiogeräts (Lautsprecher, Headset, Mikrofon usw.) | Antworten mit **Freisprechanlage** <br> Erneut mit **Telefonlautsprecher** wählen | Lautsprecher <br> Freisprechanlage <br> Bluetooth
Category (Kategorie) | Einfach | Die Kategorie einer Nachricht oder E-Mail, diese muss eine klare Definition im E-Mail-System haben, z. B. „ungelesen“, „Flag“. Beschreibungen ohne eine eindeutige Definition, z. B. „Neu“ oder „Letzte“, sind keine Kategorien. | Markiere alle E-Mails als **gelesen**.  <br> Ändere die E-Mail an Paul in **hohe Priorität**. | Wichtig <br> Hohe Priorität <br> read
ContactAttribute | Einfach | Ein Attribut des Kontakts, den der Benutzer abfragt| Stehen im nächsten Monat **Geburtstage** an, die ich wissen sollte? | Geburtstage <br> address <br> Telefonnummer
ContactName | personName  | Der Name eines Kontakts oder Nachrichtenempfängers | Sende eine E-Mail an **Stevens**. | Stevens
Date/Time | datetime | Der DateTime-Wert einer empfangenen E-Mail | E-Mails von **heute** lesen <br> Wer hat mir **heute** eine E-Mail gesendet? <br> Wer hat mich um **19:00 Uhr** angerufen? | heute <br> morgen
DestinationPhone | Einfach | Das Ziel für eine Nachricht oder einen Anruf | Rufe **zu Hause** an. <br> Sende eine Textnachricht an die **Familie**. | zu Hause <br> home
EmailAddress | email | Die E-Mail-Adresse, an die etwas gesendet oder die abgefragt werden soll | Sende eine E-Mail an Megan.Flynn@MKF.com.<br> abc@outlook.com 
EmailSubject | simple, pattern.Any | Der Text, der als Betreffzeile für eine E-Mail-Nachricht verwendet wird | Verfasse eine E-Mail an David mit dem Betreff **Hallo**.  | AW: Interessante Geschichte
Schlüssel | Einfach | Die zu drückende Taste | Drücke die **Leertaste**. <br> Drücke **9**. | Nummernzeichen <br> Sternchen <br> 8
Zeile | Einfach | Der Kanal, der zum Senden einer E-Mail oder einer Textnachricht verwendet werden soll | Lies meine letzte E-Mail an mein **Hotmail**-Konto. <br> Rufe Peter **mobil** an. <br> Rufe Papa über meine **Dienstrufnummer** an.| Hotmail <br> Skype <br> D-Netz
SenderName | personName | Der Name des Absenders. | Lies die E-Mail von **David** vor. <br> E-Mails von Chanda | David <br> Chanda
FromRelationshipName | Einfach | Der Beziehungsname des Absenders | Lies die Nachricht von **Papa** vor. <br> Lies bitte alle Textnachrichten von **Mama** vor. | Papa <br> Mama 
`Message` | simple, pattern.Any |  Die Nachricht, die als E-Mail oder SMS gesendet werden soll.  | Sende eine E-Mail mit dem Inhalt **Ich bin beschäftigt**. | Ich bin beschäftigt
OrderReference | Einfach | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt. | Was war die **letzte** von mir gesendete Nachricht? <br> Lies die **neueste** Nokia-E-Mail vor. <br> Lies die **neuen** Textnachrichten vor. | last <br> latest <br> letzte <br> neueste
PositionReference | simple, ordinal | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt.| Was war die **erste** von mir gesendete Nachricht? <br> Die **dritte**| First (Erster) <br> dritte
phoneNumber | phoneNumber | Die Telefonnummer für eine Nachricht oder einen Anruf | Sende eine Textnachricht an **vier eins fünf sechs acht vier fünf zwei acht vier**. | 3525214446
RelationshipName | Einfach | Der Beziehungsname eines Kontakts oder Nachrichtenempfängers | Sende eine E-Mail an meine **Frau**. | Frau
SearchTexts | simple, pattern.any | Die zum Filtern von E-Mails oder Nachrichten verwendeten Texte | Suche alle E-Mails, die **Surface Pro** enthalten. | Surface Pro
SpeedDial | Einfach | Die Kurzwahl | Rufe **drei vier fünf** an. <br> Lege als Kurzwahl **eins** fest. | 345 <br> 5

## <a name="email"></a>**E-Mail**
„Email“ ist eine Unterdomäne der Domäne *Communication*. Sie enthält hauptsächlich Anforderungen zum Senden und Empfangen von Nachrichten über E-Mails.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
AddMore | Fügen Sie im Rahmen des schrittweisen Verfassens einer E-Mail oder eines Texts mehr hinzu. | Ergänze den E-Mail-Text.
Abbrechen | Abbrechen eines Vorgangs <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „Cancel“ finden Sie in der Domäne **Utilities**.* | Abbrechen Nicht senden <br> Beenden
CheckMessages | Überprüfen auf neue Nachrichten/E-Mails ohne bedingte Abfrage. Wenn eine Bedingung vorliegt, gehören die Äußerungen zur Absicht *SearchMessage*. | Überprüfe mein Postfach. <br> Habe ich neue E-Mail-Nachrichten? 
Confirm | Bestätigen einer laufenden Aktion im Zusammenhang mit der Verarbeitung von E-Mails <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „Confirm“ finden Sie in der Domäne **Utilities**.* | Ja, senden. <br> Ich bestätige, dass ich diese E-Mail senden möchte.
Löschen | Löschen einer E-Mail oder der nach bestimmten Bedingungen gefilterten E-Mails | Verschiebe die E-Mail in den Papierkorb. <br> Leere mein Postfach. <br> Entferne die E-Mail von Mary.
ReadAloud | Lautes Vorlesen einer Nachricht oder E-Mail für den Benutzer. <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „ReadAloud“ finden Sie in der Domäne **Utilities**.*  | Lies die E-Mail von Mary vor.
Reply | Antworten auf eine Nachricht an die aktuelle E-Mail-Adresse | Erstelle eine Antwort-E-Mail. <br> Antworte durch Eingabe von „Vielen Dank. Mit freundlichen Grüßen John“.
SearchMessages | Suchen von Nachrichten nach bestimmten Bedingungen, einschließlich Absendername, Uhrzeit und Betreff | Zeige mir die Nachrichten von Nisheen. <br> Durchsuche meine E-Mails nach Nachrichten mit dem Titel „ABC“.
SendEmail | Senden von E-Mail. | E-Mail an Mike: Mike, that dinner last week was splendid (E-Mail an Mike Waters: Mike, das Essen letzte Woche war hervorragend) <br> Sende eine E-Mail an Bob.
ShowNext | Anzeigen der nächsten Elemente in einer Liste von Textnachrichten oder E-Mails <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „ShowNext“ finden Sie in der Domäne **Utilities**.* | Nächstes anzeigen <br> Gehe zur nächsten Seite.
ShowPrevious | Anzeigen der vorherigen Elemente in einer Liste von Textnachrichten oder E-Mails <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „ShowPrevious“ finden Sie in der Domäne **Utilities**.* | Zeige den letzten an. <br> Vorherige <br> Gehe zum Vorherigen.
Weiter | Weiterleiten einer E-Mail | Leite diese E-Mail an Greg weiter.
AddFlag | Hinzufügen eines Flags zu einer E-Mail | Markiere diese E-Mail. <br> Füge diese E-Mail ein Flag hinzu.
QueryLastText | Abfragen der letzten E-Mail | Wer hat mir eine E-Mail gesendet? <br> Wer habe mir vor Kurzem eine E-Mail geschickt?


### <a name="entities"></a>**Entitäten**
Entitätsname | Entitätstyp | BESCHREIBUNG | Beispiele | Slots
------|-------|----------|--------------|---------------
Attachment | Einfach | Die Anlage, die per Text oder E-Mail gesendet werden soll | Sende eine **Datei** per E-Mail aus OneNote. <br> Sende die **DOC**-Datei mit meinem Haushaltsplan an Katie. | file <br> DOC
ContactName | personName  | Der Name eines Kontakts oder Nachrichtenempfängers | Sende eine E-Mail an **Stevens**. | Stevens
Date | datetime | Das Datum einer empfangenen E-Mail | E-Mails von **heute** lesen <br> Wer hat mir **heute** eine E-Mail gesendet? | heute
EmailAddress | email | Die E-Mail-Adresse, an die etwas gesendet oder die abgefragt werden soll | Sende eine E-Mail an Megan.Flynn@MKF.com.<br> abc@outlook.com 
EmailSubject | simple, pattern.Any | Der Text, der als Betreffzeile für eine E-Mail-Nachricht verwendet wird | Verfasse eine E-Mail an David mit dem Betreff **Hallo**.  | AW: Interessante Geschichte
SenderName | personName | Der Name des Absenders. | Lies die E-Mail von **David** vor. <br> E-Mails von Chanda | David <br> Chanda
FromRelationshipName | Einfach | Der Beziehungsname des Absenders | Lies die Nachricht von **Papa** vor. <br> Lies bitte alle Textnachrichten von **Mama** vor. | Papa <br> Mama 
`Message` | simple, pattern.Any |  Die Nachricht, die als E-Mail oder SMS gesendet werden soll.  | Sende eine E-Mail mit dem Inhalt **Ich bin beschäftigt**. | Ich bin beschäftigt
Category (Kategorie) | Einfach | Die Kategorie einer Nachricht oder E-Mail, diese muss eine klare Definition im E-Mail-System haben, z. B. „ungelesen“, „Flag“. Beschreibungen ohne eine eindeutige Definition, z. B. „Neu“ oder „Letzte“, sind keine Kategorien. | Markiere alle E-Mails als **gelesen**.  <br> Ändere die E-Mail an Paul in **hohe Priorität**. | Wichtig <br> Hohe Priorität <br> read
OrderReference | Einfach | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt. | Was war die **letzte** von mir gesendete Nachricht? <br> Lies die **neueste** Nokia-E-Mail vor. <br> Lies die **neuen** Textnachrichten vor. | last <br> latest <br> letzte <br> neueste
PositionReference | simple, ordinal | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt.| Was war die **erste** von mir gesendete Nachricht? <br> Die **dritte**| First (Erster) <br> dritte
RelationshipName | Einfach | Der Beziehungsname eines Kontakts oder Nachrichtenempfängers | Sende eine E-Mail an meine **Frau**. | Frau
Time | datetime | Time | Sende die E-Mail **abends**. | abends
SearchTexts | simple, pattern.any | Die zum Filtern von E-Mails oder Nachrichten verwendeten Texte | Suche alle E-Mails, die **Surface Pro** enthalten. | Surface Pro 
Zeile | Einfach | Der Kanal, der zum Senden einer E-Mail verwendet werden soll | Lies meine letzte E-Mail an mein **Hotmail**-Konto. <br> Sende eine E-Mail von meinem **Geschäftskonto**.| Hotmail <br> Geschäftskonto 

## <a name="homeautomation"></a>**HomeAutomation**
Die HomeAutomation-Domäne stellt Absichten und Entitäten zum Steuern von Geräten im Smarthome bereit. Sie unterstützt hauptsächlich Steuerungsbefehle für die Beleuchtung und die Klimaanlage. Darüber hinaus bietet sie jedoch einige Möglichkeiten zur Generalisierung, um sie auch für andere Geräte zu verwenden.
### <a name="supported-devices-and-properties"></a>**Unterstützte Geräte und Eigenschaften**
Gerät | Properties
-------|---------
Temperatursensor | Temperatur
Beleuchtung, Lampe | Ein/Aus, Helligkeit, Farbe
TV, Radio | Ein/Aus, Lautstärke
Klimaanlage (A/C), Thermostat | Ein/Aus, Temperatur, Stromversorgung
Lüfter | Ein/Aus, Stromversorgung
Steckdosen, DVD-Player, Eismaschinen usw. | Ein/Aus

### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
 TurnOff | Der Benutzer möchte Geräte ausschalten oder Einstellungen deaktivieren.  | Mach das Licht aus. <br> Schalte „etwas“ aus. <br> „Etwas“ ausschalten
 TurnOn | Einschalten eines Geräts oder Einschalten und Versetzen des Geräts in eine bestimmte Einstellung oder einen bestimmten Modus | Stelle die Beleuchtung auf 50 % ein. <br> Schalte die Kaffeemaschine ein. <br> Kannst du die Kaffeemaschine einschalten?
 SetDevice | Versetzen des Geräts in eine bestimmte Einstellung oder einen bestimmten Modus  | Stelle die Klimaanlage auf 26 Grad ein. <br> Schalte die Lichter auf Blau um. <br> Benenne dieses Licht mit Nachtlicht.
 QueryState | Abfragen des Status eines Geräts oder einer Einstellung  | Auf welche Stufe ist das Thermostat eingestellt? <br> Ist die Klimaanlage eingeschaltet? <br> Wie ist die Temperatur im Schlafzimmer?
 TurnUp | Erhöhen von Einstellungen oder der Helligkeit von Geräten | Mache die Beleuchtung um 75 Prozent heller. <br> Erhöhe die Helligkeit hier um 10 Prozent.  <br> Mache das Wohnzimmer wärmer.
 TurnDown | Verringern, ABER NICHT Ausschalten eines Geräts, durch Dimmen oder Senken der Temperatur oder Helligkeit der Beleuchtung | Mache die Bibliothek um 44 % dunkler. <br> Dimme das Licht. <br> Mach den Raum kühler.
### <a name="entities"></a>**Entitäten**
Entitätsname | Entitätstyp | BESCHREIBUNG | Beispiele
-------|----------|--------------|---------------
DeviceName | List | Benutzerdefinierter Text für die Geräte | Blau<br> Weihnachten <br> Schreibtisch
DeviceType | List | Unterstützte Geräte | Leuchten <br> Klimaanlage <br> Nachtlicht
Location | Einfach | Der Ort oder Raum, in dem sich das Gerät befindet | Küche<br> Erdgeschoss <br> Schlafzimmer
NumericalChange | Einfach | Der Betrag, um den eine Einstellung angehoben oder verringert wird <br> <br> _Der Slot gilt nur für die Absichten „turn_up“ und „turn_down“._ | 3<br> 50%<br>
OrderReference | ordinal | Mit diesem Slot wird die Auswahl der Elemente erfasst. Er gibt die Position des Elements in einer Liste an. | First (Erster)<br> Sekunde
Quantifier | List | Der Quantifizierer gibt an, auf wie viele Instanzen einer bestimmten Entität verwiesen wird. Beispiele: „alle“, „jedes“ usw. | Alle<br> Jede<br> Alles
Einstellung | Einfach | Die Einstellung, auf die der Benutzer sein Gerät festlegen möchte, einschließlich Szene, Stufe, Intensität, Farbe, Modus, Temperatur und Status des Geräts | Blau<br> 72 <br> Hitze 
SettingType | List | Die gewünschte Geräteeinstellung | Temperatur<br> 
Time/Date |  datetime | Zeit und Dauer des Betriebs eines Geräts| 5 Minuten <br> 15:00 Uhr
Unit | List | Zum Markieren von Wörtern wie „Grad“, „Prozent“, „Fahrenheit“ oder „Celsius“ sollte jeder Einheitenbegriff separat markiert werden. | Grad<br> Percent


## <a name="notes"></a>**Hinweise**
Die Domäne „Notes“ ermöglicht das Erstellen von Notizen und das Schreiben von Elementen für Benutzer.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
AddToNote | Hinzufügen von Informationen zu einer geöffneten Notiz | Füge meiner Planung einen Hinweis hinzu, dass ich meinen Chef wegen des Projekts kontaktieren muss.
Clear | Löschen aller Elemente in einer bereits vorhandenen Notiz. | Entferne alle Elemente aus meiner Urlaubsnotiz. <br>Lösche alle Einträge in meiner Lesenotiz.
Confirm | Bestätigen einer Aktion im Zusammenhang mit einer Notiz. <br> ***Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Confirm“ im Notes-Szenario. Wenn Sie allgemeinere Ausdrücke für „Confirm“ benötigen, verwenden Sie die Absicht „Confirm“ in der Domäne **Utilities**.* | Diese Notiz ist für mich in Ordnung. <br>Ich bestätige, dass alle Elemente in Listen erhalten bleiben sollen.
Erstellen | Erstellen einer neuen Notiz. | Erstelle eine Notiz. <br>Notiz zur Erinnerung, dass Jason in der ersten Maiwoche in der Stadt ist 
Löschen | Löschen einer gesamten Notiz. | Lösche meine Urlaubsnotiz. <br>Lösche meine Einkaufsnotiz.
ReadAloud | Lautes Vorlesen einer Notiz | Lies mir die erste Notiz vor. <br>Lies mir die Details vor.
Close (Schließen) | Schließe die aktuelle Notiz. | Schließe die Notiz. <br>Schließe die aktuelle Notiz.
Öffnen | Öffnen einer bereits vorhandenen Notiz | Öffne meine Berufungsnotiz. <br>Öffne die Notiz „Planung“.
RemoveSentence | Entfernen eines Satzes aus einer Notiz | Entferne den letzten Satz. <br>Lösche Chips aus meiner Einkaufsnotiz. <br>Entferne Stifte aus meiner Schulbedarfsnotiz.
ChangeTitle | Ändern des Titels der Notiz | Bezeichne diese Notiz mit „Planung“.

### <a name="entities"></a>**Entitäten**
Entitätsname | Entitätstyp | BESCHREIBUNG | Beispiele 
------- | ------- | ------- | -------
Text | simple, pattern.Any | Der Text einer Notiz oder Erinnerung. | Dehnen vor dem Lauf <br> Morgen Langlauf
Titel | simple, pattern.Any | Der Titel einer Notiz. | Lebensmittel <br> Anzurufende Personen <br> Aufgabe
CreationDate | datetimeV2 | Dieser Slot gilt für den Fall, dass Benutzer nach Notizen fragen, die innerhalb eines bestimmten Datums-/Zeitfensters erstellt wurden. | 
Quantifier | List | Wenn ein Benutzer eine Aktion für „alle“ Elemente, „jedes“ Element oder „beliebige“ Elemente oder den gesamten Text in einer Notiz anfordert | alle <br> beliebig <br> jede
OrderReference | ordinal | Ausführen von Aktionen mit den „ersten“, „letzten“, „nächsten“ usw. Elementen | first <br> last


## <a name="places"></a>**Places**
„Places“ umfasst Geschäfte, Institutionen, Restaurants, öffentliche Räume und Adressen. Die Domäne unterstützt das Suchen von Orten und Abfragen von Informationen zu einem öffentlichen Ort wie dem Standort, den Öffnungszeiten oder der Entfernung. 
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
MakeCall | Tätigen eines Telefonanrufs an einen Ort. | Applebees auf Rojas St 1000/200 und anrufen
FindPlace | Suche nach einem Ort (Unternehmen, Institution, Restaurant, öffentlicher Ort, Adresse). Aber nicht: <li> Nur Unternehmensname: Starbucks <li> Nur Standortname: Seattle/Norwegen <li> Nur Küche, Essen oder Produkt: Pizza/Guacamole/Italienisch | Wo ist die nächste Bibliothek? <br> Starbucks in Seattle <br> Wo ist die nächste Bibliothek? <br> 
GetAddress | Fragen nach der Adresse eines Orts. | Zeige mir die Adresse von Guu in der Robson Street. <br> Wie lautet die Adresse des nächsten Starbucks?
GetDistance | Abfragen der Entfernung vom aktuellen Standort zu einem bestimmten Ort | Wie weit ist es bis zum Holiday Inn?<br> Wie weit ist es von hier bis zum Bellevue Square? <br> Wie ist die Entfernung nach Tahoe?
GetPhoneNumber | Frage nach der Telefonnummer für einen Ort | Wie lautet die Telefonnummer des nächsten Starbucks?<br> Gib mir die Nummer von Home Depot. <br> Eine Telefonnummer für Disneyland
GetPriceRange | Abfragen des Bereichs des Pro-Kopf-Verbrauchs für einen Ort | Durchschnittlicher Preis von J.Sushi in Seattle <br> Kostet das Cineplex Mittwoch die Hälfte? <br> Wieviel kostet ein Steak bei Vapiano?
GetStarRating | Frage nach der Bewertung in Sternen für einen Ort. | Wie wird Zucca bewertet?<br> Wie viele Sterne hat die französische Wäscherei?<br> Ist das Aquarium in Duisburg gut?
GetHours | Frage nach den Geschäftszeiten eines Orts. | Um wie viel Uhr schließt Rewe?<br> Welche Öffnungszeiten hat Bauhaus?<br> Ist Starbucks noch geöffnet?
GetReviews | Fragen nach Rezensionen zu einem Ort. | Zeige mir Rezensionen der Cheesecake Factory. <br> Lies die Rezensionen von Cineplex vor. <br> Gibt es aktuelle Bewertungen für Humperdinks?
GetMenu | Frage nach der Speisekarte für ein Restaurant. | Bietet Zucca irgendetwas Veganes an? <br> Was ist bei Sizzler auf der Speisekarte? <br> Zeige mir die Karte von Applebee.
RatePlace | Bewerten eines Orts | 4-Sterne-Bewertung für Maxi‘s Pizza in Kimberly <br> Gib Bonefish bei TripAdvisor 4 Sterne. <br> Erstelle eine 4-Sterne-Bewertung für La Hacienda.
AddFavoritePlace | Hinzufügen eines Orts zu den Favoriten oder der MVP-Liste | Speichere diesen Ort in meinen Favoriten. <br> Füge Best Buy meinen Favoriten hinzu.

### <a name="entities"></a>**Entitäten**
LUIS-Entitäten | Entitätstyp | BESCHREIBUNG | Beispiele | Beispiele für Äußerungen
--------------|-------------|-------------|----------|-------------------
AbsoluteLocation | Einfach | Der Ort oder die Adresse eines Orts. | Garmisch-Partenkirchen <br> Südstr. 12 <br> Seattle | **1020 Middlefield Rd.** in **Palo Alto** <br> Geschäfte für Vogelfutter in **Seattle** <br> Zeige mir die Entfernung zwischen hier und **112th Ave SE 300**.
Amenities | Einfach | Die objektiven Merkmale und Vorzüge eines öffentlichen Orts | Am Ufer <br> Kostenloses Parken | Fischrestaurants in Kirkland **am Wasser** <br> Gibt es in meiner Nähe einen **kostenlosen Parkplatz**?
Cuisine | Einfach | Die Art des Speisenangebots, der Kochstil oder die Nationalküche | Chinesisch <br> Italienisch <br> Sushi <br> Nudeln <br> | Hilf mir bei der Suche nach einem **chinesischen** Restaurant. <br> Welche Öffnungszeiten hat das **Sushi**-Restaurant? <br> Wo ist das nächstgelegene **Steak**-Haus?
Date | datetime | Datum und Uhrzeit oder Dauer bis Zielort | morgen <br> heute <br> 18:00 Uhr | Wann schließt das Aquarium **morgen**? <br> Der nächstgelegene Fahrradladen, der nach **18 Uhr** noch geöffnet hat
Distance | Dimension | Der Abstand zu einem öffentlichen Ort vom aktuellen Standort des Benutzers | 15 Kilometer <br> 10 Kilometer | Bekleidungsgeschäft innerhalb von **15 Kilometern** <br> Ein Restaurant für Kinder, das maximal **10 Kilometer** entfernt ist
MealType | List | Art der Mahlzeit, wie Frühstück oder Mittagessen. | Frühstück <br> Abendessen | Suche **Frühstück** in Greenwood Seattle. <br> Suche mir einen Ort für das **Mittagessen**.
In der Nähe | List | Beschreiben eines nahe gelegenen Orts ohne absolute Position oder Adresse | nächste <br> in diesem Gebiet <br> in meiner Umgebung | Suche mir das **nächstgelegene** indische Restaurant. <br> Wo befindet sich mein **lokaler** Wetherspoon? <br> Gibt es **in meiner Umgebung** gute Restaurants?
OpenStatus | List | Anzeige, ob ein Ort offen oder geschlossen ist. | öffnen <br> closed | Wann **schließt** Yogurt Land heute? <br> Was sind die **Öffnungszeiten** von Costco?
PlaceName | Einfach | Der Name eines Ziels, der ein Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt. Der Name des Orts kann einen Ortstyp enthalten, wenn dieser häufig verwendet wird. | Palmengarten <br> Rewe <br> Edeka| Um welche Uhrzeit öffnet die **Safeway**-Apotheke? <br> Ist **Walmart** offen?
PlaceType | Einfach | Der Typ eines Ziels, der ein ortsansässiges Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt. | Restaurant <br> Oper <br> Kino | **Kinos** in Cambridge <br> Gibt es ein **Restaurant** in meiner Umgebung?
PriceRange | Einfach | Der Preisbereich der Produkte oder Dienste des Orts | billig <br> kostengünstig <br> teuer | Suche eine **günstige** Reparaturwerkstatt. <br> Welche **billige** Pizzeria ist jetzt geöffnet?
Produkt | Einfach | Das an einem Ort angebotene Produkt. | Bekleidung <br> Fernseher | Wo gibt es das beste **Essen**? <br> Suche in East Kilbride nach **Fernsehern**.
Rating | Einfach | Die Bewertung eines Orts. | 5 Sterne <br> top <br> gut | Gibt es **gute** Orte, an denen ich morgen essen gehen kann? <br> **Beste** Restaurants in Amsterdam <br> Liste die drei **Top**-Pizzerien auf.


## <a name="restaurantreservation"></a>**RestaurantReservation**
Die Domäne „RestaurantReservation“ unterstützt Absichten für die Behandlung von Reservierungen für Restaurants.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
Reserve | Anfordern einer Reservierung für ein Restaurant | Reserviere bei Zucca einen Tisch für zwei für heute Abend. <br> Buche einen Tisch für morgen. <br> Tisch für 3 in Palo Alto um sieben <br> Reserviere für 3 Personen bei Red Lobster.
DeleteReservation | Stornieren oder Löschen einer Reservierung für ein Restaurant | Storniere die Reservierung bei Zucca morgen Abend. <br> Vergiss meine Reservierung für Red Lobster um 19:00 Uhr. Nächster Freitag <br> Ich brauche keine Reservierung für Zucca, storniere sie.
ChangeReservation | Ändern von Uhrzeit, Ort oder Anzahl der Personen für eine vorhandene Restaurantreservierung | Ändere meine Reservierung auf 21:00 Uhr. <br> Ändere meine Reservierung von Zucca in Red Lobster! <br> 7 Personen anstatt 6 für die Reservierung bei Zucca
Confirm | Bestätigen einer Aktion im Zusammenhang mit einer Reservierung <br> ***Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Confirm“ im RestaurantReservation-Szenario. Wenn Sie allgemeinere Ausdrücke für „Confirm“ benötigen, verwenden Sie die Absicht „Confirm“ in der Domäne **Utilities**.* | Ja, ich habe eine Reservierung für heute Abend um 8 bei Pasta Maker gemacht. <br> Ja, einfach buchen <br> Bestätige die Reservierung in der Sushi-Bar.
FindReservationDetail | Anzeigen detaillierter Informationen zu einer vorhandenen Reservierung | Suche meine Reservierung für das Renaissance San Diego. <br> Zeige mir die Reservierung für morgen an. <br> Zeige Details zu meiner Reservierung für Zucca an.
FindReservationWhere | Überprüfen des absoluten Standorts der Reservierung | Wo ist der Standort von Zucca in meiner Reservierung? <br> Zeige den Ort meiner Reservierung morgen an.
FindReservationWhen | Überprüfen der genauen Uhrzeit einer Reservierung | Wann ist die Reservierung für Zucca für morgen? <br> Die Uhrzeit meiner Reservierung bei Red Lobster
Reject | Ablehnen des Vorschlags des virtuellen Assistenten zur Verwaltung einer Reservierung <br> ***Hinweis:** Weitere allgemeine Äußerungen zu „Reject“ finden Sie in der Domäne **Utilities**.* | Die Veranstaltung muss nicht eingetragen werden. | Nein, ich möchte die Reservierung nicht ändern. <br> Nein, nicht buchen, ich habe einen Fehler gemacht.

### <a name="entities"></a>**Entitäten**
LUIS-Entität | Entitätstyp | BESCHREIBUNG | Beispiele
-------|------|---------|-------------------
Adresse | Einfach | Ein Veranstaltungsort oder die Adresse für eine Reservierung | Garmisch-Partenkirchen<br>Südstr. 12<br>Seattle
Atmosphere | List | Eine Beschreibung der Atmosphäre eines Restaurants | romantisch<br>ungezwungen<br>gut für Gruppen geeignet<br>sehr schön
Cuisine | Einfach | Die Art des Speisenangebots, der Kochstil oder die Nationalküche | Chinesisch<br>Italienisch<br>Mexikanisch<br>Sushi<br>Nudeln<br>Steak
MealType | List | Die Art der Mahlzeit, auf die sich eine Reservierung bezieht. | Frühstück<br>Abendessen<br>Mittagessen<br>Imbiss
PlaceName | Einfach | Der Name eines Restaurants | Zucca<br>Käsekuchenfabrik<br>Red Lobster
Rating | Einfach | Die Bewertung eines Orts oder eines Restaurants. | 5 Sterne<br>3 Sterne<br>4 Sterne
NumberPeople | Einfach | Anzahl der Personen für eine Reservierung | 3<br>sechs
Time | datetime| Die Zeit einer Restaurantreservierung | morgen<br>abends<br>19:00 Uhr<br>Weihnachten


## <a name="todo"></a>**ToDo**
Die Domäne _ToDo_ stellt Typen von Aufgabenlisten für Benutzer bereit, damit sie Aufgaben hinzufügen, markieren und löschen können.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
AddToDo | Hinzufügen von Aufgabenelementen für einen beliebigen Listentyp |  Erinnere mich daran, Milch zu kaufen. <br> Füge einen Eintrag mit dem Namen „Milch kaufen“ hinzu.
Confirm | Bestätigen einer laufenden Aktion. Die Äußerung enthält ein explizites Signal wie „Ja“ zur Bestätigung eines Vorgangs. <br> <br> ***Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Confirm“ im ToDo-Szenario. Wenn Sie allgemeinere Ausdrücke für „Confirm“ benötigen, verwenden Sie die Absicht „Confirm“ in der Domäne **Utilities**.* | Lösche diese Aufgabe. <br> Ich bin sicher, dass ich diese Aufgabe hinzufügen möchte. <br> Ja, ich möchte dies tun.
Abbrechen | Abbrechen einer laufenden Aktion  <br> <br> ***Hinweis:** Diese Absicht umfasst hauptsächlich die Aktion „Confirm“ im RestaurantReservation-Szenario. Wenn Sie allgemeinere Ausdrücke für „Confirm“ benötigen, verwenden Sie die Absicht „Confirm“ in der Domäne **Utilities**.* | Nein, vergiss es. <br> Brich die Aufgabe einfach ab. <br> Nein, nicht hinzufügen
DeleteToDo | Löschen eines Elements in der Aufgabenliste durch Verweis auf seine Position oder Löschen aller Aufgabenelemente | Entferne alle Aufgaben. <br> Hilf mir, die zweite zu löschen.
MarkToDo | Markieren einer Aufgabe als abgeschlossen oder fertig durch Verweis auf die Position in der Liste oder den Inhalt der Aufgabe | Markiere die Aufgabe „Milch kaufen“ als erledigt. <br> Schließe die Aufgabe „Lesen“ ab. <br> Markiere alle als abgeschlossen.
ShowNextPage | Aufteilen einer Liste auf mehrere Seiten für die Anzeige. Anzeigen der Listenelemente auf der nächsten Seite. | Kannst du mir die nächste Seite in der Einkaufsliste anzeigen? <br> Was steht als Nächstes auf der Liste? <br> Wie geht es weiter?
ShowPreviousPage | Anzeigen der Listenelemente auf der vorherigen Seite | Zeige die letzte. <br> Ich muss vorherige Aufgaben überprüfen.
ShowToDo | Anzeigen aller Elemente in der Aufgabenliste | Zeige meine Einkaufsliste an. <br> Zeige meine Lebensmittelliste an.

### <a name="entities"></a>**Entitäten**
LUIS-Entität | Entitätstyp | BESCHREIBUNG | Beispiele
-------|------|---------|-------------------
ContainsAll | Einfach | Anzeigen aller oder einiger Elemente der Aufgabenliste | Kannst du mir helfen, **alle** Aufgaben zu entfernen? <br> **Alles** abschließen
ordinal | ordinal | Ein Ordinalwert oder ein numerischer Verweis auf ein Element | Markiere das **Dritte** als abgeschlossen. <br> Lösche die **erste** Aufgabe.
ListType | Einfach | Typ der Aufgabenliste  | Füge Schuhe auf meiner **Einkaufsliste** hinzu.
FoodOfGrocery | List | Erkennen einer Liste mit Lebensmitteln | Erinnere mich daran, **Milch** zu kaufen. <br> Schreibe **Rindfleisch** auf meine Einkaufsliste.
TaskContent | simple, pattern.any | Erkennen des Inhalts einer Aufgabe | Erinnere mich daran, **meine Mutter anzurufen**. <br> Füge meiner Aufgabenliste **Johns Geburtstagsfeier** hinzu.


## <a name="utilities"></a>**Utilities**
Die Domäne _Utilities_ ist eine allgemeine Domäne für alle vordefinierten _LUIS_-Modelle, die allgemeine Absichten und Äußerungen in unterschiedlichen Szenarien enthält.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
 Abbrechen | Abbrechen einer Aktion/Anforderung/Aufgabe/Diensts | Brich das ab. <br> Egal, vergiss es.
 Confirm | Bestätigen einer Aktion. | Ja, natürlich <br> Ja, bitte <br> Bestätigen Sie den Vorgang.
 Reject | Ablehnen des Vorschlags des virtuellen Assistenten | Nein
 FinishTask | Abschließen einer Aufgabe, die vom Benutzer gestartet wurde | Ich bin fertig. <br> Das ist alles. <br> Fertig
 GoBack | Gehe einen Schritt zurück, oder wechsle zu einem vorhergehenden Schritt | Gehe einen Schritt zurück. <br> Zurück
 Hilfe | Anforderung von Hilfe | Hilfe, bitte. <br> Öffne die Hilfe.
 Repeat | Wiederholen einer Aktion | Sage es noch einmal.
 ShowNext | Zeige die nächsten Elemente an, oder lies sie vor. | Nächstes anzeigen
 ShowPrevious | Anzeigen des vorhergehenden Elements in einer Reihe | Vorheriges anzeigen
 StartOver | Neustart der App oder Starten einer neuen Sitzung. | Neu starten
 Beenden | Auffordern des virtuellen Assistenten, die Ansage zu beenden  | Bitte sage das nicht mehr.
 SelectAny | Auffordern zur Auswahl von Elementen oder Ergebnissen, die auf dem Bildschirm angezeigt werden | Eines davon. <br> Wähle ein beliebiges aus.
 SelectNone | Keine Auswahl der vorhandenen Elemente und Anforderung weiterer Optionen | Zeige mir weitere Vorschläge. <br> Keins dieser Elemente
  SelectItem | Auffordern zur Auswahl eines Elements oder Ergebnisses, das auf dem Bildschirm angezeigt wird | Wähle das dritte aus. <br> Wähle das rechts oben aus.
 Escalate | Anfordern des Kundendiensts bei einem Problem | Kann ich mit einer Person sprechen?
 ReadAloud | Lautes Vorlesen für den Benutzer | Lies diese Seite vor. <br> Lies es laut vor.

### <a name="entities"></a>**Entitäten**
LUIS-Entität | Entitätstyp | BESCHREIBUNG | Beispiele
------------|-------------|-------------|---------
ordinal | ordinal | Ein Ordinalwert oder ein numerischer Verweis auf ein Element | Das **zweite** <br> Das **nächste**
number | number | Anzahl der gewünschten Elemente | Die nächsten **3** Elemente
DirectionalReference | Einfach | Ein Bezugspunkt für die Position auf dem Bildschirm, an der sich ein Element befindet | Das rechte <br> upper

## <a name="weather"></a>**Weather**
Der Schwerpunkt der Domäne Wetter liegt auf der Überprüfung der Wetterbedingungen und auf Empfehlungen für Ort und Uhrzeit oder das Überprüfen der Zeit anhand von Wetterbedingungen.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
 CheckWeatherValue | Erfragen des Wetters oder wetterbezogener Faktoren für einen Standort mit Vorhersage oder älteren Informationen. <br> Zu den wetterbezogenen Faktoren gehören: <li> Temperatur </li> <li> Regen/Schnee/Niederschlag </li> <li> trocken/nass/Luftfeuchtigkeit </li> <li> Nebel </li> <li> UV-Index </li> <li> Millimeter Regen/Schnee </li> | Was ist der Luftqualitätsindex von Peking? <br> Wie viel Regen wird in Seattle im März erwartet? <br> Höchste Temperatur auf Hawaii
 CheckWeatherTime | Erfragen der Zeit einer Vorhersage oder historischer wetterbezogener Faktoren für einen Standort | Wann soll es regnen? <br> Eine gute Zeit, um nach Kanada zu fahren <br> Welches ist der heißeste Monat in Minnesota?
 QueryWeather | Erfragen von Wetterbedingungen oder wetterbezogenen Faktoren für einen bestimmten Standort, wobei „Ja“, „Nein“ oder „Vielleicht“ als Antwort erwartet wird, oder Fragen nach Empfehlungen für Aktivitäten, die von den Wetterverhältnissen abhängen | Gibt es morgen Regen? <br> Ist es heute sonnig? <br> Ist Mai zu früh, um nach Alaska zu reisen?
 ChangeTemperatureUnit | Ändern der Wettereinheit, einschließlich Celsius, Kelvin und Fahrenheit | In Celsius konvertieren <br> Kann ich das in Kelvin haben?
 GetWeatherAdvisory | Abfragen von Wetterempfehlungen oder -warnungen für einen bestimmten Standort | Gibt es Wetterempfehlungen für Memphis? <br> Gibt es Wetterwarnungen für meine Region?

### <a name="entities"></a>**Entitäten**
LUIS-Entität | Entitätstyp | BESCHREIBUNG | Beispiele
------------|-------------|-------------|---------
Location | geography | Der absolute oder implizite Standort für eine Wetteranfrage | Garmisch-Partenkirchen<br>Shanghai<br>Seattle<br>Delvina<br>
Date/Time   | datetime | Datum und Uhrzeit oder Dauer für die Wetterabfrage | November<br>Stündlich<br>morgens<br>Dieses Wochenende<br>10 Tage<br>
AdditionalWeatherCondition | list | Zusätzliches Wort zur Beschreibung von Wetter, z. B. die Geschwindigkeit oder die Richtung des Winds | direction<br>Schnell<br>Geschwindigkeit
Historic | Einfach | Wörter zur Beschreibung historischer Wetterbedingungen, darunter Durchschnitts- und Grenzwerte aus der Vergangenheit | Vergangenheit<br>historisch/Geschichte<br>saisonal<br>beste Zeit<br>seit der Aufzeichnung
PrecipitationUnit | Dimension | Die Niederschlagsmenge für Schnee oder Regen | 5 Liter<br>6 cm
SuitableFor | Einfach | Beschreibung einer menschlichen Aktivität bei bestimmten Wetterbedingungen, die bei Benutzerabfragen gängig sind für Aktivitäten, die von den Wetterverhältnissen abhängen | Jacke<br>Regenschirm<br>Schwimmbad
TemperatureUnit |Temperatur | Temperatur | 18 Celsius<br>7 Grad Kelvin
WeatherRange | Einfach | Die spezifische Bedingung für Temperatur, Wind und andere Wetterbedingungen in einem bestimmten Zeitraum | maximum<br>high<br>niedrig<br>durchschnittliche Höhe<br>Maximal
WeatherCondition | Einfach | Beschreibung der Wetterbedingungen | sonnig<br>Regen<br>regnerisch<br>Temperatur<br>Schnee<br>hot (warm)
WindDirectionUnit | list | Wörter für die Windrichtung | north<br>Süden<br>Osten<br>Westen<br>Nordosten


## <a name="web"></a>**Web**
Die Domäne „Web“ stellt Absichten und Entitäten für die Suche von Websites bereit.
### <a name="intents"></a>**Absichten**
Name der Absicht | BESCHREIBUNG | Beispiele
---------|----------|---------
 WebSearch | Eine Anforderung, zu einer angegebenen Website zu navigieren oder in einer Suchmaschine zu suchen | Suche „Surface“ in google.com. <br> Suche den Song „Happy Birthday“ im Internet. <br> Gehe zu www.twitter.com.

### <a name="entities"></a>**Entitäten**
LUIS-Entität | Entitätstyp | BESCHREIBUNG | Beispiele
------------|-------------|-------------|---------
SearchEngine | List | Die gewünschte Suchmaschine | Bing <br> Google
SearchText | simple, pattern.Any | Der zu suchende Text. <br> _Markieren Sie „Freunde bei Facebook“ als SearchText, wenn die Website nach „in“ keine Suchmaschine ist. Die URL sollte auch als SearchText markiert werden._ | Film <br> Deep Learning <br> Tom Cruise
Link | url | Der Link der Website | www.twitter.com

