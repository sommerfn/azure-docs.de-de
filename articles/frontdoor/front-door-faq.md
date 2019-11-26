---
title: Azure Front Door Service – Häufig gestellte Fragen zu Front Door | Microsoft-Dokumentation
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Azure Front Door Service.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2019
ms.author: sharadag
ms.openlocfilehash: 4001b3ed8d7a92d8a751486ab4fd3a097d032373
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809736"
---
# <a name="frequently-asked-questions-for-azure-front-door-service"></a>Häufig gestellte Fragen zu Azure Front Door Service

In diesem Artikel werden häufige Fragen zu den Features und der Funktionalität von Azure Front Door Service beantwortet. Wenn Sie hier keine Antwort auf Ihre Frage finden, können Sie sich über die folgenden Kanäle an uns wenden (Eskalationsreihenfolge):

1. Im Abschnitt „Kommentare“ dieses Artikels.
2. [Azure Front Door Service UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Microsoft-Support:** Wählen Sie zum Erstellen einer neuen Supportanfrage im Azure-Portal auf der Registerkarte **Hilfe** die Schaltfläche **Hilfe und Support** und anschließend die Option **Neue Supportanfrage**.

## <a name="general"></a>Allgemein

### <a name="what-is-azure-front-door-service"></a>Was ist Azure Front Door Service?

Azure Front Door Service ist ein ADN (Application Delivery Network) als Dienst und bietet verschiedene Lastenausgleichsfunktionen auf Schicht 7 für Ihre Anwendung. Es bietet die Beschleunigung dynamischer Websites (Dynamic Site Acceleration, DSA) zusammen mit globalem Lastenausgleich mit Failover nahezu in Echtzeit. Es ist ein hoch verfügbarer und skalierbarer Dienst, der vollständig von Azure verwaltet wird.

### <a name="what-features-does-azure-front-door-service-support"></a>Welche Features werden von Azure Front Door Service unterstützt?

Azure Front Door Service unterstützt die Beschleunigung dynamischer Websites (Dynamic Site Acceleration, DSA), SSL-Abladung und End-to-End-SSL, Web Application Firewall, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis, kostenfreie Zertifikate und Verwaltung mehrerer Domänen sowie weitere Features. Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Azure Front Door Service](front-door-overview.md).

### <a name="what-is-the-difference-between-azure-front-door-service-and-azure-application-gateway"></a>Was ist der Unterschied zwischen Azure Front Door Service und Azure Application Gateway?

Obwohl sowohl Front Door als auch Application Gateway einen Lastenausgleich der Schicht 7 (HTTP/HTTPS) darstellt, besteht der Hauptunterschied darin, dass Front Door ein globaler und Application Gateway ein regionaler Dienst ist. Während Front Door den Lastenausgleich zwischen den verschiedenen Skalierungseinheiten, Clustern und Stempeleinheiten über Regionen hinweg ermöglicht, gestattet Application Gateway den Lastenausgleich zwischen VMs/Containern usw., die sich innerhalb der Skalierungseinheit befinden.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Wann sollte ein Application Gateway hinter Front Door bereitgestellt werden?

Wichtige Szenarien, in denen Application Gateway hinter Front Door verwendet werden sollte:

- Front Door kann einen pfadbasierten Lastenausgleich nur auf globaler Ebene durchführen, aber wenn der Datenverkehr in ihrem virtuellen Netzwerk (VNET) noch weiter ausgeglichen werden soll, dann sollte Application Gateway verwendet werden.
- Da Front Door auf VM-/Containerebene nicht funktioniert, kann es keinen Verbindungsausgleich durchführen. Mit Application Gateway können Sie jedoch den Verbindungsausgleich durchführen. 
- Mit einem Application Gateway hinter Azure Front Door Service (AFD) können Sie 100 % SSL-Abladung erreichen und nur HTTP-Anforderungen in ihrem virtuellen Netzwerk (VNET) weiterleiten.
- Sowohl Front Door als auch Application Gateway unterstützen die Sitzungsaffinität. Während Front Door den nachfolgenden Datenverkehr von einer Benutzersitzung auf denselben Cluster oder Back-End in einer bestimmten Region leiten kann, kann Application Gateway den Datenverkehr direkt auf demselben Server innerhalb des Clusters zuordnen.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Kann Azure Load Balancer hinter Front Door bereitgestellt werden?

