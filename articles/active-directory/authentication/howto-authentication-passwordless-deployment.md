---
title: Durchführen einer Bereitstellung der kennwortlosen Authentifizierung mit Azure AD
description: Durchführen einer Bereitstellung der kennwortlosen Authentifizierung mit Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: baselden, librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea4b8ea98365ec13f7f027215a75b9d79ea5c54e
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72452952"
---
# <a name="complete-a-passwordless-authentication-deployment"></a>Durchführen einer Bereitstellung der kennwortlosen Authentifizierung

Die meisten Cyberangriffe beginnen mit einem Benutzernamen und einem Kennwort, die von einer Person in einer Organisation gestohlen wurden. 

Organisationen versuchen, der Bedrohung zu begegnen, indem sie von den Benutzer Folgendes verlangen:

- Lange Kennwörter
- Komplexe Kennwörter
- Häufige Kennwortänderungen
- Multi-Factor Authentication (MFA)

Die [Untersuchung von Microsoft zeigt](https://aka.ms/passwordguidance), dass diese Anforderungen Benutzern lästig sind und Supportkosten in die Höhe treiben. Weitere Informationen finden Sie unter [Your Pa$$word doesn’t matter](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984) (Ihr Kennwort spielt keine Rolle).

Das Bereitstellen der kennwortlosen Authentifizierung bietet die folgenden Vorteile:

- Erhöhte Sicherheit. Reduziertes Risiko von Phishing- und Kennwort-Spray-Angriffen, da Kennwörter als Angriffsfläche entfallen.
- Höhere Benutzerfreundlichkeit. Räumen Sie Benutzern eine mühelose Möglichkeit ein, von jedem Ort aus auf Daten zuzugreifen, und sorgen Sie für unkomplizierten Zugriff auf Outlook, OneDrive, Office und mehr.
- Verlässliche Einblicke. Sie erhalten Einblicke in die kennwortlose Aktivität der Benutzer mit stabiler Protokollierung und Überwachung.

Das Kennwort wird durch etwas ersetzt, das Sie haben, plus etwas, das Sie sind oder wissen. Beispiel: In Windows Hello for Business eine biometrische Geste, z. B. ein Gesicht oder Fingerabdruck, oder eine gerätespezifische PIN, die nicht über ein Netzwerk übertragen wird.

Microsoft bietet drei Optionen für die kennwortlose Authentifizierung, die viele Szenarios abdecken. Diese Methoden können zusammen verwendet werden. 

- [Windows Hello for Business-](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) eignet sich am besten für Benutzer von dedizierten Windows-Computern.
- Die Sicherheitsschlüsselanmeldung mit [FIDO2-Sicherheitsschlüsseln](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) ist besonders nützlich für Benutzer, die sich bei gemeinsam genutzten Computern wie Kiosken anmelden, in Situationen, in denen die Verwendung von Telefonen eingeschränkt ist, und für hochprivilegierte Identitäten.
- Die Anmeldung per Telefon mit der [Microsoft Authenticator-App](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) ist hilfreich, um für Benutzer mit mobilen Geräten eine kennwortlose Option bereitzustellen. Die zuverlässige kennwortlose Anmeldung ist mit jedem iOS- oder Android-Telefon möglich, denn Benutzer können sich bei beliebigen Plattformen oder Browsern anmelden. Um sich anzumelden, erhalten Benutzer eine Benachrichtigung über ihr Telefon, gleichen eine auf dem Bildschirm angezeigte Zahl mit der Zahl auf ihrem Telefon ab und verwenden dann ihre biometrischen Daten oder ihre PIN zur Bestätigung.

## <a name="compare-passwordless-authentication-methods"></a>Vergleichen von Methoden zur kennwortlosen Authentifizierung

Die Methoden zur kennwortlosen Authentifizierung von Microsoft ermöglichen verschiedene Szenarien. Beachten Sie die Anforderungen Ihrer Organisation, die Voraussetzungen und die Funktionen der einzelnen Authentifizierungsmethoden, um Ihre Strategie für die kennwortlose Authentifizierung auszuwählen. Jede Organisation, die Windows 10-Geräte verwendet, sollte Windows Hello for Business verwenden. Fügen Sie dann entweder die telefonische Anmeldung (mit der Microsoft Authenticator-App) oder Sicherheitsschlüssel für weitere Szenarien hinzu.

### <a name="passwordless-authentication-scenarios"></a>Szenarien der kennwortlosen Authentifizierung

| Szenario | Telefonauthentifizierung | Sicherheitsschlüssel | Windows Hello for Business |
| --- | --- | --- | --- |
| **Computeranmeldung**: <br> Von zugewiesenem Windows 10-Gerät | **Nein** | **Ja** <br> Mit biometrischer Erkennung, PIN | **Ja**<br>Mit biometrischer Erkennung und/oder PIN |
| **Computeranmeldung**: <br> Von gemeinsam genutztem Windows 10-Gerät | **Nein** | **Ja** <br> Mit biometrischer Erkennung, PIN  | **Nein** |
| **Web-App-Anmeldung**: <br>Von einem benutzerdedizierten Computer | **Ja** | **Ja** <br> Bereitgestelltes einmaliges Anmelden bei Apps wird durch Computeranmeldung aktiviert | **Ja**<br> Bereitgestelltes einmaliges Anmelden bei Apps wird durch Computeranmeldung aktiviert |
| **Web-App-Anmeldung**: <br> Von einem Mobilgerät oder Nicht-Windows-Gerät | **Ja** | **Nein** | **Nein** |
| **Computeranmeldung**: <br> Nicht-Windows-Computer | **Nein** | **Nein** | **Nein** |

### <a name="technical-considerations-for-the-microsoft-authenticator-app"></a>Technische Überlegungen zur Microsoft Authenticator-App

**Integration der Active Directory-Verbunddienste (AD FS)** : Wenn ein Benutzer die kennwortlosen Microsoft Authenticator-Anmeldeinformationen aktiviert, wird die Authentifizierung für diesen Benutzer standardmäßig auf das Senden einer Genehmigungsbenachrichtigung festgelegt. Benutzer in einem hybriden Mandanten werden daran gehindert, zur Anmeldung an AD FS weitergeleitet zu werden, es sei denn, sie wählen „Stattdessen Ihr Kennwort verwenden“ aus. Bei diesem Prozess werden auch alle lokalen Richtlinien für bedingten Zugriff und Passthrough-Authentifizierungsflüsse umgangen. Wenn jedoch ein Anmeldehinweis (login_hint) angegeben ist, wird ein Benutzer zu AD FS weitergeleitet, und die Option zur Verwendung von kennwortlosen Anmeldeinformationen wird umgangen.

**Azure MFA-Server**: Für die MFA über den lokalen Azure MFA-Server einer Organisation aktivierte Endbenutzer können weiterhin einen einzelnen Satz von Anmeldeinformationen für die Anmeldung per Smartphone ohne Kennwort erstellen und verwenden. Wenn der Benutzer versucht, mehrere Installationen (fünf oder mehr) von Microsoft Authenticator mit den Anmeldeinformationen zu aktualisieren, kann diese Änderung zu einem Fehler führen.

**Geräteregistrierung**: Um die Authenticator-App für die kennwortlose Authentifizierung zu verwenden, muss das Gerät im Azure AD-Mandanten registriert sein und darf kein gemeinsam genutztes Gerät sein. Ein Gerät kann nur bei einem einzigen Mandanten registriert werden. Dies bedeutet, dass nur ein einziges Geschäfts-, Schul- oder Unikonto für die Anmeldung per Authenticator-App unterstützt wird.

## <a name="prerequisites"></a>Voraussetzungen

Organisationen müssen die folgenden Voraussetzungen erfüllen, bevor sie eine kennwortlose Bereitstellung beginnen können.

| Voraussetzung | Authenticator-App | FIDO2-Sicherheitsschlüssel |
| --- | --- | --- |
| [Kombinierte Registrierung für Azure MFA und Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR)](howto-registration-mfa-sspr-combined.md) ist aktiviert (Previewfunktion) | √ | √ |
| [Benutzer können Azure MFA ausführen](howto-mfa-getstarted.md) | √ | √ |
| [Benutzer haben sich für Azure MFA und SSPR registriert](howto-registration-mfa-sspr-combined.md) | √ | √ |
| [Benutzer haben ihre mobilen Geräte für Azure Active Directory registriert](../devices/overview.md) | √ |   |
| Windows 10, Version 1809 oder höher, mit einem unterstützten Browser wie Microsoft Edge oder Mozilla Firefox <br> (Version 67 oder höher). <br> *Microsoft empfiehlt Version 1903 oder höher für native Unterstützung*. |   | √ |
| Kompatible FIDO2-Sicherheitsschlüssel. Stellen Sie sicher, dass Sie ein [von Microsoft getestetes und verifiziertes](howto-authentication-passwordless-enable.md) FIDO2-Sicherheitsgerät oder ein anderes kompatibles FIDO2-Sicherheitsgerät verwenden. |   | √ |

