---
title: Durchführen eines Upgrades für HDInsight-Cluster auf eine neuere Version – Azure
description: Hier finden Sie Richtlinien für das Upgrade Ihres Azure HDInsight-Clusters auf eine neuere Version.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: f4458b33edb9d55c30de8c5168668743abb33161
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70811644"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Aktualisieren eines HDInsight-Clusters auf eine neuere Version
Um die neuesten HDInsight-Funktionen nutzen zu können, wird empfohlen, HDInsight-Cluster auf die neueste Version zu aktualisieren. Befolgen Sie die nachstehenden Leitlinien zum Aktualisieren Ihrer HDInsight-Clusterversionen.

> [!NOTE]  
> Informationen zu unterstützten HDInsight-Versionen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="upgrade-tasks"></a>Aufgaben für das Upgrade
Der Workflow für ein Upgrade eines HDInsight-Clusters ist wie folgt.

![Diagramm des Upgradeworkflows](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Lesen Sie jeden Abschnitt dieses Dokuments, um zu verstehen, welche Änderungen möglicherweise erforderlich sind, wenn Sie Ihren HDInsight-Cluster aktualisieren.
2. Erstellen Sie einen Cluster als Test-/Qualitätssicherungsumgebung. Weitere Informationen zur Erstellung eines Clusters finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
3. Kopieren Sie vorhandene Aufträge, Datenquellen und Senken in die neue Umgebung.
4. Führen Sie Überprüfungstests durch, um sicherzustellen, dass Ihre Aufträge im neuen Cluster wie erwartet funktionieren.

Nachdem Sie überprüft haben, dass alles wie erwartet funktioniert, planen Sie für die Migration eine Ausfallzeit ein. Führen Sie während dieser Ausfallzeit die folgenden Aktionen durch:

1.  Sichern Sie alle Daten, die vorübergehend lokal auf dem Clusterknoten gespeichert sind. zum Beispiel direkt auf einem Hauptknoten gespeicherte Daten.
2.  Löschen Sie den vorhandenen Cluster.
3.  Erstellen Sie einen Cluster im gleichen VNET-Subnetz mit der aktuellen (oder einer unterstützten) HDI-Version, die den gleichen Standarddatenspeicher wie der vorherige Cluster verwendet. Dadurch wird dem neuen Cluster ermöglicht, weiterhin mit Ihren vorhandenen Produktionsdaten zu arbeiten.
4.  Importieren Sie alle vorübergehenden Daten, die Sie gesichert haben.
5.  Starten Sie Aufträge/Verarbeiten Sie weiterhin mithilfe des neuen Clusters.

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie, wie Sie Linux-basierte HDInsight-Cluster erstellen](hdinsight-hadoop-provision-linux-clusters.md)
* [Herstellen von Verbindungen mit HDInsight per SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwalten eines Linux-basierten Clusters mithilfe von Apache Ambari](hdinsight-hadoop-manage-ambari.md)

