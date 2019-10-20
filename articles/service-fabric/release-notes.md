---
title: Azure Service Fabric-Versionen
description: Versionshinweise zu den neuesten Features und Verbesserungen in Service Fabric
author: athinanthny
manager: chackdan
ms.author: atsenthi
ms.date: 6/10/2019
ms.topic: conceptual
ms.service: service-fabric
hide_comments: true
hideEdit: true
ms.openlocfilehash: 4a681b3a09def3a7b27b603cf5201aebdbf2e4bf
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72386213"
---
# <a name="service-fabric-releases"></a>Service Fabric-Versionen

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Handbücher zur Problembehandlung</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">Problemverfolgung</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">Supportoptionen</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">Unterstützte Versionen</a> 
| <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Codebeispiele</a>

Dieser Artikel enthält weitere Informationen zu den neuesten Versionen und Updates für die Service Fabric-Runtime und -SDKs.

## <a name="whats-new-in-service-fabric"></a>Neuigkeiten in Service Fabric

### <a name="service-fabric-65"></a>Service Fabric 6.5

Die neueste Version von Service Fabric umfasst Verbesserungen hinsichtlich Unterstützungsmöglichkeiten, Zuverlässigkeit und Leistung sowie neue Features, Fehlerbehebungen und Verbesserungen, um die Cluster- und Anwendungslebenszyklusverwaltung zu vereinfachen.

> [!IMPORTANT]
> Service Fabric 6.5 ist die endgültige Version mit Unterstützung für Service Fabric-Tools in Visual Studio 2015. Kunden wird empfohlen, in Zukunft auf [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) umzusteigen.

Dies sind die Neuigkeiten in Service Fabric 6.5:

- Service Fabric Explorer enthält einen [Image Store-Viewer](service-fabric-visualizing-your-cluster.md#image-store-viewer) zum Überprüfen von Anwendungen, die Sie in den Imagespeicher hochgeladen haben.

- [Patch Orchestration Application (POA)](service-fabric-patch-orchestration-application.md) Version [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) umfasst viele Verbesserungen für die Selbstdiagnose. Kunden von POA wird empfohlen, auf diese Version umzusteigen.

- [EventStore-Dienst ist standardmäßig für Service Fabric 6.5-Cluster aktiviert](service-fabric-visualizing-your-cluster.md#event-store), es sei denn, Sie haben sich abgemeldet.

- [Replikatlebenszyklusereignisse](service-fabric-diagnostics-event-generation-operational.md#replica-events) wurden für zustandsbehaftete Dienste hinzugefügt.

- [Bessere Sichtbarkeit der Status von Startknoten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), einschließlich Clusterebenenwarnungen, wenn ein Startknoten (Seedknoten) fehlerhaft ist (*Ausgefallen*, *Entfernt* oder *Unbekannt*).

- Das [Service Fabric Application Disaster Recovery-Tool](https://github.com/Microsoft/Service-Fabric-AppDRTool) ermöglicht, dass zustandsbehaftete Service Fabric-Dienste nach einem Notfall im primären SF-Cluster schnell wiederhergestellt werden können. Daten aus dem primären Cluster werden ständig in der sekundären Standbyanwendung über regelmäßige Sicherung und Wiederherstellung synchronisiert.

- Visual Studio-Unterstützung für [Veröffentlichen von .NET Core-Apps in Linux-basierten Clustern](service-fabric-how-to-publish-linux-app-vs.md).

- [Azure Service Fabric CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) wird für Service Fabric 6.5 (und höhere Versionen) automatisch installiert, wenn Sie ein Upgrade ausführen oder einen neuen Linux-Cluster in Azure erstellen.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) wird in MacOS/Linux OneBox-Clustern standardmäßig erstellt.

Weitere Informationen finden Sie in den [Versionshinweisen zu Service Fabric 6.5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

### <a name="service-fabric-65-releases"></a>Service Fabric 6.5-Releases

| Herausgabedatum | Release | Weitere Informationen |
|---|---|---|
| 11. Juni 2019 | [Azure Service Fabric 6.5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2\. Juli 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29. Juli 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| 23. August 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |
| 14. Oktober 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Fifth-Refresh-Release/ba-p/913296)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU5.md)  |


## <a name="previous-versions"></a>Vorherige Versionen

### <a name="service-fabric-64-releases"></a>Service Fabric 6.4-Releases

| Herausgabedatum | Release | Weitere Informationen |
|---|---|---|
| 30. November 2018 | [Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12. Dezember 2018 | [Azure Service Fabric 6.4 Refresh Release jfür Windows-Cluster](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4\. Februar 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4\. März 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8\. April 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2\. Mai 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28. Mai 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
