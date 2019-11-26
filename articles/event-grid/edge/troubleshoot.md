---
title: Problembehandlung – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: In diesem Artikel wird die Problembehandlung in Event Grid in IoT Edge beschrieben.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/24/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 95181d0eb23d5956b2c6af52c77f85714b107345
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73100153"
---
# <a name="common-issues"></a>Häufige Probleme

Wenn in Ihrer Umgebung Probleme bei der Verwendung von Azure Event Grid in IoT Edge auftreten, verwenden Sie diesen Artikel als Leitfaden für die Behandlung und Behebung von Problemen.

## <a name="view-event-grid-module-logs"></a>Anzeigen der Protokolle des Event Grid-Moduls

Für die Problembehandlung müssen Sie möglicherweise auf die Protokolle des Event Grid-Moduls zugreifen. Führen Sie dazu auf der VM, auf der das Modul bereitgestellt ist, den folgenden Befehl aus:

Unter Windows:

```sh
docker -H npipe:////./pipe/iotedge_moby_engine container logs eventgridmodule
```

Unter Linux:

```sh
sudo docker logs eventgridmodule
```

## <a name="unable-to-make-https-requests"></a>Ausführen von HTTPS-Anforderungen nicht möglich

* Stellen Sie zuerst sicher, dass für das Event Grid-Modul **inbound:serverAuth:tlsPolicy** auf **strict** oder **enabled** festgelegt ist.

* Wenn die Kommunikation zwischen Modulen erfolgt, vergewissern Sie sich, dass der Aufruf an Port **4438** erfolgt und dass der Name des Moduls dem bereitgestellten Modul entspricht. 

  Wenn das Event Grid-Modul beispielsweise mit dem Namen **eventgridmodule** bereitgestellt wurde, muss die URL **https://eventgridmodule:4438** lauten. Stellen Sie sicher, dass die Groß- und Kleinschreibung und die Portnummer richtig sind.
    
* Bei einem Nicht-IoT-Modul muss der Event Grid-Port während der Bereitstellung dem Hostcomputer zugeordnet werden. Beispiel:

    ```json
    "HostConfig": {
                "PortBindings": {
                  "4438/tcp": [
                    {
                        "HostPort": "4438"
                    }
                  ]
                }
     }
    ```

## <a name="unable-to-make-http-requests"></a>Ausführen von HTTP-Anforderungen nicht möglich

* Stellen Sie zuerst sicher, dass für das Event Grid-Modul **inbound:serverAuth:tlsPolicy** auf **enabled** oder **disabled** festgelegt ist.

* Wenn die Kommunikation zwischen Modulen erfolgt, vergewissern Sie sich, dass der Aufruf an Port **5888** erfolgt und dass der Name des Moduls dem bereitgestellten Modul entspricht. 

  Wenn das Event Grid-Modul beispielsweise mit dem Namen **eventgridmodule** bereitgestellt wurde, muss die URL **http://eventgridmodule:5888** lauten. Stellen Sie sicher, dass die Groß- und Kleinschreibung und die Portnummer richtig sind.
    
* Bei einem Nicht-IoT-Modul muss der Event Grid-Port während der Bereitstellung dem Hostcomputer zugeordnet werden. Beispiel:

    ```json
    "HostConfig": {
                "PortBindings": {
                  "5888/tcp": [
                    {
                        "HostPort": "5888"
                    }
                  ]
                }
    }
    ```

## <a name="certificate-chain-was-issued-by-an-authority-thats-not-trusted"></a>Die Zertifikatvertrauenskette wurde von einer Zertifizierungsstelle ausgestellt, die nicht vertrauenswürdig ist

Standardmäßig ist das Event Grid-Modul so konfiguriert, dass Clients mit einem Zertifikat mit einem vom IoT Edge-Sicherheits-Daemon ausgestellten authentifiziert werden. Stellen Sie sicher, dass der Client ein Zertifikat aus dieser Kette präsentiert.

Die **IoTSecurity**-Klasse in [https://github.com/Azure/event-grid-iot-edge](https://github.com/Azure/event-grid-iot-edge) zeigt, wie Zertifikate aus dem IoT Edge-Sicherheits-Daemon abgerufen und zum Konfigurieren von ausgehenden Aufrufen verwendet werden.

Handelt es sich nicht um eine Produktionsumgebung, können Sie die Clientauthentifizierung deaktivieren. Ausführliche Informationen zur Vorgehensweise finden Sie unter [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="debug-events-not-received-by-subscriber"></a>Vom Abonnenten nicht empfangene Debugereignisse

Typische Gründe:

* Das Ereignis wurde nie erfolgreich veröffentlicht. Der HTTP-Statuscode 200(OK) sollte beim Veröffentlichen eines Ereignisses im Event Grid-Modul empfangen werden.

* Überprüfen Sie im Ereignisabonnement Folgendes:
    * Die Endpunkt-URL ist gültig.
    * Es gibt keine Filter im Abonnement, die bewirken, dass das Ereignis gelöscht wird.

* Überprüfen Sie, ob das Abonnentenmodul ausgeführt wird.

* Melden Sie sich bei der VM an, auf der das Event Grid-Modul bereitgestellt ist, und überprüfen Sie dort die Protokolle.

* Aktivieren Sie die Protokollierung einzelner Zustellungen, indem Sie **broker:logDeliverySuccess=true** festlegen, das Event Grid-Modul erneut bereitstellen und die Anforderung erneut senden. Das Aktivieren der Protokollierung einzelner Zustellungen kann sich auf Durchsatz und Latenz auswirken. Es empfiehlt sich daher, nach Abschluss des Debuggens wieder **broker:logDeliverySuccess=false** festzulegen und das Event Grid-Modul erneut bereitzustellen.

* Aktivieren Sie Metriken, indem Sie **metrics:reportertype=console** festlegen und das Event Grid-Modul erneut bereitstellen. Bei allen anschließenden Vorgängen werden Metriken in der Konsole des Event Grid-Moduls protokolliert, wodurch ein weiteres Debuggen möglich ist. Es wird empfohlen, Metriken nur für das Debuggen zu aktivieren und sie anschließend wieder zu deaktivieren. Legen Sie dazu **metrics:reportertype=none** fest, und stellen Sie das Event Grid-Modul erneut bereit.

## <a name="next-steps"></a>Nächste Schritte

Unterbreiten Sie Vorschläge und melden Sie etwaige Probleme bei der Verwendung von Event Grid in IoT Edge unter [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues).