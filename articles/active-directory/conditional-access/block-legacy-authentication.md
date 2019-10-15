---
title: Blockieren der Legacyauthentifizierung bei Azure Active Directory (Azure AD) mit bedingtem Zugriff | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie durch Blockieren der Legacyauthentifizierung mithilfe des bedingten Azure AD-Zugriffs Ihren Sicherheitsstatus verbessern.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4e4dc33d670c5f6c5ebefa21ccf1a1ff941e913
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72024578"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Gewusst wie: Blockieren der Legacyauthentifizierung bei Azure AD mit bedingtem Zugriff   

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure Active Directory (Azure AD) eine Vielzahl von Authentifizierungsprotokollen einschließlich der Legacyauthentifizierung. Ältere Protokolle unterstützen jedoch nicht die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). MFA ist in vielen Umgebungen eine allgemeine Anforderung zum Schutz vor Identitätsdiebstahl. 

Wenn Ihre Umgebung für das Blockieren der Legacyauthentifizierung bereit ist, um den Schutz Ihres Mandanten zu verbessern, können Sie dieses Ziel mit bedingtem Zugriff erreichen. In diesem Artikel wird erläutert, wie Sie die Richtlinien für bedingten Zugriff konfigurieren können, mit denen die Legacyauthentifizierung für Ihren Mandanten blockiert wird.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind: 

- Den [grundlegenden Konzepten](overview.md) des bedingten Azure AD-Zugriffs 
- Den [Best Practices](best-practices.md) für das Konfigurieren von Richtlinien für bedingten Zugriff im Azure-Portal

## <a name="scenario-description"></a>Beschreibung des Szenarios

Azure AD unterstützt mehrere der am häufigsten verwendeten Protokolle zur Authentifizierung und Autorisierung, einschließlich der Legacyauthentifizierung. Die Legacyauthentifizierung bezieht sich auf Protokolle, die die Standardauthentifizierung verwenden. In der Regel können diese Protokolle keinen Typ der zweistufigen Authentifizierung erzwingen. Beispiele für Apps, die auf der Legacyauthentifizierung basieren, sind:

- Ältere Microsoft Office-Apps
- Apps, die E-Mail-Protokolle wie POP, IMAP und SMTP verwenden

Eine einstufige Authentifizierung (z. B. mit Benutzername und Kennwort) reicht heutzutage nicht mehr aus. Kennwörter sind unzulänglich, weil sie leicht zu erraten sind, und wir (Menschen) sind schlecht darin, gute Kennwörter auszuwählen. Kennwörter sind auch für eine Vielzahl von Angriffen wie Phishing und Kennwort-Spray anfällig. Eine der einfachsten Maßnahmen, die Sie ergreifen können, um sich vor Kennwortsicherheitsverletzungen zu schützen, ist das Implementieren von MFA. Denn selbst wenn ein Angreifer in den Besitz des Kennworts eines Benutzers gelangt, reicht bei Verwendung von MFA das Kennwort allein nicht aus, um sich erfolgreich authentifizieren und auf die Daten zugreifen zu können.

Wie können Sie verhindern, dass Apps mit Legacyauthentifizierung auf Ressourcen Ihres Mandanten zugreifen? Es wird empfohlen, diese Apps einfach mit einer Richtlinie für bedingten Zugriff zu blockieren. Gegebenenfalls können Sie nur bestimmten Benutzern und bestimmten Netzwerkadressen die Verwendung von Apps erlauben, die auf der Legacyauthentifizierung basieren.

Richtlinien für den bedingten Zugriff werden durchgesetzt, nachdem die First-Factor-Authentifizierung abgeschlossen ist. Daher ist der bedingte Zugriff nicht als erste Abwehrmaßnahme für Szenarien wie Denial-of-Service-Angriffe (DoS) gedacht, sondern kann Signale von diesen Ereignissen (z. B. der Risikostufe für die Anmeldung, den Standort der Anforderung usw.) nutzen, um den Zugriff zu bestimmen.

## <a name="implementation"></a>Implementierung

In diesem Abschnitt wird erläutert, wie eine Richtlinie für bedingten Zugriff zum Blockieren der Legacyauthentifizierung konfiguriert wird. 

### <a name="identify-legacy-authentication-use"></a>Identifizieren der Verwendung der Legacyauthentifizierung

Bevor Sie die Legacyauthentifizierung in Ihrem Verzeichnis blockieren können, müssen Sie zuerst wissen, ob Ihre Benutzer über Apps verfügen, die die Legacyauthentifizierung verwenden, und wie sich dies auf Ihr gesamtes Verzeichnis auswirkt. Sie können Azure AD-Anmeldungsprotokolle verwenden, um herauszufinden, ob Sie die Legacyauthentifizierung verwenden.

1. Navigieren Sie zu **Azure-Portal** > **Azure Active Directory** > **Anmeldungen**.
1. Falls die Spalte „Client-App“ nicht angezeigt wird, fügen Sie sie durch Klicken auf **Spalten** > **Client-App** hinzu.
1. **Filter hinzufügen** > **Client-App** > alle Optionen für **Andere Clients** auswählen und auf **Übernehmen** klicken.

Durch das Filtern werden Ihnen nur Anmeldeversuche von Legacyauthentifizierungsprotokollen angezeigt. Bei Klicken auf jeden einzelnen Anmeldeversuch werden Ihnen weitere Details angezeigt. Das **Client-App**-Feld auf der Registerkarte **Grundlegende Informationen** gibt an, welche Legacyauthentifizierungsprotokolle verwendet wurden.

