---
title: 'Azure Databricks: Häufig gestellte Fragen und Hilfe'
description: Erhalten Sie Antworten auf häufig gestellte Fragen und Informationen zur Problembehandlung für Azure Databricks.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 0436a0898f27d3059069ce2df1fdfb143a98f325
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73601802"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Häufig gestellte Fragen zu Azure Databricks

In diesem Artikel sind die wichtigsten Fragen im Zusammenhang mit Azure Databricks aufgeführt. Außerdem werden einige häufige Probleme aufgelistet, die bei der Verwendung von Databricks auftreten können. Weitere Informationen finden Sie unter [Was ist Azure Databricks?](what-is-azure-databricks.md). 

## <a name="can-i-use-azure-key-vault-to-store-keyssecrets-to-be-used-in-azure-databricks"></a>Kann ich Schlüssel/Geheimnisse zur Verwendung in Azure Databricks in Azure Key Vault speichern?
Ja. Sie können Azure Key Vault verwenden, um Schlüssel/Geheimnisse für den Einsatz in Azure Databricks speichern. Weitere Informationen finden Sie bei den [von Azure Key Vault unterstützten Bereichen](/azure/databricks/security/secrets/secret-scopes).


## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Kann ich virtuelle Azure-Netzwerke mit Databricks verwenden?
Ja. Sie können ein virtuelles Azure-Netzwerk (VNET) mit Azure Databricks verwenden. Weitere Informationen finden Sie unter [Bereitstellen von Azure Databricks in Ihrem virtuellen Azure-Netzwerk](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject).

## <a name="how-do-i-access-azure-data-lake-storage-from-a-notebook"></a>Wie greife ich von einem Notebook aus auf Azure Data Lake Storage zu? 

Folgen Sie diesen Schritten:
1. Stellen Sie in Azure Active Directory (Azure AD) einen Dienstprinzipal bereit, und erfassen Sie dessen Schlüssel.
1. Weisen Sie dem Dienstprinzipal in Data Lake Storage die erforderlichen Berechtigungen zu.
1. Verwenden Sie zum Zugreifen auf eine Datei in Data Lake Storage die Anmeldeinformationen für den Dienstprinzipal in Notebook.

