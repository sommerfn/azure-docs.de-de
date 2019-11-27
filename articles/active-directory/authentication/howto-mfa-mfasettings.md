---
title: Konfigurieren von Azure Multi-Factor Authentication – Azure Active Directory
description: In diesem Artikel wird beschrieben, wie Sie die Einstellungen der Azure Multi-Factor Authentication im Azure-Portal konfigurieren
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4932089b7315f2756f3760b252c7257868f640fa
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74066032"
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurieren von Azure Multi-Factor Authentication-Einstellungen

Dieser Artikel unterstützt Sie beim Verwalten der Einstellungen der Azure Multi-Factor Authentication im Azure-Portal. Der Artikel umfasst eine Vielzahl von Themen, die Ihnen dabei helfen, Azure Multi-Factor Authentication optimal zu nutzen. Nicht alle Funktionen sind in jeder Version von Azure Multi-Factor Authentication verfügbar.

Sie können vom Azure-Portal aus auf Einstellungen für die Azure Multi-Factor Authentication zugreifen, indem Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** navigieren.

![Azure-Portal – Einstellungen für Azure AD Multi-Factor Authentication](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-portal.png)

## <a name="settings"></a>Einstellungen

Einige dieser Einstellungen gelten für MFA-Server, Azure-MFA oder beide Optionen.

