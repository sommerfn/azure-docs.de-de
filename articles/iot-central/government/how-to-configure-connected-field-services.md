---
title: Verbinden Ihrer Azure IoT Central-Anwendung mit Dynamics 365 for Field Service | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit Azure IoT Central und Dynamics 365 for Field Service eine End-to-End-Lösung erstellen.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 957e9337bf8ea5941b140ba4f3266417d36df6a7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498772"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Erstellen einer End-to-End-Lösung mit Azure IoT Central und Dynamics 365 for Field Service 

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Als Entwickler können Sie die Integration Ihrer Azure IoT Central-Anwendung in andere Geschäftssysteme aktivieren. 


Bei einer Lösung für die vernetzte Abfallwirtschaft können Sie die Aussendung von Müllfahrzeugen basierend auf den IoT-Sensordaten der vernetzten Mülleimer optimieren. In Ihrer [IoT Central-Anwendung für die vernetzte Abfallwirtschaft](./tutorial-connected-waste-management.md) können Sie Regeln und Aktionen konfigurieren und so festlegen, dass in Dynamics 365 for Field Service die Erstellung von Warnungen ausgelöst wird. Dies wird mit Microsoft Flow erreicht. Sie können diese Anwendung direkt in IoT Central konfigurieren, um Workflows für Anwendungen und Dienste zu automatisieren. Außerdem können Informationen basierend auf Dienstaktivitäten in Field Service zurück an Azure IoT Central gesendet werden. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Verbinden Ihrer Azure IoT Central-Anwendung mit Dynamics 365 for Field Service 

Die folgenden End-to-End-Integrationsprozesse können über eine reine Konfigurationsumgebung leicht implementiert werden:
* Azure IoT Central kann Informationen zu Geräteanomalien zur Diagnose an Connected Field Service senden (als IoT-Warnung).
* Mit Connected Field Service können Vorfälle oder Arbeitsaufträge erstellt werden, die über Geräteanomalien ausgelöst werden.
* Connected Field Service kann auch genutzt werden, um Techniker für die Inspektion einzuplanen, damit Ausfallzeiten verhindert werden.
* Das Gerätedashboard von Azure IoT Central kann mit relevanten Dienst- und Zeitplaninformationen aktualisiert werden.


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [IoT Central-Vorlagen für Behörden](./overview-iot-central-government.md).
* Informieren Sie sich über [IoT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central).
* Informieren Sie sich über [Dynamics 365 for Field Service](https://docs.microsoft.com/dynamics365/field-service/cfs-iot-overview).