Weitere Informationen finden Sie unter [Verwenden von Azure Data Lake Storage mit Azure Databricks](/azure/databricks/data/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Beheben allgemeiner Probleme

Hier folgen einige Probleme, die mit Databricks auftreten können.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problem: Dieses Abonnement ist nicht für die Verwendung des Namespace „Microsoft.Databricks“ registriert.

#### <a name="error-message"></a>Fehlermeldung

„Dieses Abonnement ist nicht für die Verwendung des Namespace „Microsoft.Databricks“ registriert. Informationen zum Registrieren von Abonnements finden Sie unter https://aka.ms/rps-not-found. (Code: MissingSubscriptionRegistration)“

#### <a name="solution"></a>Lösung

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie **Abonnements** und das von Ihnen verwendete Abonnement aus. Wählen Sie anschließend **Ressourcenanbieter** aus. 
1. Wählen Sie in der Liste der Ressourcenanbieter für **Microsoft.Databricks** die Option **Registrieren** aus. Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um den Ressourcenanbieter zu registrieren.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf.

#### <a name="error-message"></a>Fehlermeldung

„Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf. Dieser Fehler kann auch auftreten, wenn Sie ein Gastbenutzer im Mandanten sind. Bitten Sie Ihren Administrator, Ihnen den Zugriff zu gewähren oder Sie als einen Benutzer direkt im Databricks-Arbeitsbereich hinzuzufügen.“ 

#### <a name="solution"></a>Lösung

Nachfolgend sind einige Lösungen für dieses Problem aufgeführt:

* Um den Mandanten zu initialisieren, müssen Sie als ein regulärer Benutzer des Mandanten und nicht als Gastbenutzer angemeldet sein. Sie müssen außerdem über die Rolle „Mitwirkender“ für die Databricks-Arbeitsbereichsressource verfügen. Sie können einem Benutzer Zugriff über die Registerkarte **Zugriffssteuerung (IAM)** in Ihrem Databricks-Arbeitsbereich im Azure-Portal gewähren.

* Dieser Fehler kann auch auftreten, wenn Ihr E-Mail-Domänenname mehreren Verzeichnissen in Azure AD zugewiesen ist. Um dieses Problem zu umgehen, erstellen Sie einen neuen Benutzer in dem Verzeichnis, das das Abonnement mit Ihrem Databricks-Arbeitsbereich enthält.

    a. Wechseln Sie im Azure-Portal zu Azure AD. Wählen Sie **Benutzer und Gruppen** > **Benutzer hinzufügen** aus.

    b. Fügen Sie einen Benutzer mit einer E-Mail-Adresse vom Typ `@<tenant_name>.onmicrosoft.com` anstelle einer E-Mail-Adresse vom Typ `@<your_domain>` hinzu. Sie finden diese Option unter Azure AD im Azure-Portal in **Benutzerdefinierte Domänen**.
    
    c. Gewähren Sie diesem neuen Benutzer die Rolle **Mitwirkender** für die Databricks-Arbeitsbereichsressource.
    
    d. Melden Sie sich beim Azure-Portal mit dem neuen Benutzer an, und suchen Sie den Databricks-Arbeitsbereich.
    
    e. Starten Sie den Databricks-Arbeitsbereich als dieser Benutzer.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Ihr Konto {E-Mail} wurde nicht in Databricks registriert. 

#### <a name="solution"></a>Lösung

Wenn Sie den Arbeitsbereich nicht erstellt haben und als Benutzer hinzugefügt werden, wenden Sie sich an die Person, die den Arbeitsbereich erstellt hat. Lassen Sie sich von dieser Person hinzufügen, indem Sie die Azure Databricks Admin Console verwenden. Anweisungen hierzu finden Sie unter [Hinzufügen und Verwalten von Benutzern](/azure/databricks/administration-guide/users-groups/users). Wenn Sie den Arbeitsbereich erstellt haben und weiterhin diese Fehlermeldung erhalten, wählen Sie im Azure-Portal erneut **Arbeitsbereich initialisieren** aus.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Problem: Startfehler des Cloudanbieters beim Einrichten des Clusters (PublicIPCountLimitReached)

#### <a name="error-message"></a>Fehlermeldung

„Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein unerwarteter Cloudanbieterfehler aufgetreten. Weitere Informationen finden Sie im Databricks-Handbuch. Azure-Fehlercode: PublicIPCountLimitReached. Azure-Fehlermeldung: Für das Abonnement in dieser Region können höchstens 60 öffentliche IP-Adressen erstellt werden.“

#### <a name="solution"></a>Lösung

Databricks-Cluster verwenden eine öffentliche IP-Adresse pro Knoten. Wenn für Ihr Abonnement bereits alle öffentlichen IP-Adressen verwendet werden, sollten Sie [eine Erhöhung des Kontingents anfordern](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Wählen Sie **Kontingent** als **Problemtyp** und **Netzwerk: ARM** als **Kontingenttyp** aus. Fordern Sie in **Details** eine Erhöhung des Kontingents für öffentliche IP-Adressen an. Wenn Ihr aktueller Grenzwert beispielsweise bei 60 liegt und ein Cluster mit 100 Knoten erstellt werden soll, fordern Sie eine Erhöhung auf 160 an.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Problem: Zweite Art von Startfehler des Cloudanbieters beim Einrichten des Clusters (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Fehlermeldung

„Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein unerwarteter Cloudanbieterfehler aufgetreten. Weitere Informationen finden Sie im Databricks-Handbuch.
Azure-Fehlercode: MissingSubscriptionRegistration Azure-Fehlermeldung: Das Abonnement ist nicht zur Verwendung von Namespace "Microsoft.Compute" registriert. Informationen zum Registrieren von Abonnements finden Sie unter https://aka.ms/rps-not-found.“

#### <a name="solution"></a>Lösung

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie **Abonnements** und das von Ihnen verwendete Abonnement aus. Wählen Sie anschließend **Ressourcenanbieter** aus. 
1. Wählen Sie in der Liste der Ressourcenanbieter für **Microsoft.Compute** die Option **Registrieren** aus. Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um den Ressourcenanbieter zu registrieren.

Ausführlichere Anweisungen finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Problem: Azure Databricks benötigt Berechtigungen zum Zugreifen auf Ressourcen in Ihrer Organisation, die nur ein Administrator erteilen kann.

#### <a name="background"></a>Hintergrund

Azure Databricks ist in Azure Active Directory integriert. Sie können durch Angabe von Benutzern aus Azure AD Berechtigungen innerhalb von Azure Databricks festlegen (z.B. für Notebooks oder Cluster). Um in Azure Databricks die Namen der Benutzer aus Azure AD aufzulisten, sind eine Leseberechtigung für diese Informationen und eine Einwilligung erforderlich. Wenn noch keine Zustimmung vorliegt, wird die Fehlermeldung angezeigt.

#### <a name="solution"></a>Lösung

Melden Sie sich als globaler Administrator beim Azure-Portal an. Navigieren Sie in Azure Active Directory zur Registerkarte **Benutzereinstellungen**, und stellen Sie sicher, dass **Benutzer können Apps den Zugriff auf Unternehmensdaten in ihrem Namen gestatten** auf **Ja** festgelegt ist.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Ausführen eines Spark-Auftrags in Azure Databricks mit dem Azure-Portal](quickstart-create-databricks-workspace-portal.md)
- [Was ist Azure Databricks?](what-is-azure-databricks.md)

