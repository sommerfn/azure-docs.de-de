---
title: include file
description: include file
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 5cca18b881250ce99df35d681bec6091ea4a27b9
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641531"
---
In der Befehlsausgabe zeigt der Abschnitt `identity` an, dass eine Identität vom Typ `SystemAssigned` in der Aufgabe festgelegt ist. Die `principalId` sollte die Dienstprinzipal-ID der Identität sein:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Verwenden Sie den Befehl [az acr task show][az-acr-task-show], um die PrinzipalId in einer Variablen zu speichern und in späteren Befehlen zu verwenden:

```azurecli
principalID=$(az acr task show --name dockerhubtask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
