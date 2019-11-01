---
title: Verwalten von Azure Analysis Services mit PowerShell | Microsoft-Dokumentation
description: Verwaltung von Azure Analysis Services mit PowerShell
author: minewiskan
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 10/28/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 329ea51895c45c8cdf50a323328fac3c580ffc16
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73146219"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Verwalten von Azure Analysis Services mit PowerShell

Dieser Artikel beschreibt PowerShell-Cmdlets, die zum Ausführen von Azure Analysis Services-Verwaltungsaufgaben für Server und Datenbanken verwendet werden. 

Für Aufgaben zur Serverressourcenverwaltung wie das Erstellen oder Löschen eines Servers, das Anhalten oder Fortsetzen von Servervorgängen oder das Ändern der Dienstebene (Tarif) werden Azure Analysis Services-Cmdlets verwendet. Für andere Aufgaben zum Verwalten von Datenbanken, z.B. Hinzufügen oder Entfernen von Rollenmitgliedern, Verarbeiten oder Partitionieren, werden die im gleichen SqlServer-Modul wie SQL Server Analysis Services enthaltenen Cmdlets verwendet.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>Berechtigungen

Die meisten PowerShell-Aufgaben erfordern, dass Sie über Administratorberechtigungen auf dem verwalteten Analysis Services-Server verfügen. Geplante PowerShell-Aufgaben sind unbeaufsichtigte Vorgänge. Das Konto oder der Dienstprinzipal, auf dem der Scheduler ausgeführt wird, muss auf dem Analysis Services-Server über Administratorrechte verfügen. 

Bei Servervorgängen, für die Azure PowerShell-Cmdlets verwendet werden, muss Ihr Konto oder das Konto mit dem Scheduler außerdem zur Rolle „Besitzer“ für die Ressource in der [rollenbasierten Zugriffssteuerung in Azure](../role-based-access-control/overview.md) gehören. 

## <a name="resource-and-server-operations"></a>Ressourcen- und Servervorgänge 

Modul installieren: [Az.AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
Dokumentation: [Az.AnalysisServices-Referenz](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>Datenbankvorgänge

Für Azure Analysis Services-Datenbankvorgänge wird das gleiche SqlServer-Modul wie für SQL Server Analysis Services verwendet. Allerdings werden nicht alle Cmdlets für Azure Analysis Services unterstützt. 

Das SqlServer-Modul bietet aufgabenspezifische Cmdlets für die Datenbankverwaltung sowie das allgemeine Cmdlet Invoke-ASCmd, das TMSL-Abfragen (Tabular Model Scripting Language) und -Skripts akzeptiert. Die folgenden Cmdlets im SqlServer-Modul werden von Azure Analysis Services unterstützt.

Modul installieren: [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
Dokumentation: [SqlServer-Referenz](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>Unterstützte Cmdlets

|Cmdlet|BESCHREIBUNG|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Hinzufügen eines Mitglieds zu einer Datenbankrolle.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Sichern einer Analysis Services-Datenbank.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Entfernen eines Mitglieds aus einer Datenbankrolle.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Ausführen eines TMSL-Skripts.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Verarbeiten einer Datenbank.|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Verarbeiten einer Partition.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Verarbeiten einer Tabelle.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Zusammenführen einer Partition.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Wiederherstellen einer Analysis Services-Datenbank.| 
  

## <a name="related-information"></a>Verwandte Informationen

* [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)      
* [Herunterladen des SQL Server PowerShell-Moduls](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Herunterladen von SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SqlServer-Modul im PowerShell-Katalog](https://www.powershellgallery.com/packages/SqlServer)    
* [Programmierung von tabellarischen Modellen für den Kompatibilitätsgrad 1200 und höher](https://docs.microsoft.com/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
