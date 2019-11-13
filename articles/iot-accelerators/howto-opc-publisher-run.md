---
title: 'Ausführen von OPC Publisher: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie OPC Publisher ausführen und debuggen. Darüber hinaus wird auf Leistungs- und Arbeitsspeicheraspekte eingegangen.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 66e2cb30dcd58b7ad0c6cedbb547f75c8039bc58
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824136"
---
# <a name="run-opc-publisher"></a>Ausführen von OPC Publisher

In diesem Artikel wird beschrieben, wie Sie OPC Publisher ausführen und debuggen. Darüber hinaus wird auf Leistungs- und Arbeitsspeicheraspekte eingegangen.

## <a name="command-line-options"></a>Befehlszeilenoptionen

Die Anwendungsnutzung wird angezeigt, indem die Befehlszeilenoption `--help` wie folgt verwendet wird:

```sh/cmd
Current directory is: /appdata
Log file is: <hostname>-publisher.log
Log level is: info

OPC Publisher V2.3.0
Informational version: V2.3.0+Branch.develop_hans_methodlog.Sha.0985e54f01a0b0d7f143b1248936022ea5d749f9

Usage: opcpublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]

OPC Edge Publisher to subscribe to configured OPC UA servers and send telemetry to Azure IoT Hub.
To exit the application, just press CTRL-C while it is running.

applicationname: the OPC UA application name to use, required
                  The application name is also used to register the publisher under this name in the
                  IoT Hub device registry.

IoT Hubconnectionstring: the IoT Hub owner connectionstring, optional

There are a couple of environment variables which can be used to control the application:
_HUB_CS: sets the IoT Hub owner connectionstring
_GW_LOGP: sets the filename of the log file to use
_TPC_SP: sets the path to store certificates of trusted stations
_GW_PNFP: sets the filename of the publishing configuration file

Command line arguments overrule environment variable settings.

Options:
      --pf, --publishfile=VALUE
                              the filename to configure the nodes to publish.
                                Default: '/appdata/publishednodes.json'
      --tc, --telemetryconfigfile=VALUE
                              the filename to configure the ingested telemetry
                                Default: ''
  -s, --site=VALUE           the site OPC Publisher is working in. if specified
                                this domain is appended (delimited by a ':' to
                                the 'ApplicationURI' property when telemetry is
                                sent to IoT Hub.
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --ic, --iotcentral     publisher will send OPC UA data in IoTCentral
                                compatible format (DisplayName of a node is used
                                as key, this key is the Field name in IoTCentral)
                                . you need to ensure that all DisplayName's are
                                unique. (Auto enables fetch display name)
                                Default: False
      --sw, --sessionconnectwait=VALUE
                              specify the wait time in seconds publisher is
                                trying to connect to disconnected endpoints and
                                starts monitoring unmonitored items
                                Min: 10
                                Default: 10
      --mq, --monitoreditemqueuecapacity=VALUE
                              specify how many notifications of monitored items
                                can be stored in the internal queue, if the data
                                can not be sent quick enough to IoT Hub
                                Min: 1024
                                Default: 8192
      --di, --diagnosticsinterval=VALUE
                              shows publisher diagnostic info at the specified
                                interval in seconds (need log level info).
                                -1 disables remote diagnostic log and diagnostic
                                output
                                0 disables diagnostic output
                                Default: 0
      --ns, --noshutdown=VALUE
                              same as runforever.
                                Default: False
      --rf, --runforever     publisher can not be stopped by pressing a key on
                                the console, but will run forever.
                                Default: False
      --lf, --logfile=VALUE  the filename of the logfile to use.
                                Default: './<hostname>-publisher.log'
      --lt, --logflushtimespan=VALUE
                              the timespan in seconds when the logfile should be
                                flushed.
                                Default: 00:00:30 sec
      --ll, --loglevel=VALUE the loglevel to use (allowed: fatal, error, warn,
                                info, debug, verbose).
                                Default: info
        --ih, --IoT Hubprotocol=VALUE
                              the protocol to use for communication with IoT Hub (
                                allowed values: Amqp, Http1, Amqp_WebSocket_Only,
                                  Amqp_Tcp_Only, Mqtt, Mqtt_WebSocket_Only, Mqtt_
                                Tcp_Only) or IoT EdgeHub (allowed values: Mqtt_
                                Tcp_Only, Amqp_Tcp_Only).
                                Default for IoT Hub: Mqtt_WebSocket_Only
                                Default for IoT EdgeHub: Amqp_Tcp_Only
      --ms, --IoT Hubmessagesize=VALUE
                              the max size of a message which can be send to
                                IoT Hub. when telemetry of this size is available
                                it will be sent.
                                0 will enforce immediate send when telemetry is
                                available
                                Min: 0
                                Max: 262144
                                Default: 262144
      --si, --IoT Hubsendinterval=VALUE
                              the interval in seconds when telemetry should be
                                send to IoT Hub. If 0, then only the
                                IoT Hubmessagesize parameter controls when
                                telemetry is sent.
                                Default: '10'
      --dc, --deviceconnectionstring=VALUE
                              if publisher is not able to register itself with
                                IoT Hub, you can create a device with name <
                                applicationname> manually and pass in the
                                connectionstring of this device.
                                Default: none
  -c, --connectionstring=VALUE
                              the IoT Hub owner connectionstring.
                                Default: none
      --hb, --heartbeatinterval=VALUE
                              the publisher is using this as default value in
                                seconds for the heartbeat interval setting of
                                nodes without
                                a heartbeat interval setting.
                                Default: 0
      --sf, --skipfirstevent=VALUE
                              the publisher is using this as default value for
                                the skip first event setting of nodes without
                                a skip first event setting.
                                Default: False
      --pn, --portnum=VALUE  the server port of the publisher OPC server
                                endpoint.
                                Default: 62222
      --pa, --path=VALUE     the enpoint URL path part of the publisher OPC
                                server endpoint.
                                Default: '/UA/Publisher'
      --lr, --ldsreginterval=VALUE
                              the LDS(-ME) registration interval in ms. If 0,
                                then the registration is disabled.
                                Default: 0
      --ol, --opcmaxstringlen=VALUE
                              the max length of a string opc can transmit/
                                receive.
                                Default: 131072
      --ot, --operationtimeout=VALUE
                              the operation timeout of the publisher OPC UA
                                client in ms.
                                Default: 120000
      --oi, --opcsamplinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds to request the servers to sample
                                the nodes with this interval
                                this value might be revised by the OPC UA
                                servers to a supported sampling interval.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a negative value will set the sampling interval
                                to the publishing interval of the subscription
                                this node is on.
                                0 will configure the OPC UA server to sample in
                                the highest possible resolution and should be
                                taken with care.
                                Default: 1000
      --op, --opcpublishinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds for the publishing interval setting
                                of the subscriptions established to the OPC UA
                                servers.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a value less than or equal zero will let the
                                server revise the publishing interval.
                                Default: 0
      --ct, --createsessiontimeout=VALUE
                              specify the timeout in seconds used when creating
                                a session to an endpoint. On unsuccessful
                                connection attemps a backoff up to 5 times the
                                specified timeout value is used.
                                Min: 1
                                Default: 10
      --ki, --keepaliveinterval=VALUE
                              specify the interval in seconds the publisher is
                                sending keep alive messages to the OPC servers
                                on the endpoints it is connected to.
                                Min: 2
                                Default: 2
      --kt, --keepalivethreshold=VALUE
                              specify the number of keep alive packets a server
                                can miss, before the session is disconneced
                                Min: 1
                                Default: 5
      --aa, --autoaccept     the publisher trusts all servers it is
                                establishing a connection to.
                                Default: False
      --tm, --trustmyself=VALUE
                              same as trustowncert.
                                Default: False
      --to, --trustowncert   the publisher certificate is put into the trusted
                                certificate store automatically.
                                Default: False
      --fd, --fetchdisplayname=VALUE
                              same as fetchname.
                                Default: False
      --fn, --fetchname      enable to read the display name of a published
                                node from the server. this will increase the
                                runtime.
                                Default: False
      --ss, --suppressedopcstatuscodes=VALUE
                              specifies the OPC UA status codes for which no
                                events should be generated.
                                Default: BadNoCommunication,
                                BadWaitingForInitialData
      --at, --appcertstoretype=VALUE
                              the own application cert store type.
                                (allowed values: Directory, X509Store)
                                Default: 'Directory'
      --ap, --appcertstorepath=VALUE
                              the path where the own application cert should be
                                stored
                                Default (depends on store type):
                                X509Store: 'CurrentUser\UA_MachineDefault'
                                Directory: 'pki/own'
      --tp, --trustedcertstorepath=VALUE
                              the path of the trusted cert store
                                Default: 'pki/trusted'
      --rp, --rejectedcertstorepath=VALUE
                              the path of the rejected cert store
                                Default 'pki/rejected'
      --ip, --issuercertstorepath=VALUE
                              the path of the trusted issuer cert store
                                Default 'pki/issuer'
      --csr                  show data to create a certificate signing request
                                Default 'False'
      --ab, --applicationcertbase64=VALUE
                              update/set this applications certificate with the
                                certificate passed in as bas64 string
      --af, --applicationcertfile=VALUE
                              update/set this applications certificate with the
                                certificate file specified
      --pb, --privatekeybase64=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as base64 string
      --pk, --privatekeyfile=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as file
      --cp, --certpassword=VALUE
                              the optional password for the PEM or PFX or the
                                installed application certificate
      --tb, --addtrustedcertbase64=VALUE
                              adds the certificate to the applications trusted
                                cert store passed in as base64 string (multiple
                                strings supported)
      --tf, --addtrustedcertfile=VALUE
                              adds the certificate file(s) to the applications
                                trusted cert store passed in as base64 string (
                                multiple filenames supported)
      --ib, --addissuercertbase64=VALUE
                              adds the specified issuer certificate to the
                                applications trusted issuer cert store passed in
                                as base64 string (multiple strings supported)
      --if, --addissuercertfile=VALUE
                              adds the specified issuer certificate file(s) to
                                the applications trusted issuer cert store (
                                multiple filenames supported)
      --rb, --updatecrlbase64=VALUE
                              update the CRL passed in as base64 string to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --uc, --updatecrlfile=VALUE
                              update the CRL passed in as file to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --rc, --removecert=VALUE
                              remove cert(s) with the given thumbprint(s) (
                                multiple thumbprints supported)
      --dt, --devicecertstoretype=VALUE
                              the IoT Hub device cert store type.
                                (allowed values: Directory, X509Store)
                                Default: X509Store
      --dp, --devicecertstorepath=VALUE
                              the path of the iot device cert store
                                Default Default (depends on store type):
                                X509Store: 'My'
                                Directory: 'CertificateStores/IoT Hub'
  -i, --install              register OPC Publisher with IoT Hub and then exits.
                                Default:  False
  -h, --help                 show this message and exit
      --st, --opcstacktracemask=VALUE
                              ignored, only supported for backward comaptibility.
      --sd, --shopfloordomain=VALUE
                              same as site option, only there for backward
                                compatibility
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --vc, --verboseconsole=VALUE
                              ignored, only supported for backward comaptibility.
      --as, --autotrustservercerts=VALUE
                              same as autoaccept, only supported for backward
                                cmpatibility.
                                Default: False
      --tt, --trustedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted cert store will always reside in a
                                directory.
      --rt, --rejectedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the rejected cert store will always reside in a
                                directory.
      --it, --issuercertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted issuer cert store will always
                                reside in a directory.
```

