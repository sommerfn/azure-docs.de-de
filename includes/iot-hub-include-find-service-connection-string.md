---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/16/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 55cdd864d73ce084d994c64233e79d5a58b17def
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558445"
---
<!-- This tells how to get the connection string for the service shared access policy of your IoT hub -->

Führen Sie zum Abrufen der IoT-Hub-Verbindungszeichenfolge für die Richtlinie **service** die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressourcengruppen** aus. Wählen Sie die Ressourcengruppe aus, in der sich der Hub befindet, und wählen Sie dann in der Liste der Ressourcen Ihren Hub aus.

1. Wählen Sie im linken Bereich Ihres IoT-Hubs **Freigegebene Zugriffsrichtlinien** aus.

1. Wählen Sie in der Liste der Richtlinien die Richtlinie **service** aus.

1. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-service-connection-string/iot-hub-get-connection-string.png)

Weitere Informationen zu SAS-Richtlinien und Berechtigungen für IoT-Hubs finden Sie unter [Access Control und Berechtigungen](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
