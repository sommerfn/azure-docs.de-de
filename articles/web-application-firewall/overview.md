---
title: Einführung zu Azure Web Application Firewall
description: Dieser Artikel enthält eine Übersicht über die Azure Web Application Firewall (WAF)
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 10/04/2019
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 7b43a6bdac254493da8693b55158e15746e76dc3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488286"
---
# <a name="what-is-azure-web-application-firewall"></a>Was ist die Azure Web Application Firewall?

Die Web Application Firewall (WAF) bietet zentralisierten Schutz Ihrer Webanwendungen vor verbreiteten Exploits und Sicherheitsrisiken. Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Einschleusung von SQL-Befehlen und websiteübergreifendes Skripting gehören zu den häufigsten Angriffen.

![WAF-Übersicht](media/overview/wafoverview.png)

Es ist schwierig, solche Angriffe im Anwendungscode zu verhindern. Hierzu können rigorose Wartungs-, Patch- und Überwachungsmaßnahmen auf mehreren Ebenen der Anwendungstopologie erforderlich sein. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich. Eine WAF bietet auch Anwendungsadministratoren eine bessere Gewähr des Schutzes vor Bedrohungen und Angriffen.

Mit einer WAF-Lösung können Sie schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede einzelne Webanwendung separat zu sichern.

WAF kann mit Azure Application Gateway und Azure Front Door Service bereitgestellt werden. Zurzeit verfügt WAF über Features, die für jeden spezifischen Dienst angepasst werden. Weitere Informationen zu den WAF-Features für die einzelnen Dienste finden Sie in der Übersicht zu den einzelnen Diensten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Web Application Firewall auf Application Gateway finden Sie unter [Web Application Firewall auf Azure Application Gateway](./ag/ag-overview.md).
- Weitere Informationen zu Web Application Firewall auf Azure Front Door Service finden Sie unter [Web Application Firewall auf Azure Front Door Service](./afds/afds-overview.md).