### <a name="prerequisites-for-windows-hello-for-business"></a>Voraussetzungen für Windows Hello for Business

Die Voraussetzungen für Windows Hello for Business sind stark davon abhängig, ob Sie die Bereitstellung in einer lokalen, Hybrid- oder reinen Cloudkonfiguration durchführen. Weitere Informationen finden Sie in der [vollständigen Liste der Voraussetzungen für Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Benutzer registrieren ihre kennwortlose Methode als Teil des Azure MFA-Registrierungsflusses. Die mehrstufige Authentifizierung mit einem Benutzernamen und einem Kennwort sowie einer anderen registrierten Methode kann ersatzweise in Szenarios verwendet werden, in denen sie ihr Smartphone oder ihren Sicherheitsschlüssel nicht verwenden können.

### <a name="security-key-lifecycle"></a>Lebenszyklus des Sicherheitsschlüssels

Sicherheitsschlüssel ermöglichen den Zugriff auf Ihre Ressourcen, und Sie sollten die Verwaltung dieser physischen Geräte planen.

1. Schlüsselverteilung: Planen Sie, wie Sie Schlüssel für Ihre Organisation bereitstellen möchten. Möglicherweise verfügen Sie über einen zentralisierten Bereitstellungsprozess oder erlauben den Endbenutzern, mit FIDO 2.0 kompatible Schlüssel zu erwerben.
1. Schlüsselaktivierung: Endbenutzer müssen den Sicherheitsschlüssel selbst aktivieren. Endbenutzer registrieren ihre Sicherheitsschlüssel unter [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) und aktivieren den zweiten Faktor (PIN oder biometrisch) bei der ersten Verwendung.
1. Deaktivieren eines Schlüssels: Die Sicherheitsschlüsselfunktionalität befindet sich zwar in der Vorschauphase, aber ein Administrator kann keinen Schlüssel aus einem Benutzerkonto entfernen. Der Benutzer muss ihn entfernen. Wenn ein Schlüssel verloren geht oder gestohlen wird:
   1. Entfernen Sie den Benutzer aus jeder Gruppe, für die die kennwortlose Authentifizierung aktiviert ist.
   1. Vergewissern Sie sich, dass der Schlüssel als Authentifizierungsmethode entfernt wurde.
   1. Geben Sie einen neuen Schlüssel aus.
1. Schlüsselersetzung: Benutzer können zwei Sicherheitsschlüssel gleichzeitig aktivieren. Wenn Sie einen Sicherheitsschlüssel ersetzen, stellen Sie sicher, dass der Benutzer den zu ersetzenden Schlüssel auch entfernt hat.

### <a name="enable-windows-10-support"></a>Aktivieren von Windows 10-Unterstützung

Um die Windows 10-Anmeldung mit FIDO2-Sicherheitsschlüsseln zu aktivieren, muss die Anmeldeinformationsanbieter-Funktionalität in Windows 10 aktiviert werden. Sie können zum Aktivieren zwischen zwei Möglichkeiten wählen:

- [Aktivieren der zielgerichteten Intune-Bereitstellung](howto-authentication-passwordless-security-key.md#enable-targeted-intune-deployment)
   - Die Intune-Bereitstellung ist die empfohlene Option für Computer, die mit Azure Active Directory verbunden sind.
- [Aktivieren des Anmeldeinformationsanbieters mittels eines Bereitstellungspakets](howto-authentication-passwordless-security-key.md#enable-credential-provider-via-provisioning-package)
   - Wenn die Intune-Bereitstellung nicht möglich ist, müssen Administratoren auf jedem Computer ein Paket bereitstellen, um die Anmeldeinformationsanbieter-Funktionalität zu aktivieren. Die Paketinstallation kann mit einer der folgenden Optionen ausgeführt werden:
      - Gruppenrichtlinie oder System Center Configuration Manager (SCCM)
      - Lokale Installation auf einem Windows 10-Computer

### <a name="register-security-keys"></a>Registrieren von Sicherheitsschlüsseln

Benutzer müssen ihren Sicherheitsschlüssel auf jedem ihrer mit Azure Active Directory verbundenen Windows 10-Computer registrieren.

Weitere Informationen finden Sie unter [Benutzerregistrierung und Verwaltung von FIDO2-Sicherheitsschlüsseln](howto-authentication-passwordless-security-key.md).

### <a name="licensing-for-passwordless-authentication"></a>Lizenzierung der kennwortlosen Authentifizierung

Es fallen keine zusätzlichen Kosten für die kennwortlose Authentifizierung an, obwohl einige Voraussetzungen möglicherweise ein Premium-Abonnement erfordern. Ausführliche Informationen zu Features und Lizenzen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="develop-a-plan"></a>Entwickeln eines Plans

Berücksichtigen Sie Ihre geschäftlichen Anforderungen und die Anwendungsfälle für jede Authentifizierungsmethode. Wählen Sie dann die Methode aus, die Ihren Anforderungen am besten entspricht.

### <a name="use-cases"></a>Anwendungsfälle

In der folgenden Tabelle werden die Anwendungsfälle erläutert, die während dieses Projekts implementiert werden müssen.

| Bereich | BESCHREIBUNG |
| --- | --- |
| **zugreifen** | Die kennwortlose Anmeldung ist von einem firmeneigenen oder persönlichen Gerät innerhalb oder außerhalb des Unternehmensnetzwerks aus verfügbar. |
| **Überwachung** | Nutzungsdaten stehen Administratoren zur Überwachung nahezu in Echtzeit zur Verfügung. <br> Die Nutzungsdaten werden mindestens alle 29 Tage in die Unternehmenssysteme heruntergeladen, oder das SIEM-Tool (Security Information & Event Management) wird verwendet. |
| **Governance** | Der Lebenszyklus von Benutzerzuweisungen zur entsprechenden Authentifizierungsmethode und zu zugeordneten Gruppen wird definiert und überwacht. |
| **Sicherheit** | Der Zugriff auf die entsprechende Authentifizierungsmethode wird über Benutzer- und Gruppenzuweisungen gesteuert. <br> Nur autorisierte Benutzer können die kennwortlose Anmeldung verwenden. |
| **Leistung** | Die Zeitachsen für die Verteilung von Zugriffszuweisungen werden dokumentiert und überwacht. <br> Die Anmeldezeiten werden zur einfachen Nutzung gemessen. |
| **Benutzerfreundlichkeit** | Die Benutzer kennen die mobile Kompatibilität. <br> Benutzer können die Authenticator-App für die kennwortlose Anmeldung konfigurieren. |
| **Unterstützung** | Benutzer wissen, wie sie Unterstützung bei Problemen mit der kennwortlosen Anmeldung finden. |

### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten

Wenn Technologieprojekte nicht gelingen, ist dies in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Um diese Fallstricke zu vermeiden, stellen Sie sicher, dass Sie die [richtigen Beteiligten hinzuziehen](../fundamentals/active-directory-deployment-plans.md#include-the-right-stakeholders) und dass die Rollen der Beteiligten im Projekt gut verstanden werden.

### <a name="organization-communications"></a>Organisationskommunikation

Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Kommunizieren Sie proaktiv darüber, wie sich die Erfahrung der Benutzer ändern wird, wann sie sich ändert und wie sie Unterstützung erhalten, wenn sie auf Probleme stoßen.

Ihre Kommunikation mit Endbenutzern muss Folgendes umfassen:

- [Aktivieren der kombinierten Sicherheitsregistrierung](howto-authentication-passwordless-phone.md)
- [Herunterladen der Microsoft Authenticator-App](../user-help/user-help-auth-app-download-install.md)
- [Registrieren der Microsoft Authenticator-App](howto-authentication-passwordless-phone.md)
- [Anmelden per Telefon](../user-help/user-help-auth-app-sign-in.md)

Microsoft bietet MFA-[Kommunikationsvorlagen](https://aka.ms/mfatemplates), Self-Service-Kennwortzurücksetzung-[Kommunikationsvorlagen](https://www.microsoft.com/download/details.aspx?id=56768) (Self-Service Password Reset, SSPR) und eine [Endbenutzerdokumentation](../user-help/security-info-setup-signin.md), womit Sie Ihre Kommunikation entwerfen können. Sie können Benutzer auf [https://myprofile.microsoft.com](https://myprofile.microsoft.com/) verweisen, wo sie sich durch Auswahl der Sicherheitsinformationen-Links auf dieser Seite direkt registrieren können.

### <a name="testing-passwordless"></a>Testen der kennwortlosen Authentifizierung

Stellen Sie in jeder Phase der Bereitstellung sicher, dass Sie testen, ob die Ergebnisse den Erwartungen entsprechen.

#### <a name="testing-the-microsoft-authenticator-app"></a>Testen der Microsoft Authenticator-App

Im Folgenden finden Sie Beispieltestfälle für die kennwortlose Authentifizierung mit der Microsoft Authenticator-App.

| Szenario | Erwartete Ergebnisse |
| --- | --- |
| Benutzer kann Microsoft Authenticator-App registrieren | Benutzer kann App über „aka.ms/mysecurityinfo“ registrieren |
| Benutzer kann die Anmeldung per Telefon aktivieren | Anmeldung per Telefon ist für das Geschäftskonto konfiguriert |
| Benutzer kann mit der telefonischen Anmeldung auf eine App zugreifen | Benutzer durchläuft den Anmeldevorgang per Telefon und erreicht die angegebene Anwendung. |
| Testen des Rollbacks der Registrierung für die telefonische Anmeldung durch Deaktivieren der kennwortlosen Microsoft Authenticator-Anmeldung im Authentifizierungsmethoden-Bildschirm des Azure Active Directory-Portals | Zuvor aktivierte Benutzer können die kennwortlose Anmeldung mit Microsoft Authenticator nicht verwenden. |
| Entfernen der telefonischen Anmeldung per Microsoft Authenticator-App | Geschäftskonto ist auf Microsoft Authenticator nicht mehr verfügbar |

#### <a name="testing-security-keys"></a>Testen von Sicherheitsschlüsseln

Im Folgenden finden Sie Beispieltestfälle für die kennwortlose Authentifizierung mit Sicherheitsschlüsseln.

**Kennwortlose FIDO-Anmeldung bei mit Azure Active Directory verbundenen Windows 10-Geräten**

| Szenario | Erwartete Ergebnisse |
| --- | --- |
| Der Benutzer kann das FIDO2-Gerät registrieren (1809) | Der Benutzer kann das FIDO2-Gerät unter „Einstellungen > Konten > Anmeldeoptionen > Sicherheitsschlüssel“ registrieren |
| Der Benutzer kann das FIDO2-Gerät zurücksetzen (1809) | Der Benutzer kann das FIDO2-Gerät mithilfe der Herstellersoftware zurücksetzen |
| Der Benutzer kann sich mit dem FIDO2-Gerät (1809) anmelden | Der Benutzer kann den Sicherheitsschlüssel im Anmeldefenster auswählen und sich erfolgreich anmelden. |
| Der Benutzer kann das FIDO2-Gerät registrieren (1903) | Der Benutzer kann das FIDO2-Gerät unter „Einstellungen > Konten > Anmeldeoptionen > Sicherheitsschlüssel“ registrieren |
| Der Benutzer kann das FIDO2-Gerät zurücksetzen (1903) | Der Benutzer kann das FIDO2-Gerät unter „Einstellungen > Konten > Anmeldeoptionen > Sicherheitsschlüssel“ zurücksetzen |
| Der Benutzer kann sich mit dem FIDO2-Gerät (1809) anmelden | Der Benutzer kann den Sicherheitsschlüssel im Anmeldefenster auswählen und sich erfolgreich anmelden. |

**Kennwortlose FIDO-Anmeldung bei Azure AD-Web-Apps**

| Szenario | Erwartete Ergebnisse |
| --- | --- |
| Der Benutzer kann das FIDO2-Gerät über „aka.ms/mysecurityinfo“ mit Microsoft Edge registrieren | Registrierung sollte erfolgreich sein |
| Der Benutzer kann das FIDO2-Gerät über „aka.ms/mysecurityinfo“ mit Firefox registrieren | Registrierung sollte erfolgreich sein |
| Der Benutzer kann sich mithilfe des FIDO2-Geräts über Microsoft Edge bei OneDrive online anmelden | Anmeldung sollte erfolgreich sein |
| Der Benutzer kann sich mithilfe des FIDO2-Geräts über Firefox bei OneDrive online anmelden | Anmeldung sollte erfolgreich sein |
| Testen des Rollbacks der FIDO2-Geräteregistrierung durch Deaktivieren der FIDO2-Sicherheitsschlüssel auf dem Authentifizierungsmethoden-Blatt im Azure Active Directory-Portal | Benutzer werden aufgefordert, sich mit ihrem Sicherheitsschlüssel anzumelden. Sie werden erfolgreich angemeldet, und es wird eine Fehlermeldung angezeigt: „Für Ihre Unternehmensrichtlinie müssen Sie eine andere Methode für die Anmeldung verwenden.“ Benutzer sollten dann in der Lage sein, eine andere Methode auszuwählen und sich erfolgreich anzumelden. Schließen Sie das Fenster, und melden Sie sich erneut an, um zu überprüfen, ob diese Fehlermeldung nicht angezeigt wird. |

### <a name="auditing-passwordless"></a>Überwachen der kennwortlosen Authentifizierung

Azure AD stellt Berichte zur Verfügung, die technische und geschäftliche Einblicke bieten. Sorgen Sie dafür, dass die Besitzer Ihrer geschäftlichen und technischen Anwendungen den Besitz dieser Berichte übernehmen und sie den Anforderungen Ihrer Organisation gemäß nutzen.

Im Authentifizierungsmethoden-Abschnitt des Azure Active Directory-Portals können Administratoren Einstellungen für kennwortlose Anmeldeinformationen aktivieren und verwalten.

Azure AD fügt den Überwachungsprotokollen in folgenden Fällen Einträge hinzu:

- Ein Administrator nimmt im Authentifizierungsmethoden-Abschnitt Änderungen vor.
- Ein Benutzer nimmt in Azure Active Directory Änderungen an seinen Anmeldeinformationen vor.

Die folgende Tabelle enthält einige Beispiele für typische Berichtsszenarien.

|   | Risikomanagement | Steigerung der Produktivität | Governance und Einhaltung |
| --- | --- | --- | --- |
| **Berichtstypen** | Authentifizierungsmethoden: Benutzer, die für die kombinierte Sicherheitsregistrierung registriert sind | Authentifizierungsmethoden: Benutzer, die für die App-Benachrichtigung registriert sind | Anmeldungen: Überprüfen, wer wie auf den Mandanten zugreift |
| **Mögliche Aktionen** | Zielbenutzer noch nicht registriert | Übernahme von Microsoft Authenticator-App oder Sicherheitsschlüsseln steuern | Zugriff widerrufen oder zusätzliche Sicherheitsrichtlinien für Administratoren erzwingen |

**Azure AD speichert die meisten Überwachungsdaten 30 Tage lang** und stellt die Daten über das Azure-Verwaltungsportal oder eine API zur Verfügung, damit Sie sie in Ihre Analysesysteme herunterladen können. Wenn Ihre Organisation eine längere Aufbewahrungszeit fordert, müssen die Protokolle exportiert und in einem SIEM-Tool wie etwa [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), Splunk oder Sumo Logic genutzt werden. [Erfahren Sie mehr über das Anzeigen von Zugriffs- und Verwendungsberichten](../reports-monitoring/overview-reports.md).

Um ihre Anmeldeinformationen zu registrieren und zu verwalten, können Benutzer zu [https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) navigieren. Dieser Link leitet Benutzer zur Benutzeroberfläche für die Verwaltung von Endbenutzer-Anmeldeinformationen, die über die kombinierte SSPR/MFA-Registrierung aktiviert wurde. Alle Registrierungen von FIDO2-Sicherheitsgeräten oder Änderungen an Authentifizierungsmethoden durch einen Benutzer werden in den Azure Active Directory-Überwachungsprotokollen protokolliert.

Wenn Benutzer das Konto für einen Sicherheitsschlüssel aktivieren oder deaktivieren bzw. den zweiten Faktor für den Sicherheitsschlüssel auf ihren Windows 10-Computern zurücksetzen, wird dem Sicherheitsprotokoll unter den folgenden Ereignis-IDs ein Eintrag hinzugefügt: 4670, 5382.

### <a name="plan-for-rollback"></a>Planen der Zurücksetzung

Obwohl die kennwortlose Authentifizierung ein einfaches Feature mit minimalen Auswirkungen auf Endbenutzer ist, muss möglicherweise ein Rollback ausgeführt werden.

Für das Rollback muss der Administrator sich beim Azure Active Directory-Portal anmelden, die gewünschten starken Authentifizierungsmethoden auswählen und die Aktivierungsoption in „Nein“ ändern. Durch diesen Vorgang wird die kennwortlose Funktionalität für alle Benutzer deaktiviert.

Benutzer, die bereits FIDO2-Sicherheitsgeräte registriert haben, werden bei der nächsten Anmeldung aufgefordert, das Sicherheitsgerät zu verwenden, und die folgende Fehlermeldung wird angezeigt:

![Auswählen einer anderen Anmeldeoption](./media/howto-authentication-passwordless-deployment/passwordless-choose-sign-in.png)

### <a name="plan-to-pilot"></a>Planen eines Pilotversuchs

Wenn Sie die kennwortlose Authentifizierung bereitstellen, sollten Sie zunächst mindestens eine Pilotgruppe aktivieren. Sie können speziell zu diesem Zweck [Gruppen erstellen](../fundamentals/active-directory-groups-create-azure-portal.md). Fügen Sie die Benutzer, die am Pilotversuch teilnehmen, den Gruppen hinzu. Aktivieren Sie dann neue kennwortlose Authentifizierungsmethoden für die ausgewählten Gruppen.

Gruppen können aus einem lokalen Verzeichnis oder Azure AD synchronisiert werden. Sobald Sie mit den Ergebnissen Ihres Pilotversuchs zufrieden sind, können Sie die kennwortlose Authentifizierung für alle Benutzer aktivieren.

Weitere Informationen finden Sie auf der Bereitstellungspläneseite unter [Bewährte Methoden für einen Pilotversuch](https://aka.ms/deploymentplans).

## <a name="deploy-passwordless-authentication"></a>Bereitstellen der kennwortlosen Authentifizierung

Befolgen Sie die unten aufgeführten Schritte, die der Methode Ihrer Wahl entsprechen.

### <a name="required-administrative-roles"></a>Erforderliche Administratorrollen

| Azure AD-Rolle | BESCHREIBUNG |
| --- | --- |
| Authentifizierungsadministrator | Die am wenigsten privilegierte Rolle, die Authentifizierungsmethoden implementieren und verwalten kann |
| Benutzer | Die am wenigsten privilegierte Rolle zum Konfigurieren der Authenticator-App auf dem Gerät oder zum Registrieren eines Sicherheitsschlüsselgeräts für Web- oder Windows 10-Anmeldung. |

### <a name="deploy-phone-sign-in-with-the-microsoft-authenticator-app"></a>Bereitstellen der telefonischen Anmeldung per Microsoft Authenticator-App

Führen Sie die Schritte im Artikel [Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschauversion)](howto-authentication-passwordless-phone.md) aus, um die Microsoft Authenticator-App als kennwortlose Authentifizierungsmethode in Ihrer Organisation zu aktivieren.

### <a name="deploy-fido2-security-key-sign-in"></a>Bereitstellen der FIDO2-Sicherheitsschlüsselanmeldung

Führen Sie die Schritte im Artikel [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei Azure AD (Vorschauversion)](howto-authentication-passwordless-security-key.md) aus, um FIDO2-Sicherheitsschlüssel als kennwortlose Authentifizierungsmethoden in Ihrer Organisation zu aktivieren.
 
### <a name="troubleshoot-phone-sign-in"></a>Problembehandlung bei der Anmeldung per Telefon

| Szenario | Lösung |
| --- | --- |
| Benutzer kann keine kombinierte Registrierung ausführen | Sicherstellen, dass die [kombinierte Registrierung](concept-registration-mfa-sspr-combined.md) aktiviert ist. |
| Benutzer kann die telefonische Anmeldung nicht in der Authenticator-App aktivieren | Sicherstellen, dass sich der Benutzer im Bereitstellungsbereich befindet |
| Benutzer befindet sich NICHT im Gültigkeitsbereich für die kennwortlose Authentifizierung, wird jedoch mit der Option für kennwortlose Anmeldung angezeigt, die er nicht ausführen kann. | Dieses Szenario tritt ein, wenn der Benutzer vor der Erstellung der Richtlinie die telefonische Anmeldung in der Anwendung aktiviert hat. <br> So aktivieren Sie die Anmeldung: Fügen Sie den Benutzer dem Bereich der Benutzer hinzu, für die die kennwortlose Anmeldung aktiviert ist. <br> So blockieren Sie die Anmeldung: Veranlassen Sie, dass der Benutzer seine Anmeldeinformationen aus der Anwendung entfernt. |

### <a name="troubleshoot-security-key-sign-in"></a>Problembehandlung bei der Anmeldung mit Sicherheitsschlüssel

| Szenario | Lösung |
| --- | --- |
| Benutzer kann keine kombinierte Registrierung ausführen | Sicherstellen, dass die [kombinierte Registrierung](concept-registration-mfa-sspr-combined.md) aktiviert ist. |
| Benutzer kann in seinen [Sicherheitseinstellungen](https://aka.ms/mysecurityinfo) keinen Sicherheitsschlüssel hinzufügen | Sicherstellen, dass [Sicherheitsschlüssel](howto-authentication-passwordless-security-key.md) aktiviert sind. |
| Benutzer kann den Windows 10-Anmeldeoptionen keinen Sicherheitsschlüssel hinzufügen | [Sicherstellen, dass Sicherheitsschlüssel für die Windows-Anmeldung vorhanden sind](howto-authentication-passwordless-enable.md) |
| **Fehlermeldung**: Wir haben festgestellt, dass dieser Browser oder dieses Betriebssystem keine FIDO2-Sicherheitsschlüssel unterstützt. | Kennwortlose FIDO2-Sicherheitsgeräte können nur in unterstützten Browsern (Microsoft Edge, Firefox-Version 67) unter Windows 10, Version 1809 oder höher, registriert werden. |
| **Fehlermeldung**: Für Ihre Unternehmensrichtlinie müssen Sie eine andere Anmeldungsmethode verwenden. | Unsichere Sicherheitsschlüssel sind im Mandanten aktiviert. |
| Benutzer kann meinen Sicherheitsschlüssel unter Windows 10, Version 1809 nicht verwalten | Version 1809 erfordert, dass Sie die vom FIDO2-Schlüsselanbieter bereitgestellte Sicherheitsschlüssel-Verwaltungssoftware verwenden. Bitten Sie den Anbieter um Support. |
| Ich denke, dass mein FIDO2-Sicherheitsschlüssel möglicherweise fehlerhaft ist – wie kann ich ihn testen? | Navigieren Sie zu [https://webauthntest.azurewebsites.net/](https://webauthntest.azurewebsites.net/), geben Sie die Anmeldeinformationen für ein Testkonto ein, stecken Sie den eventuell fehlerhaften Sicherheitsschlüssel ein, klicken Sie oben rechts auf dem Bildschirm auf die Schaltfläche „+“, klicken Sie auf „Erstellen“, und durchlaufen Sie den Erstellungsvorgang. Wenn in diesem Szenario ein Fehler auftritt, ist Ihr Gerät möglicherweise fehlerhaft. |

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei Azure AD](howto-authentication-passwordless-security-key.md)
- [Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschauversion)](howto-authentication-passwordless-phone.md)
- [Authentifizierungsmethoden: Nutzung und Erkenntnisse](howto-authentication-methods-usage-insights.md)
