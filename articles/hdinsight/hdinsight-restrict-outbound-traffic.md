---
title: Konfigurieren von Einschränkungen des ausgehenden Netzwerkdatenverkehrs – Azure HDInsight
description: Erfahren Sie, wie Sie Einschränkungen des ausgehenden Netzwerkdatenverkehrs für Azure HDInsight-Cluster konfigurieren.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 8f6959eb6f9d17a368e7df7b95ecc511d0396f87
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73621453"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Konfigurieren des ausgehenden Netzwerkdatenverkehrs für Azure HDInsight-Cluster mittels Firewall

In diesem Artikel werden die Schritte beschrieben, mit denen Sie aus Ihrem HDInsight-Cluster ausgehenden Datenverkehr mithilfe von Azure Firewall schützen. Die folgenden Schritte setzen voraus, dass Sie eine Azure Firewall-Instanz für einen vorhandenen Cluster konfigurieren. Wenn Sie einen neuen Cluster hinter einer Firewall bereitstellen, erstellen Sie zunächst Ihren HDInsight-Cluster und das Subnetz, und führen Sie dann die Schritte in diesem Leitfaden aus.

## <a name="background"></a>Hintergrund

Azure HDInsight-Cluster werden normalerweise in Ihrem eigenen virtuellen Netzwerk bereitgestellt. Der Cluster verfügt über Abhängigkeiten von Diensten außerhalb dieses virtuellen Netzwerks, die erfordern, dass der Netzwerkzugriff ordnungsgemäß funktioniert.

Mehrere Abhängigkeiten erfordern eingehenden Datenverkehr. Der eingehende Verwaltungsdatenverkehr kann nicht über ein Firewallgerät gesendet werden. Die Quelladressen für diesen Datenverkehr sind bekannt und werden [hier](hdinsight-management-ip-addresses.md) veröffentlicht. Sie können auch Netzwerksicherheitsgruppen-Regeln (NSG) mit diesen Informationen erstellen, um bei den Clustern eingehenden Datenverkehr zu sichern.

Die Abhängigkeiten des ausgehenden HDInsight-Datenverkehrs werden fast ausschließlich mit FQDNs definiert, hinter denen sich keine statischen IP-Adressen befinden. Das Fehlen statischer Adressen bedeutet, dass Netzwerksicherheitsgruppen (NSGs) nicht verwendet werden können, um den ausgehenden Datenverkehr eines Clusters zu sperren. Die Adressen ändern sich häufig, sodass keine Regeln auf Grundlage der aktuellen Auflösung aufgestellt und keine NSGs damit eingerichtet werden können.

