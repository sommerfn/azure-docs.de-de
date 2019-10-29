---
title: Was ist Privileged Identity Management? – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ae9429920a4d3a6bac8830d6add2782276850d7
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595358"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Was ist Azure AD Privileged Identity Management?

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) ist ein Dienst, mit dem Sie den Zugriff auf wichtige Ressourcen innerhalb Ihrer Organisation verwalten, steuern und überwachen können. Diese Ressourcen umfassen Ressourcen in Azure AD, Azure und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.

## <a name="reasons-to-use"></a>Argumente für die Verwendung

Organisationen möchten die Anzahl von Personen mit Zugriff auf sichere Informationen oder Ressourcen möglichst gering halten, da sich dadurch das Risiko verringert, dass ein böswilliger Akteur Zugriff darauf erhält oder dass ein autorisierter Benutzer versehentlich eine sensible Ressource kompromittiert. Benutzer müssen jedoch in Azure AD, Azure, Office 365 oder SaaS-Apps weiterhin privilegierte Vorgänge ausführen. Organisationen können Benutzern privilegierten JIT-Zugriff (Just-In-Time) auf Azure-Ressourcen und Azure AD gewähren. Dabei muss jedoch überwacht werden, wofür diese Benutzer ihre Administratorrechte nutzen.

## <a name="what-does-it-do"></a>Behandelte Themen

Privileged Identity Management bietet eine zeit- und genehmigungsbasierte Rollenaktivierung, um die Risiken durch übermäßige, unnötige oder missbrauchte Zugriffsberechtigungen für wichtige Ressourcen zu verringern. Im Anschluss folgen einige der wichtigsten Features von Privileged Identity Management:

- Gewähren von privilegiertem **Just-In-Time**-Zugriff auf Azure AD- und Azure-Ressourcen
- Zuweisen von **zeitgebundenem** Zugriff auf Ressourcen (mit Start- und Enddatum)
- Anfordern einer **Genehmigung** bei der Aktivierung privilegierter Rollen
- Erzwingen der **mehrstufigen Authentifizierung** für alle Rollenaktivierungen
- Anfordern einer **Begründung**, um nachvollziehen zu können, warum Benutzer eine Aktivierung vornehmen
- Erhalten von **Benachrichtigungen**, wenn privilegierte Rollen aktiviert werden
- Durchführen von **Zugriffsüberprüfungen**, um zu prüfen, ob Benutzer die Rollen weiterhin benötigen
- Herunterladen eines **Überwachungsverlaufs** zur internen oder externen Überwachung

## <a name="what-can-i-do-with-it"></a>Verwendungsmöglichkeiten

Nachdem Sie Privileged Identity Management eingerichtet haben, werden im linken Navigationsmenü die Optionen **Aufgaben**, **Verwalten** und **Aktivität** angezeigt. Als Administrator können Sie zwischen der Verwaltung von **Azure AD-Rollen** und **Azure-Ressourcenrollen** wählen. Wenn Sie die Art der zu verwaltenden Rollen auswählen, werden ähnliche Optionen für diesen Rollentyp angezeigt.

![Screenshot: Privileged Identity Management im Azure-Portal](./media/pim-configure/pim-overview.png)

## <a name="who-can-do-what"></a>Berechtigungen und Rollen

Wenn Sie der erste Benutzer von Privileged Identity Management sind, werden Ihnen automatisch die Rollen [Sicherheitsadministrator](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) und [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) im Verzeichnis zugewiesen.

Für Azure AD-Rollen in Privileged Identity Management gilt: Nur Benutzer mit der Rolle „Administrator für privilegierte Rollen“ können Zuweisungen für andere Administratoren verwalten. Sie können [anderen Administratoren Zugriff für die Verwaltung von Privileged Identity Management gewähren](pim-how-to-give-access-to-pim.md). Globale Administratoren, Sicherheitsadministratoren, globale Leser und Benutzer mit Leseberechtigung für Sicherheitsfunktionen können auch Azure AD-Rollenzuweisungen in Privileged Identity Management anzeigen.

Bei Azure-Ressourcenrollen in Privileged Identity Management können nur Abonnementadministratoren, Ressourcenbesitzer und Ressourcen-Benutzerzugriffsadministratoren Zuweisungen für andere Administratoren verwalten. Administratoren für privilegierte Rollen, Sicherheitsadministratoren und Benutzer mit Leseberechtigung für Sicherheitsfunktionen haben standardmäßig keinen Zugriff auf Azure-Ressourcenrollenzuweisungen in Privileged Identity Management.

## <a name="scenarios"></a>Szenarien

Privileged Identity Management unterstützt folgende Szenarien:

### <a name="privileged-role-administrator-permissions"></a>Berechtigungen von Administratoren für privilegierte Rollen

