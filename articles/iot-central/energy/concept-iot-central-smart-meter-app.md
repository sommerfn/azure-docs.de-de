---
title: Architektonische Konzepte in Azure IoT Central – Energie | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte im Zusammenhang mit der Architektur der Energie-Anwendungsvorlage von Azure IoT Central vorgestellt.
author: op-ravi
ms.author: omravi
ms.date: 10/22/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: cab593990369bb8a4135e82534a2eb409b574e96
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582760"
---
# <a name="azure-iot-central---smart-meter-app-architecture"></a>Azure IoT Central – Architektur der Intelligenter Stromzähler-Anwendung

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Dieser Artikel bietet eine Übersicht der Architektur der Vorlage der Überwachungs-App für intelligente Stromzähler. Das nachstehende Diagramm zeigt eine häufig verwendete Architektur für die Intelligenter Stromzähler-App in Azure unter Verwendung der IoT Central-Plattform.

> [!div class="mx-imgBorder"]
> ![Architektur für intelligente Stromzähler](media/concept-iot-central-smart-meter/smart-meter-app-architecture.png)

Diese Architektur umfasst die folgenden Komponenten. Für einige Lösungen sind möglicherweise nicht alle der hier aufgeführten Komponenten erforderlich.

## <a name="smart-meters-and-connectivity"></a>Intelligente Stromzähler und Konnektivität 

Ein intelligenter Stromzähler zählt unter den Energieressourcen zu den wichtigsten Geräten. Er zeichnet Daten zum Energieverbrauch auf und kommuniziert sie an Hilfsprogramme zur Überwachung und für andere Anwendungen, wie etwa Abrechnung und Reaktion auf die Nachfrage. Je nach Art des intelligenten Stromzählers kann er Verbindungen mit IoT Central entweder über Gateways oder über andere zwischengeschaltete Geräte oder Systeme, wie etwa Edgegeräte und Kopfteilgeräte, herstellen. Erstellen Sie eine IoT Central-Gerätebridge, um Geräte zu verbinden, die nicht direkt verbunden werden können. Bei der IoT Central-Gerätebridge handelt es sich um eine Open-Source-Lösung. Die Details finden Sie [hier](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge). 


## <a name="iot-central-platform"></a>IoT Central-Plattform

Azure IoT Central ist eine Plattform, die das Erstellen Ihrer IoT-Lösungen vereinfacht und den Aufwand und die Kosten für die Verwaltung, den Betrieb und die Entwicklung von IoT-Lösungen senkt. Mit IoT Central können Sie Ihre Sie IoT-Ressourcen (Internet of Things) einfach und in jeder Größenordnung verbinden, überwachen und verwalten. Nachdem Sie Ihre intelligenten Stromzähler mit IoT Central verbunden haben, verwendet die App-Vorlage integrierte Features wie Gerätemodelle, Befehle und Dashboards. Die App-Vorlage verwendet außerdem den IoT Central-Speicher für Szenarien mit warmen Pfaden, z.B. Datenüberwachung nahezu in Echtzeit, Analysen, Regeln und Visualisierung. 


## <a name="extensibility-options-to-build-with-iot-central"></a>Erweiterungsoptionen für das Erstellen mit IoT Central
Die IoT Central-Plattform bietet zwei Erweiterungsoptionen: Kontinuierlichen Datenexport (Continuous Data Export, CDE) und APIs. Die Kunden und Partner können zwischen diesen Optionen wählen, um ihre Lösungen an bestimmte Anforderungen anzupassen. Beispielsweise konfigurierte einer unserer Partner CDE mit Azure Data Lake Storage (ADLS). Er verwendet ADLS für die langfristige Datenaufbewahrung und andere Speicherszenarien mit kalten Pfaden, z.B. Batchverarbeitung, Überwachung und Berichterstellung. 

## <a name="next-steps"></a>Nächste Schritte

* Nachdem Sie sich nun mit der Architektur vertraut gemacht haben, [erstellen Sie die Intelligenter Stromzähler-App kostenlos](https://apps.azureiotcentral.com/build/new/smart-meter-monitoring).
* Weitere Informationen zu IoT Central finden Sie in der [Übersicht über IoT Central](https://docs.microsoft.com/azure/iot-central/).
