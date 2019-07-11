---
title: Konfigurieren Ihrer Azure Security Center für IoT-Lösung (Vorschauversion) | Microsoft-Dokumentation
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
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 7f90dba899651b677740e9ceb88bdd579ebb073c
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616640"
---
# <a name="quickstart-configure-your-iot-solution"></a>Schnellstart: Konfigurieren Ihrer IoT-Lösung

> [!IMPORTANT]
> Azure Security Center für IoT befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mithilfe von ASC für IoT erläutert. 

## <a name="azure-security-center-asc-for-iot"></a>Azure Security Center (ASC) für IoT

ASC für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit ASC für IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

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
5. Klicken Sie auf **Hinzufügen**. 

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressource hinzugefügt.

ASC für IoT überwacht nun die neu hinzugefügten Ressourcen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Sicherheitsmodule erstellen:

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)