---
title: 'Tutorial: Installieren des eigenständigen Service Fabric-Clients – Azure Service Fabric | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie in dem Cluster, den Sie im vorherigen Tutorial erstellt haben, den eigenständigen Service Fabric-Client installieren.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/22/2019
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 82358f4d5548464f1304d77a846d71863ed9efa1
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385006"
---
# <a name="tutorial-install-and-create-service-fabric-cluster"></a>Tutorial: Installieren und Erstellen eines Service Fabric-Clusters

Mit eigenständigen Service Fabric-Clustern können Sie Ihre eigene Umgebung wählen und einen Cluster im Rahmen des Service Fabric-Konzepts „Jedes Betriebssystem, jede Cloud“ erstellen. In dieser Tutorialreihe erstellen Sie einen eigenständigen, in AWS oder Azure gehosteten Cluster und installieren darin eine Anwendung.

Dieses Tutorial ist der zweite Teil einer Reihe. Dieses Tutorial führt Sie durch die Schritte zum Erstellen eines eigenständigen Service Fabric-Clusters.

Im zweiten Teil der Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Herunterladen und Installieren des eigenständigen Service Fabric-Pakets
> * Erstellen des Service Fabric-Clusters
> * Herstellen einer Verbindung mit dem Service Fabric-Cluster

## <a name="download-the-service-fabric-for-windows-server-package"></a>Herunterladen des Service Fabric-Pakets für Windows Server

