---
title: Allgemeine Schnittstellen – IoT Plug & Play (Vorschau) | Microsoft-Dokumentation
description: Beschreibung der allgemeinen Schnittstellen für IoT Plug & Play-Entwickler
author: ChrisGMsft
ms.author: chrisgre
ms.date: 09/08/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 2eae778230fa5fce1be095106a02b2b643ff436e
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935321"
---
# <a name="iot-plug-and-play-preview-common-interfaces"></a>IoT Plug & Play (Vorschau): allgemeine Schnittstellen

Alle IoT Plug & Play-Geräte sollten einige allgemeine Schnittstellen implementieren. Allgemeine Schnittstellen sind von Vorteil für IoT-Lösungen, weil sie eine konsistente Funktionalität bereitstellen. Die [Zertifizierung](tutorial-build-device-certification.md) erfordert, dass Ihr Gerät einige allgemeine Schnittstellen implementiert. Sie können die allgemeinen Schnittstellendefinitionen aus dem öffentlichen Modellrepository abrufen.

## <a name="summary-of-common-interfaces"></a>Zusammenfassung der allgemeinen Schnittstellen

| NAME | id | BESCHREIBUNG | Implementiert durch Azure IoT SDK | Muss in einem Funktionsmodell deklariert sein |
| -------- | -------- | -------- | -------- | -------- | -------- |
| Modellinformationen | urn:azureiot:ModelDiscovery:ModelInformation:1 | Hiermit deklarieren Geräte Funktionsmodell-ID und Schnittstellen. Für alle IoT Plug & Play-Geräte erforderlich. | Ja | Nein |
| Informationen zum Client-SDK des digitalen Zwillings | urn:azureiot:Client:SDKInformation:1 | Client-SDK zum Verbinden des Geräts mit Azure. Ist für die [Zertifizierung](tutorial-build-device-certification.md) erforderlich. | Ja | Nein |
| Geräteinformationen | urn:azureiot:DeviceManagement:DeviceInformation:1 | Hardware- und Betriebssysteminformationen über das Gerät. Ist für die [Zertifizierung](tutorial-build-device-certification.md) erforderlich. | Nein | Ja |
| Modelldefinition | urn:azureiot:ModelDiscovery:ModelDefinition:1 | Hiermit deklarieren Geräte die vollständige Definition für ihr Funktionsmodell und ihre Schnittstellen. Muss implementiert werden, wenn Modelldefinitionen nicht in einem Modellrepository gehostet werden. | Nein | Ja |
| Digitaler Zwilling | urn:azureiot:ModelDiscovery:DigitalTwin:1 | Hiermit rufen Lösungsentwickler die Funktionsmodell-ID und die Schnittstellen-IDs für einen digitalen Zwilling ab. Diese Schnittstelle wird nicht durch ein IoT Plug & Play-Gerät deklariert oder implementiert. | Nein | Nein |

- „Implementiert durch Azure IoT SDK“: Hiermit wird angegeben, ob das Azure IoT SDK die in den Schnittstellen deklarierten Funktionen implementiert. IoT Plug & Play-Geräte, die das Azure IoT SDK verwenden, müssen diese Schnittstelle nicht implementieren.
- „Muss in einem Funktionsmodell deklariert sein“: Wenn hier „Ja“ angegeben ist, muss diese Schnittstelle im Abschnitt `"implements":` des Gerätefunktionsmodells für dieses IoT Plug & Play-Gerät deklariert sein.

## <a name="retrieve-interface-definitions-from-the-public-repository"></a>Abrufen von Schnittstellendefinitionen aus dem öffentlichen Repository

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Sie können die Azure-IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle verwenden, um die allgemeinen Schnittstellen aus dem öffentlichen Modellrepository abzurufen.

```cmd/sh
az iot pnp interface show --interface {InterfaceID}
```

```cmd/sh
az iot pnp capability-model show --model {ModelID}
```

### <a name="vs-code"></a>VS-Code

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Modellrepository öffnen** aus. Wählen Sie **Public repository** (Öffentliches Repository) aus. Das öffentliche Modellrepository wird in VS Code geöffnet.

1. Geben Sie den Schnittstellennamen im Suchfeld des öffentlichen Modellrepositorys ein.

1. Um eine lokale Kopie der Schnittstelle zu erstellen, wählen Sie die Schnittstelle in den Suchergebnissen aus, und klicken Sie dann auf **Download**.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit allgemeinen Schnittstellen vertraut gemacht haben, finden Sie weitere Informationen in folgenden Artikeln:

- [Digital Twin Definition Language (DTDL)](https://aka.ms/DTDL)
- [C-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT-REST-API](https://docs.microsoft.com/rest/api/iothub/device)
