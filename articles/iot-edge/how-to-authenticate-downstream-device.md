---
title: Authentifizieren nachgeschalteter Geräte – Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie nachgeschaltete Geräten oder Blattgeräte bei IoT Hub authentifizieren und ihre Verbindung über Azure IoT Edge-Gatewaygeräte weiterleiten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1e184691ebbd34de0f69e93419d9c34ab18edbe6
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025952"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub

In einem Szenario mit transparenten Gateways benötigen nachgeschaltete Geräte (auch als Blattgeräte oder untergeordnete Geräte bezeichnet) wie alle anderen Geräte Identitäten in IoT Hub. In diesem Artikel werden die Optionen für die Authentifizierung von nachgeschalteten Geräten bei IoT Hub beschrieben. Außerdem wird gezeigt, wie Sie die Gatewayverbindung deklarieren.

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der zweite Schritt behandelt:

1. Das Gatewaygerät muss eine sichere Verbindung mit nachgeschalteten Geräten herstellen, Nachrichten von nachgeschalteten Geräten empfangen und Nachrichten an das richtige Ziel weiterleiten können. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md).
2. **Das nachgeschaltete Gerät benötigt eine Geräteidentität, damit es sich bei IoT Hub authentifizieren kann und weiß, dass es über sein Gatewaygerät kommunizieren kann.**
3. Das nachgeschaltete Gerät muss eine sichere Verbindung mit seinem Gatewaygerät herstellen können. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).

Nachgeschaltete Geräte können sich bei IoT Hub mithilfe einer von drei Methoden authentifizieren: symmetrische Schlüssel (auch als SAS-Schlüssel bezeichnet), selbst signierte X.509-Zertifikate oder von einer Zertifizierungsstelle signierte X.509-Zertifikate. Die Authentifizierungsschritte ähneln den Schritten zum Einrichten anderer IoT Edge-Geräte mit IoT Hub. Kleinere Unterschiede bestehen bei der Deklaration der Gatewaybeziehung.

Die Schritte in diesem Artikel veranschaulichen die manuelle Gerätebereitstellung, also die nicht automatisierte Bereitstellung mit dem Azure IoT Hub Device Provisioning Service. 

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md) aus. Wenn Sie die X.509-Authentifizierung für Ihr nachgeschaltetes Gerät verwenden, müssen Sie dasselbe Skript für die Skripterstellung verwenden, das Sie im Artikel zum transparenten Gateway eingerichtet haben. 

Im Artikel wird an mehreren Stellen auf den *Gatewayhostnamen* verwiesen. Der Gatewayhostname wird im **hostname**-Parameter der Datei „config.yaml“ auf dem IoT Edge-Gatewaygerät deklariert. Er wird verwendet, um die Zertifikate in diesem Artikel zu erstellen. Außerdem wird in der Verbindungszeichenfolge der nachgeschalteten Geräten darauf verwiesen. Der Gatewayhostname muss in eine IP-Adresse aufgelöst werden können – entweder mithilfe von DNS oder einem Eintrag in der Hostdatei.

## <a name="symmetric-key-authentication"></a>Authentifizierung mit symmetrischen Schlüsseln

Die Authentifizierung mit symmetrischen Schlüsseln (oder SAS-Schlüsselauthentifizierung) ist die einfachste Möglichkeit für die Authentifizierung bei IoT Hub. Bei der Authentifizierung mit symmetrischen Schlüsseln wird der ID Ihres IoT-Gerätes in IoT Hub ein Base64-Schlüssel zugeordnet. Sie fügen diesen Schlüssel in Ihre IoT-Anwendungen ein, sodass Ihr Gerät diesen beim Herstellen einer Verbindung mit IoT Hub vorweisen kann. 

### <a name="create-the-device-identity"></a>Erstellen der Geräteidentität 

Fügen Sie Ihrem IoT-Hub ein neues IoT-Gerät hinzu. Verwenden Sie dazu entweder das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder die IoT-Erweiterung für Visual Studio Code. Denken Sie daran, dass nachgeschaltete Geräte in IoT Hub als reguläre IoT-Geräte – nicht als IoT Edge-Geräte – angegeben werden müssen. 

Geben Sie beim Erstellen der neuen Geräteidentität die folgenden Informationen an: 

* Erstellen Sie eine ID für Ihr Gerät.

* Wählen Sie als Authentifizierungstyp **Symmetrischer Schlüssel** aus. 

