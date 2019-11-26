---
title: 'Tutorial: Einrichten einer Umgebung: Machine Learning in Azure IoT Edge'
description: 'Tutorial: Es wird beschrieben, wie Sie Ihre Umgebung für die Entwicklung und Bereitstellung von Modulen für maschinelles Lernen im Edgebereich vorbereiten.'
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2ea4248ebaedd318e4112e41169f72bc80b1120f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114072"
---
# <a name="tutorial-set-up-an-environment-for-machine-learning-on-iot-edge"></a>Tutorial: Einrichten einer Umgebung für maschinelles Lernen in IoT Edge

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

Dieser Artikel der umfassenden Tutorialreihe zum Thema Azure Machine Learning in IoT Edge dient Ihnen als Hilfe beim Vorbereiten Ihrer Umgebung für die Entwicklung und Bereitstellung. Richten Sie zuerst einen Entwicklungscomputer mit allen benötigten Tools ein. Erstellen Sie anschließend die erforderlichen Cloudressourcen in Azure.

## <a name="set-up-a-development-machine"></a>Einrichten eines Entwicklungscomputers

Dieser Schritt wird normalerweise von einem Cloudentwickler ausgeführt. Ein Teil der Software ist ggf. auch für einen Data Scientist hilfreich.

Im Laufe dieses Artikels führen wir verschiedene Entwickleraufgaben durch, z. B. das Codieren, Kompilieren, Konfigurieren und Bereitstellen von IoT Edge-Modulen und IoT-Geräten. Der Einfachheit halber haben wir ein PowerShell-Skript zusammengestellt, mit dem ein virtueller Azure-Computer erstellt wird, für den viele erforderliche Komponenten bereits konfiguriert sind. Die von uns erstellte VM muss die [geschachtelte Virtualisierung](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) verarbeiten können. Aus diesem Grund haben wir einen Computer mit der Größe [Standard_D8s_v3](../virtual-machines/windows/sizes-general.md#dsv3-series-1) gewählt.

Für die Einrichtung des virtuellen Entwicklungscomputers wird Folgendes verwendet:

* Windows 10
* [Chocolatey](https://chocolatey.org/)
* [Docker Desktop für Windows](https://www.docker.com/products/docker-desktop)
* [Git für Windows](https://gitforwindows.org/)
* [Git Credential Manager für Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)
* [.Net Core SDK](https://dotnet.microsoft.com/)
* [Python 3](https://www.python.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0)
* [VS Code-Erweiterungen](https://marketplace.visualstudio.com/search?target=VSCode)
  * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
  * [PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

Der virtuelle Entwicklungscomputer ist nicht unbedingt erforderlich. Alle Entwicklungstools können auf einem lokalen Computer ausgeführt werden. Wir empfehlen Ihnen aber dringend die Nutzung des virtuellen Computers, damit bei Ihnen die gleichen Voraussetzungen bestehen.

Es dauert ungefähr 30 Minuten, um den virtuellen Computer zu erstellen und zu konfigurieren.

### <a name="get-the-script"></a>Beschaffen des Skripts

Klonen Sie das PowerShell-Skript über das Beispielrepository [Machine Learning and IoT Edge](https://github.com/Azure-Samples/IoTEdgeAndMlSample) (bzw. laden Sie es herunter).

### <a name="create-an-azure-virtual-machine"></a>Erstellen eines virtuellen Azure-Computers

Das Verzeichnis „DevVM“ enthält die Dateien, die zum Erstellen eines für dieses Tutorial geeigneten virtuellen Azure-Computers benötigt werden.

1. Öffnen Sie PowerShell als Administrator, und navigieren Sie zu dem Verzeichnis, in das Sie den Code heruntergeladen haben. Das Stammverzeichnis für Ihre Quelle hat die Bezeichnung `<srcdir>`.

    ```powershell
    cd <srcdir>\IoTEdgeAndMlSample\DevVM
    ```

2. Führen Sie den folgenden Befehl aus, um die Ausführung von Skripts zu ermöglichen. Wählen Sie **Ja, alle**, wenn die Aufforderung angezeigt wird.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

3. Führen Sie „Create-AzureDevVM.ps1“ aus diesem Verzeichnis aus.

    ```powershell
    .\Create-AzureDevVm.ps1
    ```

    * Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:
      * **Azure-Abonnement-ID**: Ihre Abonnement-ID (über das Azure-Portal ermittelbar).
      * **Ressourcengruppenname**: Der Name einer neuen oder vorhandenen Ressourcengruppe in Azure.
      * **Standort**: Wählen Sie einen Azure-Standort aus, an dem der virtuelle Computer erstellt wird. Beispiel: „westus2“ oder „northeurope“. Weitere Informationen finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).
      * **AdminUsername**: Geben Sie einen einprägsamen Namen für das Administratorkonto an, das Sie erstellen und auf dem virtuellen Computer verwenden möchten.
      * **AdminPassword**: Legen Sie ein Kennwort für das Administratorkonto auf dem virtuellen Computer fest.

    * Wenn Sie Azure PowerShell nicht installiert haben, wird mit dem Skript das [Azure PowerShell-Az-Modul](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0) installiert.

    * Sie werden aufgefordert, sich bei Azure anzumelden.

    * Das Skript bestätigt die Informationen für die Erstellung Ihres virtuellen Computers. Drücken Sie `y` oder `Enter`, um fortzufahren.

Die Ausführung des Skripts dauert mehrere Minuten, und es werden die folgenden Schritte ausgeführt:

* Erstellen der Ressourcengruppe, falls nicht vorhanden
* Bereitstellen des virtuellen Computers
* Aktivieren von Hyper-V auf der VM
* Installieren der erforderlichen Software für die Entwicklung und Klonen des Beispielrepositorys
* Neustarten der VM
* Erstellen einer RDP-Datei auf Ihrem Desktop für die Verbindungsherstellung mit der VM

### <a name="set-auto-shutdown-schedule"></a>Festlegen eines Zeitplans zum automatischen Herunterfahren

Als Beitrag zur Kostenreduzierung wurde die VM mit einem Zeitplan für das automatische Herunterfahren erstellt, der auf 19:00 Uhr PST festgelegt ist. Unter Umständen müssen Sie diese Zeitangabe je nach Standort und Zeitplan aktualisieren. Aktualisieren Sie den Zeitplan zum Herunterfahren wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem virtuellen Computer in der Ressourcengruppe, die Sie im vorherigen Abschnitt angegeben haben.

3. Wählen Sie im Seitennavigator die Option **Automatisch herunterfahren**.

4. Geben Sie einen neuen Zeitpunkt für das Herunterfahren in **Geplantes Herunterfahren**  ein, oder ändern Sie die **Zeitzone**, und klicken Sie anschließend auf **Speichern**.

### <a name="connect-and-configure-development-machine"></a>Verbinden und Konfigurieren des Entwicklungscomputers

Nachdem wir nun eine VM erstellt haben, müssen wir die Installation der Software abschließen, die für das Tutorial benötigt wird.

#### <a name="start-a-remote-desktop-session"></a>Starten einer Remotedesktopsitzung

1. Mit dem Skript für die Erstellung der VM wurde auf Ihrem Desktop eine RDP-Datei erstellt.

2. Doppelklicken Sie auf die Datei mit dem Namen **\<Name der Azure-VM\>.rdp**.

3. Es wird ein Dialogfeld mit dem Hinweis angezeigt, dass der Herausgeber der Remoteverbindung unbekannt ist. Klicken Sie in das Kontrollkästchen **Nicht erneut nach Verbindungen mit diesem Computer fragen**, und wählen Sie anschließend **Verbinden**.

4. Geben Sie bei entsprechender Aufforderung das AdminPassword an, das Sie beim Ausführen des Skripts zum Einrichten der VM verwendet haben, und klicken Sie auf **OK**.

5. Sie werden aufgefordert, das Zertifikat für den virtuellen Computer zu akzeptieren. Wählen Sie die Option **Nicht erneut nach Verbindungen mit diesem Computer fragen** und dann **Ja**.

#### <a name="install-visual-studio-code-extensions"></a>Installieren von Visual Studio Code-Erweiterungen

Nachdem Sie nun eine Verbindung mit dem Entwicklungscomputer hergestellt haben, sollten Sie Visual Studio Code einige nützliche Erweiterungen hinzufügen, um die Entwicklungsumgebung zu vereinfachen.

1. Navigieren Sie in einem PowerShell-Fenster zu **C:\\source\\IoTEdgeAndMlSample\\DevVM**.

2. Lassen Sie zu, dass auf dem virtuellen Computer Skripts ausgeführt werden, indem Sie Folgendes eingeben:

    ```powershell
    Set-ExecutionPolicy Bypass -Scope CurrentUser -Force
    ```

3. Führen Sie das Skript aus.

    ```powershell
    .\Enable-CodeExtensions.ps1
    ```

4. Die Ausführung des Skripts dauert einige Minuten, während die VS Code-Erweiterungen installiert werden:

    * Azure IoT-Tools
    * Python
    * C#
    * Docker
    * PowerShell

## <a name="set-up-iot-hub-and-storage"></a>Einrichten von IoT Hub und Storage

Diese Schritte werden normalerweise von einem Cloudentwickler ausgeführt.

Azure IoT Hub ist das Kernstück jeder IoT-Anwendung. Es ist für die sichere Kommunikation zwischen IoT-Geräten und der Cloud zuständig. Es ist das wichtigste Element für die Koordination des Betriebs einer IoT Edge Machine Learning-Lösung.

* Für IoT Hub werden Routen genutzt, um eingehende Daten von IoT-Geräten an andere nachgeschaltete Dienste zu leiten. Wir nutzen IoT Hub-Routen zum Senden von Gerätedaten an Azure Storage, wo sie von Azure Machine Learning eingesetzt werden können, um den Klassifizierer für die Restlebensdauer (RUL) zu trainieren.

* Später im Tutorial verwenden wir IoT Hub zum Konfigurieren und Verwalten unseres Azure IoT Edge-Geräts.

In diesem Abschnitt nutzen Sie ein Skript, um eine Azure IoT Hub-Instanz und ein Azure Storage-Konto zu erstellen. Anschließend konfigurieren Sie eine Route, über die vom Hub empfangene Daten mit dem Azure-Portal an einen Azure Storage Blob-Container weitergeleitet werden. Es dauert ungefähr zehn Minuten, bis diese Schritte abgeschlossen sind.

### <a name="create-cloud-resources"></a>Erstellen von Cloudressourcen

1. Öffnen Sie auf Ihrem Entwicklungscomputer ein PowerShell-Fenster.

1. Wechseln Sie zum Verzeichnis „IoTHub“.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\IoTHub
    ```

1. Führen Sie das Erstellungsskript aus. Verwenden Sie die gleichen Werte für Abonnement-ID, Standort und Ressourcengruppe wie beim Erstellen des virtuellen Entwicklungscomputers.

    ```powershell
    .\New-HubAndStorage.ps1 -SubscriptionId <subscription id> -Location
    <location> -ResourceGroupName <resource group>
    ```

    * Sie werden aufgefordert, sich bei Azure anzumelden.
    * Das Skript bestätigt die Informationen für die Erstellung Ihres Hubs und Speicherkontos. Drücken Sie `y` oder `Enter`, um fortzufahren.

Die Ausführung des Skripts dauert ungefähr zwei Minuten. Nach Abschluss des Vorgangs gibt das Skript den Namen des Hubs und des Speicherkontos aus.

### <a name="review-route-to-storage-in-iot-hub"></a>Überprüfen der Route zum Speicher in IoT Hub

Bei der Erstellung des IoT-Hubs wurden mit dem Skript, das wir im vorherigen Abschnitt ausgeführt haben, auch ein benutzerdefinierter Endpunkt und eine Route erstellt. IoT-Hub-Routen bestehen aus einem Abfrageausdruck und einem Endpunkt. Wenn eine Nachricht mit dem Ausdruck übereinstimmt, werden die Daten über die Route an den zugeordneten Endpunkt gesendet. Bei Endpunkten kann es sich um Event Hubs und Service Bus-Warteschlangen und -Themen handeln. In diesem Fall ist der Endpunkt ein Blobcontainer in einem Speicherkonto. Wir verwenden das Azure-Portal, um die vom Skript erstellte Route zu überprüfen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Wählen Sie im linken Navigator alle Dienste aus, geben Sie im Suchfeld „IoT“ ein, und wählen Sie **IoT Hub**.

1. Wählen Sie die IoT Hub-Instanz aus, die Sie im vorherigen Schritt erstellt haben.

1. Wählen Sie im IoT Hub-Seitennavigator die Option **Nachrichtenrouting**.

1. Die Seite für das Nachrichtenrouting enthält die beiden Registerkarten **Routen** und **Benutzerdefinierte Endpunkte**. Wählen Sie die Registerkarte **Benutzerdefinierte Endpunkte**.

1. Wählen Sie unter **Blobspeicher** die Option **turbofanDeviceStorage**.

1. Beachten Sie Folgendes: Dieser Endpunkt verweist auf einen Blobcontainer mit dem Namen **devicedata** in dem Speicherkonto, das Sie im letzten Schritt erstellt haben. Es hat den Namen **iotedgeandml\<eindeutiges Suffix\>** .

1. Außerdem weist das **Format für Blobdateinamen**  nicht mehr das Standardformat auf, sondern die Partition ist im Namen als letztes Element angegeben. Dieses Format ist besser für die Dateivorgänge geeignet, die wir später in diesem Tutorial mit Azure Notebooks durchführen.

1. Schließen Sie das Blatt mit den Endpunktdetails, um zur Seite **Nachrichtenrouting** zurückzukehren.

1. Wählen Sie die Registerkarte **Routen**.

1. Wählen Sie die Route mit dem Namen **turbofanDeviceDataToStorage**.

1. Beachten Sie, dass der Endpunkt der Route der benutzerdefinierte Endpunkt **turbofanDeviceStorage** ist.

1. Sehen Sie sich die Routingabfrage an, die auf **true** festgelegt ist. Dies bedeutet, dass alle Gerätetelemetrienachrichten mit dieser Route übereinstimmen, sodass alle Nachrichten an den Endpunkt **turbofanDeviceStorage** gesendet werden.

1. Schließen Sie die Routendetails.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir eine IoT Hub-Instanz erstellt und eine Route zu einem Azure Storage-Konto konfiguriert. Im nächsten Artikel senden wir Daten von einer Gruppe simulierter Geräte über die IoT Hub-Instanz an das Speicherkonto. Später in diesem Tutorial nach der Konfiguration unseres IoT Edge-Geräts und der Module gehen wir noch einmal auf Routen und die Routingabfrage ein.

Weitere Informationen zu den Schritten, die in diesem Teil des Tutorials zu Machine Learning in IoT Edge behandelt werden, finden Sie unter:

* [Azure IoT-Grundlagen](https://docs.microsoft.com/azure/iot-fundamentals/)
* [Konfigurieren der Nachrichtenweiterleitung mit IoT Hub](../iot-hub/tutorial-routing.md)
* [Erstellen eines IoT Hubs über das Portal](../iot-hub/iot-hub-create-through-portal.md)

Fahren Sie mit dem nächsten Artikel fort, um ein zu überwachendes simuliertes Gerät zu erstellen.

> [!div class="nextstepaction"]
> [Generieren von Gerätedaten](tutorial-machine-learning-edge-03-generate-data.md)
