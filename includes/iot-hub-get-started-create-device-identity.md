---
title: include file
description: include file
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b2bce9788006a564def9bd8c1375a85dc4184b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814862"
---
## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität

In diesem Abschnitt verwenden Sie die Azure-Befehlszeilenschnittstelle zum Erstellen einer Geräteidentität für dieses Tutorial. Die Azure-Befehlszeilenschnittstelle ist in der [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vorinstalliert. Sie können [die Azure-Befehlszeilenschnittstelle aber auch lokal installieren](/cli/azure/install-azure-cli). Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

1. Führen Sie den folgenden Befehl in der Befehlszeilenumgebung aus, in der Sie die IoT-Erweiterung mit der Azure-Befehlszeilenschnittstelle installieren:

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. Wenn Sie die Azure-Befehlszeilenschnittstelle lokal ausführen, verwenden Sie den folgenden Befehl, um sich bei Ihrem Azure-Konto anzumelden. (Wenn Sie Cloud Shell verwenden, sind Sie automatisch angemeldet und müssen diesen Befehl nicht ausführen.)

    ```cmd/sh
    az login
    ```

1. Erstellen Sie abschließend die neue Geräteidentität `myDeviceId`, und rufen Sie die Verbindungszeichenfolge für das Gerät mit den folgenden Befehlen ab:

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Notieren Sie sich die Verbindungszeichenfolge für das Gerät aus dem Ergebnis. Diese Geräteverbindungszeichenfolge wird von der Geräte-App verwendet, um die Verbindung mit Ihrem IoT Hub als Gerät herzustellen.

<!-- images and links -->
