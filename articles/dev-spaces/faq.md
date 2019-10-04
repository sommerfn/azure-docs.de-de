---
title: Häufig gestellte Fragen zu Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/25/2019
ms.topic: conceptual
description: Hier finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure Dev Spaces.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: de44bf323b300a4467dc8b30555798f557898494
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71305913"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Häufig gestellte Fragen zu Azure Dev Spaces

Im Folgenden werden einige der häufig gestellten Fragen zu Azure Dev Spaces beantwortet.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>In welchen Azure-Regionen wird Azure Dev Spaces derzeit zur Verfügung gestellt?

Eine vollständige Liste der verfügbaren Regionen finden Sie unter [Unterstützte Regionen und Konfigurationen][supported-regions].

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Kann ich Azure Dev Spaces auch ohne eine öffentliche IP-Adresse verwenden?

Nein, Azure Dev Spaces kann ohne öffentliche IP-Adresse nicht in einem AKS-Cluster bereitgestellt werden. Eine öffentliche IP-Adresse ist bei Azure Dev Spaces [für das Routing erforderlich][dev-spaces-routing].

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Kann ich mit Azure Dev Spaces meinen eigenen Dateneingangscontroller verwenden?

Ja, Sie können neben dem von Azure Dev Spaces erstellten Controller auch einen eigenen konfigurieren. Hierzu können Sie z. B. [traefik][ingress-traefik] verwenden.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Kann ich HTTPS mit Azure Dev Spaces verwenden?

Ja, Sie können mithilfe von [traefik][ingress-https-traefik] Ihren eigenen Eingangscontroller mit HTTPS konfigurieren.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Kann ich Azure Dev Spaces in einem Cluster verwenden, der CNI anstelle von Kubenet verwendet? 

Ja, Sie können Azure Dev Spaces in einem AKS-Cluster verwenden, der für Netzwerke CNI verwendet. Beispielsweise können Sie Azure Dev Spaces in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] verwenden, der für Netzwerke CNI verwendet.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Kann ich Azure Dev Spaces mit Windows-Containern verwenden?

Derzeit ist Azure Dev Spaces nur für die Ausführung auf Linux-Pods und -Knoten vorgesehen. Sie können Azure Dev Spaces aber in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] ausführen.


[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: about.md#supported-regions-and-configurations
[windows-containers]: how-to/run-dev-spaces-windows-containers.md