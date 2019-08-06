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
ms.openlocfilehash: 55dc712d323c43849fa3de23c83e29fc348a0f1e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403970"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

Gehen Sie wie folgt vor, um eine SAS-Richtlinie zu erstellen, die die Berechtigungen **Dienstverbindung** und **Lesevorgänge in Registrierung** gewährt, und eine Verbindungszeichenfolge für diese Richtlinie abzurufen:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren IoT-Hub. Wählen Sie dazu am besten **Ressourcengruppen**, anschließend die Ressourcengruppe, in der sich Ihr IoT-Hub befindet, und dann in der Ressourcenliste diesen IoT-Hub aus.

2. Wählen Sie im linken Bereich Ihres IoT-Hubs **Freigegebene Zugriffsrichtlinien** aus.

3. Wählen Sie im Menü über der Richtlinienliste die Option **Hinzufügen** aus.

4. Geben Sie im Bereich **Richtlinie für den gemeinsamen Zugriff hinzufügen** einen aussagekräftigen Namen für Ihre Richtlinie ein (beispielsweise *serviceAndRegistryRead*). Wählen Sie unter **Berechtigungen** die Berechtigungen **Lesevorgänge in Registrierung** und **Dienstverbindung** und anschließend die Option **Erstellen** aus.

    ![Hinzufügen einer neuen SAS-Richtlinie](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

5. Wählen Sie im Bereich **SAS-Richtlinien** in der Richtlinienliste Ihre neue Richtlinie aus.

6. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

Weitere Informationen zu SAS-Richtlinien und Berechtigungen für IoT-Hubs finden Sie unter [Access Control und Berechtigungen](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
