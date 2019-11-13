---
title: 'Schnellstart: Erstellen eines Event Hubs mit einer Consumergruppe: Azure Event Hubs'
description: 'Schnellstart: Erstellen eines Event Hubs-Namespace mit einem Event Hub und einer Consumergruppe mithilfe von Azure Resource Manager-Vorlagen'
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: b262f5f4122256a55c005002c4323205a5b564f0
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717659"
---
# <a name="quickstart-create-an-event-hub-by-using-an-azure-resource-manager-template"></a>Schnellstart: Erstellen eines Event Hubs mithilfe einer Azure Resource Manager-Vorlage

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

In dieser Schnellstartanleitung erstellen Sie einen Event Hub mithilfe einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-overview.md). Sie stellen eine Azure Resource Manager-Vorlage bereit, um einen Namespace vom Typ [Event Hubs](event-hubs-what-is-event-hubs.md) mit einem Event Hub zu erstellen. Dieser Artikel zeigt Ihnen, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen. Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][Authoring Azure Resource Manager templates]. Informationen zur JSON-Syntax und zu den Eigenschaften, die in einer Vorlage verwendet werden sollen, finden Sie unter [Microsoft.EventHub-Ressourcentypen](/azure/templates/microsoft.eventhub/allversions).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

In diesem Schnellstart verwenden Sie eine [vorhandene Schnellstartvorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json):

[!code-json[create-azure-event-hub-namespace](~/quickstart-templates/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json)]

Weitere Beispiele für Vorlagen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular).

So stellen Sie die Vorlage bereit:

1. Wählen Sie im folgenden Codeblock **Ausprobieren** aus, und folgen Sie dann den Anweisungen, um sich bei Azure Cloud Shell anzumelden.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   Es dauert einen Augenblick, einen Event Hub zu erstellen.

1. Wählen Sie **Kopieren**, um das PowerShell-Skript zu kopieren.
1. Klicken Sie mit der rechten Maustaste auf die Shellkonsole, und wählen Sie **Einfügen** aus.

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Um die Bereitstellung zu überprüfen, öffnen Sie entweder die Ressourcengruppe im [Azure-Portal](https://portal.azure.com) oder verwenden das folgende Azure PowerShell-Skript.  Wenn die Cloud Shell noch geöffnet ist, muss die erste Zeile (Read-Host) nicht kopiert/ausgeführt werden.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen. Wenn die Cloud Shell noch geöffnet ist, muss die erste Zeile (Read-Host) nicht kopiert/ausgeführt werden.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen Event Hubs-Namespace und einen Event Hub im Namespace erstellt. Schritt-für-Schritt-Anleitungen zum Senden von Ereignissen an einen Event Hub oder Empfangen von Ereignissen von einem Event Hub finden Sie in den Tutorials zum **Senden und Empfangen von Ereignissen**:

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (nur senden)](event-hubs-c-getstarted-send.md)
- [Apache Storm (nur empfangen)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
