---
title: 'Tutorial: Bereinigen des eigenständigen Service Fabric-Clusters – Azure Service Fabric | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Ihren eigenständigen Cluster bereinigen.
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
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 9127ad1fe148f85779913454adf6578a9a8a1055
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274096"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Tutorial: Bereinigen Ihres eigenständigen Clusters

Mit eigenständigen Service Fabric-Clustern können Sie Ihre eigene Umgebung wählen und einen Cluster im Rahmen des Service Fabric-Konzepts „Jedes Betriebssystem, jede Cloud“ erstellen. In dieser Tutorialreihe erstellen Sie einen eigenständigen, in AWS oder Azure gehosteten Cluster und installieren darin eine Anwendung.

Dieses Tutorial ist der vierte Teil einer Serie. In diesem Teil des Tutorials erfahren Sie, wie Sie die AWS- oder Azure-Ressourcen bereinigen, die Sie zum Hosten Ihres Service Fabric-Clusters erstellt haben.

Im vierten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereinigen des Service Fabric-Clusters
> * Bereinigen Ihrer AWS- oder Azure-Ressourcen

## <a name="clean-up-service-fabric-cluster"></a>Bereinigen des Service Fabric-Clusters

1. Stellen Sie eine RDP-Verbindung mit der VM her, die Sie zum Installieren von Service Fabric verwendet haben.
2. Öffnen Sie PowerShell.
3. Wechseln Sie zum Verzeichnis des extrahierten Ordners aus dem zweiten Tutorial.
4. Führen Sie den folgenden Befehl aus, um den Service Fabric-Cluster zu entfernen:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Geben Sie `Y` ein, wenn Sie dazu aufgefordert werden. Wenn der Vorgang erfolgreich war, erhalten Sie eine Ausgabe wie die folgende (mit Ihren eigenen IP-Adressen):

  ```powershell
  Best Practices Analyzer completed successfully.
  Removing configuration from machine 172.31.21.141
  Removing configuration from machine 172.31.27.1
  Removing configuration from machine 172.31.20.163
  Configuration removed from machine 172.31.21.141
  Configuration removed from machine 172.31.27.1
  Configuration removed from machine 172.31.20.163
  The cluster is successfully removed.
  ```

## <a name="clean-up-aws-resources"></a>Bereinigen der AWS-Ressourcen

1. Melden Sie sich bei Ihrem AWS-Konto an.
2. Navigieren Sie zur EC2-Konsole.
3. Wählen Sie die drei Knoten aus, die Sie im ersten Teil des Tutorials erstellt haben.
4. Klicken Sie auf **Aktionen** > **Instanzstatus** > **Beenden**.

## <a name="clean-up-azure-resources"></a>Bereinigen von Azure-Ressourcen

1. Melden Sie sich beim Azure-Portal an.
2. Wechseln Sie zum Abschnitt **Virtuelle Computer**.
3. Aktivieren Sie die Kontrollkästchen der drei Knoten, die Sie im ersten Teil des Tutorials erstellt haben.
4. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Im vierten Teil der Reihe haben Sie gelernt, wie Sie die Ressourcen bereinigen, die in den vorherigen Schritten erstellt wurden.

> [!div class="checklist"]
> * Bereinigen von Ressourcen

> [!div class="nextstepaction"]
> [Zum Anfang](service-fabric-tutorial-standalone-create-infrastructure.md)
