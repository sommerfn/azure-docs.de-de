---
title: Azure Active Directory Identity Protection-Playbook| Microsoft Docs
description: Erfahren Sie, wie Sie mit Azure AD Identity Protection für Angreifer die Möglichkeit einschränken können, eine kompromittierte Identität oder ein Gerät auszunutzen, und wie Sie eine Identität oder ein Gerät schützen, das zuvor vermutlich oder mit Sicherheit kompromittiert war.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 273a6aca2050676650b955ec078b47b2ffcfe319
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333927"
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory Identity Protection-Playbook

Dieses Playbook hilft Ihnen:

* Daten in der Identity Protection Umgebung durch das Simulieren von Risikoereignissen und Sicherheitsrisiken aufzufüllen
* Richtlinien für den risikobasierten bedingten Zugriff einzurichten und die Auswirkungen dieser Richtlinien zu testen

## <a name="simulating-risk-events"></a>Simulieren von Risikoereignissen

Dieser Abschnitt bietet Ihnen die Schritte zum Simulieren der folgenden Risikoereignistypen:

* Anmeldungen von anonymen IP-Adressen (einfach)
* Anmeldungen von unbekannten Standorten (mittelschwer)
* Unmöglicher Ortswechsel zu atypischen Orten (schwierig)

Andere Risikoereignisse können nicht auf sichere Weise simuliert werden.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anmeldungen von anonymen IP-Adressen

