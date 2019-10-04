---
title: Behandeln von Fehlern bei der Clustererstellung mit Azure HDInsight
description: Hier erfahren Sie, wie Sie Probleme bei der Apache-Clustererstellung für Azure HDInsight behandeln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/26/2019
ms.openlocfilehash: 607eacc531166d9d770f31cc64825e8ffea9ca76
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70810677"
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

Gehen Sie wie folgt vor, wenn Sie Azure Data Lake Storage Gen2 verwenden und den Fehler ```AmbariClusterCreationFailedErrorCode```, ```Internal server error occurred while processing the request. Please retry the request or contact support.``` erhalten: Öffnen Sie das Azure-Portal, navigieren Sie zu Ihrem Storage-Konto, und vergewissern Sie sich unter „Zugriffssteuerung (IAM)“, dass die Rolle **Mitwirkender an Storage-Blobdaten** oder **Besitzer von Speicherblobdaten** Zugriff auf die **Benutzerseitig zugewiesene verwaltete Identität** für das Abonnement hat. Eine ausführliche Anleitung finden Sie unter [Festlegen von Berechtigungen für verwaltete Identitäten im Data Lake Storage Gen2-Konto](../hdinsight-hadoop-use-data-lake-storage-gen2.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account).

Die Einrichtungs- und Konfigurationsanleitung für Azure Data Lake Storage Gen1 finden Sie [hier](../hdinsight-hadoop-use-data-lake-store.md). Data Lake Storage Gen1 wird für HBase-Cluster sowie in der HDInsight-Version 4.0 nicht unterstützt.

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

Für die Clustererstellung gelten noch weitere Benennungseinschränkungen. Weitere Informationen finden Sie unter [Clustername](../hdinsight-hadoop-provision-linux-clusters.md#cluster-name).

## <a name="service-outages"></a>Dienstausfälle

Überprüfen Sie den [Azure-Status](https://status.azure.com/status) auf mögliche Dienstunterbrechungen oder -probleme.

## <a name="next-steps"></a>Nächste Schritte

* [Erweitern von Azure HDInsight per Azure Virtual Network](../hdinsight-plan-virtual-network-deployment.md)
* [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Verwenden von Azure Storage mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-blob-storage.md)
* [Einrichten von Clustern in HDInsight mit Apache Hadoop, Apache Spark, Apache Kafka usw.](../hdinsight-hadoop-provision-linux-clusters.md)