| Feature | BESCHREIBUNG |
| ------- | ----------- |
| Kontosperrung | Hiermit werden Konten im MFA-Dienst temporär gesperrt, wenn zu viele aufeinanderfolgende Authentifizierungsversuche abgelehnt werden. Dieses Feature wird nur auf Benutzer angewendet, die zur Authentifizierung eine PIN eingeben. (MFA-Server) |
| [Benutzer sperren/zulassen](#block-and-unblock-users) | Wird verwendet, um zu verhindern, dass bestimmte Benutzer Multi-Factor Authentication-Anforderungen empfangen. Authentifizierungsversuche für gesperrte Benutzer werden automatisch abgelehnt. Benutzer bleiben ab dem Zeitpunkt der Sperrung 90 Tage lang gesperrt. |
| [Betrugswarnung](#fraud-alert) | Konfigurieren Sie Einstellungen, damit Benutzer betrügerische Überprüfungsanforderungen melden können. |
| [Notifications](#notifications) | Aktivieren Sie Benachrichtigungen für Ereignisse vom MFA-Server. |
| [OATH-Token](concept-authentication-methods.md#oath-hardware-tokens-public-preview) | Wird in Cloud-basierten Azure MFA-Umgebungen verwendet, um OATH-Token für Benutzer zu verwalten. |
| [Einstellungen für Telefonanruf](#phone-call-settings) | Konfigurieren Sie Einstellungen für Telefonanrufe und Ansagen für Cloud- und lokale Umgebungen. |
| Anbieter | Hier werden alle vorhandenen Authentifizierungsanbieter angezeigt, die Sie möglicherweise mit Ihrem Konto verknüpft haben. Neue Authentifizierungsanbieter können ab 1. September 2018 nicht mehr erstellt werden. |

## <a name="manage-mfa-server"></a>Verwalten von MFA-Server

Die Einstellungen in diesem Abschnitt gelten nur für MFA-Server.

| Feature | BESCHREIBUNG |
| ------- | ----------- |
| Servereinstellungen | Laden Sie MFA-Server herunter und generieren Sie Aktivierungsdaten, um Ihre Umgebung zu initialisieren. |
| [Einmalumgehung](#one-time-bypass) | Ermöglichen Sie einem Benutzer für eine begrenzte Zeit, die Authentifizierung, ohne eine zweistufige Überprüfung durchzuführen. |
| [Cacheregeln](#caching-rules) |  Das Zwischenspeichern wird hauptsächlich verwendet, wenn lokale Systeme, z.B. VPNs, mehrere Überprüfungsanforderungen senden, während die erste Anforderung noch verarbeitet wird. Mit diesem Feature werden die nachfolgenden Anforderungen automatisch erfolgreich ausgeführt, nachdem die erste laufende Überprüfungsanforderung für den Benutzer erfolgreich ausgeführt wurde. |
| Serverstatus | Zeigen Sie den Status Ihrer lokalen MFA-Server an, einschließlich Version, Status, IP und Uhrzeit und Datum der letzten Kommunikation. |

## <a name="activity-report"></a>Aktivitätsbericht

Diese Berichterstellung ist nur für den MFA-Server (lokal) verfügbar. Lesen Sie für die Azure MFA (Cloud)-Berichte die Anmeldeberichte in Azure AD.

## <a name="block-and-unblock-users"></a>Benutzer sperren/zulassen

Verwenden Sie das Feature _Benutzer sperren/zulassen_, um zu verhindern, dass Benutzer Authentifizierungsanforderungen erhalten. Authentifizierungsversuche für gesperrte Benutzer werden automatisch abgelehnt. Benutzer bleiben ab dem Zeitpunkt der Sperrung 90 Tage lang gesperrt.

### <a name="block-a-user"></a>Sperren eines Benutzers

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Benutzer sperren/zulassen**.
3. Wählen Sie **Hinzufügen** aus, um einen Benutzer zu sperren.
4. Wählen Sie die **Replikationsgruppe** aus. Geben Sie den Benutzernamen für den gesperrten Benutzer im Format **username\@domain.com** ein. Geben Sie im Feld **Grund** einen Kommentar ein.
5. Wählen Sie **Hinzufügen** aus, um das Sperren des Benutzers abzuschließen.

### <a name="unblock-a-user"></a>Zulassen eines Benutzers

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Benutzer sperren/zulassen**.
3. Wählen Sie neben dem zu entsperrenden Benutzer in der Spalte **Aktion** die Option **Zulassen** aus.
4. Geben Sie im Feld **Grund für Entsperren** einen Kommentar ein.
5. Wählen Sie **Zulassen** aus, um das Entsperren des Benutzers abzuschließen.

## <a name="fraud-alert"></a>Betrugswarnung

Konfigurieren Sie das Feature _Betrugswarnung_, damit Ihre Benutzer betrügerische Versuche, auf ihre Ressourcen zuzugreifen, melden können. Benutzer können betrügerische Versuche mit der mobilen App oder per Telefon melden.

### <a name="turn-on-fraud-alerts"></a>Aktivieren von Betrugswarnungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Betrugswarnung**.
3. Legen Sie die Einstellung **Benutzern die Ausgabe von Betrugswarnungen erlauben** auf **Ein** fest.
4. Wählen Sie **Speichern** aus.

### <a name="configuration-options"></a>Konfigurationsoptionen

* **Benutzer bei Betrugsmeldung sperren**: Wenn ein Benutzer einen Betrug meldet, wird sein Konto 90 Tage lang oder so lange gesperrt, bis ein Administrator die Sperre für das Konto aufhebt. Ein Administrator kann anhand des Anmeldeberichts Anmeldungen überprüfen und entsprechende Maßnahmen ergreifen, um weiterem Betrug vorzubeugen. Ein Administrator kann dann die Sperre für das Konto des Benutzers [aufheben](#unblock-a-user).
* **Code zum Melden von Betrugsversuchen während der Begrüßung**: Wenn Benutzer einen Telefonanruf zur Ausführung der zweistufigen Überprüfung empfangen, drücken sie normalerweise die **#** -Taste, um ihre Anmeldung zu bestätigen. Wenn sie einen Betrug melden möchten, geben sie vor dem Drücken der **#** -Taste einen Code ein. Dieser Code ist standardmäßig **0**, Sie können ihn jedoch anpassen.

   >[!NOTE]
   >In der Standardansage von Microsoft wird der Benutzer aufgefordert, zum Senden einer Betrugswarnung die Zeichenfolge **0#** einzugeben. Wenn Sie einen anderen Code als **0** verwenden möchten, sollten Sie eine benutzerdefinierte Ansage mit den passenden Anweisungen für Ihre Benutzer aufnehmen und hochladen.
   >

### <a name="view-fraud-reports"></a>Anzeigen von Betrugsberichten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Anmeldungen** aus. Der Betrugsbericht ist damit Teil des standardmäßigen Azure AD-Berichts zu Anmeldungen.

## <a name="notifications"></a>Benachrichtigungen

Konfigurieren Sie hier E-Mail-Adressen für Benutzer, die E-Mails mit Betrugswarnungen erhalten.

![Benachrichtigungsbeispiel für eine E-Mail mit Betrugswarnung](./media/howto-mfa-mfasettings/multi-factor-authentication-fraud-alert-email.png)

## <a name="phone-call-settings"></a>Einstellungen für Telefonanruf

### <a name="caller-id"></a>Anrufer-ID

**MFA-Anrufer-ID** – Dies ist die Nummer, die Ihrem Benutzer auf dem Telefon angezeigt wird. Es sind nur Nummern aus den USA zulässig.

>[!NOTE]
>Wenn Multi-Factor Authentication-Anrufe über das öffentliche Telefonnetz geleitet werden, laufen sie manchmal über einen Betreiber, der Anrufer-IDs nicht unterstützt. Aus diesem Grund ist die Anrufer-ID nicht garantiert, obwohl das Multi-Factor Authentication-System sie immer sendet.

Wenn Sie in den USA nicht die MFA-Anrufer-ID konfiguriert haben, stammen Anrufe von Microsoft von folgenden Telefonnummern: + 1 (866) 539 4191, + 1 (855) 330 8653 und + 1 (877) 668 6536. Wenn Sie Spamfilter verwenden, stellen Sie sicher, dass diese Nummern ausgeschlossen werden.

### <a name="custom-voice-messages"></a>Benutzerdefinierte Sprachnachrichten

Mit dem Feature für _benutzerdefinierte Sprachnachrichten_ können Sie Ihre eigenen Aufzeichnungen oder Begrüßungen für die zweistufige Überprüfung verwenden. Diese Nachrichten können zusätzlich zu den Microsoft-Aufzeichnungen verwendet werden oder diese ersetzen.

Bevor Sie beginnen, sollten Sie die folgenden Einschränkungen beachten:

* Die unterstützten Dateiformate sind WAV und MP3.
* Die maximale Dateigröße beträgt 1 MB.
* Authentifizierungsnachrichten sollten kürzer als 20 Sekunden sein. Bei Nachrichten mit einer Länge von mehr als 20 Sekunden ist die Überprüfung unter Umständen nicht erfolgreich. Der Benutzer reagiert möglicherweise erst dann, wenn die Nachricht zu Ende ist, sodass bei der Überprüfung ein Timeout auftritt.

### <a name="custom-message-language-behavior"></a>Benutzerdefiniertes Nachrichtensprachenverhalten

Wenn für den Benutzer eine benutzerdefinierte Sprachnachricht wiedergegeben wird, hängt die Sprache der Meldung von diesen Faktoren ab:

* Sprache des aktuellen Benutzers.
  * Vom Browser des Benutzers erkannte Sprache.
  * Andere Authentifizierungsszenarios verhalten sich möglicherweise anders.
* Die Sprache jeglicher verfügbarer benutzerdefinierter Nachrichten.
  * Diese Sprache wird vom Administrator ausgewählt, wenn eine benutzerdefinierte Nachricht hinzugefügt wird.

Wenn beispielsweise nur eine benutzerdefinierte Nachricht in der Sprache Deutsch vorhanden ist:

* Ein Benutzer, der sich in deutscher Sprache authentifiziert, hört dann die benutzerdefinierte deutsche Nachricht.
* Ein Benutzer, der sich in Englisch authentifiziert, hört dann die englische Standardnachricht.

### <a name="set-up-a-custom-message"></a>Einrichten einer benutzerdefinierten Nachricht

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Einstellungen für Telefonanruf**.
1. Wählen Sie **Begrüßung hinzufügen**.
1. Wählen Sie den Begrüßungstyp aus.
1. Wählen Sie die Sprache aus.
1. Wählen Sie eine MP3- oder WAV-Audiodatei zum Hochladen aus.
1. Wählen Sie **Hinzufügen**.

### <a name="custom-voice-message-defaults"></a>Benutzerdefinierte Standardsprachnachrichten

Beispielskripts zum Erstellen von benutzerdefinierten Nachrichten.

| Nachrichtenname | Skript |
| --- | --- |
| Authentifizierung erfolgreich | Ihre Anmeldung wurde erfolgreich überprüft. Auf Wiedersehen. |
| Grußformel bei Durchwahl | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie #, um Ihre Anmeldung abzuschließen. |
| Betrugsmeldung bestätigen | Eine Betrugswarnung wurde ausgegeben. Wenden Sie sich an den IT-Helpdesk Ihres Unternehmens, um die Sperrung Ihres Kontos aufzuheben. |
| Betrug – Grußformel (Standardmodus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. Wenn Sie diese Überprüfung nicht initiiert haben, versucht möglicherweise jemand, auf Ihr Konto zuzugreifen. Drücken Sie die Taste 0 (null), um eine Betrugswarnung abzusenden. So benachrichtigen Sie das IT-Team Ihres Unternehmens und blockieren weitere Überprüfungsversuche. |
| Betrug wurde gemeldet. Eine Betrugswarnung wurde ausgegeben. | Wenden Sie sich an den IT-Helpdesk Ihres Unternehmens, um die Sperrung Ihres Kontos aufzuheben. |
| Aktivierung | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. |
| Authentifizierung abgelehnt. Erneut versuchen. | Überprüfung abgelehnt. |
| Erneut versuchen (Standardmodus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. |
| Grußformel (Standardmodus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. |
| Grußformel (PIN-Modus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Geben Sie Ihre PIN ein, und drücken Sie danach das Nummernzeichen, um die Überprüfung abzuschließen. |
| Betrug – Grußformel (PIN-Modus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden.  Geben Sie Ihre PIN ein, und drücken Sie danach das Nummernzeichen, um die Überprüfung abzuschließen. Wenn Sie diese Überprüfung nicht initiiert haben, versucht möglicherweise jemand, auf Ihr Konto zuzugreifen. Drücken Sie die Taste 0 (null), um eine Betrugswarnung abzusenden. So benachrichtigen Sie das IT-Team Ihres Unternehmens und blockieren weitere Überprüfungsversuche. |
| Erneut versuchen (PIN-Modus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Geben Sie Ihre PIN ein, und drücken Sie danach das Nummernzeichen, um die Überprüfung abzuschließen. |
| Grußformel nach Ansage der Durchwahlziffern | Wenn Sie bereits bei dieser Durchwahl sind, drücken Sie zum Fortfahren das Nummernzeichen. |
| Authentifizierung abgelehnt | Sie können zurzeit leider nicht angemeldet werden. Versuchen Sie es später noch mal. |
| Grußformel bei der Aktivierung (Standardmodus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. |
| Aktivierung erneut versuchen (Standardmodus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Drücken Sie das Nummernzeichen, um die Überprüfung abzuschließen. |
| Grußformel bei der Aktivierung (PIN-Modus) | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Geben Sie Ihre PIN ein, und drücken Sie danach das Nummernzeichen, um die Überprüfung abzuschließen. |
| Grußformel vor Ansage der Durchwahlziffern | Vielen Dank, dass Sie das Anmeldungsüberprüfungssystem von Microsoft verwenden. Übertragen Sie diesen Anruf an Durchwahl ... |

## <a name="one-time-bypass"></a>Einmalumgehung

Mit dem Feature _Einmalumgehung_ kann sich ein Benutzer ein einziges Mal authentifizieren, ohne die zweistufige Überprüfung durchzuführen. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab. Falls die Mobilanwendung oder das Telefon keine Benachrichtigung bzw. keinen Telefonanruf empfängt, können Sie eine Einmalumgehung aktivieren, damit der Benutzer auf die gewünschte Ressource zugreifen kann.

### <a name="create-a-one-time-bypass"></a>Erstellen einer Einmalumgehung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Einmalumgehung**.
3. Wählen Sie **Hinzufügen**.
4. Wählen Sie bei Bedarf die Replikationsgruppe für die Umgehung aus.
5. Geben Sie den Benutzernamen im Format **username\@domain.com** ein. Geben Sie die Dauer der Umgehung in Sekunden ein. Geben Sie den Grund für die Umgehung ein.
6. Wählen Sie **Hinzufügen**. Das Zeitlimit gilt sofort. Der Benutzer muss sich anmelden, bevor die Einmalumgehung abläuft.

### <a name="view-the-one-time-bypass-report"></a>Anzeigen des Berichts für die Einmalumgehung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Einmalumgehung**.

## <a name="caching-rules"></a>Cacheregeln

Mit dem Feature _Zwischenspeichern_ können Sie einen Zeitraum festlegen, sodass Authentifizierungsversuche zulässig sind, nachdem ein Benutzer authentifiziert wurde. Nachfolgende Authentifizierungsversuche des Benutzers innerhalb des angegebenen Zeitraums sind automatisch erfolgreich. Das Zwischenspeichern wird hauptsächlich verwendet, wenn lokale Systeme, z.B. VPNs, mehrere Überprüfungsanforderungen senden, während die erste Anforderung noch verarbeitet wird. Mit diesem Feature werden die nachfolgenden Anforderungen automatisch erfolgreich ausgeführt, nachdem die erste laufende Überprüfungsanforderung für den Benutzer erfolgreich ausgeführt wurde.

>[!NOTE]
>Die Zwischenspeicherung ist nicht für Anmeldungen bei Azure Active Directory (Azure AD) vorgesehen.

### <a name="set-up-caching"></a>Einrichten der Zwischenspeicherung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Cacheregeln**.
3. Wählen Sie **Hinzufügen**.
4. Wählen Sie den **Cachetyp** in der Dropdownliste aus. Geben Sie die maximale **Cacheaufbewahrungsdauer (Sek.)** ein.
5. Wählen Sie bei Bedarf einen Authentifizierungstyp aus, und geben Sie eine Anwendung an.
6. Wählen Sie **Hinzufügen**.

## <a name="mfa-service-settings"></a>MFA-Diensteinstellungen

Einstellungen für App-Kennwörter, vertrauenswürdige IPs, Überprüfungsoptionen und das Speichern von Multi-Factor Authentication für die Azure Multi-Factor Authentication finden Sie in den Diensteinstellungen. Auf die Diensteinstellungen kann über das Azure-Portal zugegriffen werden, indem Sie zu **Azure Active Directory** > **Sicherheit** > **MFA** > **Erste Schritte** > **Konfigurieren** > **Zusätzliche Cloud-basierte MFA-Einstellungen** navigieren.

![Diensteinstellung für Azure Multi-Factor Authentication](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-service-settings.png)

Die vertrauenswürdigen IP-Adressbereiche können privat oder öffentlich sein.

## <a name="app-passwords"></a>App-Kennwörter

Einige Anwendungen wie Office 2010 oder frühere Versionen und Apple Mail vor iOS 11unterstützen die zweistufige Überprüfung nicht. Diese Anwendungen sind nicht für die Annahme einer zweiten Überprüfung konfiguriert. Um diese Anwendungen zu verwenden, können Sie das Feature für _App-Kennwörter_ nutzen. Sie können anstelle des herkömmlichen Kennworts ein App-Kennwort verwenden, sodass eine Anwendung die zweistufige Überprüfung umgehen und weiter ausgeführt werden kann.

Die moderne Authentifizierung für Microsoft Office 2013-Clients und neuere Clients wird unterstützt. Office 2013-Clients (einschließlich Outlook) unterstützen moderne Authentifizierungsprotokolle und können für die zweistufige Überprüfung aktiviert werden. Nach der Aktivierung sind für diese Clients keine App-Kennwörter erforderlich.

>[!NOTE]
>App-Kennwörter funktionieren nicht mit auf bedingtem Zugriff basierenden Multi-Factor Authentication-Richtlinien und moderner Authentifizierung.

### <a name="considerations-about-app-passwords"></a>Informationen zu App-Kennwörtern

Bei der Verwendung von App-Kennwörtern sollten die folgenden wichtigen Punkte beachtet werden:

* App-Kennwörter werden pro Anwendung nur einmal eingegeben. So müssen sich Benutzer Kennwörter nicht merken und jedes Mal erneut eingeben.
* Das eigentliche Kennwort wird automatisch erzeugt und nicht vom Benutzer festgelegt. Automatisch generierte Kennwörter sind sicherer und für einen Angreifer schwerer zu erraten.
* Pro Benutzer können maximal 40 Kennwörter festgelegt werden.
* Bei Anwendungen, die Kennwörter zwischenspeichern und in lokalen Szenarien nutzen, können Fehler auftreten, da das App-Kennwort außerhalb des Geschäfts-, Schul- oder Unikontos nicht bekannt ist. Ein Beispiel dafür sind Exchange-E-Mails, die lokal vorhanden sind, bei denen sich die archivierten Nachrichten jedoch in der Cloud befinden. Das gleiche Kennwort funktioniert in diesem Fall nicht.
* Nachdem Multi-Factor Authentication für ein Benutzerkonto aktiviert wurde, können für die meisten Nicht-Browserclients, z.B. Outlook oder Microsoft Skype for Business, App-Kennwörter verwendet werden. Administrative Aufgaben können über Nicht-Browseranwendungen wie z.B. Windows PowerShell nicht mit App-Kennwörtern durchgeführt werden. Die Aufgaben können selbst dann nicht durchgeführt werden, wenn der betreffende Benutzer über ein administratives Konto verfügt. Erstellen Sie zum Ausführen von PowerShell-Skripts ein Dienstkonto mit einem sicheren Kennwort, und aktivieren Sie für das Konto nicht die zweistufige Überprüfung.

>[!WARNING]
>App-Kennwörter funktionieren nicht in Hybridumgebungen, in denen Clients sowohl mit lokalen AutoErmittlung-Endpunkten als auch mit solchen in der Cloud kommunizieren. Für die lokale Authentifizierung werden Domänenkennwörter benötigt. Für die Authentifizierung in der Cloud sind App-Kennwörter erforderlich.

### <a name="guidance-for-app-password-names"></a>Richtlinien für die Benennung von App-Kennwörtern

Die Namen der App-Kennwörter sollten auf das Gerät hinweisen, auf dem sie verwendet werden. Für einen Laptop, der über Nicht-Browseranwendungen wie Outlook, Word und Excel verfügt, erstellen Sie ein App-Kennwort mit dem Namen **Laptop** für diese Anwendungen. Erstellen Sie ein weiteres App-Kennwort namens **Desktop** für die gleichen auf dem Desktopcomputer ausgeführten Anwendungen.

>[!NOTE]
>Es empfiehlt sich, Sie jeweils ein App-Kennwort pro Gerät (und nicht pro Anwendung) zu erstellen.

### <a name="federated-or-single-sign-on-app-passwords"></a>App-Kennwörter im Verbund oder mit einmaligem Anmelden

Azure AD unterstützt den Verbund oder das einmalige Anmelden (SSO) mit lokalen Windows Server Active Directory Domain Services (AD DS). Wenn Ihre Organisation im Verbund mit Azure AD besteht und Sie Azure Multi-Factor Authentication verwenden, sind die folgenden Punkte zu App-Kennwörtern zu beachten.

>[!NOTE]
>Die folgenden Punkte gelten nur für Verbundkunden (SSO).

* App-Kennwörter werden von Azure AD überprüft, sodass der Verbund umgangen wird. Der Verbund wird nur beim Einrichten von App-Kennwörtern aktiv verwendet.
* Im Gegensatz zum passiven Ablauf erfolgt für Verbundbenutzer (SSO) keine Kontaktaufnahme mit dem Identitätsanbieter (Identity Provider, IdP). Die App-Kennwörter werden im Geschäfts-, Schul- oder Unikonto gespeichert. Wenn ein Benutzer das Unternehmen verlässt, werden die Benutzerinformationen mithilfe von **DirSync** in Echtzeit in das Geschäfts-, Schul- oder Unikonto übertragen. Die Synchronisierung nach dem Deaktivieren oder Löschen des Kontos kann bis zu drei Stunden dauern, sodass das Deaktivieren bzw. Löschen des App-Kennworts in Azure AD sich verzögern kann.
* Lokale Einstellungen für die Clientzugriffssteuerung werden vom Feature für App-Kennwörter nicht berücksichtigt.
* Zur Verwendung des Features für App-Kennwörter ist keine lokale Funktion zur Protokollierung oder Überwachung der Authentifizierung verfügbar.
* In bestimmten erweiterten Architekturen ist für die zweistufige Überprüfung mit Clients eine Kombination aus Anmeldeinformationen erforderlich. Diese Anmeldeinformationen können den Benutzernamen und Kennwörter eines Geschäfts-, Schul- oder Unikontos und App-Kennwörter umfassen. Die jeweiligen Anforderungen hängen davon ab, wie die Authentifizierung durchgeführt wird. Bei Clients, die sich bei einer lokalen Infrastruktur authentifizieren, sind der Benutzername und das Kennwort eines Geschäfts-, Schul- oder Unikontos erforderlich. Für Clients, die sich bei Azure AD authentifizieren, wird ein App-Kennwort benötigt.

  Angenommen, Sie verfügen über die folgende Architektur:

  * Ihre lokale Instanz von Active Directory bildet einen Verbund mit Azure AD.
  * Sie verwenden Exchange Online.
  * Sie verwenden Skype for Business lokal.
  * Sie verwenden Azure Multi-Factor Authentication.

  In diesem Szenario verwenden Sie die folgenden Anmeldeinformationen:

  * Verwenden Sie für die Anmeldung bei Skype for Business den Benutzernamen und das Kennwort für Ihr Geschäfts-, Schul- oder Unikonto.
  * Verwenden Sie ein App-Kennwort für den Zugriff auf das Adressbuch über einen Outlook-Client, der die Verbindung mit Exchange Online herstellt.

### <a name="allow-users-to-create-app-passwords"></a>Zulassen der Erstellung von App-Kennwörtern durch Benutzer

Standardmäßig können Benutzer keine App-Kennwörter erstellen. Das Feature für App-Kennwörter muss aktiviert werden. Gehen Sie wie folgt vor, um Benutzern die Erstellung von App-Kennwörtern zu ermöglichen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Bereich **Azure Active Directory** > **Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus.
4. Klicken Sie unter „Multi-Factor Authentication“ auf **Diensteinstellungen**.
5. Wählen Sie auf der Seite **Diensteinstellungen** die Option **Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei nicht browserbasierten Apps gestatten** aus.

### <a name="create-app-passwords"></a>Erstellen von App-Kennwörter

Benutzer können App-Kennwörter während ihrer ersten Registrierung erstellen. Am Ende des Registrierungsvorgangs haben Benutzer die Möglichkeit, App-Kennwörter zu erstellen.

Benutzer können auch App-Kennwörter nach der Registrierung erstellen. Weitere Informationen und detaillierte Schritte für Ihre Benutzer finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](../user-help/multi-factor-authentication-end-user-app-passwords.md)

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen

Das Feature _Vertrauenswürdige IPs_ von Azure Multi-Factor Authentication wird von Administratoren eines verwalteten Mandanten oder Verbundmandanten verwendet. Mit dem Feature wird die zweistufige Überprüfung für Benutzer umgangen, die sich über das Intranet des Unternehmens anmelden. Das Feature ist in der Vollversion von Azure Multi-Factor Authentication, aber nicht in der kostenlosen Version für Administratoren verfügbar. Weitere Informationen zum Beziehen der Vollversion von Azure Multi-Factor Authentication finden Sie unter [Azure Multi-Factor Authentication](multi-factor-authentication.md).

> [!NOTE]
> Für MFA vertrauenswürdige IPs und benannte Orte mit bedingtem Zugriff funktionieren nur mit IPV4-Adressen.

Wenn Ihre Organisation die NPS-Erweiterung bereitstellt, um MFA für lokale Anwendungen bereitzustellen, beachten Sie, dass als Quell-IP-Adresse immer die des NPS-Servers angezeigt wird, über den der Authentifizierungsversuch verläuft.

| Azure AD-Mandantentyp | Optionen des Features „Vertrauenswürdige IPs“ |
|:--- |:--- |
| Verwaltet |**Bestimmter Bereich von IP-Adressen**: Administratoren geben einen Bereich von IP-Adressen an, die die zweistufige Überprüfung für Benutzer, die sich über das Intranet des Unternehmens anmelden, umgehen können. Es können maximal 50 vertrauenswürdige IP-Adressbereiche konfiguriert werden.|
| Im Verbund |**Alle Verbundbenutzer**: Alle Verbundbenutzer, die sich von innerhalb der Organisation aus anmelden, können die zweistufige Überprüfung umgehen. Die Benutzer umgehen die Überprüfung mithilfe eines durch Active Directory-Verbunddienste (AD FS) ausgestellten Anspruchs.<br/>**Bestimmter Bereich von IP-Adressen**: Administratoren geben einen Bereich von IP-Adressen an, die die zweistufige Überprüfung für Benutzer, die sich über das Intranet des Unternehmens anmelden, umgehen können. |

Die Umgehung über vertrauenswürdige IPs funktioniert nur innerhalb des Intranets eines Unternehmens. Wenn Sie die Option **Alle Verbundbenutzer** ausgewählt haben und sich ein Benutzer von außerhalb des Unternehmensintranets anmeldet, muss sich der Benutzer mit der zweistufigen Überprüfung authentifizieren. Dies ist auch der Fall, wenn der Benutzer einen AD FS-Anspruch vorweisen kann. 

### <a name="end-user-experience-inside-of-corpnet"></a>Endbenutzererfahrung innerhalb des Unternehmensnetzwerks

Wenn das Feature „Vertrauenswürdige IPs“ deaktiviert ist, ist die zweistufige Überprüfung für Browserabläufe erforderlich. Für ältere Rich Client-Anwendungen werden App-Kennwörter benötigt.

Wenn das Feature „Vertrauenswürdige IPs“ aktiviert ist, ist die zweistufige Überprüfung für Browserabläufe *nicht* erforderlich. App-Kennwörter sind für ältere Rich Client-Anwendungen *nicht* erforderlich, sofern der Benutzer nicht bereits ein App-Kennwort erstellt hat. Sobald ein App-Kennwort verwendet wird, ist es erforderlich. 

### <a name="end-user-experience-outside-corpnet"></a>Endbenutzererfahrung außerhalb des Unternehmensnetzwerks

Unabhängig davon, ob das Feature „Vertrauenswürdige IPs“ aktiviert ist, ist die zweistufige Überprüfung für Browserabläufe erforderlich. Für ältere Rich Client-Anwendungen werden App-Kennwörter benötigt.

### <a name="enable-named-locations-by-using-conditional-access"></a>Aktivieren benannter Orte mit bedingtem Zugriff

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite die Optionen **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff** > **Benannte Orte** aus.
3. Wählen Sie **Neuer Ort** aus.
4. Geben Sie einen Namen für den Standort ein.
5. Wählen Sie **Als vertrauenswürdigen Standort markieren** aus.
6. Geben Sie den IP-Adressbereich in CIDR-Notation (z.B. **192.168.1.1/24**) ein.
7. Klicken Sie auf **Erstellen**.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>Aktivieren des Features „Vertrauenswürdige IPs“ mit bedingtem Zugriff

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite die Optionen **Azure Active Directory** > **Sicherheit** >  **Bedingter Zugriff** > **Benannte Orte** aus.
3. Wählen Sie **Durch MFA bestätigte IPs konfigurieren** aus.
4. Wählen Sie auf der Seite **Diensteinstellungen** unter **Vertrauenswürdige IPs** eine der folgenden beiden Optionen aus:

   * **Für Anforderungen von Verbundbenutzern, die aus meinem Intranet stammen**: Aktivieren Sie das Kontrollkästchen, um diese Option auszuwählen. Alle Verbundbenutzer, die sich vom Unternehmensnetzwerk aus anmelden, umgehen die zweistufige Überprüfung mithilfe eines von AD FS ausgestellten Anspruchs. Stellen Sie sicher, dass AD FS über eine Regel zum Hinzufügen des Intranetanspruchs zum entsprechenden Datenverkehr verfügt. Wenn die Regel nicht vorhanden ist, erstellen Sie die folgende Regel in AD FS:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Für Anforderungen aus einem bestimmten Bereich öffentlicher IPs**: Geben Sie zum Auswählen dieser Option die IP-Adressen in der CIDR-Notation im Textfeld ein.
      * Verwenden Sie für IP-Adressen im Bereich xxx.xxx.xxx.1 bis xxx.xxx.xxx.254 die Notation **xxx.xxx.xxx.0/24**.
      * Verwenden Sie für eine einzelne IP-Adresse die Notation **xxx.xxx.xxx.xxx/32**.
      * Sie können bis zu 50 IP-Adressbereiche eingeben. Benutzer, die sich über diese IP-Adressen anmelden, umgehen die zweistufige Überprüfung.

5. Wählen Sie **Speichern** aus.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>Aktivieren des Features „Vertrauenswürdige IPs“ mit Diensteinstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Bereich **Azure Active Directory** > **Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus.
4. Klicken Sie unter „Multi-Factor Authentication“ auf **Diensteinstellungen**.
5. Wählen Sie auf der Seite **Diensteinstellungen** unter **Vertrauenswürdige IPs** eine (oder beide) der folgenden zwei Optionen aus:

   * **Für Anforderungen von Verbundbenutzern in meinem Intranet**: Aktivieren Sie das Kontrollkästchen, um diese Option auszuwählen. Alle Verbundbenutzer, die sich vom Unternehmensnetzwerk aus anmelden, umgehen die zweistufige Überprüfung mithilfe eines von AD FS ausgestellten Anspruchs. Stellen Sie sicher, dass AD FS über eine Regel zum Hinzufügen des Intranetanspruchs zum entsprechenden Datenverkehr verfügt. Wenn die Regel nicht vorhanden ist, erstellen Sie die folgende Regel in AD FS:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Für Anforderungen aus einem angegebenen Bereich von IP-Adresssubnetzen**: Geben Sie zum Auswählen dieser Option die IP-Adressen in der CIDR-Notation im Textfeld ein.
      * Verwenden Sie für IP-Adressen im Bereich xxx.xxx.xxx.1 bis xxx.xxx.xxx.254 die Notation **xxx.xxx.xxx.0/24**.
      * Verwenden Sie für eine einzelne IP-Adresse die Notation **xxx.xxx.xxx.xxx/32**.
      * Sie können bis zu 50 IP-Adressbereiche eingeben. Benutzer, die sich über diese IP-Adressen anmelden, umgehen die zweistufige Überprüfung.

6. Wählen Sie **Speichern** aus.

## <a name="verification-methods"></a>Überprüfungsmethoden

Sie können auswählen, welche Überprüfungsmethoden für Ihre Benutzer verfügbar sind. In der folgenden Tabelle finden Sie eine kurze Übersicht über die einzelnen Methoden.

Wenn Ihre Benutzer ihre Konten für Azure Multi-Factor Authentication registrieren, wählen sie ihre bevorzugte Überprüfungsmethode aus den Optionen aus, die Sie aktiviert haben. Anleitungen zum Benutzerregistrierungsprozess finden Sie unter [Einrichten meines Kontos für die zweistufige Überprüfung](../user-help/multi-factor-authentication-end-user-first-time.md).

| Methode | BESCHREIBUNG |
|:--- |:--- |
| Auf Telefon anrufen |Startet einen automatisierten Sprachanruf. Der Benutzer nimmt den Anruf an und drückt die #-Taste auf der Telefontastatur, um sich zu authentifizieren. Die Telefonnummer wird nicht mit dem lokalen Active Directory synchronisiert. |
| Textnachricht an Telefon |Sendet eine Textnachricht mit einem Überprüfungscode. Der Benutzer wird über die Anmeldeoberfläche zur Eingabe des Prüfcodes aufgefordert. Dieser Vorgang wird als „unidirektionale SMS“ bezeichnet. Bei einer bidirektionalen SMS muss der Benutzer einen bestimmten Code per SMS zurücksenden. Die Funktion für bidirektionale SMS ist veraltet und wird ab dem 14. November 2018 nicht mehr unterstützt. Benutzer, die für die Verwendung bidirektionaler SMS konfiguriert sind, werden dann automatisch auf die Überprüfungsoption _Auf Telefon anrufen_ umgestellt.|
| Benachrichtigung über mobile App |Sendet eine Pushbenachrichtigung an Ihr Telefon oder registriertes Gerät. Der Benutzer zeigt die Benachrichtigung an und wählt **Überprüfen**, um die Überprüfung abzuschließen. Die Microsoft Authenticator-App ist für [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6), [Android](https://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](https://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |
| Prüfcode aus mobiler App oder Hardwaretoken |Die Microsoft Authenticator-App generiert alle 30 Sekunden einen neuen OATH-Überprüfungscode. Der Benutzer gibt den Überprüfungscode auf der Anmeldeoberfläche ein. Die Microsoft Authenticator-App ist für [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6), [Android](https://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](https://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |

### <a name="enable-and-disable-verification-methods"></a>Aktivieren und Deaktivieren von Überprüfungsmethoden

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Bereich **Azure Active Directory** > **Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus.
4. Klicken Sie unter „Multi-Factor Authentication“ auf **Diensteinstellungen**.
5. Aktivieren oder deaktivieren Sie auf der Seite **Diensteinstellungen** unter **Überprüfungsoptionen** die Methoden, die den Benutzern zur Verfügung gestellt werden.
6. Klicken Sie auf **Speichern**.

Weitere Informationen zur Verwendung von Authentifizierungsmethoden und finden Sie im Artikel [Was sind Authentifizierungsmethoden?](concept-authentication-methods.md).

## <a name="remember-multi-factor-authentication"></a>Speichern der Multi-Factor Authentication

Das Feature _Multi-Factor Authentication speichern_ für Geräte und Browser, die Benutzer als vertrauenswürdig einstufen, ist ein kostenloses Feature für alle Benutzer von Multi-Factor Authentication. Benutzer können nachfolgende Überprüfungen für eine angegebene Anzahl von Tagen umgehen, nachdem sie sich mithilfe von Multi-Factor Authentication erfolgreich bei einem Gerät angemeldet haben. Bei Verwendung dieses Features wird die Benutzerfreundlichkeit verbessert, indem der Benutzer auf einem Gerät seltener eine zweistufige Überprüfung durchführen muss.

>[!IMPORTANT]
>Wenn ein Konto oder Gerät gefährdet ist, kann das Speichern von Multi-Factor Authentication für vertrauenswürdige Geräte die Sicherheit beeinträchtigen. Wenn ein Unternehmenskonto kompromittiert oder ein vertrauenswürdiges Gerät verloren geht oder gestohlen wird, sollten Sie [MFA-Sitzungen widerrufen](howto-mfa-userdevicesettings.md).
>
>Durch die Wiederherstellungsaktion wird der vertrauenswürdige Status aller Geräte widerrufen, und der Benutzer muss wieder die zweistufige Überprüfung ausführen. Sie können Ihre Benutzer auch anweisen, Multi-Factor Authentication auf ihren eigenen Geräten anhand der Anweisungen unter [Verwalten der Einstellungen für die zweistufige Überprüfung](../user-help/multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device) wiederherzustellen.

### <a name="how-the-feature-works"></a>Funktionsweise des Features

Durch das Feature „Multi-Factor Authentication speichern“ wird ein permanentes Cookie im Browser festgelegt, wenn ein Benutzer die Option **Die nächsten X Tage nicht erneut fragen** auswählt. Der Benutzer wird von diesem Browser bis zum Ablauf des Cookies nicht erneut zur Multi-Factor Authentication aufgefordert. Wenn der Benutzer einen anderen Browser auf dem gleichen Gerät öffnet oder seine Cookies löscht, wird er wieder zur Überprüfung aufgefordert.

Die Option **Die nächsten X Tage nicht erneut fragen** wird in Nicht-Browseranwendungen nicht angezeigt, unabhängig davon, ob diese die moderne Authentifizierung unterstützen. Diese Apps verwenden _Aktualisierungstoken_, die jede Stunde neue Zugriffstoken bereitstellen. Bei der Überprüfung eines Aktualisierungstokens überprüft Azure AD, ob die letzte zweistufige Überprüfung innerhalb der angegebenen Anzahl von Tagen durchgeführt wurde.

Mit dem Feature reduziert sich die Anzahl der Authentifizierungen für Web-Apps, die normalerweise jedes Mal dazu auffordern. Das Feature erhöht die Anzahl der Authentifizierungen für Clients mit moderner Authentifizierung, die normalerweise alle 90 Tage zur Authentifizierung auffordern. Die Anzahl der Authentifizierungen kann sich auch erhöhen, wenn mit bedingten Zugriffsrichtlinien kombiniert wird.

>[!IMPORTANT]
>Die Funktion **Multi-Factor Authentication speichern** ist nicht kompatibel mit dem AD FS-Feature **Angemeldet bleiben**, bei dem Benutzer die zweistufige Überprüfung für AD FS über Azure Multi-Factor Authentication Server oder eine Lösung zur mehrstufigen Authentifizierung von Drittanbietern ausführen.
>
>Wenn Ihre Benutzer in AD FS **Angemeldet bleiben** auswählen und ihr Gerät außerdem als für Multi-Factor Authentication vertrauenswürdig markieren, werden sie nach Ablauf der Anzahl von Tagen, die unter **Multi-Factor Authentication speichern** angegeben wurde, nicht automatisch überprüft. Azure AD fordert eine neue zweistufige Überprüfung an, aber AD FS gibt ein Token mit dem ursprünglichen Multi-Factor Authentication-Anspruch und Datum zurück, statt die zweistufige Überprüfung erneut durchzuführen. **Durch diese Reaktion entsteht eine Schleife bei der Überprüfung zwischen Azure AD und AD FS.**
>
>Das Feature **Multi-Factor Authentication speichern** ist nicht mit B2B-Benutzern kompatibel und wird B2B-Benutzer nicht angezeigt, wenn sie sich bei den eingeladenen Mandanten anmelden.
>

### <a name="enable-remember-multi-factor-authentication"></a>Aktivieren der Speicherung von Multi-Factor Authentication

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Bereich **Azure Active Directory** > **Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus.
4. Klicken Sie unter „Multi-Factor Authentication“ auf **Diensteinstellungen**.
5. Wählen Sie auf der Seite **Diensteinstellungen** unter **Multi-Factor Authentication speichern** die Option **Benutzern das Speichern der mehrstufigen Authentifizierung auf vertrauenswürdigen Geräten ermöglichen** aus.
6. Legen Sie fest, für wie viele Tage die vertrauenswürdigen Geräte die zweistufige Überprüfung umgehen können. Der Standardwert ist 14 Tage.
7. Wählen Sie **Speichern** aus.

### <a name="mark-a-device-as-trusted"></a>Markieren eines Geräts als vertrauenswürdig

Nachdem Sie das Feature „Multi-Factor Authentication speichern“ aktiviert haben, können Benutzer bei der Anmeldung ein Gerät als vertrauenswürdig markieren, indem sie **Nicht erneut nachfragen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

[Ändern des Brandings der Azure AD-Anmeldeseite](../fundamentals/customize-branding.md)
