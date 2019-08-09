---
title: Voraussetzungen für Azure Security Center für IoT | Microsoft-Dokumentation
description: Hier finden Sie Details zu allen Voraussetzungen für den Einstieg in den Azure Security Center für IoT-Dienst.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: mlottner
ms.openlocfilehash: cc2dc3a190e3ad06bdc048f2a5770eae2a6990ec
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596819"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Voraussetzungen für Azure Security Center für IoT

Dieser Artikel erläutert die verschiedenen Bausteine des Azure Security Center (ASC) für IoT-Diensts, die Anforderungen für den Einstieg und die grundlegenden Konzepte, um den Dienst besser zu verstehen. 

## <a name="minimum-requirements"></a>Mindestanforderungen

- IoT Hub-Standard-Tarif
    - Berechtigungen der RBAC-Rolle auf **Besitzer**-Ebene 
- [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (empfohlen)
    - Die Verwendung von Azure Security Center ist eine Empfehlung und keine Anforderung. Ohne Azure Security Center können Sie Ihre anderen Azure-Ressourcen in IoT Hub nicht anzeigen. 
 
## <a name="working-with-asc-for-iot-service"></a>Arbeiten mit dem ASC für IoT-Dienst

ASC für IoT-Einblicke und -Berichte sind über Azure IoT Hub und Azure Security Center verfügbar. Um ASC für IoT auf Ihrer Instanz von Azure IoT Hub zu aktivieren, ist ein Konto mit Berechtigungen der Ebene **Besitzer** erforderlich. Nach der Aktivierung von ASC für IoT in Ihrer IoT Hub-Instanz werden ASC für IoT-Einblicke in Azure IoT Hub als das Feature **Sicherheit** und in Azure Security Center als **IoT** angezeigt. 

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche 

ASC für IoT wird derzeit für IoT Hub-Instanzen in den folgenden Azure-Regionen unterstützt:
  - USA (Mitte)  
  - East US 
  - USA (Ost) 2
  - USA, Westen-Mitte
  - USA (Westen)
  - USA, Westen 2
  - USA, Süden-Mitte
  - USA Nord Mitte
  - Kanada, Mitte
  - Kanada, Osten 
  - Nordeuropa    
  - Brasilien Süd
  - Frankreich, Mitte  
  - UK, Westen 
  - UK, Süden
  - Europa, Westen 
  - Nordeuropa 
  - Japan, Westen  
  - Japan, Osten  
  - Australien, Südosten
  - Australien (Osten)
  - Asien, Osten   
  - Asien, Südosten
  - Korea, Mitte
  - Korea, Süden 
  - Indien, Mitte
  - Indien (Süden)
  
## <a name="wheres-my-iot-hub"></a>Wo ist meine IoT Hub-Instanz?

Überprüfen Sie Ihren IoT Hub-Standort, um die Verfügbarkeit des Diensts zu sicherzustellen, bevor Sie beginnen. 

1. Öffnen Sie Ihre IoT Hub-Instanz. 
2. Klicken Sie auf **Overview**. 
3. Überprüfen Sie, ob der aufgeführte Standort mit einer der [unterstützten Dienstregionen](#supported-service-regions) übereinstimmt. 


## <a name="supported-platforms-for-agents"></a>Unterstützte Plattformen für Agents 

ASC für IoT-Agents unterstützt eine wachsende Anzahl von Geräten und Plattformen. In der [Liste der unterstützten Plattformen](how-to-deploy-agent.md) können Sie Ihre vorhandene oder geplante Gerätebibliothek überprüfen.  

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie die [Übersicht](overview.md) über Azure IoT-Sicherheit.
- Erfahren Sie, wie Sie [den Dienst aktivieren](quickstart-onboard-iot-hub.md).
- Lesen Sie [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md).
- Informieren Sie sich über [Verstehen der Warnungen in Azure Security Center für IoT](concept-security-alerts.md).
