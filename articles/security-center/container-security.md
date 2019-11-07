---
title: Containersicherheit in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Containersicherheitsfeatures von Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 41332a76c3c4e8228e6827e1553d73da3a85fc09
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521536"
---
# <a name="container-security-in-security-center"></a>Containersicherheit in Security Center

Azure Security Center ist die native Azure-Lösung für die Containersicherheit. Security Center ist auch die optimale zentralisierte Benutzeroberfläche für die Sicherheit Ihrer Cloudworkloads, VMs, Server und Container.

In diesem Artikel wird beschrieben, wie Sie die Sicherheit ihrer Container und deren Apps verbessern, überwachen und verwalten können. Sie erfahren, wie Security Center diese Kernaspekte der Containersicherheit unterstützt:

* Verwaltung von Sicherheitsrisiken
* Härtung der Containerumgebung
* Laufzeitschutz

[![Registerkarte „Containersicherheit“ in Azure Security Center](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

## <a name="vulnerability-management---scanning-container-images-preview"></a>Verwaltung von Sicherheitsrisiken: Scannen von Containerimages (Vorschau)
Um Ihre Azure Container Registry zu überwachen, verbinden Sie sie mit Security Center. Wenn ein neues Image per Push bereitgestellt wird, überprüft Security Center das Image mithilfe eines Scanners von Qualys, dem branchenführenden Hersteller von Anwendungen zur Sicherheitsrisikoüberprüfung.

Werden Probleme gefunden – von Qualys oder Security Center –, werden Sie im Security Center-Dashboard benachrichtigt. Für jedes Sicherheitsrisiko bietet Security Center Handlungsempfehlungen sowie eine Klassifizierung des Schweregrads und Anleitungen für die Behebung des Problems. Ausführliche Informationen zu Security Center-Empfehlungen finden Sie in der Liste der Empfehlungen unter [Schutz von virtuellen Computern](security-center-virtual-machine-protection.md##compute-and-app-recs).

## <a name="environment-hardening"></a>Umgebungshärtung

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Kontinuierliche Überwachung ihrer Docker-Konfiguration
Azure Security Center identifiziert nicht verwaltete Container, die auf IaaS-Linux-VMs oder anderen Linux-Computern gehostet werden, auf denen Docker-Container ausgeführt werden. Security Center bewertet kontinuierlich die Konfigurationen dieser Container. Anschließend werden sie mit dem [Docker-Benchmark von Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/docker/) verglichen. 

Security Center umfasst den gesamten Regelsatz des CIS-Docker-Benchmark und benachrichtigt Sie, sobald Ihre Container eine der Steuerungen nicht erfüllen. Werden Fehlkonfigurationen gefunden, generiert Security Center Sicherheitsempfehlungen. Verwenden Sie die Seite **Empfehlungen**, um Empfehlungen anzuzeigen und Probleme zu beheben. Die Empfehlungen werden auch auf der Registerkarte **Container** angezeigt, auf der alle mit Docker bereitgestellten virtuellen Computer aufgeführt sind. Wenn Sie die Sicherheitsprobleme auf einem virtuellen Computer untersuchen, werden von Security Center zusätzliche Informationen zu den Containern auf dem Computer bereitgestellt. Zu diesen Informationen gehören die Docker-Version und Anzahl der auf dem Host ausgeführten Images. Ausführliche Informationen zu den Empfehlungen finden Sie [hier](https://docs.microsoft.com/azure/security-center/security-center-virtual-machine-protection). 

>[!NOTE]
> Diese CIS-Benchmarkprüfungen können nicht auf von AKS oder Databricks verwalteten virtuellen Computern ausgeführt werden.

### <a name="continuous-monitoring-of-your-kubernetes-clusters-preview"></a>Fortlaufende Überwachung ihrer Kubernetes-Cluster (Vorschau)
Security Center kann zusammen mit Azure Kubernetes Service (AKS) verwendet werden, dem verwalteten Containerorchestrierungsdienst von Microsoft für Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern.

AKS bietet Sicherheitssteuerungen und Einblicke in den Sicherheitsstatus ihrer Cluster. Security Center verwendet diese Features für Folgendes:
* Konstante Überwachung der Konfiguration Ihrer AKS-Cluster
* Generieren von an Branchenstandards ausgerichteten Sicherheitsempfehlungen

Ausführliche Informationen zu den Security Center-Empfehlungen finden Sie unter [Schutz für virtuelle Computer](security-center-virtual-machine-protection.md).

## <a name="run-time-protection---real-time-threat-detection"></a>Laufzeitschutz: Bedrohungserkennung in Echtzeit 

Security Center bietet eine Echtzeit-Bedrohungserkennung für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Bedrohungen werden auf Host- und AKS-Clusterebene erkannt. Ausführliche Informationen finden Sie unter [Azure Container Service](https://docs.microsoft.com/azure/security-center/security-center-alerts-compute#azure-container-service-).


## <a name="to-view-the-security-posture-of-your-container-related-resources"></a>So zeigen Sie den Sicherheitsstatus ihrer containerbezogenen Ressourcen an
1.  Öffnen Sie die Seite **Compute und Apps** von Security Center.
2.  Klicken Sie auf die Registerkarte **Container**. Der Status Ihrer AKS-Cluster, ACR-Registrierungen und virtuellen Computern, die Docker ausführen, wird angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Containersicherheit in Azure Security Center finden Sie unter:
* Ausführliche Informationen zur [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* Ausführliche Informationen zur [Integration mit Azure Container Registry](azure-container-registry-integration.md)