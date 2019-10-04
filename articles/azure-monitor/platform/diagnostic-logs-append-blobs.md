---
title: Vorbereiten der Formatumstellung auf Azure Monitor-Diagnoseprotokolle
description: Für Azure-Diagnoseprotokolle wird am 1. November 2018 die Umstellung auf die Nutzung von Anfügeblobs durchgeführt.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: a5589828570455c61f857dbeadc896e8fef27178
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71258380"
---
# <a name="prepare-for-format-change-to-azure-monitor-diagnostic-logs-archived-to-a-storage-account"></a>Vorbereiten der Formatumstellung auf Azure Monitor-Diagnoseprotokolle, die in einem Speicherkonto archiviert werden

> [!WARNING]
> Wenn Sie [mithilfe von Ressourcendiagnoseeinstellungen Azure-Ressourcendiagnoseprotokolle oder -metriken an ein Speicherkonto](./../../azure-monitor/platform/archive-diagnostic-logs.md) oder [Aktivitätsprotokolle mithilfe von Protokollprofilen an ein Speicherkonto](./../../azure-monitor/platform/archive-activity-log.md) senden, ändert sich das Format der Daten im Speicherkonto am 1. November 2018 in JSON Lines. Unten wird beschrieben, welche Auswirkungen sich ergeben und wie Sie Ihre Tools für die Verarbeitung des neuen Formats aktualisieren. 
>
> 

## <a name="what-is-changing"></a>Was ändert sich?

Azure Monitor verfügt über eine Funktion, mit der Sie Ressourcendiagnosedaten und Aktivitätsprotokolldaten an ein Azure-Speicherkonto, einen Event Hubs-Namespace oder Log Analytics-Arbeitsbereich in Azure Monitor senden können. Am **1. November 2018 um Mitternacht (UTC)** ändert sich das Format der Protokolldaten, die an den Blobspeicher gesendet werden, um ein Problem mit der Systemleistung zu beheben. Falls Sie über Tools verfügen, mit denen Daten aus dem Blobspeicher gelesen werden, müssen Sie diese Tools aktualisieren, damit das neue Datenformat gelesen werden kann.

