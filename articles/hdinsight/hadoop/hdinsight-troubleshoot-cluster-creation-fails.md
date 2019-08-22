---
title: Behandeln von Fehlern bei der Clustererstellung mit Azure HDInsight
description: Hier erfahren Sie, wie Sie Probleme bei der Clustererstellung für Azure HDInsight behandeln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/06/2019
ms.openlocfilehash: c7092b2cbcef01ef71261b6f5498cde56a40c358
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856533"
---
# <a name="troubleshoot-cluster-creation-failures-with-azure-hdinsight"></a>Behandeln von Fehlern bei der Clustererstellung mit Azure HDInsight

Fehler bei der Clustererstellung sind häufig auf folgende Probleme zurückzuführen:

- Berechtigungsprobleme
- Einschränkungen durch Ressourcenrichtlinien
- Firewalls
- Ressourcensperren
- Nicht unterstützte Komponentenversionen
- Einschränkungen für Speicherkontonamen
- Dienstausfälle

## <a name="permissions-issues"></a>Berechtigungsprobleme

Achten Sie bei Verwendung von Data Lake Storage Gen2 darauf, dass sich die benutzerseitig zugewiesene verwaltete Identität, die Ihrem HDInsight-Cluster zugewiesen ist, entweder in der Rolle  **Mitwirkender an Speicherblobdaten** oder in der Rolle **Besitzer von Speicherblobdaten** befindet. Eine umfassende Einrichtungsanleitung finden Sie unter [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-storage-gen2.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account).

Die Einrichtungs- und Konfigurationsanleitung für Data Lake Storage Gen1 finden Sie [hier](../hdinsight-hadoop-use-data-lake-store.md). Data Lake Storage Gen1 wird für HBase-Cluster sowie in der HDInsight-Version 4.0 nicht unterstützt.

Achten Sie bei Verwendung von Azure Storage darauf, dass der Name des Speicherkontos bei der Clustererstellung gültig ist.

## <a name="resource-policy-restrictions"></a>Einschränkungen durch Ressourcenrichtlinien

Abonnementbasierte Azure-Richtlinien können die Erstellung öffentlicher IP-Adressen verhindern. Zum Erstellen von HDInsight-Clustern sind zwei öffentliche IP-Adressen erforderlich.  

Ganz allgemein können sich die folgenden Richtlinien auf die Clustererstellung auswirken:

* Richtlinien, die die Erstellung von IP-Adressen und Lastenausgleichsmodulen innerhalb des Abonnements verhindern
* Eine Richtlinie, die die Erstellung eines Speicherkontos verhindert
* Eine Richtlinie, die das Löschen von Netzwerkressourcen (IP-Adressen und Lastenausgleichsmodule) verhindert

## <a name="firewalls"></a>Firewalls

Firewalls in Ihrem virtuellen Netzwerk oder Speicherkonto können die Kommunikation mit HDInsight-Verwaltungs-IP-Adressen verhindern.

Lassen Sie Datenverkehr von den IP-Adressen aus der folgenden Tabelle zu:

| Quell-IP-Adresse | Destination | Direction |
|---|---|---|
| 168.61.49.99 | *:443 | Eingehend |
| 23.99.5.239 | *:443 | Eingehend |
| 168.61.48.131 | *:443 | Eingehend |
| 138.91.141.162 | *:443 | Eingehend |

Fügen Sie auch die spezifischen IP-Adressen für die Region hinzu, in der der Cluster erstellt wird. Eine Liste mit den Adressen für die einzelnen Azure-Regionen finden Sie unter [HDInsight-Verwaltungs-IP-Adressen](../hdinsight-management-ip-addresses.md).

Wenn Sie ExpressRoute oder Ihren eigenen benutzerdefinierten DNS-Server verwenden, lesen Sie [Verbinden von mehreren Netzwerken](../hdinsight-plan-virtual-network-deployment.md#multinet) im Artikel „Planen eines virtuellen Netzwerks für Azure HDInsight“.

## <a name="resources-locks"></a>Ressourcensperren  

Vergewissern Sie sich, dass keine [Sperren für Ihr virtuelles Netzwerk oder für Ihre Ressourcengruppe](../../azure-resource-manager/resource-group-lock-resources.md) vorhanden sind.  

## <a name="unsupported-component-versions"></a>Nicht unterstützte Komponentenversionen

Vergewissern Sie sich, dass Sie in Ihrer Lösung eine [unterstützte Version von Azure HDInsight](../hdinsight-component-versioning.md) sowie unterstützte Versionen von [Apache Hadoop-Komponenten](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) verwenden.  

## <a name="storage-account-name-restrictions"></a>Einschränkungen für Speicherkontonamen

Speicherkontonamen dürfen nicht mehr als 24 Zeichen und keine Sonderzeichen enthalten. Diese Einschränkungen gelten auch für den Standardcontainernamen im Speicherkonto.

## <a name="service-outages"></a>Dienstausfälle

Überprüfen Sie den [Azure-Status](https://status.azure.com/status) auf mögliche Dienstunterbrechungen oder -probleme.

## <a name="next-steps"></a>Nächste Schritte

* [Erweitern von Azure HDInsight per Azure Virtual Network](../hdinsight-plan-virtual-network-deployment.md)
* [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Verwenden von Azure Storage mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-blob-storage.md)
* [Einrichten von Clustern in HDInsight mit Apache Hadoop, Apache Spark, Apache Kafka usw.](../hdinsight-hadoop-provision-linux-clusters.md)
