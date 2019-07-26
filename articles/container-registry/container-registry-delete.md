---
title: Löschen von Imageressourcen in Azure Container Registry
description: Erfahren Sie mehr über die effektive Verwaltung der Registrierungsgröße durch das Löschen von Containerimagedaten.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 06/17/2019
ms.author: danlep
ms.openlocfilehash: eaf3b3e591ca2ddbd29fd5547d334ef90b24fc5e
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309641"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Löschen von Containerimages in Azure Container Registry

Damit Ihre Azure-Containerregistrierung nicht zu groß wird, sollten Sie regelmäßig veraltete Imagedaten löschen. Während einige Containerimages, die in der Produktion bereitgestellt werden, eine längerfristige Speicherung erfordern, können andere in der Regel schneller gelöscht werden. In einem automatisierten Build- und Testszenario kann sich Ihre Registrierung beispielsweise schnell mit Images füllen, die möglicherweise nie bereitgestellt werden und kurz nach Abschluss der Build- und Test-Phase gelöscht werden können.

Da Sie Imagedaten auf verschiedenen Wegen löschen können, müssen Sie wissen, wie sich jeder Löschvorgang auf die Speicherauslastung auswirkt. In diesem Artikel werden verschiedene Methoden zum Löschen von Imagedaten behandelt:

* Löschen eines [Repositorys](#delete-repository): Löscht alle Images und alle eindeutigen Ebenen innerhalb des Repositorys.
* Löschen nach [Tag](#delete-by-tag): Löscht ein Image, das Tag, alle eindeutigen Ebenen, die auf das Image verweisen, und alle anderen Tags, die dem Image zugeordnet sind.
* Löschen nach [Manifest-Digest](#delete-by-manifest-digest): Löscht ein Image, alle eindeutigen Ebenen, die auf das Image verweisen, und alle Tags, die dem Image zugeordnet sind.

Zum Automatisieren von Löschvorgängen sind Beispielskripts verfügbar.

Eine Einführung zu diesen Konzepten finden Sie unter [Informationen zu Registrierungen, Repositorys und Images.](container-registry-concepts.md)

## <a name="delete-repository"></a>Löschen des Repositorys

Beim Löschen eines Repositorys werden alle Images im Repository einschließlich aller Tags, eindeutigen Ebenen und Manifeste gelöscht. Wenn Sie ein Repository löschen, stellen Sie den von in diesem Repository auf eindeutige Ebenen verweisende Images belegten Speicherplatz wieder her.

Der folgende Azure CLI-Befehl löscht das Repository „acr-helloworld“ und alle Tags und Manifeste innerhalb des Repositorys. Wenn auf die Ebenen, auf die die gelöschten Manifeste verweisen, nicht von anderen Images in der Registrierung verwiesen wird, werden deren Ebenendaten ebenfalls gelöscht, um den Speicherplatz wiederherzustellen.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Löschen nach Tag

Sie können einzelne Bilder aus einem Repository löschen, indem Sie den Namen und das Tag des Repositorys im Löschvorgang angeben. Wenn Sie nach Tag löschen, stellen Sie den Speicherplatz wieder her, der durch eindeutige Ebenen im Image (Ebenen, die nicht von anderen Images in der Registrierung freigegeben wurden) belegt war.

Verwenden Sie zum Löschen nach Tag [az acr repository delete][az-acr-repository-delete], und legen Sie den Imagenamen im Parameter `--image` fest. Alle eindeutigen Ebenen des Images und alle zugehörigen Tags werden gelöscht.

Beispiel: Löschen des Images „acr-helloworld:latest“ aus der Registrierung „myregistry“:

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Das Löschen *nach Tag* darf nicht mit dem Löschen (Aufheben) eines Tags verwechselt werden. Sie können ein Tag mit dem Azure CLI-Befehl [az acr repository untag][az-acr-repository-untag] löschen. Beim Aufheben von Tags wird kein Speicherplatz freigegeben, da die zugehörigen [Manifest](container-registry-concepts.md#manifest)- und Ebenendaten in der Registrierung verbleiben. Nur der Tag-Verweis wird gelöscht.

## <a name="delete-by-manifest-digest"></a>Löschen nach Manifest-Digest

Ein [Manifest Digest](container-registry-concepts.md#manifest-digest) kann einem, aber nicht mehrere Tags zugeordnet werden. Beim Löschen nach Digest werden alle vom Manifest zugeordneten Tags gelöscht, da Ebenendaten für alle Ebenen für das Bild eindeutig sind. Freigegebene Ebenendaten werden nicht gelöscht.

Zum Löschen nach Digest listen Sie zuerst die Manifest-Digests für das Repository mit den zu löschenden Bildern auf. Beispiel:

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  }
]
```

Legen Sie als Nächstes den zu löschenden Digest im Befehl [az acr repository delete][az-acr-repository-delete] fest. Das Format des Befehls lautet:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

So löschen Sie beispielsweise das letzte Manifest, das in der vorherigen Ausgabe (mit dem Tag „v2“):

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

Das Image `acr-helloworld:v2` und alle eindeutigen Ebenendaten dieses Images werden aus der Registrierung gelöscht. Wenn ein Manifest mehreren Tags zugeordnet ist, werden alle zugeordneten Tags ebenfalls gelöscht.

## <a name="delete-digests-by-timestamp"></a>Löschen von Hashes nach Zeitstempel

Zum Verwalten der Größe eines Repositorys oder der Registrierung müssen Sie ggf. in regelmäßigen Abständen Manifesthashes löschen, die älter sind als ein bestimmtes Datum.

Der folgende Azure CLI-Befehl listet alle Manifesthashes in einem Repository, die älter als ein angegebener Zeitstempel sind, in aufsteigender Reihenfolge auf. Ersetzen Sie `<acrName>` und `<repositoryName>` durch entsprechende Werte für Ihre Umgebung. Bei dem Zeitstempel kann es sich um einen vollständigen Datums-/Uhrzeitausdruck oder wie im folgenden Beispiel um ein Datum handeln.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

Nachdem Sie die veralteten Manifesthashes identifiziert haben, können Sie das folgende Bash-Skript ausführen, um Manifesthashes zu löschen, die älter als ein angegebener Zeitstempel sind. Dieses Skript erfordert die Azure CLI und **xargs**. Standardmäßig führt das Skript nicht keine Löschen aus. Ändern Sie den `ENABLE_DELETE`-Wert in `true`, um das Löschen von Images zu gestatten.

> [!WARNING]
> Verwenden Sie die folgenden Beispielskripts mit Vorsicht: Gelöschte Imagedaten sind NICHT WIEDERHERSTELLBAR. Wenn Sie Systeme haben, die Images nach dem Manifesthash pullen (und nicht nach dem Imagenamen), sollten Sie diese Skripts nicht ausführen. Wenn Sie Manifesthashes löschen, können diese Systeme die Images nicht aus Ihrer Registrierung pullen. Erwägen Sie statt des Pullens nach Manifest die Einführung eines *eindeutigen Tagging*-Schemas. Dies ist eine [bewährte Methode][tagging-best-practices]. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Löschen von Images ohne Tags

Wie im Abschnitt [Manifest-Digest](container-registry-concepts.md#manifest-digest) werden beim Pushen eines veränderten Images mit vorhandenen Tags die **Tags des zuvor per Push übertragenen Images aufgehoben**. Die Folge ist ein verwaistes Image. Das Manifest des zuvor per Push übertragenen Images und dessen Ebenendaten verbleiben in der Registrierung. Gehen Sie dabei vom folgenden Ereignisablauf aus:

1. Pushen Sie das Image *acr-helloworld* mit dem Tag **latest** (neueste):`docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Überprüfen Sie die Manifeste für das Repository *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Verändern der Dockerfile *acr-helloworld*
1. Pushen Sie das Image *acr-helloworld* mit dem Tag **latest** (neueste):`docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Überprüfen Sie die Manifeste für das Repository *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Wie Sie in der Ausgabe des letzten Schritts in der Sequenz sehen können, ist nun ein verwaistes Manifest vorhanden, dessen `"tags"`-Eigenschaft eine leere Liste ist. Dieses Manifest ist weiterhin in der Registrierung vorhanden, zusammen mit eindeutigen Datenebenen, auf die es verweist. **Um solche verwaisten Images und die zugehörigen Ebenendaten zu löschen, müssen Sie nach Manifest-Digest löschen**.

## <a name="delete-all-untagged-images"></a>Löschen aller Images ohne Tags

Sie können alle nicht markierten Images in Ihrem Repository mit dem folgenden Azure-CLI-Befehl auflisten. Ersetzen Sie `<acrName>` und `<repositoryName>` durch entsprechende Werte für Ihre Umgebung.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

Mit diesem Befehl können Sie in einem Skript alle nicht in einem Repository markierten Images löschen.

> [!WARNING]
> Verwenden Sie die folgenden Beispielskripts mit Vorsicht – gelöschte Imagedaten sind NICHT WIEDERHERSTELLBAR. Wenn Sie Systeme haben, die Images nach dem Manifesthash pullen (und nicht nach dem Imagenamen), sollten Sie diese Skripts nicht ausführen. Wenn Sie Images ohne Tags löschen, hindern Sie diese Systeme daran, die Images aus Ihrer Registrierung zu löschen. Erwägen Sie statt des Pullens nach Manifest die Einführung eines *eindeutigen Tagging*-Schemas. Dies ist eine [bewährte Methode][tagging-best-practices].

**Azure CLI in Bash**

Mit dem folgenden Bash-Skript werden alle Images ohne Tag aus einem Repository gelöscht. Dieses Skript erfordert die Azure CLI und **xargs**. Standardmäßig führt das Skript nicht keine Löschen aus. Ändern Sie den `ENABLE_DELETE`-Wert in `true`, um das Löschen von Images zu gestatten.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable image deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY --query "[?tags[0]==null]" -o tsv
fi
```

**Azure CLI in PowerShell**

Mit dem folgenden PowerShell-Skript werden alle Images ohne Tags aus einem Repository gelöscht. Dazu sind PowerShell und die Azure CLI erforderlich. Standardmäßig führt das Skript nicht keine Löschen aus. Ändern Sie den `$enableDelete`-Wert in `$TRUE`, um das Löschen von Images zu gestatten.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted."
    Write-Host "Set `$enableDelete = `$TRUE to enable image deletion."
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null]" -o tsv
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Speichern von Images in Azure Container Registry finden Sie unter [Speichern von Containerimages in Azure Container Registry](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
