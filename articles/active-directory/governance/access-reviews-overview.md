---
title: Worum handelt es sich bei Zugriffsüberprüfungen? – Azure Active Directory | Microsoft-Dokumentation
description: Mithilfe von Azure Active Directory-Zugriffsüberprüfungen können Sie Gruppenmitgliedschaften und den Anwendungszugriff steuern, um die Anforderungen von Governance-, Risikomanagement- und Konformitätsinitiativen in Ihrer Organisation zu erfüllen.
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 08/05/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65def17622f026aa4869a4c60e7cb5146d56c5b0
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389526"
---
# <a name="what-are-azure-ad-access-reviews"></a>Was sind Azure AD-Zugriffsüberprüfungen?

Mithilfe von Azure Active Directory-Zugriffsüberprüfungen (Azure AD-Zugriffsüberprüfungen) können Organisationen Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Personen weiterhin Zugriff haben.

Das folgende Video bietet einen kurzen Überblick über Zugriffsüberprüfungen:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Warum sind Zugriffsüberprüfungen wichtig?

Azure AD ermöglicht Ihnen die interne Zusammenarbeit mit Mitarbeitern in Ihrer Organisation sowie die Zusammenarbeit mit Benutzern aus externen Organisationen, z. B. mit Partnern. Benutzer können Gruppen beitreten, Gäste einladen, Verbindungen mit Cloud-Apps herstellen und mit ihren Firmen- oder persönlichen Geräten remote arbeiten. Die Annehmlichkeit, die Möglichkeiten von Self-Service nutzen zu können, hat zur Notwendigkeit besserer Zugriffsverwaltungsfunktionen geführt.

- Wie stellen Sie sicher, dass neue Mitarbeiter die richtigen Zugriffsrechte erhalten, damit sie produktiv arbeiten können?
- Wie stellen Sie sicher, dass die alten Zugriffsrechte von Personen entfernt werden, die Teams wechseln oder das Unternehmen verlassen, insbesondere wenn es sich um Gäste handelt?
- Zu viele Zugriffsrechte können zu Auditergebnissen und Sicherheitsrisiken führen, weil sie auf eine mangelnde Kontrolle über den Zugriff hinweisen.
- Sie müssen sich proaktiv mit Ressourcenbesitzern austauschen, um sicherzustellen, dass sie regelmäßig überprüfen, wer Zugriff auf ihre Ressourcen hat.

## <a name="when-to-use-access-reviews"></a>In welchen Fällen sollten Zugriffsüberprüfungen verwendet werden?

