---
title: Verwalten von Bedrohungen für Ressourcen in Azure Active Directory B2C
description: Informieren Sie sich über Verfahren zur Erkennung und Abwehr von Denial-of-Service- und Kennwortangriffen in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: eae82fbd56782faf5b15479c13fe530e189a0e3e
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256902"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Verwalten von Bedrohungen für Ressourcen in Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) verfügt über integrierte Features, mit denen Sie Ihre Ressourcen und Daten vor Bedrohungen schützen können. Zu diesen Bedrohungen gehören Denial-of-Service- und Kennwortangriffe. Denial-of-Service-Angriffe können dazu führen, dass Ressourcen nicht mehr für die vorgesehenen Benutzer zur Verfügung stehen. Kennwortangriffe führen zu nicht autorisiertem Zugriff auf Ressourcen.

## <a name="denial-of-service-attacks"></a>Denial-of-Service-Angriffe

Azure AD B2C dient zur Abwehr von SYN-Flutangriffen über ein SYN-Cookie. Außerdem schützt Azure AD B2C vor Denial-of-Service-Angriffen, indem Einschränkungen für Raten und Verbindungen festgelegt werden.

## <a name="password-attacks"></a>Kennwortangriffe

Benutzerkennwörter müssen ausreichend komplex sein. Azure AD B2C umfasst Abwehrmaßnahmen für Kennwortangriffe. Diese decken unter anderem die Erkennung von Brute-Force-Kennwortangriffen und Wörterbuchangriffen ab. Anhand verschiedener Signale analysiert Azure AD B2C die Integrität von Anforderungen. Azure AD B2C kann auf intelligente Weise zwischen vorgesehenen Benutzern und Hackern oder Botnetzen unterscheiden.

Azure AD B2C verwendet eine ausgereifte Strategie zum Sperren von Konten. Die Konten werden basierend auf der IP-Adresse der Anforderung und den eingegebenen Kennwörtern gesperrt. Die Dauer der Sperre wird zudem gemäß der Wahrscheinlichkeit erhöht, dass es sich um einen Angriff handelt. Nachdem ein Kennwort 10 Mal (der Standardschwellenwert für fehlgeschlagene Versuche) erfolglos eingegeben wurde, kommt es zu einer Sperre von einer Minute. Bei der nächsten fehlgeschlagenen Anmeldung nach der Entsperrung des Kontos (d. h., nachdem das Konto nach Ablauf des Sperrzeitraums automatisch vom Dienst entsperrt wurde) erfolgt eine weitere Sperre von 1 Minute für jede fehlgeschlagene Anmeldung. Die wiederholte Eingabe des gleichen Kennworts wird nicht als mehrere erfolglose Anmeldungen gezählt.

Die ersten 10 Sperrzeiträume sind eine Minute lang. Die nächsten 10 Sperrzeiträume sind etwas länger, und die Dauer wird jeweils nach 10 Sperrzeiträumen verlängert. Der Zähler für die Kontosperrung wird nach einer erfolgreichen Anmeldung auf null zurückgesetzt, wenn das Konto nicht gesperrt ist. Sperrzeiträume können bis zu fünf Stunden dauern.

## <a name="manage-password-protection-settings"></a>Verwalten der Kennwortschutzeinstellungen

So verwalten Sie die Kennwortschutzeinstellungen, einschließlich des Sperrschwellenwerts:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Verwenden Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter**, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie unter **Sicherheit** die Option **Authentifizierungsmethoden (Vorschau)** und dann **Kennwortschutz** aus.
1. Geben Sie die gewünschten Kennwortschutzeinstellungen ein, und klicken Sie dann auf **Speichern**.

    ![Seite „Kennwortschutz“ in den Azure AD-Einstellungen des Azure-Portals](media/active-directory-b2c-reference-threat-management/portal-02-password-protection.png)
    <br />*Festlegen des Sperrschwellenwerts auf 5 in den Einstellungen für **Kennwortschutz*** .

## <a name="view-locked-out-accounts"></a>Anzeigen gesperrter Konten

Informationen zu gesperrten Konten finden Sie im [Anmeldeaktivitätsbericht](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md) von Active Directory. Klicken Sie unter **Status** auf **Fehler**. Fehlgeschlagene Anmeldungen mit dem **Anmeldefehlercode** `50053` geben ein gesperrtes Konto an:

![Abschnitt des Azure AD-Anmeldeberichts, in dem ein gesperrtes Konto angezeigt wird](media/active-directory-b2c-reference-threat-management/portal-01-locked-account.png)

Informationen zum Anzeigen des Anmeldeaktivitätsberichts in Azure Active Directory finden Sie unter [Fehlercodes des Berichts mit den Anmeldeaktivitäten](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md).
