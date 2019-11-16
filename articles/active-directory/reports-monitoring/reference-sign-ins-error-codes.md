---
title: Fehlercodes im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Enthält eine Referenz zu den Fehlercodes des Berichts mit den Anmeldeaktivitäten.
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
ms.date: 08/08/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84e9d484da1b59c456b30cf0b465ec6de5af77d1
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014300"
---
# <a name="sign-in-activity-report-error-codes"></a>Fehlercodes des Berichts mit den Anmeldeaktivitäten 

Mit den Informationen, die vom [Bericht zu Benutzeranmeldungen](concept-sign-ins.md) geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wer hat sich bei meiner Anwendung angemeldet?
- Bei welchen Anwendungen haben sich Benutzer angemeldet?
- Welche Anmeldeversuche waren nicht erfolgreich, und warum?

Wenn eine Anmeldung fehlschlägt, sehen Sie einen Fehlercode, der dem Fehler entspricht. In diesem Artikel werden die Fehlercodes und deren Beschreibungen ggf. zusammen mit einer empfohlenen Vorgehensweise aufgeführt. 

## <a name="how-can-i-display-failed-sign-ins"></a>Wie kann ich Anmeldungen anzeigen, für die Fehler aufgetreten sind? 

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum [Anmeldebericht](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns).

![Anmeldeaktivität](./media/reference-sign-ins-error-codes/61.png "Anmeldeaktivität")

Filtern Sie den Bericht, um alle fehlgeschlagenen Anmeldungen anzuzeigen, indem Sie aus dem Dropdownfeld **Anmeldestatus** die Option **Fehler** auswählen.

![Anmeldeaktivität](./media/reference-sign-ins-error-codes/06.png "Anmeldeaktivität")

Wenn Sie in der gefilterten Liste einen Eintrag auswählen, wird das Blatt **Aktivitätsdetails: Anmeldungen** geöffnet. Diese Ansicht enthält weitere Informationen über das fehlgeschlagene Anmeldeereignis, einschließlich des **Fehlercodes der Anmeldung** und der **Fehlerursache**.

![Anmeldeaktivität](./media/reference-sign-ins-error-codes/05.png "Anmeldeaktivität")

Sie können auch programmgesteuert mithilfe der [Berichterstellungs-API](concept-reporting-api.md) auf die Anmeldedaten zugreifen.

## <a name="error-codes"></a>Fehlercodes


