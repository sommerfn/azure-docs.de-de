---
title: 'Azure-Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe der Azure CLI | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Azure CLI verwenden, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: 1f3143eced90f97c090c0005375ef50fe48c5f5f
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747917"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-azure-cli"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs über die Azure CLI

Die Azure CLI ist die Befehlszeilenumgebung von Azure und dient zum Verwalten von Azure-Ressourcen. Sie können sie in Ihrem Browser mit Azure Cloud Shell verwenden. Außerdem können Sie sie unter macOS, Linux oder Windows installieren und über die Befehlszeile ausführen. In diesem Schnellstart wird beschrieben, wie Sie die Azure CLI zum Hoch- und Herunterladen von Daten für Azure Blob Storage verwenden.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um Ihre Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Blobs werden immer in einen Container hochgeladen. Sie können Gruppen von Blobs ähnlich wie Dateien in Ordnern auf Ihrem Computer organisieren.

Erstellen Sie mit dem Befehl [az storage container create](/cli/azure/storage/container) einen Container zum Speichern von Blobs.

```azurecli-interactive
az storage container create --name sample-container
```

## <a name="upload-a-blob"></a>Hochladen eines Blobs

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Die Beispiele in dieser Schnellstartanleitung zeigen, wie Sie Blockblobs verwenden.

Erstellen Sie zunächst eine Datei für den Upload in ein Blockblob. Verwenden Sie bei Verwendung der Azure Cloud Shell den folgenden Befehl, um eine Datei zu erstellen:

```bash
vi helloworld
```

Wenn die Datei geöffnet wird, drücken Sie **Einfg**. Geben Sie *Hello World* ein und drücken Sie dann **Esc**. Geben Sie danach *:x*ein und drücken Sie dann die **Eingabetaste**.

In diesem Beispiel laden Sie mithilfe des Befehls [az storage blob upload](/cli/azure/storage/blob) ein Blob in den Container hoch, den Sie im letzten Schritt erstellt haben. Es ist nicht erforderlich, einen Dateipfad anzugeben, da die Datei im Stammverzeichnis erstellt wurde:

```azurecli-interactive
az storage blob upload \
    --container-name sample-container \
    --name helloworld \
    --file helloworld
```

Dabei wird das Blob erstellt, falls es nicht vorhanden ist, oder überschrieben, falls es bereits vorhanden ist. Laden Sie beliebig viele Dateien hoch, bevor Sie fortfahren.

Wenn Sie mehrere Dateien gleichzeitig hochladen möchten, können Sie den Befehl [az storage blob upload-batch](/cli/azure/storage/blob) verwenden.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Sie können die Blobs im Container mit dem Befehl [az storage blob list](/cli/azure/storage/blob) auflisten.

```azurecli-interactive
az storage blob list \
    --container-name sample-container \
    --output table
```

## <a name="download-a-blob"></a>Herunterladen eines Blobs

Mit dem Befehl [az storage blob download](/cli/azure/storage/blob) können Sie das zuvor hochgeladene Blob herunterladen.

```azurecli-interactive
az storage blob download \
    --container-name sample-container \
    --name helloworld \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Übertragen von Daten mit AzCopy

Das Dienstprogramm [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ist eine weitere Option für eine leistungsstarke, skriptfähige Datenübertragung für Azure Storage. Sie können AzCopy zum Übertragen von Daten in und aus Blob-, Datei- und Tabellenspeicher verwenden.

Im folgenden Beispiel wird AzCopy verwendet, um eine Datei mit dem Namen *myfile.txt* in den Container*sample-container* hochzuladen. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://<account-name>.blob.core.windows.net/sample-container \
    --dest-key <account-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in der Ressourcengruppe, einschließlich des in dieser Schnellstartanleitung erstellten Speicherkontos, nicht mehr benötigen, löschen Sie die Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group). Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az group delete --name <resource-group-name>
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie Dateien zwischen einem lokalen Dateisystem und einem Container in Azure Blobspeicher übertragen. Weitere Informationen zum Arbeiten mit Blobs in Azure Storage finden Sie im Tutorial zum Arbeiten mit Azure Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge mit der Azure-Befehlszeilenschnittstelle](storage-how-to-use-blobs-cli.md)
