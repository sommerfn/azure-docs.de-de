---
title: include file
description: include file
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: f88f6aa6daec342b24d165c4c4cc0a89a0805f7a
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558475"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Zuweisen eines Azure Storage-Kontos zu IoT Hub

Da von der simulierten Geräte-App eine Datei in ein Blob hochgeladen wird, müssen Sie über ein [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md)-Konto verfügen, das Ihrem IoT Hub zugeordnet ist. Wenn Sie Azure Storage-Konto einem IoT Hub zuordnen, generiert der IoT Hub einen SAS-URI. Ein Gerät kann diesen SAS-URI für das sichere Hochladen einer Datei zu einem Blobcontainer verwenden. Der IoT Hub-Dienst und die Geräte-SDKs koordinieren den Prozess, der die SAS-URI generiert und für ein Gerät verfügbar macht, sodass er beim Hochladen einer Datei verwendet werden kann.

Folgen Sie den Anleitungen in [Konfigurieren des Dateiuploads im Azure-Portal](../articles/iot-hub/iot-hub-configure-file-upload.md). Stellen Sie sicher, dass Ihrem IoT Hub ein Blobcontainer zugeordnet ist und die Dateibenachrichtigungen aktiviert sind.

![Aktivieren von Dateibenachrichtigungen im Portal](./media/iot-hub-associate-storage/file-notifications-vs2019.png)
