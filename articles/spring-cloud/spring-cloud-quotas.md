---
title: Servicepläne und Dienstkontingente für Azure Spring Cloud
description: Erfahren Sie mehr über Dienstkontingente und Servicepläne für Azure Spring Cloud.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 9/27/2019
ms.author: jeconnoc
ms.openlocfilehash: 89934ee0ab0c901a904a1a5ac2fb620185571a23
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038685"
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

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn für Ihre Ressource eine Erhöhung erforderlich ist, übermitteln Sie uns eine Anfrage: azure-spring-cloud@service.microsoft.com.
