---
title: Erstellen einer Azure HPC Cache-Instanz
description: Hier erfahren Sie, wie Sie eine Azure HPC Cache-Instanz erstellen.
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 1b9d2d3c46aab2096dd9208e0bea7f7776c0e429
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582707"
---
# <a name="create-an-azure-hpc-cache"></a>Erstellen einer Azure HPC Cache-Instanz

Erstellen Sie Ihren Cache mithilfe des Azure-Portals.

![Screenshot: Cacheübersicht im Azure-Portal mit der Schaltfläche „Erstellen“ im unteren Bereich](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definieren grundlegender Informationen

![Screenshot: Projektdetailseite im Azure-Portal](media/hpc-cache-create-basics.png)

Wählen Sie auf der Seite **Projektdetails** das Abonnement und die Ressourcengruppe zum Hosten des Caches aus. Achten Sie darauf, dass sich das Abonnement in der [Zugriffsliste](hpc-cache-prereqs.md#azure-subscription) befindet.

Legen Sie unter **Dienstdetails** den Cachenamen sowie folgende weitere Attribute fest:

* Standort: Wählen Sie eine der [unterstützten Regionen](hpc-cache-overview.md#region-availability) aus.
* Virtuelles Netzwerk: Sie können ein bereits vorhandenes virtuelles Netzwerk auswählen oder ein neues erstellen.
* Subnetz: Wählen Sie ein Subnetz mit mindestens 64 IP-Adressen (/24) aus, das ausschließlich für diese Azure HPC Cache-Instanz verwendet wird, oder erstellen Sie ein entsprechendes Subnetz.

## <a name="set-cache-capacity"></a>Festlegen der Cachekapazität
<!-- referenced from GUI - update aka.ms link if you change this header text -->

Legen Sie auf der Seite **Cache** die Kapazität Ihres Caches fest. Die hier festgelegten Werte bestimmen, wie viele Daten der Cache enthalten und wie schnell er Clientanforderungen bewältigen kann.

Nach dem Public Preview-Zeitraum hat die Kapazität zudem Auswirkungen auf die Kosten des Caches.

Legen Sie zum Auswählen der Kapazität die beiden folgenden Werte fest:

* Die maximale Datenübertragungsrate für den Cache (Durchsatz) in GB/Sekunde
* Die zugeordnete Speichermenge für zwischengespeicherte Daten in TB

Wählen Sie einen der verfügbaren Durchsatzwerte und eine der verfügbaren Cachespeichergrößen aus.

Beachten Sie, dass die tatsächliche Datenübertragungsrate von der Arbeitsauslastung, der Netzwerkgeschwindigkeit und der Art der Speicherziele abhängt. Die von Ihnen ausgewählten Werte bestimmen den maximalen Durchsatz für das gesamte Cachesystem, ein Teil davon wird jedoch für zusätzliche Aufgaben verwendet. Wenn ein Client beispielsweise eine Datei anfordert, die noch nicht im Cache gespeichert ist, oder wenn die Datei als veraltet markiert ist, nutzt der Cache einen Teil des Durchsatzes, um sie aus dem Back-End-Speicher abzurufen.

Mit Azure HPC Cache wird verwaltet, welche Dateien zwischengespeichert und vorab geladen werden, um die Cachetrefferraten zu maximieren. Der Cacheinhalt wird kontinuierlich bewertet, und seltener verwendete Dateien werden in den langfristigen Speicher verschoben. Wählen Sie eine Cachespeichergröße, die problemlos den aktiven Satz von Arbeitsdateien aufnehmen kann und Speicherplatz für Metadaten und andere Zusatzdaten bietet.

![Screenshot: Seite zum Festlegen der Cachegröße](media/hpc-cache-create-capacity.png)

## <a name="add-resource-tags-optional"></a>Hinzufügen von Ressourcentags (optional)

Auf der Seite **Tags** können Sie Ihrer Azure HPC Cache-Instanz [Ressourcentags](https://go.microsoft.com/fwlink/?linkid=873112) hinzufügen.

## <a name="finish-creating-the-cache"></a>Fertigstellen der Cacheerstellung

Nachdem Sie den neuen Cache konfiguriert haben, klicken Sie auf die Registerkarte **Überprüfen + erstellen**. Das Portal überprüft Ihre Auswahl und ermöglicht es Ihnen, die gewählten Optionen selbst noch einmal zu überprüfen. Ist alles korrekt, klicken Sie auf **Erstellen**.

Die Cacheerstellung dauert ungefähr zehn Minuten. Sie können den Status über den Benachrichtigungsbereich des Azure-Portals nachverfolgen.

![Screenshot der Cache-Erstellungsseiten „Bereitstellung wird ausgeführt“ und „Benachrichtigungen“ im Portal](media/hpc-cache-deploy-status.png)

Wenn die Erstellung abgeschlossen ist, wird eine Benachrichtigung mit einem Link zur neuen Azure HPC Cache-Instanz angezeigt, und der Cache wird in der Liste **Ressourcen** Ihres Abonnements angezeigt.

![Screenshot: Azure HPC Cache-Instanz im Azure-Portal](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Cache in der Liste **Ressourcen** angezeigt wird, definieren Sie Speicherziele, um dem Cache Zugriff auf Ihre Datenquellen zu gewähren.

* [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md)
