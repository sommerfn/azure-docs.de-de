---
title: Schließen von aktiven Risikoereignissen in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Lernen Sie die Optionen zum Schließen von aktiven Risikoereignissen kennen.
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55c56674b04c4359fba741d10176fc91e3a991eb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109028"
---
# <a name="how-to-close-active-risk-events"></a>Anleitung: Schließen von aktiven Risikoereignissen

Mit [Risikoereignissen](../reports-monitoring/concept-risk-events.md) erkennt Azure Active Directory Indikatoren für möglicherweise kompromittierten Benutzerkonten. Als Administrator sollten Sie dafür sorgen, dass alle Risikoereignisse geschlossen werden, damit die betroffenen Benutzer nicht mehr gefährdet sind.

Dieser Artikel bietet eine Übersicht über die zusätzlichen Optionen zum Schließen von aktiven Risikoereignissen.

## <a name="options-to-close-risk-events"></a>Optionen zum Schließen von Risikoereignissen 

Der Status eines Risikoereignisses lautet entweder **Aktiv** oder **Geschlossen**. Alle aktiven Risikoereignisse fließen in die Berechnung eines Werts ein, der als „Benutzerrisikostufe“ bezeichnet wird. Die Benutzerrisikostufe ist ein Indikator (niedrig, mittel, hoch) für die Wahrscheinlichkeit, dass ein Konto kompromittiert wurde. 

Zum Schließen von aktiven Risikoereignissen haben Sie folgende Optionen:

- Kennwortzurücksetzung über eine Richtlinie zum Benutzerrisiko anfordern

- Manuelles Zurücksetzen des Kennworts
 
- Alle Risikoereignisse schließen 

- Einzelne Risikoereignisse manuell schließen



## <a name="require-password-reset-with-a-user-risk-policy"></a>Kennwortzurücksetzung über eine Richtlinie zum Benutzerrisiko anfordern

Durch Konfigurieren der [Richtlinie zum Benutzerrisiko für bedingten Zugriff](howto-user-risk-policy.md) können Sie eine Kennwortänderung erzwingen, wenn eine festgelegte Benutzerrisikostufe automatisch erkannt wurde. 

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/13.png)

Das Zurücksetzen des Kennworts schließt alle aktiven Risikoereignisse des entsprechenden Benutzers und versetzt die Identität wieder in einen sicheren Status. Die Verwendung eine Richtlinie zum Benutzerrisiko ist die bevorzugte Methode zum Schließen von aktiven Risikoereignissen, da diese Methode automatisiert ist. Es ist keine Interaktion zwischen dem betroffenen Benutzer und dem Helpdesk oder einem Administrator erforderlich.

Allerdings ist eine Richtlinie zum Benutzerrisiko nicht immer anwendbar. Dies gilt beispielsweise für:

- Benutzer, die nicht für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) registriert wurden
- Benutzer mit aktiven Risikoereignissen, die gelöscht wurden
- Eine Untersuchung, die ergibt, dass ein gemeldetes Risikoereignis vom berechtigten Benutzer durchgeführt wurde


## <a name="manual-password-reset"></a>Manuelles Zurücksetzen des Kennworts

Wenn keine Kennwortzurücksetzung durch eine Richtlinie zum Benutzerrisiko angefordert werden kann, können Sie alle Risikoereignisse für einen Benutzer durch eine manuelle Kennwortzurücksetzung schließen.

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/04.png)


Das zugehörige Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/05.png)


**Temporäres Kennwort generieren**: Durch das Generieren eines temporären Kennworts können Sie eine Identität umgehend in einen sicheren Status zurückversetzen. Diese Methode erfordert eine Interaktion mit den betroffenen Benutzern, da diese das temporäre Kennwort kennen müssen. Sie können das neue temporäre Kennwort beispielsweise an eine andere E-Mail-Adresse des Benutzers oder an den Manager des Benutzers senden. Da das Kennwort temporär ist, wird der Benutzer bei der nächsten Anmeldung aufgefordert, das Kennwort zu ändern.


