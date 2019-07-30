---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 6d6451d50a00569eb1da8f5b0a0dc10d3c6b1115
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841542"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Konfigurieren einer Umgebungsvariablen für die Authentifizierung

Für Anwendungen muss der Zugriff auf die von ihnen genutzten Cognitive Services authentifiziert werden. Für die Authentifizierung empfehlen wir die Erstellung einer Umgebungsvariable zum Speichern der Schlüssel in Ihren Azure-Ressourcen. 

Wenn Sie Ihren Schlüssel vorliegen haben, können Sie ihn in eine neue Umgebungsvariable auf dem lokalen Computer schreiben, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Festlegen der Umgebungsvariablen ein Konsolenfenster, und befolgen Sie die Anleitung für Ihr Betriebssystem. Ersetzen Sie `your-key` durch einen der Schlüssel für Ihre Ressource.

* Windows

    ```console
    setx COGNITIVE_SERVICE_KEY "your-key"
    ```

    Nachdem Sie die Umgebungsvariable hinzugefügt haben, müssen Sie unter Umständen alle ausgeführten Programme neu starten, von denen die Umgebungsvariable gelesen werden muss, z.B. das Konsolenfenster. Wenn Sie beispielsweise Visual Studio als Editor verwenden, müssen Sie Visual Studio neu starten, bevor Sie das Beispiel ausführen.

* Linux
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.
    
* macOS
    
    Bearbeiten Sie Ihr „.bash_profile“, und fügen Sie die Umgebungsvariable hinzu:
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.
