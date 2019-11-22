---
title: Architektur für die kontinuierliche Patientenüberwachung in Azure IoT Central | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu einer Lösungsarchitektur für die kontinuierliche Patientenüberwachung.
author: philmea
ms.author: philmea
ms.date: 10/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: cf8cfc5fe75ff5364f812af034d772c7aadd891c
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027470"
---
# <a name="continuous-patient-monitoring-architecture"></a>Architektur für die ständige Überwachung von Patienten

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Lösungen für die kontinuierliche Patientenüberwachung können mithilfe der bereitgestellten App-Vorlage sowie auf der Grundlage der weiter unten skizzierten Architektur erstellt werden.

>[!div class="mx-imgBorder"] 
>![Architektur für die kontinuierliche Patientenüberwachung](media/cpm-architecture.png)

1. Medizinische Geräte, die über BLE (Bluetooth Low Energy) kommunizieren
1. Mobiltelefongateway, das BLE-Daten empfängt und an IoT Central sendet
1. Kontinuierlicher Datenexport von Patientengesundheitsdaten nach Azure API for FHIR&reg;
1. Machine Learning auf der Grundlage interoperabler Daten
1. Auf FHIR-Daten basierendes Dashboard für das Patientenversorgungsteam

## <a name="details"></a>Details
In diesem Abschnitt werden die einzelnen Teile des Architekturdiagramms ausführlicher erläutert.

### <a name="ble-medical-devices"></a>Medizinische BLE-Geräte
Viele medizinische Wearables im IoT-Gesundheitsbereich sind BLE-Geräte (Bluetooth Low Energy). Diese Geräte können nicht direkt mit der Cloud kommunizieren und benötigen daher ein Gateway. In dieser Architektur wird als Gateway eine Mobiltelefonanwendung verwendet.

### <a name="mobile-phone-gateway"></a>Mobiltelefongateway
Die Hauptfunktion der Mobiltelefonanwendung besteht darin, BLE-Daten von medizinischen Geräten zu erfassen und an Azure IoT Central weiterzugeben. Darüber hinaus kann die App Patienten bei der Geräteeinrichtung und -bereitstellung unterstützen und ihre persönlichen Gesundheitsdaten anzeigen. Bei anderen Lösungen kann auch ein Tabletgateway oder ein statisches Gateway innerhalb eines Krankenzimmers verwendet werden, um diesen Kommunikationsfluss zu implementieren.

### <a name="export-to-azure-api-for-fhirreg"></a>Exportieren nach Azure API for FHIR&reg;
Azure IoT Central ist zwar HIPAA-konform und HITRUST&reg;-zertifiziert, unter Umständen möchten Sie jedoch auch Patientengesundheitsdaten an Azure API for FHIR senden. [Azure API for FHIR](../../healthcare-apis/overview.md) ist eine vollständig verwaltete, standardbasierte, konforme API für Gesundheitsdaten von Kliniken, mit der Sie neue Systems of Engagement für Ihre Gesundheitsdaten entwickeln können. Sie ermöglicht einen schnellen Datenaustausch über FHIR-APIs und basiert auf einem verwalteten PaaS-Angebot (Platform-as-a-Service) in der Cloud. Daten können mithilfe der Funktion „Kontinuierlicher Datenexport“ von IoT Central an Azure API for FHIR gesendet werden.

### <a name="machine-learning"></a>Machine Learning
Nachdem Sie Ihre Daten aggregiert und in das FHIR-Format konvertiert haben, können Sie Machine Learning-Modelle erstellen, um bessere Erkenntnisse zu gewinnen und es Ihrem Patientenversorgungsteam zu ermöglichen, intelligentere Entscheidungen zu treffen. Für die Erstellung, das Trainieren und die Bereitstellung von Machine Learning-Modellen stehen verschiedene Dienste zur Verfügung. Weitere Informationen zur Verwendung der Machine Learning-Angebote von Azure finden Sie in unserer [Machine Learning-Dokumentation](../../machine-learning/index.yml).

### <a name="provider-dashboard"></a>Anbieterdashboard
Die Daten in Azure API for FHIR können zur Erstellung eines Dashboards mit Patienteninformationen verwendet oder direkt in eine EPA integriert werden, um Patientenversorgungsteams bei der Visualisierung des Patientenstatus zu unterstützen. Patientenversorgungsteams können dieses Dashboard nutzen, um Maßnahmen für Patienten zu ergreifen, die Unterstützung benötigen. Außerdem lassen sich damit frühzeitig Anzeichen für eine Verschlechterung erkennen. Informationen zum Erstellen eines Power BI-Echtzeitanbieterdashboards finden Sie in unserer [Schrittanleitung](howto-health-data-triage.md).

## <a name="next-steps"></a>Nächste Schritte
* [Tutorial: Bereitstellen einer App für die ständige Überwachung von Patienten und exemplarische Vorgehensweise für die zugehörige Vorlage](tutorial-continuous-patient-monitoring.md)