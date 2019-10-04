---
title: include file
description: include file
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: aad66a91f7de8380ac7e87f0ce8e35ed43cac4a6
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594533"
---
Der Code für alle Funktionen in einer bestimmten Funktions-App befindet sich in einem Stammprojektordner, der eine Hostkonfigurationsdatei und mindestens einen Unterordner enthält. Jeder Unterordner enthält den Code für eine separate Funktion. Die folgende Darstellung zeigt die Ordnerstruktur:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

In der Version 2.x der Functions-Runtime müssen sich alle Funktionen in der Funktions-App denselben Sprachstapel teilen.  

Die Datei [host.json](../articles/azure-functions/functions-host-json.md) enthält die runtimespezifische Konfiguration und befindet sich im Stammordner der Funktions-App. Ein Ordner *bin* enthält Pakete und andere Bibliotheksdateien, die von der Funktions-App benötigt werden. Sprachspezifische Anforderungen für ein Funktions-App-Projekt:

* [C#-Klassenbibliothek (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C#-Skript (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



