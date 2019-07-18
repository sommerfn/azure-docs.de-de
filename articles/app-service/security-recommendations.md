---
title: Sicherheitsempfehlungen für Azure App Service
description: Sicherheitsempfehlungen für Azure App Service. Die Umsetzung dieser Empfehlungen erleichtert es Ihnen, Ihre Sicherheitspflichten zu erfüllen, die im Microsoft-Modell der gemeinsamen Verantwortung beschrieben werden. Außerdem erhöhen Sie dadurch die Gesamtsicherheit Ihrer Web-App-Lösungen.
services: app-service
author: barclayn
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: barclayn
ms.openlocfilehash: f50532a21d11d2f7142fa3ee7ed08f759df36d52
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207392"
---
# <a name="security-recommendations-for-app-service"></a>Sicherheitsempfehlungen für App Service

Dieser Artikel enthält Sicherheitsempfehlungen für Azure App Service. Die Umsetzung dieser Empfehlungen erleichtert es Ihnen, Ihre Sicherheitspflichten zu erfüllen, die im Microsoft-Modell der gemeinsamen Verantwortung beschrieben werden. Außerdem erhöhen Sie dadurch die Gesamtsicherheit Ihrer Web-App-Lösungen. Weitere Informationen dazu, wie Microsoft seinen Verantwortlichkeiten als Dienstanbieter nachkommt, finden Sie im Artikel [Sicherheit der Azure-Infrastruktur](../security/azure-security-infrastructure.md).

## <a name="general"></a>Allgemein

| Empfehlung | Kommentare |
|-|-|----|
| Nutzen Sie Technologien, die auf dem neuesten Stand sind. | Verwenden Sie aktuelle Versionen der unterstützten Plattformen, Programmiersprachen, Protokolle und Frameworks. |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Empfehlung | Kommentare |
|-|----|
| Deaktivieren Sie den anonymen Zugriff. | Aktivieren Sie diesen nur dann, wenn anonyme Anforderungen unterstützt werden müssen. Weitere Informationen zu Authentifizierungsoptionen für Azure App Service finden Sie unter [Authentifizierung und Autorisierung – Azure App Service](overview-authentication-authorization.md).|
| Erzwingen Sie die Authentifizierung. | Verwenden Sie nach Möglichkeit immer das App Service-Authentifizierungsmodul, statt Code für die Authentifizierung und Autorisierung zu schreiben. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung –Azure App Service](overview-authentication-authorization.md). |
| Schützen Sie Back-End-Ressourcen durch authentifizierte Zugriffe. | Sie können entweder die Identität des Benutzers oder die Identität einer Anwendung nutzen, um eine Authentifizierung bei einer Back-End-Ressource durchzuführen. Verwenden Sie im Fall einer Anwendungsidentität eine [verwaltete Identität](overview-managed-identity.md).
| Erzwingen Sie die Authentifizierung mit Clientzertifikaten. | Die Authentifizierung mit Clientzertifikaten sorgt für mehr Sicherheit, da nur Verbindungen von Clients zugelassen werden, die sich mit von Ihnen bereitgestellten Zertifikaten authentifizieren können. |

## <a name="data-protection"></a>Datenschutz

