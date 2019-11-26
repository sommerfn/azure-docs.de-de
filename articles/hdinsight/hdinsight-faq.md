---
title: Azure HDInsight – Häufig gestellte Fragen
description: Häufig gestellte Fragen zu HDInsight
keywords: Häufig gestellte Fragen, FAQ
author: Ramakoni1
ms.author: ramakoni
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 044a63274f7f24831b1f791982f36898199616a6
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052508"
---
# <a name="azure-hdinsight-frequently-asked-questions"></a>Azure HDInsight: Häufig gestellte Fragen

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="creating-or-deleting-hdinsight-clusters"></a>Erstellen oder Löschen von HDInsight-Clustern

### <a name="how-do-i-provision-an-hdinsight-cluster"></a>Wie stelle ich einen HDInsight-Cluster bereit?

Die verfügbaren HDInsight-Clustertypen und Bereitstellungsmethoden finden Sie unter [Einrichten von Clustern in HDInsight mit Apache Hadoop, Apache Spark, Apache Kafka usw.](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters)

### <a name="how-do-i-delete-an-existing-hdinsight-cluster"></a>Wie lösche ich einen vorhandenen HDInsight-Cluster?

Weitere Informationen zum Löschen eines Clusters, der nicht mehr verwendet wird, finden Sie unter [Löschen eines HDInsight-Clusters](hdinsight-delete-cluster.md).

Warten Sie zwischen Erstell- und Löschvorgängen mindestens 30 bis 60 Minuten. Andernfalls tritt bei dem Vorgang unter Umständen ein Fehler mit der folgenden Fehlermeldung auf:

``Conflict (HTTP Status Code: 409) error when attempting to delete a cluster immediately after creation of a cluster. If you encounter this error, wait until the newly created cluster is in operational state before attempting to delete it.``

### <a name="how-do-i-select-the-correct-number-of-cores-or-nodes-for-my-workload"></a>Wie wähle ich die richtige Anzahl von Kernen oder Knoten für meine Workload aus?

Die geeignete Anzahl von Kernen und weitere Konfigurationsoptionen hängen von verschiedenen Faktoren ab.

Weitere Informationen finden Sie unter [Kapazitätsplanung für HDInsight-Cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning).

### <a name="what-can-i-do-when-cluster-provisioning-fails-because-of-a-capacity-issue"></a>Was kann ich tun, wenn die Clusterbereitstellung aufgrund eines Kapazitätsproblems fehlschlägt?

In diesem Abschnitt werden allgemeine Fehler bei Kapazitätsproblemen behandelt. Außerdem erfahren Sie, wie Sie diese verhindern.

#### <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Fehler Für die Bereitstellung wird die Kontingentgrenze von „800“ überschritten.

Azure verfügt über eine Kontingentgrenze von 800 Bereitstellungen pro Ressourcengruppe. Pro Ressourcengruppe, Abonnement, Konto oder anderen Bereichen werden unterschiedliche Kontingente angewendet. Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr als der zulässigen Anzahl von Kernen bereitzustellen, erhalten Sie eine Fehlermeldung, dass das Kontingent überschritten wurde.

Um dieses Problem zu beheben, löschen Sie die Bereitstellungen, die nicht mehr benötigt werden, über das Azure-Portal, die CLI oder PowerShell.

