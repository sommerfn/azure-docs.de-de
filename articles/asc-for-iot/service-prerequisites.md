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
ms.date: 09/25/2019
ms.author: mlottner
ms.openlocfilehash: 4440fec98d1f561da6375bcaadba4282076cc53b
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299476"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Voraussetzungen für Azure Security Center für IoT

Dieser Artikel erläutert die verschiedenen Bausteine des Azure Security Center für IoT-Diensts, die Anforderungen für den Einstieg und die grundlegenden Konzepte des Diensts. 

## <a name="minimum-requirements"></a>Mindestanforderungen

- IoT Hub-Standard-Tarif
    - Berechtigungen der RBAC-Rolle auf **Besitzer**-Ebene 
- [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (empfohlen)
    - Die Verwendung von Azure Security Center ist eine Empfehlung und keine Anforderung. Ohne Azure Security Center können Sie Ihre anderen Azure-Ressourcen in IoT Hub nicht anzeigen. 
 
## <a name="working-with-azure-security-center-for-iot-service"></a>Verwenden des Azure Security Center für IoT-Diensts

Azure Security Center für IoT-Erkenntnisse und -Berichte sind über Azure IoT Hub und Azure Security Center verfügbar. Um Azure Security Center für IoT auf Ihrer Instanz von Azure IoT Hub zu aktivieren, ist ein Konto mit Berechtigungen der Ebene **Besitzer** erforderlich. Nach der Aktivierung von Azure Security Center für IoT in Ihrer IoT Hub-Instanz werden Azure Security Center für IoT-Erkenntnisse in Azure IoT Hub als das Feature **Sicherheit** und in Azure Security Center als **IoT** angezeigt. 

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche 

Azure Security Center für IoT wird derzeit für IoT Hub-Instanzen in den folgenden Azure-Regionen unterstützt:
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

Azure Security Center für IoT leitet den gesamten Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum von „Europa, Westen“ und aus allen anderen Regionen an das regionale Rechenzentrum von „USA, Mitte“ weiter.  
  
## <a name="wheres-my-iot-hub"></a>Wo ist meine IoT Hub-Instanz?

Überprüfen Sie Ihren IoT Hub-Standort, um die Verfügbarkeit des Diensts zu sicherzustellen, bevor Sie beginnen. 

1. Öffnen Sie Ihre IoT Hub-Instanz. 
2. Klicken Sie auf **Overview**. 
3. Überprüfen Sie, ob der aufgeführte Standort mit einer der [unterstützten Dienstregionen](#supported-service-regions) übereinstimmt. 


## <a name="supported-platforms-for-agents"></a>Unterstützte Plattformen für Agents 

Azure Security Center für IoT-Agents unterstützen eine wachsende Anzahl von Geräten und Plattformen. In der [Liste der unterstützten Plattformen](how-to-deploy-agent.md) können Sie Ihre vorhandene oder geplante Gerätebibliothek überprüfen.  

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie die [Übersicht](overview.md) über Azure IoT-Sicherheit.
- Erfahren Sie, wie Sie [den Dienst aktivieren](quickstart-onboard-iot-hub.md).
- Lesen Sie [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md).
- Informieren Sie sich über [Verstehen der Warnungen in Azure Security Center für IoT](concept-security-alerts.md).
