---
title: 'Anwendungsupgrade: Weiterführende Themen | Microsoft Docs'
description: Dieser Artikel behandelt einige weiterführende Themen in Bezug auf Upgrades von Service Fabric-Anwendungen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: c37ee8177ba31ac8a5da90fef175a6fbd63a6d75
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72167599"
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric-Anwendungsupgrade: Weiterführende Themen
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Hinzufügen oder Entfernen von Diensttypen während eines Anwendungsupgrades
Wenn einer veröffentlichten Anwendung bei einem Upgrade ein neuer Diensttyp hinzugefügt wird, wird der neue Dienst der bereitgestellten Anwendung hinzugefügt. Ein solches Upgrade hat keine Auswirkungen auf Dienstinstanzen, die bereits Teil der Anwendung waren. Es muss jedoch eine Instanz des hinzugefügten Diensttyps erstellt werden, damit der neue Diensttyp aktiviert wird (siehe [New-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Entsprechend können im Rahmen eines Upgrades Diensttypen auch aus einer Anwendung entfernt werden. Alle Dienstinstanzen des Diensttyps, der entfernt werden soll, müssen jedoch vor dem Upgrade entfernt werden (siehe [Remove-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>Manueller Upgrademodus
> [!NOTE]
> Der *Monitored*-Upgrademodus wird für alle Service Fabric-Upgrades empfohlen.
> Der *UnmonitoredManual*-Upgrademodus sollte ausschließlich bei einem fehlerhaften oder angehaltenen Upgrade in Betracht gezogen werden. 
>
>

Beim *Monitored*-Modus wendet Service Fabric Integritätsrichtlinien an, um sicherzustellen, dass die Anwendung im Verlauf des Upgrades fehlerfrei ist. Wenn die Integritätsrichtlinien verletzt werden, wird abhängig von der angegebenen *FailureAction* das Upgrade entweder angehalten oder automatisch ein Rollback ausgeführt.

Im *UnmonitoredManual*-Modus hat der Anwendungsadministrator vollständige Kontrolle über den Fortschritt des Upgrades. Dieser Modus ist hilfreich, wenn eine benutzerdefinierte Auswertung von Integritätsrichtlinien durchgeführt werden soll oder wenn unkonventionelle Upgrades durchgeführt werden, bei denen die Überwachung der Systemintegrität vollständig umgangen wird (z.B. wenn die Anwendung bereits Datenverluste aufweist). Ein Upgrade in diesem Modus wird automatisch nach Abschluss jeder Upgradedomäne angehalten. Es muss mit [Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) ausdrücklich fortgesetzt werden. Wenn ein Upgrade angehalten wurde und bereit ist, vom Benutzer fortgesetzt zu werden, lautet der Upgradestatus *RollforwardPending* (siehe [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Schließlich eignet sich der *UnmonitoredAuto*-Modus für schnelle Upgradedurchläufe während der Dienstentwicklung oder der Testphase, da keine Benutzereingaben erforderlich sind und keine Richtlinien zur Anwendungsintegrität ausgewertet werden.

## <a name="upgrade-with-a-diff-package"></a>Upgrade mit einem Diff-Paket
Anstatt ein vollständiges Anwendungspaket bereitzustellen, können Upgrades auch durch die Bereitstellung von Diff-Paketen durchgeführt werden. Diese enthalten nur die aktualisierten Code-/Konfigurations-/Datenpakete sowie das vollständige Anwendungsmanifest und sämtliche Dienstmanifeste. Vollständige Anwendungspakete werden für die Erstinstallation einer Anwendung im Cluster benötigt. Nachfolgende Upgrades können über vollständige Anwendungspakete oder Diff-Pakete erfolgen.  

Alle Verweise im Anwendungsmanifest oder den Dienstmanifesten eines Diff-Pakets, die im Anwendungspaket nicht gefunden werden können, werden automatisch durch die derzeit bereitgestellte Version ersetzt.

Szenarien für die Verwendung eines Diff-Pakets:

* Das Anwendungspaket ist sehr umfangreich und enthält Verweise auf mehrere Dienstmanifestdateien sowie mehrere Code-, Konfigurations- oder Datenpakete.
* Das Bereitstellungssystem generiert das Buildlayout direkt aus dem Anwendungsbuildprozess. In diesem Fall weisen neu erstellte Assemblys eine andere Prüfsumme auf, auch wenn sich der Code nicht geändert hat. Bei Verwendung eines vollständigen Anwendungspakets müssten Sie die Version in allen Codepaketen ändern. Bei Verwendung eines Diff-Pakets stellen Sie dagegen nur die geänderten Dateien und die Manifestdateien bereit, in denen sich die Version geändert hat.

Wenn eine Anwendung mit Visual Studio aktualisiert wird, wird automatisch ein Diff-Paket veröffentlicht. Um ein Diff-Paket manuell zu erstellen, müssen das Anwendungsmanifest und die Dienstmanifeste aktualisiert werden. Doch nur die geänderten Pakete dürfen in das endgültige Anwendungspaket eingeschlossen werden.

Beispielsweise beginnen wir mit der folgenden Anwendung (Versionsnummern werden zum einfacheren Verständnis angegeben):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Nehmen Sie an, dass Sie nur das Codepaket von Service1 mithilfe eines Diff-Pakets aktualisieren möchten. Ihre aktualisierte Anwendung enthält die folgenden Versionsänderungen:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

In diesem Fall aktualisieren Sie das Anwendungsmanifest auf 2.0.0 und das Dienstmanifest für Service1, um die Codepaketaktualisierung widerzuspiegeln. Der Ordner Ihres Anwendungspakets hat dann die folgende Struktur:

```text
app1/
  service1/
    code/
```

Das heißt, Sie erstellen wie gewohnt ein vollständiges Anwendungspaket und entfernen dann alle Ordner mit Code-/Konfigurations-/Datenpaketen, deren Version nicht geändert wurde.

## <a name="upgrade-application-parameters-independently-of-version"></a>Aktualisieren von Anwendungsparametern unabhängig von der Version

Manchmal ist es wünschenswert, die Parameter einer Service Fabric-Anwendung zu ändern, ohne die Manifestversion zu ändern. Dies kann ganz einfach mithilfe des Flags **-ApplicationParameter** mit dem Azure Service Fabric PowerShell-Cmdlet **Start-ServiceFabricApplicationUpgrade** erfolgen. Gehen Sie von einer Service Fabric-Anwendung mit den folgenden Eigenschaften aus:

```PowerShell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/Application1

ApplicationName        : fabric:/Application1
ApplicationTypeName    : Application1Type
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : { "ImportantParameter" = "1"; "NewParameter" = "testBefore" }
```

Starten Sie nun das Anwendungsupgrade mithilfe des Cmdlets **Start ServiceFabricApplicationUpgrade**. Dieses Beispiel zeigt ein überwachtes Upgrade, es kann aber auch ein nicht überwachtes Upgrade verwendet werden. Eine vollständige Beschreibung der Flags, die von diesem Cmdlet akzeptiert werden, finden Sie in der [Referenz zum Azure Service Fabric PowerShell-Modul](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps#parameters).

```PowerShell
PS C:\> $appParams = @{ "ImportantParameter" = "2"; "NewParameter" = "testAfter"}

PS C:\> Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/Application1 -ApplicationTypeVers
ion 1.0.0 -ApplicationParameter $appParams -Monitored

```

Vergewissern Sie sich nach dem Upgrade, dass die Anwendung über die aktualisierten Parameter und die gleiche Version verfügt:

```PowerShell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/Application1

ApplicationName        : fabric:/Application1
ApplicationTypeName    : Application1Type
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : { "ImportantParameter" = "2"; "NewParameter" = "testAfter" }
```

## <a name="rolling-back-application-upgrades"></a>Ausführen von Rollbacks für Anwendungsupgrades

Während Upgrades in einem von drei Modi ausgeführt werden können (*Monitored*, *UnmonitoredAuto* oder *UnmonitoredManual*), können Rollback nur im Modus *UnmonitoredAuto* oder *UnmonitoredManual* ausgeführt werden. Ein Rollback im *UnmonitoredAuto*-Modus funktioniert genauso wie das Rollforward – lediglich der Standardwert von *UpgradeReplicaSetCheckTimeout* unterscheidet sich (siehe [Parameter für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md)). Ein Rollback im *UnmonitoredManual*-Modus funktioniert genauso wie das Rollforward – das Rollback wird automatisch nach jeder Upgradedomäne angehalten und muss explizit mit [Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) fortgesetzt werden.

Rollbacks können automatisch ausgelöst werden, wenn die Integritätsrichtlinien eines Upgrades im *Monitored*-Modus mit einer *FailureAction* mit dem Wert *Rollback* verletzt werden (siehe [Parameter für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md)), oder ausdrücklich mit [Start-ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Während des Rollbacks können der Wert von *UpgradeReplicaSetCheckTimeout* und der Modus weiterhin jederzeit mit [Update-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps) geändert werden.

## <a name="next-steps"></a>Nächste Schritte
[Ihre Anwendung mit Visual Studio upgraden](service-fabric-application-upgrade-tutorial.md) beschreibt das Upgraden von Anwendungen mit Visual Studio.

[Upgrade Ihrer Anwendung mithilfe von PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) werden Sie schrittweise durch das Upgrade der Anwendung mithilfe von PowerShell geführt.

Steuern Sie die Upgrades von Anwendungen mithilfe von [Upgradeparametern](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Anwendungsupgrades kompatibel, indem Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)vertraut machen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
