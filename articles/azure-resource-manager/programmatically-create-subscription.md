---
title: Programmgesteuertes Erstellen von Azure-Abonnements | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie programmgesteuert zusätzliche Azure-Abonnements erstellen.
services: azure-resource-manager
author: amberb
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: banders
ms.openlocfilehash: 5d977fd6ce74f9cabedd0553c5815fd64d4d09a7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376006"
---
# <a name="programmatically-create-azure-subscriptions-preview"></a>Programmgesteuertes Erstellen von Azure-Abonnements (Vorschau)

Azure-Kunden mit einem Abrechnungskonto vom Typ [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) oder [Microsoft Partner-Vereinbarung (Microsoft Partner Agreement, MPA)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) können Abonnements programmgesteuert erstellen. Dieser Artikel enthält Informationen zum programmgesteuerten Erstellen von Abonnements mithilfe von Azure Resource Manager.

Wenn Sie ein Azure-Abonnement programmgesteuert erstellen, unterliegt es der Vereinbarung, auf deren Grundlage Sie Azure-Dienste von Microsoft oder einem autorisierten Handelspartner erhalten haben. Weitere Informationen finden Sie unter [Rechtliche Informationen zu Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-subscriptions-for-an-ea-billing-account"></a>Erstellen von Abonnements für ein Abrechnungskonto vom Typ „EA“

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“ für ein Registrierungskonto verfügen, um ein Abonnement zu erstellen. Es gibt zwei Möglichkeiten, wie Sie die Rolle erhalten:

* Der Enterprise-Administrator Ihrer Registrierung kann [Sie zum Kontobesitzer machen](https://ea.azure.com/helpdocs/addNewAccount) (Anmeldung erforderlich), damit Sie zum Besitzer des Registrierungskontos werden.

* Ein vorhandener Besitzer des Registrierungskontos kann [Ihnen Zugriff gewähren](grant-access-to-create-subscription.md). Wenn Sie einen Dienstprinzipal verwenden möchten, um ein EA-Abonnement zu erstellen, müssen Sie entsprechend [diesem Dienstprinzipal die Berechtigung zum Erstellen von Abonnements erteilen](grant-access-to-create-subscription.md).

### <a name="find-accounts-you-have-access-to"></a>Ermitteln der Konten, auf die Sie Zugriff besitzen

Nachdem Sie einem Registrierungskonto hinzugefügt wurden, das einem Kontobesitzer zugeordnet ist, verwendet Azure die Beziehung zwischen dem Konto und der Registrierung für die Ermittlung, wo die Abonnementgebühren angerechnet werden sollen. Alle Abonnements, die unter dem Konto erstellt werden, werden über die EA-Registrierung abgerechnet, in der sich das Konto befindet. Um Abonnements zu erstellen, müssen Sie Werte zum Registrierungskonto und den Benutzerprinzipalen als Besitzer des Abonnements übergeben. 

Um die folgenden Befehle ausführen zu können, müssen Sie im *Basisverzeichnis* des Kontobesitzers angemeldet sein. Dies ist das Verzeichnis, in dem Abonnements standardmäßig erstellt werden.

### <a name="resttabrest"></a>[REST](#tab/rest)

Fordern Sie eine Liste aller Registrierungskonten an, auf die Sie zugreifen können:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

In der API-Antwort sind alle Registrierungskonten aufgelistet, auf die Sie Zugriff haben:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

Verwenden Sie die `principalName`-Eigenschaft zum Identifizieren des Kontos, in dem Abonnements abgerechnet werden sollen. Kopieren Sie den `name`-Wert dieses Kontos. Wenn Sie beispielsweise Abonnements unter dem Registrierungskonto SignUpEngineering@contoso.com erstellen wollten, müssten Sie ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` kopieren. Dieser Bezeichner ist die Objekt-ID des Registrierungskontos. Kopieren Sie diesen Wert, um ihn im nächsten Schritt als `enrollmentAccountObjectId`-Wert zu verwenden.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Öffnen Sie [Azure Cloud Shell](https://shell.azure.com/), und wählen Sie PowerShell aus.

Verwenden Sie das Cmdlet [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure gibt eine Liste aller Registrierungskonten zurück, auf die Sie zugreifen können:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```
Verwenden Sie die `principalName`-Eigenschaft zum Identifizieren des Kontos, in dem Abonnements abgerechnet werden sollen. Kopieren Sie den `ObjectId`-Wert dieses Kontos. Wenn Sie beispielsweise Abonnements unter dem Registrierungskonto SignUpEngineering@contoso.com erstellen wollten, müssten Sie ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` kopieren. Kopieren Sie diese Objekt-ID, um sie im nächsten Schritt als `enrollmentAccountObjectId`-Wert zu verwenden.

### <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den Befehl [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurecli-interactive 
az billing enrollment-account list
```

Azure gibt eine Liste aller Registrierungskonten zurück, auf die Sie zugreifen können:

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "SignUpEngineering@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  },
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "BillingPlatformTeam@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  }
]
```

Verwenden Sie die `principalName`-Eigenschaft zum Identifizieren des Kontos, in dem Abonnements abgerechnet werden sollen. Kopieren Sie den `name`-Wert dieses Kontos. Wenn Sie beispielsweise Abonnements unter dem Registrierungskonto SignUpEngineering@contoso.com erstellen wollten, müssten Sie ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` kopieren. Dieser Bezeichner ist die Objekt-ID des Registrierungskontos. Kopieren Sie diesen Wert, um ihn im nächsten Schritt als `enrollmentAccountObjectId`-Wert zu verwenden.

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Erstellen von Abonnements unter einem bestimmten Registrierungskonto

Im folgenden Beispiel wird ein Abonnement namens *Dev Team Subscription* in Registrierungskonto erstellt, das im vorherigen Schritt ausgewählt wurde. Bei dem Abonnementangebot handelt es sich um *MS-AZR-0017P* (reguläres Microsoft Enterprise Agreement). Es werden optional auch zwei Benutzer als RBAC-Besitzer für das Abonnement hinzugefügt.

### <a name="resttabrest"></a>[REST](#tab/rest)

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<enrollmentAccountObjectId>` durch den `name`-Wert, den Sie im ersten Schritt (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopiert haben. Wie Sie Besitzer festlegen, erfahren Sie [hier](grant-access-to-create-subscription.md#userObjectId).

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Nein      | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `offerType`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Nein       | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |

In der Antwort wird ein `subscriptionOperation`-Objekt für die Überwachung zurückgegeben. Nach Abschluss der Erstellung des Abonnements gibt das `subscriptionOperation`-Objekt ein `subscriptionLink`-Objekt zurück, das die Abonnement-ID enthält.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Installieren Sie zunächst das Vorschaumodul, indem Sie `Install-Module Az.Subscription -AllowPrerelease` ausführen. Um sicherzustellen, dass `-AllowPrerelease` funktioniert, installieren Sie eine aktuelle Version von PowerShellGet aus [PowerShellGet-Modul abrufen](/powershell/gallery/installing-psget).

Führen Sie den folgenden [New-AzSubscription](/powershell/module/az.subscription)-Befehl aus, und ersetzen Sie dabei `<enrollmentAccountObjectId>` durch den `ObjectId`-Wert, den Sie im ersten Schritt (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) abgerufen haben. Wie Sie Besitzer festlegen, erfahren Sie [hier](grant-access-to-create-subscription.md#userObjectId).

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Nein      | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `OfferType`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Ja       | Zeichenfolge | Die Objekt-ID des Registrierungskontos, unter dem das Abonnement erstellt und abgerechnet wird. Dieser Wert ist eine GUID, die Sie von `Get-AzEnrollmentAccount` abrufen. |
| `OwnerObjectId`      | Nein       | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `OwnerSignInName`    | Nein       | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden.|
| `OwnerApplicationId` | Nein       | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden. Wenn Sie diesen Parameter verwenden zu können, benötigt das Dienstprinzipal [Lesezugriff auf das Verzeichnis](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [New-AzSubscription](/powershell/module/az.subscription).

### <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Installieren Sie zunächst die Vorschauerweiterung, indem Sie `az extension add --name subscription` ausführen.

Führen Sie den folgenden [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create)-Befehl aus, und ersetzen Sie dabei `<enrollmentAccountObjectId>` durch den `name`-Wert, den Sie im ersten Schritt (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopiert haben. Wie Sie Besitzer festlegen, erfahren Sie [hier](grant-access-to-create-subscription.md#userObjectId).

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Nein      | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `offer-type`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Ja       | Zeichenfolge | Die Objekt-ID des Registrierungskontos, unter dem das Abonnement erstellt und abgerechnet wird. Dieser Wert ist eine GUID, die Sie von `az billing enrollment-account list` abrufen. |
| `owner-object-id`      | Nein       | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `owner-upn`    | Nein       | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden.|
| `owner-spn` | Nein       | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden. Wenn Sie diesen Parameter verwenden zu können, benötigt das Dienstprinzipal [Lesezugriff auf das Verzeichnis](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

---

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Einschränkungen der Azure Enterprise-Abonnementerstellungs-API

- Nur Azure Enterprise-Abonnements können mithilfe dieser API erstellt werden.
- Es sind maximal 200 Abonnements pro Registrierungskonto möglich. Im Anschluss können weitere Abonnements für das Konto nur über das Azure-Portal erstellt werden. Wenn Sie über die API weitere Abonnements erstellen möchten, erstellen Sie ein weiteres Registrierungskonto.
- Benutzer, die keine Kontobesitzer sind, aber per RBAC einem Registrierungskonto hinzugefügt wurden, können über das Azure-Portal keine Abonnements erstellen.
- Sie können den Mandanten nicht auswählen, in dem das Abonnement erstellt werden soll. Das Abonnement wird immer im Basismandanten des Kontobesitzers erstellt. Wenn Sie das Abonnement in einen anderen Mandanten verschieben möchten, finden Sie weitere Informationen unter [Ändern des Abonnementmandanten](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).


## <a name="create-subscriptions-for-an-mca-account"></a>Erstellen von Abonnements für ein Konto vom Typ „MCA“

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“, „Mitwirkender“ oder „Azure-Abonnementersteller“ für einen Rechnungsabschnitt oder die Rolle „Besitzer“ oder „Mitwirkender“ für ein Abrechnungsprofil oder -konto verfügen, um Abonnements erstellen zu können. Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](../billing/billing-understand-mca-roles.md#subscription-billing-roles-and-tasks).

Im Beispiel unten werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben 

Verwenden Sie die unten angegebene Anforderung, um alle Abrechnungskonten aufzulisten.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
In der API-Antwort sind die Abrechnungskonten aufgeführt, auf die Sie Zugriff haben.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Contoso",
        "hasReadAccess": true,
        "organizationId": "41b29574-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Fabrikam",
        "hasReadAccess": true,
        "organizationId": "41b29574-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```
Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftCustomerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) des Kontos.  Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie diesen Wert für den nächsten Schritt bereit.

