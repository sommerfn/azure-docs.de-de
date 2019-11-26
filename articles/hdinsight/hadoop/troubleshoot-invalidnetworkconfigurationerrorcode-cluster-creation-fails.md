---
title: Fehler „InvalidNetworkConfigurationErrorCode“ – Azure HDInsight
description: Verschiedene Gründe für den Fehler „InvalidNetworkConfigurationErrorCode“ bei der Clustererstellung in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: 5b8d031af9dbe6019d71e2a1caa3d3f25d4024ea
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044466"
---
# <a name="cluster-creation-fails-with-invalidnetworkconfigurationerrorcode-in-azure-hdinsight"></a>Fehler „InvalidNetworkConfigurationErrorCode“ bei der Clustererstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

Wenn der Fehlercode `InvalidNetworkConfigurationErrorCode` mit der Beschreibung „Die Konfiguration des virtuellen Netzwerks ist nicht mit der HDInsight-Anforderung kompatibel“ angezeigt wird, ist dies normalerweise ein Hinweis auf ein Problem mit der [Konfiguration des virtuellen Netzwerks](../hdinsight-plan-virtual-network-deployment.md) für Ihren Cluster. Führen Sie basierend auf der restlichen Fehlerbeschreibung die Schritte in den unten angegebenen Abschnitten aus.

## <a name="hostname-resolution-failed"></a>„Fehler beim Auflösen des Hostnamens“

### <a name="issue"></a>Problem

Fehlerbeschreibung enthält „Fehler beim Auflösen des Hostnamens“.

### <a name="cause"></a>Ursache

Dieser Fehler ist ein Hinweis auf ein Problem mit der benutzerdefinierten DNS-Konfiguration. DNS-Server innerhalb eines virtuellen Netzwerks können DNS-Abfragen an die rekursiven Resolver von Azure weiterleiten, um Hostnamen innerhalb dieses virtuellen Netzwerks aufzulösen (ausführlichere Informationen unter [Namensauflösung für Ressourcen in virtuellen Azure-Netzwerken](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)). Der Zugriff auf die rekursiven Resolver von Azure wird über die virtuelle IP 168.63.129.16 bereitgestellt. Auf diese IP kann nur von den Azure-VMs aus zugegriffen werden. Es funktioniert also nicht, wenn Sie einen lokalen DNS-Server nutzen oder Ihr DNS-Server eine Azure-VM ist, die nicht Teil des VNET des Clusters ist.

### <a name="resolution"></a>Lösung

1. Stellen Sie eine SSH-Verbindung mit der VM her, die Teil des Clusters ist, und führen Sie den Befehl `hostname -f` aus. Der vollqualifizierte Domänenname des Hosts (in der Anleitung unten als `<host_fqdn>` bezeichnet) wird zurückgegeben.

1. Führen Sie anschließend den Befehl `nslookup <host_fqdn>` aus (z. B. `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net`). Wenn dieser Befehl den Namen in eine IP-Adresse auflöst, bedeutet dies, dass Ihr DNS-Server richtig funktioniert. Erstellen Sie in diesem Fall eine Supportanfrage für HDInsight, damit wir das Problem untersuchen können. Fügen Sie der Supportanfrage die Schritte zur Problembehandlung hinzu, die Sie ausgeführt haben. Wir können das Problem dann schneller beheben.

1. Führen Sie `nslookup <host_fqdn> 168.63.129.16` aus, falls der obige Befehl keine IP-Adresse zurückgibt (z. B. `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net 168.63.129.16`). Wenn dieser Befehl die IP-Adresse auflösen kann, bedeutet dies Folgendes: Entweder leitet Ihr DNS-Server die Abfrage nicht an das DNS von Azure weiter, oder es handelt sich nicht um eine VM, die in demselben VNET wie der Cluster enthalten ist.

1. Falls Sie nicht über eine Azure-VM verfügen, die als benutzerdefinierter DNS-Server im VNET des Clusters fungieren kann, müssen Sie diese zuerst hinzufügen. Erstellen Sie eine VM im VNET, die als DNS-Weiterleitung konfiguriert wird.

1. Nachdem Sie einen virtuellen Computer in Ihrem VNET bereitgestellt haben, können Sie die Regeln für die DNS-Weiterleitung auf diesem virtuellen Computer konfigurieren. Leiten Sie alle Anforderungen für die iDNS-Namensauflösung an 168.63.129.16 und den Rest an Ihren DNS-Server weiter. [Hier](../hdinsight-plan-virtual-network-deployment.md) ist ein Beispiel für diese Einrichtung für einen benutzerdefinierten DNS-Server angegeben.

1. Fügen Sie die IP-Adresse dieser VM als ersten DNS-Eintrag für die DNS-Konfiguration des virtuellen Netzwerks hinzu.

---

## <a name="failed-to-connect-to-azure-storage-account"></a>„Fehler beim Herstellen der Verbindung mit dem Azure Storage-Konto“

### <a name="issue"></a>Problem

Fehlerbeschreibung enthält: „Fehler beim Herstellen der Verbindung mit dem Azure Storage-Konto“ oder „Fehler beim Herstellen einer Verbindung mit Azure SQL“.

### <a name="cause"></a>Ursache

Da Azure Storage und SQL nicht über feste IP-Adressen verfügen, müssen wir ausgehende Verbindungen zu allen IPs zulassen, um den Zugriff auf diese Dienste zu ermöglichen. Die genauen Lösungsschritte sind davon abhängig, ob Sie eine Netzwerksicherheitsgruppe (NSG) oder benutzerdefinierte Regeln (UDRs) eingerichtet haben. Details zu diesen Konfigurationen finden Sie im Abschnitt zum Thema [Steuern des Netzwerkdatenverkehrs über HDInsight mit Netzwerksicherheitsgruppen und benutzerdefinierten Routen](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip).

### <a name="resolution"></a>Lösung

* Bei Verwendung einer [Netzwerksicherheitsgruppe (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md) in Ihrem Cluster:

    Navigieren Sie zum Azure-Portal, und ermitteln Sie die NSG des Subnetzes, in dem der Cluster bereitgestellt wird. Lassen Sie im Abschnitt **Ausgangssicherheitsregeln** den Zugriff auf das Internet in ausgehender Richtung ohne Einschränkungen zu. (Beachten Sie, dass ein niedrigerer Wert unter **Priorität** hier für eine höhere Priorität steht.) Vergewissern Sie sich außerdem im Abschnitt **Subnetze**, ob diese NSG auf das Clustersubnetz angewendet wird.

* Bei Verwendung einer [benutzerdefinierten Route (UDR)](../../virtual-network/virtual-networks-udr-overview.md) in Ihrem Cluster:

    Navigieren Sie zum Azure-Portal, und ermitteln Sie die Routingtabelle des Subnetzes, in dem der Cluster bereitgestellt wird. Untersuchen Sie nach der Ermittlung der Routingtabelle für das Subnetz den darin enthaltenen Abschnitt **Routen**.

    Wenn Routen definiert sind, sollten Sie sicherstellen, dass Routen für IP-Adressen für die Region vorhanden sind, in der der Cluster bereitgestellt wurde, und dass **NextHopType** für jede Route auf **Internet** festgelegt ist. Für jede erforderliche IP-Adresse, die im obigen Artikel dokumentiert ist, muss eine Route definiert sein.

---

### <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
