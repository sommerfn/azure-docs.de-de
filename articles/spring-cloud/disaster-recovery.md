---
title: Georedundante Notfallwiederherstellung in Azure Spring Cloud | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Spring Cloud-Anwendung vor regionalen Ausfällen schützen.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jeconnoc
ms.openlocfilehash: 54289e808461e6374dee57763ef7ba0d0429c54c
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607849"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Notfallwiederherstellung in Azure Spring Cloud

In diesem Artikel werden einige Strategien erläutert, mit denen Sie Ihre Azure Spring Cloud-Anwendungen vor Ausfallzeiten schützen können.  In jeder Region und jedem Rechenzentrum können Ausfallzeiten aufgrund von regionalen Ausfällen auftreten, durch eine sorgfältige Planung können jedoch die Auswirkungen auf Ihre Kunden abgeschwächt werden.

## <a name="plan-your-application-deployment"></a>Planen Ihrer Anwendungsbereitstellung

Azure Spring Cloud-Anwendungen werden in einer spezifischen Region ausgeführt.  Azure betreibt Datencenter in mehreren Gebieten auf der ganzen Erde. Bei einem Azure-Gebiet handelt es sich um einen definierten Bereich der Erde, der mindestens eine Azure-Region enthält. Eine Azure-Region ist ein Bereich in einem Gebiet mit mindestens einem Rechenzentrum.  Jeder Azure-Region ist innerhalb des gleichen geografischen Gebiets eine andere Region als Regionspartner zugeordnet. In Azure werden Plattformupdates (geplante Wartung) für Regionspaare serialisiert, um sicherzustellen, dass immer nur jeweils eine Region pro Paar aktualisiert wird. Bei einem Ausfall, der sich auf mehrere Regionen auswirkt, wird mindestens eine Region in jedem Paar für die Wiederherstellung priorisiert.

Zur Sicherstellung von Hochverfügbarkeit und Schutz vor Ausfällen müssen Sie Ihre Spring Cloud-Anwendungen in mehreren Regionen bereitstellen.  Azure umfasst eine Liste mit [Regionspaaren](../best-practices-availability-paired-regions.md), sodass Sie Spring Cloud-Bereitstellungen mit Regionspaaren planen können.  Es empfiehlt sich, beim Entwurf Ihrer Microservicearchitektur drei wichtige Faktoren zu berücksichtigen: regionale Verfügbarkeit, Azure-Regionspaare und Dienstverfügbarkeit.

*  Regionale Verfügbarkeit:  Wählen Sie einen geografischen Bereich in der Nähe Ihrer Benutzer aus, um die Netzwerkverzögerung und Übertragungszeit zu minimieren.
*  Azure-Regionspaare:  Wählen Sie Regionspaare im ausgewählten geografischen Bereich aus, sodass koordinierte Plattformupdates und priorisierte Wiederherstellungsvorgänge bei Bedarf sichergestellt sind.
*  Dienstverfügbarkeit:   Legen Sie fest, ob die Regionspaare heiß/heiß, heiß/warm oder heiß/kalt sein sollen.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Weiterleiten von Datenverkehr mit Azure Traffic Manager

[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) gewährleistet einen DNS-basierten Lastausgleich des Datenverkehrs und kann Netzwerkdatenverkehr auf mehrere Regionen verteilen.  Verwenden Sie Azure Traffic Manager, um Kunden an die nächstgelegene Azure Spring Cloud-Dienstinstanz weiterzuleiten.  Optimale Leistungs- und Redundanzwerte werden erzielt, indem Sie den gesamten Anwendungsdatenverkehr über Azure Traffic Manager leiten, bevor er an den Azure Spring Cloud-Dienst gesendet wird.

Wenn Sie Azure Spring Cloud-Anwendungen in mehreren Regionen festgelegt haben, können Sie über Azure Traffic Manager den Datenverkehrsfluss in die Anwendungen in den einzelnen Regionen steuern.  Definieren Sie einen Azure Traffic Manager-Endpunkt für jeden Dienst unter Verwendung der Dienst-IP-Adresse. Kunden sollten eine Verbindung mit einem DNS-Namen in Azure Traffic Manager herstellen, der auf den Azure Spring Cloud-Dienst verweist.  Mit Azure Traffic Manager wird ein Lastausgleich des Datenverkehrs über die definierten Endpunkte vorgenommen.  Bei einem Ausfall eines Rechenzentrums leitet Azure Traffic Manager den Datenverkehr von dieser Region an die zugehörige andere Region des Regionspaars weiter, um die Dienstkontinuität sicherzustellen.