---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b1bbc11d7772e4f56d7dc6ead580b0a0cbd3cd8d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68669646"
---
Mit Functions ist es einfach, die Application Insights-Integration über das [Azure-Portal] einer Funktionen-App hinzuzufügen.

1. Wählen Sie im [Portal][Azure-Portal] die Optionen **Alle Dienste > Funktionen-Apps** und dann Ihre Funktions-App aus. Wählen Sie anschließend oben im Fenster das Banner **Application Insights** aus.

    ![Aktivieren von Application Insights über das Portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Erstellen Sie eine Application Insights-Ressource. Verwenden Sie dazu die Einstellungen, die in der Tabelle unterhalb der Abbildung angegeben sind:

   ![Erstellen einer Application Insights-Ressource](media/functions-connect-new-app-insights/ai-general.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Eindeutiger App-Name | Es ist am einfachsten, den gleichen Namen wie für Ihre Funktionen-App zu verwenden, der in Ihrem Abonnement eindeutig sein muss. | 
    | **Location** | Europa, Westen | Verwenden Sie nach Möglichkeit dieselbe [Region](https://azure.microsoft.com/regions/) wie für Ihre Funktions-App (oder eine Region in der Nähe). |

1. Klicken Sie auf **OK**. Die Application Insights-Ressource wird in derselben Ressourcengruppe und unter demselben Abonnement wie Ihre Funktionen-App erstellt. Schließen Sie nach der Erstellung der Ressource das Application Insights-Fenster.

1. Wählen Sie in Ihrer Funktions-App die Option **Anwendungseinstellungen** aus, und scrollen Sie dann nach unten zu **Anwendungseinstellungen**. Wenn die Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY` angezeigt wird, bedeutet dies, dass die Application Insights-Integration für Ihre unter Azure ausgeführte Funktions-App aktiviert ist.

[Azure-Portal]: https://portal.azure.com
