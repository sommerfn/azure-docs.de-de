---
title: 'InvalidNetworkSecurityGroupSecurityRules: Fehler bei der Clustererstellung in Azure HDInsight'
description: 'Fehler bei der Clustererstellung. Fehlercode: InvalidNetworkSecurityGroupSecurityRules'
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/31/2019
ms.openlocfilehash: 35e583fa31ee99c9d6307c01287c2c0fde021280
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700234"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Szenario: InvalidNetworkSecurityGroupSecurityRules: Fehler bei der Clustererstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie erhalten den Fehlercode `InvalidNetworkSecurityGroupSecurityRules` mit einer Beschreibung wie: „Die Sicherheitsregeln in der Netzwerksicherheitsgruppe, die für das Subnetz konfiguriert ist, lässt die erforderliche ein- bzw. ausgehende Konnektivität nicht zu“.

## <a name="cause"></a>Ursache

Es liegt vermutlich ein Problem mit den Regeln für [Netzwerksicherheitsgruppen](../../virtual-network/virtual-network-vnet-plan-design-arm.md) für eingehenden Datenverkehr vor, die für Ihren Cluster konfiguriert wurden.

## <a name="resolution"></a>Lösung

Navigieren Sie zum Azure-Portal, und ermitteln Sie die NSG, die dem Subnetz zugeordnet ist, in dem der Cluster bereitgestellt wird. Stellen Sie im Abschnitt **Eingangssicherheitsregeln** sicher, dass die Regeln den Zugriff in eingehender Richtung auf Port 443 für die [hier](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip) genannten IP-Adressen zulassen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
