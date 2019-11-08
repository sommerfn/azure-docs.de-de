---
title: Benannte Entitäten für personenbezogene Informationen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/21/2019
ms.author: aahi
ms.openlocfilehash: 6e46d58b3f1ef33902ab44043e954d9718d865af
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506881"
---
## <a name="personal-information-entity-types"></a>Entitätstypen für personenbezogene Informationen:

### <a name="person"></a>Person
Erkannte Namen und weitere Personen im Text.
Sprachen:
* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG             |
|--------------|-------------------------|
| –          | Erkannte Namen, z. B. `Bill Gates`, `Marie Curie` |

### <a name="organization"></a>Organisation  

Erkannte Organisationen, Unternehmen, Agenturen und andere Personengruppen. Beispiel: Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen. Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt. Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                                      |
|--------------|--------------------------------------------------------------------------------------------------|
| –          | Organisationen, z. B. `Microsoft`, `NASA` `National Oceanic and Atmospheric Administration` |

### <a name="phone-number"></a>Rufnummer

Telefonnummern 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps           | BESCHREIBUNG                                           |
|------------------------|-------------------------------------------------------|
| –                    | Telefonnummern, z. B. `+1 123-123-123`          |
| EU-Telefonnummer        | Telefonnummern, die spezifisch für die Europäische Union sind.         |
| EU-Mobiltelefonnummer | Mobiltelefonnummern, die spezifisch für die Europäische Union sind. |

### <a name="eu-gps-coordinates"></a>EU-GPS-Koordinaten

 GPS-Koordinaten für Standorte innerhalb der Europäischen Union. 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                               |
|--------------|-------------------------------------------|
| –          | GPS-Koordinaten innerhalb der Europäischen Union |

### <a name="azure-information"></a>Azure-Informationen

Identifizierbare Azure-Informationen, Authentifizierungsinformationen und Verbindungszeichenfolgen eingeschlossen. 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps                          | BESCHREIBUNG                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB-Authentifizierungsschlüssel             | Autorisierungsschlüssel für einen Azure DocumentDB-Server.                           |
| Verbindungszeichenfolge für Azure-IaaS-Datenbank | Verbindungszeichenfolge für eine Azure-IaaS-Datenbank (Infrastructure-as-a-Service). |
| Azure SQL-Verbindungszeichenfolge           | Verbindungszeichenfolge für eine Azure SQL-Datenbank.                                |
| Azure IoT-Verbindungszeichenfolge           | Verbindungszeichenfolge für Azure IoT (Internet of Things).                        |
| Azure-Veröffentlichungseinstellung: Kennwort        | Kennwort für die Azure-Veröffentlichungseinstellungen.                                        |
| Azure Redis Cache-Verbindungszeichenfolge   | Verbindungszeichenfolge für eine Azure Redis Cache-Instanz.                             |
| Azure SaaS                             | Verbindungszeichenfolge für Azure SaaS (Software-as-a-Service).                     |
| Azure Service Bus-Verbindungszeichenfolge   | Verbindungszeichenfolge für eine Azure Service Bus-Instanz.                                |
| Azure Storage-Kontoschlüssel             | Schlüssel für ein Azure-Speicherkonto.                                   |
| Azure Storage-Kontoschlüssel (generisch)   | Generischer Schlüssel für ein Azure-Speicherkonto.                           |
| SQL Server-Verbindungszeichenfolge          | Verbindungszeichenfolge für eine SQL Server-Instanz.                                         |

### <a name="identification"></a>Identifikation

Sprachen:

* Öffentliche Vorschau: `English`

#### <a name="financial-account-identification"></a>Identifikation bei Finanzkonten

| Name des Untertyps               | BESCHREIBUNG                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA-Nummern        | ABA-Nummern (American Bankers Association) für den Zahlungsverkehr mit US-amerikanischen Banken.                  |
| SWIFT-Code                 | SWIFT-Codes in Zahlungsanweisungen.                           |
| Kreditkarte                | Kreditkartennummern                                                       |
| IBAN-Code                  | IBAN-Codes in Zahlungsanweisungen.                            |

