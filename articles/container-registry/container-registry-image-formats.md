---
title: Inhaltsformate für die Azure Container Registry
description: Erfahren Sie mehr über die unterstützten Inhaltsformate in Azure Container Registry.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: c804dd7b73ea4a51c02c8b342a4ac60d992ec7c5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310692"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>In Azure Container Registry unterstützte Inhaltsformate

Verwenden Sie ein privates Repository in Azure Container Registry, um eines der folgenden Inhaltsformate zu verwalten. 

## <a name="docker-compatible-container-images"></a>Docker-kompatible Containerimages

Die folgenden Formate für Docker-Containerimages werden unterstützt:

* [Docker Image Manifest V2, Schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker Image Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) – enthält Manifestlisten, die es Registrys ermöglichen, Images von mehreren Plattformen unter einem einzigen Verweis „image:tag“ zu speichern

## <a name="oci-images"></a>OCI-Images

Azure Container Registry unterstützt darüber hinaus Images, die der [Spezifikation für das Imageformat der Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md) entsprechen. Zu Verpackungsformaten zählt das Format [Singularity Image Format (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/).

## <a name="helm-charts"></a>Helm-Diagramme

Azure Container Registry kann Repositorys für [Helm-Diagramme](https://helm.sh/) hosten, ein Paketformat, das zur schnellen Verwaltung und Bereitstellung von Anwendungen für Kubernetes verwendet wird. [Helm Client](https://docs.helm.sh/using_helm/#installing-helm) Version 2.11.0 oder höher wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich an wie Sie Images mit Azure Container Registry [per Push übertragen und abrufen](container-registry-get-started-docker-cli.md).

* Verwenden Sie [ACR-Aufgaben](container-registry-tasks-overview.md), um Containerimages zu erstellen und zu testen. 

* Verwenden Sie das [Moby BuildKit](https://github.com/moby/buildkit) zum Erstellen und Verpacken von Containern im OCI-Format.

* Richten Sie ein [Helm-Repository](container-registry-helm-repos.md) ein, dass in Azure Container Registry gehostet wird. 


