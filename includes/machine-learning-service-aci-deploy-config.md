---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 10/06/2019
ms.author: larryfr
ms.openlocfilehash: b3c1ad48da0ef28c73e3af5d1e30414534f4e94d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73799851"
---
Die Einträge im Dokument `deploymentconfig.json` werden den Parametern für [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py) zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | BESCHREIBUNG |
| ----- | ----- | ----- |
| `computeType` | Nicht verfügbar | Das Computeziel. Für ACI muss der Wert `ACI` sein. |
| `containerResourceRequirements` | Nicht verfügbar | Der Container für die CPU- und Arbeitsspeicherentitäten. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Die Anzahl der zuzuordnenden CPU-Kerne. Standardwert: `0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Der Arbeitsspeicherumfang (in GB), der für diesen Webdienst zugeordnet werden soll. Standardwert: `0.5` |
| `location` | `location` | Die Azure-Region, in der dieser Webdienst bereitgestellt werden soll. Wenn keine Region angegeben ist, wird der Standort des Arbeitsbereichs verwendet. Weitere Informationen zu verfügbaren Regionen finden Sie hier: [ACI-Regionen](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Gibt an, ob die Authentifizierung für diesen Webdienst aktiviert werden soll. Der Standardwert ist „FALSE“. |
| `sslEnabled` | `ssl_enabled` | Gibt an, ob für diesen Webdienst SSL aktiviert werden soll. Der Standardwert lautet „False“. |
| `appInsightsEnabled` | `enable_app_insights` | Gibt an, ob für diesen Webdienst AppInsights aktiviert werden soll. Der Standardwert ist „FALSE“. |
| `sslCertificate` | `ssl_cert_pem_file` | Die benötigte CERT-Datei, wenn SSL aktiviert ist |
| `sslKey` | `ssl_key_pem_file` | Die benötigte KEY-Datei, wenn SSL aktiviert ist |
| `cname` | `ssl_cname` | Der CNAME, wenn SSL aktiviert ist |
| `dnsNameLabel` | `dns_name_label` | Die DNS-Namensbezeichnung für den Bewertungsendpunkt. Wenn keine eindeutige DNS-Namensbezeichnung angegeben ist, wird eine für den Bewertungsendpunkt generiert. |

Der folgende JSON-Code ist ein Beispiel für eine Bereitstellungskonfiguration für die Verwendung mit der CLI:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```