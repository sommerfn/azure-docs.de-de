---
title: 'Schnellstart: Erstellen eines Lastenausgleichs im Tarif „Standard“: Azure Resource Manager-Vorlage'
titlesuffix: Azure Load Balancer
description: In diesem Schnellstart wird gezeigt, wie Sie mithilfe einer Azure Resource Manager-Vorlage einen Lastenausgleich im Tarif „Standard“ erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Standard load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: e1a04cad7fe5c9c95397ffad2faa80c7d657d0f8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68273793"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-an-azure-resource-manager-template"></a>Schnellstart: Erstellen eines Lastenausgleichs im Tarif „Standard“ für virtuelle Computer mithilfe einer Azure Resource Manager-Vorlage

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer (VMs) bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Dieser Schnellstart zeigt Ihnen, wie Sie eine Azure Resource Manager-Vorlage bereitstellen, die einen Lastenausgleich im Tarif „Standard“ für VMs erstellt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-standard-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Standard“

Ein Lastenausgleich im Tarif „Standard“ unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie einen Lastenausgleich im Tarif „Standard“ erstellen, müssen Sie auch eine neue öffentliche Standard-IP-Adresse erstellen, die für diesen Lastenausgleich als Front-End konfiguriert ist.

Es gibt viele Methoden zum Erstellen eines Lastenausgleichs im Tarif „Standard“. In diesem Schnellstart verwenden Sie Azure PowerShell, um eine [Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json) bereitzustellen. Resource Manager-Vorlagen sind JSON-Dateien, mit denen die Ressourcen definiert werden, die Sie für Ihre Lösung bereitstellen müssen.

Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie in der [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/). Weitere Vorlagen zum Azure Load Balancer finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

1. Wählen Sie **Try it** (Ausprobieren) im folgenden Codeblock aus, um Azure Cloud Shell zu öffnen. Folgen Sie dann den Anweisungen, um sich bei Azure anzumelden.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Warten Sie, bis die Aufforderung in der Konsole angezeigt wird.

1. Wählen Sie **Copy** (Kopieren) im vorherigen Codeblock aus, um das PowerShell-Skript zu kopieren.

1. Klicken Sie mit der rechten Maustaste auf den Shellkonsolenbereich, und wählen Sie **Einfügen** aus.

1. Gehen Sie die Werte ein.

   Die Vorlagenbereitstellung erstellt drei Verfügbarkeitszonen. Verfügbarkeitszonen werden nur in [bestimmten Regionen](../availability-zones/az-overview.md) unterstützt. Verwenden Sie eine der unterstützten Regionen. Wenn Sie nicht sicher sind, geben Sie **centralus** ein.

   Der Ressourcengruppenname ist der Projektname mit dem Zusatz **rg**. Sie benötigen den Ressourcengruppennamen im nächsten Abschnitt.

Das Bereitstellen der Vorlage dauert ungefähr 10 Minuten.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

1. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname entspricht standardmäßig dem Projektnamen mit dem Zusatz **rg**.

1. Wählen Sie den Load Balancer aus. Sein Standardname entspricht dem Projektnamen mit dem Zusatz **-lb**.

1. Kopieren Sie nur den IP-Adressteil der öffentlichen IP-Adresse, und fügen Sie ihn in die Adressleiste des Browsers ein. Der Browser zeigt die Standardseite des Internetinformationsdienste-Webservers (Internet Information Services, IIS) an.

   ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Sie können vom Clientcomputer aus eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Lastenausgleich den Datenverkehr auf alle drei virtuellen Computer verteilt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu im Azure-Portal die Ressourcengruppe aus, die den Lastenausgleich enthält, und wählen Sie anschließend **Ressourcengruppe löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen Lastenausgleich im Tarif „Standard“ erstellt, virtuelle Computer angefügt, die Datenverkehrsregel für den Lastenausgleich konfiguriert, einen Integritätstest durchgeführt und den Lastenausgleich getestet.

Weitere Informationen zum Lastenausgleich finden Sie in den Tutorials.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
 