|Error|BESCHREIBUNG|
|---|---|
|16000|Dies ist ein internes Implementierungsdetail und keine Fehlerbedingung. Sie können diesen Verweis ignorieren.|
|20001|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|20012|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|20033|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|40008|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|40009|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|40014|Es besteht ein Problem mit Ihrem Verbundidentitätsanbieter. Wenden Sie sich an Ihren Identitätsanbieter, um das Problem zu beheben.|
|50000|Es besteht ein Problem mit unserem Anmeldedienst. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md), um dieses Problem zu beheben.|
|50001|Der Dienstprinzipalname wurde unter diesem Mandanten nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde oder wenn der Ressourcenprinzipal im Verzeichnis nicht gefunden wurde oder ungültig ist.|
|50002|Fehler bei der Anmeldung aufgrund von eingeschränktem Proxyzugriff auf dem Mandanten. Falls es sich um Ihre eigene Mandantenrichtlinie handelt, können Sie die Einschränkungseinstellungen des Mandanten ändern, um dieses Problem zu beheben.|
|50003|Fehler bei der Anmeldung aufgrund eines fehlenden Signaturschlüssels oder Zertifikats. Der Grund hierfür kann sein, dass in der Anwendung kein Signaturschlüssel konfiguriert wurde. Sehen Sie sich die Lösungen unter [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured) an. Wenn das Problem weiterhin besteht, wenden Sie sich an den Anwendungsbesitzer oder den Anwendungsadministrator.|
|50005|Der Benutzer hat versucht, sich über eine Plattform an einem Gerät anzumelden, die aufgrund einer Richtlinie für bedingten Zugriff derzeit nicht unterstützt wird.|
|50006| Fehler bei der Signaturüberprüfung aufgrund einer ungültigen Signatur. Sehen Sie sich die Lösung unter [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery) an. Wenn das Problem weiterhin besteht, wenden Sie sich an den Anwendungsbesitzer oder den Anwendungsadministrator.|
|50007|Das Partnerverschlüsselungszertifikat für diese Anwendung wurde nicht gefunden. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md) bei Microsoft, um dieses Problem zu beheben.|
|50008|Die SAML-Assertion fehlt oder wurde im Token falsch konfiguriert. Wenden Sie sich an Ihren Verbundanbieter.|
|50010|Fehler bei der Überprüfung des Benutzergruppen-URI für die Anwendung, da keine Tokenzielgruppen konfiguriert wurden. Wenden Sie sich zur Lösung an den Anwendungsbesitzer.|
|50011|Die Antwortadresse fehlt, ist falsch konfiguriert oder stimmt nicht mit den für die Anwendung konfigurierten Antwortadressen überein. Versuchen Sie es mit der Lösung, die unter [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application) angegeben ist. Wenn das Problem weiterhin besteht, wenden Sie sich an den Anwendungsbesitzer oder den Anwendungsadministrator.|
|50012| Dies ist eine generische Fehlermeldung, die angibt, dass ein Authentifizierungsfehler aufgetreten ist. Dies kann z.B. durch fehlende oder ungültige Anmeldeinformationen oder Ansprüche in der Anforderung verursacht werden. Stellen Sie sicher, dass die Anforderung mit den richtigen Anmeldeinformationen und Ansprüchen gesendet wird. |
|50013|Die Assertion ist aus verschiedenen Gründen ungültig. Beispiele: Der Aussteller des Tokens stimmt nicht mit der API-Version innerhalb des gültigen Zeitraums überein, das Token ist abgelaufen oder falsch formatiert, oder das Aktualisierungstoken in der Assertion ist kein primäres Aktualisierungstoken.|
|50017|Fehler bei der Überprüfung der Zertifizierung aus den folgenden Gründen:<ul><li>Das Ausstellerzertifikat kann in der Liste mit den vertrauenswürdigen Zertifikaten nicht gefunden werden.</li><li>Das erwartete CrlSegment-Element kann nicht gefunden werden.</li><li>Das Ausstellerzertifikat kann in der Liste mit den vertrauenswürdigen Zertifikaten nicht gefunden werden.</li><li>Delta-Zertifikatsperrlisten-Verteilungspunkt wurde ohne entsprechenden Zertifikatsperrlisten-Verteilungspunkt konfiguriert.</li><li>Aufgrund einer Zeitüberschreitung können keine gültigen Zertifikatsperrlisten-Segmente abgerufen werden.</li><li>Die Zertifikatsperrliste kann nicht heruntergeladen werden.</li></ul>Wenden Sie sich an den Administrator des Mandanten.|
|50020|Der Benutzer ist aus einem der folgenden Gründe nicht autorisiert.<ul><li>Der Benutzer versucht, für die Anmeldung ein MSA-Konto mit dem v1-Endpunkt zu verwenden.</li><li>Der Benutzer ist im Mandanten nicht vorhanden.</li></ul> Wenden Sie sich an den Besitzer der Anwendung.|
|50027|Das JWT-Token ist aus den folgenden Gründen ungültig:<ul><li>Enthält keinen Nonce-Anspruch, untergeordneten Anspruch</li><li>Antragstellerbezeichner stimmt nicht überein</li><li>Doppelter Anspruch in idToken-Ansprüchen</li><li>Unerwarteter Aussteller</li><li>Unerwartete Zielgruppe</li><li>Liegt nicht innerhalb des gültigen Zeitbereichs </li><li>Tokenformat ist fehlerhaft</li><li>Fehler bei Signaturüberprüfung für das Token für die externe ID vom Aussteller.</li></ul>Wenden Sie sich an den Besitzer der Anwendung.|
|50029|Ungültiger URI: Domänenname enthält ungültige Zeichen. Wenden Sie sich an den Administrator des Mandanten.|
|50034|Der Benutzer ist nicht im Verzeichnis vorhanden. Wenden Sie sich an den Administrator des Mandanten.|
|50042|Der Salt-Wert, der für das Generieren eines paarweisen Bezeichners erforderlich ist, fehlt im Prinzipal. Wenden Sie sich an den Administrator des Mandanten.|
|50048|Antragsteller stimmt nicht mit dem Ausstelleranspruch in der Clientassertion überein. Wenden Sie sich an den Administrator des Mandanten.|
|50050|Die Anforderung ist falsch formatiert. Wenden Sie sich an den Besitzer der Anwendung.|
|50053|Das Konto ist gesperrt, weil der Benutzer zu häufig versucht hat, sich mit einer falschen Benutzer-ID bzw. einem falschen Kennwort anzumelden.|
|50055|Ungültiges Kennwort. Es wurde ein abgelaufenes Kennwort eingegeben.|
|50056|Ungültiges oder NULL-Kennwort: Das Kennwort ist für diesen Benutzer nicht im Speicher vorhanden.|
|50057|Das Benutzerkonto ist deaktiviert. Das Konto wurde von einem Administrator deaktiviert.|
|50058|Die Anwendung hat versucht, eine automatische Anmeldung durchzuführen, und der Benutzer konnte nicht automatisch angemeldet werden. Die Anwendung muss einen interaktiven Datenfluss starten, um für Benutzer eine Option zum Anmelden bereitzustellen. Wenden Sie sich an den Anwendungsbesitzer.|
|50059|Der Benutzer ist nicht im Verzeichnis vorhanden. Wenden Sie sich an den Administrator des Mandanten.|
|50061|Die Abmeldeanforderung ist ungültig. Wenden Sie sich an den Besitzer der Anwendung.|
|50072|Benutzer müssen sich für die zweistufige Authentifizierung registrieren (interaktiv).|
|50074|Der Benutzer hat die MFA-Überprüfung nicht bestanden.|
|50076|Der Benutzer hat die MFA-Überprüfung nicht bestanden (nicht interaktiv).|
|50079|Benutzer müssen sich für die zweistufige Authentifizierung registrieren (nicht interaktive Anmeldungen).|
|50085|Für Aktualisierungstoken ist eine IdP-Anmeldung per sozialem Netzwerk erforderlich. Bitten Sie Benutzer, sich mit ihrem Benutzernamen und ihrem Kennwort erneut anzumelden.|
|50089|Flow-Token abgelaufen: Fehler bei der Authentifizierung. Bitten Sie Benutzer, sich mit ihrem Benutzernamen und ihrem Kennwort erneut anzumelden.|
|50097|Geräteauthentifizierung erforderlich. Ursache könnte sein, dass die DeviceId- oder DeviceAltSecId-Ansprüche **null** sind oder kein Gerät vorhanden ist, das zum Gerätebezeichner passt.|
|50099|JWT-Signatur ist ungültig. Wenden Sie sich an den Besitzer der Anwendung.|
|50105|Der angemeldete Benutzer ist keiner Rolle für die angemeldete Anwendung zugewiesen. Weisen Sie den Benutzer der Anwendung zu. Weitere Informationen: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|Das angeforderte Verbundbereichsobjekt ist nicht vorhanden. Wenden Sie sich an den Administrator des Mandanten.|
|50120|Es besteht ein Problem mit dem JWT-Header. Wenden Sie sich an den Administrator des Mandanten.|
|50124|Die Anspruchstransformation enthält ungültige Eingabeparameter. Wenden Sie sich an den Administrator des Mandanten, um die Richtlinie zu aktualisieren.|
|50125|Die Anmeldung wurde aufgrund des Zurücksetzens des Kennworts oder eines Kennwortregistrierungseintrags unterbrochen.|
|50126|Ungültiger Benutzername bzw. ungültiges Kennwort oder ungültiger lokaler Benutzername bzw. ungültiges lokales Kennwort.|
|50127|Der Benutzer muss für den Zugriff auf diesen Inhalt eine Brokeranwendung installieren.|
|50128|Ungültiger Domänenname: Es wurden keine Informationen zur Identifizierung des Mandanten in der Anforderung gefunden bzw. nicht über angegebene Anmeldeinformationen impliziert.|
|50129|Das Gerät ist nicht mit dem Arbeitsbereich verbunden: Um das Gerät zu registrieren, ist ein **Workplace Join** erforderlich.|
|50130|Der Anspruchswert kann nicht als bekannte Authentifizierungsmethode interpretiert werden.|
|50131|Wird für verschiedene Fehler in Bezug auf den bedingten Zugriff verwendet. Beispiel: Fehlerhafter Windows-Gerätestatus, Blockierung der Anforderung aufgrund von verdächtigen Aktivitäten und Zugriffs- und Sicherheitsrichtlinien-Entscheidungen.|
|50132|Die Anmeldeinformationen wurden aus den folgenden Gründen widerrufen:<ul><li>Das SSO-Artefakt ist ungültig oder abgelaufen.</li><li>Die Sitzung ist für die Anwendung nicht aktuell genug.</li><li>Eine Anforderung zur automatischen Anmeldung wurde gesendet, aber die Sitzung des Benutzers für Azure AD ist ungültig oder abgelaufen.</li></ul>|
|50133|Die Sitzung ist ungültig, weil das Kennwort abgelaufen ist oder vor Kurzem geändert wurde.|
|50135|Aufgrund eines Risikos, das für das Konto besteht, ist eine Kennwortänderung erforderlich.|
|50136|Umleitung der MSA-Sitzung an die Anwendung: Einzelne MSA-Sitzung erkannt. |
|50140|Dieser Fehler ist aufgetreten, weil beim Anmelden des Benutzers der Interrupt-Vorgang „Angemeldet bleiben“ aufgetreten ist. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md) mit der Korrelations-ID, Anforderungs-ID und dem Fehlercode, um weitere Details anzuzeigen. |
|50143|Sitzungskonflikt: Die Sitzung ist ungültig, weil der Benutzermandant aufgrund einer anderen Ressource nicht mit dem Domänenhinweis übereinstimmt.  [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md) mit Korrelations-ID, Anforderungs-ID und Fehlercode, um weitere Informationen dazu zu erhalten.|
|50144|Das Active Directory-Kennwort des Benutzers ist abgelaufen. Generieren Sie ein neues Kennwort für den Benutzer, oder bitten Sie ihn, das Self-Service-Tool für die Zurücksetzung zu nutzen.|
|50146|Diese Anwendung muss mit einem anwendungsspezifischen Signaturschlüssel konfiguriert werden. Entweder ist dies nicht der Fall, oder der Schlüssel ist abgelaufen oder noch nicht gültig. Wenden Sie sich an den Besitzer der Anwendung.|
|50148|Der „code_verifier“ stimmt in der Autorisierungsanforderung für PKCE nicht mit dem „code_challenge“-Element überein. Wenden Sie sich an den Anwendungsentwickler. |
|50155|Fehler bei der Geräteauthentifizierung für diesen Benutzer.|
|50158|Externe Sicherheitsabfrage wurde nicht erfüllt.|
|50161|Vom externen Anbieter gesendete Ansprüche sind nicht ausreichend, oder vom externen Anbieter angeforderter Anspruch fehlt.|
|50166|Fehler beim Senden der Anforderung an den Anspruchsanbieter.|
|50169|Der Bereich ist kein konfigurierter Bereich des aktuellen Dienstnamespace.|
|50172|Der externe Anspruchsanbieter ist nicht genehmigt. Wenden Sie sich an den Administrator des Mandanten.|
|50173|Aktuelles Autorisierungstoken erforderlich. Bitten Sie den Benutzer, sich mit aktuellen Anmeldeinformationen erneut anzumelden.|
|50177|Die externe Überprüfung wird für Passthroughbenutzer nicht unterstützt.|
|50178|Die Sitzungssteuerung wird für Passthroughbenutzer nicht unterstützt.|
|50180|Integrierte Windows-Authentifizierung ist erforderlich. Aktivieren Sie den Mandanten für das nahtlose einmalige Anmelden.|
|50181|OTP-bezogener Fehler bei der Anmeldung. |
|50201|Diese Meldung wird dem Benutzer während der Anmeldung angezeigt, wenn dem Benutzer zusätzliche Informationen bereitgestellt werden müssen.|
|51001|Domänenhinweis ist für lokale Sicherheits-ID nicht vorhanden – Lokaler UPN.|
|51004|Benutzerkonto ist nicht im Verzeichnis vorhanden.|
|51006|Integrierte Windows-Authentifizierung ist erforderlich. Angemeldeter Benutzer nutzt ein Sitzungstoken, dem der Anspruch der integrierten Windows-Authentifizierung fehlt. Bitten Sie den Benutzer, die Anmeldung erneut durchzuführen.|
|52004|Der Benutzer hat seine Zustimmung für den Zugriff auf LinkedIn-Ressourcen nicht gegeben. |
|53000|Die Richtlinie für bedingten Zugriff erfordert ein konformes Gerät, und das Gerät ist nicht konform. Bitten Sie den Benutzer, das Gerät mit einem genehmigten MDM-Anbieter, z.B. Intune, zu registrieren.|
|53001|Für die Richtlinie für bedingten Zugriff ist ein in die Domäne eingebundenes Gerät erforderlich, und das Gerät ist nicht in eine Domäne eingebunden. Bitten Sie den Benutzer, ein in die Domäne eingebundenes Gerät zu nutzen.|
|53002|Die genutzte Anwendung ist keine genehmigte Anwendung für bedingten Zugriff. Der Benutzer muss eine der Apps aus der Liste mit den genehmigten Anwendungen nutzen, um Zugriff zu erhalten.|
|53003|Der Zugriff wurde aufgrund von Richtlinien für bedingten Zugriff blockiert.|
|53004|Der Benutzer muss den Registrierungsprozess für die mehrstufige Authentifizierung durchführen, bevor er auf diesen Inhalt zugreifen kann. Benutzer sollten sich für die mehrstufige Authentifizierung registrieren.|
|65001|Anwendung X ist nicht berechtigt, auf Anwendung Y zuzugreifen, oder die Berechtigung wurde widerrufen. Oder: Der Benutzer oder Administrator hat der Verwendung der Anwendung mit ID X nicht zugestimmt. Senden Sie eine interaktive Autorisierungsanforderung für diesen Benutzer und diese Ressource. Oder: Der Benutzer oder Administrator hat nicht zugestimmt, die Anwendung mit ID X zu verwenden. Senden Sie eine Autorisierungsanforderung an Ihren Mandantenadministrator, damit dieser im Namen der App Y für Ressource Z fungiert.|
|65004|Der Benutzer hat seine Zustimmung für den Zugriff auf die App abgelehnt. Bitten Sie den Benutzer, die Anmeldung zu wiederholen und die Zustimmung für die App zu erteilen.|
|65005|Die für die Anwendung erforderliche Liste für den Ressourcenzugriff enthält keine Anwendungen, die von der Ressource ermittelt werden können, die Clientanwendung hat den Zugriff auf eine Ressource angefordert, die nicht in der erforderlichen Liste für den Ressourcenzugriff angegeben ist, oder der Graph-Dienst hat „Fehlerhafte Anforderung“ oder „Ressource nicht gefunden“ zurückgegeben. Wenn die Anwendung SAML unterstützt, haben Sie die Anwendung ggf. mit dem falschen Bezeichner (Entität) konfiguriert. Probieren Sie es mit der Lösung, die unter dem folgenden Link für SAML angegeben ist: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)|
|70000|Ungültige Erteilung aus den folgenden Gründen:<ul><li>Die angeforderte SAML 2.0-Assertion verfügt über eine ungültige Methode zur Antragstellerbestätigung.</li><li>OnBehalfOf-App-Datenfluss wird für V2 nicht unterstützt.</li><li>Das primäre Aktualisierungstoken wurde nicht mit dem Sitzungsschlüssel signiert.</li><li>Ungültiges externes Aktualisierungstoken.</li><li>Die Zugriffsberechtigung wurde für einen anderen Mandanten abgerufen.</li></ul>|
|70001|Die Anwendung mit dem Namen X wurde im Mandanten mit dem Namen Y nicht gefunden. Dies kann auftreten, wenn die Anwendung mit dem Bezeichner X nicht vom Administrator des Mandanten installiert wurde oder wenn sie von den Benutzern des Mandanten keine Zustimmung erhalten hat. Unter Umständen haben Sie den Bezeichnerwert für die Anwendung falsch konfiguriert oder die Authentifizierungsanforderung an den falschen Mandanten gesendet.|
|70002|Die Anwendung hat ungültige Clientanmeldeinformationen zurückgegeben. Wenden Sie sich an den Besitzer der Anwendung.|
|70003|Die Anwendung hat einen nicht unterstützten Gewährungstyp zurückgegeben. Wenden Sie sich an den Besitzer der Anwendung.|
|70004|Die Anwendung hat einen ungültigen Umleitungs-URI zurückgegeben. Die vom Client angegebene Umleitungsadresse stimmt nicht mit den konfigurierten Adressen oder anderen Adressen in der Liste mit den OIDC-Genehmigungen überein. Wenden Sie sich an den Besitzer der Anwendung.|
|70005|Die Anwendung hat aus den folgenden Gründen einen nicht unterstützten Antworttyp zurückgegeben:<ul><li>Der Antworttyp „token“ ist für die Anwendung nicht aktiviert.</li><li>Für den Antworttyp „id_token“ ist der Bereich „OpenID“ erforderlich: - Enthält einen nicht unterstützten OAuth-Parameterwert im codierten wctx-Element.</li></ul>Wenden Sie sich an den Besitzer der Anwendung.|
|70007|Die Anwendung hat beim Anfordern eines Tokens den nicht unterstützten Wert „response_mode“ zurückgegeben. Wenden Sie sich an den Besitzer der Anwendung.|
|70008|Der bereitgestellte Autorisierungscode oder das Aktualisierungstoken ist abgelaufen oder wurde widerrufen. Bitten Sie den Benutzer, die Anmeldung noch einmal durchzuführen.|
|70011|Der von der Anwendung angeforderte Bereich ist ungültig. Wenden Sie sich an den Besitzer der Anwendung.|
|70012|Beim Authentifizieren eines MSA-Benutzers (Consumer) ist ein Serverfehler aufgetreten. Wiederholen Sie die Anmeldung. Wenn das Problem weiterhin besteht, [öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md) .|
|70018|Ungültiger Überprüfungscode, weil der Benutzer den falschen Benutzercode für den Gerätecode-Datenfluss eingegeben hat. Die Autorisierung wird nicht genehmigt.|
|70019|Der Überprüfungscode ist abgelaufen. Bitten Sie den Benutzer, die Anmeldung erneut durchzuführen.|
|70037|Falsche Überprüfungsantwort angegeben. Die Remote-Authentifizierungssitzung wurde verweigert.|
|70043|Die Sitzungsverwaltung für bedingten Zugriff in Azure erzwingt das Ablaufen der Sitzung.|
|70044|Die Sitzungsverwaltung für bedingten Zugriff in Azure erzwingt das Ablaufen der Sitzung.|
|75001|Während der SAML-Nachrichtenbindung ist ein Fehler aufgetreten.|
|75003|Die Anwendung hat einen Fehler zu einer nicht unterstützten Bindung zurückgegeben (SAML-Protokollantwort kann nicht über andere Bindungen als HTTP POST gesendet werden). Wenden Sie sich an den Besitzer der Anwendung.|
|75005|Azure AD unterstützt die von der Anwendung für einmaliges Anmelden gesendete SAML-Anforderung nicht. Wenden Sie sich an den Besitzer der Anwendung.|
|75008|Die Anforderung von der Anwendung wurde abgelehnt, da die SAML-Anforderung über ein unerwartetes Ziel verfügt. Wenden Sie sich an den Besitzer der Anwendung.|
|75011|Die Authentifizierungsmethode, mit der sich der Benutzer beim Dienst authentifiziert hat, stimmt nicht mit der angeforderten Authentifizierungsmethode überein. Wenden Sie sich an den Besitzer der Anwendung.|
|75016|Die SAML2-Authentifizierungsanforderung verfügt über eine ungültige NameIdPolicy. Wenden Sie sich an den Besitzer der Anwendung.|
|80001|Der Authentifizierungs-Agent kann keine Verbindung mit Active Directory herstellen. Stellen Sie sicher, dass der Authentifizierungs-Agent auf einem in die Domäne eingebundenen Computer mit „Sichtverbindung“ zu einem Domänencontroller installiert ist, über den die Anmeldeanforderung des Benutzers bereitgestellt werden kann.|
|80002|Interner Fehler. Für die Anforderung zur Kennwortüberprüfung ist ein Timeout aufgetreten. Es war nicht möglich, die Authentifizierungsanforderung an den internen Hybrid-Identitätsdienst zu senden. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md), um weitere Details zum Fehler zu erhalten.|
|80003|Der Authentifizierungs-Agent hat eine ungültige Antwort erhalten. Beim Versuch, die Authentifizierung über die lokale Active Directory-Instanz durchzuführen, ist ein unbekannter Fehler aufgetreten. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md), um weitere Details zum Fehler zu erhalten.|
|80005|Authentifizierungs-Agent: Beim Verarbeiten der Antwort vom Authentifizierungs-Agent ist ein unbekannter Fehler aufgetreten. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md), um weitere Details zum Fehler zu erhalten.|
|80007|Der Authentifizierungs-Agent kann das Kennwort des Benutzers nicht überprüfen.|
|80010|Der Authentifizierungs-Agent kann das Kennwort nicht entschlüsseln. |
|80011|Der Authentifizierungs-Agent kann den Verschlüsselungsschlüssel nicht abrufen.|
|80012|Die Benutzer haben versucht, sich außerhalb der zulässigen Zeiten (in AD angegeben) anzumelden.|
|80013|Der Authentifizierungsversuch konnte nicht abgeschlossen werden, da zwischen dem Computer, auf dem der Authentifizierungs-Agent ausgeführt wird, und AD eine Zeitabweichung besteht. Beheben Sie die Probleme mit der Zeitsynchronisierung.|
|80014|Für den Authentifizierungs-Agent ist ein Timeout aufgetreten. [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md) mit dem Fehlercode, der Korrelations-ID und dem Datetime-Wert, um weitere Details zu diesem Fehler zu erhalten.|
|81001|Das Kerberos-Ticket des Benutzers ist zu groß. Dies kann passieren, wenn der Benutzer zu vielen Gruppen angehört und das Kerberos-Ticket aus diesem Grund zu viele Gruppenmitgliedschaften enthält. Reduzieren Sie die Gruppenmitgliedschaften des Benutzers, und versuchen Sie es erneut.|
|81005|Das Authentifizierungspaket wird nicht unterstützt.|
|81007|Der Mandant ist nicht für das nahtlose einmalige Anmelden aktiviert.|
|81012|Dies ist keine Fehlerbedingung. Es wird darauf hingewiesen, dass der Benutzer, der versucht, sich an Azure AD anzumelden, sich von dem Benutzer, der am Gerät angemeldet ist, unterscheidet. Sie können diesen Code in den Protokollen ignorieren.|
|90010|Die Anforderung wird aus verschiedenen Gründen nicht unterstützt. Die Anforderung wird z.B. mit einer nicht unterstützten Anforderungsmethode ausgeführt (nur die POST-Methode wird unterstützt), oder der Tokensignaturalgorithmus, der angefordert wurde, wird nicht unterstützt. Wenden Sie sich an den Anwendungsentwickler.|
|90014| Ein Pflichtfeld für eine Protokollmeldung fehlt. Wenden Sie sich an den Besitzer der Anwendung. Wenn Sie der Besitzer der Anwendung sind, stellen Sie sicher, dass Sie über alle erforderlichen Parameter für die Anmeldeanforderung verfügen. |
|90051| Invalid Delegation Token. Invalid national Cloud ID ({cloudId}) is specified. (Das Delegierungstoken ist ungültig. Eine ungültige nationale Cloud-ID ({cloudId}) wurde angegeben.)|
|90072| Das Konto muss zunächst als externer Benutzer im Mandanten hinzugefügt werden. Melden Sie sich ab, und melden Sie sich mit einem anderen Azure AD-Konto erneut an.|
|90094| Für die Gewährung sind Administratorberechtigungen erforderlich. Bitten Sie den Mandantenadministrator, Zustimmung für diese Anwendung bereitzustellen.|
|500021|Mandant wird durch den Unternehmensproxy eingeschränkt. Ressourcenzugriff wird verweigert.|
|500121| Fehler bei der Authentifizierung während starker Authentifizierungsanforderung.|
|500133| Die Assertion befindet sich nicht im gültigen Zeitbereich. Überprüfen Sie, ob das Zugriffstoken abgelaufen ist, bevor Sie es für die Benutzerassertion verwenden oder ein neues Token anfordern.|
|530021|Die Anwendung erfüllt nicht die Anforderungen für den bedingten Zugriff.|
|530032|Blockiert durch eine Sicherheitsrichtlinie.| 
|700016|Die Anwendung mit dem Bezeichner „{appIdentifier}“ wurde im Verzeichnis „{tenantName}“ nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde oder wenn sie von den Benutzern des Mandanten keine Zustimmung erhalten hat. Unter Umständen haben Sie Ihre Authentifizierungsanforderung an den falschen Mandanten gesendet.|
|900432|Vertrauliche Clients werden in einer cloudübergreifenden Anforderung nicht unterstützt.|
|7000218|The request body must contain the following parameter: 'client_assertion' or 'client_secret'. (Der Anforderungstext muss den folgenden Parameter enthalten: 'client_assertion' oder 'client_secret'.)|


## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Berichte zu Anmeldungen](concept-sign-ins.md)
* [Programmgesteuerter Zugriff auf Azure AD-Berichte](concept-reporting-api.md)
