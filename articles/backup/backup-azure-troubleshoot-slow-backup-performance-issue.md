---
title: Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup
description: Enthält Informationen zu Problembehandlungsschritten, mit denen Sie die Ursache von Azure Backup-Leistungsproblemen diagnostizieren können.
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
ms.openlocfilehash: 592a46077bb9e3469f3a42a95173af1b6db93510
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704932"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup
Dieser Artikel enthält Informationen zu Problembehandlungsschritten, mit denen Sie eine niedrige Geschwindigkeit beim Sichern von Dateien und Ordnern mit Azure Backup diagnostizieren können. Wenn Sie den Azure Backup-Agent zum Sichern von Dateien verwenden, kann der Sicherungsprozess länger als erwartet dauern. Diese Verzögerung kann eine oder mehrere der folgenden Ursachen haben:

* [Es treten Leistungsengpässe auf dem Computer auf, der gesichert wird.](#cause1)
* [Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt.](#cause2)
* [Der Backup-Agent wird auf einem virtuellen Azure-Computer (VM) ausgeführt.](#cause3)  
* [Sie sichern eine große Zahl von Dateien (mehrere Millionen).](#cause4)

Bevor Sie mit der Problembehandlung beginnen, empfehlen wir Ihnen, den [aktuellen Azure Backup-Agent](https://aka.ms/azurebackup_agent)herunterzuladen und zu installieren. Wir stellen häufig Updates für den Backup-Agent zur Verfügung, um verschiedene Probleme zu beheben, Funktionen hinzuzufügen und die Leistung zu verbessern.

Wir empfehlen Ihnen auch dringend den Artikel [Azure Backup-Dienst – FAQ](backup-azure-backup-faq.md) , damit Sie häufig auftretende Konfigurationsprobleme vermeiden können.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Ursache: Leistungsengpässe auf dem Computer
Leistungsengpässe auf dem Computer, der gesichert wird, können Verzögerungen verursachen. Die Fähigkeit des Computers, vom Datenträger zu lesen oder darauf zu schreiben, und die verfügbare Bandbreite zum Senden von Daten über das Netzwerk können beispielsweise für Engpässe sorgen.

Zum Erkennen dieser Engpässe bietet Windows ein integriertes Tool, den [Systemmonitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon).

Hier sind einige Leistungsindikatoren und Bereiche angegeben, die bei der Diagnose von Engpässen, um optimale Sicherungen zu erzielen, hilfreich sein können.

| Indikator | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |• 100% Leerlauf bis 50% Leerlauf = fehlerfrei</br>• 49 % Leerlauf bis 20 % Leerlauf = Warnung oder Überwachung</br>• 19% Leerlauf bis 0% Leerlauf = kritisch oder außerhalb der Spezifikation |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |• 0,001 ms bis 0,015 ms = fehlerfrei</br>• 0,015 ms bis 0,025 ms = Warnung oder Überwachung</br>• 0,026ms oder mehr = kritisch oder außerhalb der Spezifikation |
| Logical Disk(Physical Disk)--Current Disk Queue Length (für alle Instanzen) |80 Anforderungen über einen Zeitraum von mehr als 6 Minuten |
| Memory--Pool Non Paged Bytes |• Weniger als 60% des Pools verbraucht = fehlerfrei<br>• 61 % bis 80 % des Pools verbraucht = Warnung oder Überwachung</br>• Mehr als 80% des Pools verbraucht = kritisch oder außerhalb der Spezifikation |
| Memory--Pool Paged Bytes |• Weniger als 60% des Pools verbraucht = fehlerfrei</br>• 61 % bis 80 % des Pools verbraucht = Warnung oder Überwachung</br>• Mehr als 80% des Pools verbraucht = kritisch oder außerhalb der Spezifikation |
| Memory--Available Megabytes |• 50% freier Arbeitsspeicher oder mehr = fehlerfrei</br>• 25 % freier Arbeitsspeicher = Überwachung</br>• 10 % freier Arbeitsspeicher = Warnung</br>• Weniger als 100MB oder 5% freier Arbeitsspeicher verfügbar = kritisch oder außerhalb der Spezifikation |
| Processor--\%Processor Time (all instances) |• Less than 60% consumed = Healthy</br>• 61% to 90% consumed = Monitor or Caution</br>• 91% to 100% consumed = Critical |

> [!NOTE]
> If you determine that the infrastructure is the culprit, we recommend that you defragment the disks regularly for better performance.
>
>

<a id="cause2"></a>

## Cause: Another process or antivirus software interfering with Azure Backup
We've seen several instances where other processes in the Windows system have negatively affected performance of the Azure Backup agent process. For example, if you use both the Azure Backup agent and another program to back up data, or if antivirus software is running and has a lock on files to be backed up, the multiple locks on files might cause contention. In this situation, the backup might fail, or the job might take longer than expected.

The best recommendation in this scenario is to turn off the other backup program to see whether the backup time for the Azure Backup agent changes. Usually, making sure that multiple backup jobs are not running at the same time is sufficient to prevent them from affecting each other.

For antivirus programs, we recommend that you exclude the following files and locations:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe as a process
* C:\Program Files\Microsoft Azure Recovery Services Agent\ folders
* Scratch location (if you're not using the standard location)

<a id="cause3"></a>

## Cause: Backup agent running on an Azure virtual machine
If you're running the Backup agent on a VM, performance will be slower than when you run it on a physical machine. This is expected due to IOPS limitations.  However, you can optimize the performance by switching the data drives that are being backed up to Azure Premium Storage. We're working on fixing this issue, and the fix will be available in a future release.

<a id="cause4"></a>

## Cause: Backing up a large number (millions) of files
Moving a large volume of data will take longer than moving a smaller volume of data. In some cases, backup time is related to not only the size of the data, but also the number of files or folders. This is especially true when millions of small files (a few bytes to a few kilobytes) are being backed up.

This behavior occurs because while you're backing up the data and moving it to Azure, Azure is simultaneously cataloging your files. In some rare scenarios, the catalog operation might take longer than expected.

The following indicators can help you understand the bottleneck and accordingly work on the next steps:

* **UI is showing progress for the data transfer**. The data is still being transferred. The network bandwidth or the size of data might be causing delays.
* **UI is not showing progress for the data transfer**. Open the logs located at C:\Program Files\Microsoft Azure Recovery Services Agent\Temp, and then check for the FileProvider::EndData entry in the logs. This entry signifies that the data transfer finished and the catalog operation is happening. Don't cancel the backup jobs. Instead, wait a little longer for the catalog operation to finish. If the problem persists, contact [Azure support](https://portal.azure.com/#create/Microsoft.Support).Processor--\`Processor Time (alle Instanzen)es and folders in Azure Backup
description: Provides troubleshooting guidance to help you diagnose the cause of Azure Backup performance issues
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
---
# Troubleshoot slow backup of files and folders in Azure Backup
This article provides troubleshooting guidance to help you diagnose the cause of slow backup performance for files and folders when you're using Azure Backup. When you use the Azure Backup agent to back up files, the backup process might take longer than expected. This delay might be caused by one or more of the following:

* [There are performance bottlenecks on the computer that’s being backed up.](#cause1)
* [Another process or antivirus software is interfering with the Azure Backup process.](#cause2)
* [The Backup agent is running on an Azure virtual machine (VM).](#cause3)  
* [You're backing up a large number (millions) of files.](#cause4)

Before you start troubleshooting issues, we recommend that you download and install the [latest Azure Backup agent](https://aka.ms/azurebackup_agent). We make frequent
updates to the Backup agent to fix various issues, add features, and improve performance.

We also strongly recommend that you review the [Azure Backup service FAQ](backup-azure-backup-faq.md) to make sure you're not experiencing any of the common configuration issues.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## Cause: Performance bottlenecks on the computer
Bottlenecks on the computer that's being backed up can cause delays. For example, the computer's ability to read or write to disk, or available bandwidth to send data over the network, can cause bottlenecks.

Windows provides a built-in tool that's called [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) to detect these bottlenecks.

Here are some performance counters and ranges that can be helpful in diagnosing bottlenecks for optimal backups.

| Counter | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |• 100% idle to 50% idle = Healthy</br>• 49% idle to 20% idle = Warning or Monitor</br>• 19% idle to 0% idle = Critical or Out of Spec |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |• 0.001 ms to 0.015 ms  = Healthy</br>• 0.015 ms to 0.025 ms = Warning or Monitor</br>• 0.026 ms or longer = Critical or Out of Spec |
| Logical Disk(Physical Disk)--Current Disk Queue Length (for all instances) |80 requests for more than 6 minutes |
| Memory--Pool Non Paged Bytes |• Less than 60% of pool consumed = Healthy<br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Pool Paged Bytes |• Less than 60% of pool consumed = Healthy</br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Available Megabytes |• 50% of free memory available or more = Healthy</br>• 25% of free memory available = Monitor</br>• 10% of free memory available = Warning</br>• Less than 100 MB or 5% of free memory available = Critical or Out of Spec |
| Processor--\%Processor Time (all instances) |• Weniger als 60% verbraucht = fehlerfrei</br>• 61 % bis 90 % verbraucht = Überwachung oder Achtung</br>• 91% - 100% verbraucht = kritisch |

> [!NOTE]
> Wenn Sie feststellen, dass die Infrastruktur die Ursache ist, empfehlen wir, die Datenträger in regelmäßigen Abständen für eine bessere Leistung zu defragmentieren.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Ursache: Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt
Es sind mehrere Fälle bekannt, in denen sich andere Prozesse innerhalb des Windows-Systems negativ auf die Leistung des Azure Backup-Agent-Prozesses ausgewirkt haben. Falls Sie beispielsweise sowohl den Azure Backup-Agent als auch ein anderes Programm zum Sichern der Daten verwenden, oder Antivirensoftware ausgeführt wird, und die zu sichernden Dateien mit einer Sperre versehen wurden, können die mehrfachen Sperren der Dateien zu Konflikten führen. In diesem Fall kann bei der Sicherung ein Fehler auftreten, oder der Auftrag kann länger als erwartet dauern.

Bei diesem Szenario deaktivieren Sie am besten das andere Sicherungsprogramm, um zu testen, ob sich die Sicherungsdauer des Azure Backup-Agents ändert. Wenn Sie sicherstellen, dass nicht mehrere Sicherungsaufträge gleichzeitig ausgeführt werden, reicht dies normalerweise aus, um eine gegenseitige Behinderung zu vermeiden.

Für Antivirenprogramme empfehlen wir Ihnen, die folgenden Dateien und Speicherorte auszuschließen:

* „C:\Programme\Microsoft Azure Recovery Services Agent\bin\cbengine.exe“
* „C:\Programme\Microsoft Azure Recovery Services Agent\“
* Scratchverzeichnis (falls Sie nicht den Standardspeicherort verwenden).

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Ursache: Auf einem virtuellen Azure-Computer ausgeführter Backup-Agent
Wenn Sie den Backup-Agent auf einem virtuellen Computer ausführen, ist die Leistung niedriger als bei Ausführung auf einem physischen Computer. Dies ist aufgrund der IOPS-Beschränkungen zu erwarten.  Sie können die Leistung aber optimieren, indem Sie die Datenlaufwerke, die gesichert werden, auf Azure Storage Premium umstellen. Wir arbeiten an der Behebung dieses Problems, und die Lösung wird Teil einer zukünftigen Version sein.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Ursache: Sichern einer großen Zahl von Dateien (mehrere Millionen)
Das Verschieben einer großen Datenmenge dauert länger als das Verschieben einer kleineren Datenmenge . In einigen Fällen hängt die Sicherungszeit nicht nur von der Datenmenge ab, sondern auch von der Anzahl der Dateien oder Ordner. Dies ist insbesondere dann der Fall, wenn Millionen von kleinen Dateien (ein paar Bytes bis wenige Kilobytes) gesichert werden.

Dieses Verhalten tritt auf, weil Azure simultan Ihre Dateien katalogisiert, während Sie die Daten sichern und in Azure verschieben. In einigen seltenen Fällen kann die Katalogisierung länger als erwartet dauern.

Anhand der folgenden Indikatoren können Sie den Engpass identifizieren und dann die entsprechenden nächsten Schritte ausführen:

* **Auf der Benutzeroberfläche wird der Status der Datenübertragung angezeigt**. Die Daten werden immer noch übertragen. Die Netzwerkbandbreite oder die Datenmenge könnte möglicherweise Verzögerungen verursachen.
* **Auf der Benutzeroberfläche wird der Status der Datenübertragung nicht angezeigt**. Öffnen Sie die Protokolle unter „C:\Programme\Microsoft Azure Recovery Services Agent\Temp“, und suchen Sie in den Protokollen nach dem Eintrag „FileProvider::EndData“. Dieser Eintrag verdeutlicht, dass die Datenübertragung abgeschlossen ist und der Katalogisierungsvorgang durchgeführt wird. Brechen Sie die Sicherungsaufträge nicht ab. Warten Sie stattdessen noch ein wenig, bis die Katalogisierung beendet ist. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support), wenn das Problem weiterhin besteht.
