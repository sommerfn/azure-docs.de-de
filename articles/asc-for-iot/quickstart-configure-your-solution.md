---
title: Konfigurieren Ihrer Azure Security Center für IoT-Lösung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ihre End-to-End-IoT-Lösung mithilfe von Azure Security Center für IoT konfigurieren.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2019
ms.author: mlottner
ms.openlocfilehash: a546d153c6fe4f14ccc8c21308bd4a33385870c3
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299691"
---
# <a name="quickstart-configure-your-iot-solution"></a>Schnellstart: Konfigurieren Ihrer IoT-Lösung

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mithilfe von Azure Security Center für IoT erläutert. 

## <a name="azure-security-center-for-iot"></a>Azure Security Center für IoT

Azure Security Center für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit Azure Security Center für IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

Nach der Aktivierung in Ihrer IoT Hub-Instanz erkennt Azure Security Center für IoT automatisch andere Azure-Dienste, die ebenfalls mit Ihrer IoT Hub-Instanz verbunden sind und mit Ihrer IoT-Lösung zusammenhängen.

Zusätzlich zur automatischen Beziehungserkennung können Sie auch weitere Azure-Ressourcengruppen als Teil Ihrer IoT-Lösung kennzeichnen. 

Dadurch können Sie vollständige Abonnements, Ressourcengruppen oder auch einzelne Ressourcen hinzufügen. 

Nachdem Sie alle Ressourcenbeziehungen definiert haben, stellt Azure Security Center für IoT mithilfe von Azure Security Center Sicherheitsempfehlungen und Warnungen für diese Ressourcen bereit.

## <a name="add-azure-resources-to-your-iot-solution"></a>Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung

Gehen Sie wie folgt vor, um Ihrer IoT-Lösung neue Ressourcen hinzuzufügen: 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**. 
1. Öffnen Sie **Ressourcen** (im linken Menü unter **Sicherheit**). 
1. Wählen Sie **Bearbeiten** und anschließend die Ressourcengruppen aus, die zu Ihrer IoT-Lösung gehören.
1. Klicken Sie auf **Hinzufügen**. 

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressourcengruppe hinzugefügt.

Azure Security Center für IoT überwacht nun die neu hinzugefügten Ressourcengruppen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Sicherheitsmodule erstellen:

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)