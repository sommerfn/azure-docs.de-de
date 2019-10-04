---
title: Inhaltsformate für die Azure Container Registry
description: Erfahren Sie mehr über die unterstützten Inhaltsformate in Azure Container Registry.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 08/30/2019
ms.author: danlep
ms.openlocfilehash: d49aab89c9568f168808c40508b4fe7d3175e902
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70164522"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>In Azure Container Registry unterstützte Inhaltsformate

Verwenden Sie ein privates Repository in Azure Container Registry, um eines der folgenden Inhaltsformate zu verwalten. 

## <a name="docker-compatible-container-images"></a>Docker-kompatible Containerimages

Die folgenden Formate für Docker-Containerimages werden unterstützt:

* [Docker Image Manifest V2, Schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker Image Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) – enthält Manifestlisten, die es Registrys ermöglichen, Images von mehreren Plattformen unter einem einzigen Verweis „image:tag“ zu speichern

## <a name="oci-images"></a>OCI-Images

Azure Container Registry unterstützt Images, die der [Spezifikation für das Imageformat der Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md) entsprechen. Zu Verpackungsformaten zählt das Format [Singularity Image Format (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/).

## <a name="oci-artifacts"></a>OCI-Artefakte

Azure Container Registry unterstützt die [OCI Distribution Specification](https://github.com/opencontainers/distribution-spec) (OCI-Verteilungsspezifikation). Hierbei handelt es sich um eine anbieterneutrale, cloudunabhängige Spezifikation zum Speichern, Freigeben, Schützen und Bereitstellen von Containerimages und anderen Inhaltstypen (Artefakten). Die Spezifikation ermöglicht einer Registrierung das Speichern eines breiten Bereichs von Artefakten zusätzlich zu den Containerimages. Sie verwenden Tools, die für das Artefakt geeignet sind, um Push- und Pullvorgänge für Artefakte durchzuführen. Lesen Sie beispielsweise den Artikel [Pushen und Pullen eines OCI-Artefakts unter Verwendung einer Azure-Containerregistrierung](container-registry-oci-artifacts.md).

Weitere Informationen zu OCI-Artefakten finden Sie im Repository [OCI Registry as Storage (ORAS)](https://github.com/deislabs/oras) (OCI-Registrierung als Speicher (ORAS)) und im Repository [OCI Artifacts](https://github.com/opencontainers/artifacts) (OCI-Artefakte) auf GitHub.

## <a name="helm-charts"></a>Helm-Diagramme

Azure Container Registry kann Repositorys für [Helm-Diagramme](https://helm.sh/) hosten, ein Paketformat, das zur schnellen Verwaltung und Bereitstellung von Anwendungen für Kubernetes verwendet wird. [Helm Client](https://docs.helm.sh/using_helm/#installing-helm) Version 2 (2.11.0 oder höher) wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich an wie Sie Images mit Azure Container Registry [per Push übertragen und abrufen](container-registry-get-started-docker-cli.md).

* Verwenden Sie [ACR-Aufgaben](container-registry-tasks-overview.md), um Containerimages zu erstellen und zu testen. 

* Verwenden Sie das [Moby BuildKit](https://github.com/moby/buildkit) zum Erstellen und Verpacken von Containern im OCI-Format.

* Richten Sie ein [Helm-Repository](container-registry-helm-repos.md) ein, dass in Azure Container Registry gehostet wird. 


