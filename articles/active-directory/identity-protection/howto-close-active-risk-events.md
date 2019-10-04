---
title: Schließen von aktiven Risikoerkennungen in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Lernen Sie die Optionen zum Schließen von aktiven Risikoerkennungen kennen.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d24ec94d0381fc2e79fdef97b6d525b7cec33fef
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126473"
---
# <a name="how-to-close-active-risk-detections"></a>Anleitung: Schließen von aktiven Risikoerkennungen

Mit [Risikoerkennungen](../reports-monitoring/concept-risk-events.md) erkennt Azure Active Directory Indikatoren für möglicherweise kompromittierte Benutzerkonten. Als Administrator sollten Sie dafür sorgen, dass alle Risikoerkennungen geschlossen werden, damit die betroffenen Benutzer nicht mehr gefährdet sind.

Dieser Artikel bietet eine Übersicht über die zusätzlichen Optionen zum Schließen von aktiven Risikoerkennungen.

## <a name="options-to-close-risk-detections"></a>Optionen zum Schließen von Risikoerkennungen 

Der Status einer Risikoerkennung lautet entweder **Aktiv** oder **Geschlossen**. Alle aktiven Risikoerkennungen fließen in die Berechnung eines Werts ein, der als „Benutzerrisikostufe“ bezeichnet wird. Die Benutzerrisikostufe ist ein Indikator (niedrig, mittel, hoch) für die Wahrscheinlichkeit, dass ein Konto kompromittiert wurde. 

Zum Schließen von aktiven Risikoerkennungen haben Sie folgende Optionen:

- Kennwortzurücksetzung über eine Richtlinie zum Benutzerrisiko anfordern
- Manuelles Zurücksetzen des Kennworts
- Alle Risikoerkennungen schließen 
- Einzelne Risikoerkennungen manuell schließen

## <a name="require-password-reset-with-a-user-risk-policy"></a>Kennwortzurücksetzung über eine Richtlinie zum Benutzerrisiko anfordern

Durch Konfigurieren der [Richtlinie zum Benutzerrisiko für bedingten Zugriff](howto-user-risk-policy.md) können Sie eine Kennwortänderung erzwingen, wenn eine festgelegte Benutzerrisikostufe automatisch erkannt wurde. 

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/13.png)

Das Zurücksetzen des Kennworts schließt alle aktiven Risikoereignisse des entsprechenden Benutzers und versetzt die Identität wieder in einen sicheren Status. Die Verwendung einer Benutzerrisiko-Richtlinie ist die bevorzugte Methode zum Schließen von aktiven Risikoerkennungen, da diese Methode automatisiert ist. Es ist keine Interaktion zwischen dem betroffenen Benutzer und dem Helpdesk oder einem Administrator erforderlich.

Allerdings ist eine Richtlinie zum Benutzerrisiko nicht immer anwendbar. Dies gilt beispielsweise für:

- Benutzer, die nicht für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) registriert wurden
- Benutzer mit aktiven Risikoerkennungen, die gelöscht wurden
- Eine Untersuchung, die ergibt, dass eine gemeldete Risikoerkennung vom berechtigten Benutzer durchgeführt wurde

## <a name="manual-password-reset"></a>Manuelles Zurücksetzen des Kennworts

Wenn keine Kennwortzurücksetzung durch eine Benutzerrisiko-Richtlinie angefordert werden kann, können Sie alle Risikoerkennungen für einen Benutzer durch eine manuelle Kennwortzurücksetzung schließen.

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/04.png)

Das zugehörige Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/05.png)

**Temporäres Kennwort generieren**: Durch das Generieren eines temporären Kennworts können Sie eine Identität umgehend in einen sicheren Status zurückversetzen. Diese Methode erfordert eine Interaktion mit den betroffenen Benutzern, da diese das temporäre Kennwort kennen müssen. Sie können das neue temporäre Kennwort beispielsweise an eine andere E-Mail-Adresse des Benutzers oder an den Manager des Benutzers senden. Da das Kennwort temporär ist, wird der Benutzer bei der nächsten Anmeldung aufgefordert, das Kennwort zu ändern.

