---
title: Containersicherheit
titleSuffix: Azure Cognitive Services
description: Informationen zum Schützen Ihres Containers
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: dapine
ms.openlocfilehash: d8d069dddbce6ab6ddb541db460634ad3f6fa067
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70994960"
---
## <a name="azure-cognitive-services-container-security"></a>Containersicherheit in Azure Cognitive Services

Bei der Entwicklung von Anwendungen sollte die Sicherheit im Vordergrund stehen. Die Bedeutung von Sicherheit ist ein Erfolgskriterium. Wenn Sie eine Softwarelösung entwickeln, die Cognitive Services-Container enthält, ist es wichtig, die Grenzen und Möglichkeiten zu verstehen, die Ihnen zur Verfügung stehen. Weitere Informationen finden Sie unter [Azure Security][az-security].

> [!IMPORTANT]
> Standardmäßig ist *keine Sicherheit* für die Cognitive Services-Container-API vorhanden. Der Grund dafür ist, dass der Container meist als Teil eines Pod ausgeführt wird, das von außen durch eine Netzwerkbrücke geschützt ist. Es ist jedoch möglich, die Authentifizierung zu aktivieren, die identisch zu der Authentifizierung funktioniert, die beim Zugriff auf die [cloudbasierten Cognitive Services][request-authentication] verwendet wird.

Das folgende Diagramm veranschaulicht den standardmäßigen und **unsicheren** Ansatz:

![Containersicherheit](../media/container-security.svg)

Als alternativer und *sicherer* Ansatz könnten Consumer von Cognitive Services-Containern einen Container mit einer Frontkomponente erweitern, wodurch der Containerendpunkt privat bleibt. Betrachten wir ein Szenario, in dem [Istio][istio] als Eingangsgateway verwendet wird. Istio unterstützt HTTPS/SSL und die Authentifizierung über Clientzertifikate. In diesem Szenario stellt das Istio-Front-End den Containerzugriff bereit und präsentiert das Clientzertifikat, das zuvor von Istio zur Whitelist hinzugefügt wurde.

[Nginx][nginx] ist eine weitere beliebte Option in derselben Kategorie. Sowohl Istio als auch Nginx fungieren als Dienstnetz und bieten zusätzliche Features wie Lastenausgleich, Routing und Ratensteuerung.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../../security/fundamentals/overview.md
