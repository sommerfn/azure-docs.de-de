---
title: Bereitstellen eines selbstgehosteten Azure API Management-Gateways für Docker | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein selbstgehostetes Azure-API Management-Gateway für Docker bereitstellen.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: fc7e0f7e4e0e80a030a437bc4f0f13360595f32e
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747691"
---
# <a name="deploy-an-azure-api-management-self-hosted-gateway-to-docker"></a>Bereitstellen eines selbstgehosteten Azure API Management-Gateways für Docker

Dieser Artikel beschreibt die Schritte für die Bereitstellung eines selbstgehosteten Azure API Management-Gateways in einer Docker-Umgebung.

> [!NOTE]
> Das Feature „selbstgehostetes Gateway“ ist als Vorschauversion verfügbar. Während der Vorschauphase ist das selbstgehostete Gateway nur in den Tarifen „Developer“ und „Premium“ ohne zusätzliche Kosten verfügbar. Der Tarif „Developer“ ist auf eine Bereitstellung mit einem einzelnen selbstgehosteten Gateway beschränkt.

## <a name="prerequisites"></a>Voraussetzungen

- Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Erstellen Sie eine Docker-Umgebung. [Docker für Desktop](https://www.docker.com/products/docker-desktop) ist eine gute Option für Entwicklungs- und Evaluierungszwecke. Weitere Informationen zu allen Docker-Editionen, ihren Features sowie eine umfassende Dokumentation zu Docker selbst finden Sie unter [Docker-Dokumentation](https://docs.docker.com).
- [Bereitstellen einer Gatewayressource in Ihrer API Management-Instanz](api-management-howto-provision-self-hosted-gateway.md)

> [!NOTE]
> Das selbstgehostete Gateway ist als Linux-basierter Docker-Container gepackt.

## <a name="deploy-the-self-hosted-gateway-to-docker"></a>Bereitstellen des selbstgehosteten Gateways für Docker

1. Wählen Sie **Gateways** unter **Einstellungen** aus.
2. Wählen Sie die Gatewayressource aus, die Sie bereitstellen möchten.
3. Wählen Sie **Bereitstellung** aus.
4. Beachten Sie, dass ein neues Token im Textfeld **Token** automatisch für Sie mit den Standardwerten **Ablauf** und **Geheimer Schlüssel** generiert wurde. Passen einen Wert oder beide Werte an, und wählen Sie **Generieren** aus, um ein neues Token zu erstellen.
4. Stellen Sie sicher, dass **Docker** unter **Bereitstellungsskripts** ausgewählt ist.
5. Wählen Sie den Link zur Datei **env.conf** neben **Umgebung** aus, um die Datei herunterzuladen.
6. Wählen Sie das Symbol **Kopieren** rechts neben dem Textfeld **Ausführen** aus, um den Docker-Befehl in der Zwischenablage zu speichern.
7. Fügen Sie den Befehl in das Terminalfenster (oder Befehlsfenster) ein. Passen Sie die Portzuordnungen und den Containernamen nach Bedarf an. Beachten Sie, dass der Befehl erwartet, dass die heruntergeladene Umgebungsdatei im aktuellen Verzeichnis vorhanden ist.
```
    docker run -d -p 80:8080 -p 443:8081 --name <gateway-name> --env-file env.conf mcr.microsoft.com/azure-api-management/gateway:<tag>
```
8. Führen Sie den Befehl aus. Der Befehl weist Ihre Docker-Umgebung an, den Container auszuführen. Dabei wird das aus Microsoft Container Registry heruntergeladene Image des selbstgehosteten Gateways verwendet, und die HTTP-Ports (8080) und HTTPS-Ports (8081) des Containers werden den Ports 80 und 443 auf dem Host zugeordnet.
9. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaypod ausgeführt wird:
```console
docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                         NAMES
895ef0ecf13b        mcr.microsoft.com/azure-api-management/gateway:beta   "/bin/sh -c 'dotnet …"   5 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp, 0.0.0.0:443->8081/tcp   my-gateway
```
10. Navigieren Sie zurück zum Azure-Portal, und vergewissern Sie sich, dass der soeben bereitgestellte Gatewayknoten einen fehlerfreien Status meldet.

![Gatewaystatus](media/api-management-howto-deploy-self-hosted-gateway-to-docker/status.png)

> [!TIP]
> Verwenden Sie den Befehl <code>console docker container logs <gateway-name></code>, um eine Momentaufnahme des Protokolls des selbstgehosteten Gateways anzuzeigen.
>
> Verwenden Sie den Befehl <code>docker container logs --help</code>, um alle Optionen für die Protokollanzeige anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum selbstgehosteten Gateway finden Sie unter [Azure API Management: Übersicht über das selbstgehostete Gateway](self-hosted-gateway-overview.md).
* [Konfigurieren eines benutzerdefinierten Domänennamens für das selbstgehostete Gateway](api-management-howto-configure-custom-domain-gateway.md)
