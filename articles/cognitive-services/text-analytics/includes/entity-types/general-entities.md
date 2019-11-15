---
title: Allgemeine benannte Entitäten
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/18/2019
ms.author: aahi
ms.openlocfilehash: 693a81cfb15407541311d7ab053bb2ab6a267b29
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799870"
---
## <a name="general-entity-types"></a>Allgemeine Entitätstypen:

### <a name="person"></a>Person
Erkannte Namen und weitere Personen im Text.
Sprachen:
* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG             |
|--------------|-------------------------|
| –          | Erkannte Namen, z.B. `Bill Gates`, `Marie Curie` |

### <a name="location"></a>Location

Natürliche und von Menschen hergestellte Wahrzeichen, Strukturen und geografische Merkmale.

Sprachen:


* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                                      |
|--------------|--------------------------------------------------------------------------------------------------|
| –          | Orte, z.B. `Atlantic Ocean`, `library`, `Eiffel Tower`, `Statue of Liberty` |

### <a name="organization"></a>Organisation  

Erkannte Organisationen, Unternehmen, Agenturen und andere Personengruppen. Beispiel: Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen. Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt. Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                                      |
|--------------|--------------------------------------------------------------------------------------------------|
| –          | Organisationen, z.B. `Microsoft`, `NASA` `National Oceanic and Atmospheric Administration` |

### <a name="phone-number"></a>Rufnummer

Telefonnummern 

Sprachen:


* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                  |
|----------|----------------------------------------------|
| –         | Telefonnummern, z.B. `+1 123-123-123` |

### <a name="email"></a>Email

E-Mail-Adresse. 

Sprachen:


* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                  |
|----------|----------------------------------------------|
| –         | E-Mail-Adresse, z. B. `support@contoso.com` |

### <a name="url"></a>URL

Internet-URLs.

Sprachen:


* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                           |
|----------|-------------------------------------------------------|
| –         | URLs zu Websites, z.B. `https://www.bing.com`. |

###  <a name="number"></a>Number

Zahlen und numerische Mengen. 

Sprachen:


* Öffentliche Vorschau: `English`

| Name des Untertyps    | Beispiele                     |
|-------------|------------------------------|
| –         | `6`, `six`                   |
| Prozentsatz  | `50%`, `fifty percent`       |
| Ordinal     | `2nd`, `second`              |
| Currency    | `$10.99`, `€30.00`           |
| Dimension   | `10 miles`, `40 cm`          |
| Temperatur | `32 degrees`, `10°C`         |
