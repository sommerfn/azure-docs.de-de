---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 1ea0b01997d3d5cecff73f951c51de5898c2f07a
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503633"
---
Die folgenden Azure CLI-Befehle stellen eine Ressource für die Anomalieerkennung im kostenlosen Tarif bereit. Klicken Sie auf die Schaltfläche **Ausprobieren**, fügen Sie den Code ein, und drücken Sie die Eingabetaste, um ihn in der Azure Cloud Shell auszuführen. Diese druckt Ihre Schlüssel zur Authentifizierung Ihre Anwendung. Nach Abschluss [erstellen Sie eine Umgebungsvariable](../articles/cognitive-services/cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) für Ihre Schlüssel mit dem Namen `ANOMALY_DETECTOR_KEY`.

> [!NOTE]
> * Optional haben Sie auch folgende Möglichkeiten:
>    * Erstellen Sie eine Ressource mithilfe des [Azure-Portals](../articles/cognitive-services/cognitive-services-apis-create-account.md) oder der [Azure CLI](../articles/cognitive-services/cognitive-services-apis-create-account.md) auf Ihrem lokalen Computer.
>    * Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, mit dem Sie sieben Tage kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.
>    * Sie können die Ressource im [Azure-Portal](https://portal.azure.com/) anzeigen. 

Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie bereitstellen. Jedes Cognitive Services-Konto (und die zugehörigen Azure-Ressource) muss einer Azure-Ressourcengruppe zugeordnet werden.

1. Erstellen einer Azure-Ressourcengruppe

    ```azurecli-interactive
    az group create \
        --name example-anomaly-detector-resource-group \
        --location westus2
    ```

2. Erstellen einer Ressource zur Anomalieerkennung im kostenlosen Tarif

    ```azurecli-interactive
    az cognitiveservices account create \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group \
        --kind AnomalyDetector \
        --sku F0 \
        --location westus2 \
        --yes
    ```

3. Auflisten der Schlüssel für Ihre Ressource

    ```azurecli-interactive
    az cognitiveservices account keys list \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group
    ```