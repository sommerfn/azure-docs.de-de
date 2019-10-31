---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: abc8727c2a9ad7bb6621b8c8e019e14fb8cdec97
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529834"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Herunterladen und Installieren der Istio-Clientbinärdatei „istioctl“

Verwenden Sie in einer Bash-basierten Shell unter Linux oder dem [Windows-Subsystem für Linux][install-wsl] `curl`, um das Istio-Release herunterzuladen, und extrahieren Sie dieses dann wie folgt mit `tar`:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.3.2

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

Die Clientbinärdatei `istioctl` wird auf dem Clientcomputer ausgeführt und ermöglicht die Interaktion mit dem Istio-Dienstmesh. Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer Bash-basierten Shell unter Linux oder [Windows-Subsystem für Linux][install-wsl] zu installieren. Diese Befehle kopieren die `istioctl`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Wenn Sie die Befehlszeilen-Vervollständigung für die Istio-Clientbinärdatei `istioctl` wünschen, richten Sie diese wie folgt ein:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

<!-- LINKS - external -->
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10