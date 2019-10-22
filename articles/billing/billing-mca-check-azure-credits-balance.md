---
title: Nachverfolgen des Azure-Gutschriftensaldos für eine Microsoft-Kundenvereinbarung
description: Es wird beschrieben, wie Sie den Azure-Gutschriftensaldo für eine Microsoft-Kundenvereinbarung überprüfen.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 4eae7299ab696b01c57a27fd46cbf903c9395152
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375531"
---
# <a name="track-microsoft-customer-agreement-azure-credit-balance"></a>Nachverfolgen des Azure-Gutschriftensaldos der Microsoft-Kundenvereinbarung

Sie können das Azure-Guthaben Ihres Abrechnungskontos für eine Microsoft-Kundenvereinbarung im Azure-Portal überprüfen. 

Das Guthaben kann zur Bezahlung von Gebühren genutzt werden, für die diese Art der Bezahlung möglich ist. Wenn Sie Produkte verwenden, für die kein Guthaben genutzt werden kann, oder wenn Ihre Nutzung das Guthaben übersteigt, werden Ihnen die entsprechenden Kosten in Rechnung gestellt. Weitere Informationen finden Sie unter [Von Azure-Gutschriften nicht abgedeckte Produkte](#products-that-arent-covered-by-azure-credits).

Guthaben wird im Abrechnungskonto für eine Microsoft-Kundenvereinbarung einem Abrechnungsprofil zugewiesen. Jedes Abrechnungsprofil verfügt über ein eigenes Guthaben. Sie müssen über die Rolle „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ für das Abrechnungsprofil oder über die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ für das Abrechnungskonto verfügen, um das Azure-Guthaben für ein Abrechnungsprofil anzeigen zu können. Weitere Informationen zu den Rollen finden Sie unter [Grundlegendes zu Verwaltungsrollen für Microsoft-Kundenvereinbarungen in Azure](billing-understand-mca-roles.md).

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Kundenvereinbarung haben.](#check-access-to-a-microsoft-customer-agreement)

## <a name="check-your-credit-balance-in-the-azure-portal"></a>Überprüfen Ihres Guthabens im Azure-Portal

1. Melden Sie sich beim [Azure-Portal]( https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3.  Wählen Sie links die Option **Azure-Gutschriften**. Abhängig von Ihren Zugriffsberechtigungen müssen Sie unter Umständen zuerst ein Abrechnungskonto oder -profil und dann **Azure-Gutschriften** wählen.

4. Auf dieser Seite werden die folgenden Informationen angezeigt:

   ![Screenshot: Guthaben und Transaktionen für ein Abrechnungsprofil](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Begriff               | Definition                           |
   |--------------------|--------------------------------------------------------|
   | Geschätzter Saldo  | Geschätztes Guthaben unter Einbeziehung aller in Rechnung gestellten und ausstehenden Transaktionen |
   | Aktueller Saldo    | Guthaben seit der letzten Rechnung, ohne Berücksichtigung der ausstehenden Transaktionen |
   | Transaktionen       | Abrechnungstransaktionen, die sich auf Ihr Azure-Guthaben ausgewirkt haben |

   Fällt Ihr geschätzter Saldo auf Null, wird Ihnen jegliche Nutzung in Rechnung gestellt – auch für Produkte, für die Guthaben genutzt werden kann.

6. Klicken Sie auf **Credits list** (Guthabenliste), um eine Liste der Guthaben für das Abrechnungsprofil anzuzeigen. Diese Guthabenliste enthält die folgenden Informationen:

   ![Screenshot: Guthabenliste für ein Abrechnungsprofil](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Begriff | Definition |
   |---|---|
   | `Source` | Die Erwerbsquelle des Guthabens |
   | Startdatum | Das Datum, an dem Sie die Gutschrift erhalten haben |
   | Ablaufdatum | Das Datum, an dem das Guthaben verfällt |
   | Aktueller Saldo | Der Saldo seit Ihrer letzten Rechnung |
   | Ursprünglicher Betrag | Der ursprüngliche Guthabenbetrag |
   | Status | Der aktuelle Status des Guthabens. Die möglichen Status sind „Aktiv“, „Used“ (Verwendet), „Expired“ (Abgelaufen) und „Läuft ab“. |

## <a name="check-your-credit-balance-programmatically"></a>Programmgesteuertes Überprüfen Ihres Guthabens

Mithilfe der Azure-APIs für die [Abrechnung](https://docs.microsoft.com/rest/api/billing/) und [Nutzung](https://docs.microsoft.com/rest/api/consumption/) können Sie das Guthaben für Ihr Abrechnungskonto programmgesteuert abrufen.

In den weiter unten bereitgestellten Beispielen werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

### <a name="find-billing-profiles-you-have-access-to"></a>Suchen von Abrechnungsprofilen, auf die Sie Zugriff haben

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?$expand=billingProfiles&api-version=2019-10-01-preview
```
In der API-Antwort wird eine Liste mit Abrechnungskonten und deren Abrechnungsprofilen zurückgegeben.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx",
      "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "billingProfiles": [
          {
            "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx",
            "name": "PBFV-xxxx-xxx-xxx",
            "properties": {
              "address": {
                "addressLine1": "AddressLine1",
                "city": "City",
                "companyName": "CompanyName",
                "country": "Country",
                "postalCode": "xxxxx",
                "region": "Region"
              },
              "currency": "USD",
              "displayName": "Development",
              "hasReadAccess": true,
              "invoiceDay": 5,
              "invoiceEmailOptIn": true
            },
            "type": "Microsoft.Billing/billingAccounts/billingProfiles"
          }
        ],
        "displayName": "Contoso",
        "hasReadAccess": true,
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Identifizieren Sie anhand der Eigenschaft `displayName` das Abrechnungsprofil, für das Sie das Guthaben überprüfen möchten. Kopieren Sie die ID (`id`) des Abrechnungsprofils. Wenn Sie also beispielsweise das Guthaben für das Abrechnungsprofil **Development** überprüfen möchten, kopieren Sie ```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx```. Halten Sie diesen Wert für den nächsten Schritt bereit.

### <a name="get-azure-credit-balance"></a>Abrufen des Azure-Guthabens 

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<billingProfileId>` durch den `id`-Wert, den Sie im ersten Schritt (```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx```) kopiert haben. 

```json
GET https://management.azure.com<billingProfileId>/providers/Microsoft.Consumption/credits/balanceSummary?api-version=2019-10-01
```

In der API-Antwort werden der geschätzte und der aktuelle Saldo für das Abrechnungsprofil zurückgegeben.

```json
{
  "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx/providers/Microsoft.Consumption/credits/balanceSummary/57c2e8df-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "57c2e8df-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Consumption/credits/balanceSummary",
  "eTag": null,
  "properties": {
    "balanceSummary": {
      "estimatedBalance": {
        "currency": "USD",
        "value": 996.13
      },
      "currentBalance": {
        "currency": "USD",
        "value": 997.87
      }
    },
    "pendingCreditAdjustments": {
      "currency": "USD",
      "value": 0.0
    },
    "expiredCredit": {
      "currency": "USD",
      "value": 0.0
    },
    "pendingEligibleCharges": {
      "currency": "USD",
      "value": -1.74
    }
  }
}
```

| Elementname  | BESCHREIBUNG                                                                           |
|---------------|---------------------------------------------------------------------------------------|
| `estimatedBalance` | Ihr geschätztes Guthaben nach Berücksichtigung aller in Rechnung gestellten und ausstehenden Transaktionen. |
| `currentBalance`   | Das Guthaben seit Ihrer letzten Rechnung ohne Berücksichtigung ausstehender Transaktionen.    |
| `pendingCreditAdjustments`      | Die noch nicht fakturierten Anpassungen (beispielsweise Erstattungen).  |
| `expiredCredit`      |  Das Guthaben, das seit Ihrer letzten Rechnung verfallen ist.  |
| `pendingEligibleCharges`  | Die noch nicht fakturierten Gebühren, für die Guthaben genutzt werden kann.   |

### <a name="get-list-of-credits"></a>Abrufen der Guthabenliste

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<billingProfileId>` durch den `id`-Wert, den Sie im ersten Schritt (```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx```) kopiert haben. 

```json
GET https://management.azure.com<billingProfileId>/providers/Microsoft.Consumption/lots?api-version=2019-10-01
```
In der API-Antwort werden Listen mit Azure-Guthaben für ein Abrechnungsprofil zurückgegeben.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx/providers/Microsoft.Consumption/lots/f2ecfd94-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "f2ecfd94-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Consumption/lots",
      "eTag": null,
      "properties": {
        "originalAmount": {
          "currency": "USD",
          "value": 500.0
        },
        "closedBalance": {
          "currency": "USD",
          "value": 500.0
        },
        "source": "Azure Promotional Credit",
        "startDate": "09/18/2019 21:47:31",
        "expirationDate": "09/18/2020 21:47:30",
        "poNumber": ""
      }
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/xxxx-xxxx-xxx-xxx/providers/Microsoft.Consumption/lots/4ea40eb5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4ea40eb5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Consumption/lots",
      "eTag": null,
      "properties": {
        "originalAmount": {
          "currency": "USD",
          "value": 500.0
        },
        "closedBalance": {
          "currency": "USD",
          "value": 497.87
        },
        "source": "Azure Promotional Credit",
        "startDate": "09/18/2019 21:47:31",
        "expirationDate": "09/18/2020 21:47:30",
        "poNumber": ""
      }
    }
  ]
}
```
| Elementname  | BESCHREIBUNG                                                                                               |
|---------------|-----------------------------------------------------------------------------------------------------------|
| `originalAmount` | Der ursprüngliche Guthabenbetrag. |
| `closedBalance`   | Der Saldo seit der letzten Rechnung.    |
| `source`      | Die Quelle, die definiert, von wem/wie das Guthaben erlangt wurde. |
| `startDate`      |  Das Datum, an dem das Guthaben aktiv wurde.  |
| `expirationDate`  | Das Datum, an dem das Guthaben verfällt.   |
| `poNumber`  | Die Bestellnummer der Rechnung, über die das Guthaben abgerechnet wurde.   |

### <a name="get-transactions-that-affected-credit-balance"></a>Abrufen von Transaktionen mit Auswirkungen auf das Guthaben

Führen Sie die folgende Anforderung aus, und ersetzen Sie dabei `<billingProfileId>` durch den `id`-Wert, den Sie im ersten Schritt (```providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx```) kopiert haben. Sie müssen ein Startdatum (**startDate**) und ein Enddatum (**endDate**) übergeben, um Transaktionen für den gewünschten Zeitraum zu erhalten.

```json
GET https://management.azure.com<billingProfileId>/providers/Microsoft.Consumption/events?api-version=2019-10-01&startDate=2018-10-01T00:00:00.000Z&endDate=2019-10-11T12:00:00.000Z?api-version=2019-10-01
```
In der API-Antwort werden alle Transaktionen zurückgegeben, die sich auf das Guthaben für Ihr Abrechnungsprofil ausgewirkt haben.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx`/providers/Microsoft.Consumption/events/e2032eb5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "e2032eb5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Consumption/events",
      "eTag": null,
      "properties": {
        "transactionDate": "10/11/2019",
        "description": "Credit eligible charges as of 10/11/2019",
        "newCredit": {
          "currency": "USD",
          "value": 0.0
        },
        "adjustments": {
          "currency": "USD",
          "value": 0.0
        },
        "creditExpired": {
          "currency": "USD",
          "value": 0.0
        },
        "charges": {
          "currency": "USD",
          "value": -1.74
        },
        "closedBalance": {
          "currency": "USD",
          "value": 998.26
        },
        "eventType": "PendingCharges",
        "invoiceNumber": ""
      }
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx/providers/Microsoft.Consumption/events/381efd80-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "381efd80-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Consumption/events",
      "eTag": null,
      "properties": {
        "transactionDate": "09/18/2019",
        "description": "New credit added on 09/18/2019",
        "newCredit": {
          "currency": "USD",
          "value": 500.0
        },
        "adjustments": {
          "currency": "USD",
          "value": 0.0
        },
        "creditExpired": {
          "currency": "USD",
          "value": 0.0
        },
        "charges": {
          "currency": "USD",
          "value": 0.0
        },
        "closedBalance": {
          "currency": "USD",
          "value": 1000.0
        },
        "eventType": "PendingNewCredit",
        "invoiceNumber": ""
      }
    }
  ]
}
```
| Elementname  | BESCHREIBUNG                                                                                               |
|---------------|-----------------------------------------------------------------------------------------------------------|
| `transactionDate` | Das Datum der Transaktion. |
| `description` | Die Beschreibung der Transaktion. |
| `adjustments`   | Die Guthabenanpassungen für die Transaktion.    |
| `creditExpired`      | Das verfallene Guthaben. |
| `charges`      |  Die Gebühren für die Transaktion.  |
| `closedBalance`  | Der Saldo nach der Transaktion.   |
| `eventType`  | Der Transaktionstyp   |
| `invoiceNumber`  | Die Rechnungsnummer der Rechnung, über die die Transaktion abgerechnet wurde. Bei ausstehenden Transaktionen ist kein Wert angegeben.   |

## <a name="how-credits-are-used"></a>Verwenden von Gutschriften

In einem Abrechnungskonto für eine Microsoft-Kundenvereinbarung verwenden Sie Abrechnungsprofile, um Ihre Rechnungen und Zahlungsmethoden zu verwalten. Für jedes Abrechnungsprofil wird eine monatliche Rechnung erstellt. Die Zahlungsmethoden verwenden Sie zum Bezahlen der Rechnung.

Sie weisen erlangtes Guthaben einem Abrechnungsprofil zu. Wenn für das Abrechnungsprofil eine Rechnung generiert wird, wird das Guthaben automatisch mit den in Rechnung gestellten Gesamtgebühren verrechnet. Sie bezahlen den Restbetrag mit Ihrer Zahlungsmethode (also beispielsweise per Scheck/Überweisung oder Kreditkarte).

## <a name="products-that-arent-covered-by-azure-credits"></a>Von Azure-Gutschriften nicht abgedeckte Produkte

 Die folgenden Produkte werden nicht von Ihren Azure-Gutschriften abgedeckt. Die Abrechnung erfolgt für die Verwendung dieser Produkte unabhängig von Ihrem Guthabensaldo:

- Canonical
- Citrix XenApp Essentials
- Citrix XenDesktop
- Registrierter Benutzer
- OpenLogic
- Registrierter Benutzer von Remote Access Rights XenApp Essentials
- Ubuntu Advantage
- Visual Studio Enterprise (monatlich)
- Visual Studio Enterprise (jährlich)
- Visual Studio Professional (monatlich)
- Visual Studio Professional (jährlich)
- Azure Marketplace-Produkte
- Azure-Supportpläne

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte

- [Understand billing account for Microsoft Customer Agreement (Grundlegendes zum Abrechnungskonto für eine Microsoft-Kundenvereinbarung)](billing-mca-overview.md)
- [Understand terms on your Microsoft Customer Agreement invoice (Grundlegendes zu den Begriffen auf der Rechnung für eine Microsoft-Kundenvereinbarung)](billing-mca-understand-your-invoice.md)
