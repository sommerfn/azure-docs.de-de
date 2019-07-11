---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 46376477aae9f94c1c8f6e1dd6bc718d213cc373
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712419"
---
Der Container hat die folgenden Konfigurationseinstellungen:

|Erforderlich|Einstellung|Zweck|
|--|--|--|
|Ja|[ApiKey](#apikey-configuration-setting)|Nachverfolgen von Abrechnungsinformationen|
|Nein|[ApplicationInsights](#applicationinsights-setting)|Ermöglicht das Hinzufügen von Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten in Ihrem Container.|
|Ja|[Abrechnung](#billing-configuration-setting)|Gibt den Endpunkt-URI der Dienstressource in Azure an.|
|Ja|[Eula](#eula-setting)| Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.|
|Nein|[Fluentd](#fluentd-settings)|Schreibt Protokoll- und optional auch Metrikdaten auf einen Fluentd-Server.|
|Nein|HTTP-Proxy|Konfiguriert einen HTTP-Proxy für ausgehende Anforderungen.|
|Nein|[Protokollierung](#logging-settings)|Bietet Unterstützung für die ASP.NET Core-Protokollierung für Ihren Container. |
|Nein|[Mounts](#mount-settings)|Liest und schreibt Daten vom Hostcomputer in den Container und umgekehrt.|
