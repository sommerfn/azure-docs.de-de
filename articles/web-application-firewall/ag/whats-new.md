---
title: Neuerungen in Azure Web Application Firewall
description: Enthält Informationen zu den Neuerungen in Azure Web Application Firewall, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 10/24/2019
ms.author: victorh
ms.openlocfilehash: 0e0e67bfb893e1bf141182e45ce4a49f5f6880ca
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495494"
---
# <a name="whats-new-in-azure-web-application-firewall"></a>Neuerungen in Azure Web Application Firewall

Azure Web Application Firewall wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen

## <a name="new-features"></a>Neue Funktionen

|Feature  |BESCHREIBUNG  |Hinzufügedatum  |
|---------|---------|---------|
|Regelsatz für die Risikominderung für Bots (Vorschau)|Sie können einen Regelsatz für die Risikominderung für Bots neben dem gewählten CRS-Regelsatz aktivieren. | November 2019 |
|GeoDB-Integration (Vorschau)|Nun können Sie benutzerdefinierte Regeln erstellen, die den Datenverkehr nach Ursprungsland einschränken. | November 2019 |
|WAF pro Site/pro URI-Richtlinie (Vorschau)|WAF-v2 unterstützt jetzt das Anwenden einer Richtlinie auf Listener und pfadbasierte Regeln. Siehe [Erstellen einer WAF-Richtlinie](create-waf-policy-ag.md). | November 2019 |
|Benutzerdefinierte WAF-Regeln |Application Gateway WAF_v2 unterstützt nun das Erstellen von benutzerdefinierten Regeln. Siehe [Custom rules for Web Application Firewall v2](custom-waf-rules-overview.md) (Benutzerdefinierte Regeln für Web Application Firewall v2) |Juni 2019 |
|WAF-Konfiguration und Ausschlussliste     |Wir haben weitere Optionen hinzugefügt, die Ihnen als Hilfe beim Konfigurieren Ihrer WAF und beim Reduzieren von falsch positiven Ergebnissen dienen. Weitere Informationen finden Sie unter [WAF-Anforderungsgrößenlimits und Ausschlusslisten](application-gateway-waf-configuration.md).|Dezember 2018|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Web Application Firewall auf Application Gateway finden Sie unter [Azure Web Application Firewall auf Azure Application Gateway](ag-overview.md).
