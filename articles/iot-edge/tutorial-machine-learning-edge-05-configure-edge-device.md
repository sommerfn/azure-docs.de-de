---
title: 'Konfigurieren eines IoT Edge-Geräts: Machine Learning in Azure IoT Edge | Microsoft-Dokumentation'
description: Konfigurieren Sie einen virtuellen Azure-Computer mit Linux als Azure IoT Edge-Gerät, das als transparentes Gateway fungiert.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b9f9fe78db2d8bcf50a076fdfc3eba7b1f347201
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72965395"
---
# <a name="tutorial-configure-an-iot-edge-device"></a>Tutorial: Konfigurieren eines IoT Edge-Geräts

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

In diesem Artikel konfigurieren wir einen virtuellen Azure-Computer mit Linux als Azure IoT Edge-Gerät, das als transparentes Gateway fungiert. Die Konfiguration der transparenten Gateways ermöglicht Geräten die Herstellung einer Verbindung mit Azure IoT Hub über das Gateway, ohne zu wissen, dass das Gateway vorhanden ist. Gleichzeitig ist auch ein Benutzer, der mit den Geräten in IoT Hub interagiert, nicht über das zwischengeschaltete Gatewaygerät informiert. Wir verwenden das transparente Gateway letztendlich, um unserem System eine Edgeanalyse hinzuzufügen, indem wir auf dem Gateway IoT Edge-Module bereitstellen.

Die Schritte in diesem Artikel werden normalerweise von einem Cloudentwickler ausgeführt.

## <a name="generate-certificates"></a>Generieren von Zertifikaten

Damit ein Gerät als Gateway fungieren kann, muss es in der Lage sein, sich sicher mit nachgeschalteten Geräten zu verbinden. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des IoT Edge-Geräts bestätigen. Weitere Informationen dazu, wie IoT Edge-Geräte Zertifikate verwenden, finden Sie unter [Details zur Verwendung von Azure IoT Edge-Zertifikaten](iot-edge-certs.md).

