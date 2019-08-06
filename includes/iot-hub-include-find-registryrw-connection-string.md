---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a752427d1e5873f0a27fd231872e3a13b234d9ed
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402333"
---
<!-- This tells how to get the connection string for the registryReadWrite shared access policy of your IoT hub -->

Führen Sie zum Abrufen der IoT-Hub-Verbindungszeichenfolge für die Richtlinie **registryReadWrite** die folgenden Schritte aus:

1. Öffnen Sie Ihren IoT-Hub im [Azure-Portal](https://portal.azure.com).  Wählen Sie dazu am besten **Ressourcengruppen**, anschließend die Ressourcengruppe, in der sich Ihr IoT-Hub befindet, und dann in der Ressourcenliste diesen IoT-Hub aus.

2. Wählen Sie im linken Bereich Ihres IoT-Hubs **Freigegebene Zugriffsrichtlinien** aus.

3. Wählen Sie in der Liste der Richtlinien die Richtlinie **registryRead Write** aus.

4. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-registryrw-connection-string/iot-hub-get-connection-string.png)

Weitere Informationen zu freigegebenen Zugriffsrichtlinien und Berechtigungen für IoT-Hub finden Sie unter [Zugriffssteuerung und Berechtigungen.](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)