- **Zu viele Benutzer in privilegierten Rollen:** Sie sollten überprüfen, wie viele Benutzer über Administratorzugriff verfügen, wie viele dieser Benutzer globale Administratoren sind und ob es eingeladene Gäste oder Partner gibt, die nicht entfernt wurden, nachdem ihnen eine administrative Aufgabe zugewiesen wurde. Sie können die Rollenzuweisung für Benutzer in [Azure AD-Rollen](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) wie globale Administratoren oder in [Azure-Ressourcenrollen](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) wie Benutzerzugriffsadministratoren in [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) erneut bestätigen.
- **Wenn eine Automatisierung nicht möglich ist:** Sie können Regeln für dynamische Mitgliedschaften für Sicherheits- oder Office 365-Gruppen erstellen. Was geschieht aber, wenn die Personaldaten nicht in Azure AD gespeichert sind oder die Benutzer nach dem Verlassen der Gruppe weiterhin Zugriff benötigen, um ihre Nachfolger einzuarbeiten? Dann können Sie eine Überprüfung für diese Gruppe erstellen, um sicherzustellen, dass die Benutzer, die weiterhin Zugriff benötigen, auch weiterhin Zugriff haben.
- **Wenn eine Gruppe zu einem neuen Zweck verwendet wird:** Wenn Sie über eine Gruppe verfügen, die mit Azure AD synchronisiert wird, oder wenn Sie die Salesforce-Anwendung für alle Benutzer in der Gruppe „Vertriebsteam“ aktivieren möchten, wäre es sinnvoll, den Besitzer der Gruppe zu bitten, die Gruppenmitgliedschaft zu überprüfen, bevor die Gruppe in einem anderen Risikokontext verwendet wird.
- **Zugriff auf unternehmenskritische Daten:** Bei bestimmten Ressourcen kann es zu Überwachungszwecken erforderlich sein, die Mitarbeiter außerhalb der IT-Abteilung zu bitten, sich regelmäßig abzumelden und zu begründen, warum sie Zugriff benötigen.
- **Zum Verwalten einer Richtlinienausnahmeliste:** Im Idealfall halten sich alle Benutzer an die Zugriffsrichtlinien, um den Zugriff auf die Ressourcen Ihrer Organisation zu schützen. Es kann aber auch Geschäftsszenarien geben, in denen Ausnahmen erforderlich sind. Als IT-Administrator können Sie diese Aufgabe verwalten, das Übersehen von Richtlinienausnahmen vermeiden und Prüfern den Nachweis erbringen, dass diese Ausnahmen regelmäßig überprüft werden.
- **Auffordern von Gruppenbesitzern zur Bestätigung, dass sie weiterhin Gäste in ihren Gruppen benötigen:** Der Mitarbeiterzugriff kann mit lokalem Identity & Access Management (IAM) automatisiert werden. Dies trifft jedoch nicht auf die Zugriffsrechte eingeladener Gäste zu. Wenn eine Gruppe Gästen Zugriff auf vertrauliche Unternehmensinhalte gewährt, muss der Besitzer der Gruppe bestätigen, dass für die Gäste immer noch eine berechtigte geschäftliche Notwendigkeit des Zugriffs besteht.
- **Regelmäßige Durchführung von Überprüfungen:** Sie können die Häufigkeit der regelmäßigen Durchführung von Zugriffsüberprüfungen für Benutzer (z.B. wöchentlich, monatlich, vierteljährlich oder jährlich) einrichten. Die Prüfer werden zu Beginn jeder Überprüfung benachrichtigt. Prüfer können den Zugriff über eine benutzerfreundliche Oberfläche und mithilfe intelligenter Empfehlungen genehmigen oder verweigern.

## <a name="where-do-you-create-reviews"></a>Wo erstellen Sie Überprüfungen?

Je nachdem, was Sie überprüfen möchten, erstellen Sie Ihre Zugriffsüberprüfung in Azure AD-Zugriffsüberprüfungen, Azure AD-Unternehmens-Apps (in der Vorschauversion) oder Azure AD PIM.

| Zugriffsrechte von Benutzern | Prüfer | Überprüfung erstellt in | Prüferoberfläche |
| --- | --- | --- | --- |
| Mitglieder von Sicherheitsgruppen</br>Mitglieder von Office-Gruppen | Angegebene Prüfer</br>Gruppenbesitzer</br>Selbstüberprüfung | Azure AD-Zugriffsüberprüfungen</br>Azure AD-Gruppen | Zugriffsbereich |
| Einer verbundenen App zugewiesen | Angegebene Prüfer</br>Selbstüberprüfung | Azure AD-Zugriffsüberprüfungen</br>Azure AD-Unternehmens-Apps (in der Vorschauversion) | Zugriffsbereich |
| Azure AD-Rolle | Angegebene Prüfer</br>Selbstüberprüfung | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure-Portal |
| Azure-Ressourcenrolle | Angegebene Prüfer</br>Selbstüberprüfung | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure-Portal |


## <a name="onboard-access-reviews"></a>Zugriffsüberprüfungen integrieren

Führen Sie die folgenden Schritte aus, um das Onboarding für Zugriffsüberprüfungen auszuführen.

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim [Azure-Portal](https://portal.azure.com) an, in dem Sie Zugriffsüberprüfungen verwenden möchten.

1. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

1. Klicken Sie im linken Menü auf **Identity Governance**.

1. Klicken Sie auf **Zugriffsüberprüfungen**.
 
    ![Startseite für Zugriffsüberprüfungen](./media/access-reviews-overview/access-reviews-overview-onboard.png)

1. Klicken Sie auf der Seite auf die Schaltfläche **Jetzt integrieren**.
    
      ![Zugriffsüberprüfungen integrieren](./media/access-reviews-overview/access-reviews-overview-select-onboard.png)


## <a name="learn-about-access-reviews"></a>Informationen zu Zugriffsüberprüfungen

Schauen Sie sich das folgende kurze Demovideo an, um mehr über das Erstellen und Durchführen von Zugriffsüberprüfungen zu erfahren:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Wenn Sie Zugriffsüberprüfungen in Ihrer Organisation bereitstellen möchten, folgen Sie den im Video gezeigten Schritten, um Zugriffsüberprüfungen zu integrieren, Ihre Administratoren zu schulen und Ihre erste Zugriffsüberprüfung zu erstellen!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

### <a name="which-users-must-have-licenses"></a>Welche Benutzer benötigen Lizenzen?

Jeder Benutzer, der mit Zugriffsüberprüfungen interagiert, benötigt eine kostenpflichtige Azure AD Premium P2-Lizenz. Beispiele:

- Administratoren, die eine Zugriffsüberprüfung erstellen
- Gruppenbesitzer, die eine Zugriffsüberprüfung durchführen
- Als Reviewer zugewiesene Benutzer
- Benutzer, die eine Selbstüberprüfung ausführen

Sie können Gastbenutzer auch bitten, ihren eigenen Zugriff zu überprüfen. Für jede kostenpflichtige Azure AD Premium P2-Lizenz, die Sie einem Benutzer Ihrer eigenen Organisation zuweisen, können Sie mit Azure AD B2B (Business-to-Business) unter dem Kontingent für externe Benutzer bis zu fünf Gastbenutzer einladen. Diese Gastbenutzer können auch Azure AD Premium P2-Features nutzen. Weitere Informationen finden Sie unter [Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration](../b2b/licensing-guidance.md).

Anhand der folgenden Beispielszenarien können Sie die Anzahl der benötigten Lizenzen bestimmen.

| Szenario | Berechnung | Erforderliche Anzahl von Lizenzen |
| --- | --- | --- |
| Ein Administrator erstellt eine Zugriffsüberprüfung für Gruppe A mit 500 Benutzern. Er weist drei Gruppenbesitzer als Reviewer zu. | 1 Lizenz für den Administrator + 3 Lizenzen für jeden Gruppenbesitzer als Reviewer. | 4 |
| Ein Administrator erstellt eine Zugriffsüberprüfung für Gruppe A mit 500 Benutzern. Er legt sie als Selbstüberprüfung fest. | 1 Lizenz für den Administrator + 500 Lizenzen für jeden Benutzer als Selbstprüfer. | 501 |
| Ein Administrator erstellt eine Zugriffsüberprüfung für Gruppe B mit 5 Benutzern und 25 Gastbenutzern. Er legt sie als Selbstüberprüfung fest. | 1 Lizenz für den Administrator + 5 Lizenzen für jeden Benutzer als Selbstprüfer.<br/>(Gastbenutzer sind im erforderlichen Verhältnis von 1:5 abgedeckt.) | 6 |
| Ein Administrator erstellt eine Zugriffsüberprüfung für Gruppe C mit 5 Benutzern und 108 Gastbenutzern. Er legt sie als Selbstüberprüfung fest. | 1 Lizenz für den Administrator + 5 Lizenzen für jeden Benutzer als Selbstprüfer + 16 zusätzliche Lizenzen, um alle 108 Gastbenutzer im erforderlichen Verhältnis von 1:5 abzudecken.<br/>1 + 5 = 6 Lizenzen, die 5 \* 6 = 30 Gastbenutzer abdecken. Für die restlichen (108 - 5 \* 6) = 78 Gastbenutzer sind 78 : 5 = 16 zusätzliche Lizenzen erforderlich. Insgesamt sind also 6 + 16 = 22 Lizenzen erforderlich. | 22 |

Weitere Informationen zum Zuweisen von Lizenzen zu Ihren Benutzern finden Sie unter [Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal](../fundamentals/license-users-groups.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Überprüfen des Zugriffs auf Gruppen oder Anwendungen](perform-access-review.md)
- [Abschließen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](complete-access-review.md)
