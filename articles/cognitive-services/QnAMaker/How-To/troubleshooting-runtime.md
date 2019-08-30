---
title: 'Problembehandlung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker besteht aus Komponenten, die im Azure-Konto des Benutzers gehostet werden. Für das Debuggen kann es notwendig sein, dass Benutzer ihre Azure-Ressourcen für QnA Maker bearbeiten oder dem QnA Maker-Supportteam zusätzliche Informationen zu ihrer Installation bereitstellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b58a08c4cfa97356be4064456b2e9ec5cceccfbc
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876343"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Tipps zur Problembehandlung zur Unterstützung von QnA Maker-Dienst und -Runtime

QnA Maker umfasst Ressourcen, die im Azure-Abonnement des Benutzers gehostet werden. Für das Debuggen kann es notwendig sein, dass Benutzer ihre Azure-Ressourcen für QnA Maker bearbeiten oder dem QnA Maker-Supportteam zusätzliche Informationen zu ihrer Installation zur Verfügung stellen.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Abrufen der neuesten Updates der QnA Maker-Runtime

Die QnA Maker-Runtime ist Teil von Azure App Service, einem beim [Erstellen eines QnA Maker-Diensts](./set-up-qnamaker-service-azure.md) im Azure-Portal bereitgestellten Dienst. Updates für die Runtime werden in regelmäßigen Abständen bereitgestellt. QnA Maker App Service befindet sich im automatischen Updatemodus nach der Websiteerweiterung vom April 2019 (Version 5+). Diese Version ist bereits so konzipiert, dass sie keine Ausfallzeiten bei Upgrades berücksichtigt. 

Sehen Sie sich die aktuelle Version unter https://www.qnamaker.ai/UserSettings an. Wenn Ihre Version älter als Version 5.x ist, müssen Sie App Service neu starten, damit die neuesten Updates angewandt werden.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    ![Azure-Ressourcengruppe von QnA Maker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Klicken Sie auf „App Service“, und öffnen Sie den Abschnitt „Übersicht“.

     ![QnA Maker in App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Starten Sie App Service neu. Dieser Vorgang sollte innerhalb weniger Sekunden abgeschlossen sein. Beachten Sie, dass alle abhängigen Anwendungen oder Bots, in denen dieser QnA Maker-Dienst verwendet wird, während dieses Neustarts nicht für Endbenutzer verfügbar sind.

    ![Neustarten von App Service QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Abrufen des Namens des QnA Maker-Diensthosts
Der Name des QnA Maker-Diensthosts ist beim Debuggen hilfreich, wenn Sie sich an den QnA Maker-Support oder UserVoice wenden. Der Hostname ist eine URL im Format https:// *{Hostname}* .azurewebsites.net.
    
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    ![QnA Maker-Ressourcengruppe im Azure-Portal](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Wählen Sie die der QnA Maker-Ressource zugeordnete App Service-Instanz aus. Normalerweise sind die Namen identisch.

     ![Auswählen von QnA Maker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Sie finden die Hostnamen-URL im Abschnitt „Übersicht“.

    ![QnA Maker-Hostname](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Optimieren von Fragen der Wissensdatenbank mit aktivem Lernen](./improve-knowledge-base.md)
