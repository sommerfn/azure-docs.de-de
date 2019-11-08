---
title: Containersicherheit
titleSuffix: Azure Cognitive Services
description: Informationen zum Schützen Ihres Containers
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/30/2019
ms.author: dapine
ms.openlocfilehash: 35f5cffdc644370082e229c88d67db33e853c446
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499177"
---
## <a name="azure-cognitive-services-container-security"></a>Containersicherheit in Azure Cognitive Services

Bei der Entwicklung von Anwendungen sollte die Sicherheit im Vordergrund stehen. Die Bedeutung von Sicherheit ist ein Erfolgskriterium. Wenn Sie eine Softwarelösung entwickeln, die Cognitive Services-Container enthält, ist es wichtig, die Grenzen und Möglichkeiten zu verstehen, die Ihnen zur Verfügung stehen. Weitere Informationen zur Netzwerksicherheit finden Sie unter [Konfigurieren von virtuellen Netzwerken für Azure Cognitive Services][az-security].

> [!IMPORTANT]
> Standardmäßig ist *keine Sicherheit* für die Cognitive Services-Container-API vorhanden. Der Grund dafür ist, dass der Container meist als Teil eines Pod ausgeführt wird, das von außen durch eine Netzwerkbrücke geschützt ist. Es ist jedoch möglich, die Authentifizierung zu aktivieren, die identisch zu der Authentifizierung funktioniert, die beim Zugriff auf die [cloudbasierten Cognitive Services][request-authentication] verwendet wird.

Das Diagramm unten veranschaulicht den standardmäßigen und **nicht sicheren** Ansatz:

![Containersicherheit](../media/container-security.svg)

Als alternativer und *sicherer* Ansatz könnten Consumer von Cognitive Services-Containern einen Container mit einer Frontkomponente erweitern, wodurch der Containerendpunkt privat bleibt. Betrachten wir ein Szenario, in dem [Istio][istio] als Eingangsgateway verwendet wird. Istio unterstützt HTTPS/SSL und die Authentifizierung über Clientzertifikate. In diesem Szenario stellt das Istio-Front-End den Containerzugriff bereit und präsentiert das Clientzertifikat, das zuvor von Istio zur Whitelist hinzugefügt wurde.

[Nginx][nginx] ist eine weitere beliebte Option in derselben Kategorie. Sowohl Istio als auch Nginx fungieren als Dienstnetz und bieten zusätzliche Features wie Lastenausgleich, Routing und Ratensteuerung.

### <a name="container-networking"></a>Containernetzwerke

Die Cognitive Services-Container sind erforderlich, um Messungsinformationen zu Abrechnungszwecken zu übermitteln. Die einzige Ausnahme bilden *Offlinecontainer*, da sie einer anderen Abrechnungsmethode unterliegen. Wenn Sie die verschiedenen Netzwerkkanäle, auf die sich die Cognitive Services-Container stützen, nicht in einer Zulassungsliste aufführen, funktionieren die Container nicht.

#### <a name="allow-list-cognitive-services-domains-and-ports"></a>Cognitive Services-Domänen und Ports in der Zulassungsliste aufführen

Der Host sollte **Port 443** und die folgenden Domänen in der Zulassungsliste aufführen:

* `*.cognitive.microsoft.com`
* `*.cognitiveservices.azure.com`

#### <a name="disable-deep-packet-inspection"></a>Deaktivieren der eingehenden Paketuntersuchung

> [Eingehende Paketuntersuchung](https://en.wikipedia.org/wiki/Deep_packet_inspection) (Deep Packet Inspection, DPI) ist eine Art der Datenverarbeitung, bei der die in einem Computernetzwerk gesendeten Daten im Detail untersucht werden. Die zur Verfügung stehenden Maßnahmen sind Blockieren, Umleiten oder Protokollieren von Daten.

Deaktivieren Sie DPI für die sicheren Kanäle, die von Cognitive Services-Containern zu Microsoft-Servern erstellt werden. Andernfalls funktionieren die Container nicht ordnungsgemäß.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../cognitive-services-virtual-networks.md
