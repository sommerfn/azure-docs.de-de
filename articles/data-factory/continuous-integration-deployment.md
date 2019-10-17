---
title: Continuous Integration und Continuous Delivery in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Continuous Integration und Continuous Delivery verwenden, um Data Factory-Pipelines aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere zu verschieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: ff1d34852890a8d5005153ebdfa2fa0f9749d129
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030618"
---
# <a name="continuous-integration-and-delivery-cicd-in-azure-data-factory"></a>Continuous Integration und Continuous Delivery (CI/CD) in Azure Data Factory

## <a name="overview"></a>Übersicht

Bei Continuous Integration wird jede Änderung, die an Ihrer Codebasis vorgenommen wird, automatisch und so früh wie möglich getestet. Der Continuous Delivery-Prozess folgt auf das Testen während des Continuous Integration-Prozesses. Änderungen werden dabei in ein Staging- oder Produktionssystem gepusht.

In Azure Data Factory bedeuten Continuous Integration und Continuous Delivery, dass Data Factory-Pipelines von einer Umgebung (Entwicklung, Test, Produktion) in eine andere verschoben werden. Für die Durchführung von Continuous Integration und Continuous Delivery können Sie die Data Factory-Benutzeroberflächenintegration in Azure Resource Manager-Vorlagen verwenden. Auf der Data Factory-Benutzeroberfläche kann eine Resource Manager-Vorlage über die Dropdownliste **ARM-Vorlage** generiert werden. Wenn Sie **Export ARM template** (ARM-Vorlage exportieren) wählen, generiert das Portal die Resource Manager-Vorlage für die Data Factory und eine Konfigurationsdatei mit Ihren gesamten Verbindungszeichenfolgen und anderen Parametern. Anschließend müssen Sie eine Konfigurationsdatei für jede Umgebung erstellen (Entwicklung, Test, Produktion). Die Hauptdatei mit der Resource Manager-Vorlage bleibt für alle Umgebungen gleich.

Das folgende Video enthält eine neun-minütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="continuous-integration-lifecycle"></a>Continuous Integration-Lebenszyklus

Nachfolgend finden Sie eine Übersicht über einen Continuous Integration und Continuous Delivery-Beispiellebenszyklus in einer mit Azure Repos Git konfigurierten Azure Data Factory. Weitere Informationen zum Konfigurieren eines Git-Repositorys finden Sie unter [Quellcodeverwaltung in Azure Data Factory](source-control.md).

1.  Eine Data Factory für die Entwicklung wird mit Azure Repos Git erstellt und konfiguriert. Dabei verfügen alle Entwickler über die Berechtigung zum Erstellen von Data Factory-Ressourcen, z. B. Pipelines und Datasets.

1.  Wenn die Entwickler Änderungen in ihrem Featurebranch vornehmen, debuggen sie die Pipelineausführungen mit den neuesten Änderungen. Weitere Informationen zum Debuggen einer Pipelineausführung finden Sie unter [Iteratives Entwickeln und Debuggen mit Azure Data Factory](iterative-development-debugging.md).

1.  Wenn die Entwickler mit ihren Änderungen zufrieden sind, erstellen sie einen Pull Request von ihrem Featurebranch zum Master- oder Kollaborationsbranch, um die Änderungen von anderen Entwicklern prüfen zu lassen.

1.  Nachdem der Pull Request genehmigt wurde und Änderungen im Masterbranch zusammengeführt wurden, kann die Veröffentlichung in der Entwicklungsfactory erfolgen.

1.  Wenn das Team bereit ist, die Änderungen in der Testfactory und dann in der Produktionsfactory bereitzustellen, exportiert es die Resource Manager-Vorlage aus dem Masterbranch.

1.  Die exportierte Resource Manager-Vorlage wird mit unterschiedlichen Parameterdateien in der Testfactory und der Produktionsfactory bereitgestellt.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Erstellen einer Resource Manager-Vorlage für jede Umgebung

Wählen Sie in der Dropdownliste **ARM-Vorlage** den Eintrag **Export ARM template** (ARM-Vorlage exportieren) aus, um die Resource Manager-Vorlage für Ihre Data Factory in der Entwicklungsumgebung zu exportieren.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

Wählen Sie in den Test- und Produktionsfactorys den Eintrag **Import ARM template** (ARM-Vorlage importieren) aus. Mit dieser Aktion gelangen Sie zum Azure-Portal, in dem sie die exportierte Vorlage importieren können. Wählen Sie **Eigene Vorlage im Editor erstellen** aus, um den Editor für Resource Manager-Vorlagen zu öffnen.

![](media/continuous-integration-deployment/continuous-integration-image3.png) 

Klicken Sie auf **Datei laden**, und wählen Sie die generierte Resource Manager-Vorlage aus.

![](media/continuous-integration-deployment/continuous-integration-image4.png)

Geben Sie im Einstellungsbereich die Konfigurationswerte ein, z. B. die Anmeldeinformationen für verknüpfte Dienste. Klicken Sie anschließend auf **Kaufen**, um die Resource Manager-Vorlage bereitzustellen.

![](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Verbindungszeichenfolgen

Informationen zum Konfigurieren von Verbindungszeichenfolgen finden Sie in den Artikeln zu den einzelnen Connectors. Informationen zu Azure SQL-Datenbank finden Sie z.B. unter [Kopieren von Daten nach und aus Azure SQL-Datenbank mithilfe von Azure Data Factory](connector-azure-sql-database.md). Zum Überprüfen einer Verbindungszeichenfolge können Sie die Codeansicht für die Ressource auf der Data Factory-Benutzeroberfläche öffnen. In der Codeansicht werden das Kennwort und der Kontoschlüssel der Verbindungszeichenfolge nicht angezeigt. Um die Codeansicht zu öffnen, wählen Sie das Symbol aus, das im folgenden Screenshot hervorgehoben ist.

![Öffnen der Codeansicht zum Anzeigen der Verbindungszeichenfolge](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-with-azure-pipelines-releases"></a>Automatisieren von Continuous Integration mit Azure Pipelines-Releases

Im Folgenden finden Sie eine Anleitung zum Einrichten eines Azure Pipelines-Release, mit dem die Bereitstellung einer Data Factory für mehrere Umgebungen automatisiert wird.

![Diagramm von Continuous Integration mit Azure Pipelines](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Requirements (Anforderungen)

-   Ein Azure-Abonnement, das mit Team Foundation Server oder Azure Repos verknüpft ist und für das der [Azure Resource Manager-Dienstendpunkt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm) verwendet wird

-   Eine Data Factory-Instanz mit konfigurierter Azure Repos Git-Integration

-   Eine  [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Instanz mit den Geheimnissen für jede Umgebung

### <a name="set-up-an-azure-pipelines-release"></a>Einrichten eines Azure-Pipelines-Release

1.  Öffnen Sie auf der [Azure DevOps-Benutzeroberfläche](https://dev.azure.com/) das mit Ihrer Data Factory konfigurierte Projekt.

1.  Klicken Sie links auf der Seite auf **Pipelines**, und wählen Sie dann **Releases** aus.

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Wählen Sie **Neue Pipeline**, oder wenn Pipelines vorhanden sind, die Option **Neu** und dann **Neue Releasepipeline** aus.

1.  Wählen Sie die Vorlage **Leere Stufe** aus.

    ![](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  Geben Sie im Feld **Phasenname** den Namen der Umgebung ein.

1.  Wählen Sie **Artefakt hinzufügen** und dann dasselbe Repository aus, das für die Data Factory konfiguriert wurde. Wählen Sie `adf_publish` als Standardbranch mit der aktuellen Standardversion aus.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Fügen Sie eine Azure Resource Manager-Bereitstellungsaufgabe hinzu:

    a.  Klicken Sie in der Phasenansicht auf den Link **Stufenaufgaben anzeigen**.

    ![](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Erstellen Sie eine neue Aufgabe. Suchen Sie nach **Bereitstellung einer Azure-Ressourcengruppe**, und klicken Sie auf **Hinzufügen**.

    c.  Wählen Sie in der Bereitstellungsaufgabe das Abonnement, die Ressourcengruppe und den Standort für das Data Factory-Ziel aus, und geben Sie bei Bedarf die Anmeldeinformationen an.

    d.  Wählen Sie in der Dropdownliste „Aktion“ den Eintrag **Ressourcengruppe erstellen oder Ressourcengruppe aktualisieren** aus.

    e.  Wählen Sie **…** im Feld **Vorlage**. Suchen Sie nach der Azure Resource Manager-Vorlage, die im Schritt **Import ARM Template** (ARM-Vorlage importieren) unter [Erstellen einer Resource Manager-Vorlage für jede Umgebung](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment) erstellt wurde. Suchen Sie im Ordner `<FactoryName>` des `adf_publish`-Branchs nach dieser Datei.

    f.  Wählen Sie **…** im Feld **Vorlagenparameter** aus, um die Parameterdatei auszuwählen. Wählen Sie die richtige Datei aus. Dies richtet sich danach, ob Sie eine Kopie erstellt haben oder die Standarddatei *ARMTemplateParametersForFactory.json* verwenden.

    g.  Wählen Sie **…** neben dem Feld **Vorlagenparameter überschreiben**, und geben Sie die Informationen für das Data Factory-Ziel ein. Geben Sie für Anmeldeinformationen aus dem Schlüsseltresor den Namen des Geheimnisses in doppelten Anführungszeichen ein. Wenn der Name des Geheimnisses z. B. `cred1` lautet, geben Sie `"$(cred1)"` als Wert ein.

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

    h. Wählen Sie den Bereitstellungsmodus **Inkrementell** aus.

    > [!WARNING]
    > Bei Auswahl des Bereitstellungsmodus **Vollständig** werden vorhandene Ressourcen eventuell gelöscht, einschließlich aller Ressourcen in der Zielressourcengruppe, die nicht in der Resource Manager-Vorlage definiert sind.

1.  Speichern Sie die Releasepipeline.

1. Klicken Sie zum Auslösen eines Release auf **Release erstellen**.

![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="get-secrets-from-azure-key-vault"></a>Abrufen von Geheimnissen aus Azure Key Vault

Wenn Sie Geheimnisse in einer Azure Resource Manager-Vorlage übergeben möchten, sollten Sie Azure Key Vault mit dem Azure Pipelines-Release verwenden.

Es gibt zwei Möglichkeiten, um Geheimnisse zu verarbeiten:

1.  Fügen Sie die Geheimnisse der Parameterdatei hinzu. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](../azure-resource-manager/resource-manager-keyvault-parameter.md).

    -   Erstellen Sie eine Kopie der Parameterdatei, die in den Branch für die Veröffentlichung hochgeladen wird, und legen Sie die Werte der Parameter fest, die Sie aus dem Schlüsseltresor im folgenden Format abrufen möchten:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    -   Bei dieser Methode wird das Geheimnis per Pullvorgang automatisch aus dem Schlüsseltresor abgerufen.

    -   Die Parameterdatei muss sich auch im Branch für die Veröffentlichung befinden.

1.  Fügen Sie vor der im vorherigen Abschnitt beschriebenen Azure Resource Manager-Bereitstellung eine [Azure Key Vault-Aufgabe](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) hinzu:

    -   Wählen Sie die Registerkarte **Aufgaben**, erstellen Sie eine neue Aufgabe, suchen Sie nach **Azure Key Vault**, und fügen Sie sie hinzu.

    -   Wählen Sie in der Key Vault-Aufgabe das Abonnement aus, unter dem Sie den Schlüsseltresor erstellt haben, geben Sie bei Bedarf die Anmeldeinformationen an, und wählen Sie dann den Schlüsseltresor aus.

    ![](media/continuous-integration-deployment/continuous-integration-image8.png)

#### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Gewähren von Berechtigungen für den Azure Pipelines-Agent

Möglicherweise tritt bei der Azure Key Vault-Aufgabe ein Fehler vom Typ „Zugriff verweigert“ auf, wenn nicht die richtigen Berechtigungen vorhanden sind. Laden Sie die Protokolle für das Release herunter, und suchen Sie nach der `.ps1`-Datei mit dem Befehl zum Erteilen von Berechtigungen für den Azure Pipelines-Agent. Sie können den Befehl direkt ausführen oder die Prinzipal-ID aus der Datei kopieren und die Zugriffsrichtlinie manuell im Azure-Portal hinzufügen. **Get** und **List** sind die mindestens erforderlichen Berechtigungen.

### <a name="update-active-triggers"></a>Aktualisieren von aktiven Triggern

Für die Bereitstellung kann ein Fehler auftreten, wenn Sie versuchen, aktive Trigger zu aktualisieren. Zum Aktualisieren von aktiven Triggern müssen Sie sie manuell beenden und nach der Bereitstellung wieder starten. Dies ist über eine Azure PowerShell-Aufgabe möglich.

1.  Fügen Sie auf der Registerkarte „Aufgaben“ des Release eine **Azure PowerShell**-Aufgabe hinzu.

1.  Wählen Sie als Verbindungstyp die Option **Azure Resource Manager**, und wählen Sie anschließend Ihr Abonnement aus.

1.  Wählen Sie als Skripttyp die Option **Inlineskript**, und geben Sie dann Ihren Code an. Im folgenden Beispiel werden die Trigger beendet:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

Sie können ähnliche Schritte ausführen (mit der Funktion `Start-AzDataFactoryV2Trigger`), um die Trigger nach der Bereitstellung neu zu starten.

> [!IMPORTANT]
> In Continuous Integration- und Continuous Deployment-Szenarien muss der Integration Runtime-Typ in verschiedenen Umgebungen identisch sein. Wenn sich in Ihrer Entwicklungsumgebung z.B. eine Integration Runtime (IR) vom Typ *Selbstgehostet* befindet, muss dieselbe IR auch in anderen Umgebungen wie Test- und Produktionsumgebungen vom Typ *Selbstgehostet* sein. Wenn Sie Integration Runtimes in mehreren Phasen freigeben, müssen Sie die Integration Runtimes in allen Umgebungen wie Entwicklungs-, Test- und Produktionsumgebungen als *Verknüpft selbstgehostet* konfigurieren.

#### <a name="sample-prepostdeployment-script"></a>Beispielskript vor und nach der Bereitstellung

Nachfolgend finden Sie ein Beispielskript zum Beenden von Triggern vor der Bereitstellung und zum anschließenden Neustarten der Trigger. Außerdem enthält das Skript den Code zum Löschen von Ressourcen, die entfernt wurden. Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren von Azure PowerShell unter Windows mit PowerShellGet](https://docs.microsoft.com/powershell/azure/install-az-ps).

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder,
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start Active triggers - After cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Verwenden benutzerdefinierter Parameter mit der Resource Manager-Vorlage

Im Git-Modus können Sie die Standardeigenschaften in Ihrer Resource Manager-Vorlage überschreiben, um in der Vorlage parametrisierte Eigenschaften und hartcodierte Eigenschaften festzulegen. Die Überschreibung der standardmäßigen Parametrisierungsvorlage kann beispielsweise in folgenden Szenarien erforderlich sein:

* Sie verwenden automatisierte CI/CD und möchten einige Eigenschaften während der Resource Manager-Bereitstellung ändern, die Eigenschaften sind standardmäßig aber nicht parametrisiert.
* Die Resource Manager-Standardvorlage ist aufgrund der Größe Ihrer Factory ungültig, da sie mehr als die maximal zulässige Parameteranzahl (256) enthält.

In diesen Fällen können Sie die standardmäßige Parametrisierungsvorlage wie folgt überschreiben: Erstellen Sie eine Datei namens *arm-template-parameters-definition.json* im Stammordner des Repositorys. Der Dateiname muss exakt übereinstimmen. Data Factory versucht, diese Datei aus dem Branch zu lesen, in dem Sie sich gerade im Azure Data Factory-Portal befinden (nicht nur aus dem Kollaborationsbranch). Sie können die Datei in einem privaten Branch erstellen oder bearbeiten und Ihre Änderungen mithilfe von **ARM-Vorlage exportieren** auf der Benutzeroberfläche testen. Anschließend können Sie die Datei mit dem Kollaborationsbranch zusammenführen. Sollte keine Datei gefunden werden, wird die Standardvorlage verwendet.


### <a name="syntax-of-a-custom-parameters-file"></a>Syntax einer benutzerdefinierten Parameterdatei

Hier finden Sie einige Richtlinien für die Erstellung der benutzerdefinierten Parameterdatei. Die Datei enthält jeweils einen eigenen Abschnitt für die Entitätstypen: „trigger“, „pipeline“, „linked service“, „dataset“, „integration runtime“ usw.
* Geben Sie den Eigenschaftenpfad unter dem relevanten Entitätstyp ein.
* Wenn Sie einen Eigenschaftennamen auf \* festlegen, geben Sie dadurch an, dass alle ihm untergeordneten Eigenschaften parametrisiert werden sollen (nur bis zur ersten Ebene; nicht rekursiv). Hierfür können auch beliebige Ausnahmen angegeben werden.
* Wenn Sie den Wert einer Eigenschaft als Zeichenfolge festlegen, geben Sie an, dass Sie die Eigenschaft parametrisieren möchten. Verwenden Sie das Format `<action>:<name>:<stype>`.
   *  `<action>` kann eines der folgenden Zeichen sein:
      * `=` bedeutet, dass der aktuelle Wert als Standardwert für den Parameter beibehalten werden soll.
      * `-` bedeutet, dass der Standardwert für den Parameter nicht beibehalten werden soll.
      * `|` ist ein Sonderfall für Geheimnisse aus Azure Key Vault für Verbindungszeichenfolgen oder Schlüssel.
   * `<name>` ist der Name des Parameters. Wenn dieser Wert leer ist, wird der Name der Eigenschaft verwendet. Beginnt der Wert mit dem Zeichen `-`, wird der Name gekürzt. `AzureStorage1_properties_typeProperties_connectionString` wird beispielsweise in `AzureStorage1_connectionString` gekürzt.
   * `<stype>` ist der Typ des Parameters. Wenn `<stype>` leer ist, wird standardmäßig der Typ `string` verwendet. Unterstützte Werte: `string`, `bool`, `number`, `object` und `securestring`.
* Wenn Sie ein Array in der Definitionsdatei angeben, bedeutet das, dass die entsprechende Eigenschaft in der Vorlage ein Array ist. Data Factory durchläuft alle Objekte im Array anhand der Definition, die im Integration Runtime-Objekt des Arrays angegeben ist. Das zweite Objekt (eine Zeichenfolge) wird zum Namen der Eigenschaft, der bei jeder Iteration als Name für den Parameter verwendet wird.
* Es ist nicht möglich, eine Ressourceninstanz-spezifische Definition zu verwenden. Jede Definition gilt für alle Ressourcen dieses Typs.
* Standardmäßig werden alle sicheren Zeichenfolgen (etwa Key Vault-Geheimnisse, Verbindungszeichenfolgen, Schlüssel und Token) parametrisiert.
 
### <a name="sample-parameterization-template"></a>Exemplarische Parametrisierungsvorlage

Nachfolgend finden Sie ein Beispiel dafür, wie eine Parametrisierungsvorlage aussehen kann:

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```
Im Folgenden wird das Erstellen der obigen Vorlage mit einer Aufschlüsselung nach Ressourcentypen erläutert.

#### <a name="pipelines"></a>Pipelines
    
* Jede Eigenschaft im Pfad „activities/typeProperties/waitTimeInSeconds“ wird parametrisiert. Jede Aktivität in einer Pipeline, die eine Eigenschaft auf Codeebene mit dem Namen `waitTimeInSeconds` enthält (z. B. die Aktivität `Wait`), wird als Zahl mit einem Standardnamen parametrisiert. Die Aktivität verfügt jedoch nicht über einen Standardwert in der Resource Manager-Vorlage. Hierbei handelt es sich um eine erforderliche Eingabe bei der Resource Manager-Bereitstellung.
* Analog dazu wird eine Eigenschaft namens `headers` (etwa in einer Aktivität vom Typ `Web`) mit dem Typ `object` (JObject) parametrisiert. Sie besitzt einen Standardwert (der gleiche Wert wie in der Quellfactory).

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Alle Eigenschaften unter dem Pfad `typeProperties` werden mit ihren jeweiligen Standardwerten parametrisiert. Beispielsweise gibt es unter Eigenschaften vom Typ **IntegrationRuntimes** zwei Eigenschaften: `computeProperties` und `ssisProperties`. Beide Eigenschaftentypen werden mit ihren jeweiligen Standardwerten und -typen (Objekt) erstellt.

#### <a name="triggers"></a>Trigger

* Unter `typeProperties` werden zwei Eigenschaften parametrisiert. Die erste ist `maxConcurrency`. Diese Eigenschaft besitzt einen Standardwert und ist vom Typ `string`. Der standardmäßige Parametername lautet `<entityName>_properties_typeProperties_maxConcurrency`.
* Die Eigenschaft `recurrence` wird ebenfalls parametrisiert. Darunter werden alle Eigenschaften auf dieser Ebene gemäß Angabe als Zeichenfolgen mit Standardwerten und Parameternamen parametrisiert. Eine Ausnahme ist die Eigenschaft `interval`: Sie wird als Zahlentyp mit dem Parameternamensuffix `<entityName>_properties_typeProperties_recurrence_triggerSuffix` parametrisiert. Analog dazu ist die Eigenschaft `freq` eine Zeichenfolge und wird als Zeichenfolge parametrisiert. Die Eigenschaft `freq` wird jedoch ohne Standardwert parametrisiert. Der Name wird verkürzt und mit einem Suffix versehen. Beispiel: `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Verknüpfte Dienste sind ein Sonderfall. Da verknüpfte Dienste und Datasets eine breite Palette von Typen umfassen, können Sie eine typspezifische Anpassung vornehmen. In diesem Beispiel wird für alle verknüpften Dienste vom Typ `AzureDataLakeStore` eine bestimmte Vorlage angewandt und für alle anderen (über \*) eine andere Vorlage.
* Die `connectionString`-Eigenschaft wird als Wert vom Typ `securestring` parametrisiert. Sie besitzt keinen Standardwert und erhält einen verkürzten Parameternamen mit dem Suffix `connectionString`.
* Die `secretAccessKey`-Eigenschaft ist eine Eigenschaft vom Typ `AzureKeyVaultSecret` (beispielsweise in einem verknüpften `AmazonS3`-Dienst). Sie wird automatisch als Azure Key Vault-Geheimnis parametrisiert und aus dem konfigurierten Schlüsseltresor abgerufen. Auch der Schlüsseltresor kann parametrisiert werden.

#### <a name="datasets"></a>Datasets

* Für Datasets steht zwar eine typspezifische Anpassung zur Verfügung, die Konfiguration muss jedoch nicht zwingend eine Konfiguration auf der Ebene \* enthalten. Im vorherigen Beispiel werden alle Dataseteigenschaften unter `typeProperties` parametrisiert.

### <a name="default-parameterization-template"></a>Standardvorlage für die Parametrisierung

Nachfolgend ist die aktuelle Standardvorlage für die Parametrisierung dargestellt. Wenn Sie nur einen oder wenige Parameter hinzufügen möchten, kann eine direkte Bearbeitung hilfreich sein, da die vorhandene Parametrisierungsstruktur nicht verloren geht.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

Im folgenden Beispiel wird das Hinzufügen eines einzelnen Werts zur Standardvorlage für die Parametrisierung veranschaulicht. Der Parameterdatei soll lediglich die ID eines vorhandenen interaktiven Databricks-Clusters für einen verknüpften Databricks-Dienst hinzugefügt werden. Beachten Sie, dass die Datei unten identisch mit der obigen Datei ist, mit der Ausnahme, dass `existingClusterId` unter dem Feld „properties“ von `Microsoft.DataFactory/factories/linkedServices` enthalten ist.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

## <a name="linked-resource-manager-templates"></a>Verknüpfte Resource Manager-Vorlagen

Wenn Sie Continuous Integration und Continuous Deployment (CI/CD) für Ihre Data Factory-Instanzen eingerichtet haben, stoßen Sie bei zunehmender Größe der Factory möglicherweise an die Grenzen von Azure Resource Manager-Vorlagen. Ein Beispiel für eine Beschränkung ist die maximale Anzahl der Ressourcen in einer Resource Manager-Vorlage. Um die Verwendung umfangreicher Factorys zu ermöglichen, werden in Data Factory neben der Erstellung der vollständigen Resource Manager-Vorlage für eine Factory jetzt verknüpfte Resource Manager-Vorlagen generiert. Mit dieser Funktion wird die Nutzlast einer Factory auf mehrere Dateien aufgeteilt, sodass die Grenzen keine Rolle spielen.

Wenn Sie Git konfiguriert haben, werden die verknüpften Vorlagen zusammen mit den vollständigen Resource Manager-Vorlagen im Branch `adf_publish` in einem neuen Ordner mit dem Namen `linkedTemplates` generiert und gespeichert.

![Ordner für verknüpfte Resource Manager-Vorlagen](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Verknüpfte Resource Manager-Vorlagen verfügen in der Regel über eine Mastervorlage und eine Reihe von untergeordneten Vorlagen, die mit der Mastervorlage verknüpft sind. Die übergeordnete Vorlage wird als `ArmTemplate_master.json` bezeichnet, und die untergeordneten Vorlagen werden nach dem Muster `ArmTemplate_0.json`, `ArmTemplate_1.json` usw. benannt. Um anstelle der vollständigen Resource Manager-Vorlage verknüpfte Vorlagen zu verwenden, aktualisieren Sie den CI/CD-Task so, dass dieser auf `ArmTemplate_master.json` statt auf `ArmTemplateForFactory.json` (die vollständige Resource Manager-Vorlage) verweist. Resource Manager erfordert auch, dass Sie die verknüpften Vorlagen in ein Speicherkonto hochladen, sodass Azure während der Bereitstellung darauf zugreifen kann. Weitere Informationen finden Sie unter [Deploying Linked ARM Templates with VSTS](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/) (Bereitstellen von verknüpften ARM-Vorlagen mit VSTS).

Denken Sie daran, die Data Factory-Skripts vor und nach dem Bereitstellungstask zu Ihrer CI/CD-Pipeline hinzuzufügen.

Wenn Sie Git nicht konfiguriert haben, kann über die Geste **ARM-Vorlage exportieren** auf die verknüpften Vorlagen zugegriffen werden.

## <a name="hot-fix-production-branch"></a>Hotfix für den Produktionsbranch

Wenn Sie eine Factory in der Produktionsumgebung bereitstellen und feststellen, dass ein Fehler sofort behoben werden muss, Sie jedoch den aktuellen Kollaborationsbranch nicht bereitstellen können, müssen Sie möglicherweise einen Hotfix bereitstellen. Dieser Ansatz wird als Quick Fix Engineering oder QFE bezeichnet. 

1.  Navigieren Sie in Azure DevOps zu dem Release, das in der Produktionsumgebung bereitgestellt wurde, und suchen Sie den letzten bereitgestellten Commit.

2.  Suchen Sie in der Commitmeldung die Commit-ID des Kollaborationsbranch.

3.  Erstellen Sie einen neuen Hotfixbranch aus diesem Commit.

4.  Navigieren Sie zur Azure Data Factory-Benutzeroberfläche und dort zu diesem Branch.

5.  Beheben Sie den Fehler in der Azure Data Factory-Benutzeroberfläche. Testen Sie die Änderungen.

6.  Nachdem die Korrektur überprüft wurde, klicken Sie auf **Export ARM template** (ARM-Vorlage exportieren), um die Resource Manager-Hotfixvorlage abzurufen.

7.  Checken Sie diesen Build manuell im Branch „adf_publish“ ein.

8.  Wenn Sie die Releasepipeline so konfiguriert haben, dass sie bei Check-Ins in „adf_publish“ automatisch ausgelöst wird, wird automatisch ein neues Release gestartet. Reihen Sie andernfalls manuell ein Release in die Warteschlange ein.

9.  Stellen Sie das Hotfixrelease für die Test- und Produktionsfactorys bereit. Dieses Release enthält die vorherige Nutzlast der Produktionsumgebung sowie die in Schritt 5 vorgenommene Korrektur.

10. Fügen Sie dem Entwicklungsbranch die Änderungen aus dem Hotfix hinzu, sodass der Fehler bei späteren Releases nicht auftritt.

## <a name="best-practices-for-cicd"></a>Bewährte Methoden für CI/CD

Wenn Sie Git-Integration mit Ihrer Data Factory verwenden und über eine CI/CD-Pipeline verfügen, die Ihre Änderungen aus der Entwicklung- in die Test- und dann in die Produktionsumgebung verschiebt, empfehlen wir Ihnen die folgenden bewährten Methoden:

-   **Git-Integration**. Sie müssen lediglich Ihre Data Factory für die Entwicklung mit Git-Integration konfigurieren. Änderungen an den Test- und Produktionsumgebungen werden über CI/CD bereitgestellt und müssen keine Git-Integration aufweisen.

-   **Data Factory-CI/CD-Skript**. Vor der Resource Manager-Bereitstellung in CI/CD sind bestimmte Aufgaben erforderlich, z. B. Beenden und Starten von Triggern sowie die Bereinigung. Es wird empfohlen, vor und nach der Bereitstellung PowerShell-Skripts zu verwenden. Weitere Informationen finden Sie unter [Aktualisieren von aktiven Triggern](#update-active-triggers). 

-   **Integration Runtimes und Freigaben**. Integration Runtimes werden nicht sehr häufig geändert und sind in allen Phasen von CI/CD ähnlich. Daher erwartet Data Factory, dass sie in allen Phasen von CI/CD den gleichen Namen und den gleichen Typ von Intergration Runtimes aufweisen. Wenn Sie Integration Runtimes über alle Phasen hinweg freigeben möchten, können Sie eine ternäre Factory verwenden, die nur die freigegebenen Integration Runtimes enthält. Diese freigegebene Factory können Sie in allen Umgebungen als verknüpften Integration Runtime-Typ verwenden.

-   **Key Vault**. Wenn Sie verknüpfte Dienste verwenden, die auf Azure Key Vault basieren, können Sie weitere Vorteile nutzen, indem Sie separate Schlüsseltresore für unterschiedliche Umgebungen verwenden. Sie können auch separate Berechtigungsstufen für jeden von ihnen konfigurieren. Beispielsweise möchten Sie möglicherweise nicht, dass Teammitglieder über Berechtigungen für Produktionsgeheimnisse verfügen. Bei diesem Ansatz empfiehlt es sich, in allen Phasen die gleichen Geheimnisnamen beizubehalten. Wenn Sie die gleichen Namen beibehalten, müssen Sie die Resource Manager-Vorlagen in den CI/CD-Umgebungen nicht ändern, da sich lediglich der Name des Schlüsseltresors ändert, der einer der Resource Manager-Vorlagenparameter ist.

## <a name="unsupported-features"></a>Nicht unterstützte Funktionen

- ADF ist _nicht_ auf Cherrypicking-Commits oder die selektive Veröffentlichung von Ressourcen ausgelegt. Veröffentlichungen enthalten **alle** in der Data Factory vorgenommenen Änderungen.

    - Data Factory-Entitäten hängen voneinander ab, d. h. Trigger hängen z. B. von Pipelines ab, Pipelines hängen von Datasets und anderen Pipelines ab usw. Die selektive Veröffentlichung einer Teilmenge von Ressourcen _kann_ zu unerwartetem Verhalten und Fehlern führen.
    - In seltenen Fällen, in denen eine selektive Veröffentlichung erforderlich ist, können Sie einen Hotfix in Betracht ziehen. Weitere Informationen finden Sie unter [Hotfix für den Produktionsbranch](#hot-fix-production-branch).

-   Sie können nicht aus privaten Branches veröffentlichen.

-   Ab sofort können Sie keine Projekte unter Bitbucket hosten.
