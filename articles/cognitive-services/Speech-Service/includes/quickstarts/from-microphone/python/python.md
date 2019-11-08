---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, Python – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie eine Konsolenanwendung zur Spracherkennung erstellen, die das Speech SDK für Python verwendet. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 7fe11876a7ba20981f784bc41ce8094178591371
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505771"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](../../../../quickstarts/create-luis.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md)

## <a name="support-and-updates"></a>Support und Updates

Updates für das Speech SDK-Paket für Python werden über PyPI verteilt und in den [Versionshinweisen](~/articles/cognitive-services/Speech-Service/releasenotes.md) angekündigt.
Wenn eine neue Version verfügbar ist, können Sie mit dem Befehl `pip install --upgrade azure-cognitiveservices-speech` auf diese Version aktualisieren.
Die derzeit installierte Version können Sie der Variablen `azure.cognitiveservices.speech.__version__` entnehmen.

Falls Probleme auftreten oder ein Feature fehlt, sehen Sie sich die [Support- und Hilfeoptionen](~/articles/cognitive-services/Speech-Service/support.md) an.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Erstellen einer Python-Anwendung, die das Speech SDK verwendet

### <a name="run-the-sample"></a>Ausführen des Beispiels

Sie können den [Beispielcode](#sample-code) aus diesem Schnellstart in eine Quelldatei (`quickstart.py`) kopieren und in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in der Konsole ausführen:

```sh
python quickstart.py
```

Alternativ können Sie dieses Schnellstarttutorial als [Jupyter](https://jupyter.org) Notebook aus dem [Beispielrepository für das Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) herunterladen und als Notebook ausführen.

### <a name="sample-code"></a>Beispielcode

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

1. Laden Sie eine 64-Bit-Version (3.5 oder höher) von [Python](https://www.python.org/downloads/) herunter, und installieren Sie sie auf Ihrem Computer.
1. Laden Sie [Visual Studio Code](https://code.visualstudio.com/Download) herunter, und installieren Sie die Software.
1. Öffnen Sie Visual Studio Code, und installieren Sie die Python-Erweiterung. Wählen Sie im Menü **Datei** > **Einstellungen** > **Erweiterungen**. Suchen Sie nach **Python**.

   ![Installieren der Python-Erweiterung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Erstellen Sie einen Ordner zum Speichern des Projekts. Ein Beispiel ist die Verwendung von Windows-Explorer.
1. Wählen Sie in Visual Studio Code das Symbol **Datei**. Öffnen Sie dann den von Ihnen erstellten Ordner.

   ![Öffnen eines Ordners](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Erstellen Sie eine neue Python-Quelldatei (`speechsdk.py`), indem Sie das Symbol „Neue Datei“ auswählen.

   ![Erstellen von Dateien](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. Kopieren Sie den [Python-Code](#sample-code), fügen Sie ihn in die neu erstellte Datei ein, und speichern Sie die Datei.
1. Fügen Sie Ihre Abonnementinformationen für die Speech-Dienste ein.
1. Wenn ein Python-Interpreter ausgewählt wurde, wird er auf der linken Seite der Statusleiste am unteren Rand des Fensters angezeigt.
   Zeigen Sie andernfalls eine Liste der verfügbaren Python-Interpreter an. Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **Python: Select Interpreter** ein. Wählen Sie einen passenden Interpreter aus.
1. Sie können das Speech SDK-Paket für Python über Visual Studio Code installieren. Führen Sie diesen Schritt aus, wenn es für den ausgewählten Python-Interpreter noch nicht installiert wurde.
   Öffnen Sie zum Installieren des Speech SDK-Pakets ein Terminal. Rufen Sie erneut die Befehlspalette (STRG+UMSCHALT+P) auf, und geben Sie **Terminal: Neues integriertes Terminal erstellen** eingeben.
   Geben Sie im Terminal, das geöffnet wird, den Befehl `python -m pip install azure-cognitiveservices-speech` bzw. den entsprechenden Befehl für Ihr System ein.
1. Klicken Sie zum Ausführen des Beispielcodes mit der rechten Maustaste an einer beliebigen Stelle im Editor. Wählen Sie **Run Python File in Terminal** (Python-Datei im Terminal ausführen) aus.
   Sagen Sie bei entsprechender Aufforderung einige Wörter. Der transkribierte Text wird kurz danach angezeigt.

   ![Ausführen eines Beispiels](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run.png)

Falls Sie Probleme beim Ausführen dieser Anweisungen haben, können Sie das ausführlichere [Visual Studio Code-Tutorial für Python](https://code.visualstudio.com/docs/python/python-tutorial) zurate ziehen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
