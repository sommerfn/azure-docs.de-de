---
title: 'Azure CLI-Skript: Bereitstellungsbeispiel'
description: Erstellen Sie einen sicheren Service Fabric-Linux-Cluster in Azure mithilfe der Azure CLI.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 1386be8093cac4f5271886ff736b24acd483d64e
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69032825"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Erstellen eines sicheren Service Fabric-Linux-Clusters in Azure

Dieser Befehl erstellt ein selbst signiertes Zertifikat, fügt es zum Schlüsseltresor hinzu und lädt das Zertifikat lokal herunter.  Mit dem neuen Zertifikat wird der Cluster gesichert, wenn er bereitgestellt wird.  Anstelle der Erstellung eines neuen Zertifikats können Sie auch ein vorhandenes Zertifikat verwenden.  Der Name des Antragstellers für das Zertifikat muss in jedem Fall der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Installieren Sie ggf. die [Azure-Befehlszeilenschnittstelle](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, der Cluster und alle zugehörigen Ressourcen entfernt werden.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest) | Erstellt einen neuen Service Fabric-Cluster.  |

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
