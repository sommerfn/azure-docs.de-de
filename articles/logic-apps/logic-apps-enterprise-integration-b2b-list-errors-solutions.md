---
title: Fehler und Lösungen für B2B-Szenarien – Azure Logic Apps | Microsoft-Dokumentation
description: Fehler und Lösungen für B2B-Szenarien in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: f0591b47ce7ba6837f300088c856c0098fb66710
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60998812"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>B2B-Fehler und -Lösungen für Azure Logic Apps

Dieser Artikel hilft Ihnen bei der Problembehandlung von Fehlern, die möglicherweise in Logic Apps-B2B-Szenarien auftreten können, und schlägt entsprechende Aktionen zur Korrektur dieser Fehler vor.

## <a name="agreement-resolution"></a>Vereinbarungsauflösung

### <a name="no-agreement-found"></a>Keine Vereinbarung gefunden 

|   |   |  
|---|---|
| Fehlerbeschreibung | Es wurde keine Vereinbarung mit Vereinbarungsauflösungsparametern gefunden. | 
| Benutzeraktion | Die Vereinbarung sollte dem Integrationskonto mit vereinbarten Geschäftsidentitäten hinzugefügt werden. </br>Die Geschäftsidentitäten müssen mit den Eingabemeldungs-IDs übereinstimmen. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Keine Vereinbarung mit Identitäten gefunden

|   |   | 
|---|---|
| Fehlerbeschreibung | Keine Vereinbarung mit Identitäten gefunden: 'AS2Identity'::'Partner1' und 'AS2Identity'::'Partner3' | 
| Benutzeraktion | Ungültiges AS2-From oder AS2-To für Vereinbarung konfiguriert. </br>Korrigieren Sie den AS2-From- oder AS2-To-Header der AS2-Nachricht oder die Vereinbarung, sodass sie den AS2-IDs in den AS2-Nachrichtenheadern mit Vereinbarungskonfigurationen entspricht. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>AS2-Nachrichtenheader fehlen  

|   |   |  
|---|---|
| Fehlerbeschreibung | Ungültige AS2-Header. Der AS2-To- oder AS2-From-Header ist leer. | 
| Benutzeraktion | Eine AS2-Nachricht ohne AS2-From oder AS2-To bzw. beide Header wurde empfangen. </br> Überprüfen Sie den AS2-From- und AS2-To-Header der AS2-Nachricht, und korrigieren Sie ihn auf der Grundlage der Vereinbarungskonfiguration. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Text und Header von AS2-Nachricht fehlen    

|   |   |  
|---|---|
| Fehlerbeschreibung | Der Inhalt der Anforderung ist NULL oder leer. | 
| Benutzeraktion | Es wurde eine AS2-Nachricht ohne Nachrichtentext empfangen. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>Entschlüsselungsfehler bei AS2-Nachricht

|   |   | 
|---|---|
| Fehlerbeschreibung |  [processed/Error: decryption-failed] | 
| Benutzeraktion | Fügen Sie der AS2-Nachricht @base64ToBinary hinzu, bevor sie an den Partner gesendet wird. |
|||

Beispiel:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>MDN-Entschlüsselungsfehler

|   |   | 
|---|---|
| Fehlerbeschreibung |  [processed/Error: decryption-failed] | 
| Benutzeraktion | Fügen Sie MDN vor dem Senden an den Partner @base64ToBinary hinzu. | 
|||

Beispiel:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>Signaturzertifikat fehlt

|   |   |  
|---|---|
| Fehlerbeschreibung| Das Signaturzertifikat wurde nicht für die AS2-Partei konfiguriert. </br>AS2-From: partner1 AS2-To: partner2 | 
| Benutzeraktion | Konfigurieren Sie die AS2-Vereinbarungseinstellungen mit dem korrekten Zertifikat für die Signatur. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 und EDIFACT

### <a name="leading-or-trailing-space-found"></a>Vorangestelltes oder nachfolgendes Leerzeichen gefunden    
    
|   |   | 
|---|---|
| Fehlerbeschreibung | Fehler bei der Analyse. Der EDIFACT-Transaktionssatz mit der ID 123456 aus dem Austausch (ohne Gruppe) mit der ID 987654, der Absender-ID „Partner1“ und der Empfänger-ID „Partner2“ wird aufgrund folgender Fehler unterbrochen: <p>Vorangestelltes/Nachfolgendes Trennzeichen gefunden |
| Benutzeraktion | Die zu konfigurierenden Vereinbarungseinstellungen erlauben führende und nachstehende Leerzeichen. </br>Bearbeiten Sie die Vereinbarungseinstellungen, sodass vorangestellte und nachfolgende Leerzeichen zulässig sind. |
|   |   |

![Leerzeichen erlauben](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Duplikatüberprüfung wurde in der Vereinbarung aktiviert

|   |   | 
|---|---| 
| Fehlerbeschreibung | Doppelte Kontrollnummer |
| Benutzeraktion | Dieser Fehler zeigt an, dass die empfangene Nachricht über doppelte Kontrollnummern verfügt. </br>Korrigieren Sie die Kontrollnummer, und senden Sie die Nachricht erneut. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Schema in der Vereinbarung fehlt

|   |   | 
|---|---| 
| Fehlerbeschreibung | Fehler bei der Analyse. Der X12-Transaktionssatz mit der ID 564220001 in der Funktionsgruppe mit der ID 56422 aus dem Austausch mit der ID 000056422, der Absender-ID 12345678 und der Empfänger-ID 87654321 wird aufgrund folgender Fehler unterbrochen: <p>„Die Nachricht weist einen unbekannten Dokumenttyp auf und konnte in keines der vorhandenen Schemas aufgelöst werden, die in der Vereinbarung konfiguriert wurden.“ |
| Benutzeraktion | Konfigurieren Sie das Schema in den Vereinbarungseinstellungen.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Falsches Schema in der Vereinbarung

|   |   | 
|---|---| 
| Fehlerbeschreibung | Die Nachricht weist einen unbekannten Dokumenttyp auf und konnte in keines der vorhandenen Schemas aufgelöst werden, die in der Vereinbarung konfiguriert wurden. |
| Benutzeraktion | Konfigurieren Sie das korrekte Schema in den Vereinbarungseinstellungen. |
|   |   |

## <a name="flat-file"></a>Flatfile

### <a name="input-message-with-no-body"></a>Eingabenachricht ohne Text

|   |   | 
|---|---|
| Fehlerbeschreibung | InvalidTemplate. Vorlagensprachausdrücke in den Eingaben der Aktion "Flat_File_Decoding" in Zeile "1" und Spalte "1902" können nicht verarbeitet werden: Erforderliche Eigenschaft "Content" erwartet einen Wert, erhält jedoch NULL. Pfad ‚.‘“. |
| Benutzeraktion | Dieser Fehler gibt an, dass die Eingabenachricht keinen Text enthält. |
|   |   | 

