---
title: 'Schnellstart: Erstellen einer Python-App, die Azure Cache for Redis verwendet'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine Python-App erstellen, die Azure Cache for Redis verwendet.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/11/2018
ms.author: yegu
ms.custom:
- mvc
- seo-python-october2019
ms.openlocfilehash: 5367896c931bf7c5f52b0874d49ede2fc78614ab
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72435527"
---
# <a name="quickstart-create-a-python-app-that-uses-azure-cache-for-redis"></a>Schnellstart: Erstellen einer Python-App, die Azure Cache for Redis verwendet

In diesem Artikel integrieren Sie Azure Cache for Redis in eine Python-App, um Zugriff auf einen sicheren, dedizierten Cache zu erhalten, der von jeder Anwendung in Azure aus zugänglich ist.

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- [Python 2 oder 3](https://www.python.org/downloads/)

## <a name="create-an-azure-cache-for-redis-on-azure"></a>Erstellen einer Azure Cache for Redis-Instanz in Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>Installieren von redis-py

[Redis-py](https://github.com/andymccurdy/redis-py) ist eine Python-Schnittstelle für Azure Cache for Redis. Verwenden Sie das Python-Pakettool *pip*, um das redis-py-Paket zu installieren. 

Im folgenden Beispiel wird *pip3* für Python3 verwendet, um das redis-py-Paket unter Windows 10 zu installieren, indem eine Visual Studio 2019 Developer-Eingabeaufforderung mit erhöhten Administratorrechten verwendet wird.

```python
    pip3 install redis
```

![Installieren der redis-py-Python-Schnittstelle für Azure Cache for Redis](./media/cache-python-get-started/cache-python-install-redis-py.png)


## <a name="read-and-write-to-the-cache"></a>Lese- und Schreibvorgänge für den Cache

Führen Sie Python aus, und testen Sie die Cachenutzung über die Befehlszeile. Ersetzen Sie `<Your Host Name>` und `<Your Access Key>` durch die Werte für Ihre Azure Cache for Redis-Instanz. 

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>.redis.cache.windows.net',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

> [!IMPORTANT]
> Für Redis Version 3.0 oder höher wird die SSL-Zertifikatüberprüfung erzwungen. Beim Herstellen der Verbindung mit Redis muss „ssl_ca_certs“ explizit festgelegt werden. Im Falle von RH Linux befindet sich „ssl_ca_certs“ im Zertifikatsmodul „/etc/pki/tls/certs/ca-bundle.crt“.

## <a name="create-a-python-script"></a>Erstellen eines Python-Skripts

Erstellen Sie eine neue Skripttextdatei mit dem Namen *PythonApplication1.py*.

Fügen Sie das folgende Skript der Datei *PythonApplication1.py* hinzu, und speichern Sie die Datei. Mit diesem Skript wird der Cachezugriff getestet. Ersetzen Sie `<Your Host Name>` und `<Your Access Key>` durch die Werte für Ihre Azure Cache for Redis-Instanz. 

```python
import redis

myHostname = "<Your Host Name>.redis.cache.windows.net"
myPassword = "<Your Access Key>"

r = redis.StrictRedis(host=myHostname, port=6380,
                      password=myPassword, ssl=True)

result = r.ping()
print("Ping returned : " + str(result))

result = r.set("Message", "Hello!, The cache is working with Python!")
print("SET Message returned : " + str(result))

result = r.get("Message")
print("GET Message returned : " + result.decode("utf-8"))

result = r.client_list()
print("CLIENT LIST returned : ")
for c in result:
    print("id : " + c['id'] + ", addr : " + c['addr'])
```

Führen Sie das Skript mit Python aus.

![Ausführen eines Python-Skripts zum Testen des Cachezugriffs in Azure Cache for Redis](./media/cache-python-get-started/cache-python-completed.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit einem anderen Tutorial fortfahren möchten, können Sie die in dieser Schnellstartanleitung erstellten Ressourcen beibehalten und wiederverwenden.

Wenn Sie die Schnellstart-Beispielanwendung nicht mehr benötigen, können Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen löschen, um das Anfallen von Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden also dauerhaft gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Falls Sie die Ressourcen zum Hosten dieses Beispiels in einer vorhandenen Ressourcengruppe erstellt haben, die beizubehaltende Ressourcen enthält, können Sie die Ressourcen einzeln über das jeweilige Blatt löschen, statt die Ressourcengruppe zu löschen.
>

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

Geben Sie im Textfeld **Nach Name filtern...** den Namen Ihrer Ressourcengruppe ein. In diesem Artikel wurde eine Ressourcengruppe mit dem Namen *TestResources* verwendet. Klicken Sie in der Ergebnisliste in Ihrer Ressourcengruppe auf **...** und dann auf **Ressourcengruppe löschen**.

![Löschen der Schnellstart-Ressourcengruppe für Azure Cache for Redis](./media/cache-web-app-howto/delete-your-resource-group-for-azure-cache-for-redis.png)

Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie zur Bestätigung den Namen Ihrer Ressourcengruppe ein, und klicken Sie auf **Löschen**.

Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer einfachen ASP.NET-Web-App, die eine Azure Cache for Redis-Instanz verwendet](./cache-web-app-howto.md)

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