Diese Protokolle geben an, welche Benutzer weiterhin von der Legacyauthentifizierung abhängig sind, und welche Anwendungen ältere Protokolle für Authentifizierungsanforderungen verwenden. Implementieren Sie für Benutzer, die in diesen Protokollen nicht aufgeführt sind und nachweislich keine Legacyauthentifizierung verwenden, eine Richtlinie für bedingten Zugriff, die nur für diese Benutzer vorgesehen ist.

### <a name="block-legacy-authentication"></a>Blockieren älterer Authentifizierungsmethoden 

In einer Richtlinie für bedingten Zugriff können Sie eine Bedingung festlegen, die an die Client-Apps gebunden ist, die für den Zugriff auf Ihre Ressourcen verwendet werden. Mit der Client-App-Bedingung können Sie den Bereich für Apps mit Legacyauthentifizierung eingrenzen, indem Sie für **Mobile Apps und Desktopclients** die Option **Andere Clients** auswählen.

![Andere Clients](./media/block-legacy-authentication/01.png)

Um den Zugriff für diese Apps zu blockieren, müssen Sie **Zugriff blockieren** auswählen.

![Zugriff blockieren](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Auswählen von Benutzern und Cloud-Apps

Wenn Sie die Legacyauthentifizierung für Ihre Organisation blockieren möchten, denken Sie wahrscheinlich, dass Sie dies durch Auswählen der folgenden Optionen erreichen können:

- Alle Benutzer
- Alle Cloud-Apps
- Zugriff blockieren

![Arbeitsaufträge](./media/block-legacy-authentication/03.png)

Azure verfügt über eine Sicherheitsfunktion, die Sie am Erstellen einer solchen Richtlinie hindert, weil diese Konfiguration gegen die [Best Practices](best-practices.md) für Richtlinien für bedingten Zugriff verstößt.
 
![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/04.png)

Diese Sicherheitsfunktion ist erforderlich, weil das *Blockieren aller Benutzer und aller Cloud-Apps* potenziell dazu führen kann, für Ihre gesamte Organisation die Anmeldung bei Ihrem Mandanten zu blockieren. Sie müssen mindestens einen Benutzer davon ausschließen, um die Mindestanforderung der Best Practices zu erfüllen. Sie können auch eine Verzeichnisrolle ausschließen.

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/05.png)

Sie können diese Sicherheitsfunktion erfüllen, indem Sie einen Benutzer aus der Richtlinie ausschließen. Im Idealfall sollten Sie einige [Administratorkonten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md) definieren und diese aus der Richtlinie ausschließen.

## <a name="policy-deployment"></a>Richtlinienbereitstellung

Kümmern Sie sich vor dem Einsatz der Richtlinie in der Produktionsumgebung um Folgendes:
 
- **Dienstkonten**: Identifizieren Sie Benutzerkonten, die als Dienstkonten verwendet werden oder nach Geräten, wie Telefone im Konferenzraum. Stellen Sie sicher, dass diese Konten über sichere Kennwörter verfügen, und fügen Sie sie einer ausgeschlossenen Gruppe hinzu.
- **Anmeldeberichte**: Überprüfen Sie den Anmeldebericht, und suchen Sie nach dem Datenverkehr **anderer Clients**. Identifizieren Sie die höchste Nutzung, und untersuchen Sie den Grund für die Verwendung. In der Regel wird der Datenverkehr von älteren Office-Clients generiert, die keine moderne Authentifizierung oder einige E-Mail-Apps von Drittanbietern verwenden. Erarbeiten Sie einen Plan, um die Nutzung dieser Apps zu reduzieren, oder wenn die Auswirkungen gering sind, teilen Sie Ihren Benutzern mit, dass sie diese Apps nicht mehr verwenden können.
 
Weitere Informationen finden Sie unter [Wie stellen Sie eine neue Richtlinie bereit?](best-practices.md#how-should-you-deploy-a-new-policy)

## <a name="what-you-should-know"></a>Wichtige Informationen

Das Blockieren des Zugriffs mithilfe der Bedingung **Andere Clients** blockiert auch Exchange Online PowerShell und Dynamics 365 mit Standardauthentifizierung.

Durch das Konfigurieren einer Richtlinie für **Andere Clients** wird die gesamte Organisation für bestimmte Clients blockiert, z.B. SPConnect. Dies tritt ein, weil sich ältere Clients auf unerwartete Weise authentifizieren. Dieses Problem gilt nicht für Office-Hauptanwendungen wie ältere Office-Clients.

Es kann bis zu 24 Stunden dauern, bis die Richtlinie wirksam wird.

Sie können alle verfügbaren Gewährungssteuerelemente für die Bedingung **Andere Clients** auswählen. Die Endbenutzererfahrung ist jedoch immer die gleiche: Der Zugriff ist blockiert.

Wenn Sie die Legacyauthentifizierung mit der Bedingung **Andere Clients** blockieren, können Sie auch die Geräteplattform und den Speicherort als Bedingung festlegen. Wenn Sie nur ältere Authentifizierungen für mobile Geräte blockieren möchten, legen Sie z.B. die Bedingung **Geräteplattformen** fest, indem Sie Folgendes auswählen:

- Android
- iOS
- Windows Phone

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie noch nicht mit dem Konfigurieren von Richtlinien für bedingten Zugriff vertraut sind, sehen Sie sich das Beispiel unter [Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md) an.
- Weitere Informationen zur Unterstützung der modernen Authentifizierung finden Sie unter [So funktioniert die moderne Authentifizierung für Office 2013- und Office 2016-Client-Apps](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016). 