Die Lösung zum Sichern ausgehender Adressen besteht in der Verwendung eines Firewallgeräts, das den ausgehenden Datenverkehr basierend auf Domänennamen kontrolliert. Azure Firewall kann ausgehenden HTTP- und HTTPS-Datenverkehr basierend auf den FQDN des Ziels oder [FQDN-Tags](https://docs.microsoft.com/azure/firewall/fqdn-tags) beschränken.

## <a name="configuring-azure-firewall-with-hdinsight"></a>Konfigurieren von Azure Firewall mit HDInsight

Hier sehen Sie eine Zusammenfassung der Schritte, um von HDInsight ausgehenden Datenverkehr mit Azure Firewall zu sperren:

1. Erstellen einer Firewall.
1. Hinzufügen von Anwendungsregeln zur Firewall
1. Fügen Sie der Firewall Netzwerkregeln hinzu.
1. Erstellen Sie eine Routingtabelle.

### <a name="create-new-subnet"></a>Erstellen eines neuen Subnetzes

Erstellen Sie ein Subnetz namens **AzureFirewallSubnet** in dem virtuellen Netzwerk, in dem sich Ihr Cluster befindet.

### <a name="create-a-new-firewall-for-your-cluster"></a>Erstellen einer neuen Firewall für Ihren Cluster

Erstellen Sie eine Firewall mit dem Namen **Test-FW01** gemäß den Schritten unter **Bereitstellen der Firewall** in [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Konfigurieren der Firewall mit Anwendungsregeln

Erstellen Sie eine Anwendungsregelsammlung, die dem Cluster ermöglicht, wichtige Informationen zu senden und zu empfangen.

1. Wählen Sie die neue Firewall **Test-FW01** über das Azure-Portal aus.

1. Navigieren Sie zu **Einstellungen** > **Regeln** > **Anwendungsregelsammlung** >  **+ Anwendungsregelsammlung hinzufügen**.

    ![Titel: Hinzufügen einer Anwendungsregelsammlung](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. Geben Sie im Bildschirm **Anwendungsregelsammlung hinzufügen** Folgendes an:

    **Oberer Abschnitt**

    | Eigenschaft|  Wert|
    |---|---|
    |NAME| FwAppRule|
    |Priority|200|
    |Aktion|Allow|

    **Abschnitt „FQDN-Tags“**

    | NAME | Quelladresse | FQDN-Tag | Notizen |
    | --- | --- | --- | --- |
    | Regel 1 | * | „WindowsUpdate“ und „HDInsight“ | Für HDI-Dienste erforderlich |

    **Abschnitt „Ziel-FQDNs“**

    | NAME | Quelladressen | Protokoll:Port | Ziel-FQDNs | Notizen |
    | --- | --- | --- | --- | --- |
    | Regel 2 | * | https:443 | login.windows.net | Lässt Windows Anmeldeaktivität zu |
    | Regel 3 | * | https:443 | login.microsoftonline.com | Lässt Windows Anmeldeaktivität zu |
    | Regel 4 | * | https:443,http:80 | storage_account_name.blob.core.windows.net | Ersetzen Sie `storage_account_name` durch den tatsächlichen Namen Ihres Speicherkontos. Wenn Ihr Cluster WASB nutzt, fügen Sie eine Regel für WASB hinzu. Sollen nur HTTPS-Verbindungen verwendet werden, vergewissern Sie sich, dass im Speicherkonto die Option [Sichere Übertragung erforderlich](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) aktiviert ist. |

   ![Titel: Eingeben der Details der Anwendungsregelsammlung](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. Wählen Sie **Hinzufügen**.

### <a name="configure-the-firewall-with-network-rules"></a>Konfigurieren der Firewall mit Netzwerkregeln

Erstellen Sie die Netzwerkregeln, um Ihren HDInsight-Cluster ordnungsgemäß zu konfigurieren.

1. Navigieren Sie im Anschluss an den vorherigen Schritt zu **Netzwerkregelsammlung** >  **+ Netzwerkregelsammlung hinzufügen**.

1. Geben Sie im Bildschirm **Netzwerkregelsammlung hinzufügen** Folgendes an:

    **Oberer Abschnitt**

    | Eigenschaft|  Wert|
    |---|---|
    |NAME| FwNetRule|
    |Priority|200|
    |Aktion|Allow|

    **Abschnitt „IP-Adressen“**

    | NAME | Protocol | Quelladressen | Zieladressen | Zielports | Notizen |
    | --- | --- | --- | --- | --- | --- |
    | Regel 1 | UDP | * | * | 123 | Zeitdienst |
    | Regel 2 | Any | * | DC_IP_Address_1, DC_IP_Address_2 | * | Wenn Sie das Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) verwenden, fügen Sie im Abschnitt „IP-Adressen“ eine Netzwerkregel hinzu, die die Kommunikation mit AAD-DS für ESP-Cluster ermöglicht. Die IP-Adressen der Domänencontroller finden Sie im Abschnitt „AAD-DS“ im Portal. |
    | Regel 3 | TCP | * | IP-Adresse Ihres Data Lake Storage-Kontos | * | Wenn Sie Azure Data Lake Storage verwenden, können Sie im Abschnitt „IP-Adressen“ eine Netzwerkregel zur Behandlung eines SNI-Problems mit ADLS Gen1 und Gen2 hinzufügen. Diese Option leitet den Datenverkehr an die Firewall weiter, was bei großen Datenmengen zu höheren Kosten führen kann, der Datenverkehr wird jedoch in Firewallprotokollen protokolliert und überwacht. Bestimmen Sie die IP-Adresse für Ihr Data Lake Storage-Konto. Sie können mit einem PowerShell-Befehl wie `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` den FQDN in eine IP-Adresse auflösen.|
    | Regel 4 | TCP | * | * | 12000 | (Optional) Wenn Sie Log Analytics verwenden, erstellen Sie im Abschnitt „IP-Adressen“ eine Netzwerkregel, um die Kommunikation mit Ihrem Log Analytics-Arbeitsbereich zu ermöglichen. |

    **Abschnitt „Diensttags“**

    | NAME | Protocol | Quelladressen | Diensttags | Zielports | Notizen |
    | --- | --- | --- | --- | --- | --- |
    | Regel 7 | TCP | * | SQL | 1433 | Konfigurieren Sie im Abschnitt „Diensttags“ eine Regel für SQL, die es Ihnen ermöglicht, SQL-Datenverkehr zu protokollieren und zu überwachen, sofern Sie im HDInsight-Subnetz keine Dienstendpunkte für SQL Server konfiguriert haben, wodurch die Firewall umgangen wird. |

   ![Titel: Eingeben einer Anwendungsregelsammlung](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. Wählen Sie **Hinzufügen**.

### <a name="create-and-configure-a-route-table"></a>Erstellen und Konfigurieren einer Routingtabelle

Erstellen Sie eine Routingtabelle mit den folgenden Einträgen:

* Alle IP-Adressen aus der Tabelle [Integritäts- und Verwaltungsdienste: Alle Regionen](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) mit **Internet** als Typ des nächsten Hops.

* Zwei IP-Adressen für die Region, in der der Cluster erstellt wird, aus der Tabelle [Integritäts- und Verwaltungsdienste: Bestimmte Regionen](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) mit **Internet** als Typ des nächsten Hops.

* Eine Route für ein virtuelles Gerät für IP-Adresse 0.0.0.0/0, wobei der nächste Hop Ihre private Azure Firewall-IP-Adresse ist.

Gehen Sie beispielsweise wie folgt vor, um die Routingtabelle für einen in der US-Region „USA, Osten“ erstellten Cluster zu konfigurieren:

1. Wählen Sie Ihre Azure Firewall-Instanz **Test-FW01** aus. Kopieren Sie die auf dem Blatt **Übersicht** aufgelistete **Private IP-Adresse**. In diesem Beispiel verwenden wir die **Beispieladresse 10.0.2.4**.

1. Navigieren Sie anschließend zu **Alle Dienste** > **Netzwerk** > **Routingtabellen**, und wählen Sie **Routingtabelle erstellen** aus.

1. Navigieren Sie unter Ihrer neuen Route zu **Einstellungen** > **Routen** >  **+ Hinzufügen**. Fügen Sie folgende Routen hinzu:

| Routenname | Adresspräfix | Typ des nächsten Hops | Adresse des nächsten Hops |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | Nicht verfügbar |
| 23.99.5.239 | 23.99.5.239/32 | Internet | Nicht verfügbar |
| 168.61.48.131 | 168.61.48.131/32 | Internet | Nicht verfügbar |
| 138.91.141.162 | 138.91.141.162/32 | Internet | Nicht verfügbar |
| 13.82.225.233 | 13.82.225.233/32 | Internet | Nicht verfügbar |
| 40.71.175.99 | 40.71.175.99/32 | Internet | Nicht verfügbar |
| 0.0.0.0 | 0.0.0.0/0 | Virtuelles Gerät | 10.0.2.4 |

Schließen Sie die Konfiguration der Routingtabelle ab:

1. Weisen Sie die erstellte Routingtabelle durch Auswählen von **Subnetze** (unter **Einstellungen**) Ihrem HDInsight-Subnetz zu.

1. Wählen Sie **+ Zuordnen** aus.

1. Wählen Sie im Bildschirm **Subnetz zuordnen** das virtuelle Netzwerk, in dem Ihr Cluster erstellt wurde, und das **Subnetz** aus, das Sie für Ihren HDInsight-Cluster verwendet haben.

1. Klicken Sie auf **OK**.

## <a name="edge-node-or-custom-application-traffic"></a>Datenverkehr für Edgeknoten- oder benutzerdefinierte Anwendung

Mithilfe der oben genannten Schritte kann der Cluster problemlos ausgeführt werden. Sie müssen weiterhin Abhängigkeiten konfigurieren, um Ihren benutzerdefinierten Anwendungen ggf. die Ausführung auf den Edgeknoten zu ermöglichen.

Anwendungsabhängigkeiten müssen identifiziert und der Azure Firewall-Instanz oder der Routingtabelle hinzugefügt werden.

Für den Anwendungsdatenverkehr müssen Routen erstellt werden, um Probleme durch asymmetrisches Routing zu vermeiden.

Wenn Ihre Anwendungen andere Abhängigkeiten aufweisen, müssen diese Ihrer Azure Firewall-Instanz hinzugefügt werden. Erstellen Sie Anwendungsregeln, um HTTP/HTTPS-Datenverkehr und Netzwerkregeln zuzulassen.

## <a name="logging-and-scale"></a>Protokollierung und Skalierung

Azure Firewall kann Protokolle an ein paar andere Speichersysteme senden. Anweisungen zum Konfigurieren der Protokollierung für Ihre Firewall finden Sie in den Schritten von [Tutorial: Überwachen von Azure Firewall-Protokollen und -Metriken](../firewall/tutorial-diagnostics.md).

Nachdem Sie die Protokollierung eingerichtet haben, können Sie, wenn Sie bei Log Analytics eingehende Daten protokollieren, blockierten Datenverkehr mit einer Abfrage wie der folgenden anzeigen:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Die Integration Ihrer Azure Firewall-Instanz in Azure Monitor-Protokolle ist nützlich, wenn Sie eine Anwendung erstmals einrichten und nicht alle Anwendungsabhängigkeiten kennen. Weitere Informationen zu Azure Monitor-Protokollen finden Sie unter [Analysieren von Protokolldaten in Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

Weitere Informationen zu den Skalierungsgrenzwerten von Azure Firewall und zu Anforderungssteigerungen finden Sie in [diesem](../azure-subscription-service-limits.md#azure-firewall-limits) Dokument sowie in den [Häufig gestellten Fragen](../firewall/firewall-faq.md).

## <a name="access-to-the-cluster"></a>Zugriff auf den Cluster

Nach erfolgreicher Einrichtung der Firewall können Sie den internen Endpunkt (`https://CLUSTERNAME-int.azurehdinsight.net`) verwenden, um im VNET auf Ambari zuzugreifen.

Wenn Sie den öffentlichen Endpunkt (`https://CLUSTERNAME.azurehdinsight.net`) oder den SSH-Endpunkt (`CLUSTERNAME-ssh.azurehdinsight.net`) verwenden möchten, vergewissern Sie sich, dass in der Routingtabelle und den NSG-Regeln die richtigen Routen festgelegt wurden, um das [hier](../firewall/integrate-lb.md) beschriebene Problem mit asymmetrischem Routing zu vermeiden. Insbesondere in diesem Fall müssen Sie die Client-IP-Adresse in den eingehenden NSG-Regeln zulassen und sie auch der benutzerdefinierten Routingtabelle hinzufügen, wobei der nächste Hop als `internet` festgelegt sein muss. Ist dies nicht ordnungsgemäß eingerichtet, tritt ein Timeoutfehler auf.

## <a name="configure-another-network-virtual-appliance"></a>Konfigurieren eines anderen virtuellen Netzwerkgeräts

> [!Important]
> Die folgenden Informationen sind **nur** erforderlich, wenn Sie ein anderes virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) als Azure Firewall konfigurieren möchten.

Mit den vorherigen Anweisungen können Sie Azure Firewall zum Einschränken des ausgehenden Datenverkehrs Ihres HDInsight-Clusters konfigurieren. Azure Firewall wird automatisch dazu konfiguriert, den Datenverkehr für viele der häufigen, wichtigen Szenarien zuzulassen. Wenn Sie ein anderes virtuelles Netzwerkgerät nutzen möchten, müssen Sie eine Reihe zusätzlicher Features manuell konfigurieren. Bedenken Sie beim Konfigurieren Ihres virtuellen Netzwerkgeräts Folgendes:

* Dienste, die Dienstendpunkte unterstützen, sollten mit Dienstendpunkten konfiguriert werden.
* IP-Adressabhängigkeiten gelten für Nicht-HTTP/S-Datenverkehr (TCP- und UDP-Datenverkehr).
* FQDN-HTTP/HTTPS-Endpunkte können in Ihrem virtuellen Netzwerkgerät bereitgestellt werden.
* Platzhalter-HTTP/HTTPS-Endpunkte sind Abhängigkeiten, die basierend auf einer Reihe von Qualifizierern variieren können.
* Weisen Sie die Routingtabelle zu, die Sie für Ihr HDInsight-Subnetz erstellen.

### <a name="service-endpoint-capable-dependencies"></a>Dienstendpunktfähige Abhängigkeiten

| **Endpunkt** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>IP-Adressabhängigkeiten

| **Endpunkt** | **Details** |
|---|---|
| \*:123 | NTP-Uhrzeitüberprüfung. Datenverkehr wird an mehreren Endpunkten am Port 123 überprüft. |
| [Hier](hdinsight-management-ip-addresses.md) veröffentlichte IP-Adressen | Diese gehören zum HDInsight-Dienst. |
| AAD-DS – private IP-Adressen für ESP-Cluster |
| \*:16800 für KMS-Aktivierung von Windows |
| \*12000 für Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN-HTTP/HTTPS-Abhängigkeiten

> [!Important]
> Die folgende Liste enthält nur ein paar der wichtigsten FQDNs. Sie erhalten die vollständige Liste der FQDNs für die Konfiguration Ihres virtuellen Netzwerkgeräts [in dieser Datei](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Endpunkt**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Nächste Schritte

* [Virtuelle Netzwerkarchitektur mit Azure HDInsight](hdinsight-virtual-network-architecture.md)
