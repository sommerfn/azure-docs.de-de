---
title: Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden eines Azure IoT Edge-Geräts als transparentes Gateway, das Informationen von nachgeschalteten Geräten verarbeiten kann
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 467ec25bb9e41180da36f118094324e4fea48cf8
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266096"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Konfigurieren eines IoT Edge-Geräts als transparentes Gateway

Dieser Artikel enthält detaillierte Anweisungen zur Konfiguration eines IoT Edge-Geräts, das als transparentes Gateway für andere Geräte zur Kommunikation mit IoT Hub dient. In diesem Artikel bezieht sich der Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät, das als transparentes Gateway verwendet wird. Weitere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](./iot-edge-as-gateway.md).

>[!NOTE]
>Derzeit gilt Folgendes:
> * Edge-fähige Geräte können keine Verbindung mit IoT Edge-Gateways herstellen. 
> * Nachgeschaltete Geräte können keinen Dateiupload verwenden.

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der erste Schritt behandelt:

1. **Das Gatewaygerät muss eine sichere Verbindung mit nachgeschalteten Geräten herstellen, Nachrichten von nachgeschalteten Geräten empfangen und Nachrichten an das richtige Ziel weiterleiten können.**
2. Das nachgeschaltete Gerät benötigt eine Geräteidentität, damit es sich bei IoT Hub authentifizieren kann und weiß, dass es über sein Gatewaygerät kommunizieren kann. Weitere Informationen finden Sie unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. Das nachgeschaltete Gerät muss eine sichere Verbindung mit seinem Gatewaygerät herstellen können. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).


Damit ein Gerät als Gateway fungieren kann, muss es in der Lage sein, sichere Verbindungen mit nachgeschalteten Geräten herzustellen. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Gatewaygeräts bestätigen. Diese Überprüfung der Identität verhindert, dass Ihre Geräte Verbindungen mit potenziell schädlichen Gateways herstellen.

Ein nachgeschaltetes Gerät im Szenario eines transparenten Gateways kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)-Clouddienst erstellt wurde. Oft verwenden diese Anwendungen das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). In der Praxis kann ein nachgeschaltetes Gerät sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. Allerdings kann ein IoT Edge-Gerät einem IoT Edge-Gateway nicht nachgeschaltet werden. 

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Zertifizierungsstellenzertifikat](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT-Hub zugeordnet (der Stammzertifizierungsstelle von IoT Hub), und es sind eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

![Gatewayzertifikateinrichtung](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>Der in diesem Artikel verwendete Begriff „Stammzertifizierungsstelle“ (Stamm-ZS) bezieht sich auf die oberste Zertifizierungsstelle des öffentlichen Zertifikats der PKI-Vertrauenskette. Es muss nicht unbedingt der Zertifikatstamm einer fremden Zertifizierungsstelle sein. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle. 

Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung das Zertifizierungsstellenzertifikat des eigenen IoT Edge-Geräts vor. Das nachgeschaltete Gerät überprüft, ob das Zertifizierungsstellenzertifikat des IoT Edge-Geräts mit dem Zertifikat der Stammzertifizierungsstelle signiert wurde. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten auf dem Gateway geführt. Sie können mit jedem beliebigen Computer Zertifikate generieren und sie dann auf Ihr IoT Edge-Gerät kopieren. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Entwicklungscomputer zum Erstellen von Zertifikaten. 
* Sie benötigen ein Azure IoT Edge-Gerät, das als Gateway konfiguriert wurde. Verwenden Sie die IoT Edge-Installationsschritte für eines der folgenden Betriebssysteme:
  * [Windows](how-to-install-iot-edge-windows.md)
  * [Linux](how-to-install-iot-edge-linux.md)

## <a name="generate-certificates-with-windows"></a>Generieren von Zertifikaten unter Windows

Verwenden Sie die Schritte in diesem Abschnitt, um Testzertifikate unter Windows zu erstellen. Sie können zum Generieren der Zertifikate einen Windows-Computer verwenden und sie dann auf jedes IoT Edge-Gerät mit einem beliebigen unterstützten Betriebssystem kopieren. 

Die in diesem Abschnitt generierten Zertifikate sind nur für Testzwecke bestimmt. 

### <a name="install-openssl"></a>Installieren von OpenSSL

Installieren Sie OpenSSL für Windows auf dem Computer, den Sie zum Generieren der Zertifikate verwenden. Wenn Sie OpenSSL bereits auf Ihrem Windows-Gerät installiert haben, können Sie diesen Schritt überspringen. Stellen Sie jedoch sicher, dass „openssl.exe“ in Ihrer PATH-Umgebungsvariablen verfügbar ist. 

Es gibt mehrere Möglichkeiten zum Installieren von OpenSSL, darunter:

* **Einfacher:** Laden Sie beliebige [Binärdateien von OpenSSL-Drittanbietern](https://wiki.openssl.org/index.php/Binaries) herunter (z. B. von [diesem Projekt auf SourceForge](https://sourceforge.net/projects/openssl/)), und installieren Sie sie. Fügen Sie „openssl.exe“ den vollständigen Pfad zu Ihrer PATH-Umgebungsvariablen hinzu. 
   
* **Empfohlen.** Laden Sie den OpenSSL-Quellcode herunter, und erstellen Sie die Binärdateien entweder selbst oder mithilfe von [vcpkg](https://github.com/Microsoft/vcpkg) auf Ihrem Computer. In den nachfolgend aufgeführten Anweisungen wird in benutzerfreundlichen Schritten „vcpkg“ zum Herunterladen des Quellcodes sowie zum Kompilieren und Installieren von OpenSSL auf Ihrem Windows-Computer verwendet.

   1. Navigieren Sie zu einem Verzeichnis, in dem Sie „vcpkg“ installieren möchten. Wir bezeichnen das Verzeichnis als *\<VCPKGDIR>* . Folgen Sie den Anweisungen zum Herunterladen und Installieren von [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Führen Sie nach der Installation von vcpkg über eine PowerShell-Eingabeaufforderung den folgenden Befehl für die Installation des OpenSSL-Pakets für Windows x64 aus. Diese Installation dauert in der Regel 5 Minuten.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Fügen Sie `<VCPKGDIR>\installed\x64-windows\tools\openssl` Ihrer PATH-Umgebungsvariablen hinzu, damit die Datei „openssl.exe“ aufgerufen werden kann.

### <a name="prepare-creation-scripts"></a>Vorbereiten von Erstellungsskripts

Das Azure IoT Edge-Git-Repository enthält Skripts, mit denen Sie Testzertifikate erstellen können. In diesem Abschnitt klonen Sie das IoT Edge-Repository und führen die Skripts aus. 

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus. 

2. Klonen Sie das Git-Repository, das Skripts zum Generieren von Zertifikaten enthält, die nicht in der Produktion eingesetzt werden. Diese Skripts helfen Ihnen dabei, die erforderlichen Zertifikate zum Einrichten eines transparenten Gateways zu erstellen. Verwenden Sie den Befehl `git clone`, oder [laden Sie die ZIP-Datei herunter](https://github.com/Azure/iotedge/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. In diesem Artikel wird dieses Verzeichnis als *\<WRKDIR>* bezeichnet. Alle Zertifikate und Schlüssel werden in diesem Arbeitsverzeichnis erstellt.

4. Kopieren Sie die Konfigurations- und Skriptdateien aus dem geklonten Repository in Ihr Arbeitsverzeichnis. 

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Wenn Sie das Repository als ZIP-Datei heruntergeladen haben, lautet der Ordnername `iotedge-master`. Der Rest des Pfads ist identisch. 
<!--
5. Set environment variable OPENSSL_CONF to use the openssl_root_ca.cnf configuration file.

    ```powershell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```
-->
5. Aktivieren Sie PowerShell zum Ausführen der Skripts.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Fügen Sie die Funktionen, die von den Skripts verwendet werden, in den globalen Namespace von PowerShell ein.
   
   ```powershell
   . .\ca-certs.ps1
   ```

   Im PowerShell-Fenster wird eine Warnung angezeigt, das die von diesem Skript generierten Zertifikate ausschließlich für Testzwecke vorgesehen sind und nicht in Produktionsszenarien verwendet werden sollten.

8. Überprüfen Sie, ob OpenSSL richtig installiert wurde und keine Namenskonflikte mit vorhandenen Zertifikaten entstehen. Wenn Probleme auftreten, müsste das Skript Informationen darüber enthalten, wie diese auf Ihrem System behoben werden können.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Erstellen von Zertifikaten

In diesem Abschnitt erstellen Sie drei Zertifikate und verbinden sie dann als Kette. Wenn Sie die Zertifikate in einer Kettendatei platzieren, können Sie sie einfach auf Ihrem IoT Edge-Gatewaygerät und auf beliebigen nachgeschalteten Geräten installieren.  

1. Erstellen Sie das Zertifikat der Stammzertifizierungsstelle, und signieren Sie damit ein Zwischenzertifikat. Die Zertifikate werden alle im Arbeitsverzeichnis gespeichert.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Dieser Skriptbefehl erstellt mehrere Zertifikat- und Schlüsseldateien. Eine davon wird weiter unten in diesem Artikel noch genauer behandelt:
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Erstellen Sie mit dem folgenden Befehl das Zertifizierungsstellenzertifikat und den privaten Schlüssel für das IoT Edge-Gerät. Geben Sie einen Namen für das Zertifizierungsstellenzertifikat an, z.B. **MeinEdgeGerätZertifizierungsstellenzertifikat**. Der Name wird zum Benennen der Dateien und während der Generierung von Zertifikaten verwendet. 

   ```powershell
   New-CACertsEdgeDeviceCA "MyEdgeDeviceCA"
   ```

   Dieser Skriptbefehl erstellt mehrere Zertifikat- und Schlüsseldateien. Zwei davon werden weiter unten in diesem Artikel noch genauer behandelt:
   * `<WRKDIR>\certs\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-ca-MyEdgeDeviceCA.key.pem`

   >[!TIP]
   >Wenn Sie einen anderen Namen als **MeinEdgeGerätZertifizierungsstellenzertifikat** angeben, werden die durch diesen Befehl erstellten Zertifikate und Schlüssel diesen Namen anzeigen. 

Nachdem Sie nun über Zertifikate verfügen, fahren Sie mit dem [Installieren von Zertifikaten auf dem Gateway](#install-certificates-on-the-gateway) fort.

## <a name="generate-certificates-with-linux"></a>Generieren von Zertifikaten unter Linux

Verwenden Sie die Schritte in diesem Abschnitt, um Testzertifikate unter Linux zu erstellen. Sie können zum Generieren der Zertifikate einen Linux-Computer verwenden und sie dann auf jedes IoT Edge-Gerät mit einem beliebigen unterstützten Betriebssystem kopieren. 

Die in diesem Abschnitt generierten Zertifikate sind nur für Testzwecke bestimmt. 

### <a name="prepare-creation-scripts"></a>Vorbereiten von Erstellungsskripts

Das Azure IoT Edge-Git-Repository enthält Skripts, mit denen Sie Testzertifikate erstellen können. In diesem Abschnitt klonen Sie das IoT Edge-Repository und führen die Skripts aus. 

1. Klonen Sie das Git-Repository, das Skripts zum Generieren von Zertifikaten enthält, die nicht in der Produktion eingesetzt werden. Diese Skripts helfen Ihnen dabei, die erforderlichen Zertifikate zum Einrichten eines transparenten Gateways zu erstellen. 

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. Dieses Verzeichnis wird in diesem Artikel als *\<WRKDIR>* bezeichnet. Alle Zertifikat- und Schlüsseldateien werden in diesem Verzeichnis erstellt.
  
3. Kopieren Sie die Konfigurations- und Skriptdateien aus dem geklonten IoT Edge-Repository in Ihr Arbeitsverzeichnis.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

### <a name="create-certificates"></a>Erstellen von Zertifikaten

In diesem Abschnitt erstellen Sie drei Zertifikate und verbinden sie dann als Kette. Wenn Sie die Zertifikate in einer Kettendatei platzieren, können Sie sie einfacher auf Ihrem IoT Edge-Gatewaygerät und auf allen nachgeschalteten Geräten installieren.  

1. Erstellen Sie das Zertifikat der Stammzertifizierungsstelle und ein Zwischenzertifikat. Die Zertifikate werden alle in *\<WRKDIR>* platziert.

   Wenn Sie in diesem Arbeitsverzeichnis bereits Stamm- und Zwischenzertifikate erstellt haben, führen Sie dieses Skript nicht erneut aus. Durch das erneute Ausführen des Skripts werden die vorhandenen Zertifikate nämlich überschrieben. Fahren Sie stattdessen mit dem nächsten Schritt fort. 

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Das Skript erstellt mehrere Zertifikate und Schlüssel. Notieren Sie sich eine Datei, auf die im nächsten Abschnitt verwiesen wird:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`

2. Erstellen Sie mit dem folgenden Befehl das Zertifizierungsstellenzertifikat und den privaten Schlüssel für das IoT Edge-Gerät. Geben Sie einen Namen für das Zertifizierungsstellenzertifikat an, z.B. **MeinEdgeGerätZertifizierungsstellenzertifikat**. Der Name wird zum Benennen der Dateien und während der Generierung von Zertifikaten verwendet. 

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "MyEdgeDeviceCA"
   ```

   Das Skript erstellt mehrere Zertifikate und Schlüssel. Notieren Sie zwei Dateien, auf die im nächsten Abschnitt verwiesen wird: 
   * `<WRKDIR>/certs/iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-ca-MyEdgeDeviceCA.key.pem`

   >[!TIP]
   >Wenn Sie einen anderen Namen als **MeinEdgeGerätZertifizierungsstellenzertifikat** angeben, werden die durch diesen Befehl erstellten Zertifikate und Schlüssel diesen Namen anzeigen. 

## <a name="install-certificates-on-the-gateway"></a>Installieren von Zertifikaten auf dem Gateway

Nachdem Sie nun eine Zertifikatkette erstellt haben, müssen Sie diese auf dem IoT Edge-Gateway installieren und die IoT Edge-Runtime so konfigurieren, dass sie auf die neuen Zertifikate verweist. 

1. Kopieren Sie die folgenden Dateien aus *\<WRKDIR>* . Speichern Sie sie auf Ihrem IoT Edge-Gerät. Wir bezeichnen das Zielverzeichnis auf Ihrem IoT Edge-Gerät als *\<CERTDIR>* . 

   * Zertifizierungsstellenzertifikat des Geräts – `<WRKDIR>\certs\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem`
   * Privater Zertifizierungsstellenschlüssel des Geräts – `<WRKDIR>\private\iot-edge-device-ca-MyEdgeDeviceCA.key.pem`
   * Stammzertifizierungsstelle – `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

   Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.  Wenn Sie die Zertifikate auf dem IoT Edge-Gerät selbst erstellt haben, können Sie diesen Schritt überspringen und den Pfad für das Arbeitsverzeichnis verwenden.

2. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Legen Sie die Eigenschaften des **Zertifikats** in der Datei „config.yaml“ auf den vollständigen Pfad der Zertifikat- und Schlüsseldateien auf dem IoT Edge-Gerät fest. Entfernen Sie das `#`-Zeichen vor den Zertifikateigenschaften, um die Auskommentierung der vier Zeilen aufzuheben. Denken Sie daran, dass in YAML als Einzüge zwei Leerzeichen verwendet werden.

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>\\certs\\iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>\\private\\iot-edge-device-ca-MyEdgeDeviceCA.key.pem"
        trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
      ```
   
   * Linux: 
      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>/certs/iot-edge-device-ca-MyEdgeDeviceCA-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>/private/iot-edge-device-ca-MyEdgeDeviceCA.key.pem"
        trusted_ca_certs: "<CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem"
      ```

4. Stellen Sie auf Linux-Geräten sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten hat. 

## <a name="deploy-edgehub-to-the-gateway"></a>Bereitstellen von Edge Hub für das Gateway

Wenn Sie IoT Edge zum ersten Mal auf einem Gerät installieren, startet nur ein Systemmodul automatisch: der IoT Edge-Agent. Damit Ihr Gerät als Gateway fungieren kann, benötigen Sie beide Systemmodule. Wenn Sie noch keine Module auf Ihrem Gatewaygerät bereitgestellt haben, erstellen Sie eine Erstbereitstellung für Ihr Gerät, um das zweite Systemmodul zu starten: den IoT Edge-Hub. Die Bereitstellung sieht leer aus, da Sie keine Module im Assistenten hinzufügen. Sie stellt jedoch sicher, dass beide Systemmodule ausgeführt werden. 

Mit dem Befehl `iotedge list` können Sie überprüfen, welche Module auf einem Gerät ausgeführt werden. Wenn die Liste nur das Modul **edgeAgent** ohne **edgeHub** enthält, gehen Sie folgendermaßen vor:

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.

3. Wählen Sie **Module festlegen** aus.

4. Klicken Sie auf **Weiter**.

5. Auf der Seite **Routen angeben** sollte eine Standardroute aufgeführt sein, über die alle Nachrichten von allen Modulen an IoT Hub gesendet werden. Ist das nicht der Fall, können Sie den folgenden Code eingeben und dann **Weiter** auswählen.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. Wählen Sie auf der Seite **Vorlage überprüfen** die Option **Senden** aus.

## <a name="open-ports-on-gateway-device"></a>Öffnen von Ports auf dem Gatewaygerät

Standard IoT Edge-Geräte benötigen keine eingehende Verbindung, um zu funktionieren, da die gesamte Kommunikation mit dem IoT Hub über ausgehende Verbindungen erfolgt. Gatewaygeräte unterscheiden sich dadurch, dass sie in der Lage sein müssen, Nachrichten von ihren nachgeschalteten Geräten zu empfangen. Wenn zwischen den nachgeschalteten Geräten und dem Gatewaygerät eine Firewall ist, muss die Kommunikation auch über die Firewall möglich sein.

Damit ein Gatewayszenario funktioniert, muss mindestens eines der unterstützten Protokolle des IoT Edge-Hubs für den eingehenden Datenverkehr von nachgeschalteten Geräten offen sein. Die unterstützten Protokolle sind MQTT, AMQP und HTTPS. 

| Port | Protocol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP+WS | 

## <a name="route-messages-from-downstream-devices"></a>Weiterleiten von Nachrichten von nachgeschalteten Geräten
Die IoT Edge-Runtime kann Nachrichten für nachgeschaltete Geräte genau wie Nachrichten von Modulen weiterleiten. Dieses Features ermöglicht es Ihnen, die Analyse in einem auf dem Gateway ausgeführten Modul durchzuführen, bevor Daten an die Cloud gesendet werden. 

Zurzeit werden von nachgeschalteten Geräten gesendete Nachrichten weitergeleitet, indem sie von den von Modulen gesendeten Nachrichten unterschieden werden. Im Gegensatz zu Nachrichten, die von nachgeschalteten Geräten gesendet werden, enthalten von Modulen gesendete Nachrichten eine Systemeigenschaft namens **connectionModuleId**. Sie können die WHERE-Klausel der Route verwenden, um Nachrichten mit dieser Systemeigenschaft auszuschließen. 

Die unten gezeigte Route ist ein Beispiel, bei dem die Nachrichten von einem beliebigen nachgeschalteten Gerät an das Modul `ai_insights` und dann von `ai_insights` an IoT Hub gesendet werden.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Weitere Informationen zum Routing von Nachrichten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen](./module-composition.md#declare-routes).


## <a name="enable-extended-offline-operation"></a>Aktivieren des erweiterten Offlinebetriebs

Ab [Release v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) der IoT Edge-Runtime können das Gatewaygerät und die nachgeschalteten Geräte, die mit ihm verbunden sind, für einen erweiterten Offlinebetrieb konfiguriert werden. 

Mit dieser Funktion können lokale Module oder nachgeschaltete Geräte bei Bedarf eine erneute Authentifizierung beim IoT Edge-Gerät durchführen und über Nachrichten und Methoden miteinander kommunizieren, auch wenn sie vom IoT-Hub getrennt sind. Weitere Informationen finden Sie unter [Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte](offline-capabilities.md).

Für die Aktivierung der erweiterten Offlinefunktionen, stellen Sie eine Beziehung mit über- und untergeordneten Geräten zwischen dem IoT Edge-Gatewaygerät und den nachgeschalteten Geräten her, die eine Verbindung mit diesem herstellen. Diese Schritte werden unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md) ausführlicher erläutert.

## <a name="next-steps"></a>Nächste Schritte

Da nun ein IoT Edge-Gerät als transparentes Gateway fungiert, müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gateway vertrauen und Nachrichten daran senden. Lesen Sie [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md), um die nächsten Schritte zum Einrichten Ihres Szenarios eines transparenten Gateways auszuführen. 
