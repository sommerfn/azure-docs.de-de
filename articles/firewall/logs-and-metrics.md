---
title: Übersicht über Azure Firewall-Protokolle und -Metriken
description: Dieser Artikel bietet eine Übersicht über die Diagnoseprotokolle und Metriken in Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 08/22/2019
ms.author: victorh
ms.openlocfilehash: fea00358fc21cf6f57673e14ebd0feafe532b620
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876557"
---
# <a name="azure-firewall-logs-and-metrics"></a>Azure Firewall-Protokolle und -Metriken

Azure Firewall kann mithilfe von Firewallprotokollen überwacht werden. Sie können aber auch Aktivitätsprotokolle verwenden, um Vorgänge für Azure Firewall-Ressourcen zu überwachen.

Sie können auf einige dieser Protokolle über das Portal zugreifen. Protokolle können an [Azure Monitor-Protokolle](../azure-monitor/insights/azure-networking-analytics.md), Storage und Event Hubs gesendet und in Azure Monitor-Protokollen oder durch andere Tools wie Excel oder Power BI analysiert werden.

Metriken sind einfach und können Szenarien nahezu in Echtzeit unterstützen. Dadurch lassen sie sich für Warnungen und zur schnellen Problemerkennung einsetzen. 

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

 Für Azure Firewall sind folgende Diagnoseprotokolle verfügbar:

* **Anwendungsregelprotokoll**

   Das Anwendungsregelprotokoll wird nur dann in einem Speicherkonto gespeichert, an Event Hubs gestreamt und/oder an Azure Monitor-Protokolle gesendet, wenn Sie es für die einzelnen Azure Firewall-Instanzen aktiviert haben. Jede neue Verbindung, die einer Ihrer konfigurierten Anwendungsregeln entspricht, führt zu einem Protokoll für die akzeptierte/abgelehnte Verbindung. Die Daten werden im JSON-Format protokolliert, wie im folgenden Beispiel zu sehen:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Netzwerkregelprotokoll**

   Das Netzwerkregelprotokoll wird nur dann in einem Speicherkonto gespeichert, an Event Hubs gestreamt und/oder an Azure Monitor-Protokolle gesendet, wenn Sie es für die einzelnen Azure Firewall-Instanzen aktiviert haben. Jede neue Verbindung, die einer Ihrer konfigurierten Netzwerkregeln entspricht, führt zu einem Protokoll für die akzeptierte/abgelehnte Verbindung. Die Daten werden im JSON-Format protokolliert, wie im folgenden Beispiel zu sehen:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Sie haben drei Möglichkeiten, um Ihre Protokolle zu speichern:

* **Speicherkonto:** Speicherkonten eignen sich am besten für Protokolle, die eine längere Zeit gespeichert und bei Bedarf überprüft werden.
* **Event Hubs:** Event Hubs sind eine hervorragende Möglichkeit für die Integration in andere SIEM-Tools (Security Information and Event Management), um Warnungen für Ihre Ressourcen zu erhalten.
* **Azure Monitor-Protokolle:** Azure Monitor-Protokolle eignen sich am besten für eine allgemeine Echtzeitüberwachung Ihrer Anwendung oder zum Beobachten von Trends.

## <a name="activity-logs"></a>Aktivitätsprotokolle

   Aktivitätsprotokolleinträge werden standardmäßig gesammelt und können im Azure-Portal angezeigt werden.

   Mit [Azure-Aktivitätsprotokollen](../azure-resource-manager/resource-group-audit.md) (ehemals Betriebs- und Überwachungsprotokolle) können Sie alle an Ihr Azure-Abonnement übermittelten Vorgänge anzeigen.

## <a name="metrics"></a>metrics

Metriken in Azure Monitor sind numerische Werte, die einen Aspekt eines Systems zu einem bestimmten Zeitpunkt beschreiben. Metriken werden jede Minute erfasst und eignen sich gut für Warnmeldungen, da sie häufig abgefragt werden können. Eine Warnung kann mit relativ einfacher Logik schnell ausgelöst werden.

Für Azure Firewall sind folgende Metriken verfügbar:

- **Application rules hit count** (Trefferanzahl für Anwendungsregeln): Gibt an, wie oft eine Anwendungsregel ausgelöst wurde.

    Einheit: Anzahl

- **Network rules hit count** (Trefferanzahl für Netzwerkregeln): Gibt an, wie oft eine Netzwerkregel ausgelöst wurde.

    Einheit: Anzahl

- **Verarbeitete Daten:** Datenmenge, die die Firewall durchläuft.

    Einheit: Bytes

- **Firewall health state** (Firewallintegritätszustand): Gibt die Integrität der Firewall an.

    Einheit: Prozent

   Diese Metrik enthält zwei Dimensionen:
  - **Status:** Mögliche Werte sind *Fehlerfrei*, *Beeinträchtigt* und *Fehlerhaft*.
  - **Grund:** Gibt den Grund für den entsprechenden Status der Firewall an. Beispielsweise kann *SNAT-Ports* angegeben sein, wenn der Firewallstatus „Beeinträchtigt“ oder „Fehlerhaft“ lautet.





- **SNAT port utilization** (SNAT-Portnutzung): Prozentangabe der SNAT-Ports, die durch die Firewall genutzt werden.

    Einheit: Prozent

   Wenn Sie der Firewall weitere öffentliche IP-Adressen hinzufügen, sind zusätzliche SNAT-Ports verfügbar, wodurch die Auslastung der SNAT-Ports reduziert wird. Wenn die Firewall aus unterschiedlichen Gründen horizontal hochskaliert wird (z. B. CPU oder Durchsatz), sind ebenfalls zusätzliche SNAT-Ports verfügbar. Ein bestimmter Prozentsatz der SNAT-Portnutzung kann sich also effektiv verringern, ohne dass Sie öffentliche IP-Adressen hinzufügen, sondern nur weil der Dienst horizontal hochskaliert wurde. Sie können die Anzahl der verfügbaren öffentlichen IP-Adressen direkt steuern, um die für die Firewall verfügbaren Ports zu erhöhen. Die Firewallskalierung können Sie jedoch nicht direkt steuern. Derzeit werden SNAT-Ports nur für die ersten fünf öffentlichen IP-Adressen hinzugefügt.   


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Überwachen von Azure Firewall-Protokollen und Metriken finden Sie unter [Tutorial: Überwachen von Azure Firewall-Protokollen](tutorial-diagnostics.md).

- Weitere Informationen zu Metriken in Azure Monitor finden Sie unter [Metriken in Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).