---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d62da82b4a4dd35532dd8776a9111689db469201
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448365"
---
## <a name="register-extensions"></a>Registrieren von Erweiterungen

Mit Ausnahme von HTTP- und Zeitauslösern werden Functions-Bindungen in der Laufzeitversion 2.x als Erweiterungspakete implementiert. In Version 2.x der Azure Functions Runtime müssen Sie die in Ihren Funktionen für die Bindungstypen verwendeten Erweiterungen explizit registrieren. Die Ausnahmen hierbei sind HTTP-Bindungen und Zeitauslöser, die keine Erweiterungen erfordern.

Sie können auch Bindungserweiterungen einzeln installieren, oder Sie können einen Erweiterungsbündelverweis in der Datei „host.json“ hinzufügen. Erweiterungsbündel wirken eventuellen Kompatibilitätsproblemen bei Paketen entgegen, wenn mehrere Bindungstypen verwendet werden. Es handelt sich hierbei um den empfohlenen Ansatz zum Registrieren von Bindungserweiterungen. Erweiterungsbündel beseitigen außerdem die Notwendigkeit der Installation des .NET Core 2.x SDK. 

### <a name="extension-bundles"></a>Erweiterungsbündel

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Weitere Informationen finden Sie unter [Registrieren von Bindungserweiterungen von Azure Functions](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Sie sollten Erweiterungsbündel in de Datei „host.json“ hinzufügen, bevor Sie der Datei „functions.json“ Bindungen hinzufügen.

### <a name="register-individual-extensions"></a>Registrieren einzelner Erweiterungen

Wenn Sie Erweiterungen installieren müssen, die nicht in einem Bündel enthalten sind, können Sie einzelne Erweiterungspakete für bestimmte Bindungen manuell registrieren. 

> [!NOTE]
> Um Erweiterungen mithilfe von `func extensions install` manuell zu registrieren, müssen Sie das .NET Core 2.x SDK installiert haben.

Nachdem Sie Ihre Datei *function.json* aktualisiert haben, sodass sie alle von der Funktion benötigten Bindungen einschließt, führen Sie den folgenden Befehl im Projektordner aus.

```bash
func extensions install
```

Der Befehl liest die Datei *function.json*, um zu ermitteln, welche Pakete Sie benötigen, installiert diese und erzeugt das Erweiterungenprojekt neu. Er fügt alle neuen Bindungen an die aktuelle Version hinzu, aktualisiert aber keine vorhandenen Bindungen. Verwenden Sie die Option `--force`, um vorhandene Bindungen beim Installieren neuer auf die neueste Version zu aktualisieren.