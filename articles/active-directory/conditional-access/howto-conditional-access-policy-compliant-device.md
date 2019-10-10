---
title: 'Bedingter Zugriff: Anfordern von kompatiblen Geräten – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff, um konforme Geräte zu anzufordern.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1963d0ad741099fcb6602beb58f62b0eabf01aa0
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170060"
---
# <a name="conditional-access-require-compliant-devices"></a>Bedingter Zugriff: Anfordern von kompatiblen Geräten

Unternehmen, die Microsoft Intune bereitgestellt haben, können die von ihren Geräten zurückgegebenen Informationen verwenden, um Geräte zu identifizieren, die den Compliancevorgaben entsprechen, z. B.:

* Erfordern einer PIN zum Entsperren
* Erfordern der Geräteverschlüsselung
* Erfordern einer minimalen oder maximalen Betriebssystemversion
* Erfordern, dass ein Gerät keinen Jailbreak oder Rootzugriff verwendet

Diese Informationen zur Richtliniencompliance werden an Azure AD weitergeleitet, wo Entscheidungen zum bedingten Zugriff zur Gewährung oder Sperrung des Zugriffs auf Ressourcen treffen kann.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Die folgenden Schritte helfen bei der Erstellung einer Richtlinie für bedingten Zugriff, bei der Geräte, die auf Ressourcen zugreifen, als mit den Compliancerichtlinien von Intune Ihres Unternehmens konform gekennzeichnet werden.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** aus.
   1. Wenn Sie bestimmte Anwendungen von Ihrer Richtlinie ausschließen müssen, können Sie sie auf der Registerkarte **Ausschließen** unter **Ausgeschlossene Cloudanwendungen auswählen** die Option **Auswählen** auswählen.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Markieren des Geräts als kompatibel erforderlich** aus.
   1. Wählen Sie **Auswählen**.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Richtlinien für Gerätekonformität funktionieren mit Azure AD](https://docs.microsoft.com/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
