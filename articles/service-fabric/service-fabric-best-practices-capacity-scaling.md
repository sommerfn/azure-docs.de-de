---
title: Kapazitätsplanung und Skalierung für Azure Service Fabric | Microsoft-Dokumentation
description: Bewährte Methoden für die Planung und Skalierung von Service Fabric-Clustern und -Anwendungen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: fe0af4ca7b6860fff19f4df3165a975c42b54a03
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277773"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Kapazitätsplanung und Skalierung für Azure Service Fabric

Vor der Erstellung eines Azure Service Fabric-Clusters oder der Skalierung von Computeressourcen, die den Cluster hosten, ist es wichtig, die Kapazität zu planen. Weitere Informationen zur Planung der Kapazität finden Sie unter [Planen der Service Fabric-Clusterkapazität](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Weitere bewährte Methoden zur Skalierbarkeit von Clustern finden Sie unter [Überlegungen zur Skalierbarkeit](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations) bei Service Fabric.

Neben der Berücksichtigung der Merkmale von Knotentypen und Clustern sollten Sie einplanen, dass Skalierungsvorgänge für eine Produktionsumgebung länger als eine Stunde dauern. Dieser Aspekt gilt unabhängig von der Anzahl der hinzugefügten virtuellen Computer.

## <a name="autoscaling"></a>Automatische Skalierung
Sie sollten Skalierungsvorgänge über Azure Resource Manager-Vorlagen ausführen, da dies als bewährte Methode für [Ressourcenkonfigurationen als Code]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code) gilt. 

Durch die Verwendung der automatischen Skalierung über VM-Skalierungsgruppen definiert Ihre versionierte Resource Manager-Vorlage die Anzahl von Instanzen für VM-Skalierungsgruppen falsch. Durch eine falsche Definition steigt das Risiko, dass zukünftige Bereitstellungen unbeabsichtigte Skalierungsvorgänge verursachen. Im Allgemeinen sollten Sie die automatische Skalierung in folgenden Fällen verwenden:

* Die Bereitstellung Ihrer Resource Manager-Vorlagen mit der deklarierten geeigneten Kapazität unterstützt Ihren Anwendungsfall nicht.
     
   Sie können zusätzlich zur manuellen Skalierung eine [Continuous Integration- und Continuous Delivery-Pipeline in Azure DevOps Services mithilfe von Bereitstellungsprojekten für Azure-Ressourcengruppen](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts) konfigurieren. Diese Pipeline wird meist durch eine Logik-App ausgelöst, die Leistungsmetriken von virtuellen Computern nutzt, die über die [Azure Monitor-REST-API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough) abgefragt wurden. Die Pipeline führt eine effektive automatische Skalierung basierend auf Ihren gewünschten Metriken durch und optimiert gleichzeitig die Resource Manager-Vorlagen.
* Sie müssen jeweils nur einen Knoten einer VM-Skalierungsgruppe horizontal skalieren.
   
   Zum Erweitern um drei oder mehr Knoten gleichzeitig sollten Sie [einen Service Fabric-Cluster durch das Hinzufügen einer VM-Skalierungsgruppe erweitern](virtual-machine-scale-set-scale-node-type-scale-out.md). Es ist am sichersten, das Hoch- und Herunterskalieren von VM-Skalierungsgruppen horizontal und mit jeweils einem Knoten durchzuführen.
* Für den Service Fabric-Cluster ist die Zuverlässigkeitsstufe „Silber“ oder höher und die Dauerhaftigkeitsstufe „Silber“ oder höher für alle Skalierungsgruppen festgelegt, für die Sie Regeln für die automatische Skalierung konfigurieren.
  
   Die Mindestkapazität für Regeln zur automatischen Skalierung beträgt fünf VM-Instanzen. Zudem muss sie gleich oder größer dem der minimalen Zuverlässigkeitsstufe für Ihren primären Knotentyp sein.

> [!NOTE]
> Der zustandsbehaftete Service Fabric-Dienst „fabric:/System/InfastructureService/<KNOTENTYPNAME>“ wird auf jedem Knotentyp mit der Dauerhaftigkeit „Silber“ oder höher ausgeführt. Dies ist der einzige Systemdienst, dessen Ausführung in Azure auf allen Clusterknotentypen unterstützt wird.

## <a name="vertical-scaling-considerations"></a>Überlegungen zur vertikalen Skalierung