| Empfehlung | Kommentare |
|-|-|
| Richten Sie eine Umleitung von HTTP zu HTTPS ein. | Clients können sich standardmäßig über HTTP oder HTTPS mit Web-Apps verbinden. Empfohlen wird eine Umleitung von HTTP zu HTTPS, da für die letztgenannte Option das SSL/TLS-Protokoll verwendet wird. Dieses ermöglicht eine sichere Verbindung, die gleichzeitig verschlüsselt und authentifiziert ist. |
| Verschlüsseln Sie die Kommunikation mit Azure-Ressourcen. | Wenn sich Ihre App mit Azure-Ressourcen wie [SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) oder [Azure Storage](/azure/storage/) verbindet, wird eine Verbindung innerhalb des gemeinsam verwendeten Azure-Netzwerks hergestellt. Sie sollten daher Ihre Kommunikation immer verschlüsseln. |
| Erzwingen Sie den Einsatz der neuesten TLS-Version. | Seit 2018 verwenden neue Azure App Service-Apps TLS 1.2. Neuere TLS-Versionen enthalten Sicherheitsoptimierungen, die in älteren Protokollversionen fehlen. |
| Verwenden Sie FTPS. | Für die Dateibereitstellung unterstützt App Service sowohl FTP als auch FTPS. Verwenden Sie nach Möglichkeit FTPS anstelle von FTP. Wenn Sie eines dieser Protokolle (oder beide) nicht verwenden, sollten Sie die nicht verwendeten Protokolle [deaktivieren](deploy-ftp.md#enforce-ftps). |
| Schützen von Anwendungsdaten | Speichern Sie Anwendungsgeheimnisse wie Datenbank-Anmeldeinformationen, API-Token oder private Schlüssel nicht in Ihrem Code oder in Konfigurationsdateien. Für den Zugriff auf diese Geheimnisse hat es sich bewährt, [Umgebungsvariablen](https://wikipedia.org/wiki/Environment_variable) mit dem Standardmuster in der Sprache Ihrer Wahl zu verwenden. In Azure App Service können Sie Umgebungsvariablen über [App-Einstellungen](web-sites-configure.md) und [Verbindungszeichenfolgen](web-sites-configure.md) definieren. App-Einstellungen und Verbindungszeichenfolgen werden verschlüsselt in Azure gespeichert. Die App-Einstellungen werden erst entschlüsselt, wenn sie beim Start der App in den Prozessspeicher der App eingefügt werden. Die Verschlüsselungsschlüssel werden regelmäßig rotiert. Alternativ können Sie Ihre Azure App Service-App auch in [Azure Key Vault](/azure/key-vault/) integrieren, wenn Sie eine Geheimnisverwaltung mit mehr Konfigurationsmöglichkeiten benötigen. Durch den [Zugriff auf Key Vault mit einer verwalteten Identität](../key-vault/tutorial-web-application-keyvault.md) kann Ihre App Service-App sicher auf die benötigten Geheimnisse zugreifen. |

## <a name="networking"></a>Netzwerk

| Empfehlung | Kommentare |
|-|-|
| Nutzen Sie statische IP-Adressen zur Zugriffsbeschränkung. | In Azure App Service unter Windows können Sie eine Liste mit IP-Adressen definieren, über die der Zugriff auf Ihre App gestattet wird. Die Liste mit den zulässigen IP-Adressen kann einzelne IP-Adressen oder einen durch eine Subnetzmaske definierten IP-Adressbereich enthalten. Weitere Informationen finden Sie unter [Statische Azure App Service-IP-Einschränkungen](app-service-ip-restrictions.md).  |
| Verwenden Sie den Tarif „App Service (isoliert)“. | Mit Ausnahme des Tarifs „App Service (isoliert)“ wird Ihre App bei allen Tarifen in der gemeinsam genutzten Netzwerkinfrastruktur in App Service ausgeführt. Im Tarif „App Service (isoliert)“ werden Ihre Apps in einer dedizierten [App Service-Umgebung](environment/intro.md) ausgeführt, wodurch eine vollständige Netzwerkisolation erreicht wird. Eine App Service-Umgebung wird in Ihrer eigenen Instanz von [Azure Virtual Network](/azure/virtual-network/) betrieben.|
| Verwenden Sie sichere Verbindungen beim Zugriff auf lokale Ressourcen. | Sie können [Hybrid Connections](app-service-hybrid-connections.md), [die Virtual Network-Integration](web-sites-integrate-with-vnet.md) oder die [App Service-Umgebung](environment/intro.md) nutzen, um eine Verbindung mit lokalen Ressourcen herzustellen. |
| Schränken Sie den Zugriff auf eingehenden Netzwerkdatenverkehr ein. | Mit Netzwerksicherheitsgruppen können Sie den Netzwerkzugriff einschränken und die Anzahl der verfügbaren Endpunkte festlegen. Weitere Informationen finden Sie unter [Steuern von eingehendem Datenverkehr in eine App Service-Umgebung](environment/app-service-app-service-environment-control-inbound-traffic.md). |

## <a name="monitoring"></a>Überwachung

| Empfehlung | Kommentare |
|-|-|
|Verwenden Sie den Standard-Tarif von Azure Security Center. | [Azure Security Center](../security-center/security-center-app-services.md) ist nativ in Azure App Service integriert. Mit diesem Dienst können Sie Bewertungen durchführen und Sicherheitsempfehlungen bereitstellen. |

## <a name="next-steps"></a>Nächste Schritte

Erkundigen Sie sich bei Ihrem Anwendungsanbieter, ob weitere Sicherheitsanforderungen bestehen. Weitere Informationen zum Entwickeln sicherer Anwendungen finden Sie unter [Dokumentation zur sicheren Entwicklung](../security/abstract-develop-secure-apps.md).