#### <a name="government-and-country-specific-identification"></a>Behördliche und landesspezifische Informationen zur Identifikation

Die nachstehenden Entitäten werden nach Land gruppiert und aufgeführt:

Argentinien
* Nationale argentinische ID-Nummer (DNI)

Australien
* Steuernummer 
* Führerscheinnummer
* Reisepass-ID
* Krankenversicherungsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Belgien
* Nationalregisternummer

Brasilien
* Nummer juristischer Personen (CNPJ)
* CPF-Nummer
* Nationale ID-Karte (RG)

Kanada
* Reisepassnummer
* Führerscheinnummer
* Krankenversicherungsnummer
* Persönliche Krankenversicherungsnummer (Personal Health ID Number, PHIN)
* Sozialversicherungsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Chile
* ID-Kartennummer 

China
* ID-Kartennummer
* Personalausweisnummer (PRC)

Kroatien
* ID-Kartennummer
* Persönliche Identifikationsnummer (OID)

Tschechische Republik
* Personalausweisnummer

Dänemark
* Persönliche Identifikationsnummer

Europäische Union (EU)
* Nationale Identifikationsnummer
* Reisepassnummer
* Führerscheinnummer
* Sozialversicherungsnummer (oder gleichwertig)
* EU-Steueridentifikationsnummer (TIN)
* EU-Debitkartennummer

Finnland
* Nationale Identifikationsnummer
* Reisepassnummer

Frankreich
* Personalausweisnummer (CNI)
* Sozialversicherungsnummer (INSEE)
* Reisepassnummer
* Führerscheinnummer

Deutschland
* Personalausweisnummer
* Reisepassnummer
* Führerscheinnummer

Griechenland 
* Personalausweisnummer

Hongkong
* Personalausweisnummer (HKID)

Indien
* PAN (Permanent Account Number)
* Eindeutige ID-Nummer (Aadhaar)

Indonesien
* ID-Kartennummer (KTP)

Irland
* Sozialversicherungsnummer (PPS)

Israel
* Nationale Identifikationsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Italien
* Führerscheinnummer

Japan
* Melderegisternummer
* Personalausweisnummer
* Führerscheinnummer
* Sozialversicherungsnummer (SIN)
* Reisepassnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Malaysia
* ID-Kartennummer

Niederlande
* Persönliche Identifikationsnummer (BSN)

Neuseeland
* Verwaltungsnummer des Gesundheitsministeriums

Norwegen
* ID-Kartennummer

Philippinen
* Einheitliche Mehrzweck-ID-Nummer (Unified Multi-Purpose ID)

Polen
* Personalausweisnummer
* Nationale Identifikationsnummer (PESEL)
* Reisepassnummer

Portugal 
* ID-Kartennummer

Saudi-Arabien
* Nationale Identifikationsnummer

Singapur
* Nationale Registrierungs-ID-Kartennummer (NRIC)

Südafrika
* ID-Nummer
* Melderegisternummer

Südkorea
* Melderegisternummer

Spanien 
* US-Sozialversicherungsnummer (Social Security Number, SSN)

Schweden
* Nationale Identifikationsnummer
* Reisepassnummer

Taiwan 
* Nationale Identifikationsnummer
* Anwohnerzertifikatnummer (ARC/TARC)
* Reisepassnummer

Thailand
* Bürgeridentifikationscode

Vereinigtes Königreich
* Reisepassnummer
* Führerscheinnummer
* Sozialversicherungsnummer (NINO)
* Gesundheitsdienstnummer (NHS)

USA
* US-Sozialversicherungsnummer (Social Security Number, SSN)
* Führerscheinnummer
* Reisepassnummer
* Wählerverzeichnisnummer
* Kennungsnummer für Steuerzahler (ITIN)
* DEA-Nummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)