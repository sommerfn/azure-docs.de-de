---
title: Importieren einer API-App als API mit dem Azure-Portal | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie API Management (APIM) verwenden, um API-Apps als API zu importieren.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 484621668b582953d49c366336a49970d82c3fbf
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70073267"
---
# <a name="import-an-api-app-as-an-api"></a>Importieren einer API-App als API

Dieser Artikel zeigt, wie Sie eine API-App als API importieren. Der Artikel zeigt außerdem, wie Sie die APIM-API testen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Importieren einer API-App als API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ Stellen Sie sicher, dass Ihr Abonnement eine API-App enthält. Weitere Informationen finden Sie in der [App Service-Dokumentation](https://docs.microsoft.com/azure/app-service/).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie in der Liste **Neue API hinzufügen** die Option **API-App** aus.

    ![Eine API-App](./media/import-api-app-as-api/api-app.png)
3. Klicken Sie auf **Durchsuchen**, um die Liste der API-Apps in Ihrem Abonnement anzuzeigen.
4. Wählen Sie die App aus. APIM sucht den Swagger, der der ausgewählten App zugeordnet ist, ruft ihn ab und importiert ihn. 

    Falls APIM keinen Swagger findet, wird die API als „Pass-Through“-API zur Verfügung gestellt. 
5. Fügen Sie ein API-URL-Suffix hinzu. Das Suffix ist ein Name, der diese spezifische API in dieser APIM-Instanz identifiziert. Es muss in dieser APIM-Instanz eindeutig sein.
6. Veröffentlichen Sie die API, indem Sie sie einem Produkt zuordnen. In diesem Fall wird das Produkt „*Unlimited*“ verwendet.  Wenn Sie möchten, dass die API veröffentlicht wird und dann Entwicklern zur Verfügung steht, fügen Sie sie einem Produkt hinzu. Sie können dies während der Erstellung der API vornehmen oder später festlegen.

    Bei Produkten handelt es sich um API-Zuordnungen. Sie können eine Reihe von APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch standardmäßig alle Produkte abonniert.

    Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte:

    * **Starter**
    * **Unbegrenzt**   
7. Klicken Sie auf **Erstellen**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testen der neuen APIM-API im Azure-Portal

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, die Vorgänge einer API anzuzeigen und zu testen.  

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Wählen Sie einen Vorgang aus.

    Die Seite zeigt Felder für Abfrageparameter und Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird. 
1. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="call-operation"></a>Aufrufen einer Operation aus dem Entwicklerportal

Vorgänge können auch im **Entwicklerportal** aufgerufen werden, um APIs zu testen. 

1. Wählen Sie die API aus, die Sie im Schritt „Importieren und Veröffentlichen einer Back-End-API“ erstellt haben.
2. Klicken Sie auf **Entwicklerportal**.

    Die Website „Entwicklerportal“ wird geöffnet.
3. Wählen Sie die **API** aus, die Sie erstellt haben.
4. Klicken Sie auf den Vorgang, den Sie testen möchten.
5. Klicken Sie auf **Ausprobieren**.
6. Klicken Sie auf **Senden**.
    
    Nach dem Aufruf der Operation zeigt das Entwicklerportal den **Antwortstatus**, die **Antwortheader** sowie den **Antwortinhalt** an.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transformieren und Schützen einer veröffentlichten API](transform-api.md)