Azure Front Door Service benötigt einen öffentlichen VIP oder einen öffentlich verfügbaren DNS-Namen, um den Datenverkehr zu weiterzuleiten. Die Bereitstellung von Azure Load Balancer hinter Front Door ist ein häufiger Anwendungsfall.

### <a name="what-protocols-does-azure-front-door-service-support"></a>Welche Protokolle werden von Azure Front Door Service unterstützt?

Azure Front Door Service unterstützt HTTP, HTTPS und HTTP/2.

### <a name="how-does-azure-front-door-service-support-http2"></a>Wie unterstützt Azure Front Door Service HTTP/2?

Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Azure Front Door Service verbunden sind. Die Kommunikation mit den Back-Ends im Back-End-Pool erfolgt über HTTP/1.1. Die HTTP/2-Unterstützung ist standardmäßig aktiviert.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Welche Ressourcen werden derzeit als Teil des Back-End-Pools unterstützt?

Back-End-Pools können aus Storage, Web App, Kubernetes-Instanzen oder einem beliebigen anderen benutzerdefinierten Hostnamen mit öffentlicher Konnektivität bestehen. Azure Front Door Service erfordert, dass die Back-Ends entweder über eine öffentliche IP-Adresse oder einen öffentlich auflösbaren DNS-Hostnamen definiert werden. Mitglieder von Back-End-Pools können auf Zonen, Regionen oder sogar außerhalb von Azure verteilt sein oder sich außerhalb von Azure befinden, sofern sie über eine öffentliche Konnektivität verfügen.

### <a name="what-regions-is-the-service-available-in"></a>In welchen Regionen ist der Dienst verfügbar?

Azure Front Door Service ist ein globaler und nicht an eine bestimmte Azure-Region gebundener Dienst. Der einzige Speicherort, den Sie beim Erstellen einer Front Door-Instanz angeben müssen, ist der Speicherort der Ressourcengruppe, der im Wesentlichen angibt, wo die Metadaten für die Ressourcengruppe gespeichert werden. Die Front Door-Ressource selbst wird als globale Ressource erstellt und die Konfiguration wird global für alle POPs (Point of Presence) bereitgestellt. 

### <a name="what-are-the-pop-locations-for-azure-front-door-service"></a>Welche POP-Standorte sind für Azure Front Door Service verfügbar?

