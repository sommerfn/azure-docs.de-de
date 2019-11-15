---
title: Verwalten und Aktualisieren von Azure HPC Cache
description: Verwalten und Aktualisieren von Azure HPC Cache im Azure-Portal
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 62b54bfe120acdde1fd22c4a0d04165ea7243b50
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582200"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Verwalten des Caches im Azure-Portal

Auf der Übersichtsseite für den Cache im Azure-Portal werden Projektdetails, der Cachestatus und grundlegende Statistiken für den Cache angezeigt. Die Seite umfasst außerdem Steuerelemente zum Löschen des Caches, Leeren der Daten in den Langzeitspeicher oder Aktualisieren der Software.

Um die Übersichtsseite zu öffnen, wählen Sie die Cacheressource im Azure-Portal aus. Laden Sie z. B. die Seite **Alle Ressourcen**, und klicken Sie auf den Namen des Caches.

![Screenshot: Übersichtsseite einer Azure HPC Cache-Instanz](media/hpc-cache-overview.png) <!-- placeholder is identical to hpc-cache-new-overview.png; replace with better image (showing graphs, full sidebar) when available -->

Über die Schaltflächen oben auf der Seite können Sie den Cache verwalten:

* [**Leeren:** ](#flush-cached-data) Alle zwischengespeicherten Daten werden in Speicherziele geschrieben.
* [**Aktualisieren:** ](#upgrade-cache-software) Die Cachesoftware wird aktualisiert.
* **Aktualisieren:** Die Übersichtsseite wird erneut geladen.
* [**Löschen:** ](#delete-the-cache) Der Cache wird dauerhaft zerstört.

Nachfolgend finden Sie weitere Informationen zu diesen Optionen.

## <a name="flush-cached-data"></a>Leeren zwischengespeicherter Daten

Über die Schaltfläche **Leeren** auf der Übersichtsseite wird festgelegt, dass alle geänderten Daten im Cache sofort in die Back-End-Speicherziele geschrieben werden. Die Daten im Cache werden routinemäßig in den Speicherzielen gespeichert. Daher ist es nicht notwendig, dies manuell durchzuführen, es sei denn, Sie möchten sicherstellen, dass das Back-End-Speichersystem auf dem neuesten Stand ist. Beispielsweise können Sie **Leeren** verwenden, bevor Sie eine Momentaufnahme des Speichers erstellen oder die Größe des Datasets überprüfen.

> [!NOTE]
> Während der Leerung können im Cache keine Clientanforderungen verarbeitet werden. Der Cachezugriff wird angehalten und erst fortgesetzt, nachdem der Vorgang abgeschlossen wurde.

Wenn Sie den Leerungsvorgang für den Cache starten, werden im Cache keine Clientanforderungen mehr angenommen und der Cachestatus auf der Übersichtsseite ändert sich in **Wird geleert**.

Die Daten im Cache werden in den entsprechenden Speicherzielen gespeichert. Der Vorgang kann von wenigen Minuten bis zu über einer Stunde dauern, je nachdem, wie viele Daten zuvor in den Cache geschrieben wurden.

Nachdem alle Daten in den Speicherzielen gespeichert wurden, werden im Cache automatisch wieder Clientanforderungen angenommen. Als Cachestatus wird wieder **Fehlerfrei** angezeigt.

## <a name="upgrade-cache-software"></a>Ausführen eines Upgrades für die Cachesoftware

Wenn eine neue Softwareversion verfügbar ist, wird die Schaltfläche **Aktualisieren** aktiviert. Möglicherweise wird oben auf der Seite auch eine Meldung zur Aktualisierung der Software angezeigt.

![Screenshot: obere Schaltflächenleiste mit aktivierter Schaltfläche „Aktualisieren“](media/hpc-cache-upgrade-button.png)

Der Clientzugriff wird während eines Softwareupgrades nicht unterbrochen, die Leistung des Caches wird jedoch verlangsamt. Planen Sie die Aktualisierung der Software für Nebenzeiten oder innerhalb eines geplanten Wartungszeitraums.

Die Aktualisierung der Software kann mehrere Stunden in Anspruch nehmen. Bei Caches, die mit höherem Durchsatz konfiguriert wurden, dauert das Upgrade länger als bei Caches mit geringeren Spitzendurchsatzwerten.

Wenn ein Softwareupgrade verfügbar ist, können Sie es innerhalb mehrerer Tage manuell anwenden. Das Enddatum ist in der Upgrademeldung aufgeführt. Wenn Sie das Upgrade in diesem Zeitraum nicht durchführen, wird es in Azure automatisch auf den Cache angewandt. Die zeitliche Steuerung des automatischen Upgrades kann nicht konfiguriert werden. Wenn Sie Bedenken in Bezug auf die beeinträchtigte Cacheleistung haben, sollten Sie das Softwareupgrade vor Ablauf des entsprechenden Zeitraums manuell durchführen.

Klicken Sie auf die Schaltfläche **Aktualisieren**, um das Softwareupdate zu starten. Der Cachestatus ändert sich in **Upgrade wird ausgeführt**, bis der Vorgang abgeschlossen ist.

## <a name="delete-the-cache"></a>Löschen des Caches

Über die Schaltfläche **Löschen** wird der Cache zerstört. Wenn Sie einen Cache löschen, werden alle zugehörigen Ressourcen zerstört, und es fallen keine Kontogebühren mehr an.

Speicherziele sind vom Löschen des Caches nicht betroffen. Sie können sie später einem zukünftigen Cache hinzufügen oder ihre Verwendung einstellen.

Im Rahmen der endgültigen Beendigung des Caches werden alle nicht gespeicherten Daten in Speicherziele geleert.

## <a name="cache-metrics-and-monitoring"></a>Metriken und Überwachung des Caches

Auf der Übersichtsseite werden Diagramme für einige grundlegende Cachestatistiken angezeigt: Cachedurchsatz, Vorgänge pro Sekunde und Latenz.

![Screenshot: drei Liniendiagramme mit den oben genannten Statistiken für einen Beispielcache](media/hpc-cache-overview-stats.png)

Diese Diagramme sind Bestandteil der integrierten Überwachungs- und Analysetools von Azure. Weitere Tools und Warnungen sind auf den Seiten unter der Überschrift **Überwachung** auf der Seitenleiste des Portals verfügbar. Weitere Informationen finden Sie im Abschnitt zum Azure-Portal in der [Dokumentation zur Überwachung mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal).

## <a name="next-steps"></a>Nächste Schritte

<!-- * Learn more about metrics and statistics for hpc cache -->
* Weitere Informationen zu [Azure-Metriken und Statistiktools](../azure-monitor/index.yml)
* Hilfe zu [Azure HPC Cache](hpc-cache-support-ticket.md)