Die [vertikale Skalierung](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) eines Knotentyps in Azure Service Fabric erfordert mehrere Schritte und Überlegungen. Beispiel:

* Der Cluster muss vor der Skalierung fehlerfrei sein. Andernfalls wird der Cluster nur weiter destabilisiert.
* Für alle Knotentypen des Service Fabric-Clusters, die zustandsbehaftete Dienste hosten, ist die Dauerhaftigkeitsstufe „Silber“ oder höher erforderlich.

> [!NOTE]
> Der primäre Knotentyp, der zustandsbehaftete Service Fabric-Systemdienste hostet, muss mindestens die Dauerhaftigkeitsstufe „Silber“ aufweisen. Nachdem Sie die Dauerhaftigkeitsstufe „Silber“ aktiviert haben, werden Clustervorgänge wie das Aktualisieren, Hinzufügen oder Entfernen von Knoten langsamer durchgeführt, da im System die Optimierung der Datensicherheit Vorrang vor der Geschwindigkeit von Vorgängen hat.

Die vertikale Skalierung einer VM-Skalierungsgruppe ist ein destruktiver Vorgang. Führen Sie stattdessen eine horizontale Skalierung Ihres Clusters aus, indem Sie eine neue Skalierungsgruppe mit der gewünschten SKU hinzufügen. Migrieren Sie dann Ihre Dienste zur gewünschten SKU, um die sichere vertikale Skalierung abzuschließen. Die Änderung der SKU einer VM-Skalierungsgruppenressource ist ein destruktiver Vorgang, da für die Hosts ein Reimaging durchgeführt wird, bei dem alle lokal persistenten Zustände entfernt werden.

Für Ihren Cluster werden [Knoteneigenschaften und Platzierungseinschränkungen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) in Service Fabric verwendet, um festzulegen, wo die Anwendungsdienste gehostet werden. Wenn Sie Ihren primären Knotentyp vertikal skalieren, deklarieren Sie identische Eigenschaftswerte für `"nodeTypeRef"`. Sie finden diese Werte in der Service Fabric-Erweiterung für VM-Skalierungsgruppen. 

Der folgende Codeausschnitt einer Resource Manager-Vorlage zeigt die Eigenschaften, die Sie deklarieren. Er enthält den gleichen Wert für die neu bereitgestellten Skalierungsgruppen, zu denen Sie skalieren. Er wird nur als temporärer zustandsbehafteter Dienst für Ihren Cluster unterstützt.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Führen Sie den Cluster nicht länger als für den Abschluss eines erfolgreichen vertikalen Skalierungsvorgangs erforderlich mit mehreren Skalierungsgruppen aus, die den gleichen Eigenschaftswert für `nodeTypeRef` verwenden.
>
> Überprüfen Sie Vorgänge immer in Testumgebungen, bevor Sie Änderungen in der Produktionsumgebung vornehmen. Standardmäßig weisen die Systemdienste des Service Fabric-Clusters nur für den primären Zielknotentyp eine Platzierungseinschränkung auf.

Führen Sie nach dem Deklarieren der Knoteneigenschaften und Platzierungseinschränkungen die folgenden Schritte für jeweils eine VM-Instanz aus. Dies ermöglicht, dass die Systemdienste (und Ihre zustandsbehafteten Dienste) ordnungsgemäß auf der VM-Instanz heruntergefahren werden, die Sie entfernen, und gleichzeitig neue Replikate an anderer Stelle erstellt werden.

