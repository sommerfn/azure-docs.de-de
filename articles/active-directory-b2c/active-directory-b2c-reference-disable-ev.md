---
title: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C | Microsoft-Dokumentation
description: Dieses Thema veranschaulicht, wie Sie die E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d827843bdbaff1ad95dce3318f39867abe8b2d0f
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065617"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern – Azure Active Directory B2C
Wenn diese Funktion aktiviert ist, ermöglicht Azure Active Directory B2C (Azure AD B2C) den Endbenutzern, sich durch Angeben einer E-Mail-Adresse und Erstellen eines lokalen Kontos für Anwendungen zu registrieren. Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Endbenutzer diese während des Registrierungsvorgangs verifizieren müssen. B2C verhindert auch, dass ein automatisierter Prozess böswillig gefälschte Konten für die Anwendungen generiert.

Einige Anwendungsentwickler bevorzugen, die E-Mail-Überprüfung während des Registrierungsvorgangs zu überspringen und diese erst später vom Endbenutzer anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Endbenutzern zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

Standardmäßig ist die E-Mail-Überprüfung in Benutzerflows zur Registrierung aktiviert. Führen Sie zum Aktivieren der Funktion folgende Schritte aus:

1. Klicken Sie auf **Benutzerflows**.
2. Klicken Sie auf Ihren Benutzerflow (z.B. „B2C_1_SiUp“), um ihn zu öffnen.
3. Klicken Sie auf **Seitenlayouts**.
4. Klicken Sie auf **Registrierungsseite für lokales Konto**.
5. Klicken Sie im Abschnitt **Benutzerattribute** in der Spalte **Name** auf **E-Mail-Adresse**.
6. Wählen Sie unter **Verifizierung erforderlich** die Option **Nein** aus.
7. Klicken Sie oben auf dem Blatt auf **Speichern** . Sie haben es geschafft!

> [!NOTE]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie den Standardprozess deaktivieren, empfiehlt es sich, ein eigenes Überprüfungssystem einzurichten.
>
>
