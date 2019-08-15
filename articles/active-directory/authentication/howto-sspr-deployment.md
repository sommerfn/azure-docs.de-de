---
title: 'Bereitstellungsplan zur Self-Service-Kennwortzurücksetzung: Azure Active Directory'
description: Strategie für eine erfolgreiche Implementierung der Self-Service-Kennwortzurücksetzung von Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7033c7bd3e783157280709b2c7e889473166ac84
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879223"
---
# <a name="deploy-azure-ad-self-service-password-reset"></a>Bereitstellen von Self-Service-Kennwortzurücksetzung in Azure AD

Die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) ist eine Funktion von Azure Active Directory, die es Mitarbeitern ermöglicht, Ihre Kennwörter zurückzusetzen, ohne sich an IT-Mitarbeiter wenden zu müssen. Mitarbeiter müssen sich für die Self-Service-Kennwortzurücksetzung registrieren, bevor sie den Dienst verwenden. Während der Registrierung wählt der Mitarbeiter eine oder mehrere Authentifizierungsmethoden aus, die von seiner Organisation aktiviert wurden.

SSPR ermöglicht es Mitarbeitern, die Blockierung schnell aufzuheben und die Arbeit fortzusetzen, unabhängig von Aufenthaltsort und Uhrzeit. Indem sie ihren Benutzern erlaubt, die Blockierung selbst aufzuheben, kann Ihre Organisation die unproduktive Zeit und die hohen Supportkosten für die meisten allgemeinen Probleme im Zusammenhang mit Kennwörtern verringern.

Unterstützen Sie Benutzer bei der schnellen Registrierung, indem Sie SSPR zusammen mit einer anderen Anwendung oder einem anderen Dienst in Ihrer Organisation einrichten. Diese Aktion wird zu einer großen Anzahl Anmeldungen führen und fördert die Registrierung.

Vor der Bereitstellung von SSPR kann es für Organisationen sinnvoll sein, zu bestimmen, wie viele Helpdeskanrufe im Zusammenhang mit Kennwörtern im Lauf der Zeit anfallen und welche Kosten sie im Mittel verursachen. Sie können diese Daten nach der Bereitstellung verwenden, um den Wert von SSPR für die Organisation zu demonstrieren.  

## <a name="how-sspr-works"></a>Funktionsweise von SSPR

