---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 25cfcefb600bc12de3dad5b6fe2bcb76d00f0198
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "68444120"
---
## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Functions verwendet ein allgemeines Konto in Azure Storage, um den Zustand und andere Informationen über Ihre Funktionen zu verwalten. Erstellen Sie mit dem Befehl [az storage account create](/cli/azure/storage/account) ein allgemeines Storage-Konto in der erstellten Ressourcengruppe.

Ersetzen Sie im folgenden Befehl den Platzhalter `<storage_name>` durch einen eindeutigen Speicherkontonamen. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```
