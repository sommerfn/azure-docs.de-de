---
title: Erfahren Sie mehr über die Erstellung und Veröffentlichung von .NET Core-Anwendungen in einem Azure Service Fabric-Linux-Remotecluster | Microsoft-Dokumentation
description: Erstellen und Veröffentlichen von .NET Core-Apps für einen Remote-Linux-Cluster aus Visual Studio
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: 46d76edbe8cede12e8c7811f43c28a65c1ebaed0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068125"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Verwenden von Visual Studio zum Erstellen und Veröffentlichen von .NET Core-Anwendungen für einen Linux-Service Fabric-Remotecluster
Mit Visual Studio-Tools können Sie Service Fabric-.NET Core-Anwendungen für einen Linux-Service Fabric-Cluster entwickeln und veröffentlichen. Zum Bereitstellen einer .NET Core-Anwendung für Linux-Service Fabric-Cluster aus Visual Studio ist mindestens die SDK-Version 3.4 erforderlich.

> [!Note]
> Visual Studio unterstützt nicht das Debuggen von Service Fabric-Anwendungen für Linux.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>Erstellen einer Service Fabric-Anwendung für .NET Core
1. Starten Sie Visual Studio als **Administrator**.
2. Erstellen Sie ein Projekt mit **Datei -> Neu -> Projekt**.
3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Cloud > Service Fabric-Anwendung** aus.
![create-application]
4. Geben Sie der Anwendung einen Namen, und klicken Sie auf **OK**.
5. Wählen Sie auf der Seite **Neuer Service Fabric-Dienst** im **.Net Core-Abschnitt** den Diensttyp aus, den Sie erstellen möchten.
![create-service]

## <a name="deploy-to-a-remote-linux-cluster"></a>Bereitstellen für einen Remote-Linux-Cluster
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Anwendung, und wählen Sie **Erstellen** aus.
![build-application]
2. Klicken Sie nach Abschluss des Buildprozesses für die Anwendung mit der rechten Maustaste auf den Dienst, und wählen Sie das Bearbeiten der **CSPROJ-Datei** aus.
![edit-csproj]
3. Setzen Sie die UpdateServiceFabricManifestEnabled-Eigenschaft von „True“ auf **False**, wenn der Dienst ein **Akteurprojekttyp** ist. Wenn Ihre Anwendung nicht über einen Actordienst verfügt, fahren Sie mit Schritt 4 fort.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> UpdateServiceFabricManifestEnabled auf „False“ zu setzen deaktiviert Updates von „Servicemanifest.xml“ während eines Builds. Jede Änderung wie Hinzufügen, Entfernen oder Umbenennen des Diensts wird in „ServiceManifest.xml“ nicht berücksichtigt. Wenn Änderungen vorgenommen werden, müssen Sie entweder das ServiceManifest manuell aktualisieren oder UpdateServiceFabricManifestEnabled vorübergehend auf „True“ setzen und den Dienst erstellen, der „ServiceManifest.xml“ aktualisiert, und dann wieder auf „False“ zurücksetzen.
>

4. Aktualisieren Sie den RuntimeIndetifier aus win7-x64 auf die Zielplattform im Dienstprojekt.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. Aktualisieren Sie im ServiceManifest das Einstiegspunktprogramm, um „.exe“ zu entfernen. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Anwendung, und wählen Sie **Veröffentlichen** aus. Das Dialogfeld **Veröffentlichen** wird angezeigt.
7. Wählen Sie im **Verbindungsendpunkt** den Endpunkt für den gewünschten Remote-Service Fabric-Linux-Cluster aus.
![publish-application]

<!--Image references-->
[create-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[create-service]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[build-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[publish-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Erste Schritte mit Service Fabric mit .NET Core](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/).
