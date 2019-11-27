---
title: Fertigstellen der Zugriffsüberprüfung für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung von Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) abschließen und die Ergebnisse anzeigen
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe2d85d605b9ee418a5709ddcdb448c56be1d918
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74022282"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen in Privileged Identity Management

Nachdem eine [Zugriffsüberprüfung gestartet wurde](pim-how-to-start-security-review.md), können Administratoren für privilegierte Rollen den privilegierten Zugriff überprüfen.  Privileged Identity Management (PIM) sendet automatisch eine E-Mail an Benutzer in Ihrer Azure Active Directory-Organisation (Azure AD), in der sie aufgefordert werden, ihren Zugriff zu überprüfen. Benutzern, die diese E-Mail nicht erhalten, können Sie die Anweisungen unter [Ausführen einer Zugriffsüberprüfung](pim-how-to-perform-security-review.md) senden.

Wenn der Zeitraum für die Zugriffsüberprüfung abgelaufen ist oder alle Benutzer die Selbstüberprüfung abgeschlossen haben, führen Sie die Schritte in diesem Artikel aus, um die Überprüfung zu verwalten und die Ergebnisse anzuzeigen.

## <a name="manage-access-reviews"></a>Verwalten von Zugriffsüberprüfungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und wählen Sie auf dem Dashboard den **Azure AD Privileged Identity Management**-Dienst aus.
1. Wählen Sie auf dem Dashboard den Abschnitt **Zugriffsüberprüfungen** aus.
1. Wählen Sie die Zugriffsüberprüfung aus, die Sie verwalten möchten.

Auf dem Detailblatt der Zugriffsüberprüfung stehen eine Reihe von Optionen zum Verwalten dieser Überprüfung zur Verfügung.

![Privileged Identity Management-Zugriffsüberprüfungs-Schaltflächen – Screenshot](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>Erinnerung

Wenn eine Zugriffsüberprüfung so eingerichtet ist, dass die Benutzer sich selbst überprüfen, wird über die Schaltfläche **Erinnerung** eine Benachrichtigung gesendet.

### <a name="stop"></a>Beenden

Alle Zugriffsüberprüfungen weisen ein Enddatum auf, Sie können jedoch die Schaltfläche **Beenden** verwenden, um sie zu einem frühen Zeitpunkt abzuschließen. Wenn nicht alle Benutzer zu diesem Zeitpunkt überprüft wurden, ist dies nach dem Beenden der Überprüfung nicht mehr möglich. Eine Überprüfung kann nicht neu gestartet werden, nachdem sie beendet wurde.

### <a name="apply"></a>Anwenden

Nachdem eine Überprüfung abgeschlossen wurde, weil das Enddatum erreicht ist oder weil sie manuell beendet wurde, wird mit der Schaltfläche **Übernehmen** das Ergebnis der Überprüfung implementiert. Wenn bei der Überprüfung der Zugriff eine Benutzers verweigert wurde, ist dies der Schritt, mit dem die Rollenzuweisung entfernt wird.  

### <a name="export"></a>Export

Wenn Sie die Ergebnisse der Zugriffsüberprüfung manuell anwenden möchten, können Sie die Überprüfung exportieren. Über die Schaltfläche **Export** können Sie den Download einer CSV-Datei starten. Sie können die Ergebnisse in Excel oder einem anderen Programm verwalten, in dem sich CSV-Dateien öffnen lassen.

### <a name="delete"></a>Löschen

Wenn Sie an einer Überprüfung nicht weiter interessiert sind, löschen Sie sie. Über die Schaltfläche **Löschen** entfernen Sie die Überprüfung aus dem Privileged Identity Management-Dienst.

> [!IMPORTANT]
> Sie müssen diese destruktive Änderung nicht bestätigen, also überprüfen Sie, ob Sie diese Überprüfung löschen möchten.

## <a name="next-steps"></a>Nächste Schritte

- [Starten einer Zugriffsüberprüfung für Azure AD-Rollen in Privileged Identity Management](pim-how-to-start-security-review.md)
- [Ausführen einer Zugriffsüberprüfung meiner Azure AD-Rollen in Privileged Identity Management](pim-how-to-perform-security-review.md)