**Benutzer zum Zurücksetzen des Kennworts auffordern**: Wenn der Benutzer zum Zurücksetzen von Kennwörtern aufgefordert wird, ist eine Self-Service-Wiederherstellung ohne Kontakt mit dem Helpdesk oder einem Administrator möglich. Wie eine Richtlinie zum Benutzerrisiko kann diese Methode nur auf Benutzer angewendet werden, die für MFA registriert sind. Für Benutzer, die noch nicht für MFA registriert sind, ist diese Option nicht verfügbar.


## <a name="dismiss-all-risk-events"></a>Alle Risikoereignisse schließen

Wenn eine Kennwortzurücksetzung keine Option für Sie ist, können Sie auch alle Risikoereignisse schließen. 

![Kennwort zurücksetzen](./media/howto-close-active-risk-events/03.png)

Wenn Sie auf **Dismiss all events**(Alle Ereignisse schließen) klicken, werden alle Ereignisse geschlossen, und der betroffene Benutzer ist nicht mehr gefährdet. Da diese Methode jedoch keine Auswirkungen auf das vorhandene Kennwort hat, bringt sie die zugehörige Identität nicht wieder in einen sicheren Zustand. Der bevorzugte Anwendungsfall für diese Methode ist ein gelöschter Benutzers mit aktiven Risikoereignissen. 


## <a name="close-individual-risk-events-manually"></a>Einzelne Risikoereignisse manuell schließen

Sie können einzelne Risikoereignisse manuell schließen. Durch das manuelle Schließen von Risikoereignissen können Sie die Benutzerrisikostufe herabsetzen. Risikoereignisse werden in der Regel als Reaktion auf eine diesbezügliche Untersuchung manuell geschlossen. Beispielsweise wenn ein Gespräch mit dem Benutzer ergibt, dass kein aktives Risikoereignis mehr erforderlich ist. 
 
Beim manuellen Schließen von Risikoereignissen können Sie die folgenden Aktionen durchführen, um den Status eines Risikoereignisses zu ändern:

![Aktionen](./media/howto-close-active-risk-events/06.png)

- **Lösen:** Wenn Sie nach dem Untersuchen eines Risikoereignisses außerhalb von Identity Protection eine geeignete Korrekturaktion durchgeführt haben und der Meinung sind, dass das Risikoereignis als geschlossen angesehen werden kann, können Sie das Ereignis als „Gelöst“ kennzeichnen. Für Ereignisse mit dem Status „Aufgelöst“ wird der Status auf „Geschlossen“ festgelegt, und das Risikoereignis fließt nicht mehr in das Benutzerrisiko ein.

- **Als falsch positives Ergebnis kennzeichnen:** In einigen Fällen kann die Untersuchung eines Risikoereignisses ergeben, dass es fälschlicherweise als risikobehaftet gekennzeichnet wurde. Sie können zur Reduzierung der Anzahl solcher Vorfälle beitragen, indem Sie das Risikoereignis als falsch positives Ergebnis kennzeichnen. Auf diese Weise können die Machine Learning-Algorithmen die Klassifizierung ähnlicher Ereignisse in Zukunft verbessern. Der Status der falsch positiven Ereignisse wird auf „Geschlossen“ festgelegt, und sie fließen nicht mehr in die Berechnung des Benutzerrisikos ein.

- **Ignorieren:** Falls Sie keine Korrekturaktion durchgeführt haben und das Risikoereignis trotzdem aus der Liste „Aktiv“ entfernen möchten, können Sie ein Risikoereignis mit „Ignorieren“ kennzeichnen. Der Ereignisstatus wird dann in „Geschlossen“ geändert. Ignorierte Ereignisse gehen nicht in das Benutzerrisiko ein. Diese Option sollte nur unter ungewöhnlichen Umständen verwendet werden.

- **Reaktivieren:** Risikoereignisse, die manuell geschlossen wurden (durch Auswahl von „Lösen“, „Falsch positiv“ oder „Ignorieren“) können reaktiviert werden. Hierfür wird der Ereignisstatus zurück auf „Aktiv“ gesetzt. Reaktivierte Risikoereignisse fließen in die Berechnung des Benutzerrisikos ein. Risikoereignisse, die per Korrekturmaßnahme (z. B. das sichere Zurücksetzen des Kennworts) geschlossen werden, können nicht reaktiviert werden.
  

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview.md).
