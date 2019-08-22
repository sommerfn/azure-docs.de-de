---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Azure Container Instances-Ressource über die Azure CLI erstellen können.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 2080d283c6cb7466dcb4847a81d76a4c3109217a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012142"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Erstellen einer Azure Container Instances-Ressource über die Azure CLI

Im folgenden YAML-Code ist die Azure Container Instances-Ressource definiert. Kopieren Sie den Inhalt, und fügen Sie ihn in eine neue Datei mit dem Namen `my-aci.yaml` ein. Ersetzen Sie dann die kommentierten Werte durch Ihre eigenen. Gültigen YAML-Code finden Sie im [Vorlagenformat][template-format]. Die verfügbaren Imagenamen und entsprechenden Repositorys finden Sie unter [Containerrepositorys und -images][repositories-and-images].

```YAML
apiVersion: 2018-10-01
location: # < Valid location >
name: # < Container Group name >
imageRegistryCredentials:
  - server: containerpreview.azurecr.io
    username: # < The username for the preview container registry >
    password: # < The password for the preview container registry >
properties:
  containers:
  - name: # < Container name >
    properties:
      image: # < Repository/Image name >
      environmentVariables: # These env vars are required
        - name: eula
          value: accept
        - name: billing
          value: # < Service specific Endpoint URL >
        - name: apikey
          value: # < Service specific API key >
      resources:
        requests:
          cpu: 4 # Always refer to recommended minimal resources
          memoryInGb: 8 # Always refer to recommended minimal resources
      ports:
        - port: 5000
  osType: Linux
  restartPolicy: OnFailure
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 5000
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

> [!NOTE]
> Nicht alle Speicherorte weisen die gleiche Verfügbarkeit von CPU und Arbeitsspeicher auf. Eine Liste der verfügbaren Ressourcen für Container nach Speicherort und Betriebssystem finden Sie in der Tabelle [Speicherort und Ressourcen][location-to-resource].

Wir verwenden die YAML-Datei, die für den Befehl [`az container create`][azure-container-create] erstellt wurde. Führen Sie über die Azure CLI den Befehl `az container create` aus, und ersetzen Sie `<resource-group>` durch Ihre eigene Ressourcengruppe. Informationen zum Sichern der Werte in einer YAML-Bereitstellung finden Sie unter [Sichere Werte][secure-values].

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

Bei Gültigkeit lautet die Ausgabe des Befehls `Running...`.Nach einiger Zeit ändert sich die Ausgabe in eine JSON-Zeichenfolge, die die neu erstellte ACI-Ressource darstellt. Das Containerimage ist höchstwahrscheinlich für eine Weile nicht verfügbar, aber die Ressource wird nun bereitgestellt.

> [!TIP]
> Achten Sie genau auf die Speicherorte von Azure Cognitive Service-Angeboten in der öffentlichen Vorschau, da der YAML-Code entsprechend dem Speicherort angepasst werden muss.

[azure-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[template-format]: https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format

[repositories-and-images]: ../../cognitive-services-container-support.md#container-repositories-and-images
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md#availability---general
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values
