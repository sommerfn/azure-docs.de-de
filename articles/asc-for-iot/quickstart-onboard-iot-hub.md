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
ms.date: 10/17/2019
ms.author: mlottner
ms.openlocfilehash: 67361c402bdbc82d5df01709dc962b59671cb2aa
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991397"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Schnellstart: Durchführen des Onboardings für den Dienst „Azure Security Center für IoT“ in IoT Hub

In diesem Artikel erfahren Sie, wie Sie den Dienst „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz aktivieren. Besitzen Sie derzeit keine IoT Hub-Instanz, lesen Sie zum Einstieg die Informationen unter [Erstellen eines IoT Hubs über das Portal](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal). 

> [!NOTE]
> Azure Security Center für IoT unterstützt derzeit nur IoT Hub-Instanzen im Standard-Tarif.


## <a name="prerequisites-for-enabling-the-service"></a>Voraussetzungen für die Dienstaktivierung

- Log Analytics-Arbeitsbereich
  - Von Azure Security Center für IoT werden in Ihrem Log Analytics-Arbeitsbereich standardmäßig zwei Arten von Informationen gespeichert: **Sicherheitswarnungen** und **Empfehlungen**. 
  - Sie können noch einen weiteren Informationstyp hinzufügen: **Rohereignisse**. Hinweis: Für die Speicherung von **Rohereignissen** in Log Analytics fallen zusätzliche Speicherkosten an. 
- IoT Hub (Standard-Tarif)
- Erfüllen aller [Dienstvoraussetzungen](service-prerequisites.md) 

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>Aktivieren von Azure Security Center für IoT in Ihrer IoT Hub-Instanz 

So aktivieren Sie die Sicherheit für Ihre IoT Hub-Instanz: 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**. 
1. Klicken Sie im Menü **Sicherheit** auf **Secure your IoT solution** (IoT-Lösung schützen).    


Glückwunsch! Sie haben Azure Security Center für IoT in Ihrer IoT Hub-Instanz aktiviert. 

### <a name="geolocation-and-ip-address-handling"></a>Geolocation und Verarbeitung von IP-Adressen

Zum Schutz Ihrer IoT-Lösung werden standardmäßig IP-Adressen von eingehenden und ausgehenden Verbindungen für Ihre IoT-Geräte, für IoT Edge und für IoT-Hubs erfasst und gespeichert. Diese Informationen werden für die Erkennung ungewöhnlicher Verbindungen von verdächtigen IP-Quellen benötigt. Beispiele wären etwa Verbindungsversuche von einer IP-Quelle eines bekannten Botnets oder von einer IP-Quelle außerhalb Ihres geografischen Standorts. Mit dem Dienst „Azure Security Center für IoT“ kann die Erfassung von IP-Adressdaten jederzeit flexibel aktiviert oder deaktiviert werden. 

So aktivieren oder deaktivieren Sie die Erfassung von IP-Adressdaten: 

1. Öffnen Sie Ihre IoT Hub-Instanz, und wählen Sie im Menü **Sicherheit** die Option **Übersicht** aus. 
2. Wählen Sie den Bildschirm **Einstellungen** aus, und ändern Sie die Einstellungen für Geolocation und/oder IP-Behandlung gemäß Ihren Anforderungen.

### <a name="log-analytics-creation"></a>Log Analytics-Erstellung

Wenn Azure Security Center für IoT aktiviert ist, wird ein Azure Log Analytics-Standardarbeitsbereich erstellt, um Rohdaten für Sicherheitsereignisse, Warnungen und Empfehlungen für Ihre IoT-Geräte sowie für IoT Edge und IoT Hub zu speichern. Die ersten 5 GB an Daten, die monatlich pro Kunde im Azure Log Analytics-Dienst erfasst werden, sind kostenlos. Jedes GB an Daten, das in Ihrem Azure Log Analytics-Arbeitsbereich erfasst wird, wird für die ersten 31 Tage kostenfrei gespeichert. Weitere Informationen zu den Preisen für Log Analytics finden Sie [hier](https://azure.microsoft.com/pricing/details/monitor/).

So ändern Sie die Arbeitsbereichskonfiguration von Log Analytics:

1. Öffnen Sie Ihre IoT Hub-Instanz, und wählen Sie im Menü **Sicherheit** die Option **Übersicht** aus. 
2. Wählen Sie den Bildschirm **Einstellungen** aus, und ändern Sie die Arbeitsbereichskonfiguration der Log Analytics-Einstellungen gemäß Ihren Anforderungen.

### <a name="customize-your-iot-security-solution"></a>Anpassen Ihrer IoT-Sicherheitslösung
Durch Aktivieren der Lösung „Azure Security Center für IoT“ werden standardmäßig alle IoT Hub-Instanzen in Ihrem Azure-Abonnement automatisch geschützt. 

So aktivieren oder deaktivieren Sie den Dienst „Azure Security Center für IoT“ für eine bestimmte IoT Hub-Instanz: 

1. Öffnen Sie Ihre IoT Hub-Instanz, und wählen Sie im Menü **Sicherheit** die Option **Übersicht** aus. 
2. Wählen Sie den Bildschirm **Einstellungen** aus, und ändern Sie die Sicherheitseinstellungen eines beliebigen IoT-Hubs in Ihrem Azure-Abonnement gemäß Ihren Anforderungen.


## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Ihre Lösung konfigurieren:

> [!div class="nextstepaction"]
> [Konfigurieren Ihrer IoT-Lösung](quickstart-configure-your-solution.md)


