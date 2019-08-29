---
title: Generieren eines Sicherheitstokens für den Zugriff auf IoT Plug & Play-Vorschaurepository | Microsoft-Dokumentation
description: Generieren Sie ein Shared Access Signature-Token, das verwendet werden soll, wenn Sie programmgesteuert auf ein IoT Plug & Play-Vorschaumodellrepository zugreifen.
author: YasinMSFT
ms.author: yahajiza
ms.date: 08/06/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e5d6e7087a7e3d5f4a001e16c5cfa19a6df6a68e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879226"
---
# <a name="generate-sas-token"></a>Generieren des SAS-Tokens

In dieser Anleitung erfahren Sie, wie Sie programmgesteuert ein Shared Access Signature (SAS)-Token für die Verwendung mit den APIs des IoT Plug & Play-Vorschaumodellrepositorys generieren.

## <a name="python"></a>Python

Der folgende Codeausschnitt zeigt, wie Sie ein SAS-Token mithilfe von Python generieren:

```python
from base64 import b64decode, b64encode
from hashlib import sha256
from hmac import digest
from time import time
from urllib.parse import quote_plus, urlencode

def calculate_sas_token(hostname, repo_id, key_name, key, expiry_in_second):
    expire = int(time() + expiry_in_second)
    sign_value = "{0}\n{1}\n{2}".format(repo_id, quote_plus(hostname), expire)
    sig = b64encode(digest(b64decode(key), sign_value.encode("utf-8"), sha256))
    token = "SharedAccessSignature " + urlencode({
        "sr": hostname, 
        "sig": sig,
        "se": str(expire),
        "skn": key_name,
        "rid": repo_id
        })
    return token
```

## <a name="c"></a>C\#

Der folgende Codeausschnitt zeigt, wie Sie ein SAS-Token mithilfe von C\# generieren:

```csharp
public static string generateSasToken(string hostName, string repoId, string key, string keyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = repoId + "\n" + WebUtility.UrlEncode(hostName) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(
        CultureInfo.InvariantCulture,
        "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}&rid={4}",
        WebUtility.UrlEncode(hostName),
        WebUtility.UrlEncode(signature),
        expiry,
        keyName,
        repoId);

    return token;
}
```

## <a name="use-the-sas-token"></a>Verwenden des SAS-Tokens

Nachdem Sie ein SAS-Token generiert haben, können Sie es zum Erstellen einer HTTP POST-Anforderung verwenden. Beispiel:

```text
POST https:///models/{modelId}?repositoryId={repositoryId}&api-version=2019-07-01-preview
```

Wenn Sie einem Client einen SAS-Token übergeben, verfügt der Client nicht über den Primärschlüssel der Ressource und kann den Hash nicht umkehren, um ihn zu erhalten. Mit einem SAS-Token können Sie steuern, auf was der Client wie lange zugreifen kann. Wenn Sie den Primärschlüssel in der Richtlinie ändern, werden alle aus diesem Schlüssel erstellten SAS-Token ungültig.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Sicherheitstoken erstellen können, mit denen Sie auf die IoT Plug & Play-Modellvorschaurepositorys zugreifen können, empfiehlt es sich, als nächstes die [Anleitung für Entwickler – Modellierung mit IoT Plug & Play (Vorschauversion)](concepts-developer-guide.md) zu lesen, um mehr zu erfahren.
