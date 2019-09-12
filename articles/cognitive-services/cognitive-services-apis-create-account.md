---
title: Erstellen einer Cognitive Services-Ressource im Azure-Portal
titleSuffix: Azure Cognitive Services
description: Beginnen Sie mit Azure Cognitive Services, indem Sie im Azure-Portal eine Ressource erstellen und abonnieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: c7db2b4d49e3b9297c32d2e11ffe7c7702c17544
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274667"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Erstellen einer Cognitive Services-Ressource mithilfe des Azure-Portals

Verwenden Sie diese Schnellstartanleitung, um über das Azure-Portal eine Azure Cognitive Services-Ressource zu erstellen. Nachdem Sie die Cognitive Services-Ressource erfolgreich erstellt haben, erhalten Sie einen Endpunkt und einen Schlüssel, mit denen Sie Ihre Anwendungen authentifizieren können.

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement: [Erstellen Sie ein kostenloses Abonnement](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Erstellen einer neuen Azure Cognitive Services-Ressource

Vor dem Erstellen einer Cognitive Services-Ressource müssen Sie über eine Azure-Ressourcengruppe verfügen, die die Ressource enthalten soll. Beim Erstellen einer neuen Ressource haben Sie die Möglichkeit, entweder eine neue Ressourcengruppe zu erstellen oder eine bereits vorhandene zu verwenden. In diesem Artikel wird gezeigt, wie Sie eine neue Ressourcengruppe erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **+Ressource erstellen**.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Wie bereits ausgeführt, können Sie eine Cognitive Services-Ressource auf zwei Arten erstellen: mit einer Ressource für mehrere Dienste oder einer Ressource für einen einzelnen Dienst.

    #### <a name="multi-service-resourcetabmultiservice"></a>[Ressource für mehrere Dienste](#tab/multiservice)

    Um eine Ressource für mehrere Dienste zu erstellen, geben Sie **Cognitive Services** in der Suchleiste ein.

    ![Suchen nach Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    Wählen Sie auf der Seite „Cognitive Services“ die Schaltfläche **Erstellen** aus.

    ![Cognitive Services-Konto erstellen](media/cognitive-services-apis-create-account/azurecogservsearchmulti-2.png)

    #### <a name="single-service-resourcetabsingleservice"></a>[Ressource für einen einzelnen Dienst](#tab/singleservice)

    Wählen Sie zum Anzeigen aller verfügbaren Cognitive Services unter **Azure Marketplace** die Option **KI + Machine Learning** . Falls der gewünschte Dienst nicht angezeigt wird, können Sie auf **Alle anzeigen** klicken und zu **Cognitive Services** scrollen. Klicken Sie auf **Mehr anzeigen**, um den gesamten Katalog von Cognitive Services anzuzeigen.

    Wenn Sie den gewünschten Dienst markiert haben, klicken Sie auf **Erstellen**.
    
    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azureMarketplace.png)

    ***
3. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

    #### <a name="multi-service-resourcetabmultiservice"></a>[Ressource für mehrere Dienste](#tab/multiservice)

    |    |    |
    |--|--|
    | **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Beispiel: *MyCognitiveServicesResource*. |
    | **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
    | **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. Merken Sie sich Ihren Azure-Speicherort, da Sie ihn beim Aufrufen der Azure Cognitive Services ggf. benötigen. |
    | **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Ressourcengruppe** | Die Azure-Ressourcengruppe, die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

    ![Bildschirm „Ressourcenerstellung“](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    Klicken Sie auf **Create**.

    #### <a name="single-service-resourcetabsingleservice"></a>[Ressource für einen einzelnen Dienst](#tab/singleservice)

    |    |    |
    |--|--|
    | **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Beispiel: *TextAnalyticsResource*. |
    | **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
    | **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. Merken Sie sich Ihren Azure-Speicherort, da Sie ihn beim Aufrufen der Azure Cognitive Services ggf. benötigen. |
    | **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Ressourcengruppe** | Die Azure-Ressourcengruppe, die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

    ![Bildschirm „Ressourcenerstellung“](media/cognitive-services-apis-create-account/resource_create_screen.png)

    Klicken Sie auf **Create**.

    ***

## <a name="get-the-keys-for-your-resource"></a>Abrufen der Schlüssel für die Ressource

Nachdem die Ressource erfolgreich erstellt wurde, erhalten Sie oben rechts auf dem Bildschirm eine Popupbenachrichtigung. Klicken Sie in der Benachrichtigung auf **Zu Ressource wechseln**, um die von Ihnen erstellte Cognitive Services-Ressource anzuzeigen. 

![Zu Cognitive Services-Ressource wechseln](media/cognitive-services-apis-create-account/cog-serv-go-to-resource.png)

Aus dem Schnellstartbereich, der geöffnet wird, können Sie auf Ihren Endpunkt und Schlüssel zugreifen.

![Schlüssel und Endpunkt abrufen](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Tarife und Abrechnung

Die Tarife (und der Betrag, der Ihnen in Rechnung gestellt wird) basieren auf der Anzahl der unter ihren Authentifizierungsinformationen gesendeten Transaktionen. Für jeden Tarif wird Folgendes angegeben:
* Maximale Anzahl zulässiger Transaktionen pro Sekunde (TPS).
* Dienstfeatures, die innerhalb des Tarifs aktiviert sind.
* Die Kosten für eine vordefinierte Anzahl von Transaktionen. Wird diese Anzahl überschritten, fällt eine zusätzliche Gebühr gemäß den [Preisdetails](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) für Ihren Dienst an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen in der Gruppe enthaltenen Ressourcen gelöscht.

So entfernen Sie eine Ressourcengruppe über das Azure-Portal:

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Ressourcengruppen**, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie nach der zu löschenden Ressourcengruppe, und klicken Sie mit der rechten Maustaste rechts neben dem Eintrag auf die Schaltfläche „Mehr“ (...).
3. Klicken Sie auf **Ressourcengruppe löschen**, und bestätigen Sie den Vorgang.

## <a name="see-also"></a>Weitere Informationen

* [Authentifizieren von Anforderungen an Azure Cognitive Services](authentication.md)
* [Was ist Azure Cognitive Services?](Welcome.md)
* [Unterstützung für natürliche Sprache](language-support.md)
* [Containerunterstützung in Azure Cognitive Services](cognitive-services-container-support.md)
