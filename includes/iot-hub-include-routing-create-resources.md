---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: c97c8231265cf87f52333a56d21d6fb13180c554
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72808833"
---
## <a name="create-base-resources"></a>Erstellen der grundlegenden Ressourcen

Bevor Sie das Nachrichtenrouting konfigurieren können, müssen Sie zunächst eine IoT Hub-Instanz, ein Speicherkonto und eine Service Bus-Warteschlange erstellen. Diese Ressourcen können anhand eines der vier Artikel erstellt werden, die für den ersten Teil dieses Tutorials verfügbar sind. In diesen Artikeln wird die Vorgehensweise mit dem Azure-Portal, mit einer Azure Resource Manager-Vorlage, der Azure CLI und mit Azure PowerShell erläutert.

Verwenden Sie für alle Ressourcen die gleiche Ressourcengruppe und den gleichen Speicherort. Am Ende dieses Tutorials können Sie dann sämtliche Ressourcen in einem einzelnen Schritt entfernen, indem Sie die Ressourcengruppe löschen.

Nachfolgend finden Sie eine Zusammenfassung der Schritte, die in den folgenden Abschnitten ausgeführt werden müssen: 

1. Erstellen Sie eine [Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md).

2. Erstellen Sie eine IoT Hub-Instanz im Tarif S1. Fügen Sie Ihrer IoT Hub-Instanz eine Consumergruppe hinzu. Die Consumergruppe wird beim Abrufen von Daten von Azure Stream Analytics verwendet.

   > [!NOTE]
   > Sie müssen eine IoT Hub-Instanz in einem kostenpflichtigen Tarif verwenden, um dieses Tutorial abzuschließen. Im Free-Tarif können Sie nur einen einzelnen Endpunkt einrichten, und dieses Tutorial erfordert mehrere Endpunkte.
   > 

3. Erstellen Sie ein Speicherkonto des Typs „Standard V1“ mit Standard_LRS-Replikation.

4. Erstellen Sie einen Service Bus-Namespace und eine Service Bus-Warteschlange.

5. Erstellen Sie eine Geräteidentität für das simulierte Gerät, das Nachrichten an Ihren Hub sendet. Speichern Sie den Schlüssel für die Testphase. (Wenn Sie eine Resource Manager-Vorlage erstellen, erfolgt dieser Schritt nach Bereitstellung der Vorlage.)