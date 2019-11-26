---
title: Azure PowerShell-Beispiele – Service Fabric| Microsoft-Dokumentation
description: Azure PowerShell-Beispiele – Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: athinanthny
manager: chackdan
editor: ''
tags: ''
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/29/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: cae88e142c3bca15e837db7f084eef68434a78ca
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903268"
---
# <a name="azure-powershell-samples"></a>Azure PowerShell-Beispiele

Die folgende Tabelle enthält Links zu Beispielen von PowerShell-Skripts, die Service Fabric-Cluster, -Anwendungen und -Dienste erstellen und verwalten.

[!INCLUDE [links to azure CLI and service fabric CLI](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Cluster erstellen** ||
| [Erstellen eines Clusters (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Erstellt einen Azure Service Fabric-Cluster. |
| **Verwalten des Clusters, der Knoten und der Infrastruktur** ||
| [Hinzufügen eines Anwendungszertifikats](./scripts/service-fabric-powershell-add-application-certificate.md)| Erstellt ein X.509-Zertifikat für Key Vault und stellt es für eine VM-Skalierungsgruppe in Ihrem Cluster bereit. |
| [Aktualisieren des RDP-Portbereichs auf Cluster-VMs](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Ändert den RDP-Portbereich auf Clusterknoten-VMs in einem bereitgestellten Cluster|
| [Aktualisieren des Administratorbenutzers und des zugehörigen Kennworts für Clusterknoten-VMs](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Aktualisiert den Administratorbenutzer und das zugehörige Kennwort für Clusterknoten-VMs |
| [Öffnen eines Ports im Lastenausgleichsmodul](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Öffnen Sie einen Anwendungsport im Azure-Lastenausgleich, um eingehenden Datenverkehr an einem bestimmten Port zuzulassen. |
| [Erstellen einer Netzwerksicherheitsgruppen-Eingangsregel](./scripts/service-fabric-powershell-add-nsg-rule.md) | Erstellen Sie eine Netzwerksicherheitsgruppen-Eingangsregel zum Zulassen von eingehendem Datenverkehr an den Cluster an einem bestimmten Port. |
| **Anwendungen verwalten** ||
| [Bereitstellen von Anwendungen](./scripts/service-fabric-powershell-deploy-application.md)| Stellt eine Anwendung in einem Cluster bereit.|
| [Upgraden einer Anwendung](./scripts/service-fabric-powershell-upgrade-application.md)| Aktualisieren Sie eine Anwendung.|
| [Entfernen einer Anwendung](./scripts/service-fabric-powershell-remove-application.md)| Entfernt eine Anwendung aus einem Cluster.|