### <a name="find-invoice-sections-to-create-subscriptions"></a>Suchen nach Rechnungsabschnitten zum Erstellen von Abonnements

Die Gebühren für Ihr Abonnement werden in einem Abschnitt der Rechnung eines Abrechnungsprofils angezeigt. Verwenden Sie die folgende API, um die Liste mit den Rechnungsabschnitten und Abrechnungsprofilen abzurufen, für die Sie über die Berechtigung zum Erstellen von Azure-Abonnements verfügen.

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<billingAccountName>` durch den `name`-Wert, den Sie im ersten Schritt (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) kopiert haben.

```json
POST https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/listInvoiceSectionsWithCreateSubscriptionPermission?api-version=2019-10-01-preview
```
In der API-Antwort sind alle Rechnungsabschnitte und die zugehörigen Abrechnungsprofile aufgelistet, für die Sie über Zugriff zum Erstellen von Abonnements verfügen:

```json
{
    "value": [{
        "billingProfileDisplayName": "Contoso finance",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx",
        "enabledAzurePlans": [{
            "productId": "DZH318Z0BPS6",
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
        }, {
            "productId": "DZH318Z0BPS6",
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
        }],
        "invoiceSectionDisplayName": "Development",
        "invoiceSectionId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx/invoiceSections/GJ77-xxxx-xxx-xxx"
    }, {
        "billingProfileDisplayName": "Contoso finance",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx",
        "enabledAzurePlans": [{
            "productId": "DZH318Z0BPS6",
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
        }, {
            "productId": "DZH318Z0BPS6",
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
        }],
        "invoiceSectionDisplayName": "Testing",
        "invoiceSectionId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX"
  }]
}
    
