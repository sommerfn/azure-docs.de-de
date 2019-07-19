---
title: Erstellen eines Cognitive Services-Kontos im Azure-Portal
titlesuffix: Azure Cognitive Services
description: Anleitung zum Erstellen eines Azure Cognitive Services-APIs-Kontos im Azure-Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: aahi
ms.openlocfilehash: b857ee0395c447c8699b8f6a812853528812a7bd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445853"
---
# <a name="create-a-cognitive-services-account-using-the-azure-portal"></a>Erstellen eines Cognitive Services-Kontos im Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie sich für Azure Cognitive Services registrieren und ein Konto mit einem Abonnement mit einem einzelnen Dienst oder mit mehreren Diensten erstellen. Diese Dienste werden durch Azure-[Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) repräsentiert, die es Ihnen ermöglichen, Verbindungen mit einer oder mehreren der vielen verfügbaren Azure Cognitive Services-APIs herzustellen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Erstellen einer neuen Azure Cognitive Services-Ressource

Vor dem Erstellen einer Ressource müssen Sie über eine Azure-Ressourcengruppe verfügen. Jedes Cognitive Services-Konto (und die zugehörige Azure-Ressource) muss einer Azure-Ressourcengruppe zugeordnet werden. Beim Erstellen eines Kontos haben Sie die Möglichkeit, eine neue Ressourcengruppe zu erstellen oder eine vorhandene zu verwenden. In diesem Artikel wird gezeigt, wie Sie eine neue Ressourcengruppe erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **+Ressource erstellen**.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Sie können verfügbare Cognitive Services wie folgt ermitteln:
    * Verwenden Sie die Suchleiste, und geben Sie den Namen des Diensts ein, den Sie abonnieren möchten.
        * Geben Sie zum Erstellen einer Ressource für ein Abonnement mit mehreren Diensten in der Suchleiste **Cognitive Services** ein, und wählen Sie die Ressource **Cognitive Services** aus.

        ![Suchen nach Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Wählen Sie zum Anzeigen aller verfügbaren Cognitive Services unter **Azure Marketplace** die Option **KI + Machine Learning** . Falls der gewünschte Dienst nicht angezeigt wird, können Sie auf **Alle anzeigen** klicken und zu **Cognitive Services** scrollen. Klicken Sie auf **Mehr**, um den gesamten Katalog mit den Cognitive Services-APIs anzuzeigen.
    
        ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

    > [!IMPORTANT]
    > Merken Sie sich Ihren Azure-Speicherort, da Sie ihn beim Aufrufen der Azure Cognitive Services ggf. benötigen.

    |    |    |
    |--|--|
    | **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Es wird empfohlen, einen aussagekräftigen Namen zu wählen, z.B. *MeinCognitiveServicesKonto*. |
    | **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
    | **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
    | **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Ressourcengruppe** | Die [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group), die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

    ![Bildschirm „Ressourcenerstellung“](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-subscription"></a>Abrufen der Schlüssel für Ihr Abonnement

Nachdem Sie Ihre Ressource erstellt haben, können Sie sie über das Azure Dashboard aufrufen, wenn Sie sie angeheftet haben. Andernfalls finden Sie sie in **Ressourcengruppen**. Nach dem Auswählen Ihrer Ressource können Sie die Schlüssel abrufen, indem Sie unter **Ressourcenverwaltung** die Option **Schlüssel** wählen.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

So entfernen Sie eine Ressourcengruppe über das Azure-Portal:

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Ressourcengruppen**, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie nach der zu löschenden Ressourcengruppe, und klicken Sie mit der rechten Maustaste rechts neben dem Eintrag auf die Schaltfläche „Mehr“ (...).
3. Klicken Sie auf **Ressourcengruppe löschen**, und bestätigen Sie den Vorgang.

## <a name="see-also"></a>Weitere Informationen

* [Authentifizieren von Anforderungen an Azure Cognitive Services](authentication.md)
* [Was ist Azure Cognitive Services?](Welcome.md)
* [Unterstützung für natürliche Sprachen in Azure Cognitive Services](language-support.md)
* [Containerunterstützung in Azure Cognitive Services](cognitive-services-container-support.md)