In der Regel geben Sie die Verbindungszeichenfolge des IoT Hub-Besitzers nur bei der ersten Ausführung der Anwendung an. Die Verbindungszeichenfolge ist verschlüsselt und wird im Plattformzertifikatspeicher gespeichert. Bei späteren Ausführungen liest die Anwendung die Verbindungszeichenfolge aus dem Zertifikatspeicher. Wenn Sie die Verbindungszeichenfolge bei jeder Ausführung angeben, wird das Gerät, das für die Anwendung in der IoT Hub-Geräteregistrierung erstellt wird, entfernt und neu erstellt.

## <a name="run-natively-on-windows"></a>Natives Ausführen unter Windows

Öffnen Sie das Projekt **opcpublisher.sln** mit Visual Studio, erstellen Sie die Lösung, und veröffentlichen Sie sie. Sie können die Anwendung im von Ihnen veröffentlichten **Zielverzeichnis** wie folgt starten:

```cmd
dotnet opcpublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-self-built-container"></a>Verwenden eines selbst erstellten Containers

Erstellen Sie Ihren eigenen Container, und starten Sie ihn wie folgt:

```sh/cmd
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-container-from-microsoft-container-registry"></a>Verwenden eines Containers aus der Microsoft Container Registry

Die Microsoft Container Registry enthält einen vordefinierten Container. Starten Sie ihn wie folgt:

