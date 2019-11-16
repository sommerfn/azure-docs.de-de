---
title: Erstellen einer Cognitive Services-Ressource mithilfe der Azure-Befehlszeilenschnittstelle
titleSuffix: Azure Cognitive Services
description: Beginnen Sie mit Azure Cognitive Services, indem Sie mithilfe der Azure-Befehlszeilenschnittstelle eine Ressource erstellen und diese abonnieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: aahi
ms.openlocfilehash: d715252f16a34f2261c108ebd3ff5d1f6f3424d8
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887913"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-command-line-interfacecli"></a>Erstellen einer Cognitive Services-Ressource mithilfe der Azure-Befehlszeilenschnittstelle (CLI)

Verwenden Sie diese Schnellstartanleitung für die Verwendung von Azure Cognitive Services mithilfe der [Azure-Befehlszeilenschnittstelle (Azure Command Line Interface, CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Cognitive Services werden von [Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) dargestellt, die Sie in Ihrem Azure-Abonnement erstellen. Verwenden Sie nach dem Erstellen der Ressource die Schlüssel und einen für Sie zum Authentifizieren Ihrer Anwendungen generierten Endpunkt. 


In diesem Schnellstart erfahren Sie, wie Sie sich mit der [Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) für Azure Cognitive Services registrieren und ein Konto mit einem Abonnement mit einem einzelnen Dienst oder mehreren Diensten erstellen. Diese Dienste werden durch Azure-[Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) repräsentiert, die Ihnen ermöglichen, Verbindungen mit einer oder mehreren der Azure Cognitive Services-APIs herzustellen.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement. Dieses können Sie [kostenlos erstellen](https://azure.microsoft.com/free/).
* Die [Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="install-the-azure-cli-and-sign-in"></a>Installieren der Azure CLI und Anmelden 

Installieren Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Wenn Sie sich bei Ihrer lokalen Installation der Befehlszeilenschnittstelle anmelden möchten, führen Sie den Befehl [az login](https://docs.microsoft.com/cli/azure/reference-index#az-login) aus:

```console
az login
```

Sie können auch die grüne **Ausprobieren**-Schaltfläche verwenden, um diese Befehle in Ihrem Browser auszuführen.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Erstellen einer neuen Azure Cognitive Services-Ressourcengruppe

Vor dem Erstellen einer Cognitive Services-Ressource müssen Sie über eine Azure-Ressourcengruppe verfügen, die die Ressource enthalten soll. Beim Erstellen einer neuen Ressource haben Sie die Möglichkeit, entweder eine neue Ressourcengruppe zu erstellen oder eine bereits vorhandene zu verwenden. In diesem Artikel wird gezeigt, wie Sie eine neue Ressourcengruppe erstellen.

### <a name="choose-your-resource-group-location"></a>Auswählen des Speicherorts Ihrer Ressourcengruppe

Um eine Ressource zu erstellen, benötigen Sie einen der für Ihr Abonnement verfügbaren Azure-Speicherorte. Sie können eine Liste der verfügbaren Speicherorte mit dem Befehl [az account list-locations](/cli/azure/account#az-account-list-locations) abrufen. Auf die meisten Cognitive Services kann von mehreren Speicherorten aus zugegriffen werden. Wählen Sie den Ihnen am nächsten gelegenen aus, oder stellen Sie fest, welche Speicherorte für den Dienst verfügbar sind.

> [!IMPORTANT]
> * Merken Sie sich Ihren Azure-Speicherort, da Sie ihn beim Aufrufen der Azure Cognitive Services benötigen.
> * Die Verfügbarkeit einiger Cognitive Services kann je nach Region variieren. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Sobald Ihr Azure-Speicherort feststeht, erstellen Sie zuerst eine neue Ressourcengruppe in der Azure-Befehlszeilenschnittstelle, und verwenden Sie hierzu den Befehl [az group create](/cli/azure/group#az-group-create).

Ersetzen Sie im folgenden Beispiel den Azure-Speicherort `westus2` durch einen der für Ihr Abonnement verfügbaren Azure-Speicherorte.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer Cognitive Services-Ressource

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Auswählen eines Cognitive Service und eines Tarifs

Wenn Sie eine neue Ressource erstellen, müssen Sie die „Art“ des Diensts kennen, den Sie verwenden möchten, sowie den [Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/), den Sie wünschen (oder die SKU). Diese und andere Informationen werden Sie beim Erstellen der Ressource als Parameter verwenden.

### <a name="multi-service"></a>Mehrere Dienste

| Dienst                    | Variante                      |
|----------------------------|---------------------------|
| Mehrere Dienste. Weitere Informationen finden Sie auf der Seite mit der [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/).            | `CognitiveServices`     |


> [!NOTE]
> Bei vielen der unten angegebenen Cognitive Services können Sie einen kostenlosen Tarif nutzen, um den Dienst auszuprobieren. Um den kostenlosen Tarif zu nutzen, verwenden Sie `F0` als SKU für Ihre Ressource.

### <a name="vision"></a>Bildanalyse

| Dienst                    | Variante                      |
|----------------------------|---------------------------|
| Maschinelles Sehen            | `ComputerVision`          |
| Custom Vision – Vorhersage | `CustomVision.Prediction` |
| Custom Vision – Training   | `CustomVision.Training`   |
| Gesichtserkennungs-API                   | `Face`                    |
| Formularerkennung            | `FormRecognizer`          |
| Freihanderkennung             | `InkRecognizer`           |

### <a name="search"></a>Suchen,

| Dienst            | Variante                  |
|--------------------|-----------------------|
| Bing-Vorschlagssuche   | `Bing.Autosuggest.v7` |
| Benutzerdefinierte Bing-Suche | `Bing.CustomSearch`   |
| Bing-Entitätssuche | `Bing.EntitySearch`   |
| Bing-Suche        | `Bing.Search.v7`      |
| Bing-Rechtschreibprüfung   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Spracheingabe

| Dienst            | Variante                 |
|--------------------|----------------------|
| Spracherkennungsdienste    | `SpeechServices`     |
| Spracherkennung | `SpeakerRecognition` |

### <a name="language"></a>Sprache

| Dienst            | Variante                |
|--------------------|---------------------|
| Formularerkennung | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Textanalyse     | `TextAnalytics`     |
| Textübersetzung   | `TextTranslation`   |

### <a name="decision"></a>Entscheidung

| Dienst           | Variante               |
|-------------------|--------------------|
| Anomalieerkennung  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalisierung      | `Personalizer`     |

Sie können eine Liste der verfügbaren „Arten“ von Cognitive Services mit dem Befehl [az cognitiveservices account list-kinds](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) finden:

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Hinzufügen einer neuen Ressource zu Ihrer Ressourcengruppe

Um eine neue Cognitive Services-Ressource zu erstellen und zu abonnieren, verwenden Sie den Befehl [az cognitiveservices account create](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create). Dieser Befehl fügt der zuvor erstellten Ressourcengruppe eine neue abrechenbare Ressource hinzu. Wenn Sie Ihre neue Ressource erstellen, müssen Sie die „Art“ des Diensts kennen, den Sie verwenden möchten, sowie dessen Tarif (oder die SKU) und einen Azure-Speicherort.

Sie können eine (kostenlose) F0-Ressource zur Anomalieerkennung erstellen, die mit dem folgenden Befehl den Namen `anomaly-detector-resource` erhält.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-resource"></a>Abrufen der Schlüssel für die Ressource

Wenn Sie sich bei Ihrer lokalen Installation der Befehlszeilenschnittstelle (CLI) anmelden möchten, führen Sie den Befehl [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) aus.

```console
az login
```

Rufen Sie mit dem Befehl [az cognitiveservices account keys list](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) die Schlüssel für Ihre Cognitive Service-Ressource ab.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Tarife und Abrechnung

Die Tarife (und der Betrag, der Ihnen in Rechnung gestellt wird) basieren auf der Anzahl der unter ihren Authentifizierungsinformationen gesendeten Transaktionen. Für jeden Tarif wird Folgendes angegeben:
* Maximale Anzahl zulässiger Transaktionen pro Sekunde (TPS).
* Dienstfeatures, die innerhalb des Tarifs aktiviert sind.
* Die Kosten für eine vordefinierte Anzahl von Transaktionen. Wird diese Anzahl überschritten, fällt eine zusätzliche Gebühr gemäß den [Preisdetails](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) für Ihren Dienst an.

## <a name="get-current-quota-usage-for-your-resource"></a>Abrufen des aktuellen Kontingentverbrauchs für Ihre Ressource

Rufen Sie mit dem Befehl [az cognitiveservices account list-usage](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-usage) den Verbrauch für Ihre Cognitive Service-Ressource ab.

```azurecli-interactive
az cognitiveservices account list-usage \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --subscription subscription-name
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie eine Cognitive Services-Ressource bereinigen und entfernen möchten, können Sie entweder diese selbst oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen in der Gruppe enthaltenen Ressourcen gelöscht.

Verwenden Sie den Befehl „az group delete“, um die Ressourcengruppe und die zugeordneten Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name storage-resource-group
```

## <a name="see-also"></a>Weitere Informationen

* [Authentifizieren von Anforderungen an Azure Cognitive Services](authentication.md)
* [Was ist Azure Cognitive Services?](Welcome.md)
* [Unterstützung für natürliche Sprache](language-support.md)
* [Containerunterstützung in Azure Cognitive Services](cognitive-services-container-support.md)