* Wählen Sie optional **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, über das dieses nachgeschaltete Gerät verbunden wird. Dieser Schritt ist für die Authentifizierung mit symmetrischen Schlüsseln optional. Er wird aber empfohlen, da die Festlegung eines übergeordneten Geräts [Offlinefunktionen](offline-capabilities.md) für das nachgeschaltete Gerät aktiviert. Sie können das Gerät jederzeit aktualisieren, um das übergeordnete Gerät später hinzuzufügen oder zu ändern. 

   ![Erstellen der Geräte-ID bei der Authentifizierung mit symmetrischem Schlüssel im Portal](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um diesen Vorgang abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit symmetrischen Schlüsseln und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID}
```

Weitere Informationen zu Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräteerstellung und die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.

### <a name="connect-to-iot-hub-through-a-gateway"></a>Herstellen einer Verbindung mit IoT Hub über ein Gateway

Für nachgeschaltete Geräte wird der gleiche Prozess verwendet, mit dem auch reguläre IoT-Geräte mit symmetrischen Schlüsseln bei IoT Hub authentifiziert werden. Der einzige Unterschied ist, dass Sie dem Gatewaygerät einen Zeiger hinzufügen müssen, damit die Verbindung weitergeleitet wird, oder dass die Authentifizierung (bei Offlineszenarien) im Auftrag von IoT Hub verarbeitet werden muss. 

Für die Authentifizierung mit symmetrischen Schlüssels sind keine zusätzlichen Schritte auf dem Gerät erforderlich, damit dieses sich bei IoT Hub authentifizieren kann. Sie benötigen natürlich die Zertifikate, damit das nachgeschaltete Gerät eine Verbindung mit dem Gatewaygerät herstellen kann. Eine Beschreibung finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).

Nachdem Sie im Portal eine IoT-Geräteidentität erstellt haben, können Sie die zugehörigen primären oder sekundären Schlüssel abrufen. Einer dieser Schlüssel muss in der Verbindungszeichenfolge enthalten sein, die Sie in Anwendungen einfügen, die mit IoT Hub kommunizieren. IoT Hub stellt der Einfachheit halber für die Authentifizierung mit symmetrischen Schlüsseln in den Gerätedetails eine vollständige Verbindungszeichenfolge bereit. Sie müssen der Verbindungszeichenfolge zusätzliche Informationen über das Gatewaygerät hinzufügen. 

Verbindungszeichenfolgen für symmetrische Schlüssel für nachgeschaltete Geräte müssen folgende Elemente enthalten: 

* Den IoT-Hub, mit dem das Gerät eine Verbindung herstellt: `Hostname={iothub name}.azure-devices.net`
* Die beim Hub registrierte Geräte-ID: `DeviceID={device ID}`
* Entweder den primären oder den sekundären Schlüssel: `SharedAccessKey={key}`
* Das Gatewaygerät, über das das Gerät die Verbindung herstellt. Sie finden den Wert für **hostname** in der Datei „config.yaml“ des IoT Edge-Gatewaygeräts: `GatewayHostName={gateway hostname}`

Insgesamt sieht eine vollständige Verbindungszeichenfolge wie folgt aus:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Wenn Sie für dieses nachgeschalteten Gerät eine Beziehung mit über- und untergeordnetem Gerät eingerichtet haben, können Sie die Verbindungszeichenfolge vereinfachen, indem Sie das Gateway direkt als Host für die Verbindung aufrufen. Beispiel: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

## <a name="x509-authentication"></a>X.509-Authentifizierung 

Es gibt zwei Möglichkeiten, ein IoT-Gerät mithilfe von X.509-Zertifikaten zu authentifizieren. Die Schritte für das Verbinden Ihres Geräts mit IoT Hub sind unabhängig von der gewählten Authentifizierungsmethode identisch. Wählen Sie entweder selbstsignierte oder von einer Zertifizierungsstelle signierte Zertifikate für die Authentifizierung aus, und fahren Sie dann mit dem Herstellen einer Verbindung mit IoT Hub fort. 

Weitere Informationen dazu, wie IoT Hub die X.509-Authentifizierung verwendet, finden Sie in den folgenden Artikeln: 
* [Geräteauthentifizierung mit X.509-Zertifikaten](../iot-hub/iot-hub-x509ca-overview.md)
* [Konzeptgrundlagen der X.509-Zertifizierungsstellenzertifikate in der IoT-Branche](../iot-hub/iot-hub-x509ca-concept.md)

### <a name="create-the-device-identity-with-x509-self-signed-certificates"></a>Erstellen der Geräteidentität mit selbstsignierten X.509-Zertifikaten

Für die Authentifizierung mit selbstsignierten X.509-Zertifikaten (auch als Fingerabdruck-Authentifizierung bezeichnet) müssen Sie neue Zertifikate erstellen und auf Ihrem IoT-Gerät speichern. Diese Zertifikate verfügen über einen Fingerabdruck, den Sie für die Authentifizierung an IoT Hub übergeben. 

Die einfachste Möglichkeit zum Testen dieses Szenarios stellt die Verwendung desselben Geräts dar, das Sie zum Erstellen von Zertifikaten in [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md) verwendet haben. Dieser Computer sollte bereits mit dem richtigen Tool, dem Zertifikat der Stammzertifizierungsstelle und Zertifikaten der Zwischenzertifizierungsstellen eingerichtet worden sein, bevor Sie Zertifikate für das IoT-Gerät erstellen. Sie können die endgültigen Zertifikate und die zugehörigen privaten Schlüssel später auf das nachgeschaltete Gerät kopieren. Gemäß den Schritten im Artikel zum Gateway haben Sie bereits OpenSSL auf Ihrem Computer eingerichtet und dass IoT Edge-Repository geklont, um auf die Skripts zur Zertifikaterstellung zugreifen zu können. Anschließend haben Sie das Arbeitsverzeichnis **\<WRKDIR>** erstellt, in dem die Zertifikate gespeichert werden. Die Standardzertifikate sind für das Entwickeln und Testen vorgesehen und gelten daher nur 30 Tage. Sie sollten ein Zertifikat der Stammzertifizierungsstelle und eines der Zwischenzertifizierungsstelle erstellt haben. 

1. Navigieren Sie in einem Bash- oder PowerShell-Fenster zu Ihrem Arbeitsverzeichnis. 

2. Erstellen Sie zwei Zertifikate (primär und sekundär) für das nachgeschaltete Gerät. Geben Sie den Gerätenamen und dann die primäre oder sekundäre Bezeichnung an. Diese Informationen werden verwendet, um die Dateien zu benennen, damit Sie die Zertifikate für mehrere Geräte nachverfolgen können. 

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

3. Rufen Sie den SHA1-Fingerabdruck aller Zertifikate ab. Es handelt sich dabei um eine hexadezimale Zeichenfolge mit 40 Zeichen. Verwenden Sie den folgenden OpenSSL-Befehl, um das Zertifikat anzuzeigen und den Fingerabdruck zu suchen:

   ```PowerShell/bash
   openssl x509 -in <WORKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

4. Navigieren Sie im Azure-Portal zu Ihrem IoT-Hub, und erstellen Sie eine neue IoT-Geräteidentität mit den folgenden Werten: 

   * Wählen Sie als Authentifizierungstyp **X.509, selbstsigniert** aus.
   * Fügen Sie die hexadezimalen Zeichenfolgen ein, die Sie aus den primären und sekundären Zertifikaten Ihres Geräts kopiert haben.
   * Wählen Sie **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, über das dieses nachgeschaltete Gerät verbunden wird. Für die X.509-Authentifizierung ist für ein nachgeschaltetes Gerät ein übergeordnetes Gerät erforderlich. 

   ![Erstellen der Geräte-ID bei der Authentifizierung mit selbstsignierten X.509-Zertifikaten im Portal](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

5. Kopieren Sie die folgenden Dateien in ein beliebiges Verzeichnis auf dem nachgeschalteten Gerät:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device name>*.key.pem`

   Sie verweisen auf diese Dateien in den Blattgeräteanwendungen, die eine Verbindung mit IoT Hub herstellen. Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um den Vorgang zur Geräteerstellung abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit selbstsignierten X.509-Zertifikaten und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Weitere Informationen zu den Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräte- und Zertifikaterstellung sowie die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.

### <a name="create-the-device-identity-with-x509-ca-signed-certificates"></a>Erstellen der Geräteidentität mit von einer Zertifizierungsstelle signierten X.509-Zertifikaten

Für die Authentifizierung mit X.509-Zertifikaten, die von einer Zertifizierungsstelle (ZS) signiert wurden, muss in IoT Hub ein Zertifikat einer Stammzertifizierungsstelle registriert sein, mit dem Sie die Zertifikate für Ihre IoT-Geräte signieren. Jedes Gerät mit einem Zertifikat, das durch das Zertifikat der Stammzertifizierungsstelle oder einer seiner Zwischenzertifizierungsstellen ausgegeben wurde, wird zur Authentifizierung zugelassen. 

Dieser Abschnitt baut auf den Anweisungen im IoT Hub-Artikel [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md) auf. Befolgen Sie die Schritte in diesem Abschnitt, damit Sie die Werte kennen, die Sie für das Einrichten eines nachgeschalteten Geräts benötigen, das eine Verbindung über ein Gateway herstellt. 

Die einfachste Möglichkeit zum Testen dieses Szenarios stellt die Verwendung desselben Geräts dar, das Sie zum Erstellen von Zertifikaten in [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md) verwendet haben. Dieser Computer sollte bereits mit dem richtigen Tool, dem Zertifikat der Stammzertifizierungsstelle und Zertifikaten der Zwischenzertifizierungsstellen eingerichtet worden sein, bevor Sie Zertifikate für das IoT-Gerät erstellen. Sie können die endgültigen Zertifikate und die zugehörigen privaten Schlüssel später auf das nachgeschaltete Gerät kopieren. Gemäß den Schritten im Artikel zum Gateway haben Sie bereits OpenSSL auf Ihrem Computer eingerichtet und dass IoT Edge-Repository geklont, um auf die Skripts zur Zertifikaterstellung zugreifen zu können. Anschließend haben Sie das Arbeitsverzeichnis **\<WRKDIR>** erstellt, in dem die Zertifikate gespeichert werden. Die Standardzertifikate sind für das Entwickeln und Testen vorgesehen und gelten daher nur 30 Tage. Sie sollten ein Zertifikat der Stammzertifizierungsstelle und eines der Zwischenzertifizierungsstelle erstellt haben. 

1. Befolgen Sie die Anweisungen im Abschnitt [Registrieren der X.509-Zertifizierungsstellenzertifikate bei IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) unter *Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub*. In diesem Abschnitt führen Sie die folgenden Schritte aus: 

   1. Laden Sie ein Zertifikat der Stammzertifizierungsstelle hoch. Wenn Sie die Zertifikate verwenden, die Sie im Artikel zum transparenten Gateway erstellt haben, laden Sie **\<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem** als Stammzertifikatsdatei hoch. 
   2. Stellen Sie sicher, dass Sie Besitzer des Zertifikats der Stammzertifizierungsstelle sind. Sie können den Besitz mit den Zertifikattools in \<WRKDIR> überprüfen. 

      ```powershell
      New-CACertsVerificationCert "<verification code from Azure portal>"
      ```

      ```bash
      ./certGen.sh create_verification_certificate <verification code from Azure portal>"
      ```

2. Befolgen Sie die Anweisungen im Abschnitt [Erstellen eines X.509-Geräts für Ihren IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) unter *Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub*. In diesem Abschnitt führen Sie die folgenden Schritte aus: 

   1. Fügen Sie ein neues Gerät hinzu. Geben Sie in **Geräte-ID** einen Namen in Kleinbuchstaben an, und wählen Sie als Authentifizierungstyp **X.509, durch Zertifizierungsstelle signiert** aus. 
   2. Legen Sie ein übergeordnetes Gerät fest. Wählen Sie für nachgeschaltete Geräte **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, das die Verbindung mit IoT Hub bereitstellt. 

3. Erstellen Sie eine Vertrauenskette für das nachgeschaltete Gerät. Verwenden Sie dasselbe Zertifikat der Stammzertifizierungsstelle, das Sie zum Erstellen dieser Kette in IoT Hub hochgeladen haben. Verwenden Sie dieselbe Geräte-ID in Kleinbuchstaben, die Sie der Geräteidentität im Portal zugewiesen haben.

   ```powershell
   New-CACertsDevice "<device id>"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

4. Kopieren Sie die folgenden Dateien in ein beliebiges Verzeichnis auf dem nachgeschalteten Gerät: 

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device id>*.key.pem`

   Sie verweisen auf diese Dateien in den Blattgeräteanwendungen, die eine Verbindung mit IoT Hub herstellen. Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um den Vorgang zur Geräteerstellung abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit X.509-Zertifikaten, die durch eine Zertifizierungsstelle signiert wurden, und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Weitere Informationen zu Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräteerstellung und die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.


### <a name="connect-to-iot-hub-through-a-gateway"></a>Herstellen einer Verbindung mit IoT Hub über ein Gateway

Jedes Azure IoT SDK verarbeitet die X.509-Authentifizierung ein wenig anders. Für nachgeschaltete Geräte wird aber der gleiche Prozess verwendet, mit dem auch reguläre IoT-Geräte mit X.509-Zertifikaten bei IoT Hub authentifiziert werden. Der einzige Unterschied ist, dass Sie dem Gatewaygerät einen Zeiger hinzufügen müssen, damit die Verbindung weitergeleitet wird, oder dass die Authentifizierung (bei Offlineszenarien) im Auftrag von IoT Hub verarbeitet werden muss. Im Allgemeinen können Sie die gleichen Schritte für die X.509-Authentifizierung für alle IoT Hub-Geräte verwenden. Ersetzen Sie dann einfach den Wert von **Hostname** in der Verbindungszeichenfolge durch den Hostnamen Ihres Gatewaygeräts. 

Die folgenden Abschnitte zeigen Beispiele für die verschiedenen SDK-Sprachen. 

>[!IMPORTANT]
>Die folgenden Beispiele veranschaulichen, wie die IoT Hub-SDKs Zertifikate verwenden, um Geräte zu authentifizieren. In einer Produktionsbereitstellung sollten Sie alle Geheimnisse wie private oder SAS-Schlüssel in einem Hardwaresicherheitsmodul (HSM) speichern. 

#### <a name="net"></a>.NET

Ein Beispiel für ein C# Programm, das sich mit X.509-Zertifikaten bei IoT Hub authentifiziert, finden Sie unter [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates). Einige der wichtigsten Zeilen dieses Beispiels sind hier aufgeführt, um den Authentifizierungsprozess zu veranschaulichen.

Wenn Sie den Hostnamen für Ihre DeviceClient-Instanz deklarieren, verwenden Sie den Hostnamen des IoT Edge-Gatewaygeräts. Sie finden den Hostnamen in der Datei „config.yaml“ des Gatewaygeräts. 

Wenn Sie die Testzertifikate aus dem IoT Edge-Git-Repository verwenden, lautet der Schlüssel für die Zertifikate **1234**.

```csharp
try
{
    var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
    var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
    var deviceClient = DeviceClient.Create("<gateway hostname>", auth, TransportType.Amqp_Tcp_Only);

    if (deviceClient == null)
    {
        Console.WriteLine("Failed to create DeviceClient!");
    }
    else
    {
        Console.WriteLine("Successfully created DeviceClient!");
        SendEvent(deviceClient).Wait();
    }

    Console.WriteLine("Exiting...\n");
}
catch (Exception ex)
{
    Console.WriteLine("Error in sample: {0}", ex.Message);
}
```

#### <a name="c"></a>C

Ein Beispiel für ein C-Programm, das sich mit X.509-Zertifikaten bei IoT Hub authentifiziert, finden Sie im Beispiel [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample) im IoT-SDK für C. Einige der wichtigsten Zeilen dieses Beispiels sind hier aufgeführt, um den Authentifizierungsprozess zu veranschaulichen.

Verwenden Sie beim Festlegen der Verbindungszeichenfolge für das nachgeschaltete Gerät den Hostnamen des IoT Edge-Gatewaygeräts für den **HostName**-Parameter. Sie finden den Hostnamen in der Datei „config.yaml“ des Gatewaygeräts. 

```C
// If your downstream device uses X.509 authentication (self signed or X.509 CA) then
// resulting connection string should look like the following:
// "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
static const char* connectionString = "[Downstream device IoT Edge connection string]";

// Path to the Edge "owner" root CA certificate
static const char* edge_ca_cert_path = "[Path to root CA certificate]";

// When the downstream device uses X.509 authentication, a certificate and key 
// in PRM format must be provided.
static const char * x509_device_cert_path = "[Path to primary or secondary device cert]";
static const char * x509_device_key_path = "[Path to primary or secondary device key]";

int main(void)
{
    // Create the iothub handle here
    device_handle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, protocol);

    // Provide the Azure IoT device client with the same root
    // X509 CA certificate that was used to set up the IoT Edge gateway runtime
    if (edge_ca_cert_path != NULL)
    {
        cert_string = obtain_edge_ca_certificate();
        (void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
    }

    if ((x509_device_cert_path != NULL) && (x509_device_key_path != NULL))
    {
        const char *x509certificate = obtain_file_contents(x509_device_cert_path);
        const char *x509privatekey = obtain_file_contents(x509_device_key_path);
        if ((IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_CERT, x509certificate) != IOTHUB_CLIENT_OK) ||
            (IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_PRIVATE_KEY, x509privatekey) != IOTHUB_CLIENT_OK)
            )
        {
            printf("failure to set options for x509, aborting\r\n");
            exit(1);
        }
    }
}
```

#### <a name="nodejs"></a>Node.js

Ein Beispiel für ein Node.js-Programm, das sich mit X.509-Zertifikaten bei IoT Hub authentifiziert, finden Sie im Beispiel [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js) im IoT-SDK für Node.js. Einige der wichtigsten Zeilen dieses Beispiels sind hier aufgeführt, um den Authentifizierungsprozess zu veranschaulichen.

Verwenden Sie beim Festlegen der Verbindungszeichenfolge für das nachgeschaltete Gerät den Hostnamen des IoT Edge-Gatewaygeräts für den **HostName**-Parameter. Sie finden den Hostnamen in der Datei „config.yaml“ des Gatewaygeräts. 

Wenn Sie die Testzertifikate aus dem IoT Edge-Git-Repository verwenden, lautet der Schlüssel für die Zertifikate **1234**.

```node
// String containing Hostname and Device Id in the following format:
//  "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
var connectionString = '<DEVICE CONNECTION STRING WITH x509=true>';
var certFile = '<PATH-TO-CERTIFICATE-FILE>';
var keyFile = '<PATH-TO-KEY-FILE>';
var passphrase = '<KEY PASSPHRASE IF ANY>';

// fromConnectionString must specify a transport constructor, coming from any transport package.
var client = Client.fromConnectionString(connectionString, Protocol);

var options = {
   cert : fs.readFileSync(certFile, 'utf-8').toString(),
   key : fs.readFileSync(keyFile, 'utf-8').toString(),
   passphrase: passphrase
 };

// Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client transport to use x509 when connecting to IoT Hub
client.setOptions(options);
```

#### <a name="python"></a>Python

Das Python SDK unterstützt zurzeit nur die Verwendung von X509-Zertifikaten und Schlüsseln aus Dateien, keine inline definierten Schlüssel. Im folgenden Beispiel werden relevante Dateipfade in Umgebungsvariablen gespeichert.

Verwenden Sie beim Definieren des Hostnamens für das nachgeschaltete Gerät den Hostnamen des IoT Edge-Gatewaygeräts für den **HostName**-Parameter. Sie finden den Hostnamen in der Datei „config.yaml“ des Gatewaygeräts. 

```python
import os
from azure.iot.device import IoTHubDeviceClient, X509

HOSTNAME = "[IoT Edge Gateway Hostname]"
DEVICE_ID = "[Device ID]"

def iothub_client_init():
    x509 = X509(
        cert_file=os.getenv("X509_CERT_FILE"),
        key_file=os.getenv("X509_KEY_FILE")
    )

    client = IoTHubDeviceClient.create_from_x509_certificate(
        x509=x509,
        hostname=HOSTNAME,
        device_id=DEVICE_ID
    )
)