```sh/cmd
docker run mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Informieren Sie sich unter [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher) über die unterstützten Betriebssysteme und Prozessorarchitekturen. Wenn Ihr Betriebssystem und die CPU-Architektur unterstützt werden, wählt Docker automatisch den richtigen Container aus.

## <a name="run-as-an-azure-iot-edge-module"></a>Ausführen als Azure IoT Edge-Modul

OPC Publisher kann als [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge)-Modul verwendet werden. Bei Verwendung von OPC Publisher als IoT Edge-Modul werden nur die Transportprotokolle **Amqp_Tcp_Only** und **Mqtt_Tcp_Only** unterstützt.

Navigieren Sie im Azure-Portal zu Ihren IoT Hub-Einstellungen, und führen Sie die folgenden Schritte aus, um OPC Publisher Ihrer IoT Edge-Bereitstellung als Modul hinzuzufügen:

1. Navigieren Sie zu **IoT Edge**, und erstellen Sie Ihr IoT Edge-Gerät bzw. wählen Sie es aus.
1. Wählen Sie **Module festlegen** aus.
1. Wählen Sie unter **Bereitstellungsmodule** die Option **Hinzufügen** und dann **IoT Edge-Modul**.
1. Geben Sie im Feld **Name** den Namen **publisher** ein.
1. Geben Sie im Feld **Image-URI** die Zeichenfolge `mcr.microsoft.com/iotedge/opc-publisher:<tag>` ein.
1. Sie finden die verfügbaren Tags auf der [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher)-Website.
1. Fügen Sie den folgenden JSON-Code in das Feld **Optionen für Containererstellung** ein:

    ```json
    {
        "Hostname": "publisher",
        "Cmd": [
            "--aa"
        ]
    }
    ```

    Hiermit wird IoT Edge so konfiguriert, dass ein Container mit dem Namen **publisher** mit dem OPC Publisher-Image gestartet wird. Der Hostname des Containersystems ist auf **publisher** festgelegt. OPC Publisher wird mit dem folgenden Befehlszeilenargument aufgerufen: `--aa`. Bei Verwendung dieser Option vertraut OPC Publisher den Zertifikaten der OPC UA-Server, mit denen eine Verbindung hergestellt wird. Sie können alle OPC Publisher-Befehlszeilenoptionen verwenden. Die einzige Einschränkung ist die Größe in den **Optionen für Containererstellung**, die von IoT Edge unterstützt wird.

1. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
1. Wechseln Sie zurück zur Seite **Module festlegen**, wenn Sie die Ausgabe von OPC Publisher lokal mit einem anderen IoT Edge-Modul verarbeiten möchten. Greifen Sie anschließend auf die Registerkarte **Routen angeben** zu, und fügen Sie eine neue Route hinzu, die wie der folgende JSON-Code aussieht:

    ```json
    {
      "routes": {
        "processingModuleToIoT Hub": "FROM /messages/modules/processingModule/outputs/* INTO $upstream",
        "opcPublisherToProcessingModule": "FROM /messages/modules/publisher INTO BrokeredEndpoint(\"/modules/processingModule/inputs/input1\")"
      }
    }
    ```

1. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter**, bis Sie die letzte Seite der Konfiguration erreichen.
1. Wählen Sie **Senden**, um Ihre Konfiguration an IoT Edge zu senden.
1. Wenn Sie IoT Edge auf Ihrem Edgegerät gestartet haben und der Docker-Container **publisher** ausgeführt wird, können Sie die Protokollausgabe von OPC Publisher prüfen, indem Sie entweder `docker logs -f publisher` oder die Protokolldatei verwenden. Im vorherigen Beispiel befindet sich die Protokolldatei oberhalb von `d:\iiotegde\publisher-publisher.log`. Sie können auch das [Tool „iot-edge-opc-publisher-diagnostics“](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics) verwenden.

### <a name="make-the-configuration-files-accessible-on-the-host"></a>Ermöglichen des Zugriffs auf die Konfigurationsdateien auf dem Host

Verwenden Sie die folgenden **Optionen für Containererstellung**, damit die Konfigurationsdateien für das IoT Edge-Modul im Hostdateisystem zugänglich sind. Hier ist ein Beispiel für eine Bereitstellung mit Linux-Containern für Windows angegeben:

```json
{
    "Hostname": "publisher",
    "Cmd": [
        "--pf=./pn.json",
        "--aa"
    ],
    "HostConfig": {
        "Binds": [
            "d:/iiotedge:/appdata"
        ]
    }
}
```

Bei diesen Optionen liest OPC Publisher die zu veröffentlichenden Knoten aus der Datei `./pn.json` aus, und das Arbeitsverzeichnis des Containers wird beim Starten auf `/appdata` festgelegt. Mit diesen Einstellungen liest OPC Publisher die Datei `/appdata/pn.json` aus dem Container aus, um die Konfiguration zu erhalten. Ohne die Option `--pf` versucht OPC Publisher, die Standardkonfigurationsdatei `./publishednodes.json` zu lesen.

Die Protokolldatei, für die der Standardname `publisher-publisher.log` verwendet wird, wird in `/appdata` geschrieben, und das Verzeichnis `CertificateStores` wird ebenfalls in diesem Verzeichnis erstellt.

Um diese Dateien im Hostdateisystem verfügbar zu machen, benötigt die Containerkonfiguration ein Bereitstellungsvolume für die Einbindung. Mit der `d://iiotedge:/appdata`-Bindung wird das Verzeichnis `/appdata`, bei dem es sich um das aktuelle Arbeitsverzeichnis für den Containerstart handelt, dem Hostverzeichnis `d://iiotedge` zugeordnet. Ohne diese Option werden keine Dateidaten gespeichert, wenn der Container das nächste Mal gestartet wird.

