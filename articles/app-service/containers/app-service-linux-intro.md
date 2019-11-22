---
title: 'Einführung in App Service unter Linux: Azure | Microsoft-Dokumentation'
description: Informationen zu Azure App Service unter Linux.
keywords: Azure App Service, Linux, OSS
services: app-service
documentationcenter: ''
author: msangapu-msft
manager: gwallace
editor: ''
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: overview
ms.date: 1/11/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 1bbcd5e4f8c6a429def84ad77d7dd93fa11b7324
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72819687"
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Einführung in Azure App Service unter Linux

[Azure App Service](../overview.md) ist eine vollständig verwaltete Computeplattform, die für das Hosten von Websites und Webanwendungen optimiert ist. Kunden können App Service unter Linux verwenden, um Web-Apps für unterstützte Anwendungsstapel nativ unter Linux zu hosten.

## <a name="languages"></a>Languages

App Service unter Linux unterstützt eine Reihe von integrierten Images, um die Produktivität der Entwickler zu steigern. Zu den Sprachen zählen: Node.js, Java (JRE 8 und JRE 11), PHP, Python, .NET Core und Ruby. Führen Sie [`az webapp list-runtimes --linux`](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes) aus, um die neuesten Sprachen und unterstützten Versionen anzuzeigen. Wenn die Laufzeit, die Anwendung Ihre erfordert, nicht in den integrierten Images unterstützt wird, sind Anweisungen zum [Erstellen eines eigenen Docker-Images](tutorial-custom-docker-image.md) verfügbar, das in Web-App für Container bereitgestellt werden kann.

## <a name="deployments"></a>Bereitstellungen

* FTP
* Lokales Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Stagingumgebungen
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) und CI/CD in DockerHub

## <a name="console-publishing-and-debugging"></a>Konsole, Veröffentlichen und Debuggen

* Umgebungen
* Bereitstellungen
* Grundlegende Konsole
* SSH

## <a name="scaling"></a>Skalieren

* Kunden können Web-Apps zentral hoch- und herunterskalieren, indem sie den Tarif ihres [App Service-Plans](https://docs.microsoft.com/azure/app-service/overview-hosting-plans?toc=%2fazure%2fapp-service-web%2ftoc.json) ändern.

## <a name="locations"></a>Standorte

Überprüfen Sie das [Dashboard zum Azure-Status](https://azure.microsoft.com/status).

## <a name="limitations"></a>Einschränkungen

Im Azure-Portal werden nur Features angezeigt, die derzeit für Web-App für Container funktionieren. Sobald wir weitere Features aktivieren, werden sie im Portal angezeigt.

App Service für Linux wird nur in den App Service-Plänen [Free, Basic, Standard und Premium](https://azure.microsoft.com/pricing/details/app-service/plans/) unterstützt und verfügt nicht über einen [Shared](https://azure.microsoft.com/pricing/details/app-service/plans/)-Tarif. Sie können keine Linux-Web-App in einem App Service-Plan erstellen, in dem bereits Web-Apps unter anderen Betriebssystemen als Linux gehostet werden.  

Aufgrund einer aktuellen Beschränkung können Sie für dieselbe Ressourcengruppe keine Windows- und Linux-Apps in derselben Region kombinieren.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Ihre Anwendung nicht gestartet werden kann oder Sie die Protokolle Ihrer App überprüfen möchten, sehen Sie sich die Docker-Protokolle im Verzeichnis „LogFiles“ an. Sie können auf dieses Verzeichnis entweder über Ihre SCM-Website oder per FTP zugreifen. Zum Protokollieren von `stdout` und `stderr` in Ihrem Container müssen Sie unter **App Service-Protokolle** die **Protokollierung von Docker-Containern** aktivieren. Diese Einstellung wird sofort wirksam. App Service erkennt die Änderung und startet den Container automatisch neu.

Sie können auf die SCM-Website über die Option **Erweiterte Tools** im Menü **Entwicklungstools** zugreifen.

![Verwenden von Kudu zum Anzeigen von Docker-Protokollen][1]

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten Informationen zu den ersten Schritten mit App Service unter Linux mit Web-Apps in verschiedenen Sprachen:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](https://docs.microsoft.com/azure/app-service/containers/quickstart-php)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)
* [Go](quickstart-docker-go.md)
* [Apps mit mehreren Containern](quickstart-multi-container.md)

Weitere Informationen zu App Service für Linux finden Sie in den folgenden Themen:

* [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](app-service-linux-faq.md)
* [SSH-Unterstützung bei Azure App Service unter Linux](app-service-linux-ssh-support.md)
* [Einrichten von Stagingumgebungen in Azure App Service](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continuous Deployment mit Web-App für Container](app-service-linux-ci-cd.md)

In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