```

Verwenden Sie die `invoiceSectionDisplayName`-Eigenschaft, um den Rechnungsabschnitt zu identifizieren, für den Sie Abonnements erstellen möchten. Kopieren Sie die Elemente `invoiceSectionId` und `billingProfileId` sowie eines der `skuId`-Elemente für den Rechnungsabschnitt. Wenn Sie beispielsweise ein Abonnement vom Typ `Microsoft Azure plan` für den Rechnungsabschnitt `Development` erstellen möchten, kopieren Sie `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX`, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-xxxx-xxx-xxx` und `0001`. Halten Sie diese Werte für den nächsten Schritt bereit.

### <a name="create-a-subscription-for-an-invoice-section"></a>Erstellen eines Abonnements für einen Rechnungsabschnitt

Im folgenden Beispiel wird ein Abonnement mit dem Namen *Dev Team subscription* vom Typ *Microsoft Azure Plan* für den Rechnungsabschnitt *Development* erstellt. Die Kosten für das Abonnement werden dem Abrechnungsprofil von *Contoso finance* berechnet und im Abschnitt *Development* der Rechnung aufgeführt. 

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<invoiceSectionId>` durch den `invoiceSectionId`-Wert, den Sie im zweiten Schritt kopiert haben (```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX```). Sie müssen die Elemente `billingProfileId` und `skuId` übergeben, die Sie im zweiten Schritt in den Anforderungsparametern der API kopiert haben. Wie Sie Besitzer festlegen, erfahren Sie [hier](grant-access-to-create-subscription.md#userObjectId).

```json
POST https://management.azure.com<invoiceSectionId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview
```

```json
'{"displayName": "Dev Team subscription",
  "billingProfileId": "<billingProfileId>",
  "skuId": "<skuId>",
  "owners": [
      {
        "objectId": "<userObjectId>"
      },
      {
        "objectId": "<servicePrincipalObjectId>"
      }
    ],
  "costCenter": "35683",
  "managementGroupId": "/providers/Microsoft.Management/managementGroups/xxxxxxx",",
}'

