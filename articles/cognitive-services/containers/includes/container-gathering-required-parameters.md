---
title: Ermitteln erforderlicher Parameter
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Die Parameter für alle Cognitive Services-Container
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2019
ms.author: dapine
ms.openlocfilehash: bf4e8f06642a4a764794a75e2b49f15958796c61
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73465947"
---
## <a name="gathering-required-parameters"></a>Ermitteln erforderlicher Parameter

Es gibt drei primäre Parameter, die für alle Cognitive Services-Container benötigt werden. Die Lizenzbedingungen müssen mit dem Wert `accept` vorhanden sein. Außerdem werden sowohl die Endpunkt-URL als auch der API-Schlüssel benötigt.

### <a name="endpoint-uri-endpoint_uri"></a>Endpunkt-URI `{ENDPOINT_URI}`

Der **Endpunkt**-URI-Wert ist im Azure-Portal auf der Seite *Übersicht* der entsprechenden Cognitive Service-Ressource verfügbar. Navigieren Sie zur Seite *Übersicht*, und bewegen Sie den Mauszeiger auf den Endpunkt, sodass das `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span>-Symbol angezeigt wird. Kopieren und verwenden Sie diesen bei Bedarf.

![Erfassen der Endpunkt-URI für die spätere Verwendung](../media/overview-endpoint-uri.png)

### <a name="keys-api_key"></a>Schlüssel `{API_KEY}`

Dieser Schlüssel wird zum Starten des Containers verwendet und ist im Azure-Portal auf der Seite „Schlüssel“ der entsprechenden Cognitive Service-Ressource verfügbar. Navigieren Sie zur Seite *Schlüssel*, und klicken Sie auf das `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span>-Symbol.

![Abrufen eines der beiden Schlüssel für die spätere Verwendung](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> Diese Abonnementschlüssel werden für den Zugriff auf Ihre Cognitive Service-API verwendet. Geben Sie Ihre Schlüssel nicht weiter. Speichern Sie diese beispielsweise sicher mit Azure Key Vault. Es wird außerdem empfohlen, diese Schlüssel regelmäßig neu zu generieren. Für einen API-Aufruf ist nur ein Schlüssel erforderlich. Beim erneuten Generieren des ersten Schlüssels können Sie den zweiten Schlüssel für kontinuierlichen Zugriff auf den Dienst verwenden.