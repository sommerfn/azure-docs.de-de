---
title: Beibehalten des Status in Windows – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Beibehalten des Status in Windows
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 485c6d4a92539a2ba67aece319c68d31649e8045
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991763"
---
# <a name="persist-state-in-windows"></a>Beibehalten des Status in Windows

Themen und Abonnements, die im Event Grid-Modul erstellt wurden, werden standardmäßig im Containerdateisystem gespeichert. Ohne Persistenz gehen beim erneuten Bereitstellen des Moduls alle erstellten Metadaten verloren. Um die Daten für mehrere Bereitstellungen beizubehalten, müssen Sie die Daten außerhalb des Containerdateisystems als persistent festlegen. Derzeit werden nur Metadaten persistent gespeichert. Ereignisse werden speicherintern gespeichert. Wenn das Event Grid-Modul erneut bereitgestellt oder neu gestartet wird, gehen alle nicht übermittelten Ereignisse verloren.

In diesem Artikel werden die Schritte beschrieben, die zum Bereitstellen des Event Grid-Moduls mit Persistenz in Windows-Bereitstellungen erforderlich sind.

> [!NOTE]
>Das Event Grid-Modul wird als Benutzer mit geringfügigen Rechten (**ContainerUser**) in Windows ausgeführt.

## <a name="persistence-via-volume-mount"></a>Persistenz über Volumebereitstellung

[Docker-Volumes](https://docs.docker.com/storage/volumes/) werden verwendet, um Daten über Bereitstellungen hinweg beizubehalten. Zum Bereitstellen eines Volumes müssen Sie es mit Docker-Befehlen erstellen und Berechtigungen zuweisen, sodass Lese- und Schreibvorgänge des Containers im Volume möglich sind. Anschließend müssen Sie das Modul bereitstellen. Es gibt keine Möglichkeit, ein Volume mit den erforderlichen Berechtigungen unter Windows automatisch bereitzustellen. Es muss vor der Bereitstellung erstellt werden.

1. Erstellen Sie ein Volume, indem Sie den folgenden Befehl ausführen:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume create <your-volume-name-here>
    ```

    Beispiel:

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume create myeventgridvol
   ```
1. Rufen Sie das Hostverzeichnis ab, dem das Volume zugeordnet ist. Führen Sie dazu den unten stehenden Befehl aus:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume inspect <your-volume-name-here>
    ```

    Beispiel:

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume inspect myeventgridvol
   ```

   Beispielausgabe:

   ```json
   [
          {
            "CreatedAt": "2019-07-30T21:20:59Z",
            "Driver": "local",
            "Labels": {},
            "Mountpoint": "C:\\ProgramData\\iotedge-moby\u000bolumes\\myeventgridvol\\_data",
            "Name": "myeventgridvol",
            "Options": {},
            "Scope": "local"
          }
   ]
   ```
1. Fügen Sie die Gruppe **Benutzer** wie folgt dem Wert hinzu, auf den **Mountpoint** verweist:
    1. Starten Sie den Datei-Explorer.
    1. Navigieren Sie zu dem Ordner, auf den **Mountpoint**verweist.
    1. Klicken Sie mit der rechten Maustaste, und wählen Sie **Eigenschaften** aus.
    1. Wählen Sie **Sicherheit** aus.
    1. Wählen Sie unter „Gruppen- oder Benutzernamen“ die Option **Bearbeiten** aus.
    1. Wählen Sie **Hinzufügen** aus, geben Sie `Users` ein, wählen Sie **Namen überprüfen** und anschließend **OK** aus.
    1. Wählen Sie unter *Permissions for Users* (Berechtigungen für Benutzer) **Ändern** aus, und wählen Sie **OK** aus.
1. Verwenden Sie **Bindungen**, um dieses Volume bereitzustellen und das Event Grid-Modul über das Azure-Portal erneut bereitzustellen.

   Beispiel:

    ```json
        {
              "Env": [
                "inbound:serverAuth:tlsPolicy=strict",
                "inbound:serverAuth:serverCert:source=IoTEdge",
                "inbound:clientAuth:sasKeys:enabled=false",
                "inbound:clientAuth:clientCert:enabled=true",
                "inbound:clientAuth:clientCert:source=IoTEdge",
                "inbound:clientAuth:clientCert:allowUnknownCA=true",
                "outbound:clientAuth:clientCert:enabled=true",
                "outbound:clientAuth:clientCert:source=IoTEdge",
                "outbound:webhook:httpsOnly=true",
                "outbound:webhook:skipServerCertValidation=false",
                "outbound:webhook:allowUnknownCA=true"
              ],
              "HostConfig": {
                "Binds": [
                  "<your-volume-name-here>:C:\\app\\metadataDb"
                ],
                "PortBindings": {
                  "4438/tcp": [
                     {
                        "HostPort": "4438"
                     }
                  ]
                }
              }
        }
    ```

   >[!IMPORTANT]
   >Ändern Sie nicht den zweiten Teil des Bindungswerts. Er verweist auf eine bestimmte Adresse im Modul. Für das Event Grid-Modul unter Windows muss dies **C:\\app\\metadataDb** sein.


    Beispiel:

    ```json
    {
        "Env": [
            "inbound:serverAuth:tlsPolicy=strict",
            "inbound:serverAuth:serverCert:source=IoTEdge",
            "inbound:clientAuth:sasKeys:enabled=false",
            "inbound:clientAuth:clientCert:enabled=true",
            "inbound:clientAuth:clientCert:source=IoTEdge",
            "inbound:clientAuth:clientCert:allowUnknownCA=true",
            "outbound:clientAuth:clientCert:enabled=true",
            "outbound:clientAuth:clientCert:source=IoTEdge",
            "outbound:webhook:httpsOnly=true",
            "outbound:webhook:skipServerCertValidation=false",
            "outbound:webhook:allowUnknownCA=true"
         ],
         "HostConfig": {
            "Binds": [
                "myeventgridvol:C:\\app\\metadataDb"
             ],
             "PortBindings": {
                    "4438/tcp": [
                         {
                            "HostPort": "4438"
                          }
                    ]
              }
         }
    }
    ```

## <a name="persistence-via-host-directory-mount"></a>Persistenz über Bereitstellung des Hostverzeichnisses

Sie können auch ein Verzeichnis im Hostsystem erstellen und dieses Verzeichnis bereitstellen.

1. Erstellen Sie ein Verzeichnis im Hostdateisystem, indem Sie den folgenden Befehl ausführen:

   ```sh
   mkdir <your-directory-name-here>
   ```

   Beispiel:

   ```sh
   mkdir C:\myhostdir
   ```
1. Verwenden Sie **Bindungen**, um Ihr Verzeichnis bereitzustellen und das Event Grid-Modul über das Azure-Portal erneut bereitzustellen.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:C:\\app\\metadataDb"
             ]
         }
    }
    ```

    >[!IMPORTANT]
    >Ändern Sie nicht den zweiten Teil des Bindungswerts. Er verweist auf eine bestimmte Adresse im Modul. Für das Event Grid-Modul unter Windows muss dies **C:\\app\\metadataDb** sein.

    Beispiel:

    ```json
    {
        "Env": [
            "inbound:serverAuth:tlsPolicy=strict",
            "inbound:serverAuth:serverCert:source=IoTEdge",
            "inbound:clientAuth:sasKeys:enabled=false",
            "inbound:clientAuth:clientCert:enabled=true",
            "inbound:clientAuth:clientCert:source=IoTEdge",
            "inbound:clientAuth:clientCert:allowUnknownCA=true",
            "outbound:clientAuth:clientCert:enabled=true",
            "outbound:clientAuth:clientCert:source=IoTEdge",
            "outbound:webhook:httpsOnly=true",
            "outbound:webhook:skipServerCertValidation=false",
            "outbound:webhook:allowUnknownCA=true"
         ],
         "HostConfig": {
            "Binds": [
                "C:\\myhostdir:C:\\app\\metadataDb"
             ],
             "PortBindings": {
                    "4438/tcp": [
                         {
                            "HostPort": "4438"
                          }
                    ]
              }
         }
    }
    ```
