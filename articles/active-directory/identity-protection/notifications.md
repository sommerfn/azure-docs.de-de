---
title: Azure Active Directory Identity Protection-Benachrichtigungen | Microsoft Docs
description: Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9da0552e3ccc707c6b2f228b402f4e9db7dafee3
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125703"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen

Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoerkennungen dienen:

- E-Mail für erkannte gefährdete Benutzer
- Wöchentliche E-Mail mit Übersicht

Dieser Artikel gibt Ihnen eine Übersicht zu beiden Benachrichtigungs-E-Mails.

## <a name="users-at-risk-detected-email"></a>E-Mail für erkannte gefährdete Benutzer

Als Reaktion auf ein erkanntes gefährdetes Konto erstellt Azure Active Directory Identity Protection eine E-Mail-Warnung mit dem Betreff **Gefährdete Benutzer erkannt**. Die E-Mail enthält einen Link zum Bericht vom Typ **[Benutzer mit Risikokennzeichnung](../reports-monitoring/concept-user-at-risk.md)** . Als bewährte Methode sollten Sie die gefährdeten Benutzer sofort untersuchen.

Durch Konfiguration dieser Warnung können Sie angeben, bei welcher Benutzerrisikostufe die Warnung generiert werden soll. Die E-Mail wird generiert, wenn die Risikostufe des Benutzers den angegebenen Wert erreicht. Allerdings erhalten Sie für diesen Benutzer keine neuen E-Mail-Warnungen des Typs „Gefährdete Benutzer erkannt“ mehr, nachdem der Benutzer diese Risikostufe erreicht hat. Wenn Sie beispielsweise die Richtlinie für Warnungen auf ein mittleres Benutzerrisiko festlegen und der Benutzer Johann die mittlere Risikostufe erreicht, erhalten Sie für Johann eine E-Mail des Typs „Gefährdete Benutzer erkannt“. Sie erhalten allerdings keine zweite Warnung des Typs „Gefährdete Benutzer erkannt“, wenn Johann anschließend eine höhere Risikostufe erreicht oder zusätzliche Risikoerkennungen auftreten.

![E-Mail für erkannte gefährdete Benutzer](./media/notifications/01.png)

### <a name="configuration"></a>Konfiguration

Als Administrator können Sie Folgendes festlegen:

- **Die Benutzerrisikostufe, bei der die Erstellung dieser E-Mail ausgelöst wird**: Standardmäßig ist die Risikostufe auf „Hoch“ festgelegt.
- **Die Empfänger dieser E-Mail**: Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte hinzufügen.  

Um das zugehörige Dialogfeld zu öffnen, klicken auf der Seite **Identity Protection** im Abschnitt **Einstellungen** auf **Warnungen**.

![E-Mail für erkannte gefährdete Benutzer](./media/notifications/05.png)

## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht

Die wöchentliche Übersichts-E-Mail enthält eine Zusammenfassung der neuen Risikoerkennungen.  
Sie hat folgenden Inhalt:

- Gefährdete Benutzer
- Verdächtige Aktivitäten
- Erkannte Sicherheitsrisiken
- Links zu verwandten Berichten in Identity Protection

    ![Wiederherstellung](./media/notifications/400.png "Wiederherstellung")

### <a name="configuration"></a>Konfiguration

Als Administrator können Sie das Senden einer wöchentlichen Übersichts-E-Mail deaktivieren.

![Benutzerrisiken](./media/notifications/62.png "Benutzerrisiken")

Um das zugehörige Dialogfeld zu öffnen, klicken auf der Seite **Identity Protection** im Abschnitt **Einstellungen** auf **Wöchentliche Übersicht**.

![E-Mail für erkannte gefährdete Benutzer](./media/notifications/04.png)

## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
