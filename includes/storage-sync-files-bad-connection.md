---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 07cae1cee9810646de5bf9610a29991376736373
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72391650"
---
Dieser Fehler kann auftreten, wenn der Zugriff auf den Azure-Dateisynchronisierungsdienst vom Server aus nicht verfügbar ist. Sie können diesen Fehler in folgenden Schritte beheben:

1. Stellen Sie sicher, dass der Windows-Dienst `FileSyncSvc.exe` nicht von der Firewall blockiert wird.
2. Stellen Sie sicher, dass Port 443 für ausgehende Verbindungen mit dem Azure-Dateisynchronisierungsdienst geöffnet ist. Hierfür können Sie das `Test-NetConnection`-Cmdlet verwenden. Die URL für den `<azure-file-sync-endpoint>`-Platzhalter unten finden Sie im Dokument [Proxy- und Firewall-Einstellungen der Azure-Dateisynchronisierung](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Stellen Sie sicher, dass die Proxykonfiguration wie erwartet festgelegt ist. Verwenden Sie dazu das `Get-StorageSyncProxyConfiguration`-Cmdlet. Weitere Informationen zur Proxykonfiguration für die Azure-Dateisynchronisierung finden Sie unter [Proxy- und Firewall-Einstellungen der Azure-Dateisynchronisierung](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Wenden Sie sich an Ihren Netzwerkadministrator, um weitere Unterstützung zur Problembehandlung bei Netzwerkverbindungen zu erhalten.