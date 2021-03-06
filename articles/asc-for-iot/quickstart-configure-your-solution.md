---
title: Konfigurieren Ihrer ASC für IoT-Lösung (Vorschauversion) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ihre End-to-End-IoT-Lösung mithilfe von ASC für IoT konfigurieren.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3ba8b1a099dc204730813754fdfc4b0b8ce45303
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541730"
---
# <a name="quickstart-configure-your-iot-solution"></a>Schnellstart: Konfigurieren Ihrer IoT-Lösung

> [!IMPORTANT]
> ASC für IoT befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mithilfe von ASC für IoT erläutert. 

## <a name="asc-for-iot"></a>ASC für IoT

ASC für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit ASC for IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

Nach der Aktivierung in Ihrer IoT Hub-Instanz erkennt ASC für IoT automatisch andere Azure-Dienste, die ebenfalls mit Ihrem IoT-Hub verbunden sind und mit Ihrer IoT-Lösung zusammenhängen.

Zusätzlich zur automatischen Beziehungserkennung können Sie auch weitere Azure-Ressourcen als Teil Ihrer IoT-Lösung kennzeichnen.
Dadurch können Sie vollständige Abonnements, Ressourcengruppen oder auch einzelne Ressourcen hinzufügen.

Nachdem Sie alle Ressourcenbeziehungen definiert haben, stellt ASC für IoT mithilfe von Azure Security Center Sicherheitsempfehlungen und Warnungen für diese Ressourcen bereit.

## <a name="add-azure-resources-to-your-iot-solution"></a>Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung

Gehen Sie wie folgt vor, um Ihrer IoT-Lösung neue Ressourcen hinzuzufügen: 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**. 
2. Öffnen Sie **Ressourcen** (im linken Menü unter **Sicherheit**). 
3. Wählen Sie **Ressourcen hinzufügen** aus.
4. Wählen Sie Ressourcen aus, die zu Ihrer IoT-Lösung gehören.
4. Klicken Sie auf **Hinzufügen**. 

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressource hinzugefügt.

ASC für IoT überwacht nun die neu hinzugefügten Ressourcen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Sicherheitsmodule erstellen:

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)