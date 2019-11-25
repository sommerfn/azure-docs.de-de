---
title: Bewährte Methoden der Bereitstellung – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Hauptkomponenten der Bereitstellung in Azure App Service.
keywords: Azure App Service, Web-App, bereitstellen, Bereitstellung, Pipelines, Build
services: app-service
documentationcenter: ''
author: jasonfreeberg
manager: ''
editor: ''
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/31/2019
ms.author: jafreebe
ms.custom: ''
ms.openlocfilehash: 121ea4b7e29510ef86b61350ed97ffca5d133d56
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199490"
---
# <a name="deployment-best-practices"></a>Bewährte Methoden der Bereitstellung

Jedes Entwicklungsteam verfügt über spezifische Anforderungen, die die Implementierung einer effizienten Bereitstellungspipeline in einem beliebigen Clouddienst erschweren können. In diesem Artikel werden die drei Hauptkomponenten der Bereitstellung in App Service vorgestellt: Bereitstellungsquellen, Buildpipelines und Bereitstellungsmechanismen. Dieser Artikel behandelt außerdem einige bewährte Methoden und Tipps für bestimmte Sprachenstapel.

## <a name="deployment-components"></a>Bereitstellungskomponenten

### <a name="deployment-source"></a>Bereitstellungsquelle

Eine Bereitstellungsquelle ist der Speicherort Ihres Anwendungscodes. Bei Produktions-Apps handelt es sich bei der Bereitstellungsquelle normalerweise um ein Repository, das von Software zur Versionskontrolle wie [GitHub, BitBucket oder Azure Repos](deploy-continuous-deployment.md) gehostet wird. Bei Entwicklungs- und Testszenarien kann die Bereitstellungsquelle [ein Projekt auf Ihrem lokalen Computer](deploy-local-git.md) sein. App Service unterstützt auch [OneDrive- und Dropbox-Ordner](deploy-content-sync.md) als Bereitstellungsquellen. Cloudordner können Ihnen die ersten Schritte mit App Service zwar erleichtern, es wird jedoch in der Regel nicht empfohlen, diese Quelle für Produktionsanwendungen auf Unternehmensebene zu verwenden. 

### <a name="build-pipeline"></a>Buildpipeline

Sobald Sie sich für eine Bereitstellungsquelle entschieden haben, besteht Ihr nächster Schritt in der Auswahl einer Buildpipeline. Eine Buildpipeline liest ihren Quellcode aus der Bereitstellungsquelle und führt eine Reihe von Schritten aus (z. B. das Kompilieren von Code, das Minimieren von HTML und JavaScript, das Ausführen von Tests und das Packen von Komponenten), um die Anwendung in einen ausführbaren Zustand zu bringen. Die von der Buildpipeline ausgeführten spezifischen Befehle hängen von Ihrem Sprachenstapel ab. Diese Vorgänge können auf einem Buildserver, z. B. Azure Pipelines, oder lokal ausgeführt werden.

### <a name="deployment-mechanism"></a>Bereitstellungsmechanismus

Der Bereitstellungsmechanismus ist die Aktion, mit der Ihre erstellte Anwendung im Verzeichnis */home/site/wwwroot* Ihrer Web-App abgelegt wird. Das Verzeichnis */wwwroot* ist ein eingebundener Speicherort, der von allen Instanzen Ihrer Web-App gemeinsam verwendet wird. Wenn der Bereitstellungsmechanismus Ihre Anwendung in diesem Verzeichnis ablegt, erhalten Ihre Instanzen eine Benachrichtigung, um sich mit den neuen Dateien zu synchronisieren. App Service unterstützt die folgenden Bereitstellungsmechanismen:

- Kudu-Endpunkte: [Kudu](https://github.com/projectkudu/kudu/wiki) ist das Open Source-Produktivitätstool für Entwickler, das in Windows App Service als gesonderter Prozess ausgeführt wird und in Linux App Service als zweiter Container. Kudu verarbeitet fortlaufende Bereitstellungen und bietet HTTP-Endpunkte für die Bereitstellung, z. B. zipdeploy.
- FTP und WebDeploy: Unter Verwendung Ihrer [Website- oder Benutzeranmeldeinformationen](deploy-configure-credentials.md) können Sie Dateien [über FTP](deploy-ftp.md) oder WebDeploy hochladen. Diese Mechanismen durchlaufen nicht Kudu.  

Bereitstellungstools wie Azure Pipelines, Jenkins und Editor-Plug-Ins verwenden einen dieser Bereitstellungsmechanismen.

## <a name="language-specific-considerations"></a>Sprachspezifische Erwägungen

### <a name="java"></a>Java

Verwenden Sie die Kudu-API [zipdeploy/](deploy-zip.md) für die Bereitstellung von JAR-Anwendungen und [wardeploy/](deploy-zip.md#deploy-war-file) für WAR-Apps. Wenn Sie Jenkins verwenden, können Sie diese APIs direkt in der Bereitstellungsphase verwenden. [hier finden Sie weitere Informationen](../jenkins/execute-cli-jenkins-pipeline.md)

### <a name="node"></a>Knoten

Standardmäßig führt Kudu die Buildschritte für Ihre Node-Anwendung (`npm install`) aus. Wenn Sie einen Builddienst wie Azure DevOps verwenden, ist der Kudu-Build unnötig. Um den Kudu-Build zu deaktivieren, erstellen Sie eine App-Einstellung `SCM_DO_BUILD_DURING_DEPLOYMENT` mit dem Wert `false`.

### <a name="net"></a>.NET 

Standardmäßig führt Kudu die Buildschritte für Ihre .Net-Anwendung (`dotnet build`) aus. Wenn Sie einen Builddienst wie Azure DevOps verwenden, ist der Kudu-Build unnötig. Um den Kudu-Build zu deaktivieren, erstellen Sie eine App-Einstellung `SCM_DO_BUILD_DURING_DEPLOYMENT` mit dem Wert `false`.

## <a name="other-deployment-considerations"></a>Sonstige Überlegungen zur Bereitstellung

### <a name="use-deployment-slots"></a>Verwenden von Bereitstellungsslots

Verwenden Sie nach Möglichkeit immer [Bereitstellungsslots](deploy-staging-slots.md), wenn Sie einen neuen Produktionsbuild bereitstellen. Wenn Sie den App Service-Plantarif „Standard“ oder besser verwenden, können Sie Ihre App in einer Stagingumgebung bereitstellen, Ihre Änderungen überprüfen und Buildüberprüfungstests durchführen. Wenn Sie bereit dazu sind, können Sie Ihre Staging- und Produktionsslots tauschen. Durch den Tauschvorgang werden die erforderlichen Workerinstanzen kontrolliert auf Ihr Produktionsniveau gebracht, wodurch Ausfallzeiten beseitigt werden. 

### <a name="local-cache"></a>Lokaler Cache

Azure App Service-Inhalt wird in Azure Storage gespeichert und dauerhaft als Inhaltsfreigabe bereitgestellt. Manche Apps benötigen jedoch lediglich einen hochleistungsfähigen, schreibgeschützten Inhaltsspeicher, aus dem sie mit Hochverfügbarkeit ausgeführt werden können. Diese Apps können von der Verwendung eines [lokalen Caches](overview-local-cache.md) profitieren. Der lokale Cache wird für Content Management-Sites wie WordPress nicht empfohlen.

Verwenden Sie immer einen lokalen Cache zusammen mit [Bereitstellungsslots](deploy-staging-slots.md), um Ausfallzeiten zu verhindern. Informationen zur Verwendung dieser Funktionen in Kombination finden Sie in [diesem Abschnitt](overview-local-cache.md#best-practices-for-using-app-service-local-cache).

### <a name="high-cpu-or-memory"></a>Hohe CPU- oder Arbeitsspeicherauslastung

Wenn Ihr App Service-Plan mehr als 90 % der verfügbaren CPU oder des Arbeitsspeichers verwendet, hat der zugrunde liegende virtuelle Computer möglicherweise Probleme beim Verarbeiten Ihrer Bereitstellung. Wenn dies geschieht, skalieren Sie die Anzahl Ihrer Instanzen vorübergehend zentral hoch, um die Bereitstellung auszuführen. Nachdem die Bereitstellung abgeschlossen ist, können Sie die Anzahl der Instanzen auf den vorherigen Wert zurücksetzen.
