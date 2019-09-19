---
title: Erstellen einer Azure HPC Cache-Instanz
description: Hier erfahren Sie, wie Sie eine Azure HPC Cache-Instanz erstellen.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 41361a3513c052d960726498d55745bf09afdfbb
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775026"
---
# <a name="configure-aggregated-namespace"></a>Konfigurieren eines aggregierten Namespace
<!-- change link in GUI -->

Azure HPC Cache ermöglicht Clients den Zugriff auf eine Vielzahl von Speichersystemen über einen virtuellen Namespace, der die Details des Back-End-Speichersystems verbirgt.

Beim Hinzufügen eines Speicherziels legen Sie den clientseitigen Dateipfad fest. Clientcomputer binden diesen Dateipfad ein. Sie können das Speicherziel ändern, das diesem Pfad zugeordnet ist. Beispielsweise können Sie ein Hardware-Speichersystem durch Cloudspeicher ersetzen, ohne die clientseitigen Prozeduren erneut generieren zu müssen.

## <a name="aggregated-namespace-example"></a>Beispiel zum aggregierten Namespace

Planen Sie Ihren aggregierten Namespace so, dass die Clientcomputer komfortabel an die benötigten Informationen gelangen und Administratoren und Workflowtechniker die Pfade leicht unterscheiden können.

Ziehen Sie beispielsweise ein System in Erwägung, in dem eine Instanz von Azure HPC Cache zum Verarbeiten der im Azure-Blob gespeicherten Daten verwendet wird. Für die Analyse sind Vorlagendateien erforderlich, die in einem lokalen Rechenzentrum gespeichert sind.

Die Vorlagendaten sind in einem Rechenzentrum gespeichert, und die für diesen Auftrag erforderlichen Informationen sind in diesen Unterverzeichnissen gespeichert:

    /goldline/templates/acme2017/sku798
    /goldline/templates/acme2017/sku980 

Das Speichersystem des Rechenzentrums macht diese Exporte verfügbar: 

    /
    /goldline
    /goldline/templates

Die zu analysierenden Daten wurden mithilfe des [CLFSLoad-Hilfsprogramms](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload) in einen Azure-Blobspeichercontainer mit dem Namen "sourcecollection" kopiert).

Um den komfortablen Zugriff über den Cache zu ermöglichen, sollten Sie das Erstellen von Speicherzielen mit diesen virtuellen Namespacepfaden in Erwägung ziehen:

| NFS-Back-End-Dateipfad oder Blobcontainer | Pfad des virtuellen Namespace |
|-----------------------------------------|------------------------|
| /goldline/templates/acme2017/sku798     | /templates/sku798      |
| /goldline/templates/acme2017/sku980     | /templates/sku980      |
| sourcecollection                        | /source/               |

Da es sich bei den NFS-Quellpfaden um Unterverzeichnisse des gleichen Exports handelt, müssen Sie für das gleiche Speicherziel mehrere Namespacepfade definieren. 

| Hostname des Speicherziels  | NFS-Exportpfad      | Unterverzeichnispfad | Namespacepfad    |
|--------------------------|----------------------|-------------------|-------------------|
| *IP-Adresse oder Hostname* | /goldline/templates  | acme2017/sku798   | /templates/sku798 |
| *IP-Adresse oder Hostname* | /goldline/templates  | acme2017/sku980   | /templates/sku980 |

Eine Clientanwendung kann den Cache einbinden und komfortabel auf die Dateipfade „/source“, „/templates/sku798“ und „/templates/sku980“ des aggregierten Namespace zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich dafür entschieden haben, wie Sie Ihr virtuelles Dateisystem einrichten möchten, [erstellen Sie Speicherziele](hpc-cache-add-storage.md), um Ihren Back-End-Speicher Ihren clientseitigen virtuellen Dateipfaden zuzuordnen.
