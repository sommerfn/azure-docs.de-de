---
title: Übersicht über Azure HPC Cache
description: Beschreibt Azure HPC Cache, eine Dateizugriffsbeschleuniger-Lösung für High Performance Computing.
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 2a008d22de5df8d091e868153205697b4bb343ee
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582127"
---
# <a name="what-is-azure-hpc-cache"></a>Was ist Azure HPC Cache?

Azure HPC Cache beschleunigt den Zugriff auf Ihre Daten für High Performance Computing-Aufgaben (HPC). Aufgrund der Zwischenspeicherung von Dateien in Azure ermöglicht Azure HPC Cache für Ihren vorhandenen Workflow die Skalierbarkeit des Cloud Computing. Dieser Dienst kann auch für Workflows verwendet werden, bei denen Ihre Daten über WAN-Links gespeichert werden, z. B. in der NAS-Umgebung (Network Attached Storage) Ihres lokalen Rechenzentrums.

Azure HPC Cache lässt sich problemlos über das Azure-Portal starten und überwachen. Vorhandene NFS-Speicher oder neue BLOB-Container können Teil des aggregierten Namespace werden. Dies vereinfacht den Clientzugriff, auch wenn Sie das Back-End-Speicherziel ändern.

## <a name="use-cases"></a>Anwendungsfälle

Azure HPC Cache steigert die Produktivität am besten für Workflows wie diese:

* Workflow mit vielen Lesedateizugriffen
* Daten, die in über NFS zugänglichem Speicher, Azure Blob oder beidem gespeichert sind
* Computefarmen mit bis zu 75.000 CPU-Kernen

Azure HPC Cache kann einer Vielzahl von Workflows in vielen Branchen hinzugefügt werden. Jedes System, in dem eine große Anzahl von Computern bedarfsorientiert und mit geringer Latenzzeit auf einen Satz von Dateien zugreifen muss, profitiert von diesem Dienst. In den folgenden Abschnitten finden Sie spezifische Beispiele.

### <a name="visual-effects-vfx-rendering"></a>Rendern visueller Effekte (VFX)

Im Medien- und Unterhaltungsbereich kann Azure HPC Cache den Datenzugriff für zeitkritische Renderingprojekte beschleunigen. VFX-Renderingworkflows erfordern häufig eine Verarbeitung in letzter Minute durch eine große Anzahl von Computeknoten. Die Daten für diese Workflows befinden sich in der Regel in einer lokalen NAS-Umgebung. Azure HPC Cache kann diese Dateidaten in der Cloud zwischenspeichern, um die Latenzzeit zu verringern und die Flexibilität für das bedarfsgesteuerte Rendering zu erhöhen.

### <a name="life-sciences"></a>Biowissenschaften

Zahlreiche biowissenschaftliche Workflows können von der Zwischenspeicherung von Dateien mit horizontaler Skalierung profitieren.

Ein Forschungsinstitut, das seine genomischen Analyseworkflows zu Azure portieren möchte, kann sie mithilfe von Azure HPC Cache problemlos verschieben. Da der Cache den POSIX-Dateizugriff ermöglicht, kann der vorhandene Clientworkflow in der Cloud ausgeführt werden, ohne dass clientseitige Änderungen vorgenommen werden müssen.

Mit Azure HPC Cache kann auch die Effizienz von Aufgaben wie sekundärer Analyse, pharmakologischer Simulation oder KI-gesteuerter Bildanalyse verbessert werden.

### <a name="financial-services-analytics"></a>Analyse von Finanzdienstleistungen

Eine Azure HPC Cache-Bereitstellung kann zur Beschleunigung von quantitativen Analyseberechnungen, Risikoanalyse-Workloads und Monte-Carlo-Simulationen beitragen und Finanzdienstleistern so eine bessere Grundlage für strategische Entscheidungen bieten.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure HPC Cache ist in diesen Azure-Regionen verfügbar:

* East US
* USA (Ost) 2
* Nordeuropa
* Europa, Westen
* Asien, Südosten
* Sydney
* USA, Westen 2
* Korea, Mitte

Aktuelle Informationen zur Verfügbarkeit finden Sie auf der [Produktseite für Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache).

## <a name="service-availability"></a>Dienstverfügbarkeit

Sie müssen für jedes Abonnement, das Sie mit Azure HPC Cache verwenden möchten, Zugriff anfordern. Durch diese Einschränkung wird die Dienstqualität in den ersten Monaten der allgemeinen Verfügbarkeit sichergestellt.

Fordern Sie den Zugriff an, indem Sie [dieses Formular](https://aka.ms/onboard-hpc-cache) ausfüllen. Nachdem Ihr Abonnement der Zugriffsliste hinzugefügt wurde, können Sie Caches erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Funktionen finden Sie auf der [Produktseite für Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache).
* Informationen zu [Produktvoraussetzungen](hpc-cache-prereqs.md)
* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md) über das Azure-Portal
