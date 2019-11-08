---
title: Preise für VM-Angebote | Azure Marketplace
description: Erläutert die drei Methoden zum Festlegen der Preise für VM-Angebote.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: c7ea5afeb46c30837c2ae53e871bb64f5d8cf292
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827334"
---
<a name="pricing-for-virtual-machine-offers"></a>Preise für VM-Angebote
==================================

Es gibt drei Möglichkeiten, die Preise für VM-Angebote anzugeben: angepasste Kernpreise, Preise pro Kern und Tabellenpreise.


<a name="customized-core-pricing"></a>Angepasste Kernpreise
-----------------------

Die Preise sind für jede Kombination aus Region und Kern unterschiedlich. Jede Region in der Verkaufsliste muss im **virtualMachinePricing**/**regionPrices**-Abschnitt der Definition angegeben werden.  Verwenden Sie für jede [Region](#regions) in Ihrer Anforderung die richtigen Währungscodes.  Das folgende Beispiel veranschaulicht diese Anforderungen:

``` json
    "virtualMachinePricing": 
    {
        ... 
        "coreMultiplier": 
        {
            "currency": "USD",
                "individually": 
                {
                    "sharedcore": 1,
                    "1core": 2,
                    "2core": 2,
                    "4core": 2,
                    "6core": 2,
                    "8core": 2,
                    "10core": 4,
                    "12core": 4,
                    "16core": 4,
                    "20core": 4,
                    "24core": 4,
                    "32core": 6,
                    "36core": 6,
                    "40core": 6,
                    "44core": 6,
                    "48core": 10,
                    "60core": 10,
                    "64core": 10,
                    "72core": 10,
                    "80core": 12,
                    "96core": 12,
                    "120core": 15,
                    "128core": 15,
                    "208core": 20,
                    "416core": 30
                }
        }
        ...
     }
```


<a name="per-core-pricing"></a>Preise pro Kern
----------------

In diesem Fall geben die Herausgeber einen Preis in US-Dollar für ihre SKU an, und alle anderen Preise werden automatisch generiert. Der Preis pro Kern wird im **einzelnen** Parameter in der Anforderung angegeben.

``` json
     "virtualMachinePricing": 
     {
         ...
         "coreMultiplier": 
         {
             "currency": "USD",
             "single": 1.0
         }
     }
```


<a name="spreadsheet-pricing"></a>Tabellenpreise
-------------------

Der Herausgeber kann auch seine Preistabelle an einen temporären Speicherort hochladen und dann den URI wie andere Dateiartefakte in die Anforderung einbeziehen. Die Tabelle wird dann hochgeladen, übersetzt, um den angegebenen Preisplan auszuwerten, und aktualisiert schließlich das Angebot mit den Preisinformationen. Nachfolgende GET-Anforderungen für das Angebot geben den Tabellen-URI und die ausgewerteten Preise für die Region zurück.

``` json
     "virtualMachinePricing": 
     {
         ...
         "spreadSheetPricing": 
         {
             "uri": "https://blob.storage.azure.com/<your_spreadsheet_location_here>/prices.xlsx",
         }
     }
```

<a name="new-core-sizes-added-on-722019"></a>Neue Kerngrößen wurden am 2.7.2019 hinzugefügt.
---------------------------

VM-Herausgeber wurden am 2. Juli 2019 über neue Preise für neue Größen virtueller Azure-Computer (basierend auf der Anzahl der Kerne) benachrichtigt.  Die neuen Preise gelten für die Kerngrößen 10, 44, 48, 60, 120, 208 und 416.  Für vorhandene VM-Angebote wurden die neue Preise für diese Kerngrößen basierend auf den aktuellen Preisen automatisch berechnet.  Herausgeber haben bis zum 1. August 2019 Zeit, die zusätzlichen Preise zu überprüfen und alle gewünschten Änderungen vorzunehmen.  Nach diesem Datum gelten die automatisch berechneten Preise für diese neuen Kerngrößen, wenn sie nicht bereits vom Herausgeber erneut veröffentlicht wurden.


<a name="regions"></a>Regions
-------

Die folgende Tabelle zeigt die verschiedenen Regionen, die Sie für angepasste Kernpreise angeben können, und ihre entsprechenden Währungscodes.

| **Region** | **Name**             | **Währungscode** |
|------------|----------------------|-------------------|
| DZ         | Algerien              | DZD               |
| AR         | Argentinien            | ARS               |
| AU         | Australien            | AUD               |
| AT         | Österreich              | EUR               |
| BH         | Bahrain              | BHD               |
| BY         | Belarus              | RUB               |
| BE         | Belgien              | EUR               |
| BR         | Brasilien               | USD               |
| BG         | Bulgarien             | BGN               |
| CA         | Kanada               | CAD               |
| CL         | Chile                | CLP               |
| CO         | Kolumbien             | COP               |
| CR         | Costa Rica           | CRC               |
| HR         | Kroatien              | HRK               |
| CY         | Zypern               | EUR               |
| CZ         | Tschechische Republik       | CZK               |
| DK         | Dänemark              | DKK               |
| DO         | Dominikanische Republik   | USD               |
| EC         | Ecuador              | USD               |
| EG         | Ägypten                | EGP               |
| SV         | El Salvador          | USD               |
| EE         | Estland              | EUR               |
| FI         | Finnland              | EUR               |
| BV         | Frankreich               | EUR               |
| DE         | Deutschland              | EUR               |
| GR         | Griechenland               | EUR               |
| GT         | Guatemala            | GTQ               |
| HK         | Hongkong (SAR)        | HKD               |
| HU         | Ungarn              | HUF               |
| IS         | Island              | ISK               |
| IN         | Indien                | INR               |
| id         | Indonesien            | IDR               |
| IE         | Irland              | EUR               |
| IL         | Israel               | ILS               |
| IT         | Italien                | EUR               |
| JP         | Japan                | JPY               |
| JO         | Jordan               | JOD               |
| KZ         | Kasachstan           | KZT               |
| KE         | Kenia                | KES               |
| KR         | Korea                | KRW               |
| KW         | Kuwait               | KWD               |
| LV         | Lettland               | EUR               |
| LI         | Liechtenstein        | CHF               |
| LT         | Litauen            | EUR               |
| LU         | Luxemburg           | EUR               |
| MK         | Nordmazedonien      | MKD               |
| MY         | Malaysia             | MYR               |
| MT         | Malta                | EUR               |
| MX         | Mexiko               | MXN               |
| ME         | Montenegro           | EUR               |
| MA         | Marokko              | MAD               |
| NL         | Niederlande          | EUR               |
| NZ         | Neuseeland          | NZD               |
| NG         | Nigeria              | NGN               |
| NO         | Norwegen               | NOK               |
| OM         | Oman                 | OMR               |
| PK         | Pakistan             | PKR               |
| PA         | Panama               | USD               |
| PY         | Paraguay             | PYG               |
| PE         | Peru                 | PEN               |
| PH         | Philippinen          | PHP               |
| PL         | Polen               | PLN               |
| PT         | Portugal             | EUR               |
| PR         | Puerto Rico          | USD               |
| QA         | Katar                | QAR               |
| RO         | Rumänien              | RON               |
| RU         | Russland               | RUB               |
| SA         | Saudi-Arabien         | SAR               |
| RS         | Serbien               | RSD               |
| SG         | Singapur            | SGD               |
| SK         | Slowakei             | EUR               |
| SI         | Slowenien             | EUR               |
| ZA         | Südafrika         | ZAR               |
| ES         | Spanien                | EUR               |
| LK         | Sri Lanka            | USD               |
| SE         | Schweden               | SEK               |
| CH         | Schweiz          | CHF               |
| TW         | Taiwan               | TWD               |
| TH         | Thailand             | THB               |
| TT         | Trinidad und Tobago  | TTD               |
| TN         | Tunesien              | TND               |
| TR         | Türkei               | TRY               |
| UA         | Ukraine              | UAH               |
| AE         | Vereinigte Arabische Emirate | EUR               |
| GB         | Vereinigtes Königreich       | GBP               |
| US         | USA        | USD               |
| UY         | Uruguay              | UYU               |
| VE         | Venezuela            | USD               |
|  |  |  |
