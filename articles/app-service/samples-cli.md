---
title: Azure CLI-Beispiele – App Service | Microsoft-Dokumentation
description: Azure CLI-Beispiele – App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 12/12/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: dc8a08c61250994b6083e3936820e1e6025593a6
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70066701"
---
# <a name="cli-samples-for-azure-app-service"></a>CLI-Beispiele für Azure App Service

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| | |
|-|-|
|**Erstellen einer App**||
| [Erstellen einer App und Bereitstellen von Dateien über FTP](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und stellt dafür eine Datei über FTP bereit. |
| [Erstellen einer App und Bereitstellen von Code über GitHub](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und stellt Code aus einem öffentlichen GitHub-Repository bereit. |
| [Erstellen einer App mit Continuous Deployment über GitHub](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App mit kontinuierlicher Veröffentlichung aus einem GitHub-Repository, das sich in Ihrem Besitz befindet. |
| [Erstellen einer App und Bereitstellen von Code über ein lokales GitHub-Repository](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und konfiguriert einen Codepush aus einem lokalen Git-Repository. |
| [Erstellen einer App und Bereitstellen von Code in einer Stagingumgebung](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App mit einem Bereitstellungsslot für Änderungen am Stagingcode. |
| [Erstellen einer ASP.NET Core-App in einem Docker-Container](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App unter Linux und lädt ein Docker-Image aus Docker Hub. |
|**Konfigurieren der App**||
| [Zuordnen einer benutzerdefinierten Domäne zu einer App](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und ordnet ihr einen benutzerdefinierten Domänennamen zu. |
| [Binden eines benutzerdefinierten SSL-Zertifikats an eine App](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und bindet das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. |
|**Skalieren der App**||
| [Manuelles Skalieren einer App](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und skaliert sie über zwei Instanzen. |
| [Weltweites Skalieren einer App mit einer Hochverfügbarkeitsarchitektur](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt zwei App Service-Apps in zwei verschiedenen geografischen Regionen und macht sie mithilfe von Azure Traffic Manager über einen zentralen Endpunkt verfügbar. |
|**Verbinden der App mit Ressourcen**||
| [Verbinden einer App mit einer SQL-Datenbank](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und eine SQL-Datenbank und fügt dann die Datenbank-Verbindungszeichenfolge den App-Einstellungen hinzu. |
| [Verbinden einer App mit einem Speicherkonto](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine App Service-App und ein Speicherkonto und fügt dann die Speicherverbindungszeichenfolge den App-Einstellungen hinzu. |
| [Herstellen einer Verbindung zwischen einer App und einer Azure Cache for Redis-Instanz](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und eine Azure Cache for Redis-Instanz und fügt den App-Einstellungen anschließend die Redis-Verbindungsdetails hinzu. |
| [Verbinden einer App mit Cosmos DB](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und eine Cosmos DB-Instanz und fügt die Cosmos DB-Verbindungsdetails dann den App-Einstellungen hinzu. |
|**Sichern und Wiederherstellen einer App**||
| [Sichern einer App](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und eine einmalige Sicherung dafür. |
| [Erstellen einer geplanten Sicherung für eine App](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App und eine geplante Sicherung dafür. |
| [Wiederherstellen einer App aus einer Sicherung](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Stellt eine App Service-App aus einer Sicherung wieder her. |
|**Überwachen der App**||
| [Überwachen einer Web-App mit Webserverprotokollen](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine App Service-App, aktiviert die Protokollierung für die App und lädt die Protokolle auf Ihren lokalen Computer herunter. |
| | |