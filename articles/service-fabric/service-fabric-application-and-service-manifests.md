---
title: Beschreibung von Azure Service Fabric-Apps und -Diensten | Microsoft Docs
description: Beschreibt, wie Manifeste zum Definieren von Service Fabric-Anwendungen und -Diensten verwendet werden.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/12/2019
ms.author: atsenthi
ms.openlocfilehash: a5e452bf3dc9f35c345a5f27af829904b4839ece
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977128"
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric-Anwendungs- und -Dienstmanifeste
In diesem Artikel wird beschrieben, wie Service Fabric-Anwendungen und -Dienste mit den Dateien „ApplicationManifest.xml“ und „ServiceManifest.xml“ definiert und mit Versionsangaben versehen werden.  Ausführlichere Beispiele finden Sie unter [Beispiele für Anwendungs- und Dienstmanifeste](service-fabric-manifest-examples.md).  Eine Dokumentation des XML-Schemas für diese Manifestdateien finden Sie unter [ServiceFabricServiceModel.xsd – Schemadokumentation](service-fabric-service-model-schema.md).

> [!WARNING]
> Das XML-Dateischema des Manifests erzwingt die richtige Reihenfolge der untergeordneten Elemente.  Öffnen Sie als teilweise Problemumgehung „C: \Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd“ in Visual Studio, während Sie eines der Manifeste von Service Fabric erstellen oder ändern. Auf diese Weise können Sie die Reihenfolge der untergeordneten Elemente überprüfen und IntelliSense nutzen.

## <a name="describe-a-service-in-servicemanifestxml"></a>Beschreiben eines Diensts in der Datei „ServiceManifest.xml“
Das Dienstmanifest definiert deklarativ der Diensttyp und die Version. Es legt Dienstmetadaten wie Diensttyp, Integritätseigenschaften, Lastenausgleichsmetriken, Dienstbinärdateien und Konfigurationsdateien fest.  Anders ausgedrückt: Es beschreibt den Code, die Konfiguration und die Datenpakete, die ein Dienstpaket bilden, mit dem ein oder mehrere Diensttypen unterstützt werden. Ein Dienstmanifest kann mehrere Code-, Konfigurations- und Datenpakete enthalten, die unabhängig voneinander mit Versionsangaben versehen werden können. Hier ist ein Dienstmanifest für den ASP.NET Core-Web-Front-End-Dienst der [Voting-Beispielanwendung](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) angegeben (und hier einige [ausführlichere Beispiele](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Version** -Attribute sind unstrukturierte Zeichenfolgen und werden im System nicht analysiert. Version-Attribute werden dazu verwendet, jede Komponente für Upgrades mit Versionsangaben zu versehen.

**ServiceTypes** deklariert, welche Diensttypen von **CodePackages** in diesem Manifest unterstützt werden. Wenn ein Dienst mit einem dieser Diensttypen instanziiert wird, werden alle Codepakete, die in diesem Manifest deklariert sind, durch Ausführen ihrer Einstiegspunkte aktiviert. Die resultierenden Prozesse registrieren die unterstützten Diensttypen zur Laufzeit. Diensttypen werden auf Ebene der Manifeste und nicht auf Ebene der Codepakete deklariert. Wenn mehrere Codepakete vorhanden sind, werden sie daher alle aktiviert, wenn im System nach einem der deklarierten Diensttypen gesucht wird.

Die durch **EntryPoint** angegebene ausführbare Datei ist üblicherweise der Diensthost mit langer Ausführungsdauer. **SetupEntryPoint** ist ein privilegierter Einstiegspunkt, der mit den gleichen Anmeldeinformationen wie Service Fabric (i.d.R. dem *LocalSystem*-Konto) vor jedem anderen Einstiegspunkt ausgeführt wird.  Das Vorhandensein eines separaten Setupeinstiegspunkts vermeidet, dass der Diensthost über längere Zeiträume mit hohen Berechtigungen ausgeführt werden muss. Die von **EntryPoint** angegebene ausführbare Datei wird ausgeführt, nachdem **SetupEntryPoint** erfolgreich beendet wurde. Wenn der Prozess beendet wird oder abstürzt, wird der resultierende Prozess überwacht und neu gestartet (er beginnt wieder mit **SetupEntryPoint**).  

Gängige Szenarios für die Verwendung von **SetupEntryPoint** sind die Ausführung einer ausführbaren Datei vor dem Starten des Diensts sowie die Ausführung eines Vorgangs mit erhöhten Rechten. Beispiel:

* Einrichten und Initialisieren von Umgebungsvariablen, die die ausführbare Datei des Diensts benötigt. Dies ist nicht auf Dateien beschränkt, die mit den Service Fabric-Programmiermodellen geschrieben wurden. „npm.exe“ benötigt beispielsweise einige Umgebungsvariablen, die zum Bereitstellen einer node.js-Anwendung konfiguriert wurden.
* Einrichten einer Zugriffssteuerung durch Installieren von Sicherheitszertifikaten.

Weitere Informationen zum Konfigurieren von SetupEntryPoint finden Sie unter [Konfigurieren der Richtlinie für einen Setupeinstiegspunkt für Dienste](service-fabric-application-runas-security.md).

**EnvironmentVariables** (im vorhergehenden Beispiel nicht festgelegt) enthält eine Liste von Umgebungsvariablen, die für dieses Codepaket festgelegt wurden. Umgebungsvariablen können in `ApplicationManifest.xml` überschrieben werden, um unterschiedliche Werte für verschiedene Dienstinstanzen bereitzustellen. 

**DataPackage** (im vorhergehenden Beispiel nicht festgelegt) deklariert einen Ordner, der durch das **Name**-Attribut benannt wird und beliebige statische Daten enthält, die zur Laufzeit vom Prozess verarbeitet werden sollen.

**ConfigPackage** deklariert einen Ordner, der durch das **Name**-Attribut benannt wird und die Datei *Settings.xml* enthält. Diese Einstellungsdatei enthält Abschnitte mit benutzerdefinierten Einstellungen für Schlüssel-Wert-Paare, die der Prozess zur Laufzeit einliest. Wenn sich während eines Upgrades nur die **Version** von **ConfigPackage** geändert hat, wird der ausgeführte Prozess nicht neu gestartet. Stattdessen benachrichtigt ein Rückruf den Prozess, dass sich Konfigurationseinstellungen geändert haben, sodass sie dynamisch neu geladen werden können. Hier ein Beispiel der Datei *Settings.xml* :

```xml
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

Ein Service Fabric-Dienst**endpunkt** ist ein Beispiel für eine Service Fabric-Ressource. Eine Service Fabric-Ressource kann deklariert/geändert werden, ohne dass der kompilierte Code geändert wird. Der Zugriff auf die im Dienstmanifest angegebenen Service Fabric-Ressourcen kann über das **SecurityGroup**-Element im Anwendungsmanifest gesteuert werden. Wenn eine Endpunktressource im Dienstmanifest definiert wird, weist Service Fabric Ports aus dem Bereich der reservierten Anwendungsports zu, sofern nicht explizit ein Port angegeben wird. Erfahren Sie mehr über das [Angeben oder Überschreiben von Endpunktressourcen](service-fabric-service-manifest-resources.md).

 
> [!WARNING]
> Statische Ports sollten sich nicht mit dem im ClusterManifest angegebenen Anwendungsportbereich überschneiden. Wenn Sie einen statischen Port angeben, weisen Sie ihn außerhalb des Anwendungsportbereichs zu, andernfalls führt dies zu Portkonflikten. Mit Release 6.5CU2 werden wir eine **Integritätswarnung** ausgeben, wenn wir einen solchen Konflikt erkennen, aber die Bereitstellung in Übereinstimmung mit dem ausgelieferten 6.5-Verhalten fortsetzen. Es ist jedoch möglich, dass wir die Anwendungsbereitstellung ab den nächsten Hauptversionen unterbinden.
>

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Beschreiben einer Anwendung in der Datei „ApplicationManifest.xml“
Das Anwendungsmanifest beschreibt deklarativ den Typ und die Version der Anwendung. Es gibt Dienstzusammensetzungs-Metadaten wie z. B. dauerhafte Namen, Partitionierungsschema, Anzahl der Instanzen/Replikationsfaktor, Richtlinie für Sicherheit/Isolation, Platzierungseinschränkungen, Konfigurationsüberschreibungen und zugehörige Diensttypen an. Zudem werden die Lastenausgleichsdomänen beschrieben, in denen die Anwendung eingefügt wird.

Somit beschreibt ein Anwendungsmanifest Elemente auf Anwendungsebene, verweist auf ein oder mehrere Dienstmanifeste und bildet so einen Anwendungstyp. Hier ist das Anwendungsmanifest für die [Beispielanwendung „Voting“](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) angegeben (und hier einige [ausführlichere Beispiele](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
         <PlacementConstraints>(NodeType==NodeType0)</PlacementConstraints
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Wie bei Dienstmanifesten sind **Version** -Attribute unstrukturierte Zeichenfolgen, die im System nicht analysiert werden. Version-Attribute werden zudem dazu verwendet, jede Komponente für Upgrades mit Versionsangaben zu versehen.

**Parameter** definiert die im gesamten Anwendungsmanifest verwendeten Parameter. Die Werte dieser Parameter können angegeben werden, wenn die Anwendung instanziiert wird, und sie können die Anwendungs- oder Dienstkonfigurationseinstellungen außer Kraft setzen.  Der Standardparameterwert wird verwendet, wenn der Wert nicht bei der Anwendungsinstanziierung geändert wird. Weitere Informationen zum Verwalten verschiedener Anwendungs- und Dienstparameter für einzelne Umgebungen finden Sie unter [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** enthält Verweise auf Dienstmanifeste zum Erstellen des entsprechenden Anwendungstyps. Ein Anwendungsmanifest kann mehrere Dienstmanifestimporte enthalten, die jeweils unabhängig voneinander mit Versionsangaben versehen werden können. Importierte Dienstmanifeste legen fest, welche Diensttypen innerhalb des Anwendungstyps gültig sind. In ServiceManifestImport überschreiben Sie Konfigurationswerte in „Settings.xml“ und Umgebungsvariablen in „ServiceManifest.xml“. **Richtlinien** (im vorhergehenden Beispiel nicht festgelegt) für die Endpunktbindung, für Sicherheit und Zugriff sowie für die Paketfreigabe können in importierten Dienstmanifesten festgelegt werden.  Weitere Informationen finden Sie unter [Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung](service-fabric-application-runas-security.md).

**DefaultServices** deklariert Dienstinstanzen, die automatisch erstellt werden, wenn eine Anwendung mit diesem Anwendungstyp instanziiert wird. Standarddienste dienen lediglich der Benutzerfreundlichkeit und verhalten sich nach der Erstellung in jeder Hinsicht wie normale Dienste. Sie werden zusammen mit allen anderen Diensten in der Anwendungsinstanz aktualisiert und können auch entfernt werden. Ein Anwendungsmanifest kann mehrere Standarddienste enthalten.

**Zertifikate** (im vorhergehenden Beispiel nicht festgelegt) deklariert die zum [Einrichten von HTTPS-Endpunkten](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) oder zum [Verschlüsseln der geheimen Schlüssel im Anwendungsmanifest](service-fabric-application-secret-management.md) verwendeten Zertifikate.

**Platzierungseinschränkungen** sind Anweisungen, mit denen Sie definieren, an welcher Stelle Dienste ausgeführt werden sollen. Diese Anweisungen werden an einzelne Dienste angefügt, die Sie für mindestens eine Knoteneigenschaft auswählen. Weitere Informationen finden Sie unter [Syntax von Platzierungseinschränkungen und Knoteneigenschaften](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#placement-constraints-and-node-property-syntax).

**Richtlinien** (im vorherigen Beispiel nicht festgelegt) beschreibt die Richtlinien für die Protokollsammlung, das [standardmäßige Ausführungskonto](service-fabric-application-runas-security.md), die [Integrität](service-fabric-health-introduction.md#health-policies) und den [Sicherheitszugriff](service-fabric-application-runas-security.md), die auf Anwendungsebene festzulegen sind. Dazu gehört auch, ob die Dienste Zugriff auf die Service Fabric-Runtime haben.

> [!NOTE] 
> Standardmäßig haben Service Fabric-Anwendungen Zugriff auf die Service Fabric-Runtime in Form eines Endpunkts, der anwendungsspezifische Anforderungen akzeptiert, und in Form von Umgebungsvariablen, die auf Dateipfade auf dem Host verweisen, die Fabric- und anwendungsspezifische Dateien enthalten. Sie sollten erwägen, diesen Zugriff zu deaktivieren, wenn die Anwendung nicht vertrauenswürdigen Code hostet (d. h. Code, dessen Herkunft unbekannt ist oder von dem der Anwendungsbesitzer weiß, dass bei seiner Ausführung die Sicherheit gefährdet ist). Weitere Informationen finden Sie unter [Bewährte Methoden für die Sicherheit in Service Fabric](service-fabric-best-practices-security.md#platform-isolation). 
>

**Prinzipale** (im vorhergehenden Beispiel nicht festgelegt) beschreiben die Sicherheitsprinzipale (Benutzer oder Gruppen), die zum [Ausführen von Diensten und sicheren Dienstressourcen](service-fabric-application-runas-security.md) benötigt werden.  Auf Prinzipale wird in den Abschnitten über **Richtlinien** verwiesen.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Nächste Schritte
- [Packen einer Anwendung](service-fabric-package-apps.md) und Vorbereiten der Anwendung für die Bereitstellung.
- [Bereitstellen und Entfernen von Anwendungen](service-fabric-deploy-remove-applications.md)
- [Konfigurieren von Parametern und Umgebungsvariablen für verschiedene Anwendungsinstanzen](service-fabric-manage-multiple-environment-app-configuration.md)
- [Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung](service-fabric-application-runas-security.md)
- [Einrichten von HTTPS-Endpunkten](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service)
- [Verschlüsseln der geheimen Schlüssel im Anwendungsmanifest](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



