---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 8b1a4a8a6c808348ca17a9eebac17c5821ceefe5
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529734"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Herunterladen und Installieren der Linkerd-Clientbinärdatei „linkerd“

Verwenden Sie in einer Bash-basierten Shell unter macOS `curl` wie folgt, um das Linkerd-Release herunterzuladen:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-darwin"
```

Die Clientbinärdatei `linkerd` wird auf dem Clientcomputer ausgeführt und ermöglicht die Interaktion mit dem Linkerd-Dienstnetz. Verwenden Sie die folgenden Befehle, um die Linkerd-Clientbinärdatei `linkerd` in einer Bash-basierten Shell unter macOS zu installieren. Diese Befehle kopieren die `linkerd`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-linux /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Wenn Sie die Befehlszeilen-Vervollständigung für die Linkerd-Clientbinärdatei `linkerd` nutzen möchten, können Sie dies wie folgt einrichten:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```