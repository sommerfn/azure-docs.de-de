---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0fba49ec5a610d03ece2dce7fa5bb0d9d094ab4c
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203165"
---
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Funktionen-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` erstellt.  
Falls Sie nicht Cloud Shell verwenden, melden Sie sich erst mithilfe von `az login` an.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Im Allgemeinen erstellen Sie Ressourcengruppen und Ressourcen in einer [Region](https://azure.microsoft.com/global-infrastructure/regions/) in Ihrer Nähe. 
