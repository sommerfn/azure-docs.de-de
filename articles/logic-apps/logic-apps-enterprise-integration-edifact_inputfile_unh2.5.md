---
title: 'Verarbeiten von EDIFACT-Nachrichten mit UNH2.5-Segmenten: Azure Logic Apps | Microsoft-Dokumentation'
description: Auflösen von EDIFACT-Dokumenten mit UNH2.5-Segmenten in Azure Logic Apps mit Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/27/2017
ms.openlocfilehash: 926c9ebe8675d8b50d4544be813ae0b15492ae35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60681671"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Verarbeiten von EDIFACT-Dokumenten mit UNH2.5-Segmenten in Azure Logic Apps

Wenn das Feld „UNH2.5“ im EDIFACT-Dokument vorhanden ist, wird es zur Schemasuche verwendet. 

Beispiel: **EAN008** ist das UNH-Feld in der EDIFACT-Nachricht.  
UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'  

Schritte zum Verarbeiten der Nachricht 
1. Aktualisieren des Schemas
2. Überprüfen der Vereinbarungseinstellungen  

## <a name="update-the-schema"></a>Aktualisieren des Schemas
Um die Nachricht zu verarbeiten, müssen Sie ein Schema mit dem UNH2.5-Stammknotennamen bereitstellen.  Bei diesem Beispiel ist der Schemastammname **EFACT_D03B_ORDERS_EAN008**.  

Für alle D03B_ORDERS mit einem anderen UNH2.5-Segment müssen Sie ein eigenes Schema bereitstellen.  

## <a name="add-schema-to-the-edifact-agreement"></a>Hinzufügen des Schemas zur EDIFACT-Vereinbarung
### <a name="edifact-decode"></a>Decodieren von EDIFACT-Nachrichten
Um die eingehende Nachricht zu decodieren, konfigurieren Sie das Schema in den Empfangseinstellungen der EDIFACT-Vereinbarung.
1. Fügen Sie das Schema dem Integrationskonto hinzu.    
2. Konfigurieren Sie das Schema in den Empfangseinstellungen der EDIFACT-Vereinbarung. 
3. Wählen Sie die EDIFACT-Vereinbarung aus, und klicken Sie auf **Als JSON bearbeiten**.  Fügen Sie den UNH2.5-Wert in der Empfangsvereinbarung dem Abschnitt **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png) hinzu.

### <a name="edifact-encode"></a>Codieren von EDIFACT-Nachrichten
Um die eingehende Nachricht zu codieren, konfigurieren Sie das Schema in den Sendeeinstellungen der EDIFACT-Vereinbarung.
1. Fügen Sie das Schema dem Integrationskonto hinzu.    
2. Konfigurieren Sie das Schema in den Sendeeinstellungen der EDIFACT-Vereinbarung. 
3. Wählen Sie die EDIFACT-Vereinbarung aus, und klicken Sie auf **Als JSON bearbeiten**.  Fügen Sie den UNH2.5-Wert in der Sendevereinbarung dem Abschnitt **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png) hinzu.

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Integrationskontovereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  