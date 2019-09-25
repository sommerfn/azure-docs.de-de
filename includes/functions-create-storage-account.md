---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 4dd43c5bcc5a0e9a734db4ca9a4b3d7137f85250
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203175"
---
## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Functions verwendet ein allgemeines Konto in Azure Storage, um den Zustand und andere Informationen über Ihre Funktionen zu verwalten. Erstellen Sie mit dem Befehl [az storage account create](/cli/azure/storage/account#az-storage-account-create) ein allgemeines Storage-Konto in der erstellten Ressourcengruppe.

Ersetzen Sie im folgenden Befehl den Platzhalter `<storage_name>` durch einen eindeutigen Speicherkontonamen. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```
