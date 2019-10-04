---
title: Importieren eines Edge-Pakets in die Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ein IoT Edge-Paket in den Solution Accelerator für die Remoteüberwachung importieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/10/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "61442938"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Importieren eines IoT Edge-Pakets in den Solution Accelerator für die Remoteüberwachung

Ein Bereitstellungsmanifest definiert die Module, die auf einem IoT Edge-Gerät bereitgestellt werden sollen. In diesem Artikel wird vorausgesetzt, dass ein Entwickler in Ihrer Organisation das Bereitstellungsmanifest bereits erstellt hat. Um zu erfahren, wie Entwickler ein Manifest erstellen, lesen Sie eine der folgenden IoT Edge-Anleitungen:

- [Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal](../iot-edge/how-to-deploy-modules-portal.md)
- [Bereitstellen von Azure IoT Edge-Modulen mit der Azure CLI](../iot-edge/how-to-deploy-modules-cli.md)
- [Bereitstellen von Azure IoT Edge-Modulen mithilfe von Visual Studio Code](../iot-edge/how-to-deploy-modules-vscode.md)

Ein Entwickler erstellt und testet ein Bereitstellungsmanifest in einer Entwicklungsumgebung. Wenn Sie bereit sind, können Sie das Manifest in den Solution Accelerator für die Remoteüberwachung importieren.

## <a name="export-a-manifest"></a>Exportieren eines Manifests

Verwenden Sie das Azure-Portal, um das Bereitstellungsmanifest aus der Entwicklungsumgebung zu exportieren:

1. Navigieren Sie im Azure-Portal zu dem IoT-Hub, den Sie zum Entwickeln und Testen Ihre IoT Edge-Geräte verwenden. Klicken Sie auf **IoT Edge** und dann auf **IoT Edge-Bereitstellungen**: ![IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png)

1. Klicken Sie auf die Bereitstellung mit der gewünschten Bereitstellungskonfiguration. Die Seite **Bereitstellungsdetails** wird angezeigt: ![Details einer IoT Edge-Bereitstellung](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. Klicken Sie auf **IoT Edge-Manifest herunterladen**:  ![Bereitstellungsmanifest herunterladen](media/iot-accelerators-remote-monitoring-import-edge-package/download.png)

1. Speichern Sie die JSON-Datei als lokale Datei namens **deploymentmanifest.json**.

Sie haben jetzt eine Datei, die das Bereitstellungsmanifest enthält. Im nächsten Abschnitt importieren Sie dieses Manifest als Paket in die Remoteüberwachungslösung.

## <a name="import-a-package"></a>Importieren eines Pakets

Gehen Sie folgendermaßen vor, um ein Edge-Bereitstellungsmanifest als Paket in Ihre Lösung zu importieren:

1. Navigieren Sie auf der Webbenutzeroberfläche für die Remoteüberwachung zur Seite **Pakete**:  ![Seite „Pakete“](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. Klicken Sie auf **+ Neues Paket**, wählen Sie **Edge-Manifest** als Pakettyp aus, und klicken Sie auf **Durchsuchen**, um die im vorherigen Abschnitt gespeicherte Datei **deploymentmanifest.json** auszuwählen:  ![Manifest auswählen](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Klicken Sie auf **Hochladen**, um das Paket Ihrer Remoteüberwachungslösung hinzuzufügen:  ![Hochgeladenes Paket](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

Sie haben nun ein IoT Edge-Bereitstellungsmanifest als Paket hochgeladen. Auf der Seite **Bereitstellungen** können Sie dieses Paket für Ihre verbundenen IoT Edge-Geräte bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit der Bereitstellung von Modulen auf einem IoT Edge-Gerät über die Remoteüberwachungslösung vertraut gemacht haben, erfahren Sie im nächsten Schritt mehr über [IoT Edge](../iot-edge/about-iot-edge.md).
