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
ms.openlocfilehash: af01c0c2586ce7df1902a0bcc502c6fd06a5215d
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697909"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Erstellen einer Cognitive Services-Ressource mithilfe des Azure-Portals

Verwenden Sie diese Schnellstartanleitung für Ihre ersten Schritte mit Azure Cognitive Services mithilfe des Azure-Portals. Cognitive Services werden von [Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) dargestellt, die Sie in Ihrem Azure-Abonnement erstellen. Verwenden Sie nach dem Erstellen der Ressource die Schlüssel und einen für Sie zum Authentifizieren Ihrer Anwendungen generierten Endpunkt. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement: [Erstellen Sie ein kostenloses Abonnement](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Erstellen einer neuen Azure Cognitive Services-Ressource

Vor dem Erstellen einer Cognitive Services-Ressource müssen Sie über eine Azure-Ressourcengruppe verfügen, die die Ressource enthalten soll. Beim Erstellen einer neuen Ressource haben Sie die Möglichkeit, entweder eine neue Ressourcengruppe zu erstellen oder eine bereits vorhandene zu verwenden. In diesem Artikel wird gezeigt, wie Sie eine neue Ressourcengruppe erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **+Ressource erstellen**.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Sie können verfügbare Cognitive Services wie folgt ermitteln:
    * Verwenden Sie die Suchleiste, und geben Sie den Namen des Diensts ein, den Sie abonnieren möchten.
        * Geben Sie in der Suchleiste **Cognitive Services** ein, und klicken Sie auf die Ressource **Cognitive Services**, um eine Ressource für ein Abonnement mit mehreren Diensten zu erstellen.

        ![Suchen nach Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Wählen Sie zum Anzeigen aller verfügbaren Cognitive Services unter **Azure Marketplace** die Option **KI + Machine Learning** . Falls der gewünschte Dienst nicht angezeigt wird, können Sie auf **Alle anzeigen** klicken und zu **Cognitive Services** scrollen. Klicken Sie auf **Mehr**, um den gesamten Katalog mit den Cognitive Services-APIs anzuzeigen.
    
        ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

    > [!IMPORTANT]
    > Merken Sie sich Ihren Azure-Speicherort, da Sie ihn beim Aufrufen der Azure Cognitive Services ggf. benötigen.

    |    |    |
    |--|--|
    | **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Beispiel: *MyCognitiveServicesAccount*. |
    | **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
    | **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
    | **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Ressourcengruppe** | Die [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group), die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

    ![Bildschirm „Ressourcenerstellung“](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-resource"></a>Abrufen der Schlüssel für die Ressource

Nachdem Sie Ihre Ressource erstellt haben, können Sie sie über das Azure Dashboard aufrufen, wenn Sie sie angeheftet haben. Andernfalls finden Sie sie in **Ressourcengruppen**. Nach dem Auswählen Ihrer Ressource können Sie die Schlüssel abrufen, indem Sie unter **Ressourcenverwaltung** die Option **Schlüssel** wählen.

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
* [Unterstützung für natürliche Sprachen in Azure Cognitive Services](language-support.md)
* [Containerunterstützung in Azure Cognitive Services](cognitive-services-container-support.md)