Wenn Sie Windows-Container ausführen, wird eine andere Syntax für den Parameter `Binds` verwendet. Beim Containerstart lautet das Arbeitsverzeichnis `c:\appdata`. Geben Sie im Abschnitt `HostConfig` die folgende Zuordnung an, um die Konfigurationsdatei im Verzeichnis `d:\iiotedge` auf dem Host anzuordnen:

```json
"HostConfig": {
    "Binds": [
        "d:/iiotedge:c:/appdata"
    ]
}
```

Wenn Sie Linux-Container unter Linux ausführen, wird wiederum eine andere Syntax für den Parameter `Binds` verwendet. Beim Containerstart lautet das Arbeitsverzeichnis `/appdata`. Geben Sie im Abschnitt `HostConfig` die folgende Zuordnung an, um die Konfigurationsdatei im Verzeichnis `/iiotedge` auf dem Host anzuordnen:

```json
"HostConfig": {
    "Binds": [
        "/iiotedge:/appdata"
    ]
}
```

## <a name="considerations-when-using-a-container"></a>Wichtige Aspekte bei Verwendung eines Containers

In den folgenden Abschnitten sind einige Punkte aufgeführt, die Sie beim Verwenden eines Containers beachten sollten:

### <a name="access-to-the-opc-publisher-opc-ua-server"></a>Zugriff auf den OPC UA-Server von OPC Publisher

