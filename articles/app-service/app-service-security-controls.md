---
title: Sicherheitskontrollen für Azure App Service
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure App Service
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.service: app-service
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 65b1c7f4649ff90c1305d4007428ea02bb003369
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886186"
---
# <a name="security-controls-for-azure-app-service"></a>Sicherheitskontrollen für Azure App Service

In diesem Artikel werden die in Azure App Service integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Derzeit in der Vorschauversion für App Service verfügbar.| [Azure App Service-Zugriffseinschränkungen](app-service-ip-restrictions.md)
| Unterstützung der VNet-Einschleusung| Ja | App Service-Umgebungen sind private Implementierungen von App Service, die für einen einzelnen Kunden dediziert sind, der in das virtuelle Netzwerk eines Kunden eingefügt wurde. | [Einführung in die App Service-Umgebungen](environment/intro.md)
| Netzwerkisolation und Firewallunterstützung| Ja | Für die öffentliche mehrinstanzenfähige Variante von App Service können Kunden Netzwerk-Zugriffssteuerungslisten (IP-Einschränkungen) konfigurieren, um zulässigen eingehenden Datenverkehr zu sperren.  App Service-Umgebungen werden direkt in virtuellen Netzwerken bereitgestellt und können daher mit Netzwerksicherheitsgruppen geschützt werden. | [Azure App Service-Zugriffseinschränkungen](app-service-ip-restrictions.md)
| Unterstützung der Tunnelerzwingung| Ja | App Service-Umgebungen können im virtuellen Netzwerk eines Kunden bereitgestellt werden, in dem Tunnelerzwingung konfiguriert ist. | [Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling](environment/forced-tunnel-support.md)

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | App Service ist in Application Insights eingebunden für Sprachen, die Application Insights (Vollversion von .NET Framework, .NET Core, Java und Node.JS) unterstützen.  Weitere Informationen finden Sie unter [Überwachen der Leistung von Azure App Service](../azure-monitor/app/azure-web-apps.md). App Service sendet außerdem Anwendungsmetriken an Azure Monitor. | [Überwachen von Apps in Azure App Service](web-sites-monitor.md)
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle Verwaltungsvorgänge, die für App Service-Objekte ausgeführt werden, erfolgen über [Azure Resource Manager](../azure-resource-manager/index.yml). Verlaufsprotokolle dieser Vorgänge sind sowohl im Portal als auch über die CLI verfügbar. | [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../role-based-access-control/resource-provider-operations.md#microsoftweb), [az monitor activity-log](/cli/azure/monitor/activity-log)
| Protokollierung und Überwachung auf Datenebene | Nein | Die Datenebene für App Service ist eine Remotedateifreigabe, die die bereitgestellten Websiteinhalte eines Kunden enthält.  Es gibt keine Überwachung der Remotedateifreigabe. |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen |  Dokumentation
|---|---|--|
| Authentication| Ja | Kunden können Anwendungen für App Service erstellen, die automatisch in [Azure Active Directory (Azure AD)](../active-directory/index.yml) sowie andere OAuth-kompatible Identitätsanbieter integriert werden. Für den Verwaltungszugriff auf App Service-Ressourcen wird der gesamte Zugriff durch eine Kombination von durch Azure AD authentifizierten Prinzipalen und RBAC-Rollen von Azure Resource Manager gesteuert. | [Authentifizierung und Autorisierung in Azure App Service](overview-authentication-authorization.md)
| Authorization| Ja | Für Verwaltungszugriff auf App Service-Ressourcen werden sämtliche Zugriffe mit einer Kombination aus Azure AD-authentifizierten Prinzipal- und Azure Resource Manager-RBAC-Rollen gesteuert.  | [Authentifizierung und Autorisierung in Azure App Service](overview-authentication-authorization.md)

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Die Inhalte von Websitedateien werden in Azure Storage gespeichert, das die ruhenden Inhalte automatisch verschlüsselt. <br><br>Vom Kunden bereitgestellte Geheimnisse werden im Ruhezustand verschlüsselt. Die Geheimnisse werden im Ruhezustand verschlüsselt, während sie in App Service-Konfigurationsdatenbanken gespeichert sind.<br><br>Lokal angefügte Datenträger können von Websites als temporärer Speicher verwendet werden („D:\local“ und %TMP%). Lokal angefügte Datenträger werden im Ruhezustand nicht verschlüsselt. | [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](../storage/common/storage-service-encryption.md)
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja | Kunden können wählen, dass Anwendungsgeheimnisse in Key Vault gespeichert werden, und können diese zur Laufzeit abrufen. | [Verwenden von Key Vault-Verweisen für App Service und Azure Functions (Vorschau)](app-service-key-vault-references.md)
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Kunden können Websites so konfigurieren, dass für sie HTTPS für eingehenden Datenverkehr gefordert und verwendet wird.  | [Erzwingen von HTTPS für Azure App Service](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (Blogbeitrag)
| Verschlüsselte API-Aufrufe| Ja | Verwaltungsaufrufe zum Konfigurieren von App Service werden mit [Azure Resource Manager](../azure-resource-manager/index.yml)-Aufrufen über HTTPS gesendet. |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Für Verwaltungsvorgänge kann der Status einer App Service-Konfiguration als eine Azure Resource Manager-Vorlage exportiert und im Lauf der Zeit versioniert werden. Für Runtimevorgänge können Kunden App Service-Bereitstellungsslots verwenden, um mehrere verschiedene aktive Versionen einer Anwendung zu verwalten. | 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).