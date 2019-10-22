---
title: Azure EA-Portalverwaltung
description: In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: f344436b1e1f1a97b981c1ead469d965b06a12ce
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329336"
---
# <a name="azure-ea-portal-administration"></a>Azure EA-Portalverwaltung

In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen (https://ea.azure.com). Das Azure EA-Portal ist ein Onlineverwaltungsportal, das Kunden beim Verwalten der Kosten ihrer Azure EA-Dienste unterstützt. Einführende Informationen zum Azure EA-Portal finden Sie im Artikel [Erste Schritte mit dem Azure EA-Portal](billing-ea-portal-get-started.md).

## <a name="add-a-new-enterprise-administrator"></a>Hinzufügen eines neuen Unternehmensadministrators

Unternehmensadministratoren haben die meisten Berechtigungen bei der Verwaltung einer Azure EA-Registrierung. Der erste Azure EA-Administrator wurde erstellt, als die EA-Vereinbarung eingerichtet wurde. Sie können jedoch jederzeit neue Administratoren hinzufügen oder entfernen. Neue Administratoren werden nur von vorhandenen Administratoren hinzugefügt. Weitere Informationen zum Hinzufügen zusätzlicher Unternehmensadministratoren finden Sie unter [Erstellen eines weiteren Unternehmensadministrators](billing-ea-portal-get-started.md#create-another-enterprise-admin). Weitere Informationen zu Rollen und Aufgaben für ein Abrechnungsprofil finden Sie unter [Rollen und Aufgaben für ein Abrechnungsprofil](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-user-state-from-pending-to-active"></a>Aktualisieren des Benutzerstatus von „Ausstehend“ in „Aktiv“

Wenn neue Kontobesitzer (Account Owners, AO) erstmals zu einer Azure EA-Registrierung hinzugefügt werden, wird ihr Status als _Ausstehend_ angezeigt. Wenn ein neuer Kontobesitzer die Begrüßungs-E-Mail für die Aktivierung erhält, kann er sich anmelden, um sein Konto zu aktivieren. Beim Aktivieren des Kontos wird der Kontostatus von _Ausstehend_ in _Aktiv_ geändert. Neue Benutzer werden möglicherweise aufgefordert, ihren Vor- und Nachnamen einzugeben, um ein Commerce-Konto zu erstellen. In dem Fall müssen die erforderlichen Informationen hinzugefügt werden, damit der Vorgang fortgesetzt und das Konto aktiviert werden kann.

## <a name="add-a-department-admin"></a>Hinzufügen eines Abteilungsadministrators

Nachdem ein Azure EA-Administrator eine Abteilung erstellt hat, kann der Azure-Unternehmensadministrator Abteilungsadministratoren hinzufügen und diese einer Abteilung zuordnen. Ein Abteilungsadministrator kann neue Konten erstellen. Neue Konten sind erforderlich, damit Azure EA-Abonnements erstellt werden können.

Weitere Informationen zum Hinzufügen eines Abteilungsadministrators finden Sie unter [Erstellen eines Azure EA-Abteilungsadministrators](billing-ea-portal-get-started.md#add-a-department-admin).

## <a name="enterprise-user-roles"></a>Unternehmensbenutzerrollen

Das Azure EA-Portal unterstützt Sie bei der Verwaltung Ihrer Azure EA-Kosten und der Nutzung. Im Azure EA-Portal gibt es drei zentrale Rollen:

- EA-Administrator
- Abteilungsadministrator
- Kontobesitzer

Jede Rolle verfügt über eine andere Zugriffs- und Autorisierungsebene.

Weitere Informationen zu Benutzerrollen finden Sie unter „Unternehmensbenutzerrollen“.

## <a name="add-an-azure-ea-account"></a>Hinzufügen eines Azure EA-Kontos

Das Azure EA-Konto ist eine Organisationseinheit im Azure EA-Portal, die zum Verwalten von Abonnements und auch zur Berichterstellung verwendet wird. Für den Zugriff auf Azure-Dienste und ihre Verwendung müssen Sie ein Konto erstellen oder erstellen lassen.

Weitere Informationen zu Azure-Konten finden Sie unter „Hinzufügen eines Kontos“.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Übertragen eines Unternehmenskontos in eine neue Registrierung

Beachten Sie die folgenden Punkte, wenn Sie ein Unternehmenskonto in eine neue Registrierung übertragen:

- Nur die in der Anforderung angegebenen Konten werden übertragen. Wenn alle Konten ausgewählt sind, werden alle Konten übertragen.
- Bei der Quellregistrierung wird der Status „Aktiv“ oder „Erweitert“ beibehalten. Sie können die Registrierung weiterverwenden, bis sie abläuft.

### <a name="effective-transfer-date"></a>Effektives Übertragungsdatum

Das effektive Übertragungsdatum kann ein Datum ab dem Startdatum der Registrierung sein, an die die Übertragung erfolgen soll. Die Registrierung, an die die Übertragung erfolgt, ist die _Zielregistrierung_. Nach der Kontoübertragung verbleiben alle Nutzungsinformationen im Konto vor dem effektiven Übertragungsdatum in der Registrierung, von der die Übertragung erfolgt. Die Registrierung, von der die Übertragung erfolgt, ist die _Quellregistrierung_.  Die Nutzung der Quellregistrierung wird in Form eines Mindestverbrauchs oder als Überschreitung abgerechnet. Die Nutzung, die nach dem effektiven Übertragungsdatum erfolgt, wird in die neue Registrierung übertragen und entsprechend abgerechnet.

Sie können eine Registrierungsübertragung bis auf das Startdatum der Zielregistrierung oder bis auf das effektive Startdatum der Quellregistrierung zurückdatieren.

### <a name="monetary-commitment"></a>Finanzielle Verpflichtung

Der Mindestverbrauch ist zwischen Registrierungen übertragbar. Salden in Bezug auf den Mindestverbrauch sind vertraglich an die Registrierung gebunden, für die sie bestellt wurden. Der Mindestverbrauch wird nicht als Teil des Übertragungsprozesses eines Kontos oder einer Registrierung übertragen.

### <a name="services-affected"></a>Betroffene Dienste

Während der Kontoübertragung entstehen keine Ausfallzeiten. Sie kann am Tag Ihrer Anforderung abgeschlossen werden, wenn alle erforderlichen Informationen bereitgestellt werden.

### <a name="prerequisites"></a>Voraussetzungen

Wenn Sie eine Kontoübertragung anfordern, geben Sie die folgenden Informationen an:


- Kontoname und ID des Kontobesitzers des zu übertragenden Kontos
- Für die Quellregistrierung: Registrierungsnummer und zu übertragendes Konto
- Für die Zielregistrierung: die Registrierungsnummer, an die die Übertragung erfolgen soll
- Das effektive Datum der Kontoübertragung kann ein Datum ab dem Startdatum der Zielregistrierung sein.

Andere Punkte, die vor einer Kontoübertragung zu beachten sind:

- Die Genehmigung eines EA-Administrators ist für die Ziel- und Quellregistrierung erforderlich.
  - In einigen Fällen kann Microsoft eine zusätzliche Genehmigung von einem EA-Administrator der Quellregistrierung anfordern.
- Wenn eine Kontoübertragung Ihre Anforderungen nicht erfüllt, ziehen Sie eine Registrierungsübertragung in Betracht.
- Bei der Kontoübertragung werden alle Dienste und Abonnements des spezifischen Kontos übertragen.
- Nach Abschluss der Übertragung erscheint das übertragene Konto unter der Quellregistrierung inaktiv.
- Eine Kontoübertragung kann auf ein beliebiges Datum nach dem Startdatum der Zielregistrierung rückdatiert werden.
- Im Konto wird das Enddatum angezeigt, das das effektive Übertragungsdatum für die Quellregistrierung und das Startdatum für die Zielregistrierung darstellt.
- Jegliche Nutzung, die für das Konto vor dem effektiven Übertragungsdatum erfolgt ist, verbleibt unter der Quellregistrierung.


## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Übertragen einer Unternehmensregistrierung in eine neue Registrierung

Wenn Sie die Übertragung einer gesamten Unternehmensregistrierung in eine Registrierung anfordern, werden die folgenden Aktionen ausgeführt:

- Alle Azure-Dienste, Abonnements, Konten, Abteilungen sowie die gesamte Registrierungsstruktur einschließlich aller EA-Abteilungsadministratoren werden übertragen.
- Der Registrierungsstatus wird auf _Übertragen_ festgelegt. Die übertragene Registrierung ist nur für Berichte zum Nutzungsverlauf verfügbar.
- Einer übertragenen Registrierung können Sie keine Rollen oder Abonnements hinzufügen. Der Status „Übertragen“ verhindert eine zusätzliche Nutzung in Bezug auf die Registrierung.
- Verbleibendes Guthaben in Bezug auf den Mindestverbrauch im Vertrag geht dabei verloren. Dies umfasst auch zukünftige Bestimmungen.

### <a name="effective-transfer-date"></a>Effektives Übertragungsdatum

Das effektive Datum der Übertragung kann ein Datum ab dem Startdatum der Registrierung sein, die an die Zielregistrierung übertragen werden soll.

Die Nutzung der Quellregistrierung wird in Form eines Mindestverbrauchs oder als Überschreitung abgerechnet. Die Nutzung, die nach dem effektiven Übertragungsdatum erfolgt, wird in die neue Registrierung übertragen und entsprechend abgerechnet.

### <a name="effective-transfer-date-in-the-past"></a>Effektives Übertragungsdatum in der Vergangenheit

Sie können eine Kontoübertragung bis auf das Startdatum der Zielregistrierung oder bis auf das effektive Startdatum der Quellregistrierung zurückdatieren.

### <a name="monetary-commitment"></a>Finanzielle Verpflichtung

Der Mindestverbrauch ist zwischen Registrierungen übertragbar. Salden in Bezug auf den Mindestverbrauch sind vertraglich an die Registrierung gebunden, für die sie bestellt wurden. Der Mindestverbrauch wird nicht als Teil des Übertragungsprozesses eines Kontos oder einer Registrierung übertragen.

### <a name="services-affected"></a>Betroffene Dienste

Während der Kontoübertragung entstehen keine Ausfallzeiten. Sie kann am Tag Ihrer Anforderung abgeschlossen werden, wenn alle erforderlichen Informationen bereitgestellt werden.

### <a name="prerequisites"></a>Voraussetzungen

Wenn Sie eine Registrierungsübertragung anfordern, geben Sie die folgenden Informationen an:

- Für die Quellregistrierung: Registrierungsnummer und zu übertragendes Konto.
- Für die Zielregistrierung: die Registrierungsnummer, an die die Übertragung erfolgen soll.
- Das effektive Datum der Registrierungsübertragung kann ein Datum ab dem Startdatum der Zielregistrierung sein. Das ausgewählte Datum wirkt sich nicht auf die Nutzung aus, für die bereits eine Überschreitungsrechnung ausgestellt wurde.

Andere Punkte, die vor einer Registrierungsübertragung zu beachten sind:

- Die Genehmigung eines EA-Administrators ist für die Ziel- und Quellregistrierung erforderlich.
  - In einigen Fällen kann Microsoft eine zusätzliche Genehmigung von einem EA-Administrator der Quellregistrierung anfordern.
- Wenn eine Registrierungsübertragung Ihre Anforderungen nicht erfüllt, ziehen Sie eine Kontoübertragung in Betracht.
- Nur die von Ihnen angegebenen Konten werden übertragen. Sie können anfordern, dass alle Ihre Konten übertragen werden.
- Bei der Quellregistrierung wird der Status „Aktiv“/„Erweitert“ beibehalten. Sie können die Registrierung weiterverwenden, bis sie abläuft.

## <a name="change-account-owner"></a>Ändern des Kontobesitzers

Das Azure EA-Portal kann Abonnements von einem Kontobesitzer auf einen anderen übertragen. Weitere Informationen finden Sie unter [Ändern des Kontobesitzers](billing-ea-portal-get-started.md#change-account-owner).

## <a name="subscription-transfer-effects"></a>Auswirkungen der Abonnementübertragung

Wenn ein Azure-Abonnement an ein Konto im gleichen Azure Active Directory-Mandanten übertragen wird, behalten alle Benutzer, Gruppen und Dienstprinzipale ihren Zugriff, die über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) Zugriff zum Verwalten von Ressourcen hatten.

So zeigen Sie Benutzer mit RBAC-Zugriff auf das Abonnement an:

1. Öffnen Sie im Azure-Portal **Abonnements**.
2. Wählen Sie das anzuzeigende Abonnement und dann **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie **Rollenzuweisungen** aus. Auf der Seite „Rollenzuweisungen“ werden alle Benutzer mit RBAC-Zugriff auf das Abonnement aufgelistet.

Wenn das Abonnement an ein Konto in einem anderen Azure AD-Mandanten übertragen wird, _verlieren_ alle Benutzer, Gruppen und Dienstprinzipale ihren Zugriff, die über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) Zugriff zum Verwalten von Ressourcen hatten. Auch wenn kein RBAC-Zugriff vorhanden ist, ist der Zugriff auf das Abonnement unter Umständen über Sicherheitsmechanismen verfügbar, wie u. a.:

- Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../cloud-services/cloud-services-certs-create.md).
- Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Azure-Speicherkonto – Übersicht](../storage/common/storage-account-overview.md).
- Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.

