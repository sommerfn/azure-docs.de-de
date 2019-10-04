---
title: Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der SQL Server-IaaS-Agent-Erweiterung | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwaltung der SQL Server-IaaS-Agent-Erweiterung beschrieben, die bestimmte SQL Server-Verwaltungsaufgaben automatisiert. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure Key Vault-Integration.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/30/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3240bb689447c16de8c62e9e8118b0b0df2b1ea3
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259422"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-by-using-the-sql-server-iaas-agent-extension"></a>Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der SQL Server-IaaS-Agent-Erweiterung
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

Die Erweiterung für SQL Server-IaaS-Agent (SqlIaasExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieser Artikel bietet eine Übersicht über die Dienste, die von dieser Erweiterung unterstützt werden. Er enthält auch Anweisungen zu Installation, Statusanzeige und Entfernung der Erweiterung.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-IaaS-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).


## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | BESCHREIBUNG |
| --- | --- |
| **Automatisierte Sicherung für SQL Server** |Automatisiert die Planung von Sicherungen für alle Datenbanken entweder der Standardinstanz oder einer [ordnungsgemäß installierten](virtual-machines-windows-sql-server-iaas-faq.md#administration) benannten Instanz von SQL Server in der VM. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisierte Patches für SQL Server** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem wichtige Windows-Updates für den virtuellen Computer ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Key Vault-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Wenn die SQL Server-IaaS-Agent-Erweiterung installiert wurde und ausgeführt wird, stellt sie Verwaltungsfeatures folgendermaßen zur Verfügung:

* Im Bereich „SQL Server“ des virtuellen Computers im Azure-Portal sowie über Azure PowerShell für SQL Server-Images im Azure Marketplace.
* Über Azure PowerShell bei manuellen Installationen der Erweiterung. 

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der SQL Server-IaaS-Agent-Erweiterung auf Ihrem virtuellen Computer:

**Betriebssystem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server-Version:**

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell:**

* [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


##  <a name="installation"></a>Installation
Die SQL Server-IaaS-Erweiterung wird installiert, wenn Sie Ihre SQL Server-VM beim [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registrieren. Falls erforderlich, können Sie den SQL Server-IaaS-Agent mit dem folgenden PowerShell-Befehl manuell installieren: 

  ```powershell-interactive
    Set-AzVMExtension -ResourceGroupName "<ResourceGroupName>" `
    -Location "<VMLocation>" -VMName "<VMName>" `
    -Name "SqlIaasExtension" -Publisher "Microsoft.SqlServer.Management" `
    -ExtensionType "SqlIaaSAgent" -TypeHandlerVersion "2.0";  
  ```

> [!NOTE]
> Durch die Installation der Erweiterung wird der SQL Server-Dienst neu gestartet. 


### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Installieren eines virtuellen Computers mit einer einzelnen benannten SQL Server-Instanz
Die SQL Server-IaaS-Erweiterung funktioniert mit einer benannten SQL Server-Instanz, wenn die Standardinstanz deinstalliert und die IaaS-Erweiterung neu installiert wird.

Um eine benannte Instanz von SQL Server zu verwenden, gehen Sie folgendermaßen vor:
   1. Stellen Sie eine SQL Server-VM vom Azure Marketplace bereit. 
   1. Deinstallieren Sie die IaaS-Erweiterung über das [Azure-Portal](https://portal.azure.com).
   1. Deinstallieren Sie SQL Server in der SQL Server-VM vollständig.
   1. Installieren Sie SQL Server mit einer benannten Instanz in der SQL Server-VM. 
   1. Installieren Sie die IaaS-Erweiterung über das Azure-Portal.  


## <a name="get-the-status-of-the-sql-server-iaas-extension"></a>Abrufen des Status der SQL Server-IaaS-Erweiterung
Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie im Fenster des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SqlIaasExtension** sollte aufgeführt sein.

![Status der SQL Server-IaaS-Agent-Erweiterung im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzVMSqlServerExtension** verwenden:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Der Befehl oben überprüft, ob der Agent installiert ist, und stellt allgemeine Statusinformationen bereit. Mit den folgenden Befehlen können Sie spezifische Statusinformationen zu automatischen Sicherungen und Patches abrufen:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Entfernen
Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie im Fenster **Erweiterungen** in den Eigenschaften des virtuellen Computers auf die Auslassungspunkte klicken. Wählen Sie anschließend die Option **Löschen**.

![Deinstallieren der SQL Server-IaaS-Agent-Erweiterung im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzVMSqlServerExtension** verwenden:

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie einen der Dienste, die von der Erweiterung unterstützt werden. Weitere Informationen finden Sie in den Artikeln, die im Abschnitt [Unterstützte Dienste](#supported-services) dieses Artikels genannt werden.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Was ist SQL Server auf virtuellen Azure-Computern?](virtual-machines-windows-sql-server-iaas-overview.md).