Weitere Informationen über dieses Risikoereignis finden Sie unter [Anmeldungen von anonymen IP-Adressen](../reports-monitoring/concept-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

Für die folgenden Schritte müssen folgende Voraussetzungen erfüllt sein:

- Sie benötigen den [Tor-Browser](https://www.torproject.org/projects/torbrowser.html.en), um anonyme IP-Adressen zu simulieren. Möglicherweise müssen Sie einen virtuellen Computer verwenden, wenn Ihre Organisation die Verwendung des Tor-Browsers einschränkt.
- Sie benötigen ein Testkonto, das noch nicht für die mehrstufige Authentifizierung registriert ist.

**Führen Sie die folgende Schritte aus, um eine Anmeldung über eine anonyme IP-Adresse zu simulieren,** :

1. Navigieren Sie im [Tor-Browser](https://www.torproject.org/projects/torbrowser.html.en) zu [https://myapps.microsoft.com](https://myapps.microsoft.com).   
2. Geben Sie die Anmeldeinformationen des Kontos ein, das im Bericht **Anmeldungen von anonymen IP-Adressen** enthalten sein soll.

Die Anmeldung wird innerhalb von fünf bis zehn Minuten auf dem Identity Protection Dashboard angezeigt. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Anmeldungen von unbekannten Standorten

Weitere Informationen über dieses Risikoereignis finden Sie unter [Anmeldungen von unbekannten Standorten](../reports-monitoring/concept-risk-events.md#sign-in-from-unfamiliar-locations). 

Um unbekannte Standorte zu simulieren, müssen Sie sich von einem Ort und mit einem Gerät anmelden, bei dem das Testkonto noch nicht angemeldet war.

Das unten stehende Verfahren verwendet folgende neu erstellte Elemente:

- Eine VPN-Verbindung zum Simulieren eines neuen Standorts
- Einen virtuellen Computer zum Simulieren eines neuen Geräts

Für das folgende Verfahren müssen Sie ein Benutzerkonto verwenden, das folgende Voraussetzungen erfüllt:

- Anmeldeverlauf von mindestens 30 Tagen
- Aktivierte mehrstufige Authentifizierung

**Gehen Sie folgendermaßen vor, um eine Anmeldung von einem unbekannten Ort zu simulieren**:

1. Wenn Sie sich mit dem Testkonto anmelden, verursachen Sie einen Fehler bei der mehrstufigen Authentifizierung.
2. Navigieren Sie in Ihrem neuen VPN zu [https://myapps.microsoft.com](https://myapps.microsoft.com), und geben Sie die Anmeldeinformationen Ihres Testkontos ein.

Die Anmeldung wird innerhalb von fünf bis zehn Minuten auf dem Identity Protection Dashboard angezeigt.

### <a name="impossible-travel-to-atypical-location"></a>Unmöglicher Ortswechsel zu atypischen Orten

Weitere Informationen zu diesem Risikoereignis finden Sie unter [Unmöglicher Ortswechsel zu atypischen Orten](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations). 

Das Simulieren des unmöglichen Ortswechsels ist schwierig, da der Algorithmus einen Machine Learning-Ansatz nutzt, um falsch positive Ergebnisse auszusieben, z. B. der unmögliche Ortswechsel vertrauter Geräte oder Anmeldungen über VPNs, die von anderen Benutzern im Verzeichnis verwendet werden. Außerdem sind für den Algorithmus ein Anmeldeverlauf von 14 Tagen und zehn Anmeldungen des Benutzers erforderlich, bevor mit dem Generieren von Risikoereignissen begonnen wird. Aufgrund der komplexen Machine Learning-Modelle und der oben genannten Regeln besteht die Möglichkeit, dass die nachfolgenden Schritte nicht zu einem Risikoereignis führen. Sie können diese Schritte für mehrere Azure AD-Konten replizieren, um dieses Risikoereignis zu veröffentlichen.

**Führen Sie die folgenden Schritte aus, um einen unmöglichen Ortswechsel zu einen atypischen Ort zu simulieren**:

1. Navigieren Sie in Ihrem Standardbrowser zu [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Geben Sie die Anmeldeinformationen des Kontos ein, für das Sie ein Risikoereignis vom Typ „Unmöglicher Ortswechsel“ generieren möchten.
3. Ändern Sie Ihren Benutzer-Agent. Sie können den Benutzer-Agent in Internet Explorer über die Entwicklertools ändern. Ändern Sie den Benutzer-Agent in Firefox oder Chrome mit einem geeigneten Add-On (User-Agent Switcher).
4. Ändern Sie Ihre IP-Adresse. Sie können die IP-Adresse ändern, indem Sie ein VPN oder ein Tor-Add-On verwenden oder unter Azure einen neuen Computer in einem anderen Rechenzentrum einrichten.
5. Melden Sie sich unter [https://myapps.microsoft.com](https://myapps.microsoft.com) mit den gleichen Anmeldeinformationen wie vorher und innerhalb von wenigen Minuten nach der vorherigen Anmeldung an.

Die Anmeldung wird innerhalb von zwei bis vier Stunden auf dem Identity Protection Dashboard angezeigt.

## <a name="simulating-vulnerabilities"></a>Simulieren von Sicherheitsrisiken
Sicherheitsrisiken sind Schwachstellen in einer Azure AD-Umgebung, die von einem Angreifer ausgenutzt werden können. Derzeit werden in Azure AD Identity Protection drei Arten von Sicherheitsrisiken erfasst, für die andere Features von Azure AD genutzt werden. Diese Sicherheitsrisiken werden im Identity Protection Dashboard automatisch angezeigt, nachdem diese Features eingerichtet wurden.

* Azure AD – [Multi-Factor Authentication](../authentication/multi-factor-authentication.md)
* Azure AD [Cloud Discovery](https://docs.microsoft.com/cloud-app-security/)
* Azure AD [Privileged Identity Management](../privileged-identity-management/pim-configure.md) 

## <a name="testing-security-policies"></a>Testen von Sicherheitsrichtlinien

Dieser Abschnitt erläutert die Schritte, die Sie zum Testen der Sicherheitsrichtlinie für Benutzerrisiko und Anmelderisiko ausführen müssen.

### <a name="user-risk-security-policy"></a>Benutzerrisiko-Sicherheitsrichtlinie

Weitere Informationen finden Sie unter [Gewusst wie: Konfigurieren von Richtlinien zum Benutzerrisiko](howto-user-risk-policy.md).

![Benutzerrisiko](./media/playbook/02.png "Playbook")

**Um eine Benutzerrisiko-Sicherheitsrichtlinie zu testen, führen Sie die folgenden Schritte aus**:

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) mit den Anmeldeinformationen für „Globaler Administrator“ für Ihren Mandanten an.
2. Navigieren Sie zu **Identity Protection**. 
3. Klicken Sie auf der Seite **Azure AD Identity Protection** auf **Richtlinie zum Benutzerrisiko**.
4. Wählen Sie im Abschnitt **Zuweisungen** die gewünschten Benutzer (und Gruppen) und die gewünschte Benutzerrisikostufe.

    ![Benutzerrisiko](./media/playbook/03.png "Playbook")

5. Wählen Sie im Abschnitt „Steuerelemente“ die gewünschte Zugriffssteuerung aus (z.B. „Kennwortänderung anfordern“).
5. Legen Sie **Richtlinie erzwingen** auf **Aus** fest.
6. Erhöhen Sie das Benutzerrisiko eines Testkontos, indem Sie beispielsweise eines der Risikoereignisse mehrmals simulieren.
7. Warten Sie einige Minuten, und vergewissern Sie sich dann, dass die Stufe des Benutzers „Mittel“ lautet. Ist dies nicht der Fall, simulieren Sie weitere Risikoereignisse für den Benutzer.
8. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
9. Sie können den auf dem Benutzerrisiko basierenden bedingten Zugriff jetzt testen, indem Sie sich mit einem Benutzer anmelden, für den eine erhöhte Risikostufe gilt.

### <a name="sign-in-risk-security-policy"></a>Anmelderisiko-Sicherheitsrichtlinie

Weitere Informationen finden Sie unter [Gewusst wie: Konfigurieren von Richtlinien zum Anmelderisiko](howto-sign-in-risk-policy.md).

![Anmelderisiko](./media/playbook/01.png "Playbook")

**Führen Sie die folgenden Schritte aus, um eine Anmelderisiko-Sicherheitsrichtlinie zu testen:**

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) mit den Anmeldeinformationen für „Globaler Administrator“ für Ihren Mandanten an.
2. Navigieren Sie zu **Azure AD Identity Protection**.
3. Klicken Sie auf der Hauptseite von **Azure AD Identity Protection** auf **Richtlinie zum Anmelderisiko**. 
4. Wählen Sie im Abschnitt **Zuweisungen** die gewünschten Benutzer (und Gruppen) und die gewünschte Anmelderisikostufe.

    ![Anmelderisiko](./media/playbook/04.png "Playbook")

5. Wählen Sie im Abschnitt **Steuerelemente** die gewünschte Zugriffssteuerung aus (z.B. **Mehrstufige Authentifizierung erforderlich**). 
6. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
7. Klicken Sie auf **Speichern**.
8. Sie können den auf dem Anmelderisiko basierenden bedingten Zugriff jetzt testen, indem Sie sich mithilfe einer potenziell risikobehafteten Sitzung anmelden (z. B. durch Verwendung des Tor-Browsers). 

## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