Der OPC UA-Server von OPC Publisher überwacht standardmäßig Port 62222. Verwenden Sie den folgenden Befehl, um diesen eingehenden Port in einem Container verfügbar zu machen:

```sh/cmd
docker run -p 62222:62222 mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="enable-intercontainer-name-resolution"></a>Aktivieren der Namensauflösung zwischen Containern

Erstellen Sie zum Aktivieren der Namensauflösung aus dem Container für andere Container ein benutzerdefiniertes Docker-Bridge-Netzwerk, und stellen Sie für den Container mit der Option `--network` eine Verbindung mit diesem Netzwerk her. Weisen Sie dem Container außerdem einen Namen zu, indem Sie die Option `--name` wie folgt verwenden:

```sh/cmd
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Der Container ist für andere Container in demselben Netzwerk jetzt unter dem Namen `publisher` erreichbar.

### <a name="access-other-systems-from-within-the-container"></a>Zugreifen auf andere Systeme aus dem Container

Andere Container sind erreichbar, indem die im vorherigen Abschnitt beschriebenen Parameter verwendet werden. Wenn das Betriebssystem, unter dem Docker gehostet wird, DNS-fähig ist, funktioniert der Zugriff auf alle Systeme, die dem DNS bekannt sind.

In Netzwerken, für die die NetBIOS-Namensauflösung genutzt wird, wird der Zugriff auf andere Systeme ermöglicht, indem Sie Ihren Container mit der Option `--add-host` starten. Mit dieser Option wird der Hostdatei des Containers quasi ein Eintrag hinzugefügt:

```cmd/sh
docker run --add-host mydevbox:192.168.178.23  mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="assign-a-hostname"></a>Zuweisen eines Hostnamens

OPC Publisher verwendet den Hostnamen des Computers, auf dem die Anwendung ausgeführt wird, um Zertifikate und Endpunkte zu generieren. Docker wählt einen zufälligen Hostnamen aus, falls mit der Option `-h` keiner festgelegt wird. Im folgenden Beispiel wird veranschaulicht, wie der interne Hostname des Containers auf `publisher` festgelegt wird:

```sh/cmd
docker run -h publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-bind-mounts-shared-filesystem"></a>Verwenden von Bindungseinbindungen (freigegebenes Dateisystem)

Anstatt das Containerdateisystem zu verwenden, können Sie auch das Hostdateisystem auswählen, um Konfigurationsinformationen und Protokolldateien zu speichern. Verwenden Sie die Option `-v` von `docker run` im Modus für die Bindungseinbindung, um diese Option zu konfigurieren.

## <a name="opc-ua-x509-certificates"></a>X.509-Zertifikate für OPC UA

