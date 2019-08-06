---
title: Konfigurieren von Azure Firewall-Anwendungsregeln mit SQL-FQDNs
description: In diesem Artikel erfahren Sie, wie SQL-FQDNs in Azure Firewall-Anwendungsregeln konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 07/19/2019
ms.author: victorh
ms.openlocfilehash: 239998f29ac9a578174c5dba547bb24ba0755505
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68318188"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Konfigurieren von Azure Firewall-Anwendungsregeln mit SQL-FQDNs

> [!IMPORTANT]
> Azure Firewall-Anwendungsregeln mit SQL-FQDNs befinden sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können nun Azure Firewall-Anwendungsregeln mit SQL-FQDNs konfigurieren. So können Sie den Zugriff aus Ihren virtuellen Netzwerken auf die angegebenen SQL Server-Instanzen beschränken.

Mit SQL-FQDNs können Sie Datenverkehr wie folgt filtern:

- Von Ihren VNETs an eine Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Instanz. Beispiel:  Sie haben die Möglichkeit, nur Zugriffe auf *sql-server1.database.windows.net* zuzulassen.
- Von Ihrer lokalen Umgebung an verwaltete Azure SQL-Instanzen oder an SQL-IaaS in Ihren VNETs
- Von Spoke-zu-Spoke an verwaltete Azure SQL-Instanzen oder an SQL-IaaS in Ihren VNETs

Während der Public Preview-Phase wird die SQL-FQDN-Filterung nur im [Proxymodus](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) unterstützt (Port 1433). Wenn Sie SQL im standardmäßigen Umleitungsmodus verwenden, können Sie zur Filterung des Zugriffs das SQL-Diensttag im Rahmen von [Netzwerkregeln](overview.md#network-traffic-filtering-rules) verwenden.
Wenn Sie nicht standardmäßige Ports für SQL-IaaS-Datenverkehr verwenden, können Sie diese Ports in den Firewallanwendungsregeln konfigurieren.

Anwendungsregeln mit SQL-FQDNs stehen derzeit in allen Regionen über Azure-Portal, Azure-Befehlszeilenschnittstelle, REST und Vorlagen zur Verfügung.

## <a name="configure-using-azure-cli"></a>Konfigurieren über die Azure-Befehlszeilenschnittstelle

1. [Stellen Sie eine Azure Firewall-Instanz unter Verwendung der Azure-Befehlszeilenschnittstelle bereit.](deploy-cli.md)
2. Wenn Sie Datenverkehr an Azure SQL-Datenbank, SQL Data Warehouse oder an eine verwaltete SQL-Instanz filtern möchten, muss der SQL-Konnektivitätsmodus auf **Proxy** festgelegt sein. Informationen zum Ändern des SQL-Konnektivitätsmodus finden Sie unter [Verbindungsarchitektur von Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > Im SQL-Modus *Proxy* kommt es unter Umständen zu längeren Wartezeiten als im Modus *Umleiten*. Wenn Sie weiterhin den Umleitungsmodus (Standardmodus für Clients, die eine Verbindung innerhalb von Azure herstellen) verwenden möchten, können Sie den Zugriff mit dem SQL-[Diensttag](service-tags.md) in [Netzwerkregeln](tutorial-firewall-deploy-portal.md#configure-a-network-rule) der Firewall filtern.

3. Konfigurieren Sie eine Anwendungsregel mit SQL-FQDN, um Zugriff auf einen SQL-Server zu gewähren:

   ```azurecli
   az extension add -n azure-firewall

   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Konfigurieren über das Azure-Portal
1. [Stellen Sie eine Azure Firewall-Instanz unter Verwendung der Azure-Befehlszeilenschnittstelle bereit.](deploy-cli.md)
2. Wenn Sie Datenverkehr an Azure SQL-Datenbank, SQL Data Warehouse oder an eine verwaltete SQL-Instanz filtern möchten, muss der SQL-Konnektivitätsmodus auf **Proxy** festgelegt sein. Informationen zum Ändern des SQL-Konnektivitätsmodus finden Sie unter [Verbindungsarchitektur von Azure SQL](../sql-database/sql-database-connectivity-architecture.md#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > Im SQL-Modus *Proxy* kommt es unter Umständen zu längeren Wartezeiten als im Modus *Umleiten*. Wenn Sie weiterhin den Umleitungsmodus (Standardmodus für Clients, die eine Verbindung innerhalb von Azure herstellen) verwenden möchten, können Sie den Zugriff mit dem SQL-[Diensttag](service-tags.md) in [Netzwerkregeln](tutorial-firewall-deploy-portal.md#configure-a-network-rule) der Firewall filtern.
3. Fügen Sie die Anwendungsregel mit den entsprechenden Angaben für Protokoll, Port und SQL-FQDN hinzu, und wählen Sie anschließend **Speichern** aus.
   ![Anwendungsregel mit SQL-FQDN](media/sql-fqdn-filtering/application-rule-sql.png)
4. Greifen Sie über einen virtuellen Computer in einem VNET mit firewallbasierter Datenverkehrsfilterung auf SQL zu. 
5. Vergewissern Sie sich anhand von [Azure Firewall-Protokollen](log-analytics-samples.md), dass der Datenverkehr zugelassen wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den SQL-Modi „Proxy“ und „Umleiten“ finden Sie unter [Verbindungsarchitektur von Azure SQL](../sql-database/sql-database-connectivity-architecture.md).