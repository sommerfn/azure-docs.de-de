---
title: Häufig gestellte Fragen (FAQ) zu Azure Private Link
description: Erfahren Sie mehr über Azure Private Link.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 32e570e098c9a3f95dfc5393348cccb1ebe1fb60
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104585"
---
# <a name="azure-private-link-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Azure Private Link

## <a name="private-link"></a>Private Link

### <a name="what-is-azure-private-endpoint-and-azure-private-link-service"></a>Was sind der private Endpunkt in Azure und der Azure Private Link-Dienst?

- **[Privater Endpunkt in Azure](private-endpoint-overview.md)** : Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. Sie können private Endpunkte verwenden, um eine Verbindung mit einem Private Link unterstützenden Azure PaaS-Dienst oder mit Ihrem eigenen Private Link-Dienst herzustellen.
- **[Azure Private Link-Dienst](private-link-service-overview.md)** : Der Azure Private Link-Dienst ist ein von einem Dienstanbieter erstellter Dienst. Derzeit kann ein Private Link-Dienst an die Front-End-IP-Konfiguration eines Load Balancer Standard angefügt werden. 

### <a name="how-is-traffic-being-sent-when-using-private-link"></a>Wie wird der Datenverkehr bei der Verwendung von Private Link gesendet?
Der Datenverkehr wird privat über den Microsoft-Backbone übertragen. Er gelangt nicht ins Internet.  
 
### <a name="what-is-the-difference-between-a-service-endpoints-and-a-private-endpoints"></a>Worin besteht der Unterschied zwischen einem Dienstendpunkt und einem privaten Endpunkt?
- Bei der Verwendung von privaten Endpunkten wird der Netzwerkzugriff auf bestimmte Ressourcen hinter einem bestimmten Dienst gewährt, der eine differenzierte Segmentierung ermöglicht. Außerdem kann der Datenverkehr lokal auf die Dienstressource zugreifen, ohne öffentliche Endpunkte zu verwenden.

### <a name="what-is-the-relationship-between-private-link-service-and-private-endpoint"></a>Welche Beziehung besteht zwischen dem Private Link-Dienst und dem privaten Endpunkt?
Der private Endpunkt bietet Zugriff auf mehrere Ressourcentypen für private Links, einschließlich Azure PaaS-Dienste und Ihren eigenen Private Link-Dienst. Es handelt sich um eine 1:n-Beziehung. Ein Private Link-Dienst kann Verbindungen von mehreren privaten Endpunkten empfangen. Andererseits kann ein privater Endpunkt nur eine Verbindung mit einem Private Link-Dienst herstellen.    

## <a name="private-endpoint"></a>Privater Endpunkt 
 
### <a name="can-i-create-multiple-private-endpoints-in-same-vnet-can-they-connect-to-different-services"></a>Kann ich mehrere private Endpunkte in demselben VNet erstellen? Können sie Verbindungen zu anderen Diensten herstellen? 
Ja. Sie können mehrere private Endpunkte in demselben VNet oder Subnetz verwenden. Sie können Verbindungen zu anderen Diensten herstellen.  
 
### <a name="do-i-require-a-dedicated-subnet-for-private-endpoints"></a>Benötige ich ein dediziertes Subnetz für private Endpunkte? 
Nein. Sie benötigen kein dediziertes Subnetz für private Endpunkte. Sie können eine private Endpunkt-IP aus einem beliebigen Subnetz des VNet auswählen, in dem Ihr Dienst bereitgestellt wird.  
 
### <a name="can-private-endpoint-connect-to-private-link-service-across-azure-active-directory-tenants"></a>Kann der private Endpunkt eine Verbindung mit dem Private Link-Dienst über Azure Active Directory-Mandanten hinweg herstellen? 
Ja. Private Endpunkte können über AD-Mandanten eine Verbindung mit Private Link-Diensten oder mit Azure PaaS herstellen.  
 
