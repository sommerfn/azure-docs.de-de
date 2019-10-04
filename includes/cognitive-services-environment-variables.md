---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 367ae27a6afec803d2e3f98f54bdcf852330ddc6
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274692"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Konfigurieren einer Umgebungsvariablen für die Authentifizierung

Für Anwendungen muss der Zugriff auf die von ihnen genutzten Cognitive Services authentifiziert werden. Für die Authentifizierung empfehlen wir die Erstellung einer Umgebungsvariable zum Speichern der Schlüssel in Ihren Azure-Ressourcen. 

Wenn Sie Ihren Schlüssel vorliegen haben, können Sie ihn in eine neue Umgebungsvariable auf dem lokalen Computer schreiben, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Festlegen der Umgebungsvariablen ein Konsolenfenster, und befolgen Sie die Anleitung für Ihr Betriebssystem. Ersetzen Sie `your-key` durch einen der Schlüssel für Ihre Ressource.

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx COGNITIVE_SERVICE_KEY "your-key"
```

Nachdem Sie die Umgebungsvariable hinzugefügt haben, müssen Sie unter Umständen alle ausgeführten Programme neu starten, von denen die Umgebungsvariable gelesen werden muss, z.B. das Konsolenfenster. Wenn Sie beispielsweise Visual Studio als Editor verwenden, müssen Sie Visual Studio neu starten, bevor Sie das Beispiel ausführen.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Bearbeiten Sie Ihr „.bash_profile“, und fügen Sie die Umgebungsvariable hinzu:

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.

***