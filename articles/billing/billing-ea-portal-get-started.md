---
title: Erste Schritte mit dem Azure EA-Portal
description: In diesem Artikel wird erläutert, wie Azure EA-Kunden das Azure EA-Portal verwenden.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 1882b283f376a1bb8706132263c83e1a24ec0705
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900929"
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
2. Bestätigen Sie den Kontobesitz, indem Sie den Status anzeigen. Der Status sollte sich von **Ausstehend** in **Start-/Enddatum** ändern. Das Start-/Enddatum ist das Datum, an dem der Benutzer sich zuerst angemeldet hat, und das Enddatum der Vereinbarung.


## <a name="change-account-owner"></a>Ändern des Kontobesitzers

Unternehmensadministratoren können das Azure EA-Portal verwenden, um den Abonnementkontobesitz in einer Registrierung zu übertragen. Durch die Aktion werden alle Abonnements von einem Quellbenutzerkonto in ein Zielbenutzerkonto verschoben.

Wichtige Punkte beim Übertragen von Benutzerkontoinformationen:

- Übertragungen von einem Geschäfts-, Schul- oder Unikonto zu einem anderen Geschäfts-, Schul- oder Unikonto werden unterstützt.
- Übertragungen von einem Microsoft-Konto zu einem Geschäfts-, Schul- oder Unikonto werden unterstützt.
- Übertragungen von einem Geschäfts-, Schul- oder Unikonto zu einem Microsoft-Konto werden nicht unterstützt.
- Übertragungen von einem Microsoft-Konto zu einem anderen Microsoft-Konto werden unterstützt. Das Zielkonto muss ein gültiges Azure Commerce-Konto sein, damit es ein gültiges Ziel für Übertragungen darstellt. Bei neuen Konten werden Sie aufgefordert, ein Azure Commerce-Konto zu erstellen, wenn Sie sich beim Azure EA-Portal anmelden. Bei vorhandenen Konten müssen Sie zuerst ein neues Azure-Abonnement erstellen, bevor das Konto berechtigt ist.
- Wenn Sie eine Abonnementübertragung abschließen, aktualisiert Microsoft den Kontobesitzer.

RBAC-Richtlinien:

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

## <a name="transfer-pay-as-you-go-subscription-to-ea-subscription"></a>Übertragen eines nutzungsbasierten Abonnements in ein EA-Abonnement

Wenn Sie ein einzelnes nutzungsbasiertes Abonnement in ein EA-Abonnement übertragen möchten, müssen Sie eine neue Supportanfrage im Azure-Portal erstellen. Klicken Sie zum Erstellen einer Supportanfrage unter „Hilfe und Support“ auf **Neue Supportanfrage**.


## <a name="view-usage-summary-and-download-reports"></a>Anzeigen der Verwendungszusammenfassung und Herunterladen von Berichten

Unternehmensadministratoren können eine Zusammenfassung Ihrer Nutzungsdaten, den verbrauchten Mindestverbrauch und die Gebühren für die zusätzliche Nutzung im Azure EA-Portal anzeigen. Die Gebühren werden auf der Übersichtsebene für alle Konten und Abonnements angezeigt.

So zeigen Sie die ausführliche Nutzung für bestimmte Konten an

Laden Sie den Bericht mit Verwendungsdetails herunter. Klicken Sie auf **Berichte** und dann auf die Registerkarte **Nutzung herunterladen**. Klicken Sie in der Liste der Berichte für den monatlichen Bericht, den Sie abrufen möchten, auf **Herunterladen**.

Der Bericht enthält keine anwendbaren Steuern. Zwischen der Nutzung und der Angabe im Bericht gibt es unter Umständen eine Wartezeit von bis zu acht Stunden.

So zeigen Sie die Berichte und Diagramme zur Verwendungszusammenfassung an:

1. Klicken Sie im Azure EA-Portal im linken Navigationsbereich auf **Berichte**, und zeigen Sie die Registerkarte **Verwendungszusammenfassung** an.  
  ![](./media/billing-ea-portal-get-started/create-ea-view usage-summary-and-download-reports.png)
2. Wählen Sie einen Zeitraum aus.
3. Wechseln Sie oben rechts auf der Seite zwischen **M** (Monatlich) und **C** (Custom, benutzerdefiniert), um die **Verwendungszusammenfassung** mit benutzerdefinierten Start- und Enddaten anzuzeigen.  
  ![](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
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

Wenn Sie die CSV-Dateien mit Safari in Excel herunterladen, treten möglicherweise Formatierungsfehler auf. Öffnen Sie die Datei in einem Text-Editor, um solche Fehler zu umgehen.

![Beispiel für die Seite „Nutzung herunterladen“](./media/billing-ea-portal-get-started/create-ea-download-csv-reports.png)

Im Folgenden finden Sie ein Video, das zeigt, wie Sie Nutzungsinformationen herunterladen:

[Video zum Verbrauch im Azure EA-Portal](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

## <a name="schedule-an-onboarding-call"></a>Planen eines telefonischen Onboardinggesprächs

Wenn Sie eine persönliche Onboardingsitzung mit den Kunden planen möchten, erstellen Sie beim [Azure EA-Portal-Support](https://support.microsoft.com/supportrequestform/e114582c-4e51-af46-10b1-1f0cc141e133) eine Supportanfrage. Wählen Sie **Onboarding** als **Problemkategorie** aus.

## <a name="next-steps"></a>Nächste Schritte
- Azure EA-Portaladministratoren finden unter [Azure EA-Portalverwaltung](billing-ea-portal-administration.md) weitere Informationen zu häufigen Verwaltungsaufgaben.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](billing-ea-portal-troubleshoot.md).
