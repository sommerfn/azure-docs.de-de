---
title: Abrechnungsmodell für Azure Active Directory B2C
description: Erfahren Sie mehr über das Azure AD B2C-Abrechnungsmodell basierend auf den monatlich aktiven Benutzern (Monthly Active Users, MAU) und die Aktivierung der Abrechnung für ein bestimmtes Azure-Abonnement.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 844b62f9575249c7b99672e9e67c94cea7ec9f99
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931422"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Abrechnungsmodell für Azure Active Directory B2C

Die Nutzung von Azure Active Directory B2C (Azure AD B2C) wird einem verknüpften Azure-Abonnement in Rechnung gestellt. Dafür wird ein Abrechnungsmodell basierend auf den monatlich aktiven Benutzern (MAU) verwendet. In den folgenden Abschnitten erfahren Sie, wie Sie eine Azure AD B2C-Ressource mit einem Abonnement verknüpfen und wie das MAU-Abrechnungsmodell funktioniert.

> [!IMPORTANT]
> Dieser Artikel enthält keine Preisinformationen. Die aktuellen Informationen zur Nutzungsabrechnung und zu den Preisen finden Sie unter [Azure Active Directory B2C – Preise](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="monthly-active-users-mau-billing"></a>Abrechnung basierend auf den monatlich aktiven Benutzern (MAU)

Die Abrechnung für Azure AD B2C beruht auf der Anzahl eindeutiger Benutzer mit Authentifizierungsaktivitäten während eines Kalendermonats und wird als MAU-Abrechnung (Abrechnung basierend auf monatlich aktiven Benutzern) bezeichnet.

Ab dem **01. November 2019** werden alle neu erstellten Azure AD B2C-Mandanten auf Basis der monatlich aktiven Benutzer (MAU) abgerechnet. Die Abrechnung für vorhandene Mandanten, die am oder nach dem 01. November 2019 [mit einem Abonnement verknüpft werden](#link-an-azure-ad-b2c-tenant-to-a-subscription), erfolgt auf Basis der monatlich aktiven Benutzer (MAU).

Bei vorhandenen Azure AD B2C-Mandanten, die vor dem 01. November 2019 mit einem Abonnement verknüpft waren, können Sie wahlweise eine der folgenden Aktionen ausführen:

* Führen Sie ein Upgrade auf das MAU-Abrechnungsmodell (basierend auf den monatlich aktiven Benutzern) aus
* Behalten Sie das Abrechnungsmodell „Pro Authentifizierung“ bei

### <a name="upgrade-to-monthly-active-users-billing-model"></a>Upgrade auf das MAU-Abrechnungsmodell

Azure-Abonnementbesitzer, die über Administratorzugriff auf die Azure AD B2C-Ressource verfügen, können zum MAU-Abrechnungsmodell wechseln. Abrechnungsoptionen werden in Ihrer Azure AD B2C-Ressource konfiguriert.

Die Umstellung auf das MAU-Abrechnungsmodell ist **nicht umkehrbar**. Nachdem Sie eine Azure AD B2C-Ressource auf das MAU-Abrechnungsmodell umgestellt haben, können Sie diese Ressource nicht auf das Abrechnungsmodell „Pro Authentifizierung“ zurücksetzen.

Im Folgenden wird erläutert, wie Sie für eine vorhandene Azure AD B2C-Ressource die Umstellung auf die MAU-Abrechnung vornehmen:

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als Abonnementbesitzer an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Azure AD B2C-Verzeichnis aus, für das Sie ein Upgrade auf die MAU-Abrechnung durchführen möchten.<br/>
    ![Verzeichnis- und Abonnementfilter im Azure-Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-01-select-b2c-directory.png)
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie auf der Seite **Übersicht** des Azure AD B2C-Mandanten den Link unter **Ressourcenname** aus. Sie werden an die Azure AD B2C-Ressource in Ihrem Azure AD-Mandanten weitergeleitet.<br/>
    ![Hervorgehobener Link zur Azure AD B2C-Ressource im Azure-Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-02-b2c-resource-link.png)
1. Wählen Sie auf der Seite **Übersicht** für die Azure AD B2C-Ressource unter **Abrechenbare Einheiten** den Link **Pro Authentifizierung (Änderung in MAU)** aus.<br/>
    ![Hervorgehobener Link für die Änderung in MAU im Azure-Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-03-change-to-mau-link.png)
1. Wählen Sie **Bestätigen** aus, um das Upgrade auf die MAU-Abrechnung abzuschließen.<br/>
    ![Bestätigungsdialogfeld für die MAU-basierte Abrechnung im Azure-Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-04-confirm-change-to-mau.png)

### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Was erwartet Sie, wenn Sie die Abrechnung „Pro Authentifizierung“ auf die MAU-Abrechnung umstellen?

Die MAU-basierte Messung wird aktiviert, sobald Sie als Abonnement-/Ressourcenbesitzer die Änderung bestätigen. Ihre monatliche Rechnung enthält die Authentifizierungseinheiten, die bis zur Änderung abgerechnet wurden, und die neuen Einheiten der MAU-Abrechnung ab dem Änderungszeitpunkt.

Benutzer werden im Übergangsmonat nicht doppelt gezählt. Eindeutige aktive Benutzer, die sich vor der Änderung authentifizieren, werden in einem Kalendermonat nach dem Tarif „Pro Authentifizierung“ abgerechnet. Diese Benutzer werden im übrigen Abrechnungszeitraum des Abonnements nicht in die MAU-Berechnung einbezogen. Beispiel:

* Der B2C-Mandanten von Contoso hat 1.000 Benutzer. 250 Benutzer sind in jedem Monat aktiv. Der Abonnementadministrator ändert am 10. Tag des Monats die Abrechnung „Pro Authentifizierung“ in die MAU-Abrechnung (basierend auf den monatlich aktiven Benutzern).
* Die Abrechnung vom 1. bis 10. des Monats erfolgt nach dem Modell „Pro Authentifizierung“.
  * Wenn sich in diesem Zeitraum (1. bis 10.) 100 Benutzer anmelden, werden diese Benutzer als *für den Monat bezahlt* gekennzeichnet.
* Die Abrechnung ab dem 10. (effektiver Zeitpunkt der Umstellung) erfolgt nach dem MAU-Tarif.
  * Wenn sich in diesem Zeitraum (10. bis 30.) weitere 150 Benutzer anmelden, werden nur die weiteren 150 Benutzer in Rechnung gestellt.
  * Die fortgesetzte Aktivität der ersten 100 Benutzer hat keine Auswirkung auf die Abrechnung für den Rest des Kalendermonats.

Während des Abrechnungszeitraums der Umstellung werden dem Abonnementbesitzer in der Rechnung für sein Azure-Abonnement wahrscheinlich Einträge für beide Methoden (Pro Authentifizierung und MAU) angezeigt:

* Ein Eintrag für die Nutzung nach dem Modell „Pro Authentifizierung“ bis zum Datum/Uhrzeit der Änderung.
* Ein Eintrag für die Nutzung nach der Änderung für die MAU-Abrechnung (basierend auf monatlich aktiven Benutzern).

Die aktuellen Informationen zur Nutzungsabrechnung und zu den Preisen für Azure AD B2C finden Sie unter [Azure Active Directory B2C – Preise](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Verknüpfen eines Azure AD B2C-Mandanten mit einem Abonnement

Nutzungsgebühren für Azure Active Directory B2C (Azure AD B2C) werden einem Azure-Abonnement in Rechnung gestellt. Beim Erstellen eines Azure AD B2C-Mandanten muss der Mandantenadministrator den Azure AD B2C-Mandanten explizit mit einem Azure-Abonnement verknüpfen.

Der Link zum Abonnement wird durch Erstellen einer Azure AD B2C-*Ressource* im Azure-Zielabonnement erstellt. In einem einzelnen Azure-Abonnement können neben anderen Azure-Ressourcen wie virtuellen Computern, Speicherkonten und Logik-Apps auch mehrere Azure AD B2C-Ressourcen erstellt werden. Sie können alle Ressourcen in einem Abonnement anzeigen, indem Sie den Azure Active Directory (Azure AD)-Mandanten aufrufen, dem das Abonnement zugeordnet ist.

Ein Abonnement, das mit einem Azure AD B2C-Mandanten verknüpft ist, kann für die Abrechnung der Azure AD B2C-Nutzung oder anderer Azure-Ressourcen, einschließlich zusätzlicher Azure AD B2C-Ressourcen, verwendet werden. Das Abonnement kann nicht verwendet werden, um weitere lizenzbasierte Azure-Dienste oder Office 365-Lizenzen innerhalb des Azure AD B2C-Mandanten hinzuzufügen.

### <a name="prerequisites"></a>Voraussetzungen

* [Azure-Abonnement](https://azure.microsoft.com/free/)
* [Azure AD B2C-Mandant](active-directory-b2c-get-started.md), der mit einem Abonnement verknüpft werden soll
  * Sie müssen Mandantenadministrator sein.
  * Der Mandant darf nicht bereits mit einem Abonnement verknüpft sein.

### <a name="create-the-link"></a>Erstellen des Links

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis mit dem zu verwendenden Azure-Abonnement (*nicht* das Verzeichnis mit dem Azure AD B2C-Mandanten) aus.
1. Wählen Sie **Ressource erstellen** aus, geben Sie `Active Directory B2C` im Feld **Marketplace durchsuchen** ein, und wählen Sie dann **Azure Active Directory B2C** aus.
1. Klicken Sie auf **Erstellen**
1. Wählen Sie **Vorhandenen Azure AD B2C Mandanten mit meinem Azure-Abonnement verknüpfen** aus.
1. Wählen Sie in der Dropdownliste einen **Azure AD B2C-Mandanten** aus. Es werden nur die Mandanten angezeigt, bei denen Sie als globaler Administrator fungieren und die noch nicht mit einem Abonnement verknüpft sind. Das Feld **Name der Azure AD B2C-Ressource** wird mit dem Domänennamen des ausgewählten Azure AD B2C-Mandanten ausgefüllt.
1. Wählen Sie ein aktives Azure-**Abonnement** aus, für das Sie Administratorrechte haben.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie dann den **Ressourcengruppenstandort** an. Die hier vorgenommenen Einstellungen für die Ressourcengruppe haben keine Auswirkungen auf den Standort, die Leistung oder den Abrechnungsstatus Ihres Azure AD B2C-Mandanten.
1. Klicken Sie auf **Erstellen**.
    ![Seite zum Erstellen der Azure AD-B2C-Ressource im Azure-Portal](./media/active-directory-b2c-how-to-enable-billing/portal-01-create-b2c-resource-page.png)

Nachdem Sie diese Schritte für einen Azure AD B2C-Mandanten ausgeführt haben, wird die Abrechnung für Ihr Azure-Abonnement gemäß den Azure Direct- bzw. Enterprise Agreement-Details abgewickelt (sofern zutreffend).

### <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Verwalten Ihrer Azure AD B2C-Mandantenressourcen

Nach der Erstellung einer Azure AD B2C-Ressource in einem Azure-Abonnement sollte zusammen mit Ihren anderen Azure-Ressourcen eine neue Ressource vom Typ „B2C-Mandant“ angezeigt werden.

Diese Ressource ermöglicht Folgendes:

* Navigieren zum Abonnement zum Überprüfen der Abrechnungsinformationen
* Abrufen der Mandanten-ID des Azure AD B2C Mandanten im GUID-Format
* Navigieren zu Ihrem Azure AD B2C-Mandanten
* Senden Sie eine Supportanfrage.
* Verschieben Ihrer Azure AD B2C-Mandantenressource in ein anderes Azure-Abonnement oder eine andere Ressourcengruppe

![Seite mit Einstellungen für die B2C-Ressource im Azure-Portal](./media/active-directory-b2c-how-to-enable-billing/portal-02-b2c-resource-overview.png)

### <a name="regional-restrictions"></a>Regionale Einschränkungen

Wenn Sie bei der Erstellung von Azure-Ressourcen in Ihrem Abonnement regionale Einschränkungen festgelegt haben, können diese Einschränkungen das Erstellen der Azure AD B2C-Ressource verhindern.

Um dieses Problem zu beheben, sollten Sie Ihre regionalen Einschränkungen lockern.

## <a name="azure-cloud-solution-providers-csp-subscriptions"></a>Azure Cloud Solution Provider (CSP)-Abonnements

Azure Cloud Solution Provider (CSP)-Abonnements werden in Azure AD B2C unterstützt. Die Funktionalität ist über APIs oder das Azure-Portal für Azure AD B2C und für alle Azure-Ressourcen verfügbar. CSP-Abonnementadministratoren können auf die gleiche Weise wie bei allen anderen Azure-Ressourcen Beziehungen mit Azure AD B2C verknüpfen, verschieben und löschen.

Die Verwaltung von Azure AD B2C mit der rollenbasierten Zugriffssteuerung wird durch die Zuordnung eines Azure AD B2C-Mandanten zu einem CSP-Abonnement nicht beeinflusst. Die rollenbasierte Zugriffssteuerung wird mithilfe von mandantenbasierten (nicht abonnementbasierten) Rollen realisiert.

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Ändern des Abonnements zur Abrechnung des Azure AD B2C-Mandanten

Azure AD B2C-Mandanten können in ein anderes Abonnement verschoben werden, wenn Quell- und Zielabonnement innerhalb desselben Azure Active Directory-Mandanten liegen.

Informationen zum Verschieben von Azure-Ressourcen wie Ihr Azure AD B2C-Mandant in ein anderes Abonnement finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

Bevor Sie die Verschiebung einleiten, lesen Sie unbedingt den gesamten Artikel, um die Einschränkungen und Anforderungen für eine solche Verschiebung vollständig zu verstehen. Zusätzlich zu den Anweisungen zum Verschieben von Ressourcen enthält er wichtige Informationen wie eine vor der Verschiebung anzuwendende Checkliste und wie der Verschiebevorgang überprüft wird.

## <a name="next-steps"></a>Nächste Schritte

Neben dem Überprüfen der Nutzungs- und Abrechnungsdetails in einem ausgewählten Azure-Abonnement können Sie über die [Verwendungsberichts-API](active-directory-b2c-reference-usage-reporting-api.md) auch ausführliche Berichte zur täglichen Nutzung überprüfen.