* Am Donnerstag, den 1. November 2018, um Mitternacht (UTC) wird das Blobformat in [JSON Lines](http://jsonlines.org/) geändert. Dies bedeutet, dass die einzelnen Datensätze jeweils durch einen Zeilenumbruch getrennt sind und dass kein externes Datensatzarray und keine Kommas zwischen JSON-Datensätzen verwendet werden.
* Das Blobformat wird für alle Diagnoseeinstellungen aller Abonnements auf einmal geändert. Die erste Datei „PT1H.json“, die für den 1. November ausgegeben wird, hat dieses neue Format. Die Blob- und Containernamen bleiben unverändert.
* Wenn zwischen dem heutigen Datum und dem 1. November eine Diagnoseeinstellung festgelegt wird, werden die Daten bis zum 1. November weiterhin im derzeitigen Format ausgegeben.
* Diese Änderung tritt für alle öffentlichen Cloudregionen gleichzeitig in Kraft. Die Änderung wird noch nicht für von 21Vianet betriebenen Microsoft Azure-, Azure Deutschland- oder Azure Government-Clouds durchgeführt.
* Diese Änderung wirkt sich auf die folgenden Datentypen aus:
  * [Azure-Ressourcendiagnoseprotokolle](archive-diagnostic-logs.md) ([Liste mit Ressourcen](diagnostic-logs-schema.md))
  * [Von Diagnoseeinstellungen exportierte Azure-Ressourcenmetriken](diagnostic-settings.md)
  * [Von Protokollprofilen exportierte Azure-Aktivitätsprotokolldaten](archive-activity-log.md)
* Diese Änderung wirkt sich nicht auf Folgendes aus:
  * Netzwerkflussprotokolle
  * Azure-Dienstprotokolle, die noch nicht über Azure Monitor verfügbar gemacht werden (z. B. Azure App Service-Diagnoseprotokolle, Speicheranalyseprotokolle)
  * Routing von Azure-Diagnoseprotokollen und Aktivitätsprotokollen an andere Ziele (Event Hubs, Log Analytics)

### <a name="how-to-see-if-you-are-impacted"></a>Ermitteln der Auswirkungen für Sie

Diese Änderung hat für Sie nur Auswirkungen, wenn Folgendes gilt:
1. Sie senden Protokolldaten an ein Azure-Speicherkonto, indem Sie eine Ressourcendiagnoseeinstellung nutzen.
2. Sie verfügen über Tools, die von der JSON-Struktur dieser Protokolle im Speicher abhängig sind.
 
Um zu ermitteln, ob Sie über Ressourcendiagnoseeinstellungen verfügen, die Daten an ein Azure-Speicherkonto senden, können Sie wie folgt vorgehen: Navigieren Sie im Portal zum Abschnitt **Überwachen**, klicken Sie auf **Diagnoseeinstellungen**, und identifizieren Sie alle Ressourcen, für die **Diagnostic Status** (Diagnosestatus) auf **Aktiviert** festgelegt ist:

![Blatt „Diagnoseeinstellungen“ von Azure Monitor](./media/diagnostic-logs-append-blobs/portal-diag-settings.png)

Wenn der Diagnosestatus aktiviert ist, verfügen Sie über eine aktive Diagnoseeinstellung für diese Ressource. Klicken Sie auf die Ressource, um zu ermitteln, ob Diagnoseeinstellungen Daten an ein Speicherkonto senden:

![Aktiviertes Speicherkonto](./media/diagnostic-logs-append-blobs/portal-storage-enabled.png)

Wenn bei Ihnen Ressourcen Daten an ein Speicherkonto senden, indem diese Ressourcendiagnoseeinstellungen verwendet werden, ist das Format der Daten im Speicherkonto von dieser Änderung betroffen. Die Formatänderung wirkt sich für Sie nur aus, wenn Sie über benutzerdefinierte Tools verfügen, die basierend auf diesen Speicherkonten genutzt werden.

### <a name="details-of-the-format-change"></a>Details zur Formatänderung

Für das aktuelle Format der Datei „PT1H.json“ im Azure-Blobspeicher wird ein JSON-Array mit Datensätzen verwendet. Hier ist ein Beispiel für eine KeyVault-Protokolldatei angegeben:

```json
{
    "records": [
        {
            "time": "2016-01-05T01:32:01.2691226Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "78",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

Für das neue Format wird [JSON Lines](http://jsonlines.org/) verwendet. Dies bedeutet, dass jedes Ereignis in einer Zeile enthalten ist und mit dem Zeilenumbruchzeichen ein neues Ereignis angegeben wird. Hier ist dargestellt, wie das obige Beispiel in der Datei „PT1H.json“ nach der Änderung aussieht:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Mit diesem neuen Format kann Azure Monitor Protokolldateien mithilfe von [Anfügeblobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs) per Pushvorgang übertragen. Dies ist für das fortlaufende Anfügen neuer Ereignisdaten effizienter.

## <a name="how-to-update"></a>Durchführen der Aktualisierung

Sie müssen nur dann Aktualisierungen durchführen, wenn Sie über benutzerdefinierte Tools verfügen, mit denen diese Protokolldateien für die weitere Verarbeitung erfasst werden. Falls Sie ein externes Protokollanalyse- oder SIEM-Tool nutzen, empfehlen wir Ihnen, [stattdessen Event Hubs zum Erfassen dieser Daten zu verwenden](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/). Die Integration von Event Hubs ist in Bezug auf die Verarbeitung von Protokollen vieler Dienste und die Lesezeichenposition in einem bestimmten Protokoll einfacher möglich.

Benutzerdefinierte Tools sollten aktualisiert werden, damit diese sowohl das aktuelle Format als auch das oben beschriebene JSON Lines-Format verarbeiten können. So wird sichergestellt, dass Ihre Tools vorbereitet sind, wenn Daten im neuen Format gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Archivieren von Azure-Diagnoseprotokollen](./../../azure-monitor/platform/archive-diagnostic-logs.md).
* Informieren Sie sich über das [Archivieren des Azure-Aktivitätsprotokolls](./../../azure-monitor/platform/archive-activity-log.md).