### <a name="can-private-endpoint-connect-to-azure-paas-resources-across-azure-regions"></a>Können private Endpunkte eine Verbindung mit Azure PaaS-Ressourcen zwischen Azure-Regionen herstellen?
Ja. Private Endpunkte können eine Verbindung mit Azure PaaS-Ressourcen zwischen Azure-Regionen herstellen.

## <a name="private-link-service"></a>Private Link-Dienst
 
### <a name="what-are-the-pre-requisites-for-creating-a-private-link-service"></a>Welche Voraussetzungen müssen gegeben sein, um einen Private Link-Dienst zu erstellen? 
Ihre Dienst-Back-Ends sollten sich in einem virtuellen Netzwerk und hinter einem Load Balancer Standard befinden.
 
### <a name="how-can-i-scale-my-private-link-service"></a>Wie kann ich meinen Private Link-Dienst skalieren? 
Sie können Ihren Private Link-Dienst auf verschiedene Arten skalieren: 
- Fügen Sie dem Pool hinter Ihrem Load Balancer Standard Back-End-VMs hinzu. 
- Fügen Sie dem Private Link-Dienst eine IP-Adresse hinzu. Es sind bis zu acht IP-Adressen pro Private Link-Dienst gestattet.  
- Fügen Sie dem Load Balancer Standard einen neuen Private Link-Dienst hinzu. Es sind bis zu acht Private Link-Dienste pro Load Balancer Standard gestattet.   

### <a name="can-i-connect-my-service-to-multiple-private-endpoints"></a>Kann ich meinen Dienst mit mehreren privaten Endpunkten verbinden?
Ja. Ein Private Link-Dienst kann Verbindungen von mehreren privaten Endpunkten empfangen. Ein privater Endpunkt kann jedoch nur eine Verbindung mit einem Private Link-Dienst herstellen.  
 
### <a name="how-should-i-control-the-exposure-of-my-private-link-service"></a>Wie sollte ich die Offenlegung meines Private Link-Dienstes kontrollieren?
Sie können die Offenlegung über die Sichtbarkeitskonfiguration des Private Link-Dienstes kontrollieren. Die Sichtbarkeit unterstützt drei Einstellungen:

- **Keine**: Nur Abonnements mit RBAC-Zugriff können den Dienst finden. 
- **Restriktiv**: Nur in der Whitelist aufgeführte Abonnements, die über RBAC-Zugriff verfügen, können den Dienst finden. 
- **Alle**: Jeder kann den Dienst finden. 
 
### <a name="can-i-create-a-private-link-service-with-basic-load-balancer"></a>Kann ich mit einem Load Balancer im Tarif „Basic“ einen Private Link-Dienst erstellen? 
Nein. Der Private Link-Dienst über einen Load Balancer im Tarif „Basic“ wird nicht unterstützt.
 
### <a name="is-a-dedicated-subnet-required-for-private-link-service"></a>Ist ein dediziertes Subnetz für den Private Link-Dienst erforderlich? 
Nein. Der Private Link-Dienst erfordert kein dediziertes Subnetz. Sie können ein beliebiges Subnetz in Ihrem VNet auswählen, in dem Ihr Dienst bereitgestellt wird.   

### <a name="i-am-a-service-provider-using-azure-private-link-do-i-need-to-make-sure-all-my-customers-have-unique-ip-space-and-dont-overlap-with-my-ip-space"></a>Ich bin ein Dienstanbieter, der Azure Private Link verwendet. Muss ich sicherstellen, dass alle meine Kunden über einen eindeutigen IP-Bereich verfügen und sich nicht mit meinem IP-Bereich überschneiden? 
Nein. Azure Private Link stellt Ihnen diese Funktionen zur Verfügung. Daher ist es nicht erforderlich, dass Sie über einen Adressraum verfügen, der nicht mit dem Adressraum Ihrer Kunden überlappt. 

##  <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Private Link](private-link-overview.md).
