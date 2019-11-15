---
title: Erste Schritte mit dem Azure EA-Portal
description: In diesem Artikel wird erläutert, wie Azure EA-Kunden das Azure EA-Portal verwenden.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/07/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 567beb9de0c0a8039d774270bdf61a7db437091e
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888366"
---
# <a name="get-started-with-the-azure-ea-portal"></a>Erste Schritte mit dem Azure EA-Portal

Dieser Artikel unterstützt direkte und indirekte Azure EA-Kunden beim Einstieg in das [Azure EA-Portal](https://ea.azure.com) mit grundlegenden Informationen zu Folgendem:

- Struktur des Azure EA-Portals
- Im Azure EA-Portal verwendete Rollen
- Erstellen von Abonnements
- Kostenanalyse im Azure EA-Portal und im Azure-Portal

Dieses Video zeigt eine vollständige Onboardingsitzung zum Azure EA-Portal:

[Azure EA-Portal-Onboardingvideo ](https://www.youtube.com/watch?v=OiZ1GdBpo-I)

>[!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-ea-portal-hierarchy"></a>Azure EA-Portalhierarchie

Die Azure EA-Portalhierarchie besteht aus Folgendem:

**Microsoft Azure EA-Portal**: Das Azure EA-Portal ist ein Onlineverwaltungsportal, das Ihnen hilft, die Kosten für Ihre Azure EA-Dienste zu verwalten. Damit können Sie eine Azure EA-Hierarchie einschließlich Abteilungen, Konten und Abonnements erstellen. Außerdem verwenden Sie es, um die Kosten ihrer genutzten Dienste abzugleichen, Nutzungsberichte herunterzuladen und Preislisten anzuzeigen. Und Sie erstellen API-Schlüssel für Ihre Registrierung.

**Abteilungen**: Sie erstellen Abteilungen, um Kosten in logische Gruppen aufzuteilen und anschließend auf Abteilungsebene ein Budget oder ein Kontingent festzulegen.

**Konten**: Konten sind Organisationseinheiten im Azure EA-Portal, die zum Verwalten von Abonnements verwendet werden. Konten werden auch für die Berichterstellung genutzt.

**Abonnements**: Abonnements sind die kleinste Einheit im Azure EA-Portal. Dabei handelt es sich um Container für Azure-Dienste, die vom Dienstadministrator verwaltet werden.

Das folgende Diagramm veranschaulicht einfache Azure EA-Hierarchien.

![Diagramm einfacher Azure EA-Hierarchien](./media/billing-ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Unternehmensbenutzerrollen

Zum Verwalten der Azure-Dienste in Ihrer Registrierung gibt es vier verschiedene Unternehmensadministratorrollen:

- Unternehmensadministrator
- Abteilungsadministrator
- Kontobesitzer
- Dienstadministrator

Rollen werden verwendet, um Aufgaben in zwei verschiedenen Microsoft Azure-Portalen abzuschließen. Das Azure EA-Portal (https://ea.azure.com) unterstützt Sie beim Verwalten von Abrechnungen und Kosten. Das Azure-Portal (https://portal.azure.com) wird zum Verwalten von Azure-Diensten verwendet.

Benutzerrollen sind einem Benutzerkonto zugeordnet. Zum Überprüfen der Authentizität des Benutzers muss jeder Benutzer über ein gültiges Geschäfts-, Schul- oder Unikonto oder über ein Microsoft-Konto verfügen. Stellen Sie sicher, dass jedes Konto einer E-Mail-Adresse zugeordnet ist, die aktiv überwacht wird. Kontobenachrichtigungen werden an diese E-Mail-Adresse gesendet.

Beim Einrichten von Benutzern können Sie der Rolle „Unternehmensadministrator“ mehrere Geschäfts-, Schul- oder Unikonten oder Microsoft-Konten zuweisen. Der Rolle „Kontobesitzer“ können Sie jedoch nur ein Geschäfts-, Schul-oder Unikonto oder ein Microsoft-Konto zuordnen. Darüber hinaus kann für ein einzelnes Geschäfts-, Schul- oder Unikonto oder ein Microsoft-Konto sowohl die Rolle „Unternehmensadministrator“ als auch die Rolle „Kontobesitzer“ gelten.

### <a name="enterprise-administrator"></a>Unternehmensadministrator

Die Rolle „Unternehmensadministrator“ hat die höchste Zugriffsebene. Benutzer mit der Rolle können folgende Aktionen ausführen:

- Verwalten von Konten und Kontobesitzern
- Verwalten anderer Unternehmensadministratoren
- Verwalten von Abteilungsadministratoren
- Verwalten von Benachrichtigungskontakten
- Anzeigen der Nutzung für alle Konten
- Anzeigen nicht in Rechnung gestellter Gebühren für alle Konten

In einer Unternehmensregistrierung kann es mehrere Unternehmensadministratoren geben. Sie können Unternehmensadministratoren Lesezugriff gewähren. Sie erben alle die Abteilungsadministratorrolle.

### <a name="department-administrator"></a>Abteilungsadministrator

Benutzer mit der Rolle können folgende Aktionen ausführen:

- Erstellen und Verwalten von Abteilungen
- Erstellen neuer Kontobesitzer
- Anzeigen von Nutzungsdetails für die von ihnen verwalteten Abteilungen
- Anzeigen von Kosten, wenn die erforderlichen Berechtigungen gewährt wurden

In einer Unternehmensregistrierung kann es mehrere Abteilungsadministratoren geben.

Sie können Abteilungsadministratoren Lesezugriff gewähren. Bearbeiten oder erstellen Sie einen Abteilungsadministrator, und legen Sie die Option „Schreibgeschützt“ auf **Ja** fest, um Lesezugriff zu gewähren.

### <a name="account-owner"></a>Kontobesitzer

Benutzer mit der Rolle können folgende Aktionen ausführen:

- Erstellen und Verwalten von Abonnements
- Verwalten von Dienstadministratoren
- Anzeigen der Nutzung für Abonnements

Für jedes Konto ist ein eindeutiges Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto erforderlich. Weitere Informationen zu den Azure EA-Portal-Administratorrollen finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](billing-understand-ea-roles.md).

### <a name="service-administrator"></a>Dienstadministrator

Der Dienstadministrator verfügt über Berechtigungen zum Verwalten von Diensten im Azure-Portal und kann Benutzer der Rolle „Co-Administrator“ zuweisen.

## <a name="activate-your-enrollment"></a>Aktivieren Ihrer Registrierung

Um den Dienst zu aktivieren, öffnet der erste Unternehmensadministrator das Azure EA-Portal unter [https://ea.azure.com](https://ea.azure.com) und meldet sich mit der E-Mail-Adresse aus der Einladungs-E-Mail an.

Wenn Sie über mehrere Registrierungen verfügen, wählen Sie eine aus, die Sie aktivieren möchten. Standardmäßig werden nur aktive Registrierungen angezeigt. Um den Registrierungsverlauf anzuzeigen, deaktivieren Sie die Option **Aktiv** in der oberen rechten Ecke des Azure EA-Portals.

Unter „Registrierung“ wird der Status **Aktiv** angezeigt.

![Beispiel für eine aktive Registrierung](./media/billing-ea-portal-get-started/ea-enrollment-status.png)

Nur vorhandene Azure-Unternehmensadministratoren können andere Unternehmensadministratoren erstellen.

### <a name="create-another-enterprise-admin"></a>Erstellen eines weiteren Unternehmensadministrators

- Melden Sie sich beim [Azure EA-Portal](https://ea.azure.com) an, und navigieren Sie zu **Verwalten** > **Registrierungsdetail**, und klicken Sie rechts oben auf der Seite auf **Administrator hinzufügen**.

Stellen Sie sicher, dass Sie über die E-Mail-Adressen des Benutzers und die bevorzugte Authentifizierungsmethode verfügen, wie etwa die Geschäfts-, Schul- oder Uniauthentifizierung oder ein Microsoft-Konto. Sie benötigen diese Informationen, um einen Benutzer hinzuzufügen.

Wenn Sie nicht der EA-Administrator sind, wenden Sie sich an einen EA-Administrator, um anzufordern, dass Sie zu einer Registrierung hinzugefügt werden. Nachdem Sie einer Registrierung hinzugefügt wurden, erhalten Sie eine Aktivierungs-E-Mail.

Wenn Ihr EA-Administrator Ihnen nicht helfen kann, [erstellen Sie eine Azure EA-Portal-Supportanfrage](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Geben Sie die folgenden Informationen ein:

- Registrierungsnummer
- Hinzuzufügende E-Mail-Adresse und Authentifizierungstyp (Geschäfts-, Schul- oder Unikonto oder Microsoft-Konto)
- E-Mail-Genehmigung von einem vorhandenen EA-Administrator
  - Wenn der vorhandene EA-Administrator nicht verfügbar ist, wenden Sie sich an Ihren Partner oder Softwareberater, um anzufordern, dass die Kontaktinformationen über das VLSC-Tool geändert werden.

Weitere Informationen zu den Unternehmensadministratorrollen finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](billing-understand-ea-roles.md).

## <a name="create-an-azure-ea-department"></a>Erstellen einer Azure EA-Abteilung

Unternehmensadministratoren und Abteilungsadministratoren verwenden Abteilungen zum Organisieren und Erstellen von Berichten zu Azure-Diensten für Unternehmen und zur Nutzung nach Abteilung und Kostenstelle. Der Unternehmensadministrator kann folgende Aktionen ausführen:

- Hinzufügen oder Entfernen von Abteilungen
- Zuordnen eines Kontos zu einer Abteilung
- Erstellen von Abteilungsadministratoren
- Zulassen, dass Abteilungsadministratoren Preise und Kosten anzeigen

Ein Abteilungsadministrator kann seinen Abteilungen neue Konten hinzufügen. Er kann auch Konten aus den Abteilungen entfernen, nicht jedoch aus der Registrierung.

So fügen Sie eine Abteilung hinzu:

1. Klicken Sie in der linken Navigationsleiste auf **Verwalten**.
2. Klicken Sie auf die Registerkarte **Abteilung** und anschließend auf **Abteilung hinzufügen**, und geben Sie die erforderlichen Informationen ein.
3. Der Abteilungsname ist das einzige Pflichtfeld. Er muss mindestens drei Zeichen lang sein.
4. Klicken Sie anschließend auf **Hinzufügen**.

## <a name="add-a-department-admin"></a>Hinzufügen eines Abteilungsadministrators

Nachdem eine Abteilung erstellt wurde, kann der Azure-Unternehmensadministrator Abteilungsadministratoren hinzufügen und diese einer Abteilung zuordnen. Der Abteilungsadministrator kann folgende Aktionen ausführen:

- Erstellen anderer Abteilungsadministratoren
- Anzeigen und Bearbeiten von Abteilungseigenschaften wie dem Namen oder der Kostenstelle
- Hinzufügen eines Kontos für seine Abteilungen
- Entfernen von Konten aus den Abteilungen
- Herunterladen von Nutzungsdetails für seine Abteilungen
- Anzeigen der monatlichen Nutzung und der Gebühren für seine Abteilung, wenn ein Unternehmensadministrator ihm die Berechtigung <sup>1</sup> erteilt hat

### <a name="to-add-a-department-admin"></a>So fügen Sie einen Abteilungsadministrator hinzu

Als Unternehmensadministrator:

1. Klicken Sie in der linken Navigationsleiste auf **Verwalten**.
2. Klicken Sie auf die Registerkarte **Abteilung** und anschließend auf die Abteilung.
3. Klicken Sie auf **Administrator hinzufügen**, und fügen Sie die erforderlichen Informationen hinzu.
4. Legen Sie für Lesezugriff die Option **Schreibgeschützt** auf **Ja** fest, und klicken Sie anschließend auf **Hinzufügen**.

![Beispiel für das Dialogfeld „Abteilungsadministrator hinzufügen“](./media/billing-ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>So legen Sie Lesezugriff fest

Sie können Abteilungsadministratoren Lesezugriff gewähren. Beim Erstellen eines neuen Abteilungsadministrators:

- Legen Sie die Option „Schreibgeschützt“ auf **Ja** fest.

So bearbeiten Sie einen vorhandenen Abteilungsadministrator:

1. Wählen Sie eine Abteilung aus, und klicken Sie dann auf das Stiftsymbol neben dem **Abteilungsadministrator**, den Sie bearbeiten möchten.
2. Legen Sie die Option „Schreibgeschützt“ auf **Ja** fest. Klicken Sie anschließend auf **Speichern**.

Benutzer mit der Rolle „Unternehmensadministrator“ erhalten automatisch Abteilungsadministratorberechtigungen.

<sup>1</sup> Wenn Sie die Berechtigung zum Anzeigen der monatlichen Nutzung und Gebühren für die Abteilung erhalten haben, diese aber nicht sehen, wenden Sie sich an Ihren Partner.

## <a name="add-an-account"></a>Konto hinzufügen

Die Konto- und Abonnementstruktur wirkt sich darauf aus, wie diese verwaltet werden und wie sie auf Rechnungen und Berichten angezeigt werden. Zu Beispielen einer typischen Organisationen zählt die Strukturierung nach Geschäftsbereichen, funktionalen Teams und geografischen Standorten.

So fügen Sie ein Konto hinzu:

1. Klicken Sie im Azure EA-Portal im linken Navigationsbereich auf **Verwalten**.
2. Klicken Sie auf die Registerkarte **Konto** und dann auf der Seite **Konto** auf **Konto hinzufügen**.
3. Wählen Sie eine Abteilung aus, oder lassen Sie sie nicht zugewiesen, und wählen Sie dann den gewünschten Authentifizierungstyp aus.
4. Geben Sie einen Anzeigenamen ein, damit Sie das Konto in Berichten identifizieren können.
5. Geben Sie die **Kontobesitzer-E-Mail**-Adresse ein, die dem neuen Konto zugeordnet werden soll.
6. Bestätigen Sie die E-Mail-Adresse, und klicken Sie auf **Hinzufügen**.

![Beispiel für die Liste der Konten und die Option „Konto hinzufügen“](./media/billing-ea-portal-get-started/create-ea-add-an-account.png)

Sie können ein weiteres Konto hinzufügen, indem Sie auf **Weiteres Konto hinzufügen** klicken, oder klicken Sie in der rechten unteren Ecke der linken Symbolleiste auf **Hinzufügen**.

So bestätigen Sie den Kontobesitz:

1. Melden Sie sich beim Azure EA-Portal an.
1. Bestätigen Sie den Kontobesitz, indem Sie den Status anzeigen. Der Status sollte sich von **Ausstehend** in **Start-/Enddatum** ändern. Das Start-/Enddatum ist das Datum, an dem der Benutzer sich zuerst angemeldet hat, und das Enddatum der Vereinbarung.
1. Wenn die Warnmeldung angezeigt wird, muss ein Kontobesitzer auf **Weiter** klicken, um das Konto bei der erstmaligen Anmeldung beim Azure EA-Portal zu aktivieren.


## <a name="change-account-owner"></a>Ändern des Kontobesitzers

Unternehmensadministratoren können das Azure EA-Portal verwenden, um den Abonnementkontobesitz in einer Registrierung zu übertragen. Durch die Aktion werden alle Abonnements von einem Quellbenutzerkonto in ein Zielbenutzerkonto verschoben.

Wichtige Punkte beim Übertragen von Benutzerkontoinformationen:

- Übertragungen von einem Geschäfts-, Schul- oder Unikonto zu einem anderen Geschäfts-, Schul- oder Unikonto werden unterstützt.
- Übertragungen von einem Microsoft-Konto zu einem Geschäfts-, Schul- oder Unikonto werden unterstützt.
- Übertragungen von einem Geschäfts-, Schul- oder Unikonto zu einem Microsoft-Konto werden nicht unterstützt.
- Übertragungen von einem Microsoft-Konto zu einem anderen Microsoft-Konto werden unterstützt. Das Zielkonto muss ein gültiges Azure Commerce-Konto sein, damit es ein gültiges Ziel für Übertragungen darstellt. Bei neuen Konten werden Sie aufgefordert, ein Azure Commerce-Konto zu erstellen, wenn Sie sich beim Azure EA-Portal anmelden. Bei vorhandenen Konten müssen Sie zuerst ein neues Azure-Abonnement erstellen, bevor das Konto berechtigt ist.
- Wenn Sie eine Abonnementübertragung abschließen, aktualisiert Microsoft den Kontobesitzer.

Richtlinien für die rollenbasierte Zugriffssteuerung:

- Nur Azure-Abonnementübertragungen zwischen zwei Organisations-IDs im gleichen Mandanten behalten vorhandene RBAC-Richtlinien (Role-Based Access Control, rollenbasierte Zugriffssteuerung), Rollenzuweisungen für Dienstadministratoren und Rollenzuweisungen für Co-Administratoren in Azure bei. Andere Abonnementübertragungen führen zum Verlust Ihrer RBAC-Richtlinien und der Rollenzuweisungen für Dienstadministratoren und Co-Administratoren. Richtlinien und Administratorrollen werden nicht über verschiedene Verzeichnisse hinweg übertragen. Dienstadministratoren werden auf den Besitzer des Zielkontos aktualisiert.
- Wenn Sie Abonnementübertragungen zwischen zwei Organisations-IDs im gleichen Mandanten durchführen, werden RBAC-Richtlinien und vorhandene Dienstadministrator- und Co-Administrator-Rollen beibehalten.

Führen Sie vor dem Ändern des Kontobesitzers die folgenden Aktionen durch:

1. Zeigen Sie die Registerkarte **Konto** an, und identifizieren Sie das Quellkonto. Das Quellkonto muss aktiv sein.
2. Identifizieren Sie das Zielkonto. Es muss aktiv sein.

So übertragen Sie den Kontobesitz für alle Abonnements:

1. Klicken Sie in der linken Navigationsleiste auf **Verwalten**.
2. Klicken Sie auf die Registerkarte **Konto**, und zeigen Sie auf ein Konto.
3. Klicken Sie auf der rechten Seite auf das Symbol zum Ändern des Kontobesitzers. Das Symbol stellt eine Person dar.
4. Wählen Sie ein geeignetes Konto aus, und klicken Sie dann auf **Weiter**.
5. Bestätigen Sie die Übertragung, und klicken Sie auf **Senden**.

![Bild des Symbols zum Ändern des Kontobesitzers](./media/billing-ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

So übertragen Sie den Kontobesitz für ein einzelnes Abonnement:

1. Klicken Sie in der linken Navigationsleiste auf **Verwalten**.
2. Klicken Sie auf die Registerkarte **Konto**, und zeigen Sie auf ein Konto.
3. Klicken Sie rechts auf das Symbol zum Übertragen von Abonnements. Das Symbol stellt eine Seite dar.
4. Wählen Sie ein geeignetes Abonnement aus, und klicken Sie dann auf **Weiter**.
5. Bestätigen Sie die Übertragung, und klicken Sie auf **Senden**.

![Bild des Symbols zum Übertragen von Abonnements](./media/billing-ea-portal-get-started/ea-transfer-subscriptions.png)

Im Folgenden finden Sie ein Video, das die Benutzerverwaltung des Azure EA-Portals zeigt:

[Video zur Benutzerverwaltung des Azure EA-Portals](https://www.youtube.com/watch?v=621jVkvmwm8)

>[!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Kontobesitzer können Abonnements anzeigen und verwalten. Sie können Abonnements verwenden, um Teams in Ihrer Organisation Zugriff auf Entwicklungsumgebungen und Projekte zu ermöglichen. Beispiele dafür sind Test, Produktion, Entwicklung und Staging. Wenn Sie verschiedene Abonnements für jede Anwendungsumgebung erstellen, trägt dies zur Sicherheit der Umgebungen bei. Es ist auch möglich, dass Sie jedem Abonnement ein anderes Dienstadministratorkonto zuweisen. Sie können Abonnements beliebig viele Dienste zuordnen. Der Kontobesitzer erstellt Abonnements und weist jedem Abonnement in seinem Konto ein Dienstadministratorkonto zu.

### <a name="add-a-subscription"></a>Hinzufügen eines Abonnements

Verwenden Sie die folgenden Informationen, um ein Abonnement hinzuzufügen.

Wenn Sie Ihrem Konto zum ersten Mal ein Abonnement hinzufügen, werden Sie aufgefordert, die MOSA-Vereinbarung und einen Tarifplan zu akzeptieren. Auch wenn dies auf Enterprise Agreement-Kunden nicht zutrifft, ist es zum Erstellen des Abonnements erforderlich. Ihre Microsoft Azure Enterprise Agreement-Registrierungszusatzvereinbarung hat Vorrang vor diesen Punkten, und Ihre Vertragsbeziehung ändert sich dadurch nicht. Wenn Sie dazu aufgefordert werden, aktivieren Sie das Kontrollkästchen, das anzeigt, dass Sie die Bedingungen akzeptieren.

Alle neuen Abonnements werden mit dem Standardabonnementnamen _Microsoft Azure Enterprise_ erstellt. Sie können den Abonnementnamen aktualisieren, damit Sie ihn von den anderen Abonnements in Ihrer Registrierung unterscheiden können und um sicherzustellen, dass er in Berichten auf Unternehmensebene erkennbar ist.

So fügen Sie ein Abonnement hinzu:

1. Melden Sie sich im Azure EA-Portal beim Konto an.
2. Klicken Sie auf die Registerkarte **Administrator** und anschließend oben auf der Seite auf **Abonnement**.
2. Vergewissern Sie sich, dass Sie als Kontobesitzer des Kontos angemeldet sind.
3. Klicken Sie auf **Abonnement hinzufügen** und dann auf **Kaufen**.
  Wenn Sie ein Abonnement zum ersten Mal zu einem Konto hinzufügen, müssen Sie Ihre Kontaktinformationen angeben. Wenn Sie weitere Abonnements hinzufügen, werden Ihre Kontaktinformationen für Sie hinzugefügt.
4. Klicken Sie auf **Abonnements**, wählen Sie das von Ihnen erstellte Abonnement aus, und klicken Sie dann auf **Abonnementdetails bearbeiten**.
5. Aktualisieren Sie den **Abonnementnamen** und den **Dienstadministrator**, und aktivieren Sie das Kontrollkästchen.
  Der Abonnementname wird unter den Berichten angezeigt und ist der Name des Projekts, das dem Abonnement im Entwicklungsportal zugeordnet ist.

Es kann bis zu 24 Stunden dauern, bis neue Abonnements in der Liste der Abonnements angezeigt werden. Nachdem Sie ein Abonnement erstellt haben, können Sie folgende Aktionen ausführen:

- [Bearbeiten von Abonnementdetails](https://account.azure.com/Subscriptions)
- [Verwalten von Abonnementdiensten](https://portal.azure.com/#home)

## <a name="transfer-ea-subscription-to-pay-as-you-go-subscription"></a>Umstellen eines EA-Abonnements auf ein Abonnement mit nutzungsbasierter Bezahlung

Wenn Sie ein EA-Abonnement auf ein einzelnes Abonnement mit nutzungsbasierter Bezahlung umstellen möchten, müssen Sie im Azure EA-Portal eine neue Supportanfrage erstellen. Klicken Sie zum Erstellen einer Supportanfrage unter „Hilfe und Support“ auf **Neue Supportanfrage**.

## <a name="associate-an-existing-account-with-your-pay-as-you-go-subscription"></a>Zuordnen eines vorhandenen Kontos zu Ihrem Abonnement mit nutzungsbasierter Bezahlung

Falls Sie bereits über ein Microsoft Azure-Konto im Microsoft Azure-Portal verfügen, geben Sie das zugeordnete Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto ein, um es Ihrer Enterprise Agreement-Registrierung zuzuordnen.

### <a name="associate-an-existing-account"></a>Zuordnen eines vorhandenen Kontos

1. Klicken Sie im Enterprise Portal auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Klicken Sie auf **+ Konto hinzufügen**.
1. Geben Sie das Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto ein, das dem vorhandenen Konto zugeordnet ist.
1. Bestätigen Sie das Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto, das dem vorhandenen Konto zugeordnet ist.
1. Geben Sie einen Namen ein, den Sie zum Identifizieren dieses Kontos in Berichten verwenden möchten.
1. Klicken Sie auf **Hinzufügen**.
1. Sie können ein weiteres Konto hinzufügen, indem Sie erneut die Option **+ Konto hinzufügen** auswählen. Kehren Sie andernfalls zur Startseite zurück, indem Sie die Schaltfläche **Verwaltung** auswählen.
1. Wenn Sie durch Klicken die Seite **Konto** öffnen, wird das neu hinzugefügte Konto mit dem Status **Ausstehend** angezeigt.

### <a name="confirm-account-ownership"></a>Bestätigen des Kontobesitzes

1. Melden Sie sich bei dem E-Mail-Konto an, das dem angegebenen Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto zugeordnet ist.
1. Öffnen Sie die E-Mail-Benachrichtigung mit dem Titel _Invitation to Activate your Account on the Microsoft Azure Service from Microsoft Volume Licensing_ (Einladung zur Aktivierung Ihres Kontos im Microsoft Azure-Dienst über die Microsoft-Volumenlizenzierung).
1. Klicken Sie in der Einladung auf den Link **Log into the Microsoft Azure Enterprise Portal** (Beim Microsoft Azure Enterprise Portal anmelden).
1. Klicken Sie auf **Anmelden**.
1. Geben Sie Ihr Microsoft-Konto oder Ihr Geschäfts-, Schul- oder Unikonto sowie das zugehörige Kennwort ein, um sich anzumelden und den Kontobesitz zu bestätigen.

### <a name="azure-marketplace"></a>Azure Marketplace

Die meisten Abonnements können von einer Umgebung mit nutzungsbasierter Bezahlung in Enterprise Azure konvertiert werden. Bei Azure Marketplace-Diensten ist dies jedoch nicht der Fall. Um eine einzelne Ansicht aller Abonnements und Gebühren zu erhalten, empfiehlt es sich, die Azure Marketplace-Dienste dem Enterprise Portal hinzuzufügen:

1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Registrierung**.
1. Sehen Sie sich den Abschnitt mit den Registrierungsdetails an.
1. Klicken Sie zur Aktivierung rechts neben dem Feld „Azure Marketplace“ auf das Stiftsymbol und anschließend auf **Speichern**.

Der Kontobesitzer kann nun Azure Marketplace-Abonnements erwerben, die zuvor mit nutzungsbasierter Bezahlung verwendet wurden.

Nachdem neue Azure Marketplace-Abonnements unter Ihrer Registrierung aktiviert wurden, kündigen Sie die Marketplace-Abonnements, die in der Umgebung mit nutzungsbasierter Bezahlung erstellt wurden. Dieser Schritt ist wichtig, um einen Fehlerzustand Ihrer Marketplace-Abonnements zu vermeiden, wenn Ihr Zahlungsmittel für die nutzungsbasierte Bezahlung abläuft.

### <a name="msdn"></a>MSDN

MSDN-Abonnements werden automatisch in MSDN Dev/Test konvertiert, und das EA-Angebot verliert sämtliches ggf. vorhandenes Guthaben.

### <a name="azure-in-open"></a>Azure in Open

Wenn Sie ein Azure in Open-Abonnement einem EA zuordnen, verfällt das nicht in Anspruch genommene Azure in Open-Guthaben. Um den Verlust von Guthaben zu vermeiden, sollten Kunden das Guthaben ihrer Azure in Open-Abonnements vollständig aufbrauchen, bevor sie das Konto ihrem EA hinzufügen.  

### <a name="accounts-with-support-subscriptions"></a>Konten mit Support-Abonnements

Wenn Sie dem Enterprise Portal vorhandene Konten hinzufügen, die über ein Support-Abonnement (aber noch nicht über ein EA-Support-Abonnement) verfügen, wird das MOSA-Support-Abonnement nicht automatisch übertragen, und der Support muss in EA erneut erworben werden. Für die Supportabdeckung gilt eine Karenzzeit bis zum Ende des Folgemonats, um die Neuregelung des Supports zu ermöglichen.

## <a name="view-usage-summary-and-download-reports"></a>Anzeigen der Verwendungszusammenfassung und Herunterladen von Berichten

Unternehmensadministratoren können eine Zusammenfassung Ihrer Nutzungsdaten, den verbrauchten Mindestverbrauch und die Gebühren für die zusätzliche Nutzung im Azure EA-Portal anzeigen. Die Gebühren werden auf der Übersichtsebene für alle Konten und Abonnements angezeigt.

So zeigen Sie die ausführliche Nutzung für bestimmte Konten an:

Laden Sie den Bericht mit Verwendungsdetails herunter. Klicken Sie auf **Berichte** und dann auf die Registerkarte **Nutzung herunterladen**. Klicken Sie in der Liste der Berichte für den monatlichen Bericht, den Sie abrufen möchten, auf **Herunterladen**.

Der Bericht enthält keine anwendbaren Steuern. Zwischen der Nutzung und der Angabe im Bericht gibt es unter Umständen eine Wartezeit von bis zu acht Stunden.

So zeigen Sie die zusammengefassten Nutzungsberichte und -diagramme an:

1. Klicken Sie im Azure EA-Portal im linken Navigationsbereich auf **Berichte**, und zeigen Sie die Registerkarte **Verwendungszusammenfassung** an.  
  ![Erstellen und Anzeigen der Verwendungszusammenfassung und Herunterladen von Berichten](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports.png)
2. Wählen Sie einen Zeitraum aus.
3. Wechseln Sie oben rechts auf der Seite zwischen **M** (Monatlich) und **C** (Custom, benutzerdefiniert), um die **Verwendungszusammenfassung** mit benutzerdefinierten Start- und Enddaten anzuzeigen.  
  ![Erstellen und Anzeigen der Verwendungszusammenfassung und Herunterladen von Berichten in der benutzerdefinierten Ansicht](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
4. Wählen Sie einen Zeitraum oder einen Monat im Diagramm aus, um weitere Details anzuzeigen.
5. Das Diagramm zeigt die Nutzung pro Monat mit einer Aufschlüsselung der Nutzung, zu viel berechneten Diensten, separat berechneten Diensten und Marketplace-Gebühren an.
6. Filtern Sie für den ausgewählten Monat nach Abteilungen, Konten und Abonnements unterhalb des Diagramms.
7. Wechseln Sie zwischen **Gebühr nach Diensten** und **Gebühr nach Hierarchie**.
8. Erweitern und reduzieren Sie **Azure-Dienst**, **Getrennt abgerechnete Gebühren** und **Azure Marketplace**, um Details anzuzeigen.

Im Folgenden finden Sie ein Video, das zeigt, wie Sie den Verbrauch anzeigen:

[Video zum Verbrauch im Azure EA-Portal](https://www.youtube.com/watch?v=Cv2IZ9QCn9E)

>[!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Herunterladen von CSV-Berichten

Unternehmensadministratoren verwenden die Seite „Download des monatlichen Berichts“, um verschiedene Berichte als CSV-Dateien herunterzuladen. Hierzu gehören folgende Berechtigungen:

- Saldo und Guthaben
- Verwendungsdetails
- Marketplace-Gebühren
- Preisblatt

So laden Sie Berichte herunter:


1. Klicken Sie im Azure EA-Portal auf **Berichte**.
2. Klicken Sie oben auf der Seite auf **Nutzung herunterladen**.
3. Wählen Sie neben dem Bericht des Monats **Herunterladen** aus.

Zwischen dem Nutzungsdatum und dem Zeitpunkt, zu dem die Nutzung in den Berichten angezeigt wird, gibt es unter Umständen eine Wartezeit von bis zu fünf Tagen.

Wenn Sie die CSV-Dateien mit Safari in Excel herunterladen, treten möglicherweise Formatierungsfehler auf. Öffnen Sie die Datei in einem Text-Editor, um solche Fehler zu vermeiden.

![Beispiel für die Seite „Nutzung herunterladen“](./media/billing-ea-portal-get-started/create-ea-download-csv-reports.png)

Im Folgenden finden Sie ein Video, das zeigt, wie Sie Nutzungsinformationen herunterladen:

[Video zum Verbrauch im Azure EA-Portal](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

### <a name="advanced-report-download"></a>Herunterladen des erweiterten Berichts

Zur Berichterstellung für bestimmte Datumsbereiche oder Konten kann die Option „Erweiterten Bericht herunterladen“ verwendet werden. Am 30. August 2016 wurde das Format der Ausgabedatei von XLSX in CSV geändert, um größere Datensatzgruppen zu unterstützen.

1. Wählen Sie **Erweiterten Bericht herunterladen** aus.
1. Wählen Sie einen **Entsprechenden Datumsbereich** aus.
1. Wählen Sie **Entsprechende Konten** aus.
1. Wählen Sie **Nutzungsdaten anfordern** aus.
1. Wählen Sie die Schaltfläche **Aktualisieren** aus, bis der Status des Berichts **Herunterladen** lautet.
1. Laden Sie den Bericht herunter.

## <a name="ea-term-glossary"></a>Glossar für EA-Begriffe

- **Konto:** Eine Organisationseinheit im Azure EA-Portal zur Abonnementverwaltung und Berichterstellung.
- **Kontobesitzer:** Die Person, die für die Verwaltung von Abonnements und Dienstadministratoren in Microsoft Azure zuständig ist. Kontobesitzer können Nutzungsdaten für dieses Konto und die zugehörigen Abonnements anzeigen.
- **Zusatzabonnement:** Ein einzelnes Jahres- oder Anschlussabonnement unter der Registrierungszusatzvereinbarung.
- **Prepaidtarif:** Prepaidtarif in Form eines jährlichen Geldbetrags für Microsoft Azure-Dienste zu einem rabattierten Satz für die Nutzung im Rahmen dieser Vorauszahlung.
- **Abteilungsadministrator:** Personen, die Abteilungen verwalten, neue Konten und Kontobesitzer erstellen, Nutzungsdetails für die von ihnen verwalteten Abteilungen sowie Kosten anzeigen können, wenn ihnen Berechtigungen erteilt werden.
- **Registrierungsnummer:** Ein eindeutiger, von Microsoft bereitgestellter Bezeichner zur Identifizierung der spezifischen Registrierung, die einem Enterprise Agreement zugeordnet ist.
- **Unternehmensadministrator:** Personen, die Abteilungen und Abteilungsbesitzer sowie Konten und Kontobesitzer in Microsoft Azure verwalten können. Diese Personen können sowohl Unternehmensadministratoren verwalten als auch Nutzungsdaten, in Rechnung gestellte Mengen sowie nicht in Rechnung gestellte Gebühren für alle Konten und Abonnements anzeigen, die der Unternehmensregistrierung zugeordnet sind.
- **Enterprise Agreement:** Ein Microsoft-Lizenzvertrag für Kunden mit zentralisiertem Einkauf, die ihre gesamte Organisation auf der Grundlage von Microsoft-Technologie standardisieren und eine auf Microsoft-Software basierende IT-Infrastruktur nutzen möchten.
- **Enterprise Agreement-Registrierung:** Eine Registrierung im Enterprise Agreement-Programm für die Volumenbereitstellung von Microsoft-Produkten zu vergünstigten Preisen.
- **Microsoft-Konto**: Ein webbasierter Dienst, der beteiligten Websites die Authentifizierung eines Benutzers mit einem einzelnen Satz von Anmeldeinformationen ermöglicht.
- **Microsoft Azure Enterprise-Registrierungszusatzvereinbarung (Registrierungszusatzvereinbarung):** Eine von einem Unternehmen unterzeichnete Zusatzvereinbarung, durch die das Unternehmen im Rahmen seiner Registrierung Zugang zu Microsoft Azure erhält.
- **Azure EA-Portal:** Das Portal, das von unseren Unternehmenskunden für die Verwaltung ihrer Microsoft Azure-Konten und der zugehörigen Abonnements verwendet wird.
- **Verbrauchte Ressourcenmenge:** Die Menge eines einzelnen Microsoft Azure-Diensts, die innerhalb eines Monats genutzt wurde.
- **Dienstadministrator:** Die Person, die auf Abonnements und Entwicklungsprojekte im Azure EA-Portal zugreifen und diese verwalten kann.
- **Abonnement**: Stellt ein Azure EA-Portalabonnement dar und ist ein Container mit Microsoft Azure-Diensten, die vom gleichen Dienstadministrator verwaltet werden.
- **Geschäfts-, Schul- oder Unikonto:** Konto für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden.

### <a name="enrollment-statuses"></a>Optionen für den Registrierungsstatus:

- **Pending**: Der Registrierungsadministrator muss sich beim Azure EA-Portal anmelden. Nach der Anmeldung wechselt die Registrierung in einen aktiven Status.
- **Aktiv:** Die Registrierung ist aktiv, und Konten und Abonnements können im Azure EA-Portal erstellt werden. Die Registrierung bleibt bis zum Enddatum des Enterprise Agreements aktiv.
- **Indefinite Extended Term** (Unbestimmte Laufzeitverlängerung): Eine unbestimmte Laufzeitverlängerung tritt auf, wenn das Enddatum des Enterprise Agreements überschritten wurde. Dieser Status ermöglicht es EA-Kunden, die sich für die Laufzeitverlängerung entschieden haben, Azure am Ende ihres Enterprise Agreements unbefristet weiterzuverwenden. Bevor die EA-Registrierung das Enddatum des Enterprise Agreements erreicht, muss der Registrierungsadministrator entscheiden, ob er die Registrierung durch Hinzufügen eines weiteren Mindestverbrauchs verlängern oder die Registrierung auf eine neue Registrierung übertragen oder zum Microsoft Online Subscription-Programm (MOSP) migrieren möchte. Alternativ kann er die Deaktivierung aller Dienste bestätigen, die der Registrierung zugeordnet sind.
- **Abgelaufen:** Der EA-Kunde hat sich gegen die Laufzeitverlängerung entschieden, und die EA-Registrierung hat das Enddatum des Enterprise Agreements erreicht. Die Registrierung läuft ab, und alle zugeordneten Dienste werden deaktiviert.
- **Übertragen:** Status für Registrierungen, bei denen alle zugeordneten Konten und Dienste in eine neue Registrierung übertragen wurden. Wichtig: Registrierungen werden nicht automatisch übertragen, wenn bei der Verlängerung eine neue Registrierungsnummer generiert wird. Die vorherige Registrierungsnummer muss in die Verlängerungsdokumente des Kunden eingeschlossen werden, um eine automatische Übertragung zu ermöglichen.

## <a name="get-started-on-azure-ea-faq"></a>Häufig gestellte Fragen zu den ersten Schritten mit Azure EA

Dieses Dokument enthält ausführliche Informationen zu typischen Fragen, die Kunden während des Onboardingprozesses stellen.  

### <a name="can-i-associate-my-existing-azure-account-to-enterprise-enrollment"></a>Kann ich mein vorhandenes Azure-Konto der Unternehmensregistrierung zuordnen?

Ja, das ist möglich. Beachten Sie allerdings, dass alle Azure-Abonnements, für die Sie als Kontobesitzer fungieren, in Ihr Enterprise Agreement konvertiert werden. Dazu zählen auch Abonnements, für die ein monatliches Guthaben genutzt wird (beispielsweise Visual Studio, AzurePass, MPN, BizSpark usw.). Das bedeutet, dass dadurch monatliches Guthaben verloren geht.

### <a name="i-accidentally-associated-my-existing-azure-account-with-enterprise-enrollment-as-a-result-i-lost-my-monthly-credit-is-it-possible-to-get-my-monthly-credit-back"></a>Ich habe versehentlich mein vorhandenes Azure-Konto der Unternehmensregistrierung zugeordnet. Dadurch ist mein monatliches Guthaben verloren gegangen. Kann ich mein monatliches Guthaben wieder zurückerlangen?

Um den Azure-Vorteil Ihres Visual Studio-Abonnements zurückzuerlangen, nachdem Sie sich als EA-Kontobesitzer authentifiziert haben, müssen Sie, nachdem Sie für EA die gleiche Anmeldung verwendet haben wie für Ihr Visual Studio-Abonnement, eine der folgenden Aktionen ausführen:
1. Löschen Sie diesen Kontobesitzer aus dem EA-Portal, nachdem Sie alle Azure-Abonnements entfernt oder verschoben haben, die er besitzt, und bitten Sie ihn, sich erneut für seine Visual Studio-Azure-Vorteile zu registrieren.
 OR
1. Löschen Sie den Visual Studio-Abonnenten über die Verwaltungswebsite in VLSC, und weisen Sie das Abonnement neu zu. Der Abonnent muss diesmal allerdings eine andere Anmeldung verwenden. Danach kann er sich erneut für seine Visual Studio-Azure-Vorteile registrieren.

### <a name="what-type-of-subscription-should-i-create"></a>Welche Art von Abonnement soll ich erstellen?

Im EA-Portal stehen für Unternehmenskunden zwei Arten von Abonnements zur Verfügung:

- Microsoft Azure Enterprise. Ideal für:
  - Jegliche Verwendung in der Produktion
  - Beste Preise basierend auf Infrastrukturausgaben
  - Weitere Informationen finden Sie unter https://azure.microsoft.com/pricing/enterprise-agreement/.
- Enterprise Dev/Test. Ideal für:
  - Sämtliche Team-Dev/Test-Workloads
  - Einzelne Dev/Test-Workloads mit mittlerem bis hohem Umfang
  - Zugriff auf spezielle MSDN-Images und Vorzugstarife für Dienste
  - Weitere Informationen finden Sie unter https://azure.microsoft.com/offers/ms-azr-0148p/.

### <a name="is-it-possible-to-transfer-subscription-ownership-to-another-account"></a>Kann der Abonnementbesitz auf ein anderes Konto übertragen werden?

Ja, der Abonnementbesitz auf ein anderes Konto übertragen werden. Wenn das Konto A beispielsweise über drei Abonnements verfügt, kann der Unternehmensadministrator ein Abonnement auf das Konto B, ein Abonnement auf das Konto C und ein Abonnement auf das Konto D oder aber alle Abonnements auf das Konto E übertragen.

Die Optionen für die Besitzübertragung (Porträtsymbol) und die Abonnementübertragung (Listensymbol) werden im EA angezeigt, wenn Sie auf „Verwalten“ > „Konto“ klicken und ganz rechts auf **Konto** zeigen.

Diese Option ist nur für aktive Konten verfügbar.

### <a name="i-see-subscription-name-defaults-to-offer-name-should-i-change-the-subscription-name-to-something-meaningful-to-my-organization"></a>Als Abonnementname wird standardmäßig der Angebotsname angezeigt. Soll ich den Abonnementnamen auf einen aussagekräftigen Namen für meine Organisation festlegen?

Jedes erstellte Abonnement wird standardmäßig auf den von Ihnen gewählten Angebotstyp festgelegt. Es empfiehlt sich, den Abonnementnamen in einen Namen zu ändern, der die Nachverfolgung des Abonnements erleichtert.

**So ändern Sie den Namen:**
1. Melden Sie sich bei [https://account.windowsazure.com](https://account.windowsazure.com) an.
1. Klicken Sie auf die Abonnementliste.
1. Wählen Sie das Abonnement aus.
1. Klicken Sie auf das Symbol **Abonnement verwalten**.
1. Bearbeiten Sie die Abonnementdetails.

### <a name="how-can-i-track-cost-incurred-by-cost-center"></a>Wie kann ich Kosten nach Kostenstelle nachverfolgen?

Wenn Sie Kosten nach Kostenstelle nachverfolgen möchten, müssen Sie die Kostenstelle auf einer der folgenden Ebenen definieren:
- Department
- Konto
- Subscription

Abhängig von Ihren Anforderungen können Sie die gleiche Kostenstelle verwenden, um die Nutzung und Kosten nachzuverfolgen, die einer bestimmten Kostenstelle zugeordnet sind.

Wenn Sie also beispielsweise die Kosten für ein spezielles Projekt nachverfolgen möchten, an dem mehrere Abteilungen beteiligt sind, können Sie die Kostenstelle auf der Abonnementebene verwenden, um die Nutzung und die Kosten nachzuverfolgen.

Eine Kostenstelle kann nicht auf der Dienstebene definiert werden. Falls Sie die Nutzung auf der Dienstebene nachverfolgen möchten, können Sie das auf der Dienstebene zur Verfügung stehende Tag-Feature verwenden.

### <a name="how-do-i-track-usage-and-spend-by-different-departments-in-my-organization"></a>Wie kann ich die Nutzung und die Ausgaben verschiedener Abteilungen in meiner Organisation nachverfolgen?

Sie können unter Ihrer EA-Registrierung beliebig viele Abteilungen erstellen. Zur ordnungsgemäßen Nachverfolgung der Nutzung müssen Sie sicherstellen, dass Abonnements nicht abteilungsübergreifend verwendet werden.

Nach Abschluss der Abteilungs- und Abonnementerstellung gehen Informationen für den Nutzungsbericht ein, die Sie bei der Nachverfolgung der Nutzung und der Verwaltung von Kosten/Ausgaben auf Abteilungsebene unterstützen.

Auf die Nutzungsdaten kann auch per API zugegriffen werden. Ausführliche Informationen und Beispielcode finden Sie unter [https://ea.azure.com/helpdocs/reportingAPI](https://ea.azure.com/helpdocs/reportingAPI).

### <a name="can-i-set-the-spending-quota-and-get-alerts-as-i-approach-my-limit"></a>Kann ich das Ausgabenkontingent festlegen und Warnungen erhalten, wenn ich mich dem Grenzwert nähere?

Das Ausgabenkontingent kann auf der Abteilungsebene festgelegt werden. Daraufhin werden Sie automatisch benachrichtigt, wenn Ihre Ausgabenlimits 50 Prozent, 75 Prozent, 90 Prozent und 100 Prozent des von Ihnen definierten Kontingents erreichen.

Klicken Sie zum Definieren Ihres Ausgabenkontingents auf die Abteilung, der Sie ein Ausgabenlimit hinzufügen möchten, und klicken Sie anschließend auf das Bearbeitungssymbol. Klicken Sie auf **Speichern**, um die Angaben zu speichern.

### <a name="i-used-resource-groups-rgs-to-implement-rbac-and-track-usage-how-can-i-view-the-associated-usage-details"></a>Ich habe Ressourcengruppen (RGs) verwendet, um die RBAC zu implementieren und die Nutzung nachzuverfolgen. Wie kann ich die zugehörigen Nutzungsdetails anzeigen?

Informationen wie „Ressourcengruppen“ und „Tag“ werden (sofern verwendet) auf der Dienstebene nachverfolgt und sind in der CSV-Datei mit den ausführlichen Nutzungsdaten enthalten, die Sie über das Azure EA-Portal unter [https://ea.azure.com/report/downloadusage](https://ea.azure.com/report/downloadusage) herunterladen können.

Auf die Nutzungsdaten kann auch per API zugegriffen werden. Ausführliche Informationen und Beispielcode finden Sie unter [https://ea.azure.com/helpdocs/reportingAPI](https://ea.azure.com/helpdocs/reportingAPI).

Beachten Sie, dass Tags nur auf Ressourcen angewendet werden können, die Resource Manager-Vorgänge unterstützen. Wenn Sie virtuelle Computer, virtuelle Netzwerke oder Speicher über das klassische Bereitstellungsmodell erstellt haben (etwa über das klassische Azure-Portal), kann auf diese Ressourcen kein Tag angewendet werden. Diese Ressourcen müssen per Resource Manager neu bereitgestellt werden, um die Verwendung von Tags zu unterstützen. Alle anderen Ressourcen unterstützen die Markierung durch Tags.

### <a name="can-i-perform-analyses-using-power-bi"></a>Kann ich Analysen mit Power BI durchführen?

Ja. Mit dem Microsoft Azure Enterprise-Inhaltspaket für Power BI können Sie die Azure-Nutzungsdaten für Ihre Unternehmensregistrierung schnell importieren und analysieren. Finden Sie heraus, in welcher Abteilung, unter welchem Konto oder unter welchem Abonnement die Nutzung am höchsten war und welcher Dienst in Ihrer Organisation am meisten genutzt wurde, oder verfolgen Sie Kosten- und Nutzungstrends nach.

**Navigieren Sie zur Power BI-Website:**

 1. Melden Sie sich mit einem gültigen Geschäfts-, Schul- oder Unikonto an.
    - Das Geschäfts-, Schul- oder Unikonto kann das gleiche Konto sein, das auch für den Zugriff auf die Registrierung über das Azure EA-Portal verwendet wird, dies ist jedoch nicht zwingend erforderlich.
 1. Gehen Sie auf dem Dienstdashboard wie folgt vor:
    - Wählen Sie die Kachel „Microsoft Azure Enterprise“ aus.
    - Klicken Sie auf **Verbinden**.
 1. Wählen Sie auf dem Bildschirm „Verbindung mit Azure Enterprise herstellen“ Folgendes aus:
    - URL der Azure-Umgebung: [https://ea.azure.com](https://ea.azure.com).
    - Anzahl von Monaten: Wählen Sie einen Wert zwischen 1 und 36 aus.
    - Registrierungsnummer: Geben Sie die Registrierungsnummer ein.
    - Klicken Sie auf **Weiter**.
 1. Geben Sie im Feld „Authentifizierungsschlüssel“ den API-Schlüssel ein. Den API-Schlüssel erhalten Sie im Azure EA-Portal, indem Sie auf der Registerkarte „Nutzung herunterladen“ auf **API-Zugriffsschlüssel** klicken.
    - Kopieren Sie den Schlüssel, und fügen Sie ihn in das Feld „Kontoschlüssel“ ein.
    - Das Laden von Daten in Power BI dauert je nach Größe der Datasets etwa 5–30 Minuten.

Die Power BI-Berichterstellung steht für direkte EA-Kunden sowie für Partner und indirekte Kunden zur Verfügung, die Zugriff auf Abrechnungsinformationen haben.

## <a name="next-steps"></a>Nächste Schritte

- Azure EA-Portaladministratoren finden unter [Azure EA-Portalverwaltung](billing-ea-portal-administration.md) weitere Informationen zu häufigen Verwaltungsaufgaben.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](billing-ea-portal-troubleshoot.md).
- Einen Onboardingleitfaden zu Azure EA finden Sie unter [Onboardingleitfaden zu Azure EA](https://ea.azure.com/api/v3Help/v2AzureEAOnboardingGuide).
