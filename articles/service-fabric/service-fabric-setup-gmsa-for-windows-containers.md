---
title: Einrichten gruppenverwalteter Dienstkonten für Azure Service Fabric-Containerdienste | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie gruppenverwaltete Dienstkonten für einen in Azure Service Fabric ausgeführten Container einrichten.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: subramar
ms.openlocfilehash: 09994c7676de8470efff1707598ddf32a48e41a0
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599189"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Einrichten gruppenverwalteter Dienstkonten für in Service Fabric ausgeführte Windows-Container

Zur Einrichtung gruppenverwalteter Dienstkonten (group Managed Service Accounts, gMSAs) wird eine Datei mit Anmeldeinformationen (`credspec`) auf allen Knoten im Cluster platziert. Die Datei kann mithilfe einer VM-Erweiterung auf alle Knoten kopiert werden.  Die Datei vom Typ `credspec` muss die gMSA-Kontoinformationen enthalten. Weitere Informationen zur `credspec`-Datei finden Sie unter [Erstellen einer Anmeldeinformationen-Spezifikation](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec) (Dienstkonten). Die Angabe von Anmeldeinformationen und das Tag `Hostname` werden im Anwendungsmanifest angegeben. Das Tag `Hostname` muss dem gMSA-Kontonamen entsprechen, unter dem der Container ausgeführt wird.  Durch das Tag `Hostname` kann sich der Container gegenüber anderen Diensten in der Domäne mittels Kerberos-Authentifizierung authentifizieren.  Der folgende Codeausschnitt zeigt ein Beispiel für die Angabe von `Hostname` und `credspec` im Anwendungsmanifest:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Lesen Sie als Nächstes die folgenden Artikel:

* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)
