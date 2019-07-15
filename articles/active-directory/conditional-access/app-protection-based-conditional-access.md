---
title: Erzwingen der App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs in Azure Active Directory erzwingen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 4/4/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e2e43f13352c56f947f5e560049ab0acf871599
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509471"
---
# <a name="require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Erzwingen der App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs (Vorschauversion)

Ihre Mitarbeiter verwenden mobile Geräte sowohl für private als auch für berufliche Zwecke. Während Sie einerseits die Produktivität der Mitarbeiter sicherstellen möchten, sollten Sie andererseits Datenverluste vermeiden. Mit dem bedingten Zugriff mit Azure Active Directory (Azure AD) können Sie Ihre Unternehmensdaten schützen, indem Sie den Zugriff auf Ihre Cloud-Apps einschränken. Verwenden Sie Client-Apps zuerst mit einer Schutzrichtlinie für Anwendungen.

In diesem Artikel wird das Konfigurieren von Richtlinien für bedingten Zugriff erläutert, bei denen vor dem Erteilen des Zugriffs auf Daten eine App-Schutzrichtlinie erforderlich ist.

## <a name="overview"></a>Übersicht

Mit dem [bedingten Zugriff von Azure AD](overview.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Sie können z.B. den Zugriff auf Ihre Cloud-Apps auf vertrauenswürdige Geräte beschränken.

Zum Schutz Ihrer Unternehmensdaten können Sie [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden. Die Schutzrichtlinien für Intune-Apps erfordern keine MDM-Lösung (mobile Geräteverwaltung). Sie können die Daten Ihres Unternehmens mit oder ohne Registrierung von Geräten in einer Geräteverwaltungslösung schützen.

Der bedingte Zugriff von Azure Active Directory beschränkt den Zugriff auf Ihre Cloud-Apps auf Clientanwendungen, die Intune an Azure AD als Empfänger einer App-Schutzrichtlinie gemeldet hat. Sie können zum Beispiel den Zugriff auf Exchange Online auf die Outlook-App beschränken, die eine Intune-App-Schutzrichtlinie aufweist.

In der Terminologie des bedingten Zugriffs sind diese Client-Apps bekanntermaßen durch eine *App-Schutzrichtlinie* geschützt.  

![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)

Eine Liste der durch Richtlinien geschützten Client-Apps finden Sie unter [App-Schutzrichtlinie als Voraussetzung](technical-reference.md#approved-client-app-requirement).

Sie können Richtlinien für auf App-Schutz basierenden bedingten Zugriff mit anderen Richtlinien wie etwa [gerätebasierten Richtlinien für bedingten Zugriff](require-managed-devices.md) kombinieren. Auf diese Weise können Sie die Flexibilität beim Schutz von Daten für private und Unternehmensgeräte bereitstellen.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Vorteile des auf App-Schutz basierenden bedingten Zugriffs als Voraussetzung

Ähnlich wie bei Compliance, die von Intune für iOS und Android für ein verwaltetes Gerät gemeldet wird, teilt Intune Azure AD jetzt mit, ob eine App-Schutzrichtlinie angewendet wird. Für den bedingten Zugriff kann diese Richtlinie als Zugriffsüberprüfung verwendet werden. Diese neue Richtlinie für bedingten Zugriff, die App-Schutzrichtlinie, erhöht die Sicherheit. Sie schützt vor Administratorfehlern, z. B.:

- Benutzer ohne Intune-Lizenz.
- Benutzer, die keine Schutzrichtlinie für die Intune-App erhalten können.
- Apps mit Intune-App-Schutzrichtlinie, die nicht für den Erhalt einer Richtlinie konfiguriert sind.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind:

- Der technischen Referenz [App-Schutzrichtlinie als Voraussetzung](technical-reference.md#app-protection-policy-requirement)
- Der technischen Referenz [Genehmigte Client-App als Voraussetzung](technical-reference.md#approved-client-app-requirement)
- Den grundlegenden Konzepten des [bedingten Zugriffs in Azure Active Directory](overview.md).
- Der Vorgehensweise zum [Konfigurieren einer Richtlinie zum bedingten Zugriff](app-based-mfa.md).

## <a name="prerequisites"></a>Voraussetzungen

Um eine Richtlinie für auf App-Schutz basierenden bedingten Zugriff erstellen zu können, müssen Sie:

- Über ein Enterprise Mobility + Security- oder ein Azure Active Directory Premium-Abonnement und Intune verfügen
- Sicherstellen, dass die Benutzer für Enterprise Mobility + Security oder Azure AD + Intune lizenziert sind
- Sicherstellen, dass die Client-App in Intune für den Erhalt einer App-Schutzrichtlinie konfiguriert ist
- Sicherstellen, dass die Benutzer in Intune für den Erhalt einer Intune-App-Schutzrichtlinie konfiguriert wurden

## <a name="app-protection-based-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
- Er erhält eine E-Mail mit dem Hinweis, dass der Zugriff nur über die Outlook-App möglich ist.
- Er lädt die Anwendung über den Link herunter.
- Er öffnet die Outlook-Anwendung und meldet sich mit Azure AD-Anmeldeinformationen an.
- Er wird aufgefordert, entweder die **Microsoft Authenticator-App** oder das **Intune-Unternehmensportal** zu installieren, um den Vorgang fortzusetzen.
- Er installiert die Anwendung und kehrt zur Outlook-App zurück, um den Vorgang fortzusetzen.
- Er wird aufgefordert, ein Gerät zu registrieren.
- Er kann eine Intune-App-Schutzrichtlinie erhalten.
- Er kann auf E-Mails zugreifen.

Alle Intune App-Schutzrichtlinien müssen für die Anwendung gelten, um auf Unternehmensdaten zugreifen zu können. Die Richtlinien fordern den Benutzer möglicherweise auf, die Anwendung neu zu starten oder eine zusätzliche PIN zu verwenden. Dies ist der Fall, wenn die Richtlinien für die Anwendung und die Plattform konfiguriert sind.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/01.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus.

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Optionen **Geräteplattformen** und **Client-Apps (Vorschau)** :
   1. Wählen Sie in **Geräteplattformen** die Optionen **Android** und **iOS** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

   1. Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

1. Wählen Sie unter **Zugriffssteuerungen** die Option **App-Schutzrichtlinie erforderlich (Vorschau)** aus.

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit ActiveSync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus.

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Option **Client-Apps (Vorschau)** . 

   1. Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Exchange ActiveSync-Clients** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

   1. Wählen Sie unter **Zugriffssteuerungen** die Option **App-Schutzrichtlinie erforderlich (Vorschau)** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)

**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie oder Richtlinie für konforme Geräte für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff oder einer Richtlinie für bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für dieses Szenario:
 
- Ein Benutzer ist bereits mit Unternehmensgeräten oder ohne registriert.
- Benutzer, die nicht bei Azure AD registriert sind und eine durch eine App geschützte Anwendung verwenden, müssen ein Gerät registrieren, um auf Ressourcen zugreifen zu können.
- Registrierte Benutzer, die die durch eine App geschützte Anwendung verwenden, müssen das Gerät nicht erneut registrieren.
- Der Benutzer kann eine Intune-App-Schutzrichtlinie erhalten, wenn er nicht registriert ist.
- Der Benutzer kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen, wenn er nicht registriert ist.
- Der Benutzer kann mit Outlook auf E-Mails zugreifen, wenn das Gerät registriert ist.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/62.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus. 

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Optionen **Geräteplattformen** und **Client-Apps (Vorschau)** . 
 
   1. Wählen Sie in **Geräteplattformen** die Optionen **Android** und **iOS** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

   1. Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

5. Wählen Sie unter **Zugriffssteuerungen** die folgenden Optionen aus:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Eine der ausgewählten Steuerungen anfordern**   
 
      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/11.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit ActiveSync**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus. 

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Option **Client-Apps (Vorschau)** . 

   Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Exchange ActiveSync-Clients** aus.

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

1. Wählen Sie unter **Zugriffssteuerungen** die folgenden Optionen aus:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Eine der ausgewählten Steuerungen anfordern**

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/11.png)

**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie und Richtlinie für konforme Geräte für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff und einer Richtlinie für bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:
 
- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
- Er erhält eine E-Mail mit dem Hinweis, dass das Gerät für den Zugriff registriert werden muss.
- Er lädt das Intune-Unternehmensportal herunter und meldet sich beim Portal an.
- Er ruft seine E-Mails ab und wird zur Verwendung der Outlook-App aufgefordert.
- Er lädt die Outlook-App herunter.
- Er öffnet die Outlook-App und gibt die bei der Registrierung verwendeten Anmeldeinformationen ein.
- Er kann eine Intune-App-Schutzrichtlinie erhalten.
- Er kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen.

Alle Intune-App-Schutzrichtlinien werden aktiviert, bevor der Zugriff auf Unternehmensdaten gewährt wird. Die Richtlinien fordern den Benutzer möglicherweise auf, die Anwendung neu zu starten oder eine zusätzliche PIN zu verwenden. Dies ist der Fall, wenn die Richtlinien für die Anwendung und die Plattform konfiguriert sind.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/01.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus. 

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Optionen **Geräteplattformen** und **Client-Apps (Vorschau)** . 
   1. Wählen Sie in **Geräteplattformen** die Optionen **Android** und **iOS** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

   1. Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

1. Wählen Sie unter **Zugriffssteuerungen** die folgenden Optionen aus:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Alle ausgewählten Kontrollen anfordern**   
 
      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/13.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit ActiveSync**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus. 

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

1. Konfigurieren Sie in **Bedingungen** die Option **Client-Apps (Vorschau)** . 

   Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Exchange ActiveSync-Clients** aus.

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

1. Wählen Sie unter **Zugriffssteuerungen** die folgenden Optionen aus:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Alle ausgewählten Kontrollen anfordern**   
 
      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/13.png)

**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Auf App-Schutz basierende Richtlinie oder App-basierte Richtlinie für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer auf App-Schutz basierenden Richtlinie oder einer Richtlinie für genehmigte Apps für den Zugriff auf Exchange Online und SharePoint Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert die Clientanwendungen, die entweder in der Liste der Apps enthalten sind, die eine App-Schutzrichtlinie als Voraussetzung oder genehmigte Apps als Voraussetzung unterstützen.  
- Er verwendet Clientanwendungen, die die Anforderung „App-Schutzrichtlinie als Voraussetzung“ erfüllen und eine Intune App-Schutzrichtlinie erhalten können.
- Er verwendet Clientanwendungen, die die Richtlinienanforderung für genehmigte Apps (genehmigte Client-App als Voraussetzung) erfüllen, die Intune App-Schutzrichtlinien unterstützt.
- Er öffnet die Anwendung zum Zugreifen auf E-Mails oder Dokumente.
- Er öffnet die Outlook-Anwendung und meldet sich mit Azure AD-Anmeldeinformationen an.
- Er wird aufgefordert, entweder Microsoft Authenticator für iOS oder das Intune-Unternehmensportal für Android zu installieren, sofern dies noch nicht erfolgt ist.
- Er installiert die Anwendung und kann zur Outlook-App zurückkehren, um den Vorgang fortzusetzen.
- Er wird aufgefordert, ein Gerät zu registrieren.
- Er kann eine Intune-App-Schutzrichtlinie erhalten.
- Er kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen.
- Er kann mit einer App auf Websites und Dokumente zugreifen, die nicht in der Liste der Apps enthalten ist, für die eine App-Schutzrichtlinie als Voraussetzung gilt, sondern in der Liste der Apps enthalten ist, die genehmigte Apps als Voraussetzung unterstützen.

Alle Intune-App-Schutzrichtlinien sind erforderlich, bevor der Zugriff auf Unternehmensdaten gewährt wird. Die Richtlinien fordern den Benutzer möglicherweise auf, die Anwendung neu zu starten oder eine zusätzliche PIN zu verwenden. Dies ist der Fall, wenn die Richtlinien für die Anwendung und die Plattform konfiguriert sind.

**Anmerkungen**

- Sie können dieses Szenario verwenden, wenn Sie Richtlinien für auf App-Schutz basierenden bedingten Zugriff und Richtlinien für App-basierten bedingten Zugriff unterstützen möchten.
- Bei dieser *ODER*-Richtlinie werden Apps, für die eine App-Schutzrichtlinie als Voraussetzung gilt, vor Apps für den Zugriff ausgewertet, für die eine genehmigte Client-App als Voraussetzung gilt.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/62.png)

1. Geben Sie den Namen Ihrer Richtlinie für den bedingten Zugriff ein.
1. Wählen Sie unter **Zuweisungen** in **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe für jede Richtlinie für bedingten Zugriff aus.
1. Wählen Sie in **Cloud-Apps** die Option **Office 365 Exchange Online** aus. 

   ![Bedingter Zugriff](./media/app-protection-based-conditional-access/02.png)

1. Konfigurieren Sie in **Bedingungen** die Optionen **Geräteplattformen** und **Client-Apps (Vorschau)** . 
   1. Wählen Sie in **Geräteplattformen** die Optionen **Android** und **iOS** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

   1. Wählen Sie in **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung** aus.

      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

1. Wählen Sie unter **Zugriffssteuerungen** die folgenden Optionen aus:
   - **Genehmigte Client-App erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Eine der ausgewählten Steuerungen anfordern**
 
      ![Bedingter Zugriff](./media/app-protection-based-conditional-access/12.png)

**Schritt 2: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).
- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, helfen Ihnen die Informationen unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md) weiter.
