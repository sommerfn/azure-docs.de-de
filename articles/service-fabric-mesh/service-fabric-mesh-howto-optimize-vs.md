---
title: Optimieren der Leistung von Visual Studio für Azure Service Fabric Mesh-Projekte | Microsoft-Dokumentation
description: In diesem Artikel wird veranschaulicht, wie Sie die Leistung von Visual Studio für Service Fabric Mesh-Projekte optimieren, damit Ihr erster Debuglauf (F5) deutlich schneller durchgeführt wird.
services: service-fabric-mesh
keywords: Optimieren der Debugleistung
author: dkkapur
ms.author: dekapur
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 04aeaa6b008b50789f4380e4bb98beba3957c2e9
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73663432"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Optimieren der Leistung von Visual Studio für Service Fabric Mesh-Projekte

In diesem Artikel wird veranschaulicht, wie Sie die Leistung von Visual Studio für Service Fabric Mesh-Projekte optimieren, damit Ihr erster Debuglauf (F5) deutlich schneller durchgeführt wird.  

## <a name="change-visual-studio-settings"></a>Ändern der Visual Studio-Einstellungen
 
In Visual Studio können Sie unter **Extras** > **Optionen**  > **Service Fabric Mesh-Tools** > **Allgemein** die folgenden Einstellungen anpassen:

- Mit der Option **Erforderliche Docker-Images beim Laden des Projekts mithilfe von Pull übertragen** wird Ihr erster Debuglauf (F5) schneller durchgeführt, indem das Herunterladen des Images während des Ladevorgangs für das Projekt gestartet wird.  
- Mit der Option **Deploy application on project open** (Anwendung beim Öffnen des Projekts bereitstellen) kann Ihr erster Debuglauf (F5) beschleunigt werden, indem der Bereitstellungsprozess gestartet wird, nachdem das Projekt geöffnet wurde.  
- Mit der Option **Remove application on project close** (Anwendung beim Schließen des Projekts entfernen) können die der App zugeordneten Ressourcen (CPU, RAM) wieder beansprucht werden, indem die Mesh-App beim Schließen des Projekts entfernt wird.  

Wenn im Ausgabefenster der Service Fabric-Tools Meldungen mit dem Hinweis angezeigt werden, dass Visual Studio „Images überträgt“, „sich in der Aufwärmphase befindet“ oder „die Anwendung entfernt“, bezieht sich dies auf die obigen Einstellungen. Sie können diese Einstellungen deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Tutorial: Debuggen einer Service Fabric Mesh-Anwendung, die in Ihrem lokalen Entwicklungscluster ausgeführt wird](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md).