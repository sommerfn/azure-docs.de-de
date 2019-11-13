---
title: Selbstüberprüfung eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung
description: Erfahren Sie, wie Sie in Azure Active Directory-Zugriffsüberprüfungen (Vorschauversion) den Benutzerzugriff auf Zugriffspakete in der Berechtigungsverwaltung überprüfen.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 11/01/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89a346f48856ea33ae5e27e3537f53dfa1ca3ae3
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608362"
---
# <a name="self-review-of-an-access-package-in-azure-ad-entitlement-management"></a>Selbstüberprüfung eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung

Die Azure AD-Berechtigungsverwaltung erleichtert Unternehmen die Verwaltung des Zugriffs auf Gruppen, Anwendungen und SharePoint-Websites. In diesem Artikel wird beschrieben, wie ein Benutzer eine Selbstüberprüfung der ihm zugewiesenen Zugriffspakete durchführt.

## <a name="open-the-access-review"></a>Öffnen der Zugriffsüberprüfung

Um eine Zugriffsüberprüfung durchführen zu können, müssen Sie zunächst die Zugriffsüberprüfung öffnen. Gehen Sie wie folgt vor, um die Zugriffsüberprüfung zu finden und zu öffnen:

1. Sie erhalten möglicherweise eine E-Mail von Microsoft, in der Sie zur Überprüfung des Zugriffs aufgefordert werden. Suchen Sie nach der E-Mail, um die Zugriffsüberprüfung zu öffnen. Nachfolgend sehen Sie ein Beispiel für eine E-Mail, in der eine Überprüfung des Zugriffs angefordert wird: 
    
    ![E-Mail zur Zugriffsüberprüfung an Selbstprüfer](./media/entitlement-management-access-reviews-review-access/self-review-reviewer-email.png)

1. Klicken Sie auf den Link **Zugriff überprüfen**.

1. Sie können auch direkt zu https://myaccess.microsoft.com navigieren, um nach Ihren ausstehenden Zugriffsüberprüfungen zu suchen, wenn Sie keine E-Mail erhalten.

1. Klicken Sie auf der linken Navigationsleiste auf **Zugriffsüberprüfungen**, um eine Liste der ausstehenden Zugriffsüberprüfungen anzuzeigen, die Ihnen zugewiesen sind.


1.  Klicken Sie auf die Überprüfung, die Sie starten möchten.

## <a name="perform-the-access-review"></a>Durchführen der Zugriffsüberprüfung

Nachdem Sie die Zugriffsüberprüfung geöffnet haben, wird Ihr Zugriff angezeigt. Gehen Sie wie folgt vor, um die Zugriffsüberprüfung durchzuführen:

1.  Entscheiden Sie, ob Sie weiterhin Zugriff auf das Zugriffspaket benötigen. Wenn beispielsweise das Projekt, an dem Sie arbeiten, noch nicht abgeschlossen ist, benötigen Sie weiterhin Zugriff, um die Arbeit am Projekt fortsetzen zu können.

1.  Klicken Sie auf **Ja**, um den Zugriff zu behalten, oder klicken Sie auf **Nein**, um Ihren Zugriff zu entfernen.
    >[!NOTE]
    >Wenn Sie angegeben haben, dass Sie keinen Zugriff mehr benötigen, werden Sie nicht sofort aus dem Zugriffspaket entfernt. Sie werden aus dem Zugriffspaket entfernt, wenn die Überprüfung endet oder ein Administrator die Überprüfung beendet.

1.  Wenn Sie auf **Ja** geklickt haben, müssen Sie möglicherweise im Feld **Grund** eine Begründung angeben.

1.  Klicken Sie auf **Submit**.

Sie können zur Überprüfung zurückkehren, wenn Sie Ihre Meinung ändern und sich entschließen, Ihre Antwort vor dem Ende der Überprüfung zu ändern.

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen des Zugriffs auf Zugriffspakete](entitlement-management-access-reviews-review-access.md) 
