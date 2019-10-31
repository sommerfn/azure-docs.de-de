---
title: Patchen des Windows-Betriebssystems in Ihrem Service Fabric-Cluster | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie mithilfe von Patch Orchestration Application das Patchen des Betriebssystems in einem Service Fabric-Cluster automatisieren.
services: service-fabric
documentationcenter: .net
author: khandelwalbrijeshiitr
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: brkhande
ms.openlocfilehash: a02228593a9d8efc9fb363232da1cede3c80a8b3
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592535"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Patchen des Windows-Betriebssystem in Ihrem Service Fabric-Cluster

> 
> [!IMPORTANT]
> Patch Orchestration Application Version 1.2.* wird seit dem 30. April 2019 nicht mehr unterstützt. Führen Sie daher ein Upgrade auf die aktuelle Version durch.

> [!NOTE]
> Das Durchführen [automatischer Upgrades von Betriebssystemimages in der VM-Skalierungsgruppe](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) ist die bewährte Methode, um das Betriebssystem in Azure gepatcht zu halten. Für automatische Betriebssystemupgrades, die auf VM-Skalierungsgruppen basieren, ist für eine Skalierungsgruppe mindestens die Dauerhaftigkeitsstufe „Silver“ (Silber) erforderlich.
>

 Patch Orchestration Application (POA) ist ein Wrapper für den Repair Manager-Dienst von Azure Service Fabric, der die konfigurationsbasierte Planung von Betriebssystempatches für nicht in Azure gehostete Cluster ermöglicht. POA ist für nicht in Azure gehostete Cluster nicht erforderlich, aber die Planung von Patchinstallationen durch Updatedomänen ist erforderlich, um die Service Fabric-Clusterhosts ohne Ausfallzeiten zu patchen.

POA ist eine Service Fabric-Anwendung, mit der das Patchen von Betriebssystemen in einem Service Fabric-Cluster ohne Ausfallzeiten automatisiert werden kann.

POA bietet die folgenden Funktionen:

- **Automatische Installation von Betriebssystemupdates**. Betriebssystemupdates werden automatisch heruntergeladen und installiert. Clusterknoten werden bei Bedarf ohne Ausfallzeiten des Clusters neu gestartet.

- **Clusterfähiges Patchen und Integration von Integrität**. Beim Anwenden von Updates wird mit POA die Integrität der Clusterknoten überwacht. Die Clusterknoten werden einzeln oder in Updatedomänen aktualisiert. Wenn die Integrität des Clusters aufgrund des Patchingprozesses abnimmt, wird das Patchen unterbrochen, um eine Verschärfung des Problems zu verhindern.

## <a name="internal-details-of-poa"></a>Interne Details von POA

POA besteht aus den folgenden Unterkomponenten:

- **Koordinatordienst:** Dieser zustandsbehaftete Dienst ist für Folgendes zuständig:
    - Koordinieren der Windows Update-Aufträge im gesamten Cluster.
    - Speichern der Ergebnisse der abgeschlossenen Windows Update-Vorgänge.

- **Knoten-Agent-Dienst:** Dieser zustandslose Dienst wird auf allen Service Fabric-Clusterknoten ausgeführt. Der Dienst ist für Folgendes zuständig:
    - Bootstrapping des Knoten-Agent-NT-Diensts.
    - Überwachen des Knoten-Agent-NT-Diensts.

- **Knoten-Agent-NT-Dienst:** Dieser Windows NT-Dienst wird mit erhöhten Berechtigungen (SYSTEM) ausgeführt. Im Gegensatz dazu werden der Knoten-Agent-Dienst und der Koordinatordienst mit niedrigeren Berechtigungen ausgeführt (NETZWERKDIENST). Der Dienst ist für die Ausführung der folgenden Windows Update-Aufträge auf allen Clusterknoten zuständig:
    - Deaktivieren der automatischen Windows-Updates auf dem Knoten
    - Herunterladen und Installieren von Windows-Updates entsprechend der vom Benutzer angegebenen Richtlinie
    - Neustarten des Computers nach der Installation von Windows-Updates
    - Hochladen der Ergebnisse von Windows-Updates in den Koordinatordienst.
    - Senden von Integritätsberichten, wenn bei einem Vorgang nach allen Wiederholungsversuchen Fehler auftreten

> [!NOTE]
> In POA werden über den Repair Manager-Dienst von Service Fabric die Knoten deaktiviert und aktiviert und Integritätsprüfungen durchgeführt. Mit dem in POA erstellten Reparaturtask wird der Windows Update-Fortschritt für die einzelnen Knoten nachverfolgt.

## <a name="prerequisites"></a>Voraussetzungen

> [!NOTE]
> Als Mindestversion für .NET Framework ist Version 4.6 erforderlich.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Aktivieren des Repair Manager-Diensts (sofern er noch nicht ausgeführt wird)

Für POA muss der Repair Manager-Dienst im Cluster aktiviert sein.

#### <a name="azure-clusters"></a>Azure-Cluster

Bei Azure-Clustern mit der Dauerhaftigkeitsstufe „Silver“ ist der Repair Manager-Dienst standardmäßig aktiviert. Bei Azure-Clustern mit der Dauerhaftigkeitsstufe „Gold“ ist der Repair Manager-Dienst möglicherweise aktiviert, abhängig davon, wann diese Cluster erstellt wurden. Bei Azure-Clustern mit der Dauerhaftigkeitsstufe „Bronze“ ist der Repair Manager-Dienst nicht standardmäßig aktiviert. Wenn der Dienst bereits aktiviert ist, wird er im Service Fabric Explorer im Abschnitt mit den Systemdiensten aufgeführt.

##### <a name="the-azure-portal"></a>Das Azure-Portal
Sie können Repair Manager beim Einrichten des Clusters über das Azure-Portal aktivieren. Wählen Sie bei der Konfiguration des Clusters unter **Add-On-Features** die Option **Repair Manager einschließen** aus.

![Abbildung zur Aktivierung von Repair Manager über das Azure-Portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="the-azure-resource-manager-deployment-model"></a>Azure Resource Manager-Bereitstellungsmodell
Alternativ können Sie den Repair Manager-Dienst für neue und vorhandene Service Fabric-Cluster über das [Azure Resource Manager-Bereitstellungsmodell](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) aktivieren. Rufen Sie die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die Beispielvorlagen verwenden oder eine benutzerdefinierte Vorlage für das Azure Resource Manager-Bereitstellungsmodell erstellen. 

Gehen Sie zum Aktivieren des Repair Manager-Diensts mithilfe einer [Vorlage für das Azure Resource Manager-Bereitstellungsmodell](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) wie folgt vor:

1. Überprüfen Sie, ob `apiVersion` für die Ressource *Microsoft.ServiceFabric/clusters* auf *2017-07-01-preview* festgelegt ist. Wenn dies nicht der Fall ist, müssen Sie `apiVersion` auf *2017-07-01-preview* oder höher aktualisieren:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

1. Aktivieren Sie den Repair Manager-Dienst, indem Sie den folgenden Abschnitt `addonFeatures` nach dem Abschnitt `fabricSettings` einfügen:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Nachdem Sie diese Änderungen in der Clustervorlage vorgenommen haben, wenden Sie die Änderungen an, und warten Sie, bis das Update abgeschlossen wurde. Sie sehen nun, dass der Repair Manager-Dienst im Cluster ausgeführt wird. Der Name im Abschnitt für Systemdienste im Service Fabric Explorer lautet *fabric:/System/RepairManagerService*. 

### <a name="standalone-on-premises-clusters"></a>Eigenständige lokale Cluster

Sie können den Repair Manager-Dienst für einen neuen oder vorhandenen Service Fabric-Cluster über die [Konfigurationseinstellungen für eigenständige Windows-Cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) aktivieren.

So aktivieren Sie den Repair Manager-Dienst

1. Überprüfen Sie, ob `apiVersion` in den [allgemeinen Clusterkonfigurationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) wie im Folgenden gezeigt auf *04-2017* oder höher festgelegt ist:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

1. Aktivieren Sie den Repair Manager-Dienst, indem Sie folgenden Abschnitt `addonFeatures` nach dem Abschnitt `fabricSettings` einfügen, wie hier gezeigt:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

1. Aktualisieren Sie das Clustermanifest mit diesen Änderungen, und verwenden Sie dabei das aktualisierte Clustermanifest zum [Erstellen eines neuen Clusters](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) oder zum [Aktualisieren der Clusterkonfiguration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server). 

   Nachdem der Cluster mit einem aktualisierten Clustermanifest ausgeführt wurde, können Sie sehen, dass der Repair Manager-Dienst im Cluster ausgeführt wird. Der Name im Abschnitt für Systemdienste im Service Fabric Explorer lautet *fabric:/System/RepairManagerService*.

### <a name="configure-windows-updates-for-all-nodes"></a>Konfigurieren von Windows-Updates für alle Knoten

Automatische Windows-Updates können zu einer Verringerung der Verfügbarkeit führen, da eventuell mehrere Clusterknoten gleichzeitig neu gestartet werden. In POA wird standardmäßig versucht, die automatischen Windows-Updates auf den einzelnen Clusterknoten zu deaktivieren. Wenn die Einstellungen jedoch von einem Administrator oder durch eine Gruppenrichtlinie verwaltet werden, empfiehlt es sich, die Windows Update-Richtlinie explizit auf „Vor dem Download benachrichtigen“ festzulegen.

## <a name="download-the-application-package"></a>Herunterladen des Anwendungspakets

Um das Anwendungspaket herunterzuladen, wechseln Sie zur [Releaseseite von Patch Orchestration Application](https://github.com/microsoft/Service-Fabric-POA/releases/latest/) auf GitHub.

## <a name="configure-poa-behavior"></a>Konfigurieren des POA-Verhaltens

Sie können das POA-Verhalten entsprechend Ihren Anforderungen konfigurieren. Überschreiben Sie die Standardwerte, indem Sie während der Erstellung oder Aktualisierung einer Anwendung den Anwendungsparameter übergeben. Sie können Anwendungsparameter durch Angeben von `ApplicationParameter` in den Cmdlets `Start-ServiceFabricApplicationUpgrade` oder `New-ServiceFabricApplication` festlegen.

| Parameter        | type                          | Details |
|:-|-|-|
|MaxResultsToCache    |Long                              | Die maximale Anzahl von Windows Update-Ergebnissen, die zwischengespeichert werden sollen. <br><br>Der Standardwert ist 3.000, wobei Folgendes angenommen wird: <br> &nbsp;&nbsp;- Es sind 20 Knoten vorhanden. <br> &nbsp;&nbsp;- Jeden Monat können 5 Updates für einen Knoten durchgeführt werden. <br> &nbsp;&nbsp;- Pro Vorgang können 10 Ergebnisse vorliegen. <br> &nbsp;&nbsp;- Es sollen die Ergebnisse für die letzten drei Monate gespeichert werden. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy gibt die Richtlinie an, die vom Koordinatordienst zum Installieren von Windows-Updates auf den Service Fabric-Clusterknoten verwendet werden soll.<br><br>Zulässige Werte sind: <br>*NodeWise:* Windows-Updates werden immer nur auf jeweils einem Knoten installiert. <br> *UpgradeDomainWise:* Windows-Updates werden immer nur in jeweils einer Updatedomäne installiert. (Allenfalls kann ein Windows-Update für alle Knoten in einer Updatedomäne verwendet werden.)<br><br> Informationen dazu, welche Richtlinie für Ihren Cluster am besten geeignet ist, finden Sie im Abschnitt [Häufig gestellte Fragen](#frequently-asked-questions).
|LogsDiskQuotaInMB   |Long  <br> (Standard: *1024*)               | Die maximale Größe der Patch Orchestration Application-Protokolle in MB, die lokal auf jedem Knoten beibehalten werden können.
| WUQuery               | Zeichenfolge<br>(Standard: *IsInstalled=0*)                | Abfrage zum Abrufen von Windows-Updates. Weitere Informationen finden Sie unter [WuQuery](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx).
| InstallWindowsOSOnlyUpdates | *Boolescher Wert* <br> (Standard: false)                 | Verwenden Sie dieses Flag, um zu steuern, welche Updates heruntergeladen und installiert werden sollen. Folgende Werte sind zulässig. <br>TRUE: installiert nur Updates des Windows-Betriebssystems.<br>FALSE: installiert alle verfügbaren Updates auf dem Computer.          |
| WUOperationTimeOutInMinutes | Int <br>(Standard: *90*)                   | Gibt den Timeoutwert für jeden Windows Update-Vorgang an (Suchen/Herunterladen/Installieren). Wenn der Vorgang nicht innerhalb des angegebenen Timeoutzeitraums abgeschlossen ist, wird er abgebrochen.       |
| WURescheduleCount     | Int <br> (Standard: *5*)                  | Gibt an, wie oft der Dienst das Windows-Update maximal erneut plant, falls bei dem Vorgang wiederholt ein Fehler auftritt.          |
| WURescheduleTimeInMinutes | Int <br>(Standard: *30*) | Das Intervall, nach dem der Dienst die Windows-Updates erneut plant, falls der Fehler weiterhin besteht. |
| WUFrequency           | Durch Trennzeichen getrennte Zeichenfolge (Standard: *Wöchentlich, Mittwoch, 7:00:00*)     | Die Häufigkeit, mit der Windows-Updates installiert werden sollen. Folgende Formate und Werte sind möglich: <br>&nbsp;&nbsp;- Monatlich: TT, HH:MM:SS (z. B. *Monatlich, 5, 12:22:32*)<br>Zulässige Werte für das Feld „TT“ (Tag) sind Zahlen im Bereich 1–28 und „last“. <br> &nbsp;&nbsp;- Wöchentlich, TAG, HH:MM:SS (z. B. *Wöchentlich, Dienstag, 12:22:32*)  <br> &nbsp;&nbsp;- Täglich, HH:MM:SS (z. B. *Täglich, 12:22:32*)  <br> &nbsp;&nbsp;-  *Keine* gibt an, dass keine Windows-Updates durchgeführt werden sollen.  <br><br> Die Zeitangaben erfolgen in UTC.|
| AcceptWindowsUpdateEula | Boolean <br>(Standardwert: *true*) | Wenn Sie dieses Flag festlegen, akzeptiert die Anwendung den Endbenutzer-Lizenzvertrag für Windows Update für den Besitzer des Computers.              |

> [!TIP]
> Wenn Windows-Updates sofort ausgeführt werden sollen, legen Sie `WUFrequency` relativ zum Zeitpunkt der Anwendungsbereitstellung fest. Angenommen, Sie haben einen Testcluster mit fünf Knoten und möchten die App ca. um 17:00 Uhr (UTC) bereitstellen. Wenn Sie davon ausgehen, dass das Upgrade oder die Bereitstellung der Anwendung höchsten 30 Minuten dauert, legen Sie WUFrequency auf *Täglich, 17:30:00* fest.

## <a name="deploy-poa"></a>Bereitstellen von POA

1. Führen Sie alle erforderlichen Schritte zur Vorbereitung des Clusters aus.
1. Stellen Sie POA wie jede andere Service Fabric-Anwendung bereit. Informationen zum Bereitstellen von POA über PowerShell finden Sie unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
1. Übergeben Sie zum Konfigurieren der Anwendung zum Zeitpunkt der Bereitstellung `ApplicationParameter` an das `New-ServiceFabricApplication`-Cmdlet. Zur Vereinfachung wurde das Skript „Deploy.ps1“ zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

    - Stellen Sie mit `Connect-ServiceFabricCluster` eine Verbindung mit einem Service Fabric-Cluster her.
    - Führen Sie das PowerShell-Skript „Deploy.ps1“ mit einem geeigneten `ApplicationParameter`-Wert aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner *PatchOrchestrationApplication* im gleichen Verzeichnis.

## <a name="upgrade-poa"></a>Aktualisieren von POA

Um die POA-Version mithilfe von PowerShell zu aktualisieren, befolgen Sie die Anweisungen unter [Service Fabric-Anwendungsupgrade mithilfe von PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-poa"></a>Entfernen von POA

Befolgen Sie die Anweisungen unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications), um die Anwendung zu entfernen.

Zur Vereinfachung wird das Skript „Undeploy.ps1“ zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

  - Stellen Sie mit ```Connect-ServiceFabricCluster``` eine Verbindung mit einem Service Fabric-Cluster her.
  - Führen Sie das PowerShell-Skript „Undeploy.ps1“ aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner *PatchOrchestrationApplication* im gleichen Verzeichnis.

## <a name="view-the-windows-update-results"></a>Anzeigen der Ergebnisse von Windows Update

Mit POA werden REST-APIs verfügbar gemacht, um für Benutzer die Verlaufsergebnisse anzuzeigen. Es folgt ein Beispiel für das JSON-Ergebnis:

```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2019-05-13T08:44:56.4836889Z",
        "OperationStartTime": "2019-05-13T08:44:33.5285601Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0,
            "HResult": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```

Die JSON-Felder werden in der folgenden Tabelle beschrieben:

Feld | Werte | Details
-- | -- | --
OperationResult | 0: Erfolgreich<br> 1: Erfolgreich mit Fehlern<br> 2: Fehlgeschlagen<br> 3 : Abgebrochen<br> 4: Abgebrochen mit Timeout | Gibt das Ergebnis des Gesamtvorgangs an, der normalerweise die Installation eines oder mehrerer Updates beinhaltet.
ResultCode | Das Gleiche wie bei OperationResult | Dieses Feld gibt das Ergebnis des Installationsvorgangs für ein einzelnes Update an.
OperationType | 1: Installation<br> 0: Suchen und Herunterladen| „Installation“ ist der einzige Wert für OperationType, der standardmäßig in den Ergebnissen angezeigt wird.
WindowsUpdateQuery | Der Standardwert ist „IsInstalled=0“. | Die Windows Update-Abfrage, die für die Suche nach Updates verwendet wurde. Weitere Informationen finden Sie unter [WuQuery](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx).
RebootRequired | TRUE: Neustart war erforderlich<br> FALSE: Neustart war nicht erforderlich | Gibt an, ob ein Neustart erforderlich war, um die Installation von Updates abzuschließen.
OperationStartTime | Datetime | Gibt den Zeitpunkt an, zu dem der Vorgang (Herunterladen/Installation) gestartet wurde.
OperationTime | Datetime | Gibt den Zeitpunkt an, zu dem der Vorgang (Herunterladen/Installation) abgeschlossen wurde.
HRESULT | 0 – erfolgreich<br> Sonstiges – Fehler| Gibt den Grund des Fehlers bei dem Windows-Update mit der updateID „7392acaf-6a85-427c-8a8d-058c25beb0d6“ an.

Wenn noch keine Aktualisierung geplant ist, ist die JSON-Ausgabe leer.

Melden Sie sich beim Cluster an, um Windows Update-Ergebnisse abzufragen. Ermitteln Sie die Replikat-IP-Adresse der primären Adresse für den Koordinatordienst, und öffnen Sie die folgende URL im Browser: http://&lt;REPLIKAT-IP&gt;:&lt;Anwendungsport&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.

Der REST-Endpunkt für den Koordinatordienst verfügt über einen dynamischen Port. Die genaue URL finden Sie im Service Fabric Explorer. Die Ergebnisse sind beispielsweise unter *http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults* verfügbar.

![Abbildung des REST-Endpunkts](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

Wenn der Reverseproxy im Cluster aktiviert ist, können Sie auch von außerhalb des Clusters auf die URL zugreifen.

Sie müssen folgenden Endpunkt aufrufen: *http://&lt;SERVER-URL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults*.

Befolgen Sie zum Aktivieren des Reverseproxys im Cluster die Anweisungen unter [Reverseproxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Nachdem der Reverseproxy konfiguriert wurde, können alle Microservices im Cluster, die einen HTTP-Endpunkt verfügbar machen, außerhalb des Clusters aufgerufen werden.

## <a name="diagnostics-and-health-events"></a>Diagnose und Integritätsereignisse

In diesem Abschnitt wird erläutert, wie das Debuggen oder die Diagnose von Problemen bei Patchupdates über POA in Service Fabric-Clustern durchgeführt wird.

> [!NOTE]
> Um viele der folgenden angegebenen Verbesserungen zur Selbstdiagnose nutzen zu können, muss POA Version 1.4.0 oder höher installiert sein.

Der Knoten-Agent-NT-Dienst erstellt [Reparaturtasks](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtask?view=azure-dotnet) zum Installieren von Updates auf den Knoten. Jeder Task wird dann im Koordinatordienst anhand der Richtlinie zur Taskgenehmigung vorbereitet. Die vorbereiteten Tasks werden schließlich in Repair Manager genehmigt. In diesem Dienst wird kein Task genehmigt, wenn der Cluster sich in einem fehlerhaften Zustand befindet. 

Sie erfahren nun Schritt für Schritt, wie Updates auf einem Knoten durchgeführt werden:

1. Der auf jedem Knoten ausgeführte NodeAgentNTService sucht zum geplanten Zeitpunkt nach verfügbaren Windows-Updates. Wenn Updates verfügbar sind, werden sie auf den Knoten heruntergeladen.

1. Nachdem die Updates heruntergeladen wurden, erstellt der Knoten-Agent-NT-Dienst einen entsprechenden Reparaturtask für den Knoten mit dem Namen *POS___\<eindeutige ID>* . Sie können diese Reparaturtasks mithilfe des Cmdlets [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) oder mithilfe von SFX im Detailbereich des Knotens anzeigen. Nach dem Erstellen des Reparaturtasks wechselt dieser schnell in den [Status *Claimed*](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtaskstate?view=azure-dotnet).

1. Der Koordinatordienst sucht in regelmäßigen Abständen nach Reparaturtasks mit dem Status *Claimed* und aktualisiert sie basierend auf TaskApprovalPolicy in den Status *Preparing*. Wenn TaskApprovalPolicy als NodeWise konfiguriert ist, wird ein Reparaturtask, der einem Knoten entspricht, nur dann vorbereitet, wenn sich derzeit kein anderer Reparaturtask im Status *Preparing*, *Approved*, *Executing* oder *Restoring* befindet. 

   Ebenso sind, wenn TaskApprovalPolicy als UpgradeWise konfiguriert ist, Tasks mit den vorherigen Statuswerten nur für Knoten vorhanden, die zu derselben Updatedomäne gehören. Nachdem ein Reparaturtask in den Status *Preparing* versetzt wurde, wird der entsprechende Service Fabric-Knoten [deaktiviert](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) (Status „Disabled“) und die Absicht auf *Restart* festgelegt.

   POA Version 1.4.0 und höher sendet Ereignisse mit der ClusterPatchingStatus-Eigenschaft an den Koordinatordienst, um die Knoten anzuzeigen, die gepatcht werden. Die Updates werden wie in der folgenden Abbildung gezeigt auf „_poanode_0“ installiert:

    [![Abbildung des Clusterpatchstatus](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png)](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png#lightbox)

1. Nachdem der Knoten deaktiviert wurde, wird der Reparaturtask in den Status *Executing* gesetzt. 
   
   > [!NOTE]
   > Ein Knoten, der im Status *Disabled* hängen bleibt, kann einen neuen Reparaturtask blockieren. Dies führt dazu, dass der Patchvorgang im Cluster angehalten wird.

1. Wenn ein Reparaturtask sich im Status *Executing* befindet, wird die Patchinstallation auf diesem Knoten gestartet. Nachdem der Patch installiert wurde, wird der Knoten je nach Patch neu gestartet oder nicht neu gestartet. Anschließend wird der Reparaturtask in den Status *Restoring* geändert. Dadurch wird der Knoten erneut aktiviert. Der Reparaturtask wird dann als abgeschlossen markiert.

   In POA ab Version 1.4.0 können Sie den Status des Updates ermitteln, indem Sie die Integritätsereignisse für NodeAgentService mit der Eigenschaft „WUOperationStatus-\<Knotenname>“ anzeigen. Die hervorgehobenen Abschnitte in den folgenden Abbildungen zeigen den Status von Windows-Updates für die Knoten *poanode_0* und *poanode_2*:

   [![Abbildung des Status des Windows Update-Vorgangs](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png#lightbox)

   [![Abbildung des Status des Windows Update-Vorgangs](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png#lightbox)

   Sie können die Details auch mithilfe von PowerShell ermitteln. Stellen Sie dazu eine Verbindung mit dem Cluster her, und rufen Sie den Status des Reparaturtasks mithilfe von [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) ab. 
   
   Im folgenden Beispiel befindet sich der Task „POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15“ im Status *DownloadComplete*. Das bedeutet, dass Updates auf den Knoten *poanode_2* heruntergeladen wurden und versucht wird, sie zu installieren, wenn der Task in den Status *Executing* gesetzt ist.

   ``` powershell
    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k = Get-ServiceFabricRepairTask -TaskId "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15"

    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k.ExecutorData
    {"ExecutorSubState":2,"ExecutorTimeoutInMinutes":90,"RestartRequestedTime":"0001-01-01T00:00:00"}
    ```

   Wenn noch weitere Probleme zu finden sind, melden Sie sich bei den spezifischen virtuellen Computern an, und suchen Sie in den Windows-Ereignisprotokollen nach den entsprechenden Informationen. Der zuvor genannte Reparaturtask kann nur die folgenden Executor-Unterstatuswerte aufweisen:

      ExecutorSubState | BESCHREIBUNG
    -- | -- 
      None=1 |  Bedeutet, dass kein laufender Vorgang auf dem Knoten vorlag. Der Status befindet sich möglicherweise in einer Übergangsphase.
      DownloadCompleted=2 | Downloadvorgang wurde erfolgreich, mit Teilfehler oder Fehler abgeschlossen.
      InstallationApproved=3 | Downloadvorgang wurde frühzeitig abgeschlossen. Die Installation wurde in Repair Manager genehmigt.
      InstallationInProgress=4 | Entspricht dem Status der Ausführung des Reparaturtasks.
      InstallationCompleted=5 | Installation wurde erfolgreich, mit Teilerfolg oder Fehler abgeschlossen.
      RestartRequested=6 | Patchinstallation wurde abgeschlossen, und eine Neustartaktion auf dem Knoten steht aus.
      RestartNotNeeded=7 |  Neustart war nach dem Abschluss der Patchinstallation nicht erforderlich.
      RestartCompleted=8 | Neustart wurde erfolgreich abgeschlossen.
      OperationCompleted=9 | Windows Update-Vorgang wurde erfolgreich abgeschlossen.
      OperationAborted=10 | Windows Update-Vorgang wurde abgebrochen.

1. In POA ab Version 1.4.0 wird nach Abschluss eines Updateversuchs auf einem Knoten ein Ereignis mit der Eigenschaft „WUOperationStatus-[Knotenname]“ für NodeAgentService veröffentlicht, um anzugeben, wann der nächste Versuch gestartet wird, die Windows-Updates herunterzuladen und zu installieren. Dies ist in der folgenden Abbildung dargestellt:

     [![Abbildung des Status des Windows Update-Vorgangs](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png#lightbox)

### <a name="diagnostics-logs"></a>Diagnoseprotokolle

Patch Orchestration Application-Protokolle werden innerhalb der Service Fabric-Laufzeitprotokolle erfasst.

Sie können Protokolle mit einem Diagnosetool oder einer Diagnosepipeline Ihrer Wahl erfassen. In POA werden die folgenden festen Anbieter-IDs verwendet, um Ereignisse über die [Ereignisquelle](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1) zu protokollieren:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Integritätsberichte

In POA werden in folgenden Fällen auch Integritätsberichte für den Knoten-Agent-Dienst oder den Koordinatordienst veröffentlicht:

* Ausfall des Knoten-Agent-NT-Diensts

   Wenn der Knoten-Agent-NT-Dienst auf einem Knoten ausgefallen ist, wird für den Knoten-Agent-Dienst ein Integritätsbericht mit Warnstufe generiert.

* Nicht aktivierter Repair Manager-Dienst

   Wenn der Repair Manager-Dienst im Cluster nicht gefunden wurde, wird für den Koordinatordienst ein Integritätsbericht mit Warnstufe generiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F: Warum befindet sich mein Cluster in einem Fehlerzustand, wenn POA ausgeführt wird?**

A: Während des Installationsvorgangs werden Knoten in POA deaktiviert oder neu gestartet. Dies kann vorübergehend zu einem fehlerhaften Cluster führen.

Abhängig von der Richtlinie der Anwendung kann während eines Patchvorgangs jeder einzelne Knoten *oder* eine ganze Updatedomäne auf einmal ausfallen.

Nach Abschluss der Installation der Windows-Updates werden die Knoten neu gestartet und erneut aktiviert.

Im folgenden Beispiel befand sich der Cluster vorübergehend in einem Fehlerzustand, da zwei Knoten ausgefallen waren und die Richtlinie „MaxPercentageUnhealthyNodes“ verletzt wurde. Dies ist ein temporärer Fehler, der auftritt, bis der Patchvorgang gestartet werden kann.

![Abbildung eines Clusters mit Fehler](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sollte das Problem dauerhaft auftreten, lesen Sie die Informationen im Abschnitt zur Problembehandlung.

**F: Was kann ich tun, wenn sich POA im Warnstatus befindet?**

A: Überprüfen Sie, ob der für die Anwendung gesendete Integritätsbericht Angaben zur Grundursache enthält. Üblicherweise enthält die Warnung Details zum Problem. Wenn das Problem vorübergehend ist, wird erwarten, dass die Anwendung automatisch wiederhergestellt wird.

**F: Was kann ich tun, wenn mein Cluster einen Fehler aufweist und ich ein dringendes Betriebssystemupdate ausführen muss?**

A: POA installiert keine Updates, solange der Cluster sich in einem fehlerhaften Zustand befindet. Versuchen Sie, den Cluster in einen fehlerfreien Zustand zu versetzen, um den POA-Workflow fortzuführen.

**F: Soll ich TaskApprovalPolicy für meinen Cluster auf „NodeWise“ oder „UpgradeDomainWise“ festlegen?**

A: Die Einstellung „UpgradeDomainWise“ beschleunigt die gesamte Clusterreparatur, weil alle Knoten einer Updatedomäne gleichzeitig gepatcht werden. Während des Vorgangs sind Knoten, die zu einer vollständigen Updatedomäne gehören, nicht verfügbar ([Status *Disabled*](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled)).

Im Gegensatz dazu wird mit der Einstellung „NodeWise“ nur jeweils ein Knoten gepatcht, d. h., dass das gesamte Patchen des Clusters mehr Zeit in Anspruch nimmt. Jedoch ist während des Patchvorgangs nur maximal ein Knoten nicht verfügbar (Status *Disabled*).

Wenn der Cluster die Ausführung auf N-1 der Updatedomänen während des Patchzyklus tolerieren kann (wobei N die Gesamtanzahl der Updatedomänen im Cluster ist), können Sie die Richtlinie auf „UpgradeDomainWise“ festlegen. Andernfalls legen Sie sie auf „NodeWise“ fest.

**F: Wie lange dauert das Patchen eines Knotens?**

A: Das Patchen eines Knotens kann von wenigen Minuten (z. B. [Windows Defender-Definitionsupdates](https://www.microsoft.com/en-us/wdsi/definitions)) bis zu mehreren Stunden (z. B. [kumulative Windows-Updates](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)) dauern. Der Zeitaufwand für das Patchen eines Knotens hängt größtenteils von Folgendem ab: 
 - Der Größe der Updates
 - Der Anzahl der Updates, die in einem Patchfenster angewandt werden müssen
 - Der zum Installieren der Updates, Neustarten des Knotens (falls erforderlich) und Beenden der nach dem Neustart auszuführenden Installationsschritte benötigten Zeit
 - Der Leistung des virtuellen oder physischen Computers und den Netzwerkbedingungen

**F: Wie lange dauert es, einen kompletten Cluster zu patchen?**

A: Die zum Patchen eines kompletten Clusters benötigte Zeit hängt von folgenden Faktoren ab:

- Der zum Patchen eines Knotens benötigten Zeit

- Richtlinie des Koordinatordiensts. Die Standardrichtlinie „NodeWise“ führt dazu, dass nur jeweils ein Knoten gepatcht wird. Dies dauert länger als die Anwendung der Richtlinie „UpgradeDomainWise“. 

   Beispiel:  Das Patchen eines Knotens dauert ca. 1 Stunde. Ein Cluster mit 20 Knoten (gleichen Typs) mit 5 Updatedomänen mit jeweils 4 Knoten soll gepatcht werden. Dauer des Patchprozesses:
    - Bei „NodeWise“: ca. 20 Stunden
    - Bei „UpgradeDomainWise“: ca. 5 Stunden

- Der Clusterlast: Bei jedem Patchvorgang muss die Kundenworkload auf andere verfügbare Knoten im Cluster verschoben werden. Knoten, die gerade gepatcht werden, haben während dieser Zeit den [Status *Disabling*](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling). Wenn der Cluster in der Nähe der Spitzenlast ausgeführt wird, dauert die Deaktivierung länger. Daher scheint der gesamte Patchprozess unter solchen Belastungsbedingungen langsam zu sein.

- Clusterintegritätsfehlern während des Patchens: Der Patchprozess wird durch jede [Beeinträchtigung](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) der [Integrität des Clusters](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) unterbrochen. Durch dieses Problem erhöht sich die zum Patchen des gesamten Clusters erforderliche Zeit.

**F: Warum werden in den über REST-APIs abgerufenen Windows Update-Ergebnissen Updates angezeigt, nicht aber im Windows Update-Verlauf auf dem Computer?**

A: Einige Produktupdates werden nur im jeweiligen Update- oder Patchverlauf angezeigt. So werden beispielsweise Windows Defender-Updates unter Windows Server 2016 im Windows Update-Verlauf angezeigt oder nicht angezeigt.

**F: Kann POA für das Patchen des Entwicklungsclusters (Einzelknotencluster) verwendet werden?**

A: Nein, POA kann nicht für das Patchen eines Einzelknotenclusters verwendet werden. Diese Einschränkung ist entwurfsbedingt, da es zu Ausfallzeiten bei [Service Fabric-Systemdiensten](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) oder anderen Kunden-Apps kommen kann. Daher würden Reparaturtasks für einen Patch nie in Repair Manager genehmigt werden.

**F: Wie patche ich Clusterknoten unter Linux?**

A: Informationen zum Orchestrieren von Updates unter Linux finden Sie unter [Automatische Upgrades von Betriebssystemimages mit Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

**F: Warum dauert der Updatezyklus so lange?**

A: Fragen Sie das JSON-Ergebnis ab, und geben Sie den Updatezyklus für alle Knoten ein. Dann können Sie versuchen, mithilfe von OperationStartTime und OperationTime(OperationCompletionTime) die benötigte Zeit für die Updateinstallation auf jedem Knoten zu ermitteln. 

Wenn innerhalb eines großen Zeitfensters kein Update ausgeführt wurde, befindet sich der Cluster möglicherweise in einem Fehlerzustand, sodass in Repair Manager keine POA-Reparaturtasks genehmigt werden können. Wenn die Updateinstallation auf einem Knoten lange dauert, wurde der Knoten möglicherweise seit längerer Zeit nicht mehr aktualisiert. Viele Updates sind möglicherweise noch nicht installiert, was zu Verzögerungen führen kann. 

Es ist auch möglich, dass das Patchen auf einem Knoten blockiert wird, weil der Knoten im Status *Disabling* hängen geblieben ist. Dies liegt normalerweise daran, dass die Deaktivierung des Knotens zu einem Quorum- oder Datenverlust führen kann.

**F: Warum muss der Knoten deaktiviert sein, wenn er in POA gepatcht wird?**

A: In POA wird der Knoten mit der Absicht *Restart* deaktiviert, wodurch alle auf dem Knoten ausgeführten Service Fabric-Dienste beendet oder neu zugeordnet werden. Dies soll sicherstellen, dass Anwendungen keine Mischung aus alten und neuen DLLs verwenden. Daher wird empfohlen, einen Knoten nicht zu patchen, ohne ihn zuvor zu deaktivieren.

## <a name="disclaimers"></a>Haftungsausschlüsse

- In POA wird der Endbenutzer-Lizenzvertrag für Windows Update im Namen des Benutzers akzeptiert. Diese Einstellung kann optional in der Konfiguration der Anwendung deaktiviert werden.

- POA erfasst Telemetriedaten zum Nachverfolgen der Nutzung und Leistung. Die Telemetrieeinstellung der Anwendung folgt der Telemetrieeinstellung der Service Fabric-Laufzeit (standardmäßig ist die Einstellung aktiviert).

## <a name="troubleshooting"></a>Problembehandlung

Dieser Abschnitt enthält mögliche Lösungen zur Problembehandlung bei Problemen mit dem Patchen von Knoten.

### <a name="a-node-is-not-coming-back-to-up-state"></a>Ein Knoten wird nicht wieder in den betriebsbereiten Zustand versetzt.

* Der Status *Disabling* des Knotens kann aus folgenden Gründen nicht aufgehoben werden:

  - Eine Sicherheitsprüfung steht aus. Um in dieser Situation Abhilfe zu schaffen, stellen Sie sicher, dass genügend Knoten in fehlerfreiem Zustand verfügbar sind.

* Der Status *Disabled* des Knotens kann aus folgenden Gründen nicht aufgehoben werden:

  - Er wurde manuell deaktiviert.
  - Er wurde aufgrund eines laufenden Azure-Infrastrukturauftrags deaktiviert.
  - Er wurde von POA vorübergehend deaktiviert, um den Knoten zu patchen.

* Der Ausfall des Knotens kann aus folgenden Gründen nicht aufgehoben werden:

  - Er wurde manuell in den Status „Down“ versetzt.
  - Er wird gerade neu gestartet (dies kann durch POA ausgelöst worden sein).
  - Er ist aufgrund eines fehlerhaften virtuellen oder physischen Computers oder aufgrund von Netzwerkverbindungsproblemen ausgefallen.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates wurden auf einigen Knoten übersprungen

POA versucht, ein Windows-Update entsprechend der Neuplanungsrichtlinie zu installieren. Der Dienst versucht gemäß der Anwendungsrichtlinie, den Knoten wiederherzustellen und das Update zu überspringen.

In einem solchen Fall wird ein Integritätsbericht mit Warnstufe für den Knoten-Agent-Dienst generiert. Das Windows Update-Ergebnis enthält ebenfalls mögliche Fehlerursachen.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-is-being-installed"></a>Während der Installation des Updates wird der Cluster in einen Fehlerzustand versetzt

Ein fehlerhaftes Windows-Update kann die Integrität einer Anwendung oder eines Clusters auf einem bestimmten Knoten oder in einer Updatedomäne beeinträchtigen. POA reagiert nicht mehr auf nachfolgende Windows Update-Vorgänge, bis der Cluster wieder fehlerfrei ist.

Ein Administrator muss eingreifen und ermitteln, weshalb die Integrität der Anwendung oder des Clusters aufgrund von Windows Update beeinträchtigt wurde.

## <a name="poa-release-notes"></a>POA-Versionshinweise

>[!NOTE]
> Versionshinweise und Releases für POA Version 1.4.0 und höher finden Sie auf der [Releaseseite von Patch Orchestration Application](https://github.com/microsoft/Service-Fabric-POA/releases/) auf GitHub.

### <a name="version-110"></a>Version 1.1.0
- Öffentliche Version

### <a name="version-111"></a>Version 1.1.1
- Korrektur eines Fehlers in „SetupEntryPoint“ von „NodeAgentService“, der die Installation von „NodeAgentNTService“ verhindert hat.

### <a name="version-120"></a>Version 1.2.0

- Korrektur von Fehler im Zusammenhang mit dem Workflow für den Systemneustart.
- Korrektur eines Fehlers bei der Erstellung von RM-Aufgaben, durch den die Integritätsüberprüfung während Reparaturvorbereitungsaufgaben nicht wie erwartet erfolgt ist.
- Der Startmodus für den Windows-Dienst „POANodeSvc“ wurde von automatisch in verzögert automatisch geändert.

### <a name="version-121"></a>Version 1.2.1

- Fehlerbehebung im Workflow zum Herunterskalieren eines Clusters. Logik der automatischen Speicherbereinigung für POA-Reparaturaufgaben eingeführt, die nicht vorhandenen Knoten angehören.

### <a name="version-122"></a>Version 1.2.2

- Verschiedene Fehlerbehebungen
- Binärdateien werden jetzt signiert.
- SFPKG-Link für die Anwendung wurde hinzugefügt.

### <a name="version-130"></a>Version 1.3.0

- Nach dem Festlegen von InstallWindowsOSOnlyUpdates auf FALSE werden jetzt alle verfügbaren Updates installiert.
- Die Logik für das Deaktivieren automatischer Updates wurde geändert. Dies behebt einen Fehler, durch den automatische Updates auf Server 2016 und höher nicht deaktiviert wurden.
- Einschränkungen der parametrisierten Platzierung für beide Microservices von POA für erweiterte Anwendungsfälle.

### <a name="version-131"></a>Version 1.3.1
- Die Regression, die dazu führt, dass POA 1.3.0 unter Windows Server 2012 R2 oder früher aufgrund eines Fehlers beim Deaktivieren automatischer Updates nicht ausgeführt wird, wurde behoben. 
- Behebung eines Fehlers, der dazu führt, dass die InstallWindowsOSOnlyUpdates-Konfiguration immer als „True“ verwendet wird
- Änderung des Standardwerts von „InstallWindowsOSOnlyUpdates“ in „False“

### <a name="version-132"></a>Version 1.3.2
- Ein Problem, das den Patchinglebenszyklus auf einem Knoten beeinflusst, wenn Knoten einen Namen aufweisen, der Teil des aktuellen Knotennamens ist, wurde behoben. Bei diesen Knoten wurden unter Umständen keine Patchvorgänge ausgeführt, oder ein Neustart steht aus.