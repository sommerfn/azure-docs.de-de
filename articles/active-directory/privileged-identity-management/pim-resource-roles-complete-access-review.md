---
title: Fertigstellen der Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung von Azure-Ressourcenrollen in Privileged Identity Management (PIM) in Azure Active Directory abschließen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e45249245aaab97070b7e774d4b6bab6827bdc9
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021995"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Abschließen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in Privileged Identity Management

Nachdem privilegierte Rollenadministratoren [eine Zugriffsüberprüfung gestartet haben](pim-resource-roles-start-access-review.md), können sie den privilegierten Zugriff überprüfen. Privileged Identity Management (PIM) in Azure Active Directory (Azure AD) sendet automatisch eine E-Mail, in der die Benutzer aufgefordert werden, ihren Zugriff zu überprüfen. Wenn ein Benutzer diese E-Mail nicht erhalten hat, können Sie ihm die Anweisungen unter [Ausführen einer Zugriffsüberprüfung](pim-resource-roles-perform-access-review.md) zusenden.

Wenn der Zeitraum für die Zugriffsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## <a name="manage-access-reviews"></a>Verwalten von Zugriffsüberprüfungen

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie im Dashboard den Dienst **Azure-Ressourcen** aus.

2. Wählen Sie Ihre Ressource aus.

3. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.

    ![Azure-Ressourcen: Liste „Zugriffsüberprüfungen“ mit Rolle, Besitzer, Startdatum, Enddatum und Status](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf der Detailseite der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung. Die folgenden Optionen sind verfügbar:

![Optionen zum Verwalten einer Überprüfung: Beenden, Zurücksetzen, Anwenden, Löschen](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Beenden

Alle Zugriffsüberprüfungen haben ein Enddatum. Wählen Sie **Beenden** aus, um sie frühzeitig fertig zu stellen. Alle Benutzer, die ihre Überprüfung bis zu diesem Zeitpunkt nicht abgeschlossen haben, können sie nicht mehr abschließen, nachdem Sie die Überprüfung beendet haben. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### <a name="reset"></a>Reset

Sie können eine Zugriffsüberprüfung zurücksetzen, um alle dafür getroffenen Entscheidungen zu entfernen. Nachdem Sie eine Zugriffsüberprüfung zurückgesetzt haben, werden alle Benutzer als „Nicht überprüft“ gekennzeichnet.

### <a name="apply"></a>Anwenden

Nachdem eine Zugriffsüberprüfung abgeschlossen wurde, wählen Sie **Übernehmen** aus, um das Ergebnis der Überprüfung zu implementieren. Wenn bei der Überprüfung der Zugriff eines Benutzers verweigert wurde, wird mit diesem Schritt seine Rollenzuweisung entfernt.  

### <a name="delete"></a>Löschen

Wenn Sie an der Überprüfung nicht weiter interessiert sind, löschen Sie sie. Wählen Sie **Löschen** aus, um die Überprüfung aus dem Privileged Identity Management-Dienst zu entfernen.

## <a name="results"></a>Ergebnisse

Auf der Seite **Ergebnisse** können Sie eine Liste mit Ihren Ergebnissen der Überprüfung anzeigen und herunterladen.

![Seite „Ergebnisse“ mit Benutzern, Ergebnis, Grund, Überprüft von, Angewendet von und Ergebnis anwenden](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Prüfer

Zeigen Sie Prüfer für Ihre vorhandene Zugriffsüberprüfung an, und fügen Sie sie hinzu. Erinnern Sie Prüfer daran, ihre Überprüfungen durchzuführen.

![Seite „Prüfer“ mit Name und Benutzerprinzipalname](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Nächste Schritte

- [Starten einer Zugriffsüberprüfung für Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-start-access-review.md)
- [Durchführen einer Zugriffsüberprüfung meiner Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-perform-access-review.md)