Der neue Besitzer sollte sich überlegen, ob er die dem Dienst zugeordneten Geheimnisse aktualisiert, wenn er den Zugriff auf die Azure-Ressourcen einschränken muss. Die meisten Ressourcen können durch Ausführen der folgenden Schritte aktualisiert werden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus.
3. Wählen Sie die Ressource.
4. Klicken Sie auf der Ressourcenseite auf **Einstellungen**, um vorhandene Geheimnisse anzuzeigen und zu aktualisieren.



## <a name="close-an-azure-enterprise-enrollment"></a>Schließen einer Azure Enterprise-Registrierung

Wenn Sie Ihre Azure EA-Registrierung schließen möchten, haben Sie folgende Möglichkeiten:

- Kündigen Sie alle Abonnements, die Azure EA im Azure-Portal zugeordnet sind.
- Wenden Sie sich an Ihren Softwareberater für Kunden oder Ihren Partner, damit dieser Ihr Azure Enterprise Agreement schließt.

## <a name="update-notification-settings"></a>Aktualisierung der Benachrichtigungseinstellungen

Unternehmensadministratoren werden automatisch für den Erhalt von Nutzungsbenachrichtigungen für ihre Registrierung registriert. Jeder Unternehmensadministrator kann das Intervall der einzelnen Benachrichtigungen ändern oder diese vollständig deaktivieren.