- Aktivieren der Genehmigung für bestimmte Rollen
- Angeben von Genehmigungsbenutzern oder -gruppen für die Genehmigung von Anforderungen
- Anzeigen des Anforderungs- und Genehmigungsverlaufs für alle privilegierten Rollen

### <a name="approver-permissions"></a>Berechtigungen von genehmigenden Personen

- Anzeigen ausstehender Genehmigungen (Anforderungen)
- Genehmigen oder Ablehnen (einzelner oder mehrerer) Anforderungen zur Rechteerweiterung für Rollen
- Angeben einer Begründung für die Genehmigung/Ablehnung

### <a name="eligible-role-user-permissions"></a>Berechtigungen von Benutzern mit berechtigter Rolle

- Anfordern der Aktivierung einer Rolle, die genehmigt werden muss
- Anzeigen des Status Ihrer Aktivierungsanforderung
- Fertigstellen Ihrer Aufgabe in Azure AD, wenn die Aktivierung genehmigt wurde

## <a name="terminology"></a>Begriff

Machen Sie sich zum besseren Verständnis von Privileged Identity Management und der zugehörigen Dokumentation mit den folgenden Begriffen vertraut:

| Begriff oder Konzept | Rollenzuweisungskategorie | BESCHREIBUNG |
| --- | --- | --- |
| Berechtigt | type | Eine Rollenzuweisung, bei der ein Benutzer mindestens eine Aktion ausführen muss, um die Rolle nutzen zu können. Wenn ein Benutzer zu einer Rolle berechtigt ist, kann er die Rolle aktivieren, wenn er privilegierte Aufgaben ausführen muss. Es gibt keinen Unterschied hinsichtlich des Zugriffs zwischen einer permanenten und einer berechtigten Rollenzuweisung. Der einzige Unterschied ist, dass einige Benutzer den Zugriff nicht jederzeit benötigen. |
| aktiv | type | Eine Rollenzuweisung, bei der ein Benutzer keine Aktion ausführen muss, um die Rolle nutzen zu können. Bei als aktiv zugewiesenen Benutzern sind die Berechtigungen der Rolle zugewiesen. |
| aktivieren |  | Das Ausführen mindestens einer Aktion zum Verwenden einer Rolle, zu der der Benutzer berechtigt ist. Beispiele für Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung (MFA), die Angabe einer geschäftlichen Begründung oder das Anfordern einer Genehmigung von den angegebenen genehmigenden Personen. |
| zugewiesen | State | Ein Benutzer mit einer Rollenzuweisung vom Typ „aktiv“. |
| aktiviert | State | Ein Benutzer mit einer Rollenzuweisung vom Typ „berechtigt“, der die Aktionen zum Aktivieren der Rolle ausgeführt hat und nun aktiv ist.  Nach der Aktivierung kann der Benutzer die Rolle für einen vorkonfigurierten Zeitraum nutzen. Danach muss sie erneut aktiviert werden. |
| dauerhaft berechtigt | Duration | Eine Rollenzuweisung, bei der ein Benutzer immer zum Aktivieren der Rolle berechtigt ist. |
| dauerhaft aktiv | Duration | Eine Rollenzuweisung, bei der ein Benutzer die Rolle jederzeit ohne vorherige Aktion verwenden kann. |
| Ablauf (berechtigt) | Duration | Eine Rollenzuweisung, bei der ein Benutzer innerhalb eines angegebenen Zeitraums (Start- und Enddatum) zum Aktivieren der Rolle berechtigt ist. |
| Ablauf (aktiv) | Duration | Eine Rollenzuweisung, bei der ein Benutzer die Rolle innerhalb eines angegebenen Zeitraums (Start- und Enddatum) ohne vorherige Aktion verwenden kann. |
| Just-in-Time-Zugriff (JIT) |  | Ein Modell, bei dem Benutzer temporäre Berechtigungen zum Ausführen privilegierter Aufgaben erhalten. Dieses Modell verhindert, dass böswillige oder nicht autorisierte Benutzer nach dem Ablauf der Berechtigungen Zugriff erhalten. Der Zugriff wird nur gewährt, wenn Benutzer ihn benötigen. |
| Prinzip des Zugriffs mit den geringsten Rechten |  | Eine empfohlene Sicherheitsmethode, bei der alle Benutzer nur die zum Ausführen der Aufgaben, für die sie autorisiert sind, mindestens erforderlichen Berechtigungen erhalten. Diese Methode minimiert die Anzahl von globalen Administratoren, indem stattdessen spezifische Administratorrollen für bestimmte Szenarien verwendet werden. |

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Weitere Informationen zu Lizenzen für Benutzer finden Sie unter [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md).

## <a name="next-steps"></a>Nächste Schritte

- [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md)
- [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](pim-deployment-plan.md)
