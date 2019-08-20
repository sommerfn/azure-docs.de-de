---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/05/2019
ms.author: glenga
ms.openlocfilehash: c90587ebd7b0a608e41f19ab600f82dc484bad0a
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949929"
---
## <a name="create-the-local-function-app-project"></a>Erstellen des lokalen Funktions-App-Projekts

Führen Sie den folgenden Befehl zum Erstellen eines Funktions-App-Projekts im `MyFunctionProj`-Ordner des aktuellen lokalen Verzeichnisses über die Befehlszeile aus. Ein GitHub-Repository wird ebenfalls in `MyFunctionProj` erstellt.

```command
func init MyFunctionProj
```

Wählen Sie bei entsprechender Aufforderung eine Workerruntime aus den folgenden Sprachoptionen aus:

+ `dotnet`: Erstellt ein .NET-Klassenbibliotheksprojekt (.csproj).
+ `node`: Erstellt ein auf Node.js basierendes Projekt. Wählen Sie `javascript` oder `typescript` aus. 
+ `python`: Lesen Sie für ein Python-Projekt stattdessen den Artikel [Erstellen einer durch HTTP ausgelösten Funktion in Azure](../articles/azure-functions/functions-create-first-function-python.md).
+ `powershell`: Lesen Sie für ein PowerShell-Projekt stattdessen den Artikel [Erstellen Ihrer ersten PowerShell-Funktion in Azure](../articles/azure-functions/functions-create-first-function-powershell.md). 


Nachdem das Projekt erstellt wurde, verwenden Sie den folgenden Befehl, um zu dem neuen Projektordner `MyFunctionProj` zu navigieren.

```command
cd MyFunctionProj
```