Benachrichtigungskontakte werden im Azure EA-Portal im Abschnitt **Benachrichtigungskontakt** angezeigt. Durch die Verwaltung Ihrer Benachrichtigungskontakte stellen Sie sicher, dass die richtigen Personen in Ihrer Organisation Azure EA-Benachrichtigungen erhalten.

So zeigen Sie die aktuellen Benachrichtigungseinstellungen an:

1. Navigieren Sie im Azure EA-Portal zu **Verwalten** > **Benachrichtigungskontakt**.
2. E-Mail-Adresse: Die E-Mail-Adresse, die dem Microsoft-Konto des Unternehmensadministrators oder dem Geschäfts-, Schul- oder Unikonto zugeordnet ist, das die Benachrichtigungen erhält
3. Benachrichtigungshäufigkeit zu nicht in Rechnung gestellten Salden: Die Häufigkeit, mit der Benachrichtigungen an die einzelnen Unternehmensadministratoren gesendet werden

So fügen Sie einen Kontakt hinzu:

1. Klicken Sie auf **Kontakt hinzufügen**.
2. Geben Sie die E-Mail-Adresse ein, und bestätigen Sie sie.
3. Klicken Sie auf **Speichern**.

Der neue Benachrichtigungskontakt wird im Abschnitt **Benachrichtigungskontakt** angezeigt. Zum Ändern der Benachrichtigungshäufigkeit wählen Sie den Benachrichtigungskontakt aus, und klicken Sie auf das Stiftsymbol rechts neben der ausgewählten Zeile. Legen Sie die Häufigkeit auf **Täglich**, **Wöchentlich**, **Monatlich** oder **Keine** fest.

