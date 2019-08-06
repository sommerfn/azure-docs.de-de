---
title: Aktivieren des Diensts „Azure Security Center für IoT“ in IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Dienst „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz aktivieren.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2019
ms.author: mlottner
ms.openlocfilehash: a794ccea13323f38b20906458e216f85652bfc3e
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596963"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Schnellstart: Durchführen des Onboardings für den Dienst „Azure Security Center für IoT“ in IoT Hub

In diesem Artikel erfahren Sie, wie Sie den Dienst „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz aktivieren. Besitzen Sie derzeit keine IoT Hub-Instanz, lesen Sie zum Einstieg die Informationen unter [Erstellen eines IoT Hubs über das Portal](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal). 

> [!NOTE]
> Azure Security Center für IoT unterstützt derzeit nur IoT Hub-Instanzen im Standard-Tarif.
> Azure Security Center für IoT ist eine Lösung für einen einzelnen Hub. Sollten Sie mehrere Hubs benötigen, sind mehrere Lösungen in Azure Security Center für IoT erforderlich. 

## <a name="prerequisites-for-enabling-the-service"></a>Voraussetzungen für die Dienstaktivierung

- Log Analytics-Arbeitsbereich
  - Von Azure Security Center für IoT werden in Ihrem Log Analytics-Arbeitsbereich standardmäßig zwei Arten von Informationen gespeichert: **Sicherheitswarnungen** und **Empfehlungen**. 
  - Sie können noch einen weiteren Informationstyp hinzufügen: **Rohereignisse**. Hinweis: Für die Speicherung von **Rohereignissen** in Log Analytics fallen zusätzliche Speicherkosten an. 
- IoT Hub (Standard-Tarif)
- Erfüllen aller [Dienstvoraussetzungen](service-prerequisites.md) 

|Unterstützte Azure-Dienstregionen | ||
|---|---|---|
| USA (Mitte) |East US |USA (Ost) 2 |
| USA, Westen-Mitte |USA (Westen) |USA, Westen 2 |
| USA, Süden-Mitte|USA Nord Mitte | Kanada, Mitte|
| Kanada, Osten| Nordeuropa|Brasilien Süd|
| Frankreich, Mitte| UK, Westen|UK, Süden|
|Europa, Westen|Nordeuropa| Japan, Westen|
|Japan, Osten | Australien, Südosten|Australien (Osten)|
|Asien, Osten| Asien, Südosten| Korea, Mitte|
|Korea, Süden| Indien, Mitte| Indien (Süden)|
|

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>Aktivieren von Azure Security Center für IoT in Ihrer IoT Hub-Instanz 

Führen Sie die folgenden Schritte aus, um die Sicherheit in Ihrer IoT Hub-Instanz zu aktivieren: 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**. 
1. Klicken Sie im Menü **Sicherheit** auf **Secure your IoT solution** (IoT-Lösung schützen).
1. Behalten Sie die Standardeinstellung **Aktivieren** bei. 
1. Wählen Sie Ihren Log Analytics-Arbeitsbereich aus.
1. Geben Sie die Details Ihres Log Analytics-Arbeitsbereichs an. 
   - Legen Sie die Einstellung für **twin collection** (Zwillingserfassung) auf **Ein** fest, um die Zwillingserfassung zu **aktivieren**.
   - Wählen Sie in Log Analytics **Store raw device security events** (Sicherheitsrohereignisse für Geräte speichern) aus, um zusätzlich zu den Speicher-Standardinformationstypen **Rohereignisse** zu speichern. Lassen Sie die Umschaltfläche **Rohereignisse** auf **Ein** festgelegt. 
    
1. Klicken Sie auf **Speichern**. 

Glückwunsch! Sie haben Azure Security Center für IoT in Ihrer IoT Hub-Instanz aktiviert. 

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Ihre Lösung konfigurieren:

> [!div class="nextstepaction"]
> [Konfigurieren Ihrer IoT-Lösung](quickstart-configure-your-solution.md)


