---
title: Aktualisieren von Containern in Azure Container Instances
description: Hier erfahren Sie, wie Sie ausgeführte Container in Ihren Azure Container Instances-Containergruppen aktualisieren.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 09/03/2019
ms.author: danlep
ms.openlocfilehash: 3103fe7fbf7dcd587f43b673ef53f32893908ecb
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307712"
---
# <a name="update-containers-in-azure-container-instances"></a>Aktualisieren von Containern in Azure Container Instances

Während des normalen Betriebs Ihrer Containerinstanzen ist es unter Umständen erforderlich, die in einer [Containergruppe](container-instances-container-groups.md) ausgeführten Container zu aktualisieren. Sie möchten beispielsweise die Imageversion aktualisieren, einen DNS-Namen ändern, Umgebungsvariablen aktualisieren oder den Container aktualisieren, dessen Anwendung abgestürzt ist.

> [!NOTE]
> Beendete oder gelöschte Containergruppen können nicht aktualisiert werden. Nachdem eine Containergruppe beendet (Erfolgs- oder Fehlerstatus) oder gelöscht wurde, muss die Gruppe als neue Gruppe bereitgestellt werden.

## <a name="update-a-container-group"></a>Aktualisieren einer Containergruppe

Aktualisieren Sie die in einer Containergruppe ausgeführten Container, indem Sie eine vorhandene Gruppe mit mindestens einer geänderten Eigenschaft erneut bereitstellen. Wenn Sie eine Containergruppe aktualisieren, werden alle ausgeführten Container in der Gruppe direkt neu gestartet. Dies erfolgt in der Regel auf demselben zugrunde liegenden Containerhost.

Stellen Sie mit dem Befehl „create“ (oder über das Azure-Portal) eine vorhandene Containergruppe erneut bereit. Geben Sie dabei den Namen einer vorhandenen Gruppe an. Ändern Sie mindestens eine gültige Eigenschaft der Gruppe, wenn Sie den Befehl „create“ ausgeben, um die erneute Bereitstellung zu initiieren, und lassen Sie die übrigen Eigenschaften unverändert (oder verwenden Sie weiterhin die Standardwerte). Nicht alle Eigenschaften der Containergruppe sind für eine erneute Bereitstellung zulässig. Unter [Eigenschaften, für die eine Löschung der Container erforderlich ist](#properties-that-require-container-delete) finden Sie eine Liste der nicht unterstützten Eigenschaften.

Das folgende Azure CLI-Beispiel aktualisiert eine Containergruppe mit einer neuen DNS-Namensbezeichnung. Da die Eigenschaft der DNS-Namensbezeichnung der Gruppe geändert werden kann, wird die Containergruppe erneut bereitgestellt, und ihre Container werden neu gestartet.

Erste Bereitstellung mit der DNS-Namensbezeichnung *myapplication-staging*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Aktualisieren Sie die Containergruppe mit der neuen DNS-Namensbezeichnung *myapplication*, und belassen Sie die übrigen Eigenschaften unverändert:

```azurecli-interactive
# Update DNS name label (restarts container), leave other properties unchanged
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Vorteile der Aktualisierung

Der Hauptvorteil der Aktualisierung einer vorhandenen Containergruppe ist die schnellere Bereitstellung. Wenn Sie eine bereits vorhandene Containergruppe erneut bereitstellen, werden die Containerimageebenen aus der Zwischenspeicherung der vorherigen Bereitstellung abgerufen. Statt wie bei neuen Bereitstellungen alle Imageebenen direkt aus der Registrierung abzurufen, werden nur geänderte Ebenen (sofern vorhanden) abgerufen.

Bei Anwendungen, die auf größeren Containerimages wie Windows Server Core basieren, wird die Bereitstellung erheblich beschleunigt, wenn Sie Container aktualisieren, statt sie zu löschen und neu bereitzustellen.

## <a name="limitations"></a>Einschränkungen

Nicht alle Eigenschaften einer Containergruppe unterstützen die Aktualisierung. Bei einigen Eigenschaften einer Containergruppe müssen Sie zum Ändern die Gruppe zuerst löschen und anschließend erneut bereitstellen. Weitere Informationen finden Sie unter [Eigenschaften, für die eine Löschung der Container erforderlich ist](#properties-that-require-container-delete).

Alle Container in einer Containergruppe werden neu gestartet, wenn Sie die Containergruppe aktualisieren. Sie können keine Aktualisierung und keinen direkten Neustart eines bestimmten Containers in einer Gruppe mit mehreren Containern ausführen.

Die IP-Adresse eines Containers ändert sich bei Aktualisierungen in der Regel nicht, es wird jedoch nicht garantiert, dass sie gleich bleibt. Wird die Containergruppe auf dem gleichen zugrunde liegenden Host bereitgestellt, behält die Containergruppe ihre IP-Adresse. Von Azure Container Instances wird für die erneute Bereitstellung nach Möglichkeit der gleiche Host verwendet. In einigen seltenen Fällen kann aufgrund von Azure-internen Ereignissen jedoch eine erneute Bereitstellung auf einem anderen Host erforderlich sein. Verwenden Sie stets eine DNS-Namensbezeichnung für Ihre Containerinstanzen, um dieses Problem zu umgehen.

Beendete oder gelöschte Containergruppen können nicht aktualisiert werden. Sobald eine Containergruppe beendet (Status: *Beendet*) oder gelöscht wurde, wird die Gruppe als neue Gruppe bereitgestellt.

## <a name="properties-that-require-container-delete"></a>Eigenschaften, für die eine Löschung der Container erforderlich ist

Wie bereits erwähnt können nicht alle Eigenschaften der Containergruppe aktualisiert werden. Wenn Sie beispielsweise die Ports oder die Neustartrichtlinie eines Containers ändern möchten, müssen Sie zuerst die Containergruppe löschen und anschließend erneut erstellen.

Bei diesen Eigenschaften muss vor der erneuten Bereitstellung die Containergruppe gelöscht werden:

* Betriebssystemtyp
* CPU
* Arbeitsspeicher
* Neustartrichtlinie
* Ports

Wenn Sie eine Containergruppe löschen und erneut erstellen, wird sie nicht erneut bereitgestellt, sondern neu erstellt. Alle Imageebenen werden neu aus der Registrierung und nicht aus der Zwischenspeicherung einer vorherigen Bereitstellung abgerufen. Die IP-Adresse des Containers kann sich unter Umständen ebenfalls ändern, weil die erneute Bereitstellung auf einem anderen zugrunde liegenden Host erfolgt.

## <a name="next-steps"></a>Nächste Schritte

Die **Containergruppe** wurde in diesem Artikel bereits mehrmals erwähnt. Jeder Container in Azure Container Instances wird in einer Containergruppe bereitgestellt, und Containergruppen können mehrere Container enthalten.

[Containergruppen in Azure Container Instances](container-instances-container-groups.md)

[Bereitstellen einer Containergruppe](container-instances-multi-container-group.md)

[Manuelles Beenden oder Starten von Containern in Azure Container Instances](container-instances-stop-start.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[azure-cli-install]: /cli/azure/install-azure-cli
