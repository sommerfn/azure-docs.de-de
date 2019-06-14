---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: e5166934c4f7e1ecc917d2a5afcbb26ea9a637a2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66132755"
---
## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können Sie den folgenden Befehl verwenden, um die Ressourcengruppe, die Azure Cache for Redis-Instanz und alle dazugehörigen Ressourcen in der Ressourcengruppe zu entfernen.

```azurecli
az group delete --name contosoGroup
```