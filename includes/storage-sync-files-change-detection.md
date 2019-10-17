---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: rogarana
ms.openlocfilehash: 55456a6be938411d3c08a0eaa8fdbfb0844e7129
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72391649"
---
Änderungen, die über das Azure-Portal oder den SMB an der Azure-Dateifreigabe vorgenommen wurden, werden im Gegensatz zu Änderungen am Serverendpunkt nicht sofort erkannt und repliziert. Azure Files verfügt bislang über keine Änderungsmitteilungen oder Journalfunktion, sodass es keine Möglichkeit gibt, eine Synchronisierungssitzung automatisch zu initiieren, sobald Dateien geändert werden. Unter Windows Server verwendet die Azure-Dateisynchronisierung das [Windows-USN-Journaling](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx), um automatisch eine Synchronisierungssitzung zu starten, wenn sich Dateien ändern.

Zur Erkennung von Änderungen an der Azure-Dateifreigabe verfügt die Azure-Dateisynchronisierung über einen geplanten Auftrag: den so genannten *Änderungserkennungsauftrag*. Ein Änderungserkennungsauftrag zählt jede Datei in der Dateifreigabe auf und vergleicht sie anschließend mit der Synchronisierungsversion für die betreffende Datei. Wenn der Änderungserkennungsauftrag feststellt, dass sich Dateien geändert haben, startet die Azure-Dateisynchronisierung eine Synchronisierungssitzung. Der Änderungserkennungsauftrag wird alle 24 Stunden ausgelöst. Da der Änderungserkennungsauftrag jede Datei in der Dateifreigabe von Azure aufzählt, dauert die Änderungserkennung bei großen Namespaces länger als bei kleineren Namespaces. Bei großen Namespaces dauert die Ermittlung der geänderten Dateien unter Umständen länger als 24 Stunden.

Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet **Invoke-AzStorageSyncChangeDetection** verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Dieses Cmdlet ist für Szenarien vorgesehen, in denen irgendein automatisierter Prozess Änderungen in der Azure-Dateifreigabe vornimmt oder die Änderungen von einem Administrator vorgenommen werden (etwa Verschieben von Dateien und Verzeichnissen in die Freigabe). Für Endbenutzeränderungen empfiehlt es sich, den Azure-Dateisynchronisierungs-Agent auf einem virtuellen IaaS-Computer zu installieren und Endbenutzern den Zugriff auf die Dateifreigabe über den virtuellen IaaS-Computer zu ermöglichen. Auf diese Weise werden alle Änderungen schnell mit anderen Agents synchronisiert, ohne dass das Cmdlet „Invoke-AzStorageSyncChangeDetection“ verwendet werden muss. Weitere Informationen hierzu finden Sie in der Dokumentation von [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection).

>[!NOTE]
>Änderungen, die mithilfe von REST an einer Azure-Dateifreigabe vorgenommen wurden, bewirken keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.

Möglicherweise wird für Azure-Dateifreigaben noch eine Änderungserkennung hinzugefügt, die mit USN für Volumes unter Windows Server vergleichbar ist. Helfen Sie uns, dieses Feature für die künftige Entwicklung zu priorisieren, indem Sie auf der [UserVoice-Seite zu Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files) dafür abstimmen.