**Benutzer zum Zurücksetzen des Kennworts auffordern**: Wenn der Benutzer zum Zurücksetzen von Kennwörtern aufgefordert wird, ist eine Self-Service-Wiederherstellung ohne Kontakt mit dem Helpdesk oder einem Administrator möglich. Wie eine Richtlinie zum Benutzerrisiko kann diese Methode nur auf Benutzer angewendet werden, die für MFA registriert sind. Für Benutzer, die noch nicht für MFA registriert sind, ist diese Option nicht verfügbar.

## <a name="dismiss-all-risk-detections"></a>Alle Risikoerkennungen schließen

Wenn eine Kennwortzurücksetzung keine Option für Sie ist, können Sie auch alle Risikoerkennungen schließen. 

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/03.png)

Wenn Sie auf **Dismiss all events**(Alle Ereignisse schließen) klicken, werden alle Ereignisse geschlossen, und der betroffene Benutzer ist nicht mehr gefährdet. Da diese Methode jedoch keine Auswirkungen auf das vorhandene Kennwort hat, bringt sie die zugehörige Identität nicht wieder in einen sicheren Zustand. Der bevorzugte Anwendungsfall für diese Methode ist ein gelöschter Benutzer mit aktiven Risikoerkennungen. 

## <a name="close-individual-risk-detections-manually"></a>Einzelne Risikoerkennungen manuell schließen

Sie können einzelne Risikoerkennungen manuell schließen. Durch das manuelle Schließen von Risikoerkennungen können Sie die Benutzerrisikostufe herabsetzen. Risikoerkennungen werden in der Regel als Reaktion auf eine entsprechende Untersuchung manuell geschlossen. Dies ist beispielsweise der Fall, wenn ein Gespräch mit dem Benutzer ergibt, dass keine aktive Risikoerkennung mehr erforderlich ist. 
 
Beim manuellen Schließen von Risikoerkennungen können Sie die folgenden Aktionen ausführen, um den Status einer Risikoerkennung zu ändern:

![Aktionen](./media/howto-close-active-risk-events/06.png)

- **Lösen:** Wenn Sie nach dem Untersuchen einer Risikoerkennung außerhalb von Identity Protection eine geeignete Korrekturaktion ausgeführt haben und der Meinung sind, dass die Risikoerkennung als geschlossen angesehen werden kann, können Sie das Ereignis als „Gelöst“ kennzeichnen. Für Erkennungen mit dem Status „Gelöst“ wird der Status auf „Geschlossen“ festgelegt, und die Risikoerkennung fließt nicht mehr in das Benutzerrisiko ein.
- **Als falsch positives Ergebnis kennzeichnen:** In einigen Fällen kann die Untersuchung einer Risikoerkennung ergeben, dass sie fälschlicherweise als risikobehaftet gekennzeichnet wurde. Sie können zur Reduzierung der Anzahl solcher Vorkommen beitragen, indem Sie die Risikoerkennung als falsch positives Ergebnis kennzeichnen. Auf diese Weise können die Machine Learning-Algorithmen die Klassifizierung ähnlicher Ereignisse in Zukunft verbessern. Der Status der falsch positiven Ereignisse wird auf „Geschlossen“ festgelegt, und sie fließen nicht mehr in die Berechnung des Benutzerrisikos ein.
- **Ignorieren:** Wenn Sie keine Korrekturaktion ausgeführt haben und die Risikoerkennung dennoch aus der Liste „Aktiv“ entfernen möchten, können Sie die Risikoerkennung mit „Ignorieren“ kennzeichnen. Der Ereignisstatus wird dann in „Geschlossen“ geändert. Ignorierte Ereignisse gehen nicht in das Benutzerrisiko ein. Diese Option sollte nur unter ungewöhnlichen Umständen verwendet werden.
- **Reaktivieren:** Risikoerkennungen, die manuell geschlossen wurden (durch Auswahl von „Lösen“, „Falsch positiv“ oder „Ignorieren“) können reaktiviert werden. Hierfür wird der Ereignisstatus zurück auf „Aktiv“ gesetzt. Reaktivierte Risikoerkennungen fließen in die Berechnung des Benutzerrisikos ein. Risikoerkennungen, die per Korrekturmaßnahme (z. B. das sichere Zurücksetzen des Kennworts) geschlossen werden, können nicht reaktiviert werden.

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview.md).
