---
title: Neuerungen in Azure Application Gateway
description: Enthält Informationen zu den Neuerungen in Azure Application Gateway, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: b2e7b9f28c84b769fe64be82f91418cc2a7d634f
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044372"
---
# <a name="whats-new-in-azure-application-gateway"></a>Neuerungen in Azure Application Gateway

Azure Application Gateway wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen

## <a name="new-features"></a>Neue Funktionen

|Feature  |BESCHREIBUNG  |Hinzufügedatum  |
|---------|---------|---------|
|Verbesserungen der Tests |Mit den Verbesserungen benutzerdefinierter Tests in der Application Gateway v2-SKU wurden [Testkonfigurationen](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#create-probe-for-application-gateway-v2-sku) und [bedarfsgesteuerte Back-End-Integritätstests](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#test-backend-health-with-the-probe) vereinfacht und [mehr Diagnoseinformationen](https://docs.microsoft.com/azure/application-gateway/application-gateway-backend-health-troubleshooting#error-messages) hinzugefügt, um Sie bei der Behandlung von Problemen mit der Back-End-Integrität zu unterstützen.  |Oktober 2019 |
|Weitere Metriken |Wir haben die folgenden neuen Metriken hinzugefügt, mit denen Sie Ihre Application Gateway v2-SKU überwachen können: [„Zeitmetriken“](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#timing-metrics), „Back-End-Antwortstatus“, „Empfangene Bytes“, „Gesendete Bytes“, „Client-TLS-Protokoll“ und „Aktuelle Compute-Einheiten“. Informationen finden Sie unter [Von der Application Gateway V2-SKU unterstützte Metriken](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-supported-by-application-gateway-v2-sku). |August 2019 |
|Benutzerdefinierte WAF-Regeln |Application Gateway WAF_v2 unterstützt nun das Erstellen von benutzerdefinierten Regeln. Siehe [Custom rules for Web Application Firewall v2](custom-waf-rules-overview.md) (Benutzerdefinierte Regeln für Web Application Firewall v2) |Juni 2019 |
|Automatische Skalierung, Zonenredundanz, Unterstützung von statischen VIPs (allgemeine Verfügbarkeit) |Allgemeine Verfügbarkeit für v2 SKU, die automatische Skalierung, Zonenredundanz, Verbesserung der Leistung, statische VIPs, Schlüsseltresor und Neugenerierung von Headern unterstützt. Siehe die [Dokumentation zur automatischen Skalierung per Application Gateway](application-gateway-autoscaling-zone-redundant.md). |April 2019 |
|Schlüsseltresor-Integration |Application Gateway unterstützt jetzt die Integration mit Key Vault (in der öffentlichen Vorschau) für Serverzertifikate, die an HTTPS-fähige Listener angefügt sind. Siehe [SSL-Terminierung mit Key Vault-Zertifikaten](key-vault-certs.md). |April 2019 |
|CRUD/Umschreibungen von Headern     |Sie können die HTTP-Header jetzt umschreiben. Eine Schritt-für-Schritt-Anleitung finden Sie im [Tutorial: Erstellen eines Application Gateways und erneutes Generieren eines HTTP-Headers](tutorial-http-header-rewrite-powershell.md).|Dezember 2018|
|WAF-Konfiguration und Ausschlussliste     |Wir haben weitere Optionen hinzugefügt, die Ihnen als Hilfe beim Konfigurieren Ihrer WAF und beim Reduzieren von falsch positiven Ergebnissen dienen. Weitere Informationen finden Sie unter [WAF-Anforderungsgrößenlimits und Ausschlusslisten](application-gateway-waf-configuration.md).|Dezember 2018|
|Automatische Skalierung, Zonenredundanz, Unterstützung von statischen VIPs      |Mit der v2 SKU wurden viele Verbesserungen eingeführt, z. B. automatische Skalierung, verbesserte Leistung und mehr. Weitere Informationen finden Sie unter [Was ist Azure Application Gateway?](overview.md).|September 2018|
|Verbindungsausgleich     |Der Verbindungsausgleich ermöglicht Ihnen das korrekte Entfernen von Mitgliedern aus Ihren Back-End-Pools. Weitere Informationen finden Sie unter [Verbindungsausgleich](overview.md#connection-draining).|September 2018|
|Benutzerdefinierte Fehlerseiten     |Mit benutzerdefinierten Fehlerseiten können Sie eine Fehlerseite im Format Ihrer restlichen Websites erstellen. Informationen zur Aktivierung finden Sie unter [Erstellen von benutzerdefinierten Application Gateway-Fehlerseiten](custom-error.md).|September 2018|
|Verbesserungen für Metriken     |Dank verbesserter Metriken erhalten Sie einen besseren Einblick in Ihren Application Gateway-Zustand. Informationen zur Aktivierung von Metriken für Application Gateway finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](application-gateway-diagnostics.md).|Juni 2018|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Application Gateway finden Sie unter [Was ist Azure Application Gateway?](overview.md).
