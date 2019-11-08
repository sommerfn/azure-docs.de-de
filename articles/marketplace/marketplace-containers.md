---
title: Leitfaden für die Veröffentlichung von Containerangeboten im Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Containern im Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: ffdc6b22209c1d334ac7b75c7079e755a7405154
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822995"
---
# <a name="containers-offer-publishing-guide"></a>Leitfaden für die Veröffentlichung von Containerangeboten

Containerangebote erleichtern Ihnen die Veröffentlichung Ihres Containerimages im Azure Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Hierbei handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.

Verwenden Sie den Angebotstyp „Container“, wenn Ihre Lösung ein Docker-Container-Image ist, das als Kubernetes-basierter Azure Container Service bereitgestellt wird.

>[!NOTE]
>Z.B. ein Kubernetes-basierter Azure Container Cervice wie Azure Kubernetes Service oder Azure Container Instances, die Wahl der Azure-Kunden für eine Kubernetes-basierte Containerlaufzeit.  

Microsoft unterstützt derzeit kostenlose und Bring-Your-Own-License-Lizenzierungsmodelle (BYOL).

## <a name="containers-offer"></a>Containerangebot

| Anforderung | Details |  
|:--- |:--- |  
| Abrechnung und Messung | Es wird entweder das kostenlose oder BYOL-Abrechnungsmodell unterstützt. |  
| Auf Grundlage von Dockerfile erstelltes Image | Containerimages müssen auf der Docker-Imagespezifikation basieren und auf der Grundlage einer Dockerfile-Datei erstellt werden.<ul> <li>Weitere Informationen zum Erstellen von Docker-Images finden Sie im Abschnitt zur Verwendung auf [docs.docker.com/engine/reference/builder/#usage](https://docs.docker.com/engine/reference/builder/#usage).</li> </ul> |  
| Hosten in ACR | Containerimages müssen in einem ACR-Repository (Azure Container Registry) gehostet werden.<ul> <li>Weitere Informationen zum Arbeiten mit ACR finden Sie im „Schnellstart: Erstellen einer Containerregistrierung mit dem Azure-Portal“ auf [docs.microsoft.com/de-de/azure/container-registry/container-registry-get-started-portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).</li> </ul> |  
| Imagemarkierung | Containerimages müssen mindestens 1 Tag enthalten (maximale Tags: 16).<ul> <li>Weitere Informationen zum Markieren eines Images finden Sie auf der Docker-Tag-Seite auf [docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag).</li> </ul> |  

## <a name="next-steps"></a>Nächste Schritte

Falls Sie dies noch nicht getan haben, 

- [Registrieren Sie](https://azuremarketplace.microsoft.com/sell) beim Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,

- [melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com), um Ihr Angebot zu erstellen oder zu vervollständigen.
- Weitere Informationen finden Sie unter [Container](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer).
