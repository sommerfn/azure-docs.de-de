---
title: Verschieben von Daten in einen Azure HPC Cache-Cloudcontainer
description: Auffüllen von Azure Blob Storage für die Verwendung mit Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 07a97b1afa8049ace97f1589393cd76c24f21368
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775166"
---
# <a name="move-data-to-azure-blob-storage-for-azure-hpc-cache"></a>Verschieben von Daten in Azure Blob Storage für Azure HPC Cache

Wenn Ihr Workflow das Verschieben von Daten in Azure Blob Storage beinhaltet, achten Sie darauf, eine effiziente Strategie zum Kopieren Ihrer Daten über den Azure HPC Cache zu verwenden.

Dieser Artikel erläutert die besten Verfahren zum Verschieben von Daten in Blobspeicher für die Verwendung mit Azure HPC Cache.

Beachten Sie dabei Folgendes:

* Azure HPC Cache verwendet ein spezielles Speicherformat zum Strukturieren der Daten in Blobspeicher. Aus diesem Grund muss ein Blob Storage-Ziel entweder ein neuer, leerer Container oder ein Blobcontainer sein, der zuvor für Azure HPC Cache-Daten verwendet wurde. ([Avere vFXT für Azure](https://azure.microsoft.com/services/storage/avere-vfxt/) verwendet ebenfalls dieses Clouddateisystem.)

* Das Kopieren von Daten über den Azure HPC Cache funktioniert am besten bei mehreren Clients und parallelen Vorgängen. Ein einfacher Kopierbefehl von einem Client verschiebt Daten langsam.

Es steht ein Python-basiertes Hilfsprogramm zum Laden von Inhalten in einen Blobspeichercontainer zur Verfügung. Weitere Informationen finden Sie unter [Vorabladen von Daten in Blob Storage](#pre-load-data-in-blob-storage-with-clfsload).

Wenn Sie das Hilfsprogramm zum Laden nicht verwenden oder Inhalte zu einem vorhandenen Speicherziel hinzufügen möchten, befolgen Sie die Tipps zur parallelen Datenerfassung in [Kopieren von Daten über den Azure HPC Cache](#copy-data-through-the-azure-hpc-cache). 

## <a name="pre-load-data-in-blob-storage-with-clfsload"></a>Vorabladen von Daten in Blob Storage mit CLFSLoad

Sie können das Hilfsprogramm [Avere CLFSLoad](https://aka.ms/avere-clfsload) verwenden, um Daten in einen neuen Blobspeichercontainer zu kopieren, bevor Sie ihn als Speicherziel hinzufügen. Dieses Hilfsprogramm wird in einer Linux-VM ausgeführt und schreibt Daten in dem proprietären Format, das für Azure HPC Cache erforderlich ist. Dies ist die effizienteste Methode zum Auffüllen eines Blobspeichercontainers für die Verwendung mit dem Cache.

Diese Option funktioniert nur mit neuen, leeren Containern. Erstellen Sie den Container, bevor Sie Avere CLFSLoad verwenden.

Ausführliche Informationen finden Sie in der [Avere CLFSLoad-Infodatei](https://github.com/microsoft/Avere-CLFSLoad/blob/master/README.md). <!-- caution literal link -->

Allgemeine Übersicht des Vorgangs:

1. Bereiten Sie ein Linux-System (physisch oder VM) mit Python-Version 3.6 oder höher vor. (Empfehlenswert ist Python 3.7, das eine bessere Leistung erzielt.)
1. Installieren Sie die Avere CLFSLoad-Software auf dem Linux-System.
1. Führen Sie die Übertragung über die Linux-Befehlszeile aus.

Das Avere CLFSLoad-Hilfsprogramm benötigt die folgenden Informationen:

* Die ID des Speicherkontos, das Ihren Blobspeichercontainer enthält
* Den Namen des leeren Blobspeichercontainers
* Ein SAS-Token (Shared Access Signature), das dem Hilfsprogramm das Schreiben in den Container ermöglicht
* Einen lokalen Pfad zur Datenquelle – entweder ein lokales Verzeichnis, das die zu kopierenden Daten enthält, oder einen lokalen Pfad zu einem eingebundenen Remotesystem mit den Daten.

Die Anforderungen werden ausführlich in der [Avere CLFSLoad-Infodatei](https://aka.ms/avere-clfsload) erläutert.

## <a name="copy-data-through-the-azure-hpc-cache"></a>Kopieren von Daten über den Azure HPC Cache

Wenn Sie das Avere CLFSLoad-Hilfsprogramm nicht verwenden oder einem vorhandenen Blobspeicherziel eine große Datenmenge hinzufügen möchten, können Sie die Daten über den Cache kopieren. Azure HPC Cache wurde dafür ausgelegt, mehrere Clients gleichzeitig zu bedienen, daher sollten Sie zum Kopieren von Daten über den Cache parallele Schreibvorgänge von mehreren Clients verwenden.

![Diagramm der Datenverschiebung mit mehreren Clients und mehreren Threads: Oben links befindet sich ein Symbol für den lokalen Hardwarespeicher, von dem mehrere Pfeile ausgehen. Die Pfeile zeigen auf vier Clientcomputer. Von jedem Clientcomputer zeigen drei Pfeile auf den Azure HPC Cache. Vom Azure HPC Cache zeigen mehrere Pfeile zum Blobspeicher.](media/hpc-cache-parallel-ingest.png) 

Die Befehle ``cp`` oder ``copy``, die normalerweise verwendet werden, um Daten von einem Speichersystem zu einem anderen zu übertragen, sind Singlethread-Prozesse, die jeweils nur eine Datei zugleich kopieren. Das bedeutet, dass der Dateiserver immer nur eine Datei auf einmal erfasst, was eine Verschwendung der Ressourcen des Clusters darstellt.

In diesem Abschnitt werden Strategien zur Erstellung eines Dateikopiersystems mit mehreren Clients und mehreren Threads erläutert, um Daten mithilfe von Azure HPC Cache in Blobspeicher zu verschieben. Es werden Konzepte für die Dateiübertragung und Entscheidungspunkte erläutert, die für eine effiziente Datenkopie mit mehreren Clients und einfachen Kopierbefehlen verwendet werden können.

Es werden auch einige Hilfsprogramme erläutert, die hilfreich sein können. Das Hilfsprogramm ``msrsync`` kann verwendet werden, um den Prozess der Aufteilung eines Datasets in Buckets und der Verwendung von rsync-Befehlen teilweise zu automatisieren. Das ``parallelcp``-Skript ist ein weiteres Hilfsprogramm, das das Quellverzeichnis liest und automatisch Kopierbefehle ausgibt.

### <a name="strategic-planning"></a>Strategische Planung

Wenn Sie eine Strategie zum parallelen Kopieren von Daten erstellen, sollten Sie die Kompromisse hinsichtlich Dateigröße, Anzahl der Dateien und Verzeichnistiefe kennen.

* Bei kleinen Dateien ist die relevante Metrik „Dateien pro Sekunde“.
* Bei großen Dateien (10 MB oder mehr) ist die relevante Metrik „Bytes pro Sekunde“.

Jeder Kopiervorgang hat eine Durchsatzrate und eine Übertragungsrate, die gemessen werden kann, indem die Dauer des Kopierbefehls gemessen wird und Dateigröße sowie Anzahl der Dateien berücksichtigt werden. Die Erläuterung der Vorgehensweise zur Messung der Raten liegt außerhalb des Rahmens dieses Dokuments, aber es ist unbedingt erforderlich zu verstehen, ob es sich um kleine oder große Dateien handelt.

Zu den Strategien für die parallele Datenerfassung mit Azure HPC Cache gehören:

* Manuelles Kopieren: Sie können eine Kopie mit mehreren Threads manuell auf einem Client erstellen, indem Sie mehrere Kopierbefehle für vordefinierte Gruppen von Dateien oder Pfaden gleichzeitig im Hintergrund ausführen. Details finden Sie unter [Azure HPC Cache-Datenerfassung: manuelle Kopiermethode](hpc-cache-ingest-manual.md).

* Das teilweise automatisierte Kopieren mithilfe von ``msrsync`` - ``msrsync`` stellt ein Wrapper-Hilfsprogramm dar, das mehrere ``rsync``-Prozesse parallel ausführt. Details finden Sie unter [Azure HPC Cache-Datenerfassung: msrsync-Methode](hpc-cache-ingest-msrsync.md).

* Skriptgesteuertes Kopieren mit ``parallelcp``: Erfahren Sie in [Azure HPC Cache-Datenerfassung: parallele Kopierskriptmethode](hpc-cache-ingest-parallelcp.md), wie Sie ein paralleles Kopierskript erstellen und ausführen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Speicher eingerichtet haben, erfahren Sie, wie Clients den Cache einbinden können.

* [Zugreifen auf das Azure HPC Cache-System](hpc-cache-mount.md)
