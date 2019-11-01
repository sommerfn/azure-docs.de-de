---
title: 'Bedingter Zugriff: Kombinierte Sicherheitsinformationen – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff, um einen vertrauenswürdigen Speicherort für die Registrierung von Sicherheitsinformationen zu erfordern
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 771e4e0ecbda4baf1f38aacd1f39397875bbd0dc
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73150766"
---
# <a name="conditional-access-require-trusted-location-for-mfa-registration"></a>Bedingter Zugriff: Vorschreiben eines vertrauenswürdigen Standorts für die MFA-Registrierung

Mit Benutzeraktionen in der Richtlinie für bedingten Zugriff kann jetzt sichergestellt werden, wann und wie sich Benutzer für Azure Multi-Factor Authentication registrieren, und auch die Self-Service-Kennwortzurücksetzung ist möglich. Diese Previewfunktion ist für Organisationen verfügbar, die die [kombinierte Registrierung (Vorschauversion)](../authentication/concept-registration-mfa-sspr-combined.md) aktiviert haben. Diese Funktionalität kann in Organisationen aktiviert werden, in denen sich Benutzer an einem zentralen Ort, z. B. einem vertrauenswürdigen Netzwerkspeicherort während des Onboarding-Prozesses für Personal, für Azure Multi-Factor Authentication und SSPR registrieren sollen. Weitere Informationen zur Erstellung von vertrauenswürdigen Speicherorten bei bedingtem Zugriff finden Sie im Artikel [Was sind Standortbedingungen beim bedingten Zugriff in Azure Active Directory?](../conditional-access/location-condition.md#named-locations)

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Erstellen einer Richtlinie zum Erzwingen der Registrierung von einem vertrauenswürdigen Standort

Die folgende Richtlinie gilt für alle ausgewählten Benutzer, die versuchen, sich über die Benutzeroberfläche für die kombinierte Registrierung zu registrieren. Der Zugriff wird blockiert, sofern die Verbindung nicht von einem Standort aus hergestellt wird, der als ein vertrauenswürdiges Netzwerk gekennzeichnet ist.

1. Navigieren Sie im **Azure-Portal** zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Geben Sie unter „Name“ einen Namen für diese Richtlinie ein. Beispiel: **Kombinierte Registrierung mit Sicherheitsinformationen in vertrauenswürdigen Netzwerken**.
1. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**, und wählen Sie die Benutzer und Gruppen aus, auf die Sie diese Richtlinie anwenden möchten.

   > [!WARNING]
   > Benutzer müssen für die [kombinierte Registrierung (Vorschauversion)](../authentication/howto-registration-mfa-sspr-combined.md) aktiviert sein.

1. Wählen Sie unter **Cloud-Apps oder -aktionen** die Option **Benutzeraktionen**, und aktivieren Sie **Sicherheitsinformationen registrieren (Vorschau)** .
1. Unter **Bedingungen** > **Standorte**:
   1. Konfigurieren Sie **Ja**.
   1. Schließen Sie **Alle Standorte** ein.
   1. Schließen Sie **Alle vertrauenswürdigen Standorte** aus.
   1. Klicken Sie auf dem Blatt „Standorte“ auf **Fertig**.
   1. Klicken Sie auf dem Blatt „Bedingungen“ auf **Fertig**.
1. Unter **Zugriffssteuerungen** > **Erteilen**:
   1. Klicken Sie auf **Zugriff blockieren**.
   1. Klicken Sie dann auf **Auswählen**.
1. Legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Klicken Sie dann auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