OPC UA nutzt X.509-Zertifikate zum Authentifizieren des OPC UA-Clients und -Servers, wenn eine Verbindung dafür eingerichtet wird, und zum Verschlüsseln der entsprechenden Kommunikation. Für OPC Publisher werden Zertifikatspeicher verwendet, die vom OPC UA-Stapel gepflegt werden, um alle Zertifikate verwalten zu können. Beim Start überprüft OPC Publisher, ob ein eigenes Zertifikat vorhanden ist. Falls der Zertifikatspeicher kein Zertifikat enthält und in der Befehlszeile keins übergeben wird, wird von OPC Publisher ein selbstsigniertes Zertifikat erstellt. Weitere Informationen finden Sie unter der **InitApplicationSecurityAsync**-Methode in `OpcApplicationConfigurationSecurity.cs`.

Selbstsignierte Zertifikate sorgen nicht für Sicherheit, weil sie nicht von einer vertrauenswürdigen Zertifizierungsstelle signiert sind.

OPC Publisher verfügt über Befehlszeilenoptionen für folgende Zwecke:

- Abrufen von CSR-Informationen des aktuellen Anwendungszertifikats, das von OPC Publisher verwendet wird
- Bereitstellen von OPC Publisher mit einem von einer Zertifizierungsstelle signierten Zertifikat
- Bereitstellen von OPC Publisher mit einem neuen Schlüsselpaar und passendem signierten Zertifikat einer Zertifizierungsstelle
- Hinzufügen von Zertifikaten zum Zertifikatspeicher eines vertrauenswürdigen Peers oder Ausstellers
- Hinzufügen einer Zertifikatsperrliste
- Entfernen eines Zertifikats aus dem Zertifikatspeicher eines vertrauenswürdigen Peers oder Ausstellers

Bei all diesen Optionen können Sie Parameter übergeben, indem Sie Dateien oder Base64-codierte Zeichenfolgen verwenden.

Der Standardspeichertyp für alle Zertifikatspeicher ist das Dateisystem. Dies können Sie mit den Befehlszeilenoptionen ändern. Da der Container in seinem Dateisystem keinen beständigen Speicher bereitstellt, müssen Sie einen anderen Speichertyp auswählen. Verwenden Sie die Docker-Option `-v`, um die Zertifikatspeicher im Hostdateisystem oder auf einem Docker-Volume anzuordnen. Bei Verwendung eines Docker-Volumes können Sie Zertifikate übergeben, indem Sie Base64-codierte Zeichenfolgen verwenden.

Die Laufzeitumgebung wirkt sich darauf aus, wie Zertifikate gespeichert werden. Vermeiden Sie es, dass bei jeder Ausführung der Anwendung neue Zertifikatspeicher erstellt werden:

- Bei der nativen Ausführung unter Windows können Sie keinen Anwendungszertifikatspeicher vom Typ `Directory` nutzen, weil der Zugriff auf den privaten Schlüssel nicht erfolgreich ist. Verwenden Sie in diesem Fall die Option `--at X509Store`.
- Bei der Ausführung als Linux-Docker-Container können Sie die Zertifikatspeicher dem Hostdateisystem mit der „docker run“-Option `-v <hostdirectory>:/appdata` zuordnen. Mit dieser Option wird das Zertifikat für Anwendungsausführungen übergreifend gespeichert.
- Wenn Sie bei der Ausführung als Linux-Docker-Container einen X.509-Speicher für das Anwendungszertifikat nutzen möchten, können Sie die „docker run“-Option `-v x509certstores:/root/.dotnet/corefx/cryptography/x509stores` und die Anwendungsoption `--at X509Store` verwenden.

## <a name="performance-and-memory-considerations"></a>Leistungs- und Arbeitsspeicheraspekte

In diesem Abschnitt werden die Optionen zum Verwalten des Arbeitsspeichers und der Leistung beschrieben:

### <a name="command-line-parameters-to-control-performance-and-memory"></a>Befehlszeilenparameter zum Steuern der Leistung und des Arbeitsspeichers

Beim Ausführen von OPC Publisher müssen Sie die Leistungsanforderungen und die auf Ihrem Host verfügbaren Arbeitsspeicherressourcen kennen.

Der Arbeitsspeicher und die Leistung sind voneinander abhängig und basieren jeweils darauf, wie viele Knoten Sie für die Veröffentlichung konfigurieren. Stellen Sie sicher, dass die folgenden Parameter Ihre Anforderungen erfüllen:

- IoT Hub sendet ein Intervall: `--si`
- IoT Hub-Nachrichtengröße (Standardwert `1`): `--ms`
- Überwachte Kapazität der Elementwarteschlange: `--mq`

