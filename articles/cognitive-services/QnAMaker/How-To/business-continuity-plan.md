---
title: 'Geschäftskontinuitätsplan: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 0e748e81de39b2bef14b543063adeb51b8b3ecdb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73486710"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst

Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.

![QnA Maker-Geschäftskontinuitätsplan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Das oben dargestellte allgemeine Konzept lautet wie folgt:

1. Richten Sie zwei parallele [QnA Maker-Dienste](../How-To/set-up-qnamaker-service-azure.md) in [Azure-Regionspaaren](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) ein.

2. Halten Sie die primären und sekundären Azure-Suchindizes synchron. Verwenden Sie das [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) aufgeführte GitHub-Beispiel, um zu erfahren, wie Azure-Indizes gesichert und wiederhergestellt werden.

3. Sichern Sie die Application Insights-Daten mithilfe des [fortlaufenden Exports](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Verwenden Sie [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) nach der Einrichtung der primären und sekundären Stapel, um die zwei Endpunkte zu konfigurieren und eine Routingmethode einzurichten.

5. Sie müssen ein SSL-Zertifikat für Ihren Traffic Manager-Endpunkt erstellen. [Binden Sie das SSL-Zertifikat](https://docs.microsoft.com/azure/app-service/configure-ssl-bindings) an Ihre App-Dienste.

6. Verwenden Sie dann den Traffic Manager-Endpunkt in Ihrem Bot oder in Ihrer App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen der Kapazität für Ihre QnA Maker-Bereitstellung](../Tutorials/choosing-capacity-qnamaker-deployment.md)
