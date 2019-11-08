---
title: Eine Welt ohne Kennwörter – Grundlegendes zu Azure Active Directory | Microsoft-Dokumentation
description: Dieser Leitfaden dient als Hilfe für CEOs, CIOs, CISOs, Chief Identity Architects, Enterprise Architects sowie Sicherheits- und IT-Entscheidungsträgern, die für die Auswahl einer kennwortlosen Authentifizierungsmethode für die Azure Active Directory-Implementierung verantwortlich sind.
keywords: kennwortlos, Azure AD
author: martincoetzer
ms.author: martinco
ms.date: 07/09/2019
ms.topic: article
ms.service: security
ms.subservice: security-fundamentals
ms.workload: identity
ms.openlocfilehash: 0fbd8404e326ecc6fabef8c89991009809b1448b
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820508"
---
# <a name="a-world-without-passwords-with-azure-active-directory"></a>Eine Welt ohne Kennwörter – mit Azure Active Directory

Kennwörter sind nicht mehr zeitgemäß. In der Vergangenheit haben uns Kennwörter gute Dienste erwiesen. In der heutigen digitalen Arbeitswelt werden sie jedoch von Hackern häufig als relativ einfacher Angriffsvektor genutzt. Hacker bevorzugen Kennwörter – nachvollziehbar, wenn man bedenkt, dass Kennwörter häufig das Jahr, den Monat, die Jahreszeit oder den Namen einer lokalen Sportmannschaft enthalten – Begriffe, die in Azure Active Directory (Azure AD) deshalb gar nicht erst akzeptiert werden. Darüber hinaus haben [Forschungen](https://aka.ms/passwordguidance) gezeigt, dass die üblichen Empfehlungen für die Kennwortverwaltung wie etwa zur Längen- und Komplexitätsanforderung sowie zur Änderungshäufigkeit in vielerlei Hinsicht aufgrund des menschlichen Faktors sogar kontraproduktiv sind.

Zu den drei häufigsten Methoden zur Gefährdung von Benutzerkonten zählen Kennwort-Spraying, Phishing und Breach Replay. Azure AD-Features, wie z. B. [Smart Lockout](../../active-directory/authentication/howto-password-smart-lockout.md), [Gesperrte Kennwörter](../../active-directory/authentication/concept-password-ban-bad-on-premises.md) und [Kennwortschutz](../../active-directory/authentication/concept-password-ban-bad-on-premises.md), können gegen diese Art von Angriffen schützen. Ebenso bietet die Implementierung von [Multi-Factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (MFA) oder der zweistufigen Authentifizierung zusätzliche Sicherheit, da eine zweite Form der Authentifizierung erforderlich ist. Auf lange Sicht ist eine kennwortlose Methode jedoch die beste Lösung, um eine sichere Authentifizierung zu gewährleisten.

Dieser Artikel beschreibt Grundlegendes zur Funktion und Implementierung der kennwortlosen Lösungen von Microsoft und bietet Hilfestellung bei der Auswahl der folgenden Optionen:

* **Windows Hello for Business**. In Windows 10 können Sie über Windows Hello for Business Kennwörter durch eine sichere zweistufige Authentifizierung auf PCs und mobilen Geräten ersetzen. Diese Authentifizierung besteht aus einer neuen Form von Anmeldeinformationen, die an ein Gerät gebunden sind und biometrische Daten oder eine PIN verwenden.

* **Anmelden ohne Kennwort mit Microsoft Authenticator**. Mit der Microsoft Authenticator-App können sich Benutzer ohne Kennwort bei einem Azure AD-Konto anmelden. Ähnlich wie die Technologie von Windows Hello for Business nutzt Microsoft Authenticator die schlüsselbasierte Authentifizierung, um die Verwendung von Benutzeranmeldeinformationen zu ermöglichen, die an ein Gerät gebunden sind und auf biometrischen Daten oder einer PIN beruhen.

* **FIDO2-Sicherheitsschlüssel**. FIDO2 bietet kryptografische Anmeldeinformationen, die für jede Website eindeutig sind und auf einem lokalen Gerät wie Windows Hello oder in externen Sicherheitsschlüsseln gespeichert sind. Diese Sicherheitsschlüssel sind Phishing, Kennwortdiebstahl und Replay-Angriffen gegenüber resistent. In Kombination mit der Benutzerverifizierung mittels biometrischem Merkmal oder PIN bietet die Lösung eine zweistufige Überprüfung, die den aktuellen Sicherheitsanforderungen entspricht.

## <a name="the-future-of-passwordless-authentication"></a>Die Zukunft der kennwortlosen Authentifizierung

Heutzutage schützen Banken, Kreditkartenunternehmen und andere Organisationen sowie Onlinedienste die Konten ihrer Kunden, indem sie deren Identität zweimal überprüfen: einmal mit dem Kennwort und ein zweites Mal per Telefon, Textnachricht oder App. Die mehrstufige Authentifizierung verhindert zwar, dass Kennwörter geteilt, gestohlen oder erraten werden können, sie bietet jedoch keine Lösung für den lästigen Umstand, dass man sich die Kennwörter merken muss. Im heutigen Cloudzeitalter wollen Benutzer und Unternehmen kennwortlose Authentifizierungsmethoden, die maximale Sicherheit bieten *und* benutzerfreundlich sind.

![Komfort versus Sicherheit](./media/ad-passwordless/azure-ad-pwdless-image1.png)

Windows Hello for Business, Microsoft Authenticator für die Anmeldung ohne Kennwort und FIDO2-Sicherheitsschlüssel nutzen eine einfache, gemeinsame Architektur und bieten Benutzern eine Authentifizierungsmethode, die sowohl extrem sicher als auch benutzerfreundlich ist. Alle drei Konzepte basieren auf der Public/-Private-Key-Technologie. Sie erfordern eine lokale Geste, z. B. biometrische Daten oder eine PIN, und private Schlüssel, die an ein einzelnes Gerät gebunden sind, sicher gespeichert sind und nicht an andere weitergegeben werden.

## <a name="windows-hello-for-business"></a>Windows Hello for Business

In Windows 10 können Sie über Windows Hello for Business Kennwörter durch eine sichere zweistufige Authentifizierung auf PCs und Geräten ersetzen. Die Authentifizierung besteht aus einer neuen Form von Anmeldeinformationen, die an ein Gerät gebunden sind und biometrische Daten oder eine PIN verwenden, mit denen sich der Benutzer sowohl bei Azure AD als auch bei einem lokalen Active Directory authentifizieren kann. Die Anmeldung bei einem Gerät mit Windows Hello for Business ist einfach. Sie geben entweder eine PIN ein oder nutzen eine biometrische Geste wie z. B. Ihren Fingerabdruck oder die Gesichtserkennung.

### <a name="windows-hello-for-business-scenarios"></a>Windows Hello for Business: Szenarios

Windows Hello for Business eignet sich ideal für Information-Worker, die über einen eigenen Windows-PC verfügen. Die biometrischen Daten und die Anmeldeinformationen sind direkt mit dem PC des Benutzers verknüpft, wodurch verhindert wird, dass kein anderer außer dem Eigentümer Zugriff hat. Mit PKI-Integration und integrierter Unterstützung für einmaliges Anmelden (SSO) bietet Windows Hello for Business eine einfache und praktische Methode für den nahtlosen Zugriff auf Unternehmensressourcen lokal und in der Cloud.

### <a name="windows-hello-for-business-deployment-considerations"></a>Windows Hello for Business: Überlegungen zur Bereitstellung

Windows Hello for Business ist ein verteiltes System, das mehrere Komponenten für die Registrierung, Bereitstellung und Authentifizierung von Geräten verwendet. Aus diesem Grund muss eine Bereitstellung über mehrere Teams innerhalb der Organisation sorgfältig geplant werden. Das [Planungshandbuch](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) für Windows Hello for Business kann verwendet werden, um die Auswahl der Art der Bereitstellung von Windows Hello for Business und die zu berücksichtigenden Optionen zu erleichtern.

Für die Bereitstellung von Windows Hello for Business stehen zahlreiche Optionen zur Verfügung. Dank der verschiedenen Optionen kann nahezu jede Organisation Windows Hello for Business bereitstellen. Folgende Arten von Bereitstellungen werden unterstützt:

* [Hybridbereitstellung mit schlüsselbasiertem Vertrauensmodell für Azure AD](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-key-trust)

* [Hybridbereitstellung mit zertifikatsbasiertem Vertrauensmodell für Azure AD](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust)

* [Bereitstellungshandbücher zu Single Sign-On für Azure AD-Einbindung](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso)

* [Lokale Bereitstellung mit schlüsselbasiertem Vertrauensmodell](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-key-trust)

* [Lokale Bereitstellung mit zertifikatsbasiertem Vertrauensmodell](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-cert-trust)

Die Vielzahl der Optionen lässt die Bereitstellung komplex erscheinen. Die meisten Organisationen werden jedoch wahrscheinlich feststellen, dass sie den größten Teil der Infrastruktur, die für die Windows Hello for Business-Bereitstellung erforderlich ist, bereits implementiert haben. Unabhängig davon ist es wichtig zu verstehen, dass Windows Hello for Business ein verteiltes System ist, dessen Einführung sorgfältig geplant werden sollte.

Es empfiehlt sich, den Artikel [Planen einer Bereitstellung von Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) zu lesen, um die Entscheidung zu erleichtern, welches Bereitstellungsmodell für Ihre Organisation am besten geeignet ist. Lesen Sie dann, basierend auf Ihrer Planung, das [Windows Hello for Business-Bereitstellungshandbuch](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide), um eine erfolgreiche Bereitstellung von Windows Hello for Business in Ihrer vorhandenen Umgebung sicherzustellen.

### <a name="how-windows-hello-for-business-works"></a>So funktioniert Windows Hello for Business

#### <a name="user-sets-up-windows-hello-for-business"></a>Benutzer richtet Windows Hello for Business ein

Nach einer ersten zweistufigen Überprüfung des Benutzers während der Registrierung wird Windows Hello auf dem Gerät des Benutzers eingerichtet. Der Benutzer wird dazu aufgefordert, eine Geste festzulegen, die biometrisch sein kann, wie beispielsweise ein Fingerabdruck oder eine Gesichtserkennung. Es ist auch möglich, eine PIN festzulegen. Nach der Festlegung gibt der Benutzer die Geste zum Überprüfen der Identität an. Windows verwendet nun Windows Hello zur Authentifizierung des Benutzers.

Basierend auf den Funktionen Ihres Windows 10-Geräts verfügen Sie entweder über eine integrierte Secure Enclave-Instanz, auch als Hardware Trusted Platform Module (TPM) bezeichnet, oder über ein Software-TPM. Das TPM speichert den privaten Schlüssel, der zum Entsperren entweder einen Gesichtsscan, einen Fingerabdruck oder eine PIN benötigt. Die biometrischen Daten werden nicht weitergegeben und nicht an externe Geräte oder Server gesendet. Da Windows Hello die biometrischen Identifikationsdaten nur auf dem Gerät speichert, gibt es für Angreifer keinen Zugriffspunkt auf die biometrische Daten.

> [!TIP]
> Oberflächlich betrachtet ähnelt eine PIN sehr einem Kennwort, sie ist jedoch deutlich sicherer. Ein wichtiger Unterschied zwischen einem Kennwort und einer PIN ist, dass die PIN an das Gerät gebunden ist, auf dem sie eingerichtet wurde. Eine Person, die Ihr Kennwort stiehlt, kann sich von überall bei Ihrem Konto anmelden. Wenn diese Person jedoch Ihre PIN stiehlt, muss sie auch das physische Gerät stehlen. Da eine PIN lokal auf dem Gerät gespeichert wird, wird sie nicht an einen anderen Ort übertragen, sodass sie nicht während einer Übertragung abgefangen oder vom Server gestohlen werden kann.

#### <a name="user-using-windows-hello-for-business-for-sign-in"></a>Benutzer meldet sich über Windows Hello for Business an

Bei der Windows Hello for Business-Authentifizierung mittels PIN und Biometrie wird eine asymmetrische Verschlüsselung (öffentlicher/privater Schlüssel) verwendet. Während der Authentifizierung ist die Verschlüsselung an den TLS-Sitzungsschlüssel gebunden, der den Authentifizierungsprozess sichert. So ist es nicht möglich, dass bei einem MITM-Angriff (Man-in-the-Middle) das generierte Sicherheitstoken bzw. das resultierende Artefakt gestohlen und von anderen Orten wiedergeben werden kann.

Windows Hello for Business ermöglicht eine bequeme Anmeldung und Authentifizierung des Benutzers für Azure AD- und Active Directory-Ressourcen. Mit Azure AD verknüpfte Geräte führen während der Anmeldung eine Authentifizierung für Azure und optional für Active Directory durch. In Azure Active Directory eingebundene Hybridgeräte führen während der Anmeldung eine Authentifizierung für Active Directory und im Hintergrund für Azure Active Directory durch.

![Siehe Quelldatei](./media/ad-passwordless/azure-ad-pwdless-image2.jpeg)

Die folgenden Schritte veranschaulichen die Anmeldeauthentifizierung für Azure Active Directory.

![Windows 10-Sperrbildschirm](./media/ad-passwordless/azure-ad-pwdless-image3.png)

1. Benutzer meldet sich bei Windows mit biometrischer Geste oder PIN an. Die Geste entsperrt den privaten Schlüssel von Windows Hello for Business, und die Daten werden an den Security Support Provider für Cloudauthentifizierung, auch als Cloud AP-Anbieter bezeichnet, gesendet.

2. Der Cloud AP-Anbieter fordert eine Nonce vom Azure Active Directory an.

3. Azure AD gibt eine Nonce zurück, die 5 Minuten lang gültig ist.

4. Der Cloud AP-Anbieter signiert die Nonce mit dem privaten Schlüssel des Benutzers und gibt sie an Azure Active Directory zurück.

5. Azure Active Directory gleicht die signierte Nonce mithilfe des sicher registrierten öffentlichen Schlüssels des Benutzers mit der Nonce-Signatur ab. Nach der Verifizierung der Signatur überprüft Azure AD dann die zurückgegebene signierte Nonce. Nach der Überprüfung der Nonce erstellt Azure AD ein PRT mit Sitzungsschlüssel, das mit dem Transportschlüssel des Geräts verschlüsselt ist, und gibt es an den Cloud AP-Anbieter zurück.

6. Der Cloud-AP-Anbieter erhält das verschlüsselte PRT mit Sitzungsschlüssel. Mithilfe des privaten Transportschlüssels des Geräts entschlüsselt der Cloud AP-Anbieter den Sitzungsschlüssel und schützt den Sitzungsschlüssel mit dem TPM des Geräts.

7. Der Cloud AP-Anbieter gibt eine erfolgreiche Authentifizierungsantwort an Windows zurück. Der Benutzer kann nun sowohl auf Windows- als auch Cloud- und lokale Anwendungen zugreifen, ohne dass eine erneute Authentifizierung (SSO) erforderlich ist.

Eingehendere Informationen zum Authentifizierungsprozesses in anderen Szenarios im Zusammenhang mit Windows Hello for Business finden Sie unter [Windows Hello for Business und Authentifizierung](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-authentication#azure-ad-join-authentication-to-active-directory-using-a-key).

#### <a name="user-manages-their-windows-hello-for-business-credentials"></a>Benutzer verwaltet Windows Hello for Business-Anmeldeinformationen

Der [Microsoft-PIN-Zurücksetzungsdienst](/windows/security/identity-protection/hello-for-business/hello-feature-pin-reset) ist ein Feature in Azure AD, mit dem Benutzer ihre PIN bei Bedarf zurücksetzen können. Mithilfe einer Gruppenrichtlinie, Microsoft Intune oder einer kompatiblen MDM-Lösung kann ein Administrator Windows 10-Geräte für die sichere Verwendung des PIN-Zurücksetzungsdiensts von Microsoft konfigurieren. Dieser ermöglicht es Benutzern, ihre vergessene PIN über Einstellungen oder den Sperrbildschirm zurückzusetzen, ohne sich nochmals registrieren zu müssen.

Manchmal müssen Benutzer wieder auf die Verwendung von Kennwörtern zurückgreifen. Die [Self-Service-Kennwortzurücksetzung](../../active-directory/authentication/howto-sspr-deployment.md) (Self-Service Password Reset, SSPR) ist ein Feature, mit dem Benutzer ihre Kennwörter zurücksetzen können, ohne sich an IT-Mitarbeiter wenden zu müssen. Benutzer müssen sich für die Self-Service-Kennwortzurücksetzung registrieren, bevor sie den Dienst verwenden können. Während der Registrierung wählt der Benutzer eine oder mehrere Authentifizierungsmethoden aus, die von seiner Organisation aktiviert wurden. SSPR ermöglicht es Benutzern, die Blockierung schnell aufzuheben und die Arbeit fortzusetzen, unabhängig von Aufenthaltsort und Uhrzeit. Indem sie ihren Benutzern erlaubt, die Blockierung selbst aufzuheben, kann Ihre Organisation die unproduktive Zeit und die hohen Supportkosten für die meisten allgemeinen Probleme im Zusammenhang mit Kennwörtern verringern.

## <a name="passwordless-sign-in-with-microsoft-authenticator"></a>Anmelden ohne Kennwort mit Microsoft Authenticator

Microsoft Authenticator stellt eine weitere Methode dar, die für die kennwortlose Anmeldung bei Azure AD-Konten per Telefon verwendet werden kann. Sie müssen Ihre Identität trotzdem bestätigen, indem Sie eine Ihnen bekannte Information und einen in Ihrem Besitz befindlichen Gegenstand angeben. Bei der Anmeldung per Telefon können Sie jedoch das Eingeben Ihres Kennworts überspringen und die gesamte Identitätsüberprüfung auf Ihrem mobilen Gerät per Fingerabdruck, Gesichtserkennung oder PIN durchführen.

### <a name="microsoft-authenticator-passwordless-scenarios"></a>Anmeldeszenarios ohne Kennwort mit Microsoft Authenticator

Mit der Microsoft Authenticator-App können Benutzer ihre Identität überprüfen und sich für ihr Geschäfts- oder Privatkonto authentifizieren. Sie kann auch verwendet werden, um den Schutz eines Kennworts mit einer Einmalkennung oder Pushbenachrichtigung zu verstärken oder die Notwendigkeit eines Kennworts ganz zu ersetzen. Anstatt ein Kennwort zu verwenden, bestätigen die Benutzer ihre Identität über ihr Mobiltelefon durch Fingerabdruckscan, Gesichts- oder Iriserkennung oder PIN. Bei diesem Tool handelt es sich um eine einfache, benutzerfreundliche App für mobile Geräte, die auf einer sicheren Technologie basiert, die der ähnelt, die Windows Hello verwendet. Die Microsoft Authenticator-App ist für Android und iOS verfügbar.

### <a name="microsoft-authenticator-deployment-considerations"></a>Überlegungen zur Bereitstellung von Microsoft Authenticator

Voraussetzungen für die Verwendung der Microsoft Authenticator-App zur kennwortlosen Anmeldung bei Azure AD:

* Azure Multi-Factor Authentication ist für Endbenutzer aktiviert.

* Es wird empfohlen, dass Benutzer ihre Geräte mit Microsoft Intune oder einer Drittanbieterlösung für die Verwaltung mobiler Geräte (Mobile Device Management, MDM) registrieren, um die Bereitstellung der App zu vereinfachen.

Sind diese Voraussetzungen erfüllt, aktivieren die Administratoren die kennwortlose Anmeldung per Telefon im Mandanten mit [Windows PowerShell](../../active-directory/authentication/howto-authentication-passwordless-phone.md). Nach Aktivierung der Anmeldung per Telefon im Mandanten können sich Endbenutzer mit ihrem Telefon anmelden, indem sie ihr Geschäfts-, Schul- oder Unikonto auf dem Bildschirm **Konten** der App auswählen und dann **Anmeldung per Telefon aktivieren** auswählen.

Vorausgesetzt, die kennwortlose Anmeldung wurde durch einen Administrator aktiviert, müssen Endbenutzer noch folgende Anforderungen erfüllen:

* Für Azure Multi-Factor Authentication (MFA) registrieren

* Aktuelle Version von Microsoft Authenticator auf Geräten mit iOS 8.0 oder höher oder Android 6.0 oder höher installieren

* Geschäfts-, Schul- oder Unikonto mit Pushbenachrichtigungen zur App hinzufügen

Es wird empfohlen, die [Kontoanmeldeinformationen](../../active-directory/user-help/user-help-auth-app-backup-recovery.md) in der Cloud mit Microsoft Authenticator zu sichern, um zu vermeiden, dass Ihr Konto gesperrt wird oder Sie Konten auf einem neuen Gerät neu erstellen müssen. Nach dem Sichern können Sie die App auch zum Wiederherstellen der Informationen auf einem neuen Gerät verwenden und dadurch möglicherweise vermeiden, dass Sie gesperrt werden oder Konten neu erstellen müssen.

Die meisten Benutzer sind daran gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass die Benutzer eine entsprechende Unterweisung für diesen Prozess erhalten. So lassen sich Anrufe beim Helpdesk aufgrund von [Problemen](../../active-directory/authentication/howto-authentication-passwordless-phone.md) im Zusammenhang mit der Anmeldung mit der Microsoft Authenticator-App reduzieren.

> [!NOTE]
> Bei dieser Methode kann möglicherweise ein Point of Failure entstehen, wenn sich ein Roamingbenutzer an einem Ort ohne Internetverbindung befindet. Für FIDO2-Sicherheitsschlüssel und Windows Hello for Business gilt nicht die gleiche Einschränkung.

### <a name="how-passwordless-sign-in-with-microsoft-authenticator-works"></a>So funktioniert das Anmelden ohne Kennwort mit Microsoft Authenticator

#### <a name="user-sets-up-passwordless-sign-in-with-microsoft-authenticator"></a>Benutzer richtet die kennwortlose Anmeldung mit Microsoft Authenticator ein

Bevor die Microsoft Authenticator-App als kennwortlose Methode zur Anmeldung bei einem Azure AD-Konto verwendet werden kann, muss sowohl ein Administrator als auch der Endbenutzer bestimmte Schritte ausführen.

Zuerst muss ein Administrator die [Verwendung der App als Anmeldeinformation](../../active-directory/authentication/howto-authentication-passwordless-phone.md) im Mandanten mit Windows PowerShell aktivieren. Der Administrator muss zudem die Endbenutzer für Azure Multi-Factor Authentication (Azure MFA) aktivieren und die Microsoft Authenticator-App als eine der [Überprüfungsmethoden](../../active-directory/authentication/howto-mfa-mfasettings.md#verification-methods) konfigurieren.

Endbenutzer müssen die Microsoft Authenticator-App [herunterladen und installieren](../../active-directory/user-help/user-help-auth-app-download-install.md) und [ihr Konto einrichten](../../active-directory/user-help/security-info-setup-auth-app.md), um die Microsoft Authenticator-App als Überprüfungsmethode verwenden zu können.

> [!VIDEO https://www.youtube.com/embed/uWbkLuI4g30]

#### <a name="user-using-microsoft-authenticator-for-passwordless-sign-in"></a>Benutzer verwendet Microsoft Authenticator für die kennwortlose Anmeldung

Mit der Microsoft Authenticator-App können sich Benutzer bei jedem beliebigen Azure AD-Konto anmelden, ohne ein Kennwort zu verwenden. Der Windows 10-Sperrbildschirm führt die Microsoft Authenticator-App nicht als Anmeldeoption auf, Benutzer können jedoch trotzdem den Benutzernamen eingeben und sich eine Pushbenachrichtigung auf ihr mobiles Gerät schicken lassen, um die Anwesenheit zu bestätigen. Der Benutzer bestätigt seine Anwesenheit, indem er eine Nummer auf dem Anmeldebildschirm abgleicht und dann per Gesichtsscan, Fingerabdruck oder PIN den privaten Schlüssel entsperrt und die Authentifizierung durchführt. Diese mehrstufige Überprüfungsmethode ist sicherer als die Verwendung eines Kennworts und bequemer als die Eingabe eines Kennworts und eines Codes.

![Anmeldung mit Microsoft Authenticator](./media/ad-passwordless/azure-ad-pwdless-image4.png)

Die kennwortlose Authentifizierung mit Microsoft Authenticator erfolgt nach dem gleichen Grundmuster wie Windows Hello for Business, ist jedoch etwas komplexer, da der Benutzer identifiziert werden muss, damit Azure AD die verwendete Microsoft Authenticator-App-Version finden kann.

![Ablauf mit dem Microsoft Authenticator](./media/ad-passwordless/azure-ad-pwdless-image5.png)

1. Der Benutzer gibt seinen Benutzernamen ein.

2. Azure AD erkennt, dass der Benutzer über sichere Anmeldeinformationen verfügt, und startet den Strong Credential-Flow (Ablauf für sichere Anmeldeinformationen).

3. Über Apple Push Notification Service (APNS) (iOS) bzw. Firebase Cloud Messaging (FCM) (Android) wird eine Benachrichtigung an die App gesendet.

4. Der Benutzer erhält die Pushbenachrichtigung und öffnet die App.

5. Die App ruft Azure AD auf und erhält eine Proof-of-Presence-Abfrage sowie eine Nonce.

6. Der Benutzer beantwortet die Abfrage durch Eingabe von biometrischen Daten oder einer PIN, um den privaten Schlüssel zu entsperren.

7. Die Nonce wird mit dem privaten Schlüssel signiert und an Azure AD zurückgesendet.

8. Azure AD überprüft den öffentlichen/privaten Schlüssel und gibt ein Token zurück.

#### <a name="user-manages-their-passwordless-sign-in-with-microsoft-authenticator-credentials"></a>Benutzer meldet sich mit Microsoft Authenticator-Anmeldeinformationen ohne Verwendung eines Kennworts an

Mit der [kombinierten Registrierung](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md) können sich Benutzer registrieren und die Vorteile sowohl von Azure Multi-Factor Authentication als auch der Self-Service-Kennwortzurücksetzung nutzen. Benutzer registrieren und verwalten diese Einstellungen auf der [Seite „Mein Profil“](https://aka.ms/mysecurityinfo). Zusätzlich zur Aktivierung von SSPR unterstützt die kombinierte Registrierung mehrere Authentifizierungsmethoden und -aktionen.

## <a name="fido2-security-keys"></a>FIDO2-Sicherheitsschlüssel

FIDO2 ist die neueste Version des FIDO-Allianz-Standards und besteht aus zwei Komponenten – dem W3C-Standard (Webauthentifizierung, kurz: WebAuthn) und dem entsprechenden Client to Authenticator Protocol (CTAP2) der FIDO Allianz. Die FIDO2-Standards ermöglichen es Benutzern, Hardware-, mobile- und biometriebasierte Authentifikatoren zu nutzen, um sich problemlos für viele Anwendungen und Websites in mobilen und Desktop-Umgebungen zu authentifizieren.

Microsoft hat zusammen mit Branchenpartnern an FIDO2-Sicherheitsgeräten für Windows Hello gearbeitet, um eine einfache und sichere Authentifizierung auf gemeinsam genutzten Geräten zu ermöglichen. FIDO2-Sicherheitsschlüssel erlauben es Ihnen, Ihre Anmeldeinformationen bei sich zu tragen und sich sicher bei einem an [Azure AD](https://aka.ms/azuread418) angebundenen Windows 10-PC zu authentifizieren, der zu Ihrer Organisation gehört.

WebAuthN definiert eine API, die die Entwicklung und Implementierung einer sicheren, kennwortlosen Authentifizierung durch Web-Apps und -dienste ermöglicht. Das CTAP-Protokoll ermöglicht es externen Geräten wie z. B. FIDO-kompatiblen Sicherheitsschlüsseln, WebAuthN zu verwenden und als Authentifikatoren zu dienen. Mit der Webauthentifizierung können sich Benutzer per Gesichtserkennung, Fingerabdruck, PIN oder mobile FIDO2-Sicherheitsschlüssel bei Onlinediensten anmelden und dabei sichere Anmeldeinformationen mit öffentlichen Schlüsseln anstelle von Kennwortwörtern verwenden. Derzeit wird WebAuthN in Microsoft Edge unterstützt; die Unterstützung für Chrome und Firefox befindet sich in der Entwicklung.

Geräte und Token, die auf FIDO2-, WebAuthN- und CTAP-Protokollen beruhen, ermöglichen eine plattformübergreifende Lösung für die sichere Authentifizierung ohne Verwendung von Kennwörtern. Microsoft-Partner arbeiten an einer Vielzahl von Formfaktoren für Sicherheitsschlüssel, wie beispielsweise USB-Sicherheitsschlüssel und NFC-fähige Smartcards.

### <a name="fido2-security-keys-scenarios"></a>FIDO2-Sicherheitsschlüssel: Szenarios

FIDO2-Sicherheitsschlüssel können zur Anmeldung bei Azure AD verwendet werden. Der Sicherheitsschlüssel muss hierzu als Anmeldeinformationsanbieter auf dem Windows 10-Sperrbildschirm ausgewählt werden. Ein Benutzername oder Kennwort ist nicht erforderlich. Daher stellen sie die ideale Lösung für Firstline Worker dar, die sich PCs mit mehrere Benutzern teilen. Zudem sind sie eine ausgezeichnete Authentifizierungsoption, wenn Unternehmensrichtlinien vorschreiben, dass die Anmeldeinformationen eines Benutzers physisch von seinem Gerät getrennt sein müssen. Benutzer können sich mit ihren FIDO2-Sicherheitsschlüsseln auch bei Websites im Microsoft Edge-Browser unter Windows 10 Version 1809 oder höher anmelden.

### <a name="fido2-security-keys-deployment-considerations"></a>Überlegungen zur Bereitstellung von FIDO2-Sicherheitsschlüsseln

Administratoren können die FIDO2-Unterstützung in Azure AD aktivieren und die Funktion Benutzern oder Gruppen zuweisen. Richtlinien können auch erstellt werden, um die Art und Weise der Bereitstellung von Schlüsseln zu definieren und Einschränkungen, wie z. B. das Zulassen oder Blockieren bestimmter Hardwaresicherheitsschlüssel, zu konfigurieren. Die Schlüssel müssen physisch an die Endbenutzer verteilt werden.

**Für die Aktivierung der kennwortlosen Anmeldung bei Azure AD und bei Websites unter Verwendung von FIDO2-Sicherheitsschlüsseln sind folgende Anforderungen zu erfüllen:**

* Azure AD

* Azure Multi-Factor Authentication

* Vorschauversion der kombinierten Registrierung

* Für die Vorschau der FIDO2-Sicherheitsschlüssel ist ein kompatibler FIDO2-Sicherheitsschlüssel erforderlich

* Die Webauthentifizierung (WebAuthN) erfordert Microsoft Edge unter Windows 10 Version 1809 oder höher

* Die FIDO2-basierte Windows-Anmeldung erfordert ein in Azure AD eingebundenes Gerät mit Windows 10 Version 1809 oder höher (Windows 10 Version 1903 bietet die beste Benutzerfreundlichkeit)

Zu FIDO2-kompatiblen Formfaktoren zählen USB-, NFC- und Bluetooth-Geräte. Es empfiehlt sich, einen Formfaktor für Ihre spezifischen Anforderungen auszuwählen, da einige Plattformen und Browser noch nicht mit FIDO2 kompatibel sind.

Darüber hinaus sollte jede Organisation ein Protokoll erstellen, das die entsprechenden Schritte für Benutzer und Administratoren bei Verlust oder Diebstahl eines Sicherheitsschlüssels enthält. Der Verlust oder Diebstahl von Schlüsseln sollte vom Benutzer gemeldet werden, damit der Administrator oder der Benutzer den Sicherheitsschlüssel aus dem Profil des Benutzers löschen und einen neuen bereitstellen kann.

### <a name="how-fido2-security-keys-works"></a>So funktionieren FIDO2-Sicherheitsschlüssel

#### <a name="user-sets-up-fido2-security-key"></a>Benutzer richtet FIDO2-Sicherheitsschlüssel ein

Administratoren können [Schlüssel manuell bereitstellen](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless) und an Endbenutzer verteilen; die Bereitstellung und Aktivierung des FIDO2-Anmeldeinformationsanbieters auf dem Windows 10-Sperrbildschirm wird durch [Intune](https://docs.microsoft.com/intune/windows-enrollment-methods) unterstützt. Administratoren müssen außerdem das [Azure-Portal](https://portal.azure.com/) verwenden, um Hardwaretokengeräte als kennwortlose Authentifizierungsmethode zu aktivieren.

Für die Bereitstellung von FIDO2-Sicherheitsschlüsseln müssen die Benutzer ihre Schlüssel mit der [kombinierten Registrierung](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md) registrieren. Mit der kombinierten Registrierung registrieren sich Benutzer einmalig und profitieren von den Vorteilen sowohl von Azure Multi-Factor Authentication als auch von SSPR (Single Sign-On Password Reset).

Zusätzlich zur Auswahl des Hardwaretokens als Standardmethode für die mehrstufige Authentifizierung wird empfohlen, auch eine zusätzliche Überprüfungsoption auszuwählen.

* Microsoft Authenticator – Benachrichtigung

* Authenticator-App oder Hardwaretoken – Code

* Telefonanruf

* Textnachricht

#### <a name="user-using-fido2-security-key-for-sign-in"></a>Benutzer meldet sich mit FIDO2-Sicherheitsschlüssel an

FIDO2 bietet eine Abstraktionsschicht zwischen dem Formfaktor, der als Authentifikator verwendet wird, und der Kryptographie mit öffentlichem/privatem Schlüssel. Sie ermöglicht integrierten Plattformauthentifikatoren wie Windows Hello und Sicherheitsschlüsseln, sich in einen privaten Schlüssel aufzulösen und einen öffentlichen Schlüssel bereitzustellen, der als Identifikator für den Zugriff auf externe Ressourcen verwendet werden kann. FIDO2-Sicherheitsschlüssel sind mit einer integrierten sicheren Enclave ausgestattet, in der der private Schlüssel gespeichert wird und die zum Entsperren eine biometrische Geste oder eine PIN benötigt. Anmeldeinformationen können nicht wiederverwendet, wiedergegeben oder dienstübergreifend verwendet werden und sind nicht anfällig für Phishing-/MiTM-Angriffe oder Verletzungen der Serversicherheit.

![Die Anmeldung mit FIDO2](./media/ad-passwordless/azure-ad-pwdless-image6.png)

FIDO2-Sicherheitsschlüssel ermöglichen eine sichere Authentifizierung, unabhängig vom Formfaktor. Der Sicherheitsschlüssel enthält die Anmeldeinformationen und sollte mit einem zusätzlichen zweiten Faktor wie einem Fingerabdruck (integriert in den Sicherheitsschlüssel) oder einer PIN geschützt werden, der bzw. die bei der Windows-Anmeldung eingegeben wird. Microsoft-Partner arbeiten an einer Vielzahl von Formfaktoren für Sicherheitsschlüssel. Dazu zählen u. a. USB-Sicherheitsschlüssel und NFC-fähige Smartcards.

> [!NOTE]
> Ein Sicherheitsschlüssel MUSS bestimmte Features und Erweiterungen aus dem FIDO2-Protokoll implementieren, damit er mit [Microsoft kompatibel](https://aka.ms/fido2securitykeys) ist. Microsoft hat diese Lösungen auf Kompatibilität mit Windows 10 und Azure Active Directory getestet.

![Der Anmeldevorgang mit FIDO2](./media/ad-passwordless/azure-ad-pwdless-image9.png)

1. Der Benutzer verbindet das FIDO2-Gerät mit dem Computer.

2. Windows erkennt den FIDO2-Sicherheitsschlüssel.

3. Windows sendet eine Authentifizierungsanforderung.

4. Azure AD sendet eine Nonce zurück.

5. Der Benutzer führt eine Geste aus, um den privaten Schlüssel freizuschalten, der in der sicheren Enclave des FIDO2-Sicherheitsschlüssels gespeichert ist.

6. Der FIDO2-Sicherheitsschlüssel signiert die Nonce mit dem privaten Schlüssel.

7. Die PRT-Tokenanforderung wird mit der signierten Nonce an Azure AD gesendet.

8. Azure AD überprüft die signierte Nonce mit dem öffentlichen FIDO2-Schlüssel.

9. Azure AD gibt das PRT zurück, um den Zugriff auf lokale Ressourcen zu ermöglichen.

#### <a name="user-manages-their-fido2-security-key-credentials"></a>Benutzer verwaltet seine Anmeldeinformationen für den FIDO2-Sicherheitsschlüssel

Ähnlich wie bei der Microsoft Authenticator-App basiert die Verwaltung der Anmeldeinformationen für FIDO2-Sicherheitsschlüssel auf der kombinierten Registrierung für Endbenutzer.

## <a name="deciding-a-passwordless-method"></a>Auswählen einer kennwortlosen Methode

Welche der drei kennwortlosen Optionen die richtige ist, hängt von den Sicherheits-, Plattform- und App-Anforderungen Ihres Unternehmens ab.

Folgende Faktoren sollten Sie bei der Auswahl der Microsoft-Technologie zur kennwortlosen Anmeldung berücksichtigen:

||**Windows Hello for Business**|**Anmelden ohne Kennwort mit der Microsoft Authenticator-App**|**FIDO2-Sicherheitsschlüssel**|
|:-|:-|:-|:-|
|**Voraussetzung**| Windows 10 Version 1809 oder höher<br>Azure Active Directory| Microsoft Authenticator-App<br>Mobiltelefon (iOS- und Android-Gerät mit Android 6.0 oder höher)|Windows 10 Version 1809 oder höher<br>Azure Active Directory|
|**Mode**|Plattform|Software|Hardware|
|**Systeme und Geräte**|PC mit integriertem Trusted Platform Module (TPM)<br>PIN und biometrische Erkennung |PIN und biometrische Erkennung per Telefon|FIDO2-Sicherheitsgeräte, die mit Microsoft kompatibel sind|
|**Benutzerfreundlichkeit**|Melden Sie sich mit einer PIN oder einer biometrischen Erkennung (Gesicht, Iris oder Fingerabdruck) bei Windows-Geräten an.<br>Die Windows Hello-Authentifizierung ist gerätegebunden; der Benutzer benötigt sowohl das Gerät als auch eine Anmeldekomponente wie eine PIN oder einen biometrischen Faktor, um auf Unternehmensressourcen zugreifen zu können.|Melden Sie sich mit einem Mobiltelefon per Fingerabdruckscan, Gesichts- oder Iriserkennung oder PIN an.<br>Die Benutzer melden sich von ihrem PC oder Mobiltelefon aus am Geschäfts- oder Privatkonto an.|Anmelden mithilfe eines FIDO2-Sicherheitsgeräts (Biometrie, PIN und NFC)<br>Der Benutzer kann auf das Gerät gemäß den Richtlinien der Organisation zugreifen und sich per PIN oder biometrischem Merkmal mit Geräten wie USB-Sicherheitsschlüsseln und NFC-fähigen Smartcards, Schlüsseln oder Wearables authentifizieren.|
|**Unterstützte Szenarios**| Kennwortfreie Benutzerführung auf Windows-Gerät.<br>Geeignet für dedizierte Arbeits-PCs mit der Möglichkeit zum einmaligen Anmelden (SSO) bei Geräten und Anwendungen.|Kennwortlose, ortsunabhängige Lösung mit Mobiltelefon.<br>Geeignet für den Zugriff auf Arbeits- oder Privatanwendungen im Web von jedem Gerät aus.|Kennwortfreie Benutzerführung für Worker mit Biometrie, PIN und NFC.<br>Geeignet für gemeinsam verwendete PCs, für die die Verwendung eines Mobiltelefons keine geeignete Option ist (z. B. für Helpdeskmitarbeiter, Nutzern von öffentlichen Terminals oder Krankenhauspersonal).|

Verwenden Sie die folgende Tabelle, um herauszufinden, welche Methode Ihre Anforderungen und Benutzer am besten unterstützt.

|Persona|Szenario|Environment|Kennwortlose Technologie|
|:-|:-|:-|:-|
|**Administrator**|Sicherer Zugriff auf ein Gerät für Verwaltungsaufgaben|Zugewiesenes Windows 10-Gerät|Windows Hello for Business und/oder FIDO2-Sicherheitsschlüssel|
|**Administrator**|Verwaltungsaufgaben auf Nicht-Windows-Geräten| Mobilgerät oder Nicht-Windows-Gerät|Anmelden ohne Kennwort mit der Microsoft Authenticator-App|
|**Information-Worker**|Produktive Arbeit|Zugewiesenes Windows 10-Gerät|Windows Hello for Business und/oder FIDO2-Sicherheitsschlüssel|
|**Information-Worker**|Produktive Arbeit| Mobilgerät oder Nicht-Windows-Gerät|Anmelden ohne Kennwort mit der Microsoft Authenticator-App|
|**Mitarbeitern im Kundenkontakt**|Terminals in Fabriken, im Einzelhandel oder zur Dateneingabe|Gemeinsam genutzte Windows 10-Geräte|FIDO2-Sicherheitsschlüssel|

## <a name="getting-started"></a>Erste Schritte

Die kennwortlose Authentifizierung ist das Modell der Zukunft, das zu einer sichereren Umgebung beiträgt. Es wird empfohlen, dass Organisationen damit beginnen, diesen Wandel zu planen und die Abhängigkeit von Kennwörtern zu reduzieren. Zunächst sollten Sie sich die folgenden Ziele setzen:

* Aktivieren der Benutzer für MFA, Microsoft Authenticator-App und bedingten Zugriff.

* Rollout des Kennwortschutzes von Azure AD + Aktualisieren der Richtlinien.

* Aktivieren der Benutzer für SSPR mit kombinierten Registrierung.

* Bereitstellen der Microsoft Authenticator-App für mobile Geräte.

* Bereitstellen von Windows Hello for Business 1903 (bzw. die jeweils aktuelle Version).

* Bereitstellung von FIDO2-Geräten für Benutzer, die keine Mobiltelefone verwenden können.

* Deaktivieren der kennwortbasierten Authentifizierung (sofern möglich).

Um diese Ziele zu erreichen, empfehlen wir den folgenden Ansatz:

1. Aktivieren Sie Azure Active Directory, um die Features z. B. von Azure MFA und des bedingten Zugriffs voll auszuschöpfen.

2. Aktivieren Sie Multi-Factor Authentication, um die Sicherheit zu erhöhen.

3. Aktivieren Sie die Self-Service-Kennwortzurücksetzung, für den Fall, dass Benutzer auf die Verwendung eines Kennworts zurückgreifen müssen.

4. Ermöglichen Sie die Anmeldung per Telefon durch Bereitstellen von Microsoft Authenticator für verbesserte Mobilität.

5. Stellen Sie Windows Hello for Business für alle Windows 10-Geräte bereit.

6. Bereiten Sie die Verwendung von FIDO2-Sicherheitsschlüsseln vor.

> [!NOTE]
> Weitere Informationen zu den Lizenzbedingungen für kennwortlose Methoden finden Sie auf der [Seite zur Lizenzierung](https://azure.microsoft.com/pricing/details/active-directory/) von Azure Active Directory.

## <a name="conclusion"></a>Zusammenfassung

Schon seit einigen Jahren setzt sich Microsoft für eine Welt ohne Kennwörter ein. Mit Windows 10 hat Microsoft Windows Hello for Business eingeführt, eine Lösung zur sicheren zweistufigen Authentifizierung mit hardwaregeschützten Anmeldeinformationen, die ein einmaliges Anmelden auf Azure Active Directory und Active Directory ermöglicht. Ähnlich wie die Technologie von Windows Hello for Business nutzt die Microsoft Authenticator-App die schlüsselbasierte Authentifizierung, um die Verwendung von Benutzeranmeldeinformationen zu ermöglichen, die an ein mobiles Gerät gebunden sind und auf biometrischen Daten oder einer PIN beruhen. FIDO2-Sicherheitsschlüssel erlauben es Ihnen nun, Ihre Anmeldeinformationen bei sich zu tragen und sich bei Azure AD anzumelden. Der Sicherheitsschlüssel muss hierzu als Anmeldeinformationsanbieter auf dem Windows 10-Sperrbildschirm ausgewählt werden. Alle drei kennwortlosen Lösungen reduzieren das Risiko von Phishing, Kennwort-Spraying und Replay-Angriffen und bieten den Benutzern eine sehr sichere und komfortable Methode für die Anmeldung und den ortsunabhängigen Zugriff auf Daten.

Die Einführung moderner Technologien zur mehrstufigen Authentifizierung z. B. mit biometrischen Merkmalen oder öffentlichen Schlüsseln für allgemein zugängliche Geräte gehört zu den effektivsten Maßnahmen, um die Sicherheits- und Identitätsrisiken in Unternehmen deutlich zu reduzieren. Kennwortlose Lösungen stellen einen langfristigen Ansatz für die sichere Authentifizierung dar und entwickeln sich kontinuierlich weiter. Organisationen können sich auf die steigenden Anforderungen vorbereiten, indem sie damit beginnen, den Wechsel zu kennwortlosen Technologien zu planen.

## <a name="next-steps"></a>Nächste Schritte

* [Was bedeutet „kennwortlos“?](../../active-directory/authentication/concept-authentication-passwordless.md) – eine Übersicht
* [Anmelden bei Azure AD ohne Verwendung eines Kennworts](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)
