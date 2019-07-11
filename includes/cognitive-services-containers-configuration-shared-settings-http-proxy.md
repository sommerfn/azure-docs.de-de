---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 9911b1c92bdca6c0cdf064ea484cfb603e659467
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712546"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| NAME | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTP_PROXY|Zeichenfolge|Der zu verwendende Proxy, z. B. `http://proxy:8888`.<br><proxy-url>|
|HTTP_PROXY_CREDS|Zeichenfolge|Beliebige Anmeldeinformationen, die zur Authentifizierung bei dem Proxy erforderlich sind, z. B. Benutzername:Kennwort.|
|`<proxy-user>`|Zeichenfolge|Der Benutzer für den Proxy.|
|`proxy-password`|Zeichenfolge|Das Kennwort, das dem `<proxy-user>` für den Proxy zugeordnet ist.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```