Azure Front Door Service hat dieselbe Liste von POP-Standorten (Point of Presence) wie Azure CDN von Microsoft. Die vollständige Liste unserer POPs finden Sie unter [Azure CDN-POP-Standorte von Microsoft](https://docs.microsoft.com/azure/cdn/cdn-pop-locations).

### <a name="is-azure-front-door-service-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Ist Azure Front Door Service eine dedizierte Bereitstellung für meine Anwendung, oder wird es zur gemeinsamen Nutzung für Kunden freigegeben?

Azure Front Door Service ist ein global verteilter, mehrinstanzenfähiger Dienst. Die Infrastruktur für Front Door wird somit von allen Kunden gemeinsam genutzt. Wenn Sie jedoch ein Front Door-Profil erstellen, definieren Sie die für Ihre Anwendung erforderliche spezifische Konfiguration, und Änderungen an Ihrer Front Door-Ressource wirken sich nicht auf andere Front Door-Konfigurationen aus.

### <a name="is-http-https-redirection-supported"></a>Wird die Umleitung von HTTP zu HTTPS unterstützt?

Ja. Tatsächlich unterstützt Azure Front Door Service Host-, Pfad- und Abfragezeichenfolgenumleitung sowie einen Teil der URL-Umleitung. Weitere Informationen zur [URL-Umleitung](front-door-url-redirect.md). 

### <a name="in-what-order-are-routing-rules-processed"></a>In welcher Reihenfolge werden Routingregeln verarbeitet?

Die Routen für Front Door sind nicht sortiert und eine bestimmte Route wird anhand der besten Übereinstimmung ausgewählt. Weitere Informationen zu [Abgleich von Anforderungen mit Routingregeln durch Front Door](front-door-route-matching.md).

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door"></a>Wie kann ich den Zugriff auf mein Back-End nur auf Azure Front Door beschränken?

Um Ihre Anwendung so zu sperren, dass sie nur Datenverkehr von Ihrer spezifischen Front Door-Ressource akzeptiert, müssen Sie IP-ACLs für Ihr Back-End einrichten und dann den Satz der akzeptierten Werte auf den Header „X-Forwarded-Host“ einschränken, der von Azure Front Door gesendet wird. Diese Schritte werden wie folgt beschrieben:

- Konfigurieren Sie IP-ACLs für Ihre Back-Ends so, dass sie nur Datenverkehr aus dem Back-End-IP-Adressraum von Azure Front Door und den Infrastrukturdiensten von Azure akzeptieren. Wir arbeiten an der Integration in [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519), aber im Moment können Sie auf die IP-Bereiche wie folgt verweisen:
 
    - **IPv4**-Back-End-IP-Adressraum von Front Door: `147.243.0.0/16`
    - **IPv6**-Back-End-IP-Adressraum von Front Door: `2a01:111:2050::/44`
    - [Grundlegenden Infrastrukturdienste](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) von Azure über virtualisierte IP-Hostadressen: `168.63.129.16` und `169.254.169.254`

    > [!WARNING]
    > Der Back-End-IP-Adressbereich von Front Door kann später geändert werden, aber wir werden sicherstellen, dass zuvor eine Integration in [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) erfolgt ist. Es wird empfohlen, dass Sie [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) für alle Änderungen oder Updates abonnieren.

-   Filtern Sie auf die Werte für den eingehenden Header „**X-Forwarded-Host**“, der von Front Door gesendet wird. Die einzigen zulässigen Werte für den Header sollten alle Front-End-Hosts sein, die in Ihrer Front Door-Konfiguration definiert werden. Noch spezifischer: nur die Hostnamen, von denen Sie Datenverkehr für dieses spezielle Back-End akzeptieren möchten.
    - Beispiel: Nehmen wir an, dass Ihre Front Door-Konfiguration die folgenden Front-End-Hosts _`contoso.azurefd.net`_ (A), _`www.contoso.com`_ (B), _ (C) und _`notifications.contoso.com`_ (D) aufweist. Es werden zwei Back-Ends X und Y verwendet. 
    - Back-End X soll nur Datenverkehr von den Hostnamen A und B annehmen. Back-End Y kann Datenverkehr von A, C und D akzeptieren.
    - Daher sollten Sie auf Back-End X nur Datenverkehr akzeptieren, bei dem der Header „**X-Forwarded-Host**“ auf _`contoso.azurefd.net`_ oder auf _`www.contoso.com`_ festgelegt ist. In allen anderen Fällen sollte Back-End X den Datenverkehr ablehnen.
    - Analog dazu sollten Sie auf Back-End Y nur Datenverkehr akzeptieren, bei dem der Header „**X-Forwarded-Host**“ auf _`contoso.azurefd.net`_ , auf _`api.contoso.com`_ oder auf _`notifications.contoso.com`_ festgelegt ist. In allen anderen Fällen sollte Back-End Y den Datenverkehr ablehnen.

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Kann sich die Anycast-IP-Adresse während der Lebensdauer meiner Front Door-Instanz ändern?

Die Front-End-Anycast-IP-Adresse für Ihre Front Door-Instanz sollte sich in der Regel nicht ändern und kann während der Front Door-Lebensdauer statisch bleiben. Es gibt hierfür jedoch **keine Garantien**. Es wird empfohlen, keine direkten Abhängigkeiten von der IP-Adresse herzustellen.

### <a name="does-azure-front-door-service-support-static-or-dedicated-ips"></a>Unterstützt Azure Front Door Service statische oder dedizierte IP-Adressen?

Azure Front Door Service unterstützt derzeit keine statischen oder dedizierten Front-End-Anycast-IP-Adressen. 

### <a name="does-azure-front-door-service-support-x-forwarded-for-headers"></a>Unterstützt Azure Front Door Service X-Forwarded-For-Header?

Ja. Azure Front Door Service unterstützt die Header „X-Forwarded-For“, „X-Forwarded-Host“ und „X-Forwarded-Proto“. Wenn der Header für „X-Forwarded-For“ bereits vorhanden war, fügt Front Door die Client-Socket-IP-Adresse an diesen an. Andernfalls wird der Header mit der Client-Socket-IP-Adresse als Wert hinzugefügt. Für X-Forwarded-Host und X-Forwarded-Proto wird der Wert außer Kraft gesetzt.

Weitere Informationen zu [von Front Door unterstützten HTTP-Headern](front-door-http-headers-protocol.md).  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-service-does-my-front-door-still-work-when-being-updated"></a>Wie lange dauert die Bereitstellung von Azure Front Door Service? Funktioniert meine Front Door-Instanz auch bei einem Update weiterhin?

Eine neue Front Door-Erstellung oder jegliche Updates für eine bestehende Front Door-Instanz benötigen ungefähr drei bis fünf Minuten für die globale Bereitstellung. Das bedeutet, dass Ihre Front Door-Konfiguration in ungefähr drei bis fünf Minuten global auf allen unseren POPs bereitgestellt wird.

Hinweis: Benutzerdefinierte SSL-Zertifikatupdates benötigen etwa 30 Minuten für die globale Bereitstellung.

## <a name="configuration"></a>Konfiguration

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Kann Azure Front Door für den Datenverkehr in einem virtuellen Netzwerk einen Lastenausgleich vornehmen oder den Datenverkehr weiterleiten?

Azure Front Door (AFD) erfordert eine öffentliche IP-Adresse oder einen öffentlich auflösbaren DNS-Namen, um den Datenverkehr weiterzuleiten. Die Antwort ist somit, dass AFD den Datenverkehr in einem virtuellen Netzwerk nicht direkt weiterleiten kann, aber die zwischenzeitliche Verwendung von Application Gateway oder Azure Load Balancer führt in diesem Szenario zu einer Lösung.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door-service"></a>Welche Timeouts und Grenzwerte gelten für Azure Front Door Service?

Weitere Informationen zu allen dokumentierten [Timeouts und Grenzwerten für Azure Front Door Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-front-door-service-limits).

## <a name="performance"></a>Leistung

### <a name="how-does-azure-front-door-service-support-high-availability-and-scalability"></a>Wie unterstützt Azure Front Door Service die Hochverfügbarkeit und Skalierbarkeit?

Azure Front Door Service ist eine global verteilte mehrinstanzenfähige Plattform mit riesigem Kapazitätsvolumen, um die Skalierungsanforderungen Ihrer Anwendung zu erfüllen. Front Door wird vom Rand des globalen Netzwerks von Microsoft bereitgestellt und bietet globale Funktionen für den Lastenausgleich, mit denen Sie für Ihre gesamte Anwendung oder sogar für einzelne Microservices über Regionen oder verschiedene Clouds hinweg einen Failover ausführen können.

## <a name="ssl-configuration"></a>SSL-Konfiguration

### <a name="what-tls-versions-are-supported-by-azure-front-door-service"></a>Welche TLS-Versionen werden vom Azure Front Door Service unterstützt?

Alle Front Door-Profile, die nach September 2019 erstellt wurden, verwenden standardmäßig TLS 1.2 als Mindestvoraussetzung.

Front Door unterstützt die TLS-Versionen 1.0, 1.1 und 1.2. TLS 1.3 wird noch nicht unterstützt.

### <a name="what-certificates-are-supported-on-azure-front-door-service"></a>Welche Zertifikate werden von Azure Front Door Service unterstützt?

Um das HTTPS-Protokoll für die sichere Bereitstellung von Inhalten in einer benutzerdefinierten Front Door-Domäne zu aktivieren, können Sie ein von Azure Front Door Service verwaltetes oder Ihr eigenes Zertifikat verwenden.
Die von Front Door verwaltete Option stellt ein SSL-Standardzertifikat über Digicert zur Verfügung und wird im Schlüsseltresor von Front Door gespeichert. Wenn Sie Ihr eigenes Zertifikat verwenden, können Sie ein Zertifikat einer unterstützten Zertifizierungsstelle integrieren, bei dem es sich um ein Standard-SSL-, erweitertes Validierungs- oder sogar um ein Platzhalterzertifikat handeln kann. Selbstsignierte Zertifikate werden nicht unterstützt. Weitere Informationen zu [Aktivieren von HTTPS für eine benutzerdefinierte Domäne](https://aka.ms/FrontDoorCustomDomainHTTPS).

### <a name="does-front-door-support-autorotation-of-certificates"></a>Unterstützt Front Door die automatische Rotation von Zertifikaten?

Bei der Option für verwaltete Zertifikate von Front Door werden die Zertifikate von Front Door automatisch rotiert. Wenn Sie ein verwaltetes Zertifikat von Front Door verwenden und feststellen, dass das Ablaufdatum des Zertifikats weniger als 60 Tage entfernt ist, erstellen Sie ein Supportticket.
</br>Für Ihr eigenes benutzerdefiniertes SSL-Zertifikat wird automatische Rotation nicht unterstützt. Ähnlich wie bei der erstmaligen Einrichtung für eine bestimmte benutzerdefinierte Domäne müssen Sie Front Door auf die richtige Zertifikatversion in Ihrem Key Vault verweisen und sicherstellen, dass der Dienstprinzipal für Front Door noch Zugriff auf den Key Vault besitzt. Dieser aktualisierte Zertifikatrolloutvorgang von Front Door ist atomisch und verursacht keine Produktionsauswirkungen, sofern sich der Antragstellername oder der alternative Antragstellername (SAN) für das Zertifikat nicht ändert.

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door-service"></a>Welche Verschlüsselungssammlungen werden derzeit von Azure Front Door Service unterstützt?

Die folgenden Verschlüsselungssammlungen werden derzeit von Azure Front Door Service unterstützt:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA

### <a name="does-azure-front-door-service-also-support-re-encryption-of-traffic-to-the-backend"></a>Unterstützt Azure Front Door Service auch eine erneute Verschlüsselung des Datenverkehrs an das Back-End?

Ja. Azure Front Door Service unterstützt SSL-Abladung sowie End-to-End-SSL, das den Datenverkehr an das Back-End erneut verschlüsselt. Da die Verbindungen zum Back-End über dessen öffentliche IP-Adresse erfolgen, wird empfohlen, dass Sie Front Door so konfigurieren, dass HTTPS als Weiterleitungsprotokoll verwendet wird.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>Kann ich die SSL-Richtlinie für die Steuerung von SSL-Protokollversionen konfigurieren?

Sie können eine minimale TLS-Version in Azure Front Door über die [Azure REST-API](https://docs.microsoft.com/rest/api/frontdoorservice/frontdoor/frontdoors/createorupdate#minimumtlsversion) konfigurieren. Derzeit können Sie zwischen 1.0 und 1.2 wählen.

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Kann ich Front Door so konfigurieren, dass nur bestimmte Verschlüsselungssammlungen unterstützt werden?

Nein. Die Konfiguration von Front Door für bestimmte Verschlüsselungssammlungen wird nicht unterstützt. 

## <a name="diagnostics-and-logging"></a>Diagnose und Protokollierung

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door-service"></a>Welche Arten von Metriken und Protokollen sind mit Azure Front Door Service verfügbar?

Informationen zu Protokollen und anderen Diagnosefunktionen finden Sie unter [Überwachen von Metriken und Protokollen für Front Door](front-door-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Welche Aufbewahrungsrichtlinie gilt für die Diagnoseprotokolle?

Diagnoseprotokolle gelangen in die Speicherkonten von Kunden, und Kunden können die Aufbewahrungsrichtlinie je nach Präferenz festlegen. Diagnoseprotokolle können auch an einen Event Hub oder Azure Monitor-Protokolle gesendet werden. Weitere Informationen finden Sie unter [Azure Front Door Service-Diagnose](front-door-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door-service"></a>Wie erhalte ich Überwachungsprotokolle für Azure Front Door Service?

Überwachungsprotokolle sind für Azure Front Door Service verfügbar. Klicken Sie im Portal auf dem Menüblatt Ihrer Front Door-Instanz auf **Aktivitätsprotokoll**, um das Überwachungsprotokoll aufzurufen. 

### <a name="can-i-set-alerts-with-azure-front-door-service"></a>Kann ich mit Azure Front Door Service Warnungen festlegen?

Ja. Azure Front Door Service unterstützt Warnungen. Warnungen werden für Metriken konfiguriert. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