```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Ja      | Zeichenfolge | Der Anzeigename des Abonnements.|
| `billingProfileId`   | Ja      | Zeichenfolge | Die ID des Abrechnungsprofils, dem die Gebühren für das Abonnement berechnet werden.  |
| `skuId` | Ja      | Zeichenfolge | Die SKU-ID, die den Typ des Azure-Plans bestimmt. |
| `owners`      | Nein       | Zeichenfolge | Die Objekt-ID eines Benutzers oder Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement bei der Erstellung hinzufügen möchten.  |
| `costCenter` | Nein      | Zeichenfolge | Die Kostenstelle, die dem Abonnement zugeordnet ist. Wird in der CSV-Datei zur Nutzung angezeigt. |
| `managementGroupId` | Nein      | Zeichenfolge | Die ID der Verwaltungsgruppe, der das Abonnement hinzugefügt wird. Informationen zum Erhalt der Liste mit den Verwaltungsgruppen finden Sie im API-Artikel [Verwaltungsgruppen – Liste](https://docs.microsoft.com/rest/api/resources/managementgroups/list). Verwenden Sie die ID einer Verwaltungsgruppe über die API. |

In der Antwort wird ein `subscriptionCreationResult`-Objekt für die Überwachung zurückgegeben. Nach Abschluss der Erstellung des Abonnements gibt das `subscriptionCreationResult`-Objekt ein `subscriptionLink`-Objekt zurück, das die Abonnement-ID enthält.

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>Erstellen von Abonnements für ein Abrechnungskonto vom Typ „MPA“

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen im Cloudlösungsanbieter-Konto Ihrer Organisation über die Rolle „Globaler Administrator“ oder „Administrator-Agent“ verfügen, um ein Abonnement für Ihr Abrechnungskonto erstellen zu können. Weitere Informationen finden Sie unter [Zuweisen von Rollen und Berechtigungen zu Benutzern](https://docs.microsoft.com/partner-center/permissions-overview).

Im Beispiel unten werden REST-APIs verwendet. PowerShell und die Azure CLI werden derzeit nicht unterstützt.

### <a name="find-the-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben 

Verwenden Sie die unten angegebene Anforderung, um alle Abrechnungskonten aufzulisten, auf die Sie Zugriff haben.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
In der API-Antwort sind die Abrechnungskonten aufgeführt.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftPartnerAgreement",
        "displayName": "Contoso",
        "hasReadAccess": true,
        "organizationId": "1d100e69-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Fabrikam",
        "hasReadAccess": true,
        "organizationId": "1d100e69-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```
Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftPartnerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) für das Konto. Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie diesen Wert für den nächsten Schritt bereit.

### <a name="find-customers-that-have-azure-plans"></a>Suchen nach Kunden mit Azure-Plänen