Sie können das _bevorstehende Enddatum des Abdeckungszeitraums_ unterdrücken und deaktivieren und die _Bereitstellung von Lebenszyklusbenachrichtigungen zu bevorstehenden Daten aufheben_. Durch das Deaktivieren von Lebenszyklusbenachrichtigungen werden Benachrichtigungen über den Abdeckungszeitraum und das Enddatum der Vereinbarung unterdrückt.

## <a name="manage-partner-notifications"></a>Verwalten von Partnerbenachrichtigungen

Partneradministratoren können die Häufigkeit verwalten, mit der sie Nutzungsbenachrichtigungen für ihre Registrierungen erhalten. Sie erhalten automatisch wöchentliche Benachrichtigungen zu Ihrem nicht in Rechnung gestellten Saldo. Sie können die Häufigkeit einzelner Benachrichtigungen in monatlich, wöchentlich oder täglich ändern oder diese vollständig deaktivieren.

Wenn ein Benutzer keine Benachrichtigung erhält, überprüfen Sie anhand der folgenden Schritte, ob die Benachrichtigungseinstellungen des Benutzers korrekt sind.

1. Melden Sie sich beim Azure EA-Portal als Partneradministrator an.
2. Klicken Sie auf **Verwalten** und anschließend auf die Registerkarte **Partner**.
3. Zeigen Sie im Abschnitt **Administrator** die Liste der Administratoren an.
4. Zeigen Sie zum Bearbeiten von Benachrichtigungseinstellungen auf den entsprechenden Administrator, und klicken Sie auf das Stiftsymbol.
5. Aktualisieren Sie nach Bedarf die Benachrichtigungshäufigkeit und die Lebenszyklusbenachrichtigungen.
6. Fügen Sie bei Bedarf einen Kontakt hinzu, und klicken Sie auf **Hinzufügen**.
7. Klicken Sie auf **Speichern**.

![Beispiel für Feld „Kontakt hinzufügen“ ](./media/billing-ea-portal-administration/create-ea-manage-partner-notification.png)

## <a name="azure-sponsorship-offer"></a>Azure Sponsorship-Angebot

Das Azure Sponsorship-Angebot ist ein eingeschränktes gesponsertes Microsoft Azure-Konto. Es ist per E-Mail-Einladung nur für bestimmte, von Microsoft ausgewählte Kunden verfügbar. Wenn Sie berechtigt sind, das Microsoft Azure Sponsorship-Angebot zu nutzen, erhalten Sie eine E-Mail-Einladung für Ihre Konto-ID.

Weitere Informationen finden Sie unter

- Supportanfrage zur Sponsorship-Aktivierung: http://aka.ms/azrsponsorship

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie mit [Reservierungen virtueller Computer](billing-ea-portal-vm-reservations.md) Geld sparen können.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](billing-ea-portal-troubleshoot.md).
