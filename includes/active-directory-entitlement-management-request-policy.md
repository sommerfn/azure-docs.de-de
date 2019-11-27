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
ms.openlocfilehash: 6f2b5eb96eeb1c4b7d07219d5fe54a8a0ca9e28a
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73412971"
---
## <a name="for-users-in-your-directory"></a>Für Benutzer in Ihrem Verzeichnis

Gehen Sie folgendermaßen vor, wenn Sie möchten, dass Benutzer in Ihrem Verzeichnis dieses Zugriffspaket anfordern können sollen. Beim Definieren der Anforderungsrichtlinie können Sie einzelne Benutzer oder Gruppen von Benutzern angeben. Beispielsweise verfügt Ihre Organisation möglicherweise bereits über eine Gruppe wie **Alle Mitarbeiter**.  Wenn diese Gruppe in die Richtlinie für Benutzer eingefügt wird, die Zugriff anfordern können, können alle Mitglieder dieser Gruppe Zugriff anfordern.

1. Klicken Sie im Abschnitt **Benutzer, die Zugriff anfordern können** auf **Für in Ihrem Verzeichnis befindliche Benutzer**.

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

1. Klicken Sie im Abschnitt **Benutzer, die Zugriff anfordern können** auf **Für nicht in Ihrem Verzeichnis befindliche Benutzer**.

    Wenn Sie diese Option auswählen, werden neue Optionen angezeigt.

    ![Zugriffspaket – Anforderungen – Für nicht in Ihrem Verzeichnis befindliche Benutzer](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Wählen Sie eine der folgenden Optionen:

    |  |  |
    | --- | --- |
    | **Bestimmte verbundene Organisationen** | Wählen Sie diese Option aus, wenn Sie aus einer Liste mit Organisationen auswählen möchten, die Ihr Administrator zuvor hinzugefügt hat. Alle Benutzer aus den ausgewählten Organisationen werden dieses Zugriffspaket anfordern können. |
    | **Alle verbundenen Organisationen** | Wählen Sie diese Option aus, wenn alle Benutzer aus allen Ihren verbundenen Organisationen dieses Zugriffspaket anfordern können sollen. |

    Eine verbundene Organisation ist ein externes Azure AD-Verzeichnis bzw. eine externe Domäne, mit der eine Beziehung besteht.

1. Wenn Sie **Bestimmte verbundene Organisationen**  ausgewählt haben, klicken Sie auf **Verzeichnisse hinzufügen**, um aus einer Liste der verbundenen Organisationen auszuwählen, die Ihr Administrator zuvor hinzugefügt hat.

1. Geben Sie den Namen oder Domänennamen ein, um nach einer zuvor verbundenen Organisation zu suchen.

    ![Zugriffspaket – Anforderungen – Verzeichnisse auswählen](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    Wenn die Organisation, mit der Sie zusammenarbeiten möchten, nicht in der Liste enthalten ist, können Sie Ihren Administrator bitten, sie als verbundene Organisation hinzuzufügen. Weitere Informationen finden Sie unter [Hinzufügen einer verbundenen Organisation](../articles/active-directory/governance/entitlement-management-organization.md).

1. Nachdem Sie alle Ihre verbundenen Organisationen ausgewählt haben, klicken Sie auf **Auswählen**.

    > [!NOTE]
    > Alle Benutzer aus den ausgewählten verbundenen Organisationen werden dieses Zugriffspaket anfordern können. Dies schließt in Azure AD auch Benutzer aller Unterdomänen ein, die der Organisation zugeordnet sind, sofern diese Domänen nicht über die Zulassungs- oder Verweigerungsliste von Azure B2B blockiert werden. Weitere Informationen finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](../articles/active-directory/b2b/allow-deny-list.md).

1. Fahren Sie mit dem Abschnitt [Genehmigung](#approval) fort.

## <a name="none-administrator-direct-assignments-only"></a>Keine (nur direkte Administratorzuweisungen)

Gehen Sie folgendermaßen vor, wenn Sie Zugriffsanforderungen umgehen und Administratoren ermöglichen soll, bestimmte Benutzer direkt diesem Zugriffspaket zuzuweisen. Benutzer müssen das Zugriffspaket nicht anfordern. Sie können weiterhin Lebenszykluseinstellungen festlegen, aber es gibt keine Anforderungseinstellungen.

1. Klicken Sie im Abschnitt **Benutzer, die Zugriff anfordern können** auf **Keine (nur direkte Zuweisungen eines Administrators)** .

    ![Zugriffspaket – Anforderungen – Keine (nur direkte Administratorzuweisungen)](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    Nach der Erstellung des Zugriffspakets können Sie direkt bestimmte interne und externe Benutzer dem Zugriffspaket zuweisen. Wenn Sie einen externen Benutzer angeben, wird ein Gastbenutzerkonto in Ihrem Verzeichnis erstellt. Weitere Informationen zum direkten Zuweisen eines Benutzers finden Sie unter [Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket ](../articles/active-directory/governance/entitlement-management-access-package-assignments.md).

1. Fahren Sie mit dem Abschnitt [Ermöglichen von Anforderungen](#enable-requests) fort.

## <a name="approval"></a>Genehmigung

Im Abschnitt „Anforderung“ geben Sie an, ob eine Genehmigung erforderlich ist, wenn Benutzer dieses Zugriffspaket anfordern. Die Genehmigungseinstellungen funktionieren wie folgt:

- Nur eine der ausgewählten genehmigenden Personen bzw. genehmigenden Fallbackpersonen muss eine Anforderung genehmigen. Genehmigung durch alle genehmigenden Personen ist nicht erforderlich.
- Die Genehmigungsentscheidung basiert darauf, welche genehmigende Person die Anforderung zuerst überprüft.

Führen Sie diese Schritte aus, um die Genehmigungseinstellungen für die zuvor ausgewählten Benutzer anzugeben.

1. Um die Genehmigung für Anforderungen der ausgewählten Benutzer anzufordern, setzen Sie **Genehmigung anfordern** auf **Ja**. Um Anforderungen automatisch genehmigen zu lassen, legen Sie **Nein** fest.

1. Um von Benutzern eine Begründung für die Anforderung des Zugriffspakets anzufordern, legen Sie die Umschaltfläche **Begründung des Anforderers erforderlich** auf **Ja** fest.

    ![Zugriffspaket – Anforderungen – Genehmigungseinstellungen](./media/active-directory-entitlement-management-request-policy/approval.png)

### <a name="single-stage-approval"></a>Einstufige Genehmigung

1. Wählen Sie für genehmigende Personen **Vorgesetzter als genehmigende Person** oder **Bestimmte genehmigende Personen auswählen** aus.

    ![Zugriffspaket – Anforderungen – Einstellungen für einstufige Genehmigung](./media/active-directory-entitlement-management-request-policy/approval-single-stage.png)

1. Wenn Sie „Vorgesetzter“ als genehmigende Person ausgewählt haben, klicken Sie auf **Fallback hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die als genehmigende Fallbackperson gelten sollen für den Fall, dass die Berechtigungsverwaltung den Vorgesetzten nicht finden kann.

    Der Vorgesetzte wird durch das Attribut **Vorgesetzter** im Profil des Benutzers von Azure AD bestimmt. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren von Benutzerprofilinformationen mit Azure Active Directory](../articles/active-directory/fundamentals/active-directory-users-profile-azure-portal.md).

1. Wenn Sie „Bestimmte genehmigende Personen auswählen“ ausgewählt haben, klicken Sie auf **Genehmigende Person hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die genehmigende Personen sein sollen.

1. Geben Sie in **Zulässige Anzahl von Tagen für Entscheidungsfindung** die Anzahl der Tage an, die eine genehmigende Person Zeit hat, um eine Anforderung für dieses Zugriffspaket zu überprüfen.

    Wenn eine Anforderung nicht innerhalb dieses Zeitraums genehmigt wird, wird sie automatisch abgelehnt. Der Benutzer muss eine weitere Anforderung für das Zugriffspaket senden.

1. Um von Benutzern eine Begründung zum Anfordern des Zugriffspakets anzufordern, legen Sie **Begründung der genehmigenden Person erforderlich** auf **Ja** fest.

    Eine Begründung ist für andere genehmigende Personen und den Anfordernden sichtbar.

### <a name="alternate-approvers"></a>Alternative genehmigende Personen

Neben der Angabe der primären genehmigenden Personen, die Anforderungen genehmigen können, können Sie auch alternative genehmigende Personen angeben. Dies trägt dazu bei, dass die Anforderungen vor ihrem Ablauf (Timeout) genehmigt oder abgelehnt werden.

Durch das Angeben von alternativen genehmigenden Personen kann eine ausstehende Anforderung gemäß dem von Ihnen bei der Richtlinieneinrichtung angegebenen Weiterleitungsplan an den oder die alternativen genehmigenden Personen weitergeleitet werden, wenn die primären genehmigenden Personen die Anforderung nicht genehmigen oder ablehnen können. Die alternativen genehmigenden Personen erhalten eine E-Mail mit der Aufforderung, die ausstehende Anforderung zu genehmigen oder abzulehnen.

Nachdem die Anforderung an die alternativen genehmigenden Personen weitergeleitet wurde, können die primären genehmigenden Personen den Antrag dennoch weiterhin genehmigen oder ablehnen. Alternative genehmigende Personen nutzen dieselbe Website „Mein Zugriff“ wie die primären genehmigenden Personen, um die ausstehende Anforderung zu genehmigen oder abzulehnen.

Es können Personen oder Gruppen von Personen aufgelistet werden, die als primäre genehmigende Personen und alternative genehmigende Personen fungieren sollen. Stellen Sie sicher, dass Sie verschiedene Gruppen von Personen auflisten, die als primäre genehmigende Personen und alternative genehmigende Personen fungieren sollen.
Wenn Sie beispielsweise Alice und Bob als primäre genehmigende Personen aufgelistet haben, führen Sie Carol und Dave als alternative genehmigende Personen auf. Führen Sie die folgenden Schritte aus, um einem Zugriffspaket alternative genehmigende Personen hinzuzufügen:

1. Klicken Sie auf **Erweiterte Anforderungseinstellungen anzeigen**.

    ![Zugriffspakete – Richtlinie – Erweiterte Anforderungseinstellungen anzeigen](./media/active-directory-entitlement-management-request-policy/alternate-approvers-click-advanced-request.png)

1. Legen Sie die Umschaltfläche **Wenn keine Aktion ausgeführt wird, an alternative genehmigende Personen weiterleiten?** auf **Ja** fest.

1. Klicken Sie auf **Alternative genehmigende Personen hinzufügen**, und wählen Sie die alternativen genehmigende Personen aus der Liste aus.

    ![Zugriffspaket – Richtlinie – Alternative genehmigende Personen hinzufügen](./media/active-directory-entitlement-management-request-policy/alternate-approvers-add.png)

1. Geben Sie im Feld **Nach wie viel Tagen an alternative genehmigende Personen weiterleiten?** die Anzahl von Tagen ein, die Sie den genehmigenden Personen zum Genehmigen oder Ablehnen einer Anforderung einräumen. Wenn nach Ablauf der Anforderungsdauer keine der genehmigenden Personen die Anforderung genehmigt oder abgelehnt hat, läuft die Anforderung ab (Timeout), und der Benutzer muss eine weitere Anforderung für das Zugangspaket senden. 

    Anforderungen können erst einen Tag nach Erreichen der Hälfte der Anforderungsdauer an alternative genehmigende Personen weitergeleitet werden. In diesem Beispiel beträgt die Dauer der Anforderung 14 Tage. Dies bedeutet, dass die Hälfte der Anforderungsdauer an Tag 7 erreicht wird. Die Anforderung kann deshalb erst an Tag 8 weitergeleitet werden. Darüber hinaus können Anforderung nicht am letzten Tag der Antragsdauer an die alternative genehmigende Person weitergeleitet werden. Im Beispiel kann die Anforderung also spätestens an Tag 13 weitergeleitet werden.

## <a name="enable-requests"></a>Ermöglichen von Anforderungen

1. Wenn das Zugriffspaket Benutzern sofort in der Anforderungsrichtlinie zur Verfügung gestellt werden soll, klicken Sie auf **Ja**, um dies zu aktivieren.

    Sie können sie in der Zukunft immer aktivieren, nachdem Sie das Erstellen des Zugriffspakets abgeschlossen haben.

    Wenn Sie **Keine (nur direkte Administratorzuweisungen)** ausgewählt haben und Sie die Aktivierung auf **Nein** festlegen, können Administratoren dieses Zugriffspaket nicht direkt zuweisen.

    ![Zugriffspaket – Richtlinie – Einstellung „Richtlinie aktivieren“](./media/active-directory-entitlement-management-request-policy/enable-requests.png)

1. Klicken Sie auf **Weiter**.