Senden Sie die folgende Anforderung, indem Sie `<billingAccountName>` durch das `name`-Element aus dem ersten Schritt (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) ersetzen, um alle Kunden unter dem Abrechnungskonto aufzulisten, für die Sie Azure-Abonnements erstellen können. 

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/customers?api-version=2019-10-01-preview
```
In der API-Antwort sind die Kunden des Abrechnungskontos aufgelistet, die über Azure-Pläne verfügen. Sie können Abonnements für diese Kunden erstellen.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Contoso USD",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/JUT6-xxxx-xxxx-xxxx",
        "displayName": "Fabrikam toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/97c3fac4-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "97c3fac4-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Fabrikam sports",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/JUT6-xxxx-xxxx-xxxx",
        "displayName": "Fabrikam bakery"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    }]
}
    
```

Verwenden Sie die `displayName`-Eigenschaft, um den Kunden zu identifizieren, für den Sie Abonnements erstellen möchten. Kopieren Sie die `id` für den Kunden. Wenn Sie beispielsweise ein Abonnement für `Fabrikam toys` erstellen möchten, kopieren Sie `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. Halten Sie diesen Wert für die nachfolgenden Schritte bereit.

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Optional für indirekte Anbieter: Abrufen der Handelspartner für einen Kunden

Falls Sie im Rahmen des zweistufigen Cloudlösungsanbieter-Modells als indirekter Anbieter fungieren, können Sie einen Handelspartner angeben, während Sie Abonnements für Kunden erstellen. 

Senden Sie die folgende Anforderung, indem Sie `<customerId>` durch die `id` aus dem zweiten Schritt (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) ersetzen, um alle Handelspartner aufzulisten, die für einen Kunden verfügbar sind.

```json
GET https://management.azure.com<customerId>?$expand=resellers&api-version=2019-10-01-preview
```
In der API-Antwort sind die Handelspartner für den Kunden aufgeführt:

```json
{
  "value": [{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "3xxxxx",
        "description": "Wingtip"
      }
    ]
  }
},
{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/4ed2c793-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "4ed2c793-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "5xxxxx",
        "description": "Tailspin"
      }
    ]
  }
}]
}
```
Verwenden Sie die `description`-Eigenschaft, um den Handelspartner zu identifizieren, der dem Abonnement zugeordnet werden soll. Kopieren Sie die `resellerId` für den Handelspartner. Wenn Sie beispielsweise `Wingtip` zuordnen möchten, kopieren Sie `3xxxxx`. Halten Sie diesen Wert für den nächsten Schritt bereit.

### <a name="create-a-subscription-for-a-customer"></a>Erstellen eines Abonnements für einen Kunden

Im folgenden Beispiel wird ein Abonnement mit dem Namen *Dev Team subscription* für *Fabrikam toys* erstellt, und anschließend wird dem Abonnement der Handelspartner *Wingtip* zugeordnet. T

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<customerId>` durch den `id`-Wert, den Sie im ersten Schritt (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopiert haben. Übergeben Sie die optionale *resellerId*, die Sie im zweiten Schritt in den Anforderungsparametern der API kopiert haben. 

```json
POST https://management.azure.com<customerId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview
```

```json
'{"displayName": "Dev Team subscription",
  "skuId": "0001",
  "resellerId": "<resellerId>",
}'
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Ja      | Zeichenfolge | Der Anzeigename des Abonnements.|
| `skuId` | Ja      | Zeichenfolge | Die SKU-ID des Azure-Plans. Verwenden Sie für Abonnements vom Typ „Microsoft Azure-Plan“ die ID *0001*. |
| `resellerId`      | Nein       | Zeichenfolge | Die MPN-ID des Handelspartners, der dem Abonnement zugeordnet wird.  |

In der Antwort wird ein `subscriptionCreationResult`-Objekt für die Überwachung zurückgegeben. Nach Abschluss der Erstellung des Abonnements gibt das `subscriptionCreationResult`-Objekt ein `subscriptionLink`-Objekt zurück, das die Abonnement-ID enthält.

## <a name="next-steps"></a>Nächste Schritte

* Ein Beispiel zur Erstellung eines EA-Abonnements (Enterprise Agreement) per .NET finden Sie auf [dieser GitHub-Seite mit Beispielcode](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Da Sie nun ein Abonnement erstellt haben, können Sie diese Möglichkeit auch für andere Benutzer und Dienstprinzipale eröffnen. Weitere Informationen finden Sie unter [Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements (Vorschau) ](grant-access-to-create-subscription.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).
