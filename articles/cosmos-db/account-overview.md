---
title: Verwenden von Azure Cosmos DB-Konten
description: In diesem Artikel erfahren Sie, wie Sie Azure Cosmos DB-Konten erstellen und verwenden.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: 57b2cffbf8085e6d393765190a6dfc2dead51625
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757090"
---
# <a name="work-with-azure-cosmos-account"></a>Arbeiten mit einem Azure Cosmos-Konto

Azure Cosmos DB ist ein vollständig verwalteter PaaS-Dienst (Platform-as-a-Service, PaaS). Um Azure Cosmos DB verwenden zu können, müssen Sie zunächst ein Azure Cosmos-Konto in Ihrem Azure-Abonnement erstellen. Ihr Azure Cosmos-Konto enthält einen eindeutigen DNS-Namen, und für die Kontoverwaltung können Sie das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder verschiedene sprachspezifische SDKs verwenden. Weitere Informationen finden Sie unter [Verwalten von Datenbankkonten in Azure Cosmos DB](how-to-manage-database-account.md).

Das Azure Cosmos-Konto ist die grundlegende Einheit für globale Verteilung und Hochverfügbarkeit. Sie können Ihrem Azure Cosmos-Konto jederzeit Azure-Regionen hinzufügen oder Azure-Regionen daraus entfernen, um Ihre Daten und Ihren Durchsatz global auf mehrere Azure-Regionen zu verteilen. Ihr Azure Cosmos-Konto kann mit einer einzelnen Schreibregion oder mit mehreren Schreibregionen konfiguriert werden. Weitere Informationen finden Sie unter [Hinzufügen/Entfernen von Regionen für Ihr Datenbankkonto](how-to-manage-database-account.md). Sie können die [Standardkonsistenzebene](consistency-levels.md) für Ihr Azure Cosmos DB-Konto konfigurieren. Azure Cosmos DB bietet umfassende SLAs mit Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit. Weitere Informationen finden Sie unter [SLA für Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Zur sicheren Verwaltung des Zugriffs auf sämtliche Daten in Ihrem Azure Cosmos-Konto können Sie die [Hauptschlüssel](secure-access-to-data.md) verwenden, die Ihrem Konto zugeordnet sind. Darüber hinaus können Sie zum Schutz des Zugriffs auf Ihre Daten einen [VNET-Dienstendpunkt](vnet-service-endpoint.md) und eine [IP-Firewall](firewall-support.md) für Ihr Azure Cosmos-Konto konfigurieren. 

## <a name="elements-in-an-azure-cosmos-account"></a>Elemente in einem Azure Cosmos-Konto

Ein Azure Cosmos-Container ist die grundlegende Einheit für die Skalierbarkeit. Ein Container kann über nahezu unbegrenzten bereitgestellten Durchsatz (RU/s) und Speicher verfügen. Azure Cosmos DB partitioniert Ihren Container transparent unter Verwendung des logischen Partitionsschlüssels, den Sie angeben, um die elastische Skalierung Ihres bereitgestellten Durchsatzes und Speichers zu ermöglichen. Weitere Informationen finden Sie unter [Arbeiten mit Azure Cosmos-Datenbanken, -Containern und -Elementen](databases-containers-items.md).

Aktuell können unter einem Azure-Abonnement bis zu 100 Azure Cosmos-Konten erstellt werden. Ein einzelnes Azure Cosmos-Konto kann eine nahezu unbegrenzte Menge an Daten und bereitgestelltem Durchsatz verwalten. Zur Verwaltung Ihrer Daten und Ihres bereitgestellten Durchsatzes können Sie unter Ihrem Konto eine oder mehrere Azure Cosmos-Datenbanken und innerhalb der jeweiligen Datenbank einen oder mehrere Container erstellen. Die folgende Abbildung zeigt die Hierarchie der Elemente in einem Azure Cosmos-Konto:

![Hierarchie eines Azure Cosmos-Kontos](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten Ihres Azure Cosmos-Kontos und zu anderen Konzepten:

* [Verwalten von Datenbankkonten in Azure Cosmos DB](how-to-manage-database-account.md)
* [Globale Verteilung](distribute-data-globally.md)
* [Konsistenzebenen](consistency-levels.md)
* [Arbeiten mit Azure Cosmos-Datenbanken, -Containern und -Elementen](databases-containers-items.md)
* [Sicherer Zugriff auf ein Azure Cosmos DB-Konto durch Verwenden eines Azure Virtual Network-Dienstendpunkts](vnet-service-endpoint.md)
* [Azure Cosmos DB-Firewallunterstützung](firewall-support.md)
* [Hinzufügen/Entfernen von Regionen für Ihr Datenbankkonto](how-to-manage-database-account.md)
* [SLA für Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
