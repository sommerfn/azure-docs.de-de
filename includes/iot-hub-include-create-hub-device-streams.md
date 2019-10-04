---
title: Includedatei (Vorschau für Gerätestreams)
description: Includedatei (Vorschau für Gerätestreams)
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/20/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 46f8e59713896cd94b96de62d982072119c32513
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050183"
---
In diesem Abschnitt wird beschrieben, wie Sie über das [Azure-Portal](https://portal.azure.com) einen IoT-Hub erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Ressource erstellen** aus, und geben Sie dann ins Feld **Marketplace durchsuchen** den Begriff *IoT Hub* ein.

1. Wählen Sie in den Suchergebnissen **IoT Hub** und dann **Erstellen** aus.

1. Füllen Sie auf der Registerkarte **Grundlegende Einstellungen** die Felder wie folgt aus:

   - **Abonnement**: Wählen Sie das Abonnement aus, das Sie für Ihren Hub verwenden möchten.

   - **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue. Wählen Sie zum Erstellen einer neuen Ressourcengruppe die Option **Neu erstellen** aus, und geben Sie den Namen ein, den Sie verwenden möchten. Wenn Sie eine vorhandene Ressourcengruppe verwenden möchten, wählen Sie die entsprechende Ressourcengruppe aus. Weitere Informationen finden Sie unter [Verwalten von Azure Resource Manager-Ressourcengruppen](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   - **Region**: Wählen Sie die Region aus, in der sich Ihr Hub befinden soll. Wählen Sie eine Region aus, die die IoT Hub-Gerätestreamsvorschau unterstützt – entweder **USA, Mitte** oder **USA, Mitte (EUAP)** .

   - **IoT Hub-Name**: Geben Sie einen Namen für den Hub ein. Dieser Name muss global eindeutig sein. Wenn der eingegebene Name verfügbar ist, wird ein grünes Häkchen angezeigt.

   ![Erstellen eines IoT-Hubs im Azure-Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-device-streams.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

1. Klicken Sie auf **Weiter: Größe und Skalierung**, um die Erstellung Ihres Hubs fortzusetzen.

   ![Festlegen der Größe und Skalierung für einen neuen IoT-Hub über das Azure-Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   Unter **Größe und Skalierung** können Sie die Standardeinstellungen übernehmen und am unteren Rand **Überprüfen + erstellen** auswählen. Betrachten Sie die folgenden Optionen:

   - **Tarif und Skalierung**: Ihre ausgewählte Ebene. Wählen Sie einen der Standardtarife (**S1**, **S2** oder **S3**) oder **F1: Free-Tarif** aus. Berücksichtigen Sie bei dieser Entscheidung ggf. auch die Anzahl Ihrer Geräte sowie die zu erwartenden streamingfremden Workloads für Ihren Hub, etwa Telemetrienachrichten. Der kostenlose Tarif ist beispielsweise für Tests und Evaluierungen vorgesehen. Damit kann für 500 Geräte eine Verbindung mit dem IoT Hub hergestellt werden, und bis zu 8.000 Nachrichten pro Tag sind möglich. Jedes Azure-Abonnement kann einen IoT-Hub im kostenlosen Tarif erstellen. 

   - **Anzahl von IoT Hub-Einheiten**: Die Anzahl der pro Einheit und Tag zulässigen Nachrichten hängt von Ihrem Hubtarif ab. Diese Option hängt von der streamingfremden Workload ab, die Sie für Ihren Hub erwarten. Wählen Sie vorerst „1“ aus.

   - **Erweiterte Einstellungen** > **Gerät-zu-Cloud-Partitionen**: Diese Eigenschaft setzt die Gerät-zu-Cloud-Nachrichten in Relation zur Anzahl von gleichzeitigen Lesern der Nachrichten. Die meisten Hubs benötigen nur vier Partitionen.

   Weitere Informationen zu den Tarifoptionen finden Sie unter [Wählen des richtigen IoT Hub-Tarifs für Ihre Lösung](../articles/iot-hub/iot-hub-scaling.md).

1. Wählen Sie **Überprüfen + erstellen** aus, um Ihre Auswahl zu überprüfen. Das Ergebnis sieht etwa wie folgt aus:

   ![Informationen zum Erstellen des neuen IoT-Hubs](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

1. Wählen Sie **Erstellen** aus, um Ihren neuen IoT-Hub zu erstellen. Der Vorgang dauert einige Minuten.
