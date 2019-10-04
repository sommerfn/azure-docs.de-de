---
title: Vorschreiben der Verwendung von genehmigten Client-Apps für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs von Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verwendung genehmigter Client-Apps für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs von Azure Active Directory vorschreiben können.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45eeef7d96f194e224e5b44421e73eb5ee5d9c0d
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68515153"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Anleitung: Vorschreiben der Verwendung von genehmigten Client-Apps für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs 

Ihre Mitarbeiter verwenden mobile Geräte sowohl für private als auch für berufliche Zwecke. Während Sie einerseits die Produktivität der Mitarbeiter sicherstellen möchten, sollten Sie andererseits Datenverluste vermeiden. Mit dem bedingten Zugriff mit Azure Active Directory (Azure AD) können Sie den Zugriff auf Ihre Cloud-Apps auf genehmigte Client-Apps beschränken, die Ihre Unternehmensdaten schützen können.  

In diesem Thema wird erläutert, wie Sie Richtlinien für den bedingten Zugriff konfigurieren, die genehmigte Client-Apps erfordern.

## <a name="overview"></a>Übersicht

Mit dem [bedingten Zugriff von Azure AD](overview.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Sie können z.B. den Zugriff auf Ihre Cloud-Apps auf vertrauenswürdige Geräte beschränken.

Zum Schutz Ihrer Unternehmensdaten können Sie [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden. Da die Intune-Richtlinien für den App-Schutz keine MDM-Lösung (Mobile-Device Management, Verwaltung mobiler Geräte) erfordern, sind Ihre Unternehmensdaten unabhängig davon geschützt, ob Sie die Geräte in einer Lösung für die Geräteverwaltung registrieren oder nicht.

Der bedingte Zugriff mit Azure Active Directory ermöglicht es Ihnen, den Zugriff auf Ihre Cloud-Apps auf Client-Apps zu beschränken, die Intune-Richtlinien für den App-Schutz unterstützen. Sie können zum Beispiel den Zugriff auf Exchange Online für die Outlook-App einschränken.

Im Kontext des bedingten Zugriffs werden diese Client-Apps als **genehmigte Client-Apps** bezeichnet.  

![Bedingter Zugriff](./media/app-based-conditional-access/05.png)

Eine Liste der genehmigten Client-Apps finden Sie unter [Genehmigte Client-App als Voraussetzung](technical-reference.md#approved-client-app-requirement).

App-basierte bedingte Zugriffsrichtlinien lassen sich mit anderen Richtlinien wie etwa [gerätebasierten Richtlinien zum bedingten Zugriff](require-managed-devices.md) kombinieren und sorgen so für Flexibilität beim Schutz von Daten für Privat- und Unternehmensgeräte.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:

- Der technischen Referenz [Genehmigte Client-App als Voraussetzung](technical-reference.md#approved-client-app-requirement)
- Den grundlegenden Konzepten des [bedingten Zugriffs in Azure Active Directory](overview.md).
- Der Vorgehensweise zum [Konfigurieren einer Richtlinie zum bedingten Zugriff](app-based-mfa.md).
- Die [Migration von Richtlinien für bedingten Zugriff](best-practices.md#policy-migration).

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen einer Richtlinie zum App-basierten bedingten Zugriff benötigen Sie ein Abonnement vom Typ „Enterprise Mobility + Security“ oder „Azure Active Directory Premium“, und die Benutzer müssen für EMS oder Azure AD lizenziert sein. 

## <a name="exchange-online-policy"></a>Exchange Online-Richtlinie 

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
- Er erhält eine E-Mail mit dem Hinweis, dass der Zugriff nur über die Outlook-App möglich ist.
- Er lädt die Anwendung über den Link herunter.
- Er öffnet die Outlook-Anwendung und meldet sich mit den Azure AD-Anmeldeinformationen an.
- Er wird aufgefordert, entweder Authenticator (iOS) oder das Unternehmensportal (Android) zu installieren, um den Vorgang fortzusetzen.
- Er installiert die Anwendung und kann zur Outlook-App zurückkehren, um den Vorgang fortzusetzen.
- Er wird aufgefordert, ein Gerät zu registrieren.
- Er kann auf die E-Mails zugreifen.

Intune-Richtlinien für den App-Schutz werden beim Zugriff auf Unternehmensdaten aktiviert, und der Benutzer wird unter Umständen aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).

### <a name="configuration"></a>Konfiguration 

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen.
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren:
   1. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.
   1. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Clients mit moderner Authentifizierung**.
1. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

   ![Bedingter Zugriff](./media/app-based-conditional-access/05.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen.
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps (Vorschauversion)** konfigurieren. 
   1. Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.
   1. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

      ![Bedingter Zugriff](./media/app-based-conditional-access/05.png)

**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="exchange-online-and-sharepoint-online-policy"></a>Richtlinie für Exchange Online und SharePoint Online

Dieses Szenario umfasst eine Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps für den Zugriff auf Exchange Online und SharePoint Online mit genehmigten Apps.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er versucht, mit der SharePoint-App eine Verbindung herzustellen und Unternehmenswebsites anzuzeigen.
- Er versucht, sich mit den gleichen Anmeldeinformationen anzumelden, die er auch für die Outlook-App verwendet.
- Er muss sich nicht erneut registrieren und kann auf die Ressourcen zugreifen.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** und **Office 365 SharePoint Online** auswählen. 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren:
   1. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.
   1. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung**.
1. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

   ![Bedingter Zugriff](./media/app-based-conditional-access/05.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. Online 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps** konfigurieren:
   1. Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.
   1. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

      ![Bedingter Zugriff](./media/app-based-conditional-access/05.png)

**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-basierte Richtlinie oder Richtlinie für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff oder einer Richtlinie für den bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für dieses Szenario:
 
- Einige Benutzer sind bereits registriert (mit Unternehmensgeräten oder ohne).
- Benutzer, die nicht bei Azure AD registriert sind und eine durch eine App geschützte Anwendung verwenden, müssen ein Gerät registrieren, um auf Ressourcen zugreifen zu können.
- Registrierte Benutzer, die die durch eine App geschützte Anwendung verwenden, müssen das Gerät nicht erneut registrieren.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** und **Office 365 SharePoint Online** auswählen. 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren. 
   1. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.
   1. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung**.
1. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **Genehmigte Client-App erforderlich (Vorschau)**
   - **Eine der ausgewählten Steuerungen anfordern**   
 
      ![Bedingter Zugriff](./media/app-based-conditional-access/11.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps** konfigurieren. 
   1. Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.
1. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.
 
   ![Bedingter Zugriff](./media/app-based-conditional-access/11.png)

**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-basierte Richtlinie und Richtlinie für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff und einer Richtlinie für den bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:
 
- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
- Er erhält eine E-Mail mit dem Hinweis, dass das Gerät für den Zugriff registriert werden muss.
- Er lädt das Unternehmensportal herunter und meldet sich an.
- Er ruft seine E-Mails ab und wird zur Verwendung der Outlook-App aufgefordert.
- Er lädt die Outlook-App herunter.
- Er öffnet die Outlook-App und gibt die bei der Registrierung verwendeten Anmeldeinformationen ein.
- Der Benutzer kann auf die E-Mails zugreifen.

Intune-Richtlinien für den App-Schutz werden beim Zugriff auf die Unternehmensdaten aktiviert, und der Benutzer wird unter Umständen aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** und **Office 365 SharePoint Online** auswählen. 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren. 
   1. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.
   1. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Clients mit moderner Authentifizierung**.
1. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **Genehmigte Client-App erforderlich (Vorschau)**
   - **Alle ausgewählten Kontrollen anfordern**   
 
      ![Bedingter Zugriff](./media/app-based-conditional-access/13.png)

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.
1. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.
1. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 
1. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps (Vorschauversion)** konfigurieren. 
   1. Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.

   ![Bedingter Zugriff](./media/app-based-conditional-access/92.png)

1. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:
   - **Markieren des Geräts als kompatibel erforderlich**
   - **Genehmigte Client-App erforderlich (Vorschau)**
   - **Alle ausgewählten Kontrollen anfordern**   

**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**

![Bedingter Zugriff](./media/app-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).

Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, helfen Ihnen die Informationen unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md) weiter. 
