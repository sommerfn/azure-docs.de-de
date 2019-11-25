---
title: 'Schnellstart: Erstellen eines Sicherheitsmodulzwillings für Azure Security Center für IoT'
description: In diesem Schnellstart wird beschrieben, wie Sie einen Azure Security Center für IoT-Modulzwilling zur Verwendung mit Azure Security Center für IoT erstellen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: c782692e-1284-4c54-9d76-567bc13787cc
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: b362130c2b717f813a6332f81a3c8179bea4166a
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904166"
---
# <a name="quickstart-create-an-azureiotsecurity-module-twin"></a>Schnellstart: Erstellen eines Azure IoT-Sicherheitsmodulzwillings

In dieser Schnellstartanleitung wird beschrieben, wie Sie individuelle _azureiotsecurity_-Modulzwillinge für neue Geräte oder im Batchmodus Modulzwillinge für alle Geräte einer IoT Hub-Instanz erstellen.  

## <a name="understanding-azureiotsecurity-module-twins"></a>Grundlegendes zu azureiotsecurity-Modulzwillingen 

Für in Azure erstellte IoT-Lösungen spielen Gerätezwillinge eine wichtige Rolle – sowohl bei der Geräteverwaltung als auch bei der Prozessautomatisierung. 

Azure Security Center für IoT ermöglicht die vollständige Integration in Ihre vorhandene Plattform für die IoT-Geräteverwaltung. Auf diese Weise können Sie Ihren Gerätesicherheitsstatus verwalten und die vorhandenen Funktionen für die Gerätesteuerung nutzen.
Die Integration von Azure Security Center für IoT wird mithilfe des Mechanismus für IoT Hub-Zwillinge erreicht.  

Weitere Informationen zum allgemeinen Konzept in Bezug auf Modulzwillinge in Azure IoT Hub finden Sie unter [Verstehen und Verwenden von Modulzwillingen in IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins). 
 
Unter Azure Security Center für IoT wird der Mechanismus für Modulzwillinge genutzt und für jedes Gerät ein Sicherheitsmodulzwilling mit dem Namen _azureiotsecurity_ eingesetzt.

Der Sicherheitsmodulzwilling enthält alle Informationen, die für die Gerätesicherheit Ihrer einzelnen Geräte jeweils relevant sind. 
 
Zur vollständigen Nutzung der Features von Azure Security Center für IoT müssen Sie diese Sicherheitsmodulzwillinge für jedes Gerät des Diensts erstellen, konfigurieren und verwenden.  

## <a name="create-azureiotsecurity-module-twin"></a>Erstellen eines azureiotsecurity-Modulzwillings 

_azureiotsecurity_-Modulzwillinge können auf zwei Arten erstellt werden:
1. [Modulbatchskript](https://aka.ms/iot-security-github-create-module): Erstellt automatisch einen Modulzwilling für neue Geräte bzw. Geräte ohne Modulzwilling, indem die Standardkonfiguration verwendet wird.
2. Jeder Modulzwilling wird manuell individuell bearbeitet, und auf jedes Gerät werden spezifische Konfigurationen angewendet.

>[!NOTE] 
> Beim Verwenden der Batchmethode werden vorhandene azureiotsecurity-Modulzwillinge nicht überschrieben. Wenn NUR die Batchmethode verwendet wird, werden neue Modulzwillinge für Geräte erstellt, die nicht bereits über einen Sicherheitsmodulzwilling verfügen. 

Informationen zum Modifizieren oder Ändern der Konfiguration eines vorhandenen Modulzwillings finden Sie unter [Agent-Konfiguration](how-to-agent-configuration.md). 

Verwenden Sie die folgende Anleitung, um für ein Gerät manuell einen neuen _azureiotsecurity_-Modulzwilling zu erstellen: 

1. Suchen Sie in Ihrer IoT Hub-Instanz nach dem Gerät, für das Sie einen Sicherheitsmodulzwilling erstellen möchten, und wählen Sie es aus.
1. Klicken Sie auf Ihr Gerät und dann auf **Modulidentität hinzufügen**.
1. Geben Sie im Feld **Name der Modulidentität** den Namen **azureiotsecurity** ein.

1. Klicken Sie auf **Speichern**. 

## <a name="verify-creation-of-a-module-twin"></a>Überprüfen der Erstellung eines Modulzwillings

Überprüfen Sie wie folgt, ob für ein bestimmtes Gerät ein Sicherheitsmodulzwilling vorhanden ist:

1. Wählen Sie auf Ihrer Azure IoT Hub-Instanz im Menü **Explorer** die Option **IoT-Geräte**.    
1. Geben Sie die Geräte-ID ein, oder wählen Sie im Feld **Gerät abfragen** eine Option aus, und klicken Sie auf **Geräte abfragen**. 
    ![Geräte abfragen](./media/quickstart/verify-security-module-twin.png)
1. Wählen Sie das Gerät aus, oder doppelklicken Sie darauf, um die Seite „Gerätedetails“ zu öffnen. 
1. Wählen Sie das Menü **Modulidentitäten** aus, und bestätigen Sie das Vorhandensein des Moduls **azureiotsecurity** in der Liste mit den Modulidentitäten, die dem Gerät zugeordnet sind. 
    ![Einem Gerät zugeordnete Module](./media/quickstart/verify-security-module-twin-3.png)


Weitere Informationen zum Anpassen der Eigenschaften von Modulzwillingen in Azure Security Center für IoT finden Sie unter [Agent-Konfiguration](how-to-agent-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um sich über das Konfigurieren von benutzerdefinierten Warnungen zu informieren.

> [!div class="nextstepaction"]
> [Konfigurieren von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md)