In diesem Abschnitt erstellen wir die selbstsignierten Zertifikate mit einem Docker-Image, das wir anschließend erstellen und ausführen. Wir haben uns bei diesem Schritt für ein Docker-Image entschieden, weil sich hierdurch der Aufwand zum Erstellen der Zertifikate auf dem Windows-Entwicklungscomputer deutlich verringert. Ausführlichere Informationen zur Erstellung von Zertifikaten ohne Container finden Sie unter [Generieren von Zertifikaten unter Windows](how-to-create-transparent-gateway.md#generate-certificates-with-windows). Unter [Generieren von Zertifikaten unter Linux](how-to-create-transparent-gateway.md#generate-certificates-with-linux) finden Sie die Anweisungen, die wir mit dem Docker-Image automatisiert haben.

1. Melden Sie sich auf Ihrem virtuellen Entwicklungscomputer an.

2. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl zum Erstellen eines Verzeichnisses auf dem virtuellen Computer aus.

    ```cmd
    mkdir c:\edgeCertificates
    ```

3. Starten Sie **Docker für Windows** über das Windows-Startmenü.

4. Öffnen Sie Visual Studio Code.

5. Wählen Sie **Datei** > **Ordner öffnen...** und dann **C:\\source\\IoTEdgeAndMlSample\\CreateCertificates**.

6. Klicken Sie mit der rechten Maustaste auf die Dockerfile, und wählen Sie **Image erstellen**.

7. Übernehmen Sie im Dialogfeld den Standardwert für den Imagenamen und das Tag: **createcertificates:latest**.

8. Warten Sie, bis der Buildvorgang abgeschlossen wird.

    > [!NOTE]
    > Unter Umständen wird eine Warnung zu einem fehlenden öffentlichen Schlüssel angezeigt. Sie können diese Warnung ignorieren. Sie sehen auch eine Sicherheitswarnung mit dem Hinweis, dass Sie die Berechtigungen auf Ihrem Image überprüfen bzw. zurücksetzen sollten. Sie können diesen Hinweis für dieses Image ignorieren.

9. Führen Sie im Visual Studio Code-Terminalfenster den Container „createcertificates“ aus.

    ```cmd
    docker run --name createcertificates --rm -v c:\edgeCertificates:/edgeCertificates createcertificates /edgeCertificates
    ```

10. Docker fordert Sie auf, Zugriff auf das Laufwerk **c:\\** zu gewähren. Wählen Sie **Teilen**.

11. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

12. Überprüfen Sie nach Abschluss der Containerausführung, ob die folgenden Dateien unter **c:\\edgeCertificates** vorhanden sind:

    * c:\\edgeCertificates\\certs\\azure-iot-test-only.root.ca.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device-full-chain.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device.cert.pfx
    * c:\\edgeCertificates\\private\\new-edge-device.key.pem

## <a name="upload-certificates-to-azure-key-vault"></a>Hochladen von Zertifikaten in Azure Key Vault

Wir laden die Zertifikate in Azure Key Vault hoch, um unsere Zertifikate sicher zu speichern und den Zugriff darauf von mehreren Geräten aus zu ermöglichen. In der obigen Liste ist zu sehen, dass zwei Arten von Zertifikatdateien vorhanden sind: PFX und PEM. Wir behandeln PFX-Dateien als Key Vault-Zertifikate, die in Key Vault hochgeladen werden. Die PEM-Dateien sind Nur-Text-Dateien und werden als Key Vault-Geheimnisse behandelt. Wir verwenden die Key Vault-Instanz, die dem Azure Machine Learning Service-Arbeitsbereich zugeordnet ist, den wir durch das Ausführen der [Azure Notebooks](tutorial-machine-learning-edge-04-train-model.md#run-azure-notebooks) erstellt haben.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Machine Learning Service-Arbeitsbereich.

2. Suchen Sie auf der Übersichtsseite des Azure Machine Learning Service-Arbeitsbereichs nach dem Namen der **Key Vault**-Instanz.

    ![Kopieren des Key Vault-Namens](media/tutorial-machine-learning-edge-05-configure-edge-device/find-key-vault-name.png)

3. Laden Sie auf Ihrem Entwicklungscomputer die Zertifikate in Key Vault hoch. Ersetzen Sie **\<subscriptionId\>** und **\<keyvaultname\>** durch Ihre Ressourceninformationen.

    ```powershell
    c:\source\IoTEdgeAndMlSample\CreateCertificates\upload-keyvaultcerts.ps1 -SubscriptionId <subscriptionId> -KeyVaultName <keyvaultname>
    ```

4. Melden Sie sich an Azure an, wenn Sie dazu aufgefordert werden.

5. Das Skript wird einige Minuten lang ausgeführt, und in der Ausgabe sind die neuen Key Vault-Einträge aufgelistet.

    ![Key Vault-Skriptausgabe](media/tutorial-machine-learning-edge-05-configure-edge-device/key-vault-entries-output.png)

## <a name="create-iot-edge-device"></a>IoT Edge-Gerät erstellen

Für die Herstellung einer Verbindung für ein Azure IoT Edge-Gerät mit einem IoT-Hub erstellen wir zuerst im Hub eine Identität für das Gerät. Wir verwenden die Verbindungszeichenfolge aus der Geräteidentität in der Cloud und nutzen sie zum Konfigurieren der Runtime auf unserem IoT Edge-Gerät. Nachdem das Gerät konfiguriert wurde und eine Verbindung mit dem Hub hergestellt hat, können wir Module bereitstellen und Nachrichten senden. Wir können auch die Konfiguration des physischen IoT Edge-Geräts ändern, indem wir die Konfiguration der entsprechenden Geräteidentität im IoT-Hub ändern.

In diesem Tutorial erstellen wir die neue Geräteidentität mit Visual Studio Code. Sie können diese Schritte auch über das [Azure-Portal](how-to-register-device.md#register-in-the-azure-portal) oder die [Azure CLI](how-to-register-device.md#register-with-the-azure-cli) ausführen.

1. Öffnen Sie Visual Studio Code auf Ihrem Entwicklungscomputer.

2. Öffnen Sie in der Explorer-Ansicht von Visual Studio Code den Bereich **Azure IoT Hub-Geräte**.

3. Klicken Sie auf die Auslassungspunkte, und wählen Sie **IoT Edge-Gerät erstellen**.

4. Geben Sie dem Gerät einen Namen. Der Einfachheit halber verwenden wir **aaTurbofanEdgeDevice**, damit das Gerät an erster Stelle der Liste mit den Clientgeräten steht, die wir zuvor über die Geräteumgebung zum Senden der Testdaten erstellt haben.

5. Das neue Gerät wird in der Liste mit den Geräten angezeigt.

    ![Anzeigen des neuen Geräts „aaTurbofanEdgeDevice“ im Explorer von VS Code](media/tutorial-machine-learning-edge-05-configure-edge-device/iot-hub-devices-list.png)

## <a name="deploy-azure-virtual-machine"></a>Bereitstellen eines virtuellen Azure-Computers

Wir verwenden das Image [Azure IoT Edge unter Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) vom Azure Marketplace, um unser IoT Edge-Gerät für dieses Tutorial zu erstellen. Mit dem Image „Azure IoT Edge unter Ubuntu“ werden beim Starten die aktuelle Azure IoT Edge-Runtime und die zugehörigen Abhängigkeiten installiert. Wir stellen die VM mit dem PowerShell-Skript `Create-EdgeVM.ps1`, der Resource Manager-Vorlage `IoTEdgeVMTemplate.json` und dem Shellskript `install packages.sh` bereit.

### <a name="enable-programmatic-deployment"></a>Aktivieren der programmgesteuerten Bereitstellung

Zum Verwenden des Images vom Marketplace für eine Bereitstellung per Skript müssen wir die programmgesteuerte Bereitstellung für das Image aktivieren.

1. Melden Sie sich beim Azure-Portal an.

1. Wählen Sie **Alle Dienste** aus.

1. Geben Sie in der Suchleiste **Marketplace** ein, und wählen Sie den entsprechenden Eintrag aus.

1. Geben Sie in der Suchleiste **Azure IoT Edge unter Ubuntu** ein, und wählen Sie den entsprechenden Eintrag aus.

1. Wählen Sie den Hyperlink **Möchten Sie die programmgesteuerte Bereitstellung konfigurieren? Erste Schritte**.

1. Wählen Sie die Schaltfläche **Aktivieren** und dann **Speichern**.

    ![Aktivieren der programmgesteuerten Bereitstellung für die VM](media/tutorial-machine-learning-edge-05-configure-edge-device/deploy-ubuntu-vm.png)

1. Eine Erfolgsmeldung wird angezeigt.

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Führen Sie als Nächstes das Skript zum Erstellen des virtuellen Computers für Ihr IoT Edge-Gerät aus.

1. Öffnen Sie ein PowerShell-Fenster, und navigieren Sie zum Verzeichnis **EdgeVM**.

    ```powershell
    cd c:\source\IoTEdgeAndMlSample\EdgeVM
    ```

2. Führen Sie das Skript aus, um den virtuellen Computer zu erstellen.

    ```powershell
    .\Create-EdgeVm.ps1
    ```

3. Geben Sie einen Wert für jeden Parameter an, wenn Sie dazu aufgefordert werden. Wir empfehlen Ihnen, für das Abonnement, die Ressourcengruppe und den Standort die gleichen Angaben wie für alle anderen Ressourcen im Rahmen dieses Tutorials anzugeben.

    * **Azure-Abonnement-ID**: Über das Azure-Portal ermittelbar.
    * **Ressourcengruppenname**: Einprägsamer Name für die Gruppierung der Ressourcen für dieses Tutorial.
    * **Standort**: Azure-Standort, an dem der virtuelle Computer erstellt wird. Beispiel: „westus2“ oder „northeurope“. Weitere Standorte finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).
    * **AdminUsername**: Der Name für das Administratorkonto, das Sie zum Anmelden am virtuellen Computer verwenden.
    * **AdminPassword**: Das Kennwort, das für den AdminUsername auf dem virtuellen Computer festgelegt wird.

4. Damit die VM mit dem Skript eingerichtet werden kann, müssen Sie sich bei Azure mit den Anmeldeinformationen anmelden, die dem von Ihnen genutzten Azure-Abonnement zugeordnet sind.

5. Das Skript bestätigt die Informationen für die Erstellung Ihres virtuellen Computers. Drücken Sie **y** oder die **EINGABETASTE**, um fortzufahren.

6. Die Ausführung des Skripts dauert mehrere Minuten, und es werden die folgenden Schritte ausgeführt:

    * Erstellen der Ressourcengruppe, falls sie noch nicht vorhanden ist
    * Erstellen des virtuellen Computers
    * Hinzufügen von NSG-Ausnahmen für die VM für Ports 22 (SSH), 5671 (AMQP), 5672 (AMPQ) und 443 (SSL)
    * Installieren der [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)

7. Das Skript gibt die SSH-Verbindungszeichenfolge für die Herstellung der Verbindung mit der VM aus. Kopieren Sie die Verbindungszeichenfolge für den nächsten Schritt.

    ![Kopieren der SSH-Verbindungszeichenfolge für die VM](media/tutorial-machine-learning-edge-05-configure-edge-device/vm-ssh-connection-string.png)

## <a name="connect-to-your-iot-edge-device"></a>Herstellen einer Verbindung mit Ihrem IoT Edge-Gerät

In den nächsten Abschnitten wird der von uns erstellte virtuelle Azure-Computer konfiguriert. Der erste Schritt ist die Herstellung der Verbindung mit dem virtuellen Computer.

1. Öffnen Sie eine Eingabeaufforderung, und fügen Sie die SSH-Verbindungszeichenfolge ein, die Sie aus der Ausgabe des Skripts kopiert haben. Geben Sie Ihre eigenen Informationen für Benutzername, Suffix und Region gemäß den Werten ein, die Sie im vorherigen Abschnitt für das PowerShell-Skript angegeben haben.

    ```cmd
    ssh -l <username> iotedge-<suffix>.<region>.cloudapp.azure.com
    ```

2. Wählen Sie **Ja**, und drücken Sie die **EINGABETASTE**, wenn Sie zum Überprüfen der Echtheit des Hosts aufgefordert werden.

3. Geben Sie bei entsprechender Aufforderung Ihr Kennwort an.

4. In Ubuntu wird eine Begrüßungsnachricht eingeblendet, und dann sollte in etwa `<username>@<machinename>:~$` angezeigt werden.

## <a name="download-key-vault-certificates"></a>Herunterladen von Key Vault-Zertifikaten

Weiter oben in diesem Artikel haben wir Zertifikate in Key Vault hochgeladen, um sie für unser IoT Edge-Gerät und Blattgerät bereitzustellen. Hierbei handelt es sich um ein nachgeschaltetes Gerät, von dem das IoT Edge-Gerät als Gateway für die Kommunikation mit IoT Hub verwendet wird. Wir gehen zu einem späteren Zeitpunkt des Tutorials auf das Blattgerät ein. In diesem Abschnitt laden Sie die Zertifikate auf das IoT Edge-Gerät herunter.

1. Melden Sie sich über die SSH-Sitzung auf dem virtuellen Linux-Computer mit der Azure CLI an Azure an.

    ```bash
    az login
    ```

1. Sie werden aufgefordert, <https://microsoft.com/devicelogin> in einem Browser zu öffnen und einen eindeutigen Code anzugeben. Sie können diese Schritte auf Ihrem lokalen Computer ausführen. Schließen Sie das Browserfenster, wenn Sie die Authentifizierung abgeschlossen haben.

1. Wenn die Authentifizierung erfolgreich war, wird der virtuelle Linux-Computer angemeldet, und Ihre Azure-Abonnements werden aufgelistet.

1. Legen Sie das Azure-Abonnement fest, das Sie für Azure CLI-Befehle verwenden möchten.

    ```bash
    az account set --subscription <subscriptionId>
    ```

1. Erstellen Sie auf der VM ein Verzeichnis für die Zertifikate.

    ```bash
    sudo mkdir /edgeMlCertificates
    ```

1. Laden Sie die Zertifikate herunter, die Sie im Schlüsseltresor gespeichert haben: „new-edge-device-full-chain.cert.pem“, „new-edge-device.key.pem“ und „azure-iot-test-only.root.ca.cert.pem“.

    ```bash
    key_vault_name="<key vault name>"
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-full-chain-cert-pem -f /edgeMlCertificates/new-edge-device-full-chain.cert.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-key-pem -f /edgeMlCertificates/new-edge-device.key.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name azure-iot-test-only-root-ca-cert-pem -f /edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem
    ```

## <a name="update-the-iot-edge-device-configuration"></a>Aktualisieren der Konfiguration für das IoT Edge-Gerät

Für die IoT Edge-Runtime wird die Datei „/etc/iotedge/config.yaml“ genutzt, um die Konfiguration zu speichern. Wir müssen in dieser Datei drei Arten von Informationen aktualisieren:

* **Geräte-Verbindungszeichenfolge**: Die Verbindungszeichenfolge der Identität dieses Geräts in IoT Hub.
* **Zertifikate:** Die Zertifikate, die für die Verbindungsherstellung mit nachgeschalteten Geräten verwendet werden.
* **Hostname:** Der vollqualifizierte Domänenname (FQDN) des IoT Edge-Geräts der VM.

Das Image *Azure IoT Edge unter Ubuntu*, das wir zum Erstellen des virtuellen IoT Edge-Computers verwendet haben, verfügt über ein Shellskript, mit dem die Datei „config.yaml“ mit der Verbindungszeichenfolge aktualisiert wird.

1. Klicken Sie in Visual Studio Code mit der rechten Maustaste auf das IoT Edge-Gerät, und wählen Sie anschließend die Option **Copy Device Connection String** (Verbindungszeichenfolge des Geräts kopieren).

    ![Kopieren der Verbindungszeichenfolge aus Visual Studio Code](media/tutorial-machine-learning-edge-05-configure-edge-device/copy-device-connection-string-command.png)

2. Führen Sie in Ihrer SSH-Sitzung den Befehl zum Aktualisieren der Datei „config.yaml“ mit Ihrer Geräteverbindungszeichenfolge aus.

    ```bash
    sudo /etc/iotedge/configedge.sh "<your_iothub_edge_device_connection_string>"
    ```

Als Nächstes aktualisieren wir die Zertifikate und den Hostnamen, indem wir die Datei „config.yaml“ direkt bearbeiten.

1. Öffnen Sie die Datei „config.yaml“.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

2. Aktualisieren Sie den Abschnitt „certificates“ der Datei „config.yaml“, indem Sie das vorangestellte Zeichen `#` entfernen und den Pfad festlegen, damit die Datei wie im folgenden Beispiel aussieht:

    ```yaml
    certificates:
      device_ca_cert: "/edgeMlCertificates/new-edge-device-full-chain.cert.pem"
      device_ca_pk: "/edgeMlCertificates/new-edge-device.key.pem"
      trusted_ca_certs: "/edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem"
    ```

    Stellen Sie sicher, dass „certificates:“ keine Leerzeichen vorangestellt sind und dass vor den Zertifikaten jeweils zwei Leerstellen eingefügt wurden.

    Wenn Sie in Nano mit der rechten Maustaste klicken, wird der Inhalt Ihrer Zwischenablage an der aktuellen Cursorposition eingefügt. Verwenden Sie zum Ersetzen der Zeichenfolge die Pfeiltasten auf der Tastatur, um zur Zeichenfolge zu navigieren, die ersetzt werden soll. Löschen Sie die Zeichenfolge, und klicken Sie dann mit der rechten Maustaste, um das Einfügen aus dem Puffer durchzuführen.

3. Navigieren Sie im Azure-Portal zu Ihrem virtuellen Computer. Kopieren Sie den DNS-Namen (FQDN des Computers) aus dem Abschnitt **Übersicht**.

4. Fügen Sie den FQDN in den Bereich „hostname“ der Datei „config.yml“ ein. Stellen Sie sicher, dass der Name nur Kleinbuchstaben enthält.

    ```yaml
    hostname: '<machinename>.<region>.cloudapp.azure.com'
    ```

5. Speichern und schließen Sie die Datei (`Ctrl + X`, `Y`, `Enter`).

6. Starten Sie den Daemon „iotedge“ neu.

    ```bash
    sudo systemctl restart iotedge
    ```

7. Überprüfen Sie den Status des IoT Edge-Daemon. (Geben Sie nach dem Befehl „:q“ ein, um den Vorgang zu beenden.)

    ```bash
    systemctl status iotedge
    ```

8. Falls im Statusbereich Fehler angezeigt werden (farbiger Text mit dem Präfix „\[ERROR\]“), sollten Sie die Daemonprotokolle auf ausführliche Informationen zum Fehler untersuchen.

    ```bash
    journalctl -u iotedge --no-pager --no-full
    ```

## <a name="next-steps"></a>Nächste Schritte

Wir haben die Konfiguration einer Azure-VM als transparentes Azure IoT Edge-Gateway durchgeführt. Zuerst haben wir Testzertifikate generiert, die wir in Azure Key Vault hochgeladen haben. Anschließend haben wir ein Skript und eine Resource Manager-Vorlage verwendet, um die VM mit dem Image „Ubuntu Server 16.04 LTS + Azure IoT Edge-Runtime“ über den Azure Marketplace bereitzustellen. Im Skript wurde außerdem zusätzlich die Azure CLI installiert ([Installieren der Azure CLI mit apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt)). Nachdem die VM eingerichtet und ausgeführt wurde, haben wir eine Verbindung per SSH hergestellt, die Anmeldung bei Azure durchgeführt, Zertifikate aus Key Vault heruntergeladen und mehrere Updates an der Konfiguration der IoT Edge-Runtime vorgenommen, indem wir die Datei „config.yaml“ aktualisiert haben. Weitere Informationen zur Nutzung von IoT Edge als Gateway finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](iot-edge-as-gateway.md). Weitere Informationen zur Konfiguration eines IoT Edge-Geräts als transparentes Gateway finden Sie unter [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md).

Fahren Sie mit dem nächsten Artikel fort, um IoT Edge-Module zu erstellen.

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen von benutzerdefinierten IoT Edge-Modulen](tutorial-machine-learning-edge-06-custom-modules.md)
