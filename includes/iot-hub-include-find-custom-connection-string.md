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
ms.openlocfilehash: 6f43bbcd83861f7d39de2aa89bbe035c2ff5b809
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050433"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

Gehen Sie wie folgt vor, um eine SAS-Richtlinie zu erstellen, die die Berechtigungen **Dienstverbindung** und **Lesevorgänge in Registrierung** gewährt, und eine Verbindungszeichenfolge für diese Richtlinie abzurufen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressourcengruppen** aus. Wählen Sie die Ressourcengruppe aus, in der sich der Hub befindet, und wählen Sie dann in der Liste der Ressourcen Ihren Hub aus.

1. Wählen Sie im linken Bereich Ihres Hubs **SAS-Richtlinien** aus.

1. Wählen Sie im Menü über der Richtlinienliste die Option **Hinzufügen** aus.

1. Geben Sie unter **Richtlinie für den gemeinsamen Zugriff hinzufügen** einen aussagekräftigen Namen für Ihre Richtlinie ein (z.B. *serviceAndRegistryRead*). Wählen Sie unter **Berechtigungen** die Berechtigungen **Lesevorgänge in Registrierung** und **Dienstverbindung** und anschließend die Option **Erstellen** aus.

    ![Hinzufügen einer neuen SAS-Richtlinie](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

1. Wählen Sie Ihre neue Richtlinie aus der Liste der Richtlinien aus.

1. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

Weitere Informationen zu SAS-Richtlinien und Berechtigungen für IoT-Hubs finden Sie unter [Access Control und Berechtigungen](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
