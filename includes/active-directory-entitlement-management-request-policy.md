---
title: include file
description: include file
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: aba3dd34383edbfb555adcc2063e4a2b76af1959
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389286"
---
## <a name="for-users-in-your-directory"></a>Für Benutzer in Ihrem Verzeichnis

Gehen Sie folgendermaßen vor, wenn Sie möchten, dass Benutzer in Ihrem Verzeichnis dieses Zugriffspaket anfordern können sollen. Beim Definieren der Anforderungsrichtlinie können Sie einzelne Benutzer oder Gruppen von Benutzern angeben. Beispielsweise verfügt Ihre Organisation möglicherweise bereits über eine Gruppe wie **Alle Mitarbeiter**.  Wenn diese Gruppe in die Richtlinie für Benutzer eingefügt wird, die Zugriff anfordern können, können alle Mitglieder dieser Gruppe Zugriff anfordern.

1. Wählen Sie in der Liste **Benutzer, die Zugriff anfordern können** die Option **Für in Ihrem Verzeichnis befindliche Benutzer** aus.

    Wenn Sie diese Option auswählen, werden neue Optionen angezeigt, um weiter zu verfeinern, wer in Ihrem Verzeichnis dieses Zugriffspaket anfordern kann.

    ![Zugriffspaket – Anforderungen – Für in Ihrem Verzeichnis befindliche Benutzer](./media/active-directory-entitlement-management-request-policy/for-users-in-your-directory.png)

1. Wählen Sie eine der folgenden Optionen:

    |  |  |
    | --- | --- |
    | **Bestimmte Benutzer und Gruppen** | Wählen Sie diese Option aus, wenn Sie möchten, dass nur die von Ihnen angegebenen Benutzer und Gruppen in Ihrem Verzeichnis dieses Zugriffspaket anfordern können sollen. |
    | **Alle Mitglieder (keine Gäste)** | Wählen Sie diese Option aus, wenn Sie möchten, dass alle Mitgliedsbenutzer in Ihrem Verzeichnis dieses Zugriffspaket anfordern können sollen. Diese Option umfasst keine Gastbenutzer, die Sie möglicherweise in Ihr Verzeichnis eingeladen haben. |
    | **Alle Benutzer (einschließlich Gästen)** | Wählen Sie diese Option aus, wenn Sie möchten, dass alle Mitgliedsbenutzer und Gastbenutzer in Ihrem Verzeichnis dieses Zugriffspaket anfordern können sollen. |

    Gastbenutzer verweisen auf externe Benutzer, die mit [Azure AD B2B-](../articles/active-directory/b2b/what-is-b2b.md) in Ihr Verzeichnis eingeladen wurden. Weitere Informationen zu den Unterschieden zwischen Mitglieds- und Gastbenutzern finden Sie unter [Welche Standardbenutzerberechtigungen gibt es in Azure Active Directory?](../articles/active-directory/fundamentals/users-default-permissions.md).

1. Wenn Sie **Bestimmte Benutzer und Gruppen** ausgewählt haben, klicken Sie auf **Benutzer und Gruppen hinzufügen**.

1. Wählen Sie im Bereich „Benutzer und Gruppen auswählen“ die Benutzer und Gruppen aus, die Sie hinzufügen möchten.

    ![Zugriffspaket – Anforderungen – Benutzer und Gruppen auswählen](./media/active-directory-entitlement-management-request-policy/select-users-groups.png)

1. Klicken Sie auf **Auswählen**, um die Benutzer und Gruppen hinzuzufügen.

1. Fahren Sie mit dem Abschnitt [Genehmigung](#approval) fort.

## <a name="for-users-not-in-your-directory"></a>Für Benutzer, die sich nicht in Ihrem Verzeichnis befinden

Gehen Sie folgendermaßen vor, wenn Sie möchten, dass Benutzer, die sich nicht in Ihrem Verzeichnis befinden, dieses Zugriffspaket anfordern können sollen. **Benutzer, die sich nicht in Ihrem Verzeichnis befinden**, bezieht sich auf Benutzer in einem anderen Azure AD-Verzeichnis bzw. einer anderen Domäne, die bisher noch nicht zu Ihrem Verzeichnis eingeladen wurden. Azure AD-Verzeichnisse müssen in den **Einschränkungen bei der Zusammenarbeit** so konfiguriert werden, dass sie Einladungen zulassen. Weitere Informationen finden Sie unter [Aktivieren der externen B2B-Zusammenarbeit und Steuern, wer Gäste einladen kann](../articles/active-directory/b2b/delegate-invitations.md).

> [!NOTE]
> Für einen sich noch nicht in Ihrem Verzeichnis befindenden Benutzer, dessen Anforderung genehmigt oder automatisch genehmigt wird, wird ein Gastbenutzerkonto erstellt. Der Gast wird eingeladen, erhält jedoch keine Einladungs-E-Mail. Stattdessen erhält er eine E-Mail, wenn seine Zugriffspaketzuweisung bereitgestellt wird. Wenn dieser Gastbenutzer zu einem späteren Zeitpunkt keine Zugriffspaketzuweisungen mehr besitzt, weil die letzte Zuweisung abgelaufen ist oder abgebrochen wurde, wird das Gastbenutzerkonto standardmäßig für die Anmeldung blockiert und anschließend gelöscht. Wenn Gastbenutzer dauerhaft in Ihrem Verzeichnis bleiben sollen, auch wenn sie keine Zugriffspaketzuweisungen haben, können Sie die Einstellungen für Ihre Berechtigungsverwaltungskonfiguration ändern. Weitere Informationen zum Gastbenutzerobjekt finden Sie unter [Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers](../articles/active-directory/b2b/user-properties.md).

1. Wählen Sie in der Liste **Benutzer, die Zugriff anfordern können** die Option **Für nicht in Ihrem Verzeichnis befindliche Benutzer** aus.

    Wenn Sie diese Option auswählen, werden neue Optionen angezeigt.

    ![Zugriffspaket – Anforderungen – Für nicht in Ihrem Verzeichnis befindliche Benutzer](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Wählen Sie eine der folgenden Optionen:

    |  |  |
    | --- | --- |
    | **Bestimmte verbundene Organisationen** | Wählen Sie diese Option aus, wenn Sie aus einer Liste mit Organisationen auswählen möchten, die Ihr Administrator zuvor hinzugefügt hat. Alle Benutzer aus den ausgewählten Organisationen werden dieses Zugriffspaket anfordern können. |
    | **Alle verbundenen Organisationen** | Wählen Sie diese Option aus, wenn alle Benutzer aus allen Ihren verbundenen Organisationen dieses Zugriffspaket anfordern können sollen. |

    Eine verbundene Organisation ist ein externes Azure AD-Verzeichnis bzw. eine Domäne, mit der Sie häufig zusammenarbeiten.

1. Wenn Sie **Bestimmte verbundene Organisationen**  ausgewählt haben, klicken Sie auf **Verzeichnisse hinzufügen**, um aus einer Liste der verbundenen Organisationen auszuwählen, die Ihr Administrator zuvor hinzugefügt hat.

1. Geben Sie einen Domänennamen ein, um nach einer verbundenen Organisation mit diesem Domänennamen zu suchen.

    ![Zugriffspaket – Anforderungen – Verzeichnisse auswählen](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    Wenn die Organisation, mit der Sie zusammenarbeiten möchten, nicht in der Liste enthalten ist, können Sie Ihren Administrator bitten, sie als verbundene Organisation hinzuzufügen. 

1. Nachdem Sie alle Ihre verbundenen Organisationen ausgewählt haben, klicken Sie auf **Auswählen**.

    > [!NOTE]
    > Alle Benutzer aus den ausgewählten verbundenen Organisationen werden dieses Zugriffspaket anfordern können. Hierzu gehören Benutzer aus allen Unterdomänen, die diesen verbundenen Organisationen zugeordnet sind.

1. Fahren Sie mit dem Abschnitt [Genehmigung](#approval) fort.

## <a name="none-administrator-direct-assignments-only"></a>Keine (nur direkte Administratorzuweisungen)

Gehen Sie folgendermaßen vor, wenn Sie Zugriffsanforderungen umgehen und Administratoren ermöglichen soll, bestimmte Benutzer direkt diesem Zugriffspaket zuzuweisen. Benutzer müssen das Zugriffspaket nicht anfordern. Sie können weiterhin Lebenszykluseinstellungen festlegen, aber es gibt keine Anforderungseinstellungen.

1. Wählen Sie in der Liste **Benutzer, die Zugriff anfordern können** die Option **Keine (nur direkte Zuweisungen eines Administrators)** aus.

    ![Zugriffspaket – Anforderungen – Keine (nur direkte Administratorzuweisungen)](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    Nach der Erstellung des Zugriffspakets können Sie direkt bestimmte interne und externe Benutzer dem Zugriffspaket zuweisen. Wenn Sie einen externen Benutzer angeben, wird ein Gastbenutzerkonto in Ihrem Verzeichnis erstellt. Weitere Informationen zum direkten Zuweisen eines Benutzers finden Sie unter [Anzeigen und Ändern von Zuweisungen für ein Zugriffspaket ](../articles/active-directory/governance/entitlement-management-access-package-assignments.md).

1. Fahren Sie mit dem Abschnitt [Ermöglichen von Anforderungen](#enable-requests) fort.

## <a name="approval"></a>Genehmigung

Im Abschnitt „Anforderung“ geben Sie an, ob eine Genehmigung erforderlich ist, wenn Benutzer dieses Zugriffspaket anfordern. Die Genehmigungseinstellungen funktionieren wie folgt:

- Nur eine der ausgewählten genehmigenden Personen bzw. genehmigenden Fallbackpersonen muss eine Anforderung genehmigen. Genehmigung durch alle genehmigenden Personen ist nicht erforderlich.
- Die Genehmigungsentscheidung basiert darauf, welche genehmigende Person die Anforderung zuerst überprüft.

1. Um die Genehmigung für Anforderungen der ausgewählten Benutzer anzufordern, setzen Sie **Genehmigung anfordern** auf **Ja**. Um Anforderungen automatisch genehmigen zu lassen, legen Sie **Nein** fest.

    ![Zugriffspaket – Anforderungen – Genehmigungseinstellungen](./media/active-directory-entitlement-management-request-policy/approval.png)

1. Um von Benutzern eine Begründung für die Anforderung des Zugriffspakets anzufordern, legen Sie die Umschaltfläche **Begründung des Anforderers erforderlich** auf **Ja** fest.

1. Bestimmen Sie, ob für die Anforderung eine ein- oder mehrstufige Genehmigung erforderlich ist. Legen Sie die Umschaltfläche **Wie viele Phasen** für einstufig auf **1** fest.

1. Wählen Sie für genehmigende Personen **Vorgesetzter als genehmigende Person** oder **Bestimmte genehmigende Personen auswählen** aus.

    Der Vorgesetzte wird durch das Attribut **Vorgesetzter** im Profil des Benutzers von Azure AD bestimmt. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren von Benutzerprofilinformationen mit Azure Active Directory](../articles/active-directory/fundamentals/active-directory-users-profile-azure-portal.md).

    ![Azure Active Directory-Benutzerprofil – Attribut „Vorgesetzter“](./media/active-directory-entitlement-management-request-policy/profile-manager.png)

1. Wenn Sie „Vorgesetzter“ als genehmigende Person ausgewählt haben, klicken Sie auf **Fallback hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die als genehmigende Fallbackperson gelten sollen für den Fall, dass die Berechtigungsverwaltung den Vorgesetzten nicht finden kann.

1. Wenn Sie „Bestimmte genehmigende Personen auswählen“ ausgewählt haben, klicken Sie auf **Genehmigende Person hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die genehmigende Personen sein sollen.

1. Geben Sie in **Zulässige Anzahl von Tagen für Entscheidungsfindung** die Anzahl der Tage an, die eine genehmigende Person Zeit hat, um eine Anforderung für dieses Zugriffspaket zu überprüfen.

    Wenn eine Anforderung nicht innerhalb dieses Zeitraums genehmigt wird, wird sie automatisch abgelehnt. Der Benutzer muss eine weitere Anforderung für das Zugriffspaket senden.

1. Um von Benutzern eine Begründung zum Anfordern des Zugriffspakets anzufordern, setzen Sie **Begründung erforderlich** auf **Ja**.

    Eine Begründung ist für andere genehmigende Personen und den Anfordernden sichtbar.

## <a name="enable-requests"></a>Ermöglichen von Anforderungen

1. Wenn das Zugriffspaket Benutzern sofort in der Anforderungsrichtlinie zur Verfügung gestellt werden soll, klicken Sie auf **Ja**, um dies zu aktivieren.

    Sie können sie in der Zukunft immer aktivieren, nachdem Sie das Erstellen des Zugriffspakets abgeschlossen haben.

    Wenn Sie **Keine (nur direkte Administratorzuweisungen)** ausgewählt haben und Sie die Aktivierung auf **Nein** festlegen, können Administratoren dieses Zugriffspaket nicht direkt zuweisen.

    ![Zugriffspaket – Richtlinie – Einstellung „Richtlinie aktivieren“](./media/active-directory-entitlement-management-request-policy/enable-requests.png)

1. Klicken Sie auf **Weiter**.
