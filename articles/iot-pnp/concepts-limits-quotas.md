---
title: Grenzwerte und Kontingente in IoT Plug & Play (Vorschau) | Microsoft-Dokumentation
description: Informieren Sie sich über die Grenzwerte, Kontingente und Drosselungen, die in IoT Plug & Play (Vorschau) gelten.
author: miagdp
ms.author: miag
ms.date: 08/01/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 2df8a8820422a22b0512e24c4b052377cb0e61e0
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878926"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>IoT Plug & Play (Vorschau): Grenzwerte, Kontingente und Drosselungen

In diesem Artikel werden die IoT Plug & Play-spezifischen Grenzwerte, Kontingente und Drosselungen erläutert, die in der öffentlichen Vorschau gelten. Einige vorhandene [IoT Hub-Kontingente und Drosselungen](../iot-hub/iot-hub-devguide-quotas-throttling.md) werden ebenfalls angewendet.

## <a name="iot-hub"></a>IoT Hub

In der öffentlichen Vorschau gelten die folgenden Grenzwerte und Kontingente für einen IoT-Hub:

| Grenzwerte, Einschränkungen und Drosselungen | Wert | Notizen |
|-----|-----|-----|
| Anzahl von Gerätefunktionsmodellen (Device Capability Models, DCMs) oder Schnittstellen, die pro Hub registriert werden können | 1500 ||
| Maximale Anzahl von Schnittstellen, die pro Gerät registriert werden können | 40 ||
| Maximale Anzahl von DCMs, die pro Gerät registriert werden können | 1 ||
| Maximale Größe der Schnittstelle bzw. DCM-Datei | 512 Zeichen ||
| Maximale Größe eines Schnittstellennamens | 256 Zeichen ||
| Maximale Größe eines Eigenschaftsnamens  | 64 Byte, 7 Ebenen Tiefe (die erste Ebene ist für `$iotin` reserviert) | Zulässige Zeichen: a-z, A-Z, 0-9 (nicht als erstes Zeichen) und Unterstrich |
| Maximale Größe eines Eigenschaftswerts | 512 Bytes ||
| Maximale Größe eines Befehlsnamens | 100 Bytes ||
| Größe des Gerätezwillings | Identisch mit [IoT Hub-Grenzwerten](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) ||
| Aufrufe der Auflösungs-API in der gesamten SKU (unabhängig von den Einheiten) | 100 Anforderungen/Sekunde ||

## <a name="model-repository"></a>Modellrepository

In der öffentlichen Vorschau gelten die folgenden Grenzwerte und Kontingente für ein Modellrepository:

| Grenzwerte, Einschränkungen und Drosselungen| Wert |
|-----|-----|
| Anzahl von Modellrepositorys eines Unternehmens pro Azure Active Directory-Mandant | 1 |
| Anzahl von Autorisierungsschlüsseln pro Modellrepository | 10  |
| Anzahl von Modellen (DCMs oder Schnittstellen) pro Unternehmensmodellrepository| 1500  |
| Anzahl von Modellen (DCMs oder Schnittstellen) im öffentlichen Modellrepository pro Azure Active Directory-Mandant| 1500  |
| Anzahl von DCMs oder Schnittstellen, die in einem Unternehmensmodellrepository gelöscht werden | 10 Abfragen pro Sekunde (Queries Per Second, QPS)|
| Anzahl von Modellrepositorys, die von einem Mandanten erstellt oder aktualisiert werden| 1 QPS |
| Anzahl von Autorisierungsschlüsseln, die in einem Modellrepository erstellt, aktualisiert oder gelöscht werden | 1 QPS|
| Anzahl von DCMs, die in einem Unternehmensmodellrepository erstellt werden | 10 QPS |
| Anzahl von Schnittstellen, die in einem Unternehmensmodellrepository erstellt werden | 10 QPS|
| Anzahl von DCMs, die im öffentlichen Modellrepository erstellt werden | 10 QPS|
| Anzahl von Schnittstellen, die im öffentlichen Modellrepository erstellt werden | 10 QPS|

## <a name="parser-library"></a>Parserbibliothek

Für die Parserbibliothek gelten die Grenzwerte der [Digital Twin Definition Language](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL).

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes empfehlen wir, sich darüber zu informieren, wie Sie eine [Verbindung mit einem IoT Plug & Play-Gerät herstellen und damit interagieren](./howto-develop-solution.md).
