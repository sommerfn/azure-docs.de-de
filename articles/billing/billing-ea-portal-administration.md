---
title: Azure EA-Portalverwaltung
description: In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/08/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 3c992274ba8ff82795e9a4f917c67e7ef379d13e
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888441"
---
# <a name="azure-ea-portal-administration"></a>Azure EA-Portalverwaltung

In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen (https://ea.azure.com). Das Azure EA-Portal ist ein Onlineverwaltungsportal, das Kunden beim Verwalten der Kosten ihrer Azure EA-Dienste unterstützt. Einführende Informationen zum Azure EA-Portal finden Sie im Artikel [Erste Schritte mit dem Azure EA-Portal](billing-ea-portal-get-started.md).

## <a name="add-a-new-enterprise-administrator"></a>Hinzufügen eines neuen Unternehmensadministrators

Unternehmensadministratoren haben die meisten Berechtigungen bei der Verwaltung einer Azure EA-Registrierung. Der erste Azure EA-Administrator wurde erstellt, als die EA-Vereinbarung eingerichtet wurde. Sie können jedoch jederzeit neue Administratoren hinzufügen oder entfernen. Neue Administratoren werden nur von vorhandenen Administratoren hinzugefügt. Weitere Informationen zum Hinzufügen zusätzlicher Unternehmensadministratoren finden Sie unter [Erstellen eines weiteren Unternehmensadministrators](billing-ea-portal-get-started.md#create-another-enterprise-admin). Weitere Informationen zu Rollen und Aufgaben für ein Abrechnungsprofil finden Sie unter [Rollen und Aufgaben für ein Abrechnungsprofil](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-user-state-from-pending-to-active"></a>Aktualisieren des Benutzerstatus von „Ausstehend“ in „Aktiv“

Wenn neue Kontobesitzer (Account Owners, AO) erstmals zu einer Azure EA-Registrierung hinzugefügt werden, wird ihr Status als _Ausstehend_ angezeigt. Wenn ein neuer Kontobesitzer die Begrüßungs-E-Mail für die Aktivierung erhält, kann er sich anmelden, um sein Konto zu aktivieren. Nach Aktivierung des Kontos wird der Kontostatus von _Ausstehend_ in _Aktiv_ geändert. Der Kontobesitzer muss die Warnmeldung lesen und auf **Weiter** klicken. Neue Benutzer werden möglicherweise aufgefordert, ihren Vor- und Nachnamen einzugeben, um ein Commerce-Konto zu erstellen. In dem Fall müssen die erforderlichen Informationen hinzugefügt werden, damit der Vorgang fortgesetzt und das Konto aktiviert werden kann.

## <a name="add-a-department-admin"></a>Hinzufügen eines Abteilungsadministrators

Nachdem ein Azure EA-Administrator eine Abteilung erstellt hat, kann der Azure-Unternehmensadministrator Abteilungsadministratoren hinzufügen und diese einer Abteilung zuordnen. Ein Abteilungsadministrator kann neue Konten erstellen. Neue Konten sind erforderlich, damit Azure EA-Abonnements erstellt werden können.

Weitere Informationen zum Hinzufügen eines Abteilungsadministrators finden Sie unter [Erstellen eines Azure EA-Abteilungsadministrators](billing-ea-portal-get-started.md#add-a-department-admin).

## <a name="associate-an-account-to-a-department"></a>Zuordnen eines Kontos zu einer Abteilung

Unternehmensadministratoren können Abteilungen unter der Registrierung vorhandene Konten zuordnen.

### <a name="to-associate-an-account-to-a-department"></a>So ordnen Sie ein Konto einer Abteilung zu

1. Melden Sie sich beim Azure EA-Portal als Unternehmensadministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf **Abteilung**.
1. Zeigen Sie auf die Zeile mit dem gewünschten Konto, und klicken Sie rechts auf das Stiftsymbol.
1. Wählen Sie im Dropdownmenü die gewünschte Abteilung aus.
1. Klicken Sie auf **Speichern**.

## <a name="department-spending-quotas"></a>Ausgabenkontingente für Abteilungen

EA-Kunden können für jede Abteilung unter einer Registrierung Ausgabenkontingente festlegen oder ändern. Die Höhe des Ausgabenkontingents wird für die aktuelle Laufzeit des Prepaidtarifs festgelegt. Am Ende der aktuellen Laufzeit des Prepaidtarifs wird das vorhandene Ausgabenkontingent automatisch auf die nächste Laufzeit des Prepaidtarifs verlängert, sofern die Werte nicht aktualisiert werden.

Der Abteilungsadministrator kann das Ausgabenkontingent zwar anzeigen, aber nur der Unternehmensadministrator kann den Kontingentbetrag aktualisieren. Unternehmensadministrator und Abteilungsadministrator erhalten Benachrichtigungen, sobald 50 Prozent, 75 Prozent, 90 Prozent und 100 Prozent des Kontingents erreicht werden.

### <a name="enterprise-administrator-to-set-the-quota"></a>Als Unternehmensadministrator können Sie das Kontingent wie folgt festlegen:

 1. Öffnen Sie das Azure EA-Portal.
 1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
 1. Klicken Sie auf die Registerkarte **Abteilung**.
 1. Klicken Sie auf die gewünschte Abteilung.
 1. Klicken Sie im Abschnitt mit den Abteilungsdetails auf das Stiftsymbol, oder klicken Sie auf die Schaltfläche **+ Abteilung hinzufügen**, um ein Ausgabenkontingent zusammen mit einer neuen Abteilung hinzuzufügen.
 1. Geben Sie in den Abteilungsdetails ein Ausgabenkontingent in der Währung der Registrierung in das entsprechende Feld ein. (Der angegebene Betrag muss größer Null sein.)
    - Bei dieser Gelegenheit können Sie auch den Abteilungsnamen und die Kostenstelle bearbeiten.
 1. Klicken Sie auf **Speichern**.

Das Ausgabenkontingent für die Abteilung wird nun auf der Registerkarte „Abteilung“ in der Abteilungsliste angezeigt. Am Ende des aktuellen Prepaidtarifs behält das Azure EA-Portal die Ausgabenkontingente für die nächste Laufzeit des Prepaidtarifs bei.

Die Höhe des Abteilungskontingents ist unabhängig vom aktuellen Mindestverbrauch, und die Höhe des Kontingents sowie die Warnungen gelten nur für die Erstanbieternutzung. Das Ausgabenkontingent für die Abteilung dient nur zur Information und erzwingt keine Ausgabenlimits.

### <a name="department-administrator-to-view-the-quota"></a>Als Abteilungsadministrator können Sie das Kontingent wie folgt anzeigen:

1. Öffnen Sie das Azure EA-Portal.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Abteilung**, und sehen Sie sich die Abteilungsliste mit den Ausgabenkontingenten an.

Wenn Sie ein indirekter Kunde sind, müssen die Kostenfeatures von Ihrem Channelpartner aktiviert werden.

## <a name="enterprise-user-roles"></a>Unternehmensbenutzerrollen

Das Azure EA-Portal unterstützt Sie bei der Verwaltung Ihrer Azure EA-Kosten und der Nutzung. Im Azure EA-Portal gibt es drei zentrale Rollen:

- EA-Administrator
- Abteilungsadministrator
- Kontobesitzer

Jede Rolle verfügt über eine andere Zugriffs- und Autorisierungsebene.

Weitere Informationen zu Benutzerrollen finden Sie unter [Unternehmensbenutzerrollen](https://docs.microsoft.com/azure/billing/billing-ea-portal-get-started#enterprise-user-roles).

## <a name="add-an-azure-ea-account"></a>Hinzufügen eines Azure EA-Kontos

Das Azure EA-Konto ist eine Organisationseinheit im Azure EA-Portal, die zum Verwalten von Abonnements und auch zur Berichterstellung verwendet wird. Für den Zugriff auf Azure-Dienste und ihre Verwendung müssen Sie ein Konto erstellen oder erstellen lassen.

Weitere Informationen zu Azure-Konten finden Sie unter „Hinzufügen eines Kontos“.

## <a name="enterprise-devtest-offer"></a>Enterprise Dev/Test-Angebot

Als Azure-Unternehmensadministrator können Sie Kontobesitzern in Ihrer Organisation nun die Erstellung von Abonnements auf der Grundlage des EA Dev/Test-Angebots ermöglichen. Aktivieren Sie hierzu im Azure EA-Portal das Kontrollkästchen „Dev/Test“ für den Kontobesitzer.

Informieren Sie anschließend den Kontobesitzer, dass er nun die EA Dev/Test-Abonnements einrichten kann, die der für seine Teams von Dev/Test-Abonnenten benötigt.

Mit diesem Angebot können aktive Visual Studio-Abonnenten Entwicklungs- und Testworkloads in Azure zu speziellen Dev/Test-Tarifen ausführen und dabei auf den vollständigen Katalog von Dev/Test-Images zugreifen (einschließlich Windows 8.1 und Windows 10).

### <a name="to-set-up-the-enterprise-devtest-offer"></a>So richten Sie das Enterprise Dev/Test-Angebot ein:

1. Melden Sie sich als Unternehmensadministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Klicken Sie auf die Zeile für das Konto, unter dem Sie den Dev/Test-Zugriff aktivieren möchten.
1. Klicken Sie rechts neben der Zeile auf das Stiftsymbol.
1. Aktivieren Sie das Kontrollkästchen „Dev/Test“.
1. Klicken Sie auf **Speichern**.

Wenn ein Benutzer über das Azure EA-Portal als Kontobesitzer hinzugefügt wird, werden alle dem Kontobesitzer zugeordneten Azure-Abonnements, die entweder auf dem Dev/Test-Angebot mit nutzungsbasierter Zahlung oder auf den Angeboten mit monatlichem Guthaben für Visual Studio-Abonnenten basieren, in das EA Dev/Test-Angebot konvertiert. Abonnements, die auf anderen Angebotstypen (beispielsweise nutzungsbasierte Zahlung) basieren und dem Kontobesitzer zugeordnet sind, werden in Microsoft Azure Enterprise-Angebote konvertiert.

Das Dev/Test-Angebot gilt derzeit nicht für Azure Government-Kunden.

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
-   Wenn die Registrierung, von der aus Sie die Übertragung durchführen, über RI-Käufe verfügt, verbleibt die Gebühr für RI-Käufe in der Quellregistrierung. Sämtliche RI-Vorteile werden jedoch in die neue Registrierung übertragen und stehen dort zur Verfügung.
-   Die einmalige Gebühr für Marketplace-Käufe sowie feste Monatsgebühren, die ggf. bereits für die alte Registrierung angefallen sind, werden nicht in die neue Registrierung übertragen. Verbrauchsbasierte Marketplace-Gebühren werden übertragen.
-   Nach einer rückdatierten Registrierungsübertragung verbleiben alle einmaligen Gebühren von Käufen in der Quellregistrierung.

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

## <a name="delete-subscription"></a>Löschen eines Abonnements

So löschen Sie ein Abonnement, für das Sie als Kontobesitzer fungieren:

1. Melden Sie sich mit den Anmeldeinformationen Ihres Kontos beim Azure-Portal an.
1. Wählen Sie im Menü „Hub“ die Option **Abonnements** aus.
1. Wählen Sie links oben auf der Seite auf der Registerkarte „Abonnements“ das Abonnement aus, das Sie kündigen möchten, und klicken Sie auf **Cancel Sub** (Abo kündigen), um die Registerkarte für die Kündigung zu öffnen.
1. Geben Sie den Abonnementnamen ein, wählen Sie einen Kündigungsgrund aus, und klicken Sie auf die Schaltfläche **Cancel Sub** (Abo kündigen).

Beachten Sie, dass Abonnements nur von Kontoadministratoren gekündigt werden können.

## <a name="delete-an-account"></a>Löschen eines Kontos

Eine Kontoentfernung kann nur für aktive Konten ohne aktive Abonnements durchgeführt werden.

1. Wählen Sie im Enterprise Portal im linken Navigationsbereich die Option **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Wählen Sie in der Kontotabelle das Konto aus, das Sie löschen möchten.
1. Klicken Sie auf der rechten Seite der Kontozeile auf das X-Symbol.
1. Wenn unter dem Konto keine aktiven Abonnements mehr vorhanden sind, klicken Sie unter der Kontozeile auf die Schaltfläche **Ja**, um die Kontoentfernung zu bestätigen.

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

## <a name="manage-partner-administrators"></a>Verwalten von Partneradministratoren

Partneradministratoren können im Azure EA-Portal andere Partneradministratoren hinzuzufügen oder entfernen. Partneradministratoren werden den Partnerorganisationen indirekter Registrierungen und nicht direkt den Registrierungen zugeordnet.

### <a name="add-a-partner-administrator"></a>Hinzufügen eines Partneradministrators

Klicken Sie zum Anzeigen einer Liste aller Registrierungen, die der gleichen Partnerorganisation zugeordnet sind wie der aktuelle Benutzer, auf die Registerkarte **Registrierung**, und wählen Sie ein gewünschtes Registrierungsfeld aus.

1. Melden Sie sich als Partneradministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Partner**.
1. Klicken Sie auf **+ Administrator hinzufügen**, und geben Sie die E-Mail-Adresse, den Benachrichtigungskontakt und Benachrichtigungsdetails an.
1. Klicken Sie auf **Hinzufügen**.

### <a name="remove-a-partner-administrator"></a>Entfernen eines Partneradministrators

Klicken Sie zum Anzeigen einer Liste aller Registrierungen, die der gleichen Partnerorganisation zugeordnet sind wie der aktuelle Benutzer, auf die Registerkarte **Registrierung**, und wählen Sie ein gewünschtes Registrierungsfeld aus.

1. Melden Sie sich als Partneradministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Partner**.
1. Wählen Sie im Administratorabschnitt die entsprechende Zeile für den Administrator aus, den Sie entfernen möchten.
1. Klicken Sie rechts auf das X-Symbol.
1. Bestätigen Sie den Löschvorgang.

## <a name="manage-partner-notifications"></a>Verwalten von Partnerbenachrichtigungen

Partneradministratoren können die Häufigkeit verwalten, mit der sie Nutzungsbenachrichtigungen für ihre Registrierungen erhalten. Sie erhalten automatisch wöchentliche Benachrichtigungen zu Ihrem nicht in Rechnung gestellten Saldo. Sie können die Häufigkeit einzelner Benachrichtigungen in monatlich, wöchentlich oder täglich ändern oder diese vollständig deaktivieren.

Wenn ein Benutzer keine Benachrichtigung erhält, überprüfen Sie anhand der folgenden Schritte, ob die Benachrichtigungseinstellungen des Benutzers korrekt sind.

1. Melden Sie sich beim Azure EA-Portal als Partneradministrator an.
2. Klicken Sie auf **Verwalten** und anschließend auf die Registerkarte **Partner**.
3. Sehen Sie sich im Administratorabschnitt die Administratorliste an.
4. Zeigen Sie zum Bearbeiten von Benachrichtigungseinstellungen auf den entsprechenden Administrator, und klicken Sie auf das Stiftsymbol.
5. Aktualisieren Sie nach Bedarf die Benachrichtigungshäufigkeit und die Lebenszyklusbenachrichtigungen.
6. Fügen Sie bei Bedarf einen Kontakt hinzu, und klicken Sie auf **Hinzufügen**.
7. Klicken Sie auf **Speichern**.

![Beispiel für Feld „Kontakt hinzufügen“ ](./media/billing-ea-portal-administration/create-ea-manage-partner-notification.png)

## <a name="view-enrollments-for-partner-administrators"></a>Anzeigen von Registrierungen für Partneradministratoren

Partneradministratoren können eine Liste mit allen ihren direkten und indirekten Registrierungen im Azure EA-Portal anzeigen. Felder mit einer Übersicht über jede Registrierung werden mit der Registrierungsnummer, dem Registrierungsnamen, dem Saldo und Überschreitungsbeträgen angezeigt.

### <a name="view-a-list-of-enrollments"></a>Anzeigen einer Liste mit Registrierungen

1. Melden Sie sich als Partneradministrator an.
1. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Registrierung**.
1. Wählen Sie das Feld für die gewünschte Registrierung aus.

Im oberen Bereich wird weiterhin eine Ansicht aller Registrierungen mit Feldern für die einzelnen Registrierungen angezeigt. Darüber hinaus können Sie zwischen Registrierungen navigieren, indem Sie links auf der Seite auf die aktuelle Registrierungsnummer klicken. Daraufhin wird ein Popupfenster angezeigt, in dem Sie Registrierungen durchsuchen oder eine andere Registrierung auswählen können, indem Sie auf das entsprechende Feld klicken.

## <a name="azure-sponsorship-offer"></a>Azure Sponsorship-Angebot

Das Azure Sponsorship-Angebot ist ein eingeschränktes gesponsertes Microsoft Azure-Konto. Es ist per E-Mail-Einladung nur für bestimmte, von Microsoft ausgewählte Kunden verfügbar. Wenn Sie zur Nutzung des Microsoft Azure Sponsorship-Angebots berechtigt sind, erhalten Sie eine E-Mail-Einladung für Ihre Konto-ID.

Sollten Sie weitere Informationen benötigen, erstellen Sie eine [Supportanfrage für die Sponsorship-Aktivierung](https://aka.ms/azrsponsorship).

## <a name="conversion-to-work-or-school-account-authentication"></a>Umstellung auf die Authentifizierung per Geschäfts-, Schul- oder Unikonto

Azure-Unternehmensbenutzer können von der Authentifizierung per Microsoft-Konto (MSA oder Live-ID) zur Authentifizierung per Geschäfts-, Schul- oder Unikonto (unter Verwendung von Active Directory in Azure) wechseln.

Vorbereitung:

1. Fügen Sie dem Azure EA-Portal das Geschäfts-, Schul- oder Unikonto in den benötigten Rollen hinzu.
1. Sollten Fehler auftreten, ist das Konto in Active Directory möglicherweise nicht gültig.  Azure verwendet den Benutzerprinzipalnamen (User Principal Name, UPN), und dieser entspricht nicht immer der E-Mail-Adresse.
1. Authentifizieren Sie sich beim Azure EA-Portal mit dem Geschäfts-, Schul- oder Unikonto.

### <a name="to-convert-subscriptions-from-microsoft-accounts-to-work-or-school-accounts"></a>So stellen Sie Abonnements von Microsoft-Konten auf Geschäfts-, Schul- oder Unikonten um:

1. Melden Sie sich mit dem Microsoft-Konto, zu dem die Abonnements gehören, beim Verwaltungsportal an.
1. Verwenden Sie die Übertragung des Kontobesitzes, um zu dem neuen Konto zu wechseln.
1. Für das Microsoft-Konto sollten nun keine aktiven Abonnements mehr vorhanden sein, sodass es gelöscht werden kann.
1. Aus Abrechnungsgründen werden gelöschte Konten im Portal weiterhin angezeigt, sie haben allerdings den Status „Inaktiv“.  Wenn Sie sie aus der Ansicht herausfiltern möchten, können Sie durch Aktivieren eines Kontrollkästchens festlegen, dass nur aktive Konten angezeigt werden sollen.

## <a name="account-subscription-ownership-faq"></a>Häufige gestellte Fragen zum Kontoabonnementbesitz

In diesem Dokument werden häufig gestellte Fragen im Zusammenhang mit dem Kontoabonnementbesitz beantwortet.

### <a name="how-many-azure-account-owners-can-you-have-per-subscription"></a>Wie viele Azure-Kontobesitzer sind pro Abonnement zulässig?

Pro Abonnement ist immer nur ein einzelner Kontobesitzer zulässig.  Weitere Rollen können mithilfe des rollenbasierten Zugriffs oder der Zugriffssteuerung (IAM) unter [portal.azure.com]](https://portal.azure.com) auf der Registerkarte „Abonnement“ (links oben auf der Seite) hinzugefügt werden.

### <a name="can-an-azure-account-owner-be-listed-under-more-than-one-department"></a>Kann ein Azure-Kontobesitzer unter mehreren Abteilungen aufgeführt werden?

Ein Kontobesitzer kann immer nur einer einzelnen Abteilung zugeordnet sein.  Dadurch wird eine ordnungsgemäße Überwachung und Zuordnung von Kosten/Ausgaben im Zusammenhang mit der entsprechenden Abteilung unter der EA-Registrierung im Azure EA-Portal sichergestellt.

### <a name="can-an-azure-account-owner-be-listed-as-a-security-group"></a>Kann ein Azure-Kontobesitzer als Sicherheitsgruppe aufgeführt werden?

Nein. Bei einem Abonnementbesitzer muss es sich um eine eindeutige Authentifizierung vom Typ Microsoft-Konto (MSA) oder Azure Active Directory (AAD) handeln. Zur Regelung der Nachfolge in Ihrer Organisation können Sie ggf. generische Konten erstellen und den Abonnementzugriff mithilfe von AAD verwalten.

### <a name="can-an-individual-user-own-multiple-subscriptions"></a>Kann ein einzelner Benutzer mehrere Abonnements besitzen?

Ein Azure-Kontobesitzer kann eine unbegrenzte Anzahl von Abonnements erstellen und verwalten.

### <a name="how-can-i-accessview-all-my-organizations-subscriptions"></a>Wie kann ich auf alle Abonnements meiner Organisation zugreifen bzw. alle Abonnements meiner Organisation anzeigen?

Hierzu muss aktuell eine Richtlinie verwendet werden. Ihr Konto muss also für jedes erstellte Abonnement mithilfe des rollenbasierten Zugriffs einer Abonnementrolle hinzugefügt werden.

### <a name="where-do-i-go-to-create-a-subscription"></a>Wo kann ich ein Abonnement erstellen?

Wenn Sie ein Abonnement für ein EA-Angebot (Enterprise Azure) erstellen möchten, muss Ihr Konto zunächst im Azure EA-Portal durch den Administrator Ihrer EA-Registrierung der Rolle „Kontobesitzer“ hinzugefügt werden. Anschließend müssen Sie sich beim Azure EA-Portal anmelden, um Ihre Berechtigung zum Erstellen von Abonnements für den EA-Angebotstyp zu erhalten. Das erste EA-Abonnement sollte über den Link „+ Abonnement hinzufügen“ auf der Registerkarte „Abonnement“ des EA-Portals erstellt werden.  Sobald Ihr Konto allerdings über die nötige Berechtigung verfügt, ist es unter Umständen einfacher, Abonnements unter „portal.azure.com“ auf der Registerkarte „Abonnement“ (links oben auf der Seite) zu erstellen, da Sie Ihr Abonnement dort in einem einzelnen Schritt erstellen und umbenennen können.

### <a name="who-can-create-a-subscription"></a>Wer kann ein Abonnement erstellen?

Abonnements für den Enterprise Azure-Angebotstyp können nur von Benutzern erstellt werden, die im [EA-Portal](https://ea.azure.com) über die Rolle „Kontobesitzer“ verfügen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie mit [Reservierungen virtueller Computer](billing-ea-portal-vm-reservations.md) Geld sparen können.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](billing-ea-portal-troubleshoot.md).
