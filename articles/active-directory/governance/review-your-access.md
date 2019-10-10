---
title: Überprüfen des eigenen Zugriffs auf Gruppen oder Anwendungen mit Zugriffsüberprüfungen – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihren eigenen Zugriff auf Gruppen oder Anwendungen mit Azure Active Directory-Zugriffsüberprüfungen überprüfen.
services: active-directory
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: df8b03d513e2d78083031fbc9a3f6dbc1b7d15d9
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958567"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Überprüfen des eigenen Zugriffs auf Gruppen oder Anwendungen mit Azure AD-Zugriffsüberprüfungen

Azure Active Directory (Azure AD) erleichtert Unternehmen das Verwalten des Zugriffs auf Gruppen oder Anwendungen in Azure AD und anderen Microsoft-Onlinediensten durch eine Funktion namens „Azure AD-Zugriffsüberprüfungen“.

In diesem Artikel wird beschrieben, wie Sie Ihren eigenen Zugriff auf eine Gruppe oder Anwendung überprüfen.

## <a name="open-the-access-review"></a>Öffnen der Zugriffsüberprüfung

Bei einer Zugriffsüberprüfung müssen Sie zuerst die Zugriffsüberprüfung suchen und öffnen.

1. Achten Sie auf eine E-Mail von Microsoft, in der Sie zur Überprüfung des Zugriffs aufgefordert werden. Nachfolgend ist eine Beispiel-E-Mail zum Überprüfen Ihres Zugriffs auf eine Gruppe abgebildet.

    ![Beispiel-E-Mail von Microsoft zum Überprüfen Ihres Zugriffs auf eine Gruppe](./media/review-your-access/access-review-email.png)

1. Klicken Sie auf den Link **Zugriff überprüfen**, um die Zugriffsüberprüfung zu öffnen.

Wenn Sie keine E-Mail haben, können Sie Ihre ausstehenden Zugriffsüberprüfungen mit folgenden Schritten anzeigen.

1. Melden Sie sich auf [https://myapps.microsoft.com](https://myapps.microsoft.com) im MyApps-Portal an.

    ![MyApps-Portal mit Apps, für die Sie Berechtigungen besitzen](./media/review-your-access/myapps-access-panel.png)

1. Klicken Sie oben rechts auf der Seite auf das Benutzersymbol, neben dem Ihr Name und Ihre Standardorganisation angezeigt werden. Werden mehrere Organisationen aufgelistet, wählen Sie die Organisation aus, die eine Zugriffsüberprüfung angefordert hat.

1. Klicken Sie rechts auf der Seite auf die Kachel **Zugriffsüberprüfungen**, um eine Liste mit den ausstehenden Zugriffsüberprüfungen anzuzeigen.

    Wenn die Kachel nicht angezeigt wird, sind für diese Organisation keine Zugriffsüberprüfungen auszuführen. Daher ist zurzeit keine Aktion erforderlich.

    ![Liste mit ausstehenden Zugriffsüberprüfungen für Ihre Apps und Gruppen](./media/review-your-access/access-reviews-list.png)

1. Klicken Sie bei der Zugriffsüberprüfung, die Sie durchführen möchten, auf den Link **Überprüfung starten**.

## <a name="perform-the-access-review"></a>Durchführen der Zugriffsüberprüfung

Nachdem Sie die Zugriffsüberprüfung geöffnet haben, wird Ihr Zugriff angezeigt.

1. Überprüfen Sie Ihren Zugriff, und entscheiden Sie, ob Sie weiterhin Zugriff benötigen.

    Wenn Sie aufgefordert werden, den Zugriff für andere Benutzer zu überprüfen, sieht die Seite anders aus. Weitere Informationen finden Sie unter [Zugriffsüberprüfung von Gruppen oder Anwendungen](perform-access-review.md).

    ![Offene Zugriffsüberprüfung, in der Sie gefragt werden, ob Sie weiterhin Zugriff auf eine Gruppe benötigen](./media/review-your-access/perform-access-review.png)

1. Klicken Sie auf **Ja**, um den Zugriff zu behalten, oder klicken Sie auf **Nein**, um Ihren Zugriff zu entfernen.

1. Wenn Sie auf **Ja** klicken, müssen Sie möglicherweise im Feld **Grund** eine Begründung angeben.

    ![Abgeschlossene Zugriffsüberprüfung, in der Sie gefragt werden, ob Sie weiterhin Zugriff auf eine Gruppe benötigen](./media/review-your-access/perform-access-review-submit.png)

1. Klicken Sie auf **Submit**.

    Ihre Auswahl wird übermittelt, und Sie gelangen zurück zum MyApps-Portal.

    Wenn Sie Ihre Antwort ändern möchten, öffnen Sie erneut die Seite „Zugriffsüberprüfungen“, und aktualisieren Sie Ihre Antwort. Bis die Zugriffsüberprüfung endet, können Sie Ihre Antwort jederzeit ändern.

    > [!NOTE]
    > Wenn Sie angegeben haben, dass Sie keinen Zugriff mehr benötigen, werden Sie nicht sofort entfernt. Sie werden nach Ende der Überprüfung (oder wenn ein Administrator die Überprüfung beendet) entfernt.

## <a name="next-steps"></a>Nächste Schritte

- [Abschließen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](complete-access-review.md)