if __name__ == '__main__':
    iothub_client_init()
```

#### <a name="java"></a>Java

Ein Beispiel für ein Java-Programm, das sich mit X.509-Zertifikaten bei IoT Hub authentifiziert, finden Sie im Beispiel [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) im IoT-SDK für Java. Einige der wichtigsten Zeilen dieses Beispiels sind hier aufgeführt, um den Authentifizierungsprozess zu veranschaulichen.

Verwenden Sie beim Festlegen der Verbindungszeichenfolge für das nachgeschaltete Gerät den Hostnamen des IoT Edge-Gatewaygeräts für den **HostName**-Parameter. Sie finden den Hostnamen in der Datei „config.yaml“ des Gatewaygeräts. 

```java
//PEM encoded representation of the public key certificate
private static String publicKeyCertificateString =
    "-----BEGIN CERTIFICATE-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END CERTIFICATE-----\n";

//PEM encoded representation of the private key
private static String privateKeyString =
    "-----BEGIN EC PRIVATE KEY-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END EC PRIVATE KEY-----\n";

DeviceClient client = new DeviceClient(connectionString, protocol, publicKeyCertificateString, false, privateKeyString, false);
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben beim Durcharbeiten dieses Artikels ein IoT Edge-Geräts erstellt, das als transparentes Gateway fungiert, sowie ein nachgeschaltetes Gerät, das bei einem IoT-Hub registriert ist. Als Nächstes müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gatewaygerät vertrauen und Nachrichten an dieses senden. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).