Für Service Fabric steht ein Setuppaket zum Erstellen eigenständiger Service Fabric-Cluster bereit.  Laden Sie das [Setuppaket](https://go.microsoft.com/fwlink/?LinkId=730690) auf Ihren lokalen Computer herunter.  Kopieren Sie das erfolgreich heruntergeladene Paket über die RDP-Verbindung in Ihre VM, und fügen Sie es auf dem Desktop ein.

Klicken Sie auf die ZIP-Datei, öffnen Sie das Kontextmenü, und klicken Sie auf **Alle extrahieren** > **Extrahieren**.  Im Zuge der Datenextraktion wird auf dem Desktop ein Ordner mit dem Namen der ZIP-Datei erstellt.

Ausführlichere Informationen zum Inhalt des Setuppakets finden Sie [hier](service-fabric-cluster-standalone-package-contents.md).

## <a name="set-up-your-configuration-file"></a>Einrichten Ihrer Konfigurationsdatei

Da Sie einen Windows-Cluster mit drei Knoten erstellen, müssen Sie die Datei `ClusterConfig.Unsecure.MultiMachine.json` ändern.

Aktualisieren Sie als Nächstes die drei Zeilen vom Typ „ipAddress“ (Zeilen 8, 15 und 22 in der Datei) mit der IP-Adresse der jeweiligen Instanz.

Die aktualisierten Knoten sehen wie folgt aus:

```json
        {
            "nodeName": "vm0",
            "ipAddress": "172.31.27.1",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        }
```

Nun müssen einige der Eigenschaften aktualisiert werden.  In Zeile 34 müssen Sie die Verbindungszeichenfolge für den Diagnosespeicher so ändern, dass sie wie folgt aussieht: `"connectionstring": "C:\\ProgramData\\SF\\DiagnosticsStore"`.

Fügen Sie abschließend im Konfigurationsabschnitt `nodeTypes` einen neuen Abschnitt hinzu, um die kurzlebigen Ports zuzuordnen, die von Windows verwendet werden.  Die Konfigurationsdatei sollte wie folgt aussehen:

```json
"applicationPorts": {
    "startPort": "20001",
    "endPort": "20031"
},
"ephemeralPorts": {
    "startPort": "20606",
    "endPort": "20861"
},
"isPrimary": true
```

## <a name="validate-the-environment"></a>Überprüfen der Umgebung

Das Skript *TestConfiguration.ps1* im eigenständigen Paket wird als Best Practices Analyzer verwendet, um zu prüfen, ob ein Cluster in einer bestimmten Umgebung bereitgestellt werden kann. Informationen zu den Voraussetzungen und den Anforderungen an die Umgebung finden Sie unter [Planen und Vorbereiten der Bereitstellung eines eigenständigen Service Fabric-Clusters](service-fabric-cluster-standalone-deployment-preparation.md). Führen Sie das Skript aus, um sich zu vergewissern, dass der Entwicklungscluster erstellt werden kann:

```powershell
cd .\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

Es sollte eine Ausgabe wie die folgende angezeigt werden. Wenn das untere Feld „Passed“ als `True` zurückgegeben wird, wurden die Integritätsprüfungen erfolgreich abgeschlossen, und der Cluster kann auf der Grundlage der eingegebenen Konfiguration bereitgestellt werden.

```powershell
Trace folder already exists. Traces will be written to existing trace folder: C:\Users\Administrator\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 :
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
DataDrivesAvailable        : True
NoDomainController         : True
Passed                     : True
```

## <a name="create-the-cluster"></a>Erstellen des Clusters

Führen Sie nach erfolgreicher Überprüfung Ihrer Clusterkonfiguration das Skript *CreateServiceFabricCluster.ps1* aus, um den Service Fabric-Cluster für die virtuellen Computer in der Konfigurationsdatei bereitzustellen.

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

Wenn alles funktioniert, wird eine Ausgabe wie die folgende angezeigt:

```powershell
Your cluster is successfully created! You can connect and manage your cluster using Microsoft Azure Service Fabric Explorer or PowerShell. To connect through PowerShell, run 'Connect-ServiceFabricCluster [ClusterConnectionEndpoint]'.
```

> [!NOTE]
> Auf dem (virtuellen) Computer, auf dem Sie das PowerShell-Skript „CreateServiceFabricCluster.ps1“ ausgeführt haben, werden Ablaufverfolgungen der Bereitstellung geschrieben. Diese finden Sie im Unterordner „DeploymentTraces“ des Verzeichnisses, in dem das Skript ausgeführt wurde. Um zu ermitteln, ob Service Fabric ordnungsgemäß auf einem Computer bereitgestellt wurde, suchen Sie die installierten Dateien im Verzeichnis „FabricDataRoot“, wie im Abschnitt „FabricSettings“ der Clusterkonfigurationsdatei beschrieben (standardmäßig „C:\ProgramData\SF“). Im Task Manager wird auch die Ausführung der Prozesse „FabricHost.exe“ und „Fabric.exe“ angezeigt.
>
>

### <a name="bring-up-service-fabric-explorer"></a>Aufrufen von Service Fabric Explorer

Jetzt können Sie über Service Fabric Explorer eine Verbindung mit dem Cluster herstellen, und zwar entweder direkt von einem der Computer mit „http:\//localhost:19080/Explorer/index.html“ oder per Remotezugriff mit „http:\//<*IPAddressofaMachine*>:19080/Explorer/index.html“.

## <a name="add-and-remove-nodes"></a>Hinzufügen und Entfernen von Knoten

Wenn sich Ihre geschäftlichen Anforderungen ändern, können Sie dem eigenständigen Service Fabric-Cluster Knoten hinzufügen oder Knoten daraus entfernen. Ausführliche Schritte finden Sie unter [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="next-steps"></a>Nächste Schritte

Im zweiten Teil der Reihe haben Sie gelernt, wie Sie große Mengen von Zufallsdaten parallel in ein Speicherkonto hochladen können, wozu Folgendes gehört:

> [!div class="checklist"]
> * Konfigurieren der Verbindungszeichenfolge
> * Erstellen der Anwendung
> * Ausführen der Anwendung
> * Überprüfen der Anzahl von Verbindungen

Im dritten Teil der Reihe installieren Sie eine Anwendung in dem von Ihnen erstellten Cluster.

> [!div class="nextstepaction"]
> [Deploy an application on your Service Fabric standalone cluster](service-fabric-tutorial-standalone-install-an-application.md) (Bereitstellen einer Anwendung in Ihrem eigenständigen Service Fabric-Cluster)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