1. Führen Sie in PowerShell `Disable-ServiceFabricNode` mit der Absicht `RemoveNode` aus, um den Knoten zu deaktivieren, der entfernt werden soll. Entfernen Sie den Knotentyp mit der höchsten Zahl. Entfernen Sie bei einem Cluster mit sechs Knoten beispielsweise die VM-Instanz „MyNodeType_5“.
2. Führen Sie `Get-ServiceFabricNode` aus, um sicherzustellen, dass der Status des Knotens tatsächlich in „Deaktiviert“ geändert wurde. Falls nicht, warten Sie, bis der Knoten deaktiviert ist. Dies kann für jeden Knoten einige Stunden dauern. Fahren Sie erst fort, nachdem der Status des Knotens in „Deaktiviert“ geändert wurde.
3. Verringern Sie die Anzahl der virtuellen Computer in diesem Knotentyp um 1. Damit wird die höchste VM-Instanz entfernt.
4. Wiederholen Sie den Anforderungen entsprechend die Schritte 1 bis 3. Skalieren Sie allerdings auf keinen Fall die Anzahl der Instanzen auf den primären Knotentypen auf einen Wert herunter, der unter dem liegt, den die Zuverlässigkeitsstufe verlangt. Eine Liste der empfohlenen Instanzen finden Sie unter [Planen der Service Fabric-Clusterkapazität ](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).
5. Nachdem alle virtuellen Computer ausgeschaltet sind (dargestellt als „Inaktiv“), zeigt „fabric:/System/InfrastructureService/[Knotenname]“ einen Fehlerstatus an. Anschließend können Sie die Clusterressource aktualisieren, um den Knotentyp zu entfernen. Sie können entweder die Bereitstellung per ARM-Vorlage verwenden oder die Clusterressource über [Azure Resource Manager](https://resources.azure.com) bearbeiten. Dadurch wird ein Clusterupgrade gestartet, bei dem der Dienst „fabric:/System/InfrastructureService/[Knotentyp]“ mit dem Fehlerzustand entfernt wird.
 6. Anschließend können Sie optional die VM-Skalierungsgruppe löschen – die Knoten werden aber in der Ansicht im Service Fabric Explorer weiterhin als „Inaktiv“ angezeigt. Der letzte Schritt ist die Bereinigung mit dem Befehl `Remove-ServiceFabricNodeState`.

## <a name="horizontal-scaling"></a>Horizontale Skalierung

Sie können die horizontale Skalierung entweder [manuell](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) oder [programmgesteuert](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling) durchführen.

> [!NOTE]
> Wenn Sie einen Knotentyp mit der Dauerhaftigkeitsstufe „Silber“ oder „Gold“ skalieren, wird der Skalierungsvorgang langsam durchgeführt.

### <a name="scaling-out"></a>Horizontales Skalieren

Skalieren Sie einen Service Fabric-Cluster horizontal hoch, indem Sie die Anzahl der Instanzen für eine bestimmte VM-Skalierungsgruppe erhöhen. Sie können das horizontale Hochskalieren programmgesteuert durchführen, indem Sie mithilfe von `AzureClient` und der ID für die gewünschte Skalierungsgruppe die Kapazität erhöhen.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Für ein manuelles horizontales Hochskalieren aktualisieren Sie die Kapazität in der SKU-Eigenschaft der gewünschten [VM-Skalierungsgruppe](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile).

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Horizontales Herunterskalieren

Beim horizontalen Herunterskalieren müssen mehr Aspekte berücksichtigt werden als beim horizontalen Hochskalieren. Beispiel:

* Service Fabric-Systemdienste werden auf dem primären Knotentyp in Ihrem Cluster ausgeführt. Sie dürfen also niemals die Anzahl der Instanzen für diesen Knotentyp herunterfahren oder so herunterskalieren, dass weniger Instanzen vorhanden sind, als durch die Zuverlässigkeitsstufe vorgegeben ist. 
* Für einen zustandsbehafteten Dienst muss eine bestimmte Anzahl von Knoten stets aktiv sein, um die Verfügbarkeit sicherzustellen und den Zustand des Diensts beizubehalten. Sie benötigen mindestens eine Anzahl von Knoten, die der Anzahl der Zielreplikatgruppen der Partition oder des Diensts entspricht.

Beim manuellen Herunterskalieren gehen Sie folgendermaßen vor:

1. Führen Sie in PowerShell `Disable-ServiceFabricNode` mit der Absicht `RemoveNode` aus, um den Knoten zu deaktivieren, der entfernt werden soll. Entfernen Sie den Knotentyp mit der höchsten Zahl. Entfernen Sie bei einem Cluster mit sechs Knoten beispielsweise die VM-Instanz „MyNodeType_5“.
2. Führen Sie `Get-ServiceFabricNode` aus, um sicherzustellen, dass der Status des Knotens tatsächlich in „Deaktiviert“ geändert wurde. Falls nicht, warten Sie, bis der Knoten deaktiviert ist. Dies kann für jeden Knoten einige Stunden dauern. Fahren Sie erst fort, nachdem der Status des Knotens in „Deaktiviert“ geändert wurde.
3. Verringern Sie die Anzahl der virtuellen Computer in diesem Knotentyp um 1. Damit wird die höchste VM-Instanz entfernt.
4. Wiederholen Sie ggf. die Schritte 1 bis 3, bis die gewünschte Kapazität bereitgestellt wird. Skalieren Sie auf keinen Fall die Anzahl der Instanzen des primären Knotentyps auf einen Wert herunter, der unter dem von der Zuverlässigkeitsstufe vorgegebenen liegt. Eine Liste der empfohlenen Instanzen finden Sie unter [Planen der Service Fabric-Clusterkapazität ](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Um manuell horizontal herunterzuskalieren, aktualisieren Sie die Kapazität in der SKU-Eigenschaft der gewünschten [VM-Skalierungsgruppe](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile).

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

Damit das horizontale Herunterskalieren programmgesteuert durchgeführt werden kann, müssen Sie den Knoten auf das Herunterfahren vorbereiten. Ermitteln Sie den Knoten, der entfernt werden soll (der Knoten der höchsten Instanz). Beispiel:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Deaktivieren und entfernen Sie den Knoten, und verwenden Sie dabei die gleiche `FabricClient`-Instanz (in diesem Fall `client`) und die gleiche Knoteninstanz (in diesem Fall `instanceIdString`), die Sie im vorherigen Code verwendet haben:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Wenn Sie einen Cluster zentral herunterskalieren, wird der entfernte Knoten bzw. die entfernte VM-Instanz im Service Fabric Explorer mit einem fehlerhaften Zustand angezeigt. Eine Erklärung dieses Verhaltens finden Sie unter [Verhaltensweisen von Service Fabric Explorer, die Sie möglicherweise beobachten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer). Ihre Möglichkeiten:
> * Rufen Sie den Befehl [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) mit dem entsprechenden Knotennamen auf.
> * Stellen Sie die [Service Fabric-Hilfsanwendung für die Autoskalierung](https://github.com/Azure/service-fabric-autoscale-helper/) in Ihrem Cluster bereit. Diese Anwendung stellt sicher, dass die herunterskalierten Knoten im Service Fabric Explorer entfernt werden.

## <a name="reliability-levels"></a>Zuverlässigkeitsstufen

Die [Zuverlässigkeitsstufe](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) ist eine Eigenschaft der Service Fabric-Clusterressource. Sie kann für die einzelnen Knotentypen nicht unterschiedlich konfiguriert werden. Sie steuert den Replikationsfaktor der Systemdienste für den Cluster und ist eine Einstellung auf Ebene der Clusterressource. 

Die Zuverlässigkeitsstufe bestimmt die Mindestanzahl von Knoten, über die Ihr primärer Knotentyp verfügen muss. Für die Zuverlässigkeitsstufe können folgende Werte festgelegt werden:

* Platin: Die Systemdienste werden mit der Replikatgruppen-Zielanzahl 7 und 9 Startknoten ausgeführt.
* Gold: Die Systemdienste werden mit der Replikatgruppen-Zielanzahl 7 und 7 Startknoten ausgeführt.
* Silber: Die Systemdienste werden mit der Replikatgruppen-Zielanzahl 5 und 5 Startknoten ausgeführt.
* Bronze: Die Systemdienste werden mit der Replikatgruppen-Zielanzahl 3 und 3 Startknoten ausgeführt.

Die minimale empfohlene Zuverlässigkeitsstufe ist „Silber“.

Die Zuverlässigkeitsstufe wird im Abschnitt der Eigenschaften der Ressource [Microsoft.ServiceFabric/clusters](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters) wie folgt festgelegt:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Dauerhaftigkeitsstufen

> [!WARNING]
> Knotentypen, die mit der Dauerhaftigkeitsstufe „Bronze“ ausgeführt werden, erhalten _keine Berechtigungen_. Die Infrastrukturaufträge, die sich auf die zustandslosen Workloads auswirken, werden nicht angehalten oder verzögert. Dies kann sich auf Ihre Workloads auswirken. 
>
> Verwenden Sie die Dauerhaftigkeitsstufe „Bronze“ nur für Knotentypen, die zustandslose Workloads ausführen. Verwenden Sie für Produktionsworkloads mindestens die Dauerhaftigkeitsstufe „Silber“, um Zustandskonsistenz sicherzustellen. Wählen Sie die entsprechende Zuverlässigkeitsstufe basierend auf den Anweisungen in der [Dokumentation zur Kapazitätsplanung](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) aus.

Die Dauerhaftigkeitsstufe muss in zwei Ressourcen festgelegt werden: Eine ist das Erweiterungsprofil der [VM-Skalierungsgruppenressource](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Die andere finden Sie unter `nodeTypes` in der Ressource [Microsoft.ServiceFabric/Clusterressource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
