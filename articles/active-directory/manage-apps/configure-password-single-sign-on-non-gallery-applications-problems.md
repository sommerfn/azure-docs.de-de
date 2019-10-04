---
title: Probleme beim Konfigurieren des einmaligen Anmeldens (SSO) per Kennwort für eine nicht im Katalog enthaltene Anwendung | Microsoft-Dokumentation
description: Allgemeine Probleme, die beim Konfigurieren des einmaligen Anmeldens (SSO) per Kennwort für nicht im Azure AD-Anwendungskatalog enthaltene benutzerdefinierte Apps auftreten.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24330dc874173ba1c6f15abb7b4caf9f23e2e00c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440352"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Probleme beim Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

In diesem Artikel werden allgemeine Probleme beschrieben, die auftreten können, wenn Sie das *einmalige Anmelden per Kennwort* (SSO) für eine nicht im Katalog enthaltene App konfigurieren.

## <a name="capture-sign-in-fields-for-an-app"></a>Erfassen von Anmeldefeldern für eine Anwendung

Das Erfassen von Anmeldefeldern wird nur für HTML-fähige Anmeldeseiten unterstützt. Für nicht standardmäßige Anmeldeseiten, z.B. Seiten, die Adobe Flash oder andere nicht-HTML-fähige Technologien verwenden, wird dieses Feature nicht unterstützt.

Es gibt zwei Möglichkeiten, Anmeldefelder für Ihre benutzerdefinierten Apps zu erfassen:

- Die **automatische Erfassung von Anmeldefeldern** funktioniert bei den meisten HTML-fähigen Anmeldeseiten, *wenn diese bekannte DIV-IDs* für die Felder von Benutzernamen und Kennwörtern verwenden. Der HTML-Code auf der Seite wird nach DIV-IDs durchsucht, die bestimmte Kriterien erfüllen. Diese Metadaten werden so gespeichert, dass sie später für die App wiedergegeben werden können.

- Die **manuelle Erfassung von Anmeldefeldern** wird verwendet, wenn der App-Anbieter *die Anmeldeeingabefelder nicht mit Bezeichnungen versieht*. Die manuelle Erfassung wird auch verwendet, wenn der Anbieter *mehrere Felder rendert, die nicht automatisch erkannt werden können*. Azure Active Directory (Azure AD) kann Daten für alle Felder auf der Anmeldeseite speichern, wenn Sie Azure AD mitteilen, wo sich diese Felder auf der Seite befinden.

Allgemein gilt: Falls die automatische Erfassung von Anmeldefeldern nicht funktioniert, versuchen Sie es mit der manuellen Option.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Automatische Erfassung von Anmeldefeldern für eine App

Führen Sie die folgenden Schritte aus, um das kennwortbasierte SSO mit der automatischen Erfassung von Anmeldefeldern zu konfigurieren:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Melden Sie sich als Globaler Administrator oder Co-Administrator an.

2. Klicken Sie im Navigationsbereich auf der linken Seite auf **Alle Dienste** um die Azure AD-Erweiterung zu öffnen.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und klicken Sie auf **Azure Active Directory**.

4. Klicken Sie im Azure AD-Navigationsbereich auf **Unternehmensanwendungen**.

5. Klicken Sie auf **Alle Anwendungen**, um eine Liste mit Ihren Apps anzuzeigen.

   > [!NOTE]
   > Wenn Ihre App nicht angezeigt wird, verwenden Sie das **Filter**-Steuerelement über der Liste **Alle Anwendungen**. Legen Sie für die Option **Anzeigen** „Alle Anwendungen“ fest.

6. Wählen Sie die App aus, die Sie für SSO konfigurieren möchten.

7. Warten Sie, bis die App geladen ist, und klicken Sie anschließend im Navigationsbereich auf der linken Seite auf **Einmaliges Anmelden**.

8. Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9. Geben Sie die **Anmelde-URL** ein. Dabei handelt es sich um die URL der Seite, auf der die Benutzer ihren Benutzernamen und ihr Kennwort für die Anmeldung eingeben. *Stellen Sie sicher, dass die Anmeldefelder auf der Seite für die URL, die Sie bereitstellen, angezeigt werden*.

10. Wählen Sie **Speichern** aus.

    Die Seite wird automatisch nach Eingabefeldern für Benutzernamen und Kennwort durchsucht. Sie können mit Azure AD jetzt unter Verwendung der Browsererweiterung für den Zugriffsbereich Kennwörter auf sichere Weise an diese App übertragen.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>Manuelle Erfassung von Anmeldefeldern für eine App

Wenn Sie Anmeldefelder manuell erfassen möchten, muss die Browsererweiterung für den Zugriffsbereich installiert sein. Darüber hinaus kann Ihr Browser nicht im *inPrivate*-, *Inkognito*- oder *privaten* Modus ausgeführt werden.

Informationen zum Installieren der Erweiterung finden Sie im Abschnitt [Installieren der Browsererweiterung für den Zugriffsbereich](#install-the-access-panel-browser-extension) dieses Artikels.

Führen Sie die folgenden Schritte aus, um das kennwortbasierte SSO für eine App unter Verwendung der manuellen Erfassung von Anmeldefeldern zu konfigurieren:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Melden Sie sich als Globaler Administrator oder Co-Administrator an.

2. Klicken Sie im Navigationsbereich auf der linken Seite auf **Alle Dienste** um die Azure AD-Erweiterung zu öffnen.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und klicken Sie auf **Azure Active Directory**.

4. Klicken Sie im Azure AD-Navigationsbereich auf **Unternehmensanwendungen**.

5. Klicken Sie auf **Alle Anwendungen**, um eine Liste mit Ihren Apps anzuzeigen.

   > [!NOTE] 
   > Wenn Ihre App nicht angezeigt wird, verwenden Sie das **Filter**-Steuerelement über der Liste **Alle Anwendungen**. Legen Sie für die Option **Anzeigen** „Alle Anwendungen“ fest.

6. Wählen Sie die App aus, die Sie für SSO konfigurieren möchten.

7. Warten Sie, bis die App geladen ist, und klicken Sie anschließend im Navigationsbereich auf der linken Seite auf **Einmaliges Anmelden**.

8. Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9. Geben Sie die **Anmelde-URL** ein. Dabei handelt es sich um die Seite, auf der die Benutzer ihren Benutzernamen und ihr Kennwort für die Anmeldung eingeben. *Stellen Sie sicher, dass die Anmeldefelder auf der Seite für die URL, die Sie bereitstellen, angezeigt werden*.

10. Wählen Sie ***&lt;appname&gt;* -Einstellungen für einmaliges Anmelden über ein Kennwort konfigurieren** aus.

11. Wählen Sie **Anmeldefelder manuell erkennen** aus.

14. Klicken Sie auf **OK**.

15. Wählen Sie **Speichern** aus.

16. Folgen Sie den Anweisungen, um den Zugriffsbereich zu verwenden.

## <a name="troubleshoot-problems"></a>Behandeln von Problemen

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Die Fehlermeldung „Wir konnten unter dieser URL keine Anmeldefelder finden“ wird angezeigt

Diese Fehlermeldung wird angezeigt, wenn die automatische Erkennung von Anmeldefeldern fehlschlägt. Verwenden Sie die manuelle Erkennung von Anmeldefeldern, um das Problem zu beheben. Weitere Informationen finden Sie im Abschnitt [Manuelle Erfassung von Anmeldefeldern für eine Anwendung](#manually-capture-sign-in-fields-for-an-app) dieses Artikels.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>Die Fehlermeldung „Die Konfiguration für einmaliges Anmelden kann nicht gespeichert werden“ wird angezeigt

In seltenen Fällen tritt beim Aktualisieren der SSO-Konfiguration ein Fehler auf. Versuchen Sie, dieses Problem zu beheben, indem Sie die Konfiguration erneut speichern.

Wenn der Fehler weiterhin auftritt, öffnen Sie eine Supportanfrage. Fügen Sie die in den Abschnitten [Anzeigen der Protalbenachrichtigungsdetails](#view-portal-notification-details) und [Senden von Benachrichtigungsdetails an einen Supporttechniker, um Hilfe zu erhalten](#send-notification-details-to-a-support-engineer-to-get-help) dieses Artikels beschriebenen Informationen hinzu.

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>Die Anmeldefelder für meine App werden nicht manuell erkannt

Möglicherweise beobachten Sie das folgende Verhalten, wenn die manuelle Erkennung nicht funktioniert:

- Die manuelle Erfassung schien zu funktionieren, aber die erfassten Felder waren nicht korrekt.

- Während der Ausführung der Erfassung werden nicht die richtigen Felder hervorgehoben.

- Die Erfassung leitet Sie wie erwartet auf die Anmeldeseite der App, doch dann geschieht nichts.

- Die manuelle Erfassung scheint zu funktionieren, aber es erfolgt keine einmalige Anmeldung, wenn Benutzer über den Zugriffsbereich zur App navigieren.

Wenn bei Ihnen eines dieser Probleme auftritt, gehen Sie folgendermaßen vor:

- Stellen Sie sicher, dass Sie die neueste Version der Browsererweiterung für den Zugriffsbereich *installiert und aktiviert* haben. Informationen finden Sie im Abschnitt [Installieren der Browsererweiterung für den Zugriffsbereich](#install-the-access-panel-browser-extension) dieses Artikels.

- Vergewissern Sie sich, dass der Browser während der Erfassung nicht im *Inkognito*-, im *InPrivate*- oder im *privaten* Modus ausgeführt wird. Die Erweiterung für den Zugriffsbereich wird in diesen Modi nicht unterstützt.

- Stellen Sie sicher, dass Ihre Benutzer nicht versuchen, sich über den Zugriffsbereich bei der App anzumelden, während sie sich im *Inkognito*-, *InPrivate*- oder *privaten* Modus befinden.

- Versuchen Sie erneut die manuelle Erfassung. Stellen Sie sicher, dass sich die roten Marker über den richtigen Feldern befinden.

- Wenn die manuelle Erfassung oder die Anmeldeseite nicht mehr reagiert, versuchen Sie, die manuelle Erfassung erneut durchzuführen. Drücken Sie dieses Mal nach Abschluss des Vorgangs jedoch die Taste F12, um die Entwicklerkonsole des Browsers zu öffnen. Wählen Sie die Registerkarte **Konsole** aus. Geben Sie **window.location=" *&lt;die Anmelde-URL, die Sie beim Konfigurieren der App angegeben haben&gt;* "** ein. Drücken Sie dann die EINGABETASTE. So erzwingen Sie eine Seitenumleitung, die den Erfassungsprozess beendet und die erfassten Felder speichert.

### <a name="contact-support"></a>Support kontaktieren

Wenn weiterhin Probleme auftreten, öffnen Sie einen Fall beim Microsoft-Support. Beschreiben Sie, was Sie versucht haben. Fügen Sie die in den Abschnitten [Anzeigen der Protalbenachrichtigungsdetails](#view-portal-notification-details) und [Senden von Benachrichtigungsdetails an einen Supporttechniker, um Hilfe zu erhalten](#send-notification-details-to-a-support-engineer-to-get-help) dieses Artikels beschriebenen Informationen hinzu (sofern zutreffend).

## <a name="install-the-access-panel-browser-extension"></a>Installieren der Browsererweiterung für den Zugriffsbereich

Folgen Sie diesen Schritten:

1. Öffnen Sie den [Zugriffsbereich](https://myapps.microsoft.com) in einen unterstützten Browser. Melden Sie sich als *Benutzer* bei Azure AD an.

2. Wählen Sie im Zugriffsbereich eine für **kennwortbasiertes SSO konfigurierte Anwendung** aus.

3. Wenn Sie aufgefordert werden, die Software zu installieren, wählen Sie **Jetzt installieren** aus.

4. Sie werden zu einer Downloadseite für Ihren Browser weitergeleitet. Wählen Sie **Hinzufügen** aus, um die Erweiterung hinzuzufügen.

5. Wählen Sie **Aktivieren** oder **Zulassen** aus, wenn eine entsprechende Aufforderung angezeigt wird.

6. Starten Sie nach der Installation den Browser neu.

7. Melden Sie sich beim Zugriffsbereich an. Probieren Sie, Ihre für kennwortbasiertes SSO konfigurierten Apps zu öffnen.

Sie können auch direkt die Browsererweiterung für Chrome und Firefox über diese Links herunterladen:

-   [Zugriffsbereichserweiterung für Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Zugriffsbereichserweiterung für Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Anzeigen der Protalbenachrichtigungsdetails

Gehen Sie folgendermaßen vor, um die Protalbenachrichtigungsdetails anzuzeigen:

1. Wählen Sie im Azure-Portal in der rechten oberen Ecke das Symbol **Benachrichtigungen** (Glockensymbol) aus.

2. Wählen Sie eine Benachrichtigung mit dem Status *Fehler* aus. (Diese sind mit einem roten „!“ versehen.)

   > [!NOTE]
   > Sie können keine Benachrichtigungen mit dem Status *Erfolgreich* oder *In Bearbeitung* auswählen.

3. Der Bereich **Benachrichtigungsdetails** wird geöffnet. Lesen Sie die Informationen zu diesem Problem.

5. Wenn Sie weitere Unterstützung benötigen, teilen Sie die Informationen einem Supporttechniker oder der Produktgruppe mit. Wählen Sie das Symbol **Kopieren** rechts neben dem Feld **Fehler kopieren** aus, um die Details der Benachrichtigung zu kopieren, die Sie weiterleiten möchten.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Senden von Benachrichtigungsdetails an einen Supporttechniker, um Hilfe zu erhalten

Es ist wichtig, dass Sie *alle* in diesem Abschnitt aufgeführten Details an den Support weiterleiten, damit Ihnen schnell geholfen werden kann. Zum Aufzeichnen können Sie einen Screenshot erstellen oder **Fehler kopieren** auswählen.

In den folgenden Informationen wird erläutert, was die einzelnen Elemente der Benachrichtigung bedeuten, und es werden Beispiele bereitgestellt.

### <a name="essential-notification-items"></a>Grundlegende Benachrichtigungselemente

- **Titel**: der beschreibende Titel der Benachrichtigung.

   Beispiel: *Anwendungsproxyeinstellungen*

- **Beschreibung**: Ergebnis des Vorgangs.

   Beispiel: *Die eingegebene interne URL wird bereits von einer anderen Anwendung verwendet.*

- **Benachrichtigungs-ID**: die eindeutige ID der Benachrichtigung.

    Beispiel: *clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **Clientanforderungs-ID**: die spezifische Anforderungs-ID, die vom Browser erstellt wurde.

    Beispiel: *302fd775-3329-4670-a9f3-bea37004f0bc*

- **Zeitstempel (UTC)** : der Zeitstempel beim Auftreten der Benachrichtigung in UTC.

    Beispiel: *2017-03-23T19:50:43.7583681Z*

- **Interne Transaktions-ID**: die interne ID, über die wir den Fehler in unseren Systemen suchen können.

    Beispiel: **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN**: Der Benutzer, der den Vorgang ausgeführt hat.

    Beispiel: *tperkins\@f128.info*

- **Mandanten-ID**: die eindeutige ID des Mandanten, dem der Benutzer angehört, der den Vorgang ausgeführt hat.

    Beispiel: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Benutzerobjekt-ID**: Die eindeutige ID des Benutzers, der den Vorgang ausgeführt hat.

    Beispiel: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Detaillierte Benachrichtigungselemente

- **Anzeigename**: (kann leer sein) ein detaillierterer Anzeigename für den Fehler.

    Beispiel: *Anwendungsproxyeinstellungen*

- **Status**: der spezifische Status der Benachrichtigung.

    Beispiel: *Fehler*

- **Objekt-ID**: (kann leer sein) die Objekt-ID, für die der Vorgang ausgeführt wurde.

   Beispiel: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Details**: detaillierte Beschreibung des Ergebnisses des Vorgangs.

    Beispiel: *Interne URL „<https://bing.com/>“ ist ungültig, da sie bereits verwendet wird.*

- **Fehler kopieren**: Ermöglicht Ihnen, das Symbol **Kopieren** rechts neben dem Textfeld **Fehler kopieren** auszuwählen, um die Details der Benachrichtigung zu kopieren, die Sie an den Support weiterleiten möchten.

    Beispiel: ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md)