Weitere Informationen finden Sie unter [Das Beheben von Fehlern bei Ressourcenkontingenten](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

#### <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Fehler Die maximale Knotenzahl hat die Zahl der verfügbaren Kerne in dieser Region überschritten.

Ihr Abonnement kann so konfiguriert werden, dass die Anzahl von Kernen für eine Region begrenzt wird. Wenn Sie versuchen, eine Ressource mit mehr als der zulässigen Anzahl von Kernen bereitzustellen, erhalten Sie eine Fehlermeldung, dass das Kontingent überschritten wurde.

Führen Sie die folgenden Schritte aus, um eine Erhöhung des Kontingents anzufordern:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und wählen Sie **Hilfe und Support** aus.
   
1. Wählen Sie **Neue Supportanfrage** aus.
   
1. Geben Sie auf der Seite **Neue Supportanfrage** auf der Registerkarte **Grundlagen** die folgenden Informationen an:
   
   - **Problemtyp:** Wählen Sie **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
   - **Abonnement:** Wählen Sie das Abonnement aus, das Sie ändern möchten.
   - **Kontingenttyp:** Wählen Sie **HDInsight** aus.

Weitere Informationen finden Sie unter [Kontingente](hdinsight-capacity-planning.md#quotas).

### <a name="what-are-the-various-types-of-nodes-in-an-hdinsight-cluster"></a>Welche unterschiedlichen Knotentypen gibt es in einem HDInsight-Cluster?

In Azure HDInsight-Clustern gibt es unterschiedliche Typen virtueller Computer bzw. Knoten. Jeder Knotentyp spielt eine Rolle beim Betrieb des Systems.

Weitere Informationen finden Sie unter [Ressourcentypen in Azure HDInsight-Clustern](hdinsight-virtual-network-architecture.md#resource-types-in-azure-hdinsight-clusters).

## <a name="individual-components"></a>Einzelne Komponenten

### <a name="can-i-install-additional-components-on-my-cluster"></a>Kann ich zusätzliche Komponenten in meinem Cluster installieren?

Ja. Zum Installieren zusätzlicher Komponenten oder Anpassen der Clusterkonfiguration können Sie Folgendes verwenden:

- Skripts während oder nach der Erstellung. Skripts werden mithilfe der Konfigurationsoption [Skriptaktion](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) aufgerufen, die im Azure-Portal, in HDInsight Windows PowerShell-Cmdlets oder mit dem HDInsight .NET SDK verwendet werden kann. Diese Konfigurationsoption kann im Azure-Portal, in HDInsight Windows PowerShell-Cmdlets oder mit dem HDInsight .NET SDK verwendet werden.

- `sudo` oder andere Methoden nach der Bereitstellung des Clusters.
  
- [HDInsight-Anwendungsplattform](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) zur Installation von Ökosystemanwendungen.

Die Supportteams von Microsoft können jedoch nur Unterstützung für die folgenden Situationen bieten:

- Probleme oder Fehler, die beim Laden des Skripts auftreten. Fehler während der Ausführung von benutzerdefinierten Skripts können nicht über Supporttickets behoben werden.

- Zusätzliche Anwendungen, die Teil des Clustererstellungsprozesses sind. 

Eine Liste der unterstützten Komponenten finden Sie unter [Verfügbare Apache Hadoop-Komponenten in verschiedenen Versionen von HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).

Die Unterstützung für einzelne Komponenten kann auch je nach Clustertyp variieren. Beispielsweise wird Spark in einem Kafka-Cluster nicht unterstützt (und umgekehrt).

Wenden Sie sich bei Anwendungen und Diensten außerhalb des Clustererstellungsprozesses an den jeweiligen Anbieter oder Dienstanbieter, um Unterstützung zu erhalten. Außerdem können Sie zahlreiche Community-Supportwebsites nutzen. Beispiele sind das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) und [Stack Overflow](https://stackoverflow.com/). Für Apache-Projekte stehen auf der [Apache-Website](https://apache.org/) auch Projektwebsites zur Verfügung. Ein Beispiel hierfür ist [Hadoop](https://hadoop.apache.org/). 

Weitere Informationen zum Azure-Support finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).

### <a name="can-i-upgrade-the-individual-components-that-are-pre-installed-on-the-cluster"></a>Kann ich die einzelnen Komponenten aktualisieren, die im Cluster vorinstalliert sind?

Wenn Sie integrierte Komponenten oder Anwendungen aktualisieren, die im Cluster vorinstalliert sind, wird die resultierende Konfiguration nicht von Microsoft unterstützt. Diese Systemkonfigurationen wurden nicht von Microsoft getestet. Versuchen Sie, eine andere Version des HDInsight-Clusters zu verwenden, in der die aktualisierte Version der Komponente möglicherweise bereits vorinstalliert ist.

Das Aktualisieren von Hive als einzelne Komponente wird beispielsweise nicht unterstützt. HDInsight ist ein verwalteter Dienst, und viele Dienste sind in den Ambari-Server integriert und wurden getestet. Wenn Hive eigenständig aktualisiert wird, werden die indizierten Binärdateien anderer Komponenten geändert. Das führt zu Integrationsproblemen mit Komponenten in Ihrem Cluster.

### <a name="can-spark-and-kafka-run-on-the-same-hdinsight-cluster"></a>Können Spark und Kafka im selben HDInsight-Cluster ausgeführt werden?

Nein, Apache Kafka und Apache Spark können nicht im selben HDInsight-Cluster ausgeführt werden. Erstellen Sie separate Cluster für Kafka und Spark, um Probleme aufgrund von Ressourcenkonflikten zu vermeiden.

### <a name="how-do-i-change-timezone-in-ambari"></a>Wie ändere ich die Zeitzone in Ambari?

1. Öffnen Sie die Ambari-Webbenutzeroberfläche unter https://CLUSTERNAME.azurehdinsight.net. Dabei entspricht CLUSTERNAME dem Namen Ihres Clusters.
2. Wählen Sie oben rechts „Admin (Administrator) | Settings (Einstellungen)“ aus. 

   ![Ambari-Einstellungen](media/hdinsight-faq/ambari-settings.png)

3. Wählen Sie im Fenster „User Settings“ (Benutzereinstellungen) die neue Zeitzone aus der Dropdownliste „Timezone“ (Zeitzone) aus, und klicken Sie auf „Save“ (Speichern).

   ![Ambari-Benutzereinstellungen](media/hdinsight-faq/ambari-user-settings.png)

## <a name="metastore"></a>Metastore

### <a name="how-can-i-migrate-from-the-existing-metastore-to-azure-sql-server"></a>Wie kann ich vom vorhandenen Metastore zu Azure SQL Server migrieren? 

Informationen zum Migrieren von SQL Server zu Azure SQL Server finden Sie unter [Tutorial: Offlinemigration von SQL Server zu einer Einzel- oder Pooldatenbank in Azure SQL-Datenbank mit DMS](../dms/tutorial-sql-server-to-azure-sql.md).

### <a name="is-the-hive-metastore-deleted-when-the-cluster-is-deleted"></a>Wird der Hive-Metastore zusammen mit dem Cluster gelöscht?

Dies hängt vom Typ des Metastores ab, der für den Cluster konfiguriert ist.

Standardmetastore: Der Standardmetastore ist Teil des Clusterlebenszyklus. Wenn Sie einen Cluster löschen, werden der entsprechende Metastore und die jeweiligen Metadaten ebenfalls gelöscht.

Benutzerdefinierter Metastore: Der Lebenszyklus des Metastores ist nicht an den Lebenszyklus eines Clusters gebunden. Daher können Sie Cluster erstellen und löschen, ohne Metadaten zu verlieren. Metadaten, wie z. B. Ihre Hive-Schemas, bleiben auch erhalten, nachdem der HDInsight-Cluster gelöscht und neu erstellt wurde.

Weitere Informationen finden Sie unter [Verwenden von externen Metadatenspeichern in Azure HDInsight](hdinsight-use-external-metadata-stores.md).

### <a name="does-migrating-a-hive-metastore-also-migrate-the-default-policies-of-the-ranger-database"></a>Werden bei der Migration eines Hive-Metastores auch die Standardrichtlinien der Ranger-Datenbank migriert?

Nein, die Richtliniendefinition ist in der Ranger-Datenbank enthalten. Daher wird die Richtlinie bei der Migration der Ranger-Datenbank migriert.

### <a name="can-you-migrate-a-hive-metastore-from-an-enterprise-security-package-esp-cluster-to-a-non-esp-cluster-and-vice-versa"></a>Kann ich einen Hive-Metastore von einem Cluster mit Enterprise-Sicherheitspaket (ESP) zu einem Cluster ohne ESP migrieren und umgekehrt?

Ja, Sie können eine Hive-Metastore von einem Cluster mit Enterprise-Sicherheitspaket zu einem Cluster ohne ESP migrieren.

### <a name="how-can-i-estimate-the-size-of-a-hive-metastore-database"></a>Wie kann ich die Größe einer Hive-Metastore-Datenbank schätzen?

In einem Hive-Metastore werden die Metadaten für Datenquellen gespeichert, die auf dem Hive-Server verwendet werden. Die Größenanforderungen hängen zum Teil von der Anzahl und der Komplexität der Hive-Datenquellen ab und können nicht vorab geschätzt werden. Wie unter [Bewährte Methoden für den Hive-Metastore](hdinsight-use-external-metadata-stores.md#hive-metastore-best-practices) beschrieben, können Sie mit einem S2-Tarif mit 50 DTUs und 250 GB Speicher beginnen. Wenn Sie einen Engpass feststellen, können Sie die Datenbank zentral hochskalieren.

### <a name="do-you-support-any-other-database-other-than-azure-sql-database-as-an-external-metastore"></a>Werden andere Datenbanken als Azure SQL-Datenbank als externer Metastore unterstützt?

Nein, Microsoft unterstützt nur Azure SQL-Datenbank als externen benutzerdefinierten Metastore.

### <a name="can-i-share-a-metastore-across-multiple-clusters"></a>Kann ich einen Metastore für mehrere Cluster verwenden?

Ja, Sie können einen benutzerdefinierten Metastore für mehrere Cluster verwenden, sofern sie dieselbe HDInsight-Version verwenden.

## <a name="connectivity-and-virtual-networks"></a>Konnektivität und virtuelle Netzwerke  

### <a name="what-are-the-implications-of-blocking-ports-22-and-23-on-my-network"></a>Was geschieht, wenn ich die Ports 22 und 23 in meinem Netzwerk blockiere?

Wenn Sie die Ports 22 und 23 blockieren, haben Sie keinen SSH-Zugriff auf den Cluster. Diese Ports werden vom HDInsight-Dienst nicht genutzt.

Weitere Informationen finden Sie in den folgenden Dokumenten:

- [Steuern des Netzwerkdatenverkehrs](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic)

- [Sichern des eingehenden Datenverkehrs für HDInsight-Cluster in einem virtuellen Netzwerk mit privatem Endpunkt](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)

- [HDInsight-Verwaltungs-IP-Adressen](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)

### <a name="can-i-deploy-an-additional-virtual-machine-within-the-same-subnet-as-an-hdinsight-cluster"></a>Kann ich einen zusätzlichen virtuellen Computer im selben Subnetz wie einen HDInsight-Cluster bereitstellen?

Ja, Sie können einen zusätzlichen virtuellen Computer im selben Subnetz wie einen HDInsight-Cluster bereitstellen. Die folgenden Konfigurationen sind möglich:

- Edgeknoten: Sie können dem Cluster einen weiteren Edgeknoten hinzufügen, wie unter [Verwenden leerer Edgeknoten in Apache Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md) beschrieben.

- Eigenständige Knoten:  Sie können demselben Subnetz einen eigenständigen virtuellen Computer hinzufügen und über diesen virtuellen Computer auf den Cluster zugreifen, indem Sie den privaten Endpunkt `https://<CLUSTERNAME>-int.azurehdinsight.net` verwenden. Weitere Informationen finden Sie unter [Steuern des Netzwerkdatenverkehrs](hdinsight-plan-virtual-network-deployment.md#networktraffic).

### <a name="can-i-add-an-existing-hdinsight-cluster-to-another-virtual-network"></a>Kann ich einen vorhandenen HDInsight-Cluster einem anderen virtuellen Netzwerk hinzufügen?

Nein, das ist nicht möglich. Das virtuelle Netzwerk sollte zum Zeitpunkt der Bereitstellung angegeben werden. Wenn während der Bereitstellung kein virtuelles Netzwerk angegeben wird, wird ein internes Netzwerk erstellt, auf das ein externer Zugriff nicht möglich ist. Weitere Informationen finden Sie unter [Hinzufügen von HDInsight zu einem vorhandenen virtuellen Netzwerk](hdinsight-plan-virtual-network-deployment.md#existingvnet).

## <a name="security-and-certificates"></a>Sicherheit und Zertifikate

### <a name="what-are-the-recommendations-for-malware-protection-on-azure-hdinsight-clusters"></a>Welche Empfehlungen gibt es für den Schutz vor Schadsoftware in Azure HDInsight-Clustern?

Informationen zum Schutz vor Schadsoftware finden Sie unter [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../security/fundamentals/antimalware.md).

### <a name="how-do-i-create-a-keytab-for-an-hdinsight-esp-cluster"></a>Wie erstelle ich eine Keytab-Datei für einen HDInsight-Cluster mit Enterprise-Sicherheitspaket?

Erstellen Sie eine Kerberos-Keytab-Datei für Ihren Domänenbenutzernamen. Sie können diese Keytab-Datei später für die kennwortlose Authentifizierung bei Remoteclustern verwenden, die in die Domäne eingebunden sind. Der Domänenname wird in Großbuchstaben angegeben:

```shell
ktutil
ktutil: addent -password -p <username>@<DOMAIN.COM> -k 1 -e RC4-HMAC
Password for <username>@<DOMAIN.COM>: <password>
ktutil: wkt <username>.keytab
ktutil: q
```

### <a name="can-i-use-an-existing-azure-active-directory-tenant-to-create-an-hdinsight-cluster-that-has-the-esp"></a>Kann ich einen vorhandenen Azure Active Directory-Mandanten verwenden, um einen HDInsight-Cluster mit Enterprise-Sicherheitspaket zu erstellen?

Sie müssen Azure Active Directory Domain Services (Azure AD DS) aktivieren, damit Sie einen HDInsight-Cluster mit Enterprise-Sicherheitspaket erstellen können. Open-Source-Hadoop nutzt für die Authentifizierung Kerberos (und nicht OAuth).

Um virtuelle Computer in eine Domäne einzubinden, ist ein Domänencontroller erforderlich. Der verwaltete Domänencontroller entspricht Azure AD DS. Er wird als Erweiterung von Azure Active Directory betrachtet, die alle Kerberos-Anforderungen zum Erstellen eines sicheren Hadoop-Clusters in einer verwalteten Umgebung erfüllt. HDInsight wird als verwalteter Dienst in Azure AD DS integriert, um End-to-End-Sicherheit zu gewährleisten.

### <a name="can-i-use-a-self-signed-certificate-in-an-aad-ds-secure-ldap-setup-and-provision-an-esp-cluster"></a>Kann ich in einem AAD DS-Setup mit Secure LDAP ein selbstsigniertes Zertifikat verwenden und einen Cluster mit Enterprise-Sicherheitspaket bereitstellen?

Es wird empfohlen, ein von einer Zertifizierungsstelle ausgestelltes Zertifikat zu verwenden. Jedoch werden bei Verwendung eines Enterprise-Sicherheitspakets auch selbstsignierte Zertifikate unterstützt. Weitere Informationen finden Sie unter

- [Aktivieren von Azure Active Directory Domain Services](domain-joined/apache-domain-joined-configure-using-azure-adds.md#enable-azure-ad-ds)

- [Tutorial: Konfigurieren von Secure LDAP (LDAPS) für eine verwaltete Azure AD Domain Services-Domäne](../active-directory-domain-services/tutorial-configure-ldaps.md)

### <a name="how-can-i-pull-login-activity-shown-in-ranger"></a>Wie kann ich in Ranger angezeigte Anmeldeaktivitäten abrufen?

Microsoft empfiehlt, Azure Monitor-Protokolle zu aktivieren, wie in [Verwenden von Azure Monitor-Protokollen zum Überwachen von HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial) beschrieben, um Überwachungsanforderungen zu erfüllen.

### <a name="can-i-disable-clamscan-on-my-cluster"></a>Kann ich Clamscan in meinem Cluster deaktivieren?

Clamscan ist die im HDInsight-Cluster ausgeführte Antivirensoftware. Sie wird vom Azure-Sicherheitsprozess (azsecd) verwendet, um Ihre Cluster vor Virenangriffen zu schützen. Microsoft rät dringend davon ab, dass Benutzer Änderungen an der Standardkonfiguration von Clamscan vornehmen.

Durch diesen Prozess werden weder andere Prozesse beeinträchtigt noch CPU-Zyklen anderer Prozesse verbraucht. Der andere Prozess hat immer Vorrang. Durch Clamscan verursachte CPU-Spitzen sollten sich nur beobachten lassen, wenn sich das System im Leerlauf befindet.  

Für Szenarien, die eine zeitliche Kontrolle erfordern, können Sie die folgenden Schritte ausführen:

1. Deaktivieren Sie die automatische Ausführung mit dem folgenden Befehl:
   
   `/usr/local/vbin/azsecd config -s clamav -d Disabled`
   
1. Fügen Sie einen Cron-Auftrag hinzu, der folgenden Befehl als Root ausführt:
   
   `/usr/local/bin/azsecd manual -s clamav`

Weitere Informationen zum Einrichten und Ausführen eines Cron-Auftrags finden Sie im Thema zum [Einrichten eines Cron-Auftrags](https://askubuntu.com/questions/2368/how-do-i-set-up-a-cron-job).

### <a name="why-is-llap-available-on-spark-esp-clusters"></a>Weshalb ist LLAP in Spark-ESP-Clustern verfügbar?
LLAP ist in Spark-ESP-Clustern aus Sicherheitsgründen (d. h. Apache Ranger) und nicht aus Leistungsgründen aktiviert. Sie sollten virtuelle Computer mit einer größeren Knoten verwenden, um die Ressourcenverwendung von LLAP zu berücksichtigen (z. B. mindestens D13V2). 

### <a name="how-can-i-add-addional-aad-groups-after-creating-an-esp-cluster"></a>Wie kann ich nach dem Erstellen eines ESP-Clusters zusätzliche AAD-Gruppen hinzufügen?
Es gibt zwei Möglichkeiten, dies zu erreichen: 1. Sie können den Cluster neu erstellen und die zusätzliche Gruppe bei der Clustererstellung hinzufügen. Wenn Sie die bereichsbezogene Synchronisierung in AAD DS verwenden, stellen Sie sicher, dass Gruppe B in der bereichsbezogenen Synchronisierung enthalten ist.
2\. Fügen Sie die Gruppe als geschachtelte Untergruppe der vorherigen Gruppe hinzu, die zum Erstellen des ESP-Clusters verwendet wurde. Wenn Sie beispielsweise einen ESP-Cluster mit der Gruppe `A` erstellt haben, können Sie zu einem späteren Zeitpunkt die Gruppe `B` als geschachtelte Untergruppe von `A` hinzufügen. Nach ungefähr einer Stunde wird diese synchronisiert und ist im Cluster automatisch verfügbar. 

## <a name="storage"></a>Storage

### <a name="can-i-add-an-azure-data-lake-storage-gen2-to-an-existing-hdinsight-cluster-as-an-additional-storage-account"></a>Kann ich einem vorhandenen HDInsight-Cluster Azure Data Lake Storage Gen2 als zusätzliches Speicherkonto hinzufügen?

Nein, derzeit ist es nicht möglich, einem Cluster, der Blob Storage als primären Speicher nutzt, ein Azure Data Lake Storage Gen2-Speicherkonto hinzuzufügen. Weitere Informationen finden Sie unter [Vergleichen von Speicheroptionen](hdinsight-hadoop-compare-storage-options.md).

### <a name="how-can-i-find-the-currently-linked-service-principal-for-a-data-lake-storage-account"></a>Wie finde ich den Dienstprinzipal für ein Data Lake-Speicherkonto, der aktuell verknüpft ist?

Sie finden Ihre Einstellungen im Azure-Portal in den Clustereigenschaften unter **Data Lake Storage Gen1-Zugriff**. Weitere Informationen finden Sie unter [Überprüfen des Clustersetups](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#verify-cluster-set-up).
 
### <a name="how-can-i-calculate-the-usage-of-storage-accounts-and-blob-containers-for-my-hdinsight-clusters"></a>Wie berechne ich die Verwendung von Speicherkonten und BLOB-Containern für meine HDInsight-Cluster?

Führen Sie einen der folgenden Schritte aus:

- [Verwenden von PowerShell](../storage/scripts/storage-blobs-container-calculate-size-powershell.md)

- Ermitteln Sie die Größe des Ordners */user/hive/.Trash/* im HDInsight-Cluster über die folgende Befehlszeile:
  
  `hdfs dfs -du -h /user/hive/.Trash/`

### <a name="how-can-i-set-up-auditing-for-my-blob-storage-account"></a>Wie richte ich die Überwachung für mein BLOB-Speicherkonto ein?

Um Blobspeicherkonten zu überwachen, konfigurieren Sie die Überwachung entsprechend den unter [Überwachen eines Speicherkontos im Azure-Portal](../storage/common/storage-monitor-storage-account.md) beschriebenen Schritten. Ein HDFS-Überwachungsprotokoll enthält nur Überwachungsinformationen für das lokale HDFS-Dateisystem (hdfs://mycluster).  Im Remotespeicher ausgeführte Vorgänge werden nicht berücksichtigt.

### <a name="how-can-i-transfer-files-between-a-blob-container-and-an-hdinsight-head-node"></a>Wie übertrage ich Dateien zwischen einem BLOB-Container und einem HDInsight-Hauptknoten?

Führen Sie ein mit dem folgenden Shellskript vergleichbares Skript auf dem Hauptknoten aus:

```shell
for i in cat filenames.txt
do
   hadoop fs -get $i <local destination>
done
```
 
> [!NOTE]
> Die Datei *filenames.txt* enthält den absoluten Pfad der Dateien in den BLOB-Containern.
 
### <a name="are-there-any-ranger-plugins-for-storage"></a>Gibt es Ranger-Plug-Ins für den Speicher?

Derzeit sind keine Ranger-Plug-Ins für Blob Storage und Azure Data Lake Storage Gen1 oder Gen2 verfügbar. Für Cluster mit Enterprise-Sicherheitspaket empfiehlt sich die Verwendung von Azure Data Lake Storage, da Sie mithilfe der HDFS-Tools zumindest differenzierte Berechtigungen auf Dateisystemebene manuell festlegen können. Darüber hinaus wird bei Verwendung von Azure Data Lake Storage der Dateisystemzugriff bei Clustern mit Enterprise-Sicherheitspaket teilweise auch über Azure Active Directory auf Clusterebene gesteuert. 

Über Azure Storage-Explorer können Sie den Sicherheitsgruppen Ihrer Benutzer Datenzugriffsrichtlinien zuweisen. Weitere Informationen finden Sie unter

- [Wie richte ich Berechtigungen für Azure AD-Benutzer zum Abfragen von Daten in Data Lake Storage Gen2 mit Hive oder anderen Diensten ein?](hdinsight-hadoop-use-data-lake-storage-gen2.md#how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services)
- [Festlegen von Berechtigungen auf Datei- und Verzeichnisebene mithilfe des Azure Storage-Explorers mit Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

### <a name="can-i-increase-hdfs-storage-on-a-cluster-without-increasing-the-disk-size-of-worker-nodes"></a>Kann ich den HDFS-Speicher in einem Cluster vergrößern, ohne dass die Datenträgergröße von Workerknoten zunimmt?

Nein, es ist nicht möglich, die Datenträgergröße eines Workerknotens zu erhöhen. Die einzige Möglichkeit, den Datenträger zu vergrößern, besteht darin, den Cluster zu löschen und auf größeren virtuellen Workercomputern neu zu erstellen. Verwenden Sie HDFS nicht zum Speichern von HDInsight-Daten, da beim Löschen des Clusters auch die Daten gelöscht werden. Speichern Sie die Daten stattdessen in Azure. Sie können die Kapazität des HDInsight-Clusters auch erweitern, indem Sie den Cluster zentral hochskalieren.

## <a name="edge-nodes"></a>Edgeknoten

### <a name="can-i-add-an-edge-node-after-the-cluster-has-been-created"></a>Kann ich nach der Erstellung des Clusters einen Edgeknoten hinzufügen?

Sie können einen vorhandenen HDInsight-Cluster oder einen neuen Cluster bei der Erstellung verwenden. Weitere Informationen finden Sie unter [Verwenden leerer Edgeknoten in Apache Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md).

### <a name="how-can-i-connect-to-an-edge-node"></a>Wie stelle ich eine Verbindung mit einem Edgeknoten her?

Nachdem Sie einen Edgeknoten erstellt haben, können Sie über SSH an Port 22 eine Verbindung mit dem Edgeknoten herstellen. Den Namen des Edgeknotens finden Sie im Clusterportal. Die Namen enden im Allgemeinen auf *-ed*.

### <a name="why-are-persisted-scripts-not-running-automatically-on-newly-created-edge-nodes"></a>Warum werden permanente Skripts auf neu erstellten Edgeknoten nicht automatisch ausgeführt?

Sie verwenden permanente Skripts, um mithilfe von Skalierungsvorgängen neue Workerknoten anzupassen, die dem Cluster hinzugefügt wurden. Persistente Skripts gelten nicht für Edgeknoten.

## <a name="rest-api"></a>REST-API

### <a name="what-are-the-rest-api-calls-to-pull-a-tez-query-view-from-the-cluster"></a>Wie lauten die REST-API-Aufrufe zum Abrufen einer Tez Query-Ansicht aus dem Cluster?

Sie können die folgenden REST-Endpunkte verwenden, um die erforderlichen Informationen im JSON-Format abzurufen. Verwenden Sie standardmäßige Authentifizierungsheader, um diese Anforderungen auszuführen.

- Tez Query-Ansicht: *https:\//\<Clustername>.azurehdinsight.net/ws/v1/timeline/HIVE_QUERY_ID/*
- Tez Dag-Ansicht: *https:\//\<Clustername>.azurehdinsight.net/ws/v1/timeline/TEZ_DAG_ID/*

### <a name="how-do-i-retrieve-the-configuration-details-from-hdi-cluster-by-using-an-azure-active-directory-user"></a>Wie rufe ich die Konfigurationsdetails aus dem HDI-Cluster mithilfe eines Azure Active Directory-Benutzers ab?

Um mit dem AAD-Benutzer korrekte Authentifizierungstoken auszuhandeln, verwenden Sie das folgende Format, um das Gateway zu passieren:

* https://`<cluster dnsname>`.azurehdinsight.net/api/v1/clusters/testclusterdem/stack_versions/1/repository_versions/1 

### <a name="how-do-i-use-ambari-restful-api-to-monitor-yarn-performance"></a>Wie verwende ich die RESTful-API von Ambari, um die YARN-Leistung zu überwachen?

Wenn Sie den cURL-Befehl im selben virtuellen Netzwerk oder in einem virtuellen Netzwerk mit Peering aufrufen, verwenden Sie folgenden Befehl:

```curl
curl -u <cluster login username> -sS -G
http://<headnodehost>:8080/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
```
 
Wenn Sie den Befehl außerhalb des virtuellen Netzwerks oder in einem virtuellen Netzwerk ohne Peering aufrufen, lautet das Befehlsformat wie folgt:

- Cluster ohne Enterprise-Sicherheitspaket:
  
  ```curl
  curl -u <cluster login username> -sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

- Cluster mit Enterprise-Sicherheitspaket:
  
  ```curl
  curl -u <cluster login username>-sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

> [!NOTE]
> Von Curl wird ein Kennwort angefordert. Sie müssen ein gültiges Kennwort für den Benutzernamen für die Clusteranmeldung eingeben.

## <a name="billing"></a>Abrechnung

### <a name="how-much-does-it-cost-to-deploy-an-hdinsight-cluster"></a>Wie viel kostet die Bereitstellung eines HDInsight-Clusters?

Weitere Informationen zu Preisen und häufig gestellte Fragen zur Abrechnung finden Sie auf der Seite [Azure HDInsight: Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

### <a name="when-does-hdinsight-billing-start--stop"></a>Wann beginnt und endet die HDInsight-Abrechnung?

Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Abrechnung erfolgt pro Minute.

### <a name="how-do-i-cancel-my-subscription"></a>Wie kann ich mein Abonnement kündigen?

Informationen zum Kündigen Ihres Abonnements finden Sie unter [Kündigen Ihres Azure-Abonnements](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription).

### <a name="for-pay-as-you-go-subscriptions-what-happens-after-i-cancel-my-subscription"></a>Was geschieht bei Abonnements mit nutzungsbasierter Bezahlung, nachdem ich mein Abonnement gekündigt habe?

Informationen zu Ihrem gekündigten Abonnement finden Sie unter [Was geschieht, nachdem ich mein Abonnement gekündigt habe?](/azure/billing/billing-how-to-cancel-azure-subscription)

## <a name="hive"></a>Hive

### <a name="why-does-the-hive-version-appear-as-121000-instead-of-21-in-the-ambari-ui-even-though-i-am-running-an-hdinsight-36-cluster"></a>Warum ist in der Ambari-Benutzeroberfläche die Hive-Version 1.2.1000 anstatt 2.1 angegeben, obwohl ich einen HDInsight-Cluster der Version 3.6 ausführe?

Obwohl in der Ambari-Benutzeroberfläche nur 1.2 angezeigt wird, umfasst HDInsight 3.6 sowohl Hive 1.2 als auch Hive 2.1.

## <a name="other-faq"></a>Weitere häufig gestellte Fragen

### <a name="what-does-hdinsight-offer-in-terms-of-real-time-stream-processing-capabilities"></a>Welche Funktionen bietet HDInsight für die Streamverarbeitung in Echtzeit?

Informationen zu den Integrationsfunktionen für die Streamverarbeitung in Azure HDInsight finden Sie unter [Auswählen einer Technologie für die Datenstromverarbeitung in Azure](/azure/architecture/data-guide/technology-choices/stream-processing).

### <a name="is-there-a-way-to-dynamically-terminate-the-head-node-of-the-cluster-when-the-cluster-is-idle-for-a-specific-period"></a>Gibt es eine Möglichkeit, den Hauptknoten des Clusters dynamisch zu beenden, wenn sich der Cluster eine bestimmte Zeit im Leerlauf befindet?

Dies ist bei HDInsight-Clustern nicht möglich. Für diese Szenarien können Sie Azure Data Factory verwenden.

### <a name="what-compliance-offerings-does-hdinsight-offer"></a>Welche Complianceangebote umfasst HDInsight?

Informationen zur Compliance finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trust-center) und unter [Overview of Microsoft Azure compliance](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).