1. Wenn ein Benutzer versucht, ein Kennwort zurückzusetzen, muss er seine zuvor registrierte Authentifizierungsmethode oder -methoden bestätigen, um seine Identität zu beweisen.
1. Anschließend gibt der Benutzer ein neues Kennwort ein.
   1. Für reine Cloudbenutzer wird das neue Kennwort in Azure Active Directory gespeichert. Weitere Informationen finden Sie im Artikel [Funktionsweise von SSPR](concept-sspr-howitworks.md#how-does-the-password-reset-portal-work).
   1. Für Hybridbenutzer wird das Kennwort über den Azure AD Connect-Dienst zurück in das lokale Active Directory geschrieben. Weitere Informationen finden Sie im Artikel [Was ist Kennwortrückschreiben](concept-sspr-writeback.md#how-password-writeback-works).

## <a name="licensing-considerations"></a>Lizenzierungsaspekte

Azure Active Directory wird pro Benutzer lizenziert, d.h. jeder Benutzer muss über eine passende Lizenz für die von ihm genutzten Features verfügen.

Weitere Informationen zur Lizenzierung finden Sie auf der [Azure Active Directory-Preisverzeichnisseite](https://azure.microsoft.com/pricing/details/active-directory/)

## <a name="enable-combined-registration-for-sspr-and-mfa"></a>Aktivieren der kombinierten Registrierung für SSPR und MFA

Microsoft empfiehlt Organisationen, die kombinierte Registrierungs-Benutzeroberfläche für SSPR und mehrstufige Authentifizierung zu aktivieren. Wenn Sie diese kombinierte Registrierungs-Benutzeroberfläche aktivieren, brauchen Benutzer ihre Registrierung nur einmal auszuwählen, um beide Funktionen zu aktivieren.

![Kombinierte Registrierung von Sicherheitsinformationen](./media/howto-sspr-deployment/combined-security-info.png)

Die kombinierte Registrierungs-Benutzeroberfläche zwingt Organisationen nicht, sowohl SSPR als auch Azure Multi-Factor Authentication zu aktivieren. Die kombinierte Registrierungs-Benutzeroberfläche bietet in Organisationen eine bessere Benutzererfahrung als die herkömmlichen Einzelkomponenten. Weitere Informationen zur kombinierten Registrierung und ihrer Aktivierung finden Sie im Artikel [Kombinierte Registrierung von Sicherheitsinformationen (Vorschauversion)](concept-registration-mfa-sspr-combined.md).

## <a name="plan-the-configuration"></a>Planen der Konfiguration

Die folgenden Einstellungen sind erforderlich, um SSPR mit den empfohlenen Werten zu aktivieren.

| Bereich | Einstellung | Wert |
| --- | --- | --- |
| **SSPR-Eigenschaften** | Self-Service-Kennwortzurücksetzung aktiviert | **Ausgewählte** Gruppe für das Pilotprojekt/**Alle** für die Produktion |
| **Authentifizierungsmethoden** | Zum Registrieren erforderliche Authentifizierungsmethoden | Immer 1 mehr als für das Zurücksetzen erforderlich |
|   | Zum Zurücksetzen erforderliche Authentifizierungsmethoden | Eine oder zwei |
| **Registrierung** | Registrierung von Benutzern bei der Anmeldung verlangen | Ja |
|   | Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen | 90–180 Tage |
| **Notifications** | Benutzer über Kennwortzurücksetzungen benachrichtigen? | Ja |
|   | Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen? | Ja |
| **Anpassung** | Helpdesklink anpassen | Ja |
|   | Benutzerdefinierte Helpdesk-E-Mail oder URL | Support-Website oder E-Mail-Adresse |
| **Lokale Integration** | Zurückschreiben von Kennwörtern in das lokale AD | Ja |
|   | Benutzern das Entsperren des Kontos ohne Zurücksetzen des Kennworts erlauben | Ja |

### <a name="sspr-properties-recommendations"></a>Empfehlungen für SSPR-Eigenschaften

Wählen Sie beim Aktivieren der Self-Service-Kennwortzurücksetzung eine Sicherheitsgruppe aus, die während der Pilotphase verwendet wird.

Wenn Sie planen, den Dienst breiter einzuführen, empfehlen wir die Verwendung der Option „Alle“, um SSPR für alle Benutzer in der Organisation durchzusetzen. Wenn „Alle“ für Sie keine mögliche Option darstellt, wählen Sie die geeignete Azure AD-Sicherheitsgruppe oder mit Azure AD synchronisierte AD-Gruppe aus.

### <a name="authentication-methods"></a>Authentifizierungsmethoden

Legen Sie die zum Registrieren erforderlichen Authentifizierungsmethoden auf mindestens eine mehr als die zum Zurücksetzen erforderliche Anzahl fest. Das Zulassen mehrerer Authentifizierungsmethoden verleiht Benutzern Flexibilität beim Zurücksetzen.

Legen Sie **Anzahl der zum Zurücksetzen erforderlichen Methoden**  auf ein für Ihre Organisation angemessenes Maß fest. Eine bringt das geringste Maß an Reibung mit sich, während zwei den Sicherheitsstatus Ihrer Organisation verbessern können.

Detaillierte Informationen zu den für SSPR verfügbaren Authentifizierungsmethoden, vordefinierte Sicherheitsfragen und Anleitungen zum Erstellen benutzerdefinierter Sicherheitsfragen finden Sie unter [Was sind Authentifizierungsmethoden](concept-authentication-methods.md).

### <a name="registration-settings"></a>Registrierungseinstellungen

Legen Sie **Registrierung von Benutzern bei der Anmeldung verlangen** auf **Ja** fest. Diese Einstellung bedeutet, dass die Benutzer gezwungen werden, sich bei der Anmeldung zu registrieren, was den Schutz aller Benutzer sicherstellt.

Legen Sie **Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen** auf einen Wert zwischen **90** und **180** Tagen fest, es sei denn, für Ihre Organisation besteht die geschäftliche Anforderung eines kürzeren Zeitraums.

### <a name="notifications-settings"></a>Benachrichtigungseinstellungen

Konfigurieren Sie beide Einstellungen **Benutzer über Kennwortzurücksetzungen benachrichtigen?** und **Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen?** mit **Ja**. Das Auswählen von **Ja** für beide Einstellungen erhöht die Sicherheit, indem es sicherstellt, dass Benutzern bewusst ist, dass ihr Kennwort zurückgesetzt wurde und alle Administratoren wissen, wenn ein Administrator ein Kennwort geändert hat. Wenn Benutzer oder Administratoren eine derartige Benachrichtigung empfangen und die Änderung nicht veranlasst haben, können sie sofort eine mögliche Sicherheitsverletzung melden.

### <a name="customization"></a>Anpassung

Es ist wichtig, die **E-Mail-Adresse oder URL des Helpdesks** anzupassen, um sicherzustellen, dass Benutzer, bei denen Probleme auftreten, schnell Hilfe erhalten. Legen Sie diese Option auf eine allgemeine E-Mail-Adresse oder Website fest, die Ihren Benutzern vertraut ist.

### <a name="on-premises-integration"></a>Lokale Integration

Wenn Sie eine Hybridumgebung besitzen, stellen Sie sicher, dass **Zurückschreiben von Kennwörtern in das lokale AD** auf **Ja** festgelegt ist. Legen Sie außerdem die Einstellung „Benutzern das Entsperren des Kontos ohne Zurücksetzen des Kennworts erlauben“ auf „Ja“ fest, da dies ihnen mehr Flexibilität gibt.

### <a name="changingresetting-passwords-of-administrators"></a>Ändern/Zurücksetzen von Kennwörtern von Administratoren

Administratorkonten sind spezielle Konten mit erhöhten Berechtigungen. Um sie zu schützen, gelten für das Ändern von Kennwörtern von Administratoren die folgenden Einschränkungen:

- Lokale Enterprise-Administratoren oder Domänenadministratoren können ihr Kennwort nicht mithilfe von SSPR zurücksetzen. Sie können nur ihr Kennwort in ihrer lokalen Umgebung ändern. Daher empfehlen wir, lokale AD-Administratorkonten nicht mit Azure AD zu synchronisieren.
- Ein Administrator kann keine geheimen Fragen und Antworten als Methode zum Zurücksetzen von Kennwörtern verwenden.

### <a name="environments-with-multiple-identity-management-systems"></a>Umgebungen mit mehreren Identitätsverwaltungssystemen

Wenn es innerhalb einer Umgebung mehrere Identitätsverwaltungssysteme gibt, z.B. lokale Identitäts-Manager wie Oracle AM, SiteMinder oder andere Systeme, müssen in Active Directory geschriebene Kennwörter möglicherweise mithilfe eines Tools wie PCNS (Password Change Notification Service) mit Microsoft Identity Manager (MIM) per Synchronisierung auf die anderen Systeme übertragen werden. Informationen zu diesem komplexeren Szenario finden Sie im Artikel [Bereitstellen des MIM-Benachrichtigungsdiensts für Kennwortänderungen auf einem Domänencontroller](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller).

## <a name="plan-deployment-and-support-for-sspr"></a>Planen von Bereitstellung und Support für SSPR

### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten

Fehler in Technologieprojekten sind in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Um diese Fallstricke zu vermeiden, achten Sie darauf, die richtigen Beteiligten einzubeziehen und die Rolle der Beteiligten präzise zu definieren, indem Sie die Beteiligten, ihren Projektbeitrag und ihre Zuständigkeiten dokumentieren.

### <a name="communications-plan"></a>Kommunikationspläne

Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Kommunizieren Sie proaktiv mit Ihren Benutzern darüber, wie der Dienst verwendet werden soll und was sie tun können, wenn etwas nicht wie erwartet funktioniert. Lesen Sie die [Self-service password reset rollout materials on the Microsoft download center](https://www.microsoft.com/download/details.aspx?id=56768) (Rolloutmaterialien zur Self-Service-Kennwortzurücksetzung im Microsoft-Downloadcenter), um Ideen zum Planen Ihrer Endbenutzer-Kommunikationsstrategie zu erhalten.

### <a name="testing-plan"></a>Testplan

Um sicherzustellen, dass Ihre Bereitstellung wie erwartet funktioniert, sollten Sie eine Reihe von Testfällen planen, die Sie zum Überprüfen der Implementierung verwenden. Die folgende Tabelle enthält einige nützliche Testszenarien, die Sie zum Dokumentieren der auf der Grundlage Ihrer Richtlinien erwarteten Ergebnisse für Ihre Organisation verwenden können.

| Geschäftsszenario | Erwartetes Ergebnis |
| --- | --- |
| Auf das SSPR-Portal kann aus dem Unternehmensnetzwerk zugegriffen werden | Von Ihrer Organisation bestimmt |
| Auf das SSPR-Portal kann von außerhalb des Unternehmensnetzwerks zugegriffen werden | Von Ihrer Organisation bestimmt |
| Benutzerkennwort im Browser zurücksetzen, wenn für den Benutzer keine Kennwortzurücksetzung aktiviert ist | Der Benutzer kann nicht auf den Workflow zur Kennwortzurücksetzung zugreifen |
| Benutzerkennwort im Browser zurücksetzen, wenn sich der Benutzer nicht für die Kennwortzurücksetzung registriert hat | Der Benutzer kann nicht auf den Workflow zur Kennwortzurücksetzung zugreifen |
| Der Benutzer meldet sich an, wenn die Registrierung zur Kennwortzurücksetzung durchgesetzt wird | Der Benutzer wird aufgefordert, Sicherheitsinformationen zu registrieren |
| Der Benutzer meldet sich an, wenn die Registrierung zur Kennwortzurücksetzung abgeschlossen ist | Der Benutzer wird nicht aufgefordert, Sicherheitsinformationen zu registrieren |
| Auf das SSPR-Portal kann zugegriffen werden, wenn der Benutzer keine Lizenz besitzt | Zugriff möglich |
| Setzen Sie das Benutzerkennwort in Windows 10 AADJ oder dem H+AADJ-Gerätesperrbildschirm zurück, nachdem sich der Benutzer registriert hat | Benutzer kann Kennwort zurücksetzen |
| SSPR-Registrierungs- und Nutzungsdaten stehen Administratoren nahezu in Echtzeit zur Verfügung | Über die Überwachungsprotokolle verfügbar |

### <a name="support-plan"></a>Supportplan

Zwar bringt SSPR normalerweise keine Benutzerprobleme mit sich, es ist aber wichtig, über vorbereitete Supportmitarbeiter zu verfügen, die darauf vorbereitet sind, sich mit möglicherweise auftretenden Problemen zu befassen.

Zwar kann ein Administrator das Kennwort für Endbenutzer im Azure AD-Portal ändern oder zurücksetzen, es ist aber sinnvoller, die Lösung des Problems durch einen Self-Service-Supportprozess zu unterstützen.

Erstellen Sie im Abschnitt dieses Dokuments, das den Betriebsleitfaden enthält, eine Liste von Supportfällen und ihre wahrscheinlichen Ursachen, und erstellen Sie einen Leitfaden zur Behebung.

### <a name="auditing-and-reporting"></a>Überwachung und Berichterstellung

Nach einer Bereitstellung möchten viele Organisationen wissen, wie oder ob Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) tatsächlich verwendet wird. Über die Berichtsfunktion, die von Azure Active Directory (Azure AD) bereitgestellt wird, können Sie Fragen beantworten, indem Sie vordefinierte Berichte verwenden.

Überwachungsprotokolle für die Registrierung und die Kennwortzurücksetzung sind 30 Tage lang verfügbar. Wenn die Sicherheitsüberwachung innerhalb eines Unternehmens eine längere Aufbewahrungszeit erfordert, müssen die Protokolle exportiert und in einem SIEM-Tool wie etwa [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), Splunk oder ArcSight genutzt werden.

Dokumentieren Sie in einer Tabelle wie der unten abgebildeten den Sicherungszeitplan, das System und die zuständigen Parteien. Möglicherweise benötigen Sie keine separaten Überwachungs- und Berichtssicherungen, Sie sollten aber über eine separate Sicherung verfügen, aus der Sie im Fall eines Problems eine Wiederherstellung durchführen können.

|   | Downloadhäufigkeit | Zielsystem | Verantwortliche Partei |
| --- | --- | --- | --- |
| Überwachungssicherung |   |   |   |
| Berichtssicherung |   |   |   |
| Sicherung zur Notfallwiederherstellung |   |   |   |

## <a name="implementation"></a>Implementierung

Die Implementierung erfolgt in drei Phasen:

- Konfigurieren von Benutzern und Lizenzen
- Konfigurieren von Azure AD-SSPR für Registrierung und Self-Service
- Konfigurieren von Azure AD Connect für das Kennwortrückschreiben

### <a name="communicate-the-change"></a>Kommunizieren der Änderung

Beginnen Sie mit der Implementierung des Kommunikationsplans, den Sie in der Planungsphase entwickelt haben.

### <a name="ensure-groups-are-created-and-populated"></a>Stellen Sie sicher, dass Gruppen erstellt und aufgefüllt werden

Ziehen Sie die Referenzinformationen im Abschnitt „Planen der Methoden zur Kennwortauthentifizierung“ zu Rate, und stellen Sie sicher, dass die Gruppe(n) für die Pilot- oder Produktionsimplementierung verfügbar sind und den Gruppen alle Benutzer hinzugefügt wurden.

### <a name="apply-licenses"></a>Anwenden von Lizenzen

Den Gruppen, die Sie implementieren möchten, muss die Azure AD-Premiumlizenz zugewiesen sein. Sie können Lizenzen direkt zu Gruppen zuweisen oder vorhandene Lizenzrichtlinien verwenden, etwa mithilfe von PowerShell oder gruppenbasierter Lizenzierung.

Information über das Zuweisen von Lizenzen zu Gruppen von Benutzern finden Sie im Artikel [Zuweisen von Lizenzen zu Benutzern nach Gruppenmitgliedschaft in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md).

### <a name="configure-sspr"></a>Konfigurieren von SSPR

#### <a name="enable-groups-for-sspr"></a>Aktivieren von Gruppen für SSPR

1. Greifen Sie auf das Azure-Portal mit einem Administratorkonto zu.
1. Wählen Sie „Alle Dienste“ aus, geben Sie dann im Filterfeld „Azure Active Directory“ ein, und wählen Sie „Azure Active Directory“ aus.
1. Wählen Sie auf dem Blatt „Active Directory“ die Option „Kennwortzurücksetzung“ aus.
1. Wählen Sie im Eigenschaftenbereich „Ausgewählt“ aus. Wenn Sie alle Benutzer aktivieren möchten, wählen Sie „Alle auswählen“ aus.
1. Geben Sie auf dem Blatt „Richtlinie für die Zurücksetzung des Standardkennworts“ den Namen der ersten Gruppe ein, wählen Sie ihn aus, klicken Sie unten auf dem Bildschirm auf „Auswählen“, und wählen Sie dann oben auf dem Bildschirm „Speichern“ aus.
1. Wiederholen Sie diesen Vorgang für jede Gruppe.

#### <a name="configure-the-authentication-methods"></a>Konfigurieren der Authentifizierungsmethoden

Beziehen Sie sich anhand des Abschnitts „Planen von Methoden zur Kennwortauthentifizierung“ in diesem Dokument auf Ihre Planung.

1. Wählen Sie „Registrierung“ aus, wählen Sie unter „Registrierung von Benutzern bei der Anmeldung verlangen“ „Ja“ aus, legen Sie dann die Anzahl Tage bis zum Ablauf fest, und wählen Sie „Speichern“ aus.
1. Wählen Sie „Benachrichtigung“ aus, konfigurieren Sie gemäß Ihrem Plan, und wählen Sie dann „Speichern“ aus.
1. Wählen Sie „Anpassung“ aus, konfigurieren Sie gemäß Ihrem Plan, und wählen Sie dann „Speichern“ aus.
1. Wählen Sie „Lokale Integration“ aus, konfigurieren Sie gemäß Ihrem Plan, und wählen sie dann „Speichern“ aus.

### <a name="enable-sspr-in-windows"></a>Aktivieren von SSPR in Windows

Windows 10-Geräte, die Version 1803 oder höher ausführen und entweder Mitglied von Azure AD oder von einer Azure AD-Hybridumgebung sind, können ihre Kennwörter am Windows-Anmeldebildschirm zurücksetzen. Informationen und Schritte zum Konfigurieren dieser Funktion finden Sie im Artikel [Azure AD-Kennwortzurücksetzung über den Anmeldebildschirm](tutorial-sspr-windows.md)

### <a name="configure-password-writeback"></a>Konfigurieren von Kennwortrückschreiben

Schritte zum Konfigurieren des Kennwortrückschreibens für Ihre Organisation finden Sie im Artikel [Gewusst wie: Konfigurieren von Kennwortrückschreiben](howto-sspr-writeback.md).

## <a name="manage-sspr"></a>Verwalten von SSPR

Für die Verwaltung von Funktionen im Zusammenhang mit der Self-Service-Kennwortzurücksetzung erforderliche Rollen.

| Geschäftliche Rolle/Persona | Azure AD-Rolle (sofern erforderlich) |
| :---: | :---: |
| Helpdesk Ebene 1 | Kennwortadministrator |
| Helpdesk Ebene 2 | Benutzeradministrator |
| SSPR-Administrator | Globaler Administrator |

### <a name="support-scenarios"></a>Supportszenarien

Um den Erfolg Ihres Supportteams zu ermöglichen, können Sie auf der Grundlage der Fragen, die Ihnen von Benutzern gestellt werden, ein Dokument mit häufig gestellten Fragen erstellen. Die folgende Tabelle enthält häufige Supportszenarien.

| Szenarien | BESCHREIBUNG |
| --- | --- |
| Dem Benutzer stehen keine registrierten Authentifizierungsmethoden zur Verfügung | Ein Benutzer versucht, sein Kennwort zurückzusetzen, es ist aber keine der für ihn registrierten Authentifizierungsmethoden verfügbar (Beispiel: Handy zu Hause vergessen und kein Zugriff auf die E-Mail) |
| Der Benutzer empfängt keine SMS und keinen Anruf auf seinem Büro- oder Mobiltelefon | Ein Benutzer versucht, seine Identität per SMS oder Anruf zu bestätigen, empfängt aber keine SMS/keinen Anruf. |
| Benutzer können nicht auf das Portal für die Kennwortzurücksetzung zugreifen | Ein Benutzer möchte sein Kennwort zurücksetzen, ist aber nicht für die Kennwortzurücksetzung aktiviert und kann daher nicht auf die Seite zum Aktualisieren von Kennwörtern zugreifen. |
| Ein Benutzer kann kein neues Kennwort festlegen | Ein Benutzer schließt die Überprüfung während des Kennwortzurücksetzungs-Workflows ab, kann aber kein neues Kennwort festlegen. |
| Ein Benutzer sieht auf einem Windows 10-Gerät keinen Link „Kennwort zurücksetzen“ | Ein Benutzer versucht, sein Kennwort auf dem Windows 10-Sperrbildschirm zurückzusetzen, das Gerät ist aber entweder kein Azure AD-Mitglied, oder die Intune-Geräterichtlinie ist nicht aktiviert |

Für weiterführende Problembehandlung kann es auch sinnvoll sein, Informationen wie die folgenden mit aufzunehmen.

- Welche Gruppen sind für SSPR aktiviert
- Welche Authentifizierungsmethoden sind konfiguriert
- Die Zugriffsrichtlinien für das Unternehmensnetzwerk
- Schritte zur Problembehandlung bei häufigen Szenarien

Sie können auch unsere Onlinedokumentation zur Problembehandlung der Self-Service-Kennwortzurücksetzung zu Rate ziehen, um allgemeine Problembehandlungsschritte für die gängigsten SSPR-Szenarien zu verstehen.

## <a name="next-steps"></a>Nächste Schritte

- [Überlegungen zur Implementierung von Azure AD-Kennwortschutz](concept-password-ban-bad.md)

- [Überlegungen zur Implementierung von Smart Lockout für Azure AD](howto-password-smart-lockout.md)
