---
title: 'Geschäftskontinuitätsplan: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: f9892acb387a655e173ee5d2bde28e7346a6c535
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447532"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst

Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.

![QnA Maker-Geschäftskontinuitätsplan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Das oben dargestellte allgemeine Konzept lautet wie folgt:

1. Richten Sie zwei parallele [QnA Maker-Dienste](../How-To/set-up-qnamaker-service-azure.md) in [Azure-Regionspaaren](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) ein.

2. Halten Sie die primären und sekundären Azure-Suchindizes synchron. Verwenden Sie das [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) aufgeführte GitHub-Beispiel, um zu erfahren, wie Azure-Indizes gesichert und wiederhergestellt werden.

3. Sichern Sie die Application Insights-Daten mithilfe des [fortlaufenden Exports](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Verwenden Sie [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) nach der Einrichtung der primären und sekundären Stapel, um die zwei Endpunkte zu konfigurieren und eine Routingmethode einzurichten.

5. Sie müssen ein SSL-Zertifikat für Ihren Traffic Manager-Endpunkt erstellen. [Binden Sie das SSL-Zertifikat](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) an Ihre App-Dienste.

6. Verwenden Sie dann den Traffic Manager-Endpunkt in Ihrem Bot oder in Ihrer App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen der Kapazität für Ihre QnA Maker-Bereitstellung](../Tutorials/choosing-capacity-qnamaker-deployment.md)
