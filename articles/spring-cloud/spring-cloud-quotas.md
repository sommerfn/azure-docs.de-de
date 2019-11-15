---
title: Servicepläne und Dienstkontingente für Azure Spring Cloud
description: Erfahren Sie mehr über Dienstkontingente und Servicepläne für Azure Spring Cloud.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: 41a2b1a7d9aa5089ba2ee73cd3c5c5c5e31f5225
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607668"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Kontingente und Servicepläne für Azure Spring Cloud

Für die Ressourcen und Funktionen aller Azure-Dienste gelten festgelegte Standardlimits und -kontingente.  Während der Vorschauphase bietet Azure Spring Cloud nur einen Serviceplan.

In diesem Artikel werden die während der aktuellen Vorschauphase angebotenen Dienstkontingente ausführlich erläutert.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Dienstebenen und -kontingente für Azure Spring Cloud

Während der Vorschauphase bietet Azure Spring Cloud nur eine Dienstebene.

Resource | Amount
------- | -------
vCPU | 4
Arbeitsspeicher | 8 GB
Azure Spring Cloud-Abonnement | 1
Azure Spring Cloud-Dienstinstanzen pro Region und Abonnement | 2
Gesamtanzahl der App-Instanzen pro Azure Spring Cloud-Dienstinstanz | 50
Gesamtanzahl der App-Instanzen pro Spring-Anwendung | 20
Persistente Volumes | 10 × 50 GB

Wenn Sie ein Kontingent erreichen, erhalten Sie einen 400-Fehler mit folgendem Inhalt: „Das Kontingent überschreitet den Grenzwert für das Abonnement *Ihr Abonnement* in der Region *Region, in der Ihr Azure Spring Cloud-Dienst erstellt wurde*.“

## <a name="next-steps"></a>Nächste Schritte

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn für Ihre Ressource eine Erhöhung erforderlich ist, [erstellen Sie eine Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).
