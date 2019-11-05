---
author: ggailey777
ms.service: billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: ad87964216db72cfef447a18d4cd05b8a976491b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73476931"
---
| Resource | [Verbrauchstarif](../articles/azure-functions/functions-scale.md#consumption-plan) | [Premium-Plan](../articles/azure-functions/functions-scale.md#premium-plan) | [App Service-Plan](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Horizontales Skalieren | Ereignisgesteuert | Ereignisgesteuert | [Manuelle Skalierung/Autoskalierung](../articles/app-service/manage-scale-up.md) | 
| Maximale Anzahl Instanzen | 200 | 20 | 10 – 20 |
|Standardmäßige [Timeoutzeit](../articles/azure-functions/functions-scale.md#timeout) (in Minuten) |5 | 30 |30<sup>2</sup> |
|Maximale [Timeoutzeit](../articles/azure-functions/functions-scale.md#timeout) (in Minuten) |10 | unbounded | unbegrenzt<sup>3</sup> |
| Maximale Anzahl ausgehender Verbindungen (pro Instanz) | 600 aktive (insgesamt 1.200) | unbounded | unbounded |
| Maximale Anforderungsgröße (MB)<sup>4</sup> | 100 | 100 | 100 |
| Maximale Länge der Abfragezeichenfolge<sup>4</sup> | 4096 | 4096 | 4096 |
| Maximale Länge der Anforderungs-URL<sup>4</sup> | 8192 | 8192 | 8192 |
| [ACU](../articles/virtual-machines/windows/acu.md) pro Instanz | 100 | 210–840 | 100–840 |
| Maximaler Arbeitsspeicher (GB pro Instanz) | 1.5 | 3,5–14 | 1,75–14 |
| Funktions-Apps pro Plan |100 |100 |unbegrenzt<sup>5</sup> |
| [App Service-Pläne](../articles/app-service/overview-hosting-plans.md) | 100 pro [Region](https://azure.microsoft.com/global-infrastructure/regions/) |100 pro Ressourcengruppe |100 pro Ressourcengruppe |
| Speicher<sup>6</sup> |1 GB |250 GB |50–1.000 GB |
| Benutzerdefinierte Domänen pro App</a> |500<sup>7</sup> |500 |500 |
| benutzerdefinierte Domäne [SSL-Unterstützung](../articles/app-service/configure-ssl-bindings.md) |Unbegrenzte Anzahl von SNI SSL-Verbindungen inbegriffen | Unbegrenzte Anzahl von SNI SSL-Verbindungen und 1 IP-SSL-Verbindung inbegriffen |Unbegrenzte Anzahl von SNI SSL-Verbindungen und 1 IP-SSL-Verbindung inbegriffen | 

<sup>1</sup> Spezifische Grenzwerte für die verschiedenen Optionen des App Service-Plans finden Sie unter [Grenzwerte für App Service-Pläne](../articles/azure-subscription-service-limits.md#app-service-limits).  
<sup>2</sup> Das Timeout für die Laufzeit von Functions 1.x in einem App Service-Plan ist standardmäßig unbegrenzt.  
<sup>3</sup> Hierfür muss der App Service-Plan auf [Always On](../articles/azure-functions/functions-scale.md#always-on) festgelegt werden. Die Bezahlung erfolgt zu den üblichen [Raten](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup>Diese Grenzwerte werden [auf dem Host festgelegt](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config).  
<sup>5</sup>Die tatsächliche Anzahl der Funktions-Apps, die Sie hosten können, hängt von der Aktivität der Apps, der Größe der Computerinstanzen und der entsprechenden Ressourcenauslastung ab.  
<sup>6</sup> Der Speichergrenzwert entspricht der gesamten Inhaltsgröße aller Apps im gleichen App Service-Plan im temporären Speicher. Der Verbrauchsplan verwendet Azure Files zur temporären Speicherung.  
<sup>7</sup> Wenn Ihre Funktions-App unter einem [Verbrauchsplan](../articles/azure-functions/functions-scale.md#consumption-plan) gehostet wird, wird nur die CNAME-Option unterstützt. Für Funktions-Apps unter einem [Premium-Plan](../articles/azure-functions/functions-scale.md#premium-plan) oder einem [App Service-Plan](../articles/azure-functions/functions-scale.md#app-service-plan) können Sie eine benutzerdefinierte Domäne entweder mit einem CNAME- oder einem A-Eintrag zuordnen.
