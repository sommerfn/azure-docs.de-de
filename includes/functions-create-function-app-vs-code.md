---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: f9b853f0e86fc298ab35421928492ba813d85827
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444628"
---
## <a name="create-an-azure-functions-project"></a>Erstellen Ihres Functions-Projekts mit einer Funktion 

Mit der Azure Functions-Projektvorlage in Visual Studio Code wird ein Projekt erstellt, das in einer Funktions-App in Azure veröffentlicht werden kann. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu ermöglichen.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Create new project...`, und wählen Sie ihn aus.

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen ein:

    | Prompt | Wert | BESCHREIBUNG |
    | ------ | ----- | ----------- |
    | Auswählen einer Sprache für Ihr Funktions-App-Projekt | C# oder JavaScript | Dieser Artikel unterstützt C# und JavaScript. Lesen Sie für Python [diesen Python-Artikel](https://code.visualstudio.com/docs/python/tutorial-azure-functions) und für PowerShell [diesen PowerShell-Artikel](../articles/azure-functions/functions-create-first-function-powershell.md).  |
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | Erstellen Sie eine durch HTTP ausgelöste Funktion in der neuen Funktions-App. |
    | Angeben eines Funktionsnamens | HTTPTrigger | Drücken Sie die EINGABETASTE, um den Standardnamen zu verwenden. |
    | Angeben eines Namespaces | My.Functions | (Nur C#) C#-Klassenbibliotheken müssen einen Namespace haben.  |
    | Autorisierungsstufe | Funktion | Erfordert einen [Funktionsschlüssel](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys), um den HTTP-Endpunkt der Funktion aufzurufen. |
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich | Erstellt die Funktions-App im aktuellen Arbeitsbereich. |

Visual Studio Code erstellt das Funktions-App-Projekt in einem neuen Arbeitsbereich. Dieses Projekt enthält die Konfigurationsdateien [host.json](../articles/azure-functions/functions-host-json.md) und [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file) sowie sprachspezifische Projektdateien. 

Eine neue HTTP-gesteuerte Funktion wird ebenfalls im HttpTrigger-Ordner des Funktions-App-Projekts erstellt.