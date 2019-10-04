---
title: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage mit mehreren Ressourcen erstellen und über das Azure-Portal bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 486a13db9cf18cb44a063d37dde4a657f6dc625c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103666"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen

Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, um mehrere Ressourcen bereitzustellen, und konfigurieren Sie die Bereitstellungsreihenfolge. Nach der Erstellung der Vorlage stellen Sie sie mithilfe von Cloud Shell über das Azure-Portal bereit.

In diesem Tutorial erstellen Sie ein Speicherkonto, eine VM, ein virtuelles Netzwerk und einige andere abhängigen Ressourcen. Einige der Ressourcen können erst bereitgestellt werden, wenn eine andere Ressource vorhanden ist. Sie können den virtuellen Computer beispielsweise erst erstellen, wenn sein Speicherkonto und seine Netzwerkschnittstelle vorhanden sind. Diese Beziehung definieren Sie, indem Sie eine Ressource von den anderen Ressourcen abhängig machen. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Weitere Informationen finden Sie unter [Definieren der Reihenfolge für die Bereitstellung von Ressourcen in Azure Resource Manager-Vorlagen](./resource-group-define-dependencies.md).

![Bereitstellungsreihenfolgen-Diagramm für Ressourcen, die von der Resource Manager-Vorlage abhängig sind](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-dependent-resources-diagram.png)

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Untersuchen der Vorlage
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“.  Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="explore-the-template"></a>Untersuchen der Vorlage

Sehen Sie sich die Vorlage in diesem Abschnitt an, und versuchen Sie, die folgenden Fragen zu beantworten:

* Wie viele Azure-Ressourcen sind in dieser Vorlage definiert?
* Bei einer der Ressourcen handelt es sich um ein Azure-Speicherkonto.  Sieht die Definition wie die Definition im letzten Tutorial aus?
* Finden Sie die Vorlagenreferenzen für die in dieser Vorlage definierten Ressourcen?
* Finden Sie die Abhängigkeiten der Ressourcen?

1. Reduzieren Sie in Visual Studio Code die Elemente, bis unter **resources** nur die Elemente der ersten und zweiten Ebene angezeigt werden:

    ![Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Es gibt fünf Ressourcen, die von der Vorlage definiert werden:

   * `Microsoft.Storage/storageAccounts`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.

2. Erweitern Sie die erste Ressource. Es handelt sich um ein Speicherkonto. Vergleichen Sie die Ressourcendefinition mit der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).

    ![Azure Resource Manager-Vorlagen in Visual Studio Code: Speicherkontodefinition](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Erweitern Sie die zweite Ressource. Der Ressourcentyp lautet `Microsoft.Network/publicIPAddresses`. Vergleichen Sie die Ressourcendefinition mit der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).

    ![Azure Resource Manager-Vorlagen in Visual Studio Code: Definition der öffentlichen IP-Adresse](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Erweitern Sie die vierte Ressource. Der Ressourcentyp lautet `Microsoft.Network/networkInterfaces`:  

    ![„dependsOn“ für Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    Das Element „dependsOn“ bietet die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Die Ressource hängt von zwei weiteren Ressourcen ab:

    * `Microsoft.Network/publicIPAddresses`
    * `Microsoft.Network/virtualNetworks`

5. Erweitern Sie die fünfte Ressource. Diese Ressource ist ein virtueller Computer. Sie hängt von zwei weiteren Ressourcen ab:

    * `Microsoft.Storage/storageAccounts`
    * `Microsoft.Network/networkInterfaces`

Das folgende Diagramm veranschaulicht die Ressourcen und die Abhängigkeitsinformationen für diese Vorlage:

![Abhängigkeitsdiagramm für Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Durch die Angabe der Abhängigkeiten wird die Lösung von Resource Manager effizient bereitgestellt. Mit Resource Manager werden das Speicherkonto, die öffentliche IP-Adresse und das virtuelle Netzwerk parallel bereitgestellt, da sie über keine Abhängigkeiten verfügen. Nach der Bereitstellung der öffentlichen IP-Adresse und des virtuellen Netzwerks wird die Netzwerkschnittstelle erstellt. Wenn alle anderen Ressourcen bereitgestellt wurden, stellt Resource Manager den virtuellen Computer bereit.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Es gibt viele Methoden zum Bereitstellen von Vorlagen.  In diesem Tutorial verwenden Sie Cloud Shell aus dem Azure-Portal.

1. Melden Sie sich bei [Cloud Shell](https://shell.azure.com) an. 
2. Wählen Sie links oben in der Cloudshell die Option **PowerShell** aus, und wählen Sie dann **Bestätigen**.  In diesem Tutorial können Sie PowerShell verwenden.
3. Klicken Sie in Cloud Shell auf **Datei hochladen**:

    ![Azure-Portal, Cloud Shell, Datei hochladen](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Wählen Sie die Vorlage aus, die Sie zuvor im Tutorial gespeichert haben. Der Standardname lautet **azuredeploy.json**.  Falls Sie eine Datei mit dem gleichen Dateinamen besitzen, wird die alte Datei ohne Benachrichtigung überschrieben.

    Sie können optional den Befehl **ls $HOME** und den Befehl **cat $HOME/azuredeploy.json** verwenden, um zu überprüfen, ob die Dateien hochgeladen wurden. 

5. Führen Sie in Cloud Shell die folgenden PowerShell-Befehle aus. Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Siehe [Voraussetzungen](#prerequisites).

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
    $adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
    $dnsLabelPrefix = Read-Host -Prompt "Enter the DNS label prefix"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -adminUsername $adminUsername `
        -adminPassword $adminPassword `
        -dnsLabelPrefix $dnsLabelPrefix `
        -TemplateFile "$HOME/azuredeploy.json"
    ```

8. Führen Sie den folgenden PowerShell-Befehl zum Auflisten des neu erstellen virtuellen Computers aus:

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    Get-AzVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    Der Name des virtuellen Computers ist in der Vorlage als **SimpleWinVM** hartcodiert.

9. Stellen Sie per RDP eine Verbindung mit dem virtuellen Computer her, um sich zu vergewissern, dass der virtuelle Computer erstellt wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Vorlage entwickelt und bereitgestellt, um einen virtuellen Computer, ein virtuelles Netzwerk und die abhängigen Ressourcen zu erstellen. Informationen zum Bereitstellen von Azure-Ressourcen auf der Grundlage von Bedingungen finden Sie hier:

> [!div class="nextstepaction"]
> [Verwenden von Bedingungen](./resource-manager-tutorial-use-conditions.md)