Mit dem Parameter `--mq` wird die obere Grenze der Kapazität der internen Warteschlange gesteuert, in der alle Benachrichtigungen zu Änderungen des OPC-Knotenwerts gepuffert werden. Falls OPC Publisher Nachrichten nicht schnell genug an IoT Hub senden kann, werden die Benachrichtigungen in dieser Warteschlange gepuffert. Mit dem Parameter wird die Anzahl von Benachrichtigungen festgelegt, die gepuffert werden können. Falls Sie beobachten, dass die Anzahl von Elementen in dieser Warteschlange bei Ihren Testläufen ansteigt, sollten Sie wie folgt vorgehen, um den Verlust von Nachrichten zu vermeiden:

- Reduzieren des IoT Hub-Sendeintervalls
- Erhöhen der IoT Hub-Nachrichtengröße

Mit dem Parameter `--si` wird für OPC Publisher erzwungen, dass Nachrichten im angegebenen Intervall an IoT Hub gesendet werden. OPC Publisher sendet eine Nachricht, sobald die im Parameter `--ms` angegebene Nachrichtengröße oder das mit dem Parameter `--si` angegebene Intervall erreicht ist. Verwenden Sie `--ms 0`, um die Option für die Nachrichtengröße zu deaktivieren. In diesem Fall nutzt OPC Publisher die größtmögliche IoT Hub-Nachrichtengröße von 256 KB, um Daten in Batches zusammenzufassen.

Mit dem Parameter `--ms` können Sie Nachrichten, die an IoT Hub gesendet werden, zu Batches zusammenzufassen. Über das von Ihnen verwendete Protokoll wird ermittelt, ob der Mehraufwand für das Senden einer Nachricht an IoT Hub im Vergleich mit der tatsächlichen Zeit für das Senden der Nutzlast hoch ist. Wenn in Ihrem Szenario für von IoT Hub erfasste Daten Latenz zulässig ist, sollten Sie OPC Publisher so konfigurieren, dass die höchste Nachrichtengröße von 256 KB verwendet wird.

Bevor Sie OPC Publisher in Produktionsszenarien nutzen, sollten Sie die Leistung und Arbeitsspeicherauslastung unter Produktionsbedingungen testen. Sie können den Parameter `--di` verwenden, um das Intervall in Sekunden anzugeben, in dem von OPC Publisher Diagnoseinformationen geschrieben werden.

### <a name="test-measurements"></a>Testmessungen

In der folgenden Beispieldiagnose werden Messungen mit unterschiedlichen Werten für die Parameter `--si` und `--ms` veranschaulicht. Hierbei wurden 500 Knoten mit einem OPC Publisher-Intervall von einer Sekunde veröffentlicht.  Im Test wurde ein OPC Publisher-Debugbuild unter Windows 10 nativ für eine Dauer von 120 Sekunden verwendet. Das IoT Hub-Protokoll war das MQTT-Standardprotokoll.

#### <a name="default-configuration---si-10---ms-262144"></a>Standardkonfiguration (--si 10 --ms 262144)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:33:05 (started @ 26.10.2017 15:31:09)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54363
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54363
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:33:04
bytes sent to IoT Hub: 12709429
avg msg size: 116600
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 10
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

In der Standardkonfiguration werden alle zehn Sekunden Daten an IoT Hub gesendet – oder wenn 256 KB an Daten für die Erfassung durch IoT Hub verfügbar sind. Durch diese Konfiguration ergibt sich eine zusätzliche Latenz von ca. zehn Sekunden, aber aufgrund der hohen Nachrichtengröße besteht die geringste Wahrscheinlichkeit für Datenverlust. In der Diagnoseausgabe ist zu sehen, dass es nicht zum Verlust von OPC-Knotenupdates gekommen ist: `monitored item notifications enqueue failure: 0`.

#### <a name="constant-send-interval---si-1---ms-0"></a>Konstantes Sendeintervall (--si 1 --ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:35:59 (started @ 26.10.2017 15:34:03)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54243
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54243
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:35:59
bytes sent to IoT Hub: 12683836
avg msg size: 116365
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 1
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

Wenn die Nachrichtengröße auf „0“ festgelegt ist, werden die Daten von OPC Publisher intern zusammengefasst, indem die höchste unterstützte IoT Hub-Nachrichtengröße von 256 KB verwendet wird. In der Diagnoseausgabe ist zu sehen, dass die durchschnittliche Nachrichtengröße 115.019 Byte beträgt. Bei dieser Konfiguration kommt es für OPC Publisher nicht zum Verlust von OPC-Knotenwertupdates, und im Vergleich zum Standardwert ist die Latenz geringer.

