---
title: Aktivieren von Azure Monitor (Vorschauversion) für eine Hybridumgebung | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs für eine Hybridcloudumgebung aktivieren, die einen oder mehrere virtuelle Computer enthält.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: e9e5504125920cedaf383f8fa4299a4b1b1d60ed
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553869"
---
# <a name="enable-azure-monitor-for-vms-preview-for-a-hybrid-environment"></a>Aktivieren von Azure Monitor für VMs (Vorschauversion) für eine Hybridumgebung

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In diesem Artikel wird erläutert, wie Sie Azure Monitor für VMs (Vorschauversion) für virtuelle oder physische Computer aktivieren, die in Ihrem Rechenzentrum oder einer anderen Cloudumgebung gehostet sind. Nachdem Sie diesen Prozess durchgeführt haben, haben Sie erfolgreich damit begonnen, die virtuellen Computer in Ihrer Umgebung zu überwachen, und werden erfahren, ob Leistungs- oder Verfügbarkeitsprobleme bei diesen Computern auftreten. 

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](vminsights-enable-overview.md) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen auch diese Anforderungen erfüllen. Überprüfen Sie die Anforderungen und Bereitstellungsmethoden für den [Log Analytics-Linux- und -Windows-Agent](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>Der Dependency-Agent für das Zuordnungsfeature von Azure Monitor für VMs überträgt selbst keine Daten und erfordert keine Änderungen an Firewalls oder Ports. Die Zuordnungsdaten werden immer vom Log Analytics-Agent an den Azure Monitor-Dienst übertragen, entweder direkt oder über das [Operations Management Suite-Gateway](../../azure-monitor/platform/gateway.md), falls Ihre IT-Sicherheitsrichtlinien es nicht zulassen, dass Computer im Netzwerk eine Verbindung mit dem Internet herstellen.

Die Schritte zur Ausführung dieser Aufgabe sind wie folgt zusammengefasst:

1. Installieren Sie den Log Analytics-Agent für Windows oder Linux. Bevor Sie den Agent installieren, lesen Sie den Artikel zur [Übersicht über den Log Analytics-Agent](../platform/log-analytics-agent.md), um sich über die Systemvoraussetzungen und die Bereitstellungsmethoden zu informieren.

2. Laden Sie den Dependency-Agent für das Zuordnungsfeature von Azure Monitor für VMs für [Windows](https://aka.ms/dependencyagentwindows) oder [Linux](https://aka.ms/dependencyagentlinux) herunter, und installieren Sie ihn.

3. Aktivieren Sie das Sammeln von Leistungsindikatoren.

4. Stellen Sie Azure Monitor für VMs bereit.

>[!NOTE]
>Die in diesem Artikel beschriebenen Informationen zum Bereitstellen des Dependency-Agents gelten auch für die [Dienstzuordnungslösung](service-map.md).  

## <a name="install-the-dependency-agent-on-windows"></a>Installieren des Dependency-Agents unter Windows

Sie können den Dependency-Agent durch Ausführen von `InstallDependencyAgent-Windows.exe` manuell auf Windows-Computern installieren. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Setup-Assistent gestartet, mit dem Sie die Installation des Agent interaktiv durchführen können.

>[!NOTE]
>Zum Installieren oder Deinstallieren des Agent sind *Administratorrechte* erforderlich.

In der folgenden Tabelle sind die Parameter hervorgehoben, die von Setup für den Agent auf der Befehlszeile unterstützt werden.

| Parameter | BESCHREIBUNG |
|:--|:--|
| /? | Gibt eine Liste der Befehlszeilenoptionen zurück. |
| /S | Führt eine automatische Installation ohne Benutzereingriff aus. |

Um das Installationsprogramm beispielsweise mit dem Parameter `/?` auszuführen, geben Sie **InstallDependencyAgent-Windows.exe /?** ein.

Dateien für den Dependency-Agent für Windows werden standardmäßig in *C:\Programme\Microsoft Dependency Agent* installiert. Falls der Dependency-Agent nach Abschluss des Setups nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist *%Programfiles%\Microsoft Dependency Agent\logs*.

## <a name="install-the-dependency-agent-on-linux"></a>Installieren des Dependency-Agents unter Linux

Der Dependency-Agent wird auf Linux-Servern mit *InstallDependencyAgent-Linux64.bin* installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit `sh` ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

>[!NOTE]
> Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.
>

| Parameter | BESCHREIBUNG |
|:--|:--|
| -help | Ruft eine Liste der Befehlszeilenoptionen ab. |
| -s | Führt eine automatische Installation ohne Benutzereingaben aus. |
| --check | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Um das Installationsprogramm beispielsweise mit dem Parameter `-help` auszuführen, geben Sie **InstallDependencyAgent-Linux64.bin -help** ein.

Installieren Sie den Dependency-Agent für Linux mit Root-Berechtigungen, indem Sie den Befehl `sh InstallDependencyAgent-Linux64.bin` ausführen.

Falls der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis */var/opt/microsoft/dependency-agent/log*.

Dateien für den Dependency-Agent werden in den folgenden Verzeichnissen gespeichert:

| Dateien | Location |
|:--|:--|
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Beispiele für Installationsskripts

Mit dem folgenden Skriptbeispiel zum Herunterladen und Installieren des Dependency-Agents unter Windows oder Linux können Sie den Dependency-Agent ganz einfach auf mehreren Servern gleichzeitig bereitstellen.

### <a name="powershell-script-for-windows"></a>PowerShell-Skript für Windows

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Shellskript für Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Desired State Configuration

Zum Bereitstellen des Dependency-Agents über Desired State Configuration (DSC) können Sie das Modul „xPSDesiredStateConfiguration“ mit dem folgenden Beispielcode verwenden:

```powershell
configuration ServiceMap {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage 
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```

## <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren

Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Hierzu stehen zwei Möglichkeiten zur Verfügung:
* Manuell, wie in [Windows- und Linux-Leistungsindikatoren in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md) beschrieben.
* Durch Herunterladen und Ausführen eines PowerShell-Skripts, das über den [Azure PowerShell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.

## <a name="deploy-azure-monitor-for-vms"></a>Bereitstellen von Azure Monitor für VMs

Diese Methode umfasst eine JSON-Vorlage, die die Konfiguration zum Aktivieren der Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich angibt.

Wenn Sie nicht wissen, wie Ressourcen mithilfe einer Vorlage bereitgestellt werden, lesen Sie die folgenden Artikel:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade der Azure CLI finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Erstellen und Ausführen von Vorlagen

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Speichern Sie diese Datei als *installsolutionsforvminsights.json* in einem lokalen Ordner.

1. Erfassen Sie die Werte für *WorkspaceName*, *ResourceGroupName* und *WorkspaceLocation*. Der Wert für *WorkspaceName* ist der Name des Log Analytics-Arbeitsbereichs. Der Wert für *WorkspaceLocation* ist die Region, in der der Arbeitsbereich definiert ist.

1. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```
   Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätszustand und Metriken für den Hybridcomputer angezeigt werden.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="vm-doesnt-appear-on-the-map"></a>Der virtuelle Computer wird auf der Karte nicht angezeigt

Wenn die Installation des Dependency-Agents erfolgreich war, der Computer jedoch nicht auf der Karte angezeigt wird, diagnostizieren Sie das Problem, indem Sie die folgenden Schritte ausführen.

1. Wurde der Dependency-Agent erfolgreich installiert? Überprüfen Sie, ob der Dienst installiert wurde und ausgeführt wird.

    **Windows:** Suchen Sie nach dem Dienst „Microsoft Dependency-Agent“. 

    **Linux:** Suchen Sie nach dem laufenden Prozess „microsoft-dependency-agent“.

2. Nutzen Sie den [Tarif „Free“ von Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? Der kostenlose Plan („Free“) lässt bis zu fünf eindeutige Computer zu. Alle weiteren Computer werden auf der Karte nicht angezeigt, selbst wenn die vorherigen fünf keine Daten mehr senden.

3. Sendet der Computer Protokoll- und Leistungsdaten an Azure Monitor-Protokolle? Führen Sie die folgende Abfrage für Ihren Computer aus: 

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Hat sie ein oder mehrere Ergebnisse zurückgegeben? Sind die Daten aktuell? Wenn dies der Fall ist, funktioniert Ihr Log Analytics-Agent ordnungsgemäß und kommuniziert mit dem Dienst. Wenn nicht, überprüfen Sie den Agent auf dem Server: [Behandeln von Problemen mit dem Log Analytics-Agent für Windows](../platform/agent-windows-troubleshoot.md) oder [Behandeln von Problemen beim Linux-Agent für Log Analytics](../platform/agent-linux-troubleshoot.md).

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Der Computer wird auf der Karte angezeigt, enthält aber keine Prozesse

Wenn Ihr Server auf der Karte angezeigt wird, aber keine Prozess- oder Verbindungsdaten enthält, weist dies darauf hin, dass der Dependency-Agent installiert ist und ausgeführt wird, der Kerneltreiber aber nicht geladen wurde. 

Überprüfen Sie die Datei „C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log“ (Windows) bzw. die Datei „/var/opt/microsoft/dependency-agent/log/service.log“ (Linux). Die letzten Zeilen der Datei sollten den Grund angeben, warum der Kernel nicht geladen wurde. Beispielsweise, weil der Kernel nicht unterstützt wird, was unter Linux nach der Aktualisierung des Kernels auftreten kann.


## <a name="next-steps"></a>Nächste Schritte

Nachdem die Überwachung für Ihre virtuellen Computer aktiviert wurde, stehen diese Informationen für die Analyse mit Azure Monitor für VMs zur Verfügung.
 
- Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Azure Monitor für VMs – Zuordnung anzeigen](vminsights-maps.md).

- Informationen zum Erkennen von Engpässen und der Gesamtauslastung im Hinblick auf die Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md).
