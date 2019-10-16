---
title: Regionale Redundanz und Failoverwiederherstellung mit Azure HPC Cache (Vorschauversion)
description: Erfahren Sie etwas über Verfahren zur Bereitstellung von Failoverfunktionen für die Notfallwiederherstellung mit Azure HPC Cache.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: rohogue
ms.openlocfilehash: ca771ceed433c6eb0ed26bba6fcb1bf11db211f0
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72031035"
---
# <a name="use-multiple-caches-for-regional-failover-recovery"></a>Verwenden mehrerer Caches für die regionale Failoverwiederherstellung

Jede Azure HPC Cache-Instanz wird innerhalb eines bestimmten Abonnements und in genau einer Region ausgeführt. Dies bedeutet, dass der Cacheworkflow möglicherweise unterbrochen wird, wenn die Region vollständig ausfällt.

In diesem Artikel wird eine Strategie beschrieben, mit der Sie das Risiko einer Unterbrechung der Arbeit reduzieren können, indem Sie eine zweite Region für das Cachefailover verwenden.

Zentral ist dabei die Verwendung von Back-End-Speicher, auf den von mehreren Regionen aus zugegriffen werden kann. Bei diesem Speicher kann es sich um ein lokales NAS-System mit entsprechender DNS-Unterstützung oder um eine Azure Blob Storage-Instanz handeln, die sich in einer anderen Region als der Cache befindet.

Während Ihr Workflow in Ihrer primären Region ausgeführt wird, werden die Daten im langfristigen Speicher außerhalb der Region gespeichert. Wenn die Cacheregion nicht mehr verfügbar ist, können Sie ein Duplikat der Azure HPC Cache-Instanz in einer sekundären Region erstellen und mit dem gleichen Speicher verbinden und dann die Arbeit mit dem neuen Cache fortsetzen.

## <a name="planning-for-regional-failover"></a>Planen regionaler Failover

Führen Sie die folgenden Schritte aus, um einen Cache einzurichten, der auf ein mögliches Failover vorbereitet ist:

1. Stellen Sie sicher, dass von einer zweiten Region aus auf den Back-End-Speicher zugegriffen werden kann.
1. Sie sollten schon bei der Planung der primären Cache-Instanz die Replikation dieses Konfigurationsschritts in der zweiten Region vorbereiten. Fügen Sie Folgendes ein:

   1. Struktur mit virtuellem Netzwerk und Subnetz
   1. Cachekapazität
   1. Details des Speicherziels, Namen und Namespacepfade
   1. Details zu Clientcomputern, sofern diese sich in derselben Region wie der Cache befinden
   1. Befehl zum Einbinden für die Verwendung durch Cacheclients

   > [!NOTE]
   > Azure HPC Cache kann programmgesteuert erstellt werden – über eine [Azure Resource Manager-Vorlage](../azure-resource-manager/template-deployment-overview.md) oder durch direkten Zugriff auf die API. Ausführliche Informationen erhalten Sie vom Azure HPC Cache-Team.

## <a name="failover-example"></a>Beispiel für ein Failover

Gehen Sie beispielsweise davon aus, dass Sie Ihre Azure HPC Cache-Instanz in der Region „USA, Osten“ erstellen möchten. Sie greift auf Daten zu, die in Ihrem lokalen Rechenzentrum gespeichert sind.

Sie können einen Cache in der Region „USA, Westen 2“ als Sicherung für Failover verwenden.

Beim Erstellen des Caches in der Region „USA, Osten“ bereiten Sie einen zweiten Cache für die Bereitstellung in der Region „USA, Westen 2“ vor. Sie können Skripts oder Vorlagen verwenden, um diese Vorbereitungsschritte zu automatisieren.

Erstellen Sie im Fall eines regionsweiten Ausfalls in der Region „USA, Osten“ den Cache, den Sie in der Region „USA, Westen 2“ vorbereitet haben.

Fügen Sie nach dem Erstellen des Caches Speicherziele hinzu, die auf dieselben lokalen Datenspeicher verweisen, und verwenden Sie dieselben aggregierten Namespacepfade wie für die Speicherziele des alten Caches.

Wenn die ursprünglichen Clients betroffen sind, erstellen Sie neue Clients in der Region „USA, Westen 2“ für die Verwendung mit dem neuen Cache.

Alle Clients müssen den neuen Cache einbinden, auch wenn sie nicht vom Regionsausfall betroffen waren. Der neue Cache hat andere Einbindungsadressen als der alte.

## <a name="learn-more"></a>Weitere Informationen

Im Leitfaden zur Azure-Anwendungsarchitektur finden Sie weitere Informationen zur [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](<https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region>).
<!-- this should be an internal link instead of a URL but I can't find the tree  -->