### <a name="send-each-opc-node-value-update---si-0---ms-0"></a>Senden jedes OPC-Knotenwertupdates (--si 0 --ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:39:33 (started @ 26.10.2017 15:37:37)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 8184
monitored item notifications enqueued: 54232
monitored item notifications enqueue failure: 44624
monitored item notifications dequeued: 1424
---------------------------------
messages sent to IoT Hub: 1423
last successful msg sent @: 26.10.2017 15:39:33
bytes sent to IoT Hub: 333046
avg msg size: 234
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 96
--si setting: 0
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

Bei dieser Konfiguration wird für jede Änderung des OPC-Knotenwerts eine Nachricht an IoT Hub gesendet. Die Diagnose verdeutlicht, dass die durchschnittliche Nachrichtengröße gering ist und nur 234 Byte beträgt. Der Vorteil dieser Konfiguration ist, dass durch OPC Publisher keine Zunahme der Latenz bewirkt wird. Da die Anzahl von verlorenen OPC-Knotenwertupdates hoch ist (`monitored item notifications enqueue failure: 44624`), ist diese Konfiguration nicht für Szenarien geeignet, in denen große Mengen von Telemetriedaten veröffentlicht werden.

### <a name="maximum-batching---si-0---ms-262144"></a>Maximale Batchverarbeitung (--si 0 --ms 262144)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:42:55 (started @ 26.10.2017 15:41:00)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54137
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54137
---------------------------------
messages sent to IoT Hub: 48
last successful msg sent @: 26.10.2017 15:42:55
bytes sent to IoT Hub: 12565544
avg msg size: 261782
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 0
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Bei dieser Konfiguration werden so viele OPC-Knotenwertupdates wie möglich zu einem Batch zusammengefasst. Die maximale IoT Hub-Nachrichtengröße beträgt 256 KB. Dies ist die in diesem Fall konfigurierte Größe. Es wird kein Sendeintervall angefordert. Dies bedeutet, dass die Latenz durch die Datenmenge bestimmt wird, die von IoT Hub erfasst wird. Diese Konfiguration verfügt über die geringste Wahrscheinlichkeit für den Verlust von OPC-Knotenwerten und ist für die Veröffentlichung einer hohen Zahl von Knoten geeignet. Stellen Sie bei Verwendung dieser Konfiguration sicher, dass für Ihr Szenario keine Bedingungen gelten, die zu hoher Latenz führen, wenn die Nachrichtengröße von 256 KB nicht erreicht wird.

## <a name="debug-the-application"></a>Debuggen der Anwendung

Öffnen Sie zum Debuggen der Anwendung die Projektmappendatei **opcpublisher.sln** mit Visual Studio, und verwenden Sie die Debugtools von Visual Studio.

Wenn Sie in OPC Publisher auf den OPC UA-Server zugreifen müssen, sollten Sie sicherstellen, dass für Ihre Firewall der Zugriff auf den Port zulässig ist, über den der Server lauscht. Der Standardport ist: 62222.

## <a name="control-the-application-remotely"></a>Verwenden der Remotesteuerung der Anwendung

Die Konfiguration der zu veröffentlichenden Knoten kann mit den direkten IoT Hub-Methoden durchgeführt werden.

OPC Publisher implementiert einige zusätzliche direkte IoT Hub-Methodenaufrufe, um Folgendes zu lesen:

- Allgemeine Informationen.
- Diagnoseinformationen in OPC-Sitzungen, Abonnements und überwachten Elementen
- Diagnoseinformationen zu IoT Hub-Nachrichten und -Ereignissen
- Startprotokoll
- Die letzten 100 Zeilen des Protokolls
- Vorgang zum Herunterfahren der Anwendung

Die folgenden GitHub-Repositorys enthalten Tools zum [Konfigurieren der zu veröffentlichenden Knoten](https://github.com/Azure-Samples/iot-edge-opc-publisher-nodeconfiguration) und [Lesen der Diagnoseinformationen](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics). Beide Tools sind auch als Container in Docker Hub verfügbar.

## <a name="use-a-sample-opc-ua-server"></a>Verwenden eines OPC UA-Beispielservers

Falls Sie nicht über einen realen OPC UA-Server verfügen, können Sie das [OPC UA PLC-Beispiel](https://github.com/Azure-Samples/iot-edge-opc-plc) nutzen, um zu beginnen. Dieses PLC-Beispiel ist auch auf Docker Hub verfügbar.

Hierbei werden einige Tags implementiert, die zufällige Daten und Tags mit Anomalien generieren. Sie können das Beispiel erweitern, wenn Sie weitere Tagwerte simulieren müssen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit der Ausführung von OPC Publisher vertraut gemacht haben, empfehlen wir Ihnen, sich als Nächstes über [OPC Twin](overview-opc-twin.md) und [OPC Vault](overview-opc-vault.md) zu informieren.
