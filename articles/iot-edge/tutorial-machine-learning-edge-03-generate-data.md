---
title: 'Tutorial: Generieren simulierter Gerätedaten: Machine Learning in Azure IoT Edge'
description: 'Tutorial: Erstellen Sie virtuelle Geräte, die simulierte Telemetriedaten generieren, mit denen dann ein Machine Learning-Modell trainiert werden kann.'
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 51d93e5b83d203f3fa99b69cc5f2877bbfdb6fb1
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112857"
---
# <a name="tutorial-generate-simulated-device-data"></a>Tutorial: Generieren simulierter Gerätedaten

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

In diesem Artikel verwenden wir Machine Learning-Trainingsdaten, um ein Gerät zu simulieren, das Telemetriedaten an IoT Hub sendet. Wie eingangs erwähnt, wird in diesem umfassenden Tutorial das [Turbofan Engine Degradation Simulation-Dataset](https://c3.nasa.gov/dashlink/resources/139/) genutzt, um Flugzeugtriebwerk-Daten für das Trainieren und Testen zu simulieren.

Die beigefügte Datei „readme.txt“ liefert uns die folgenden Informationen:

* Die Daten bestehen aus mehreren multivariaten Zeitreihen.
* Jedes Dataset ist in Bereiche für das Training und für Tests unterteilt.
* Jede Zeitreihe stammt von einem anderen Triebwerk.
* Bei jedem Triebwerk wird mit unterschiedlich großem Anfangsverschleiß und unterschiedlicher Fertigungsabweichung begonnen.

In diesem Tutorial verwenden wir nur die Trainingsdaten eines Datasets (FD003).

In einer realen Umgebung stellt jedes Triebwerk ein unabhängiges IoT-Gerät dar. Da wir davon ausgehen, dass Sie nicht über eine Sammlung mit Turbofan-Triebwerken mit Internetverbindung verfügen, erstellen wir einen Softwareersatz für diese Geräte.

Der Simulator ist ein C#-Programm, bei dem die IoT Hub-APIs zum programmgesteuerten Registrieren von virtuellen Geräten bei IoT Hub verwendet werden. Anschließend lesen wir die Daten für jedes Gerät aus der Datenteilmenge aus, die von der NASA bereitgestellt wurde, und senden sie über ein simuliertes IoT-Gerät an Ihren IoT-Hub. Den gesamten Code für diesen Teil des Tutorials finden Sie im Verzeichnis „DeviceHarness“ des Repositorys.

Das DeviceHarness-Projekt ist ein .NET Core-Projekt in C#, das aus vier Klassen besteht:

* **Program:** Der Einstiegspunkt für die Ausführung, der für die Verarbeitung der Benutzereingabe und die allgemeine Koordination zuständig ist.
* **TrainingFileManager:** Zuständig für das Lesen und Analysieren der ausgewählten Datendatei.
* **CycleData:** Steht für eine einzelne Zeile mit Daten in einer Datei, die in das Nachrichtenformat konvertiert wurde.
* **TurbofanDevice:** Zuständig für die Erstellung eines IoT-Geräts, das einem Gerät (Zeitreihe) in den Daten entspricht und die Daten über das IoT-Gerät an IoT Hub überträgt.

Die Abarbeitung der in diesem Artikel beschriebenen Aufgaben sollte ungefähr 20 Minuten dauern.

Die realen Abläufe dieses Schritts werden meist von Geräte- und Cloudentwicklern durchgeführt.

## <a name="configure-visual-studio-code-and-build-deviceharness-project"></a>Konfigurieren von Visual Studio Code und Erstellen des DeviceHarness-Projekts

1. Öffnen Sie, wie im vorherigen Artikel gezeigt, eine Remotedesktopsitzung mit Ihrem virtuellen Computer.

1. Öffnen Sie Visual Studio Code.

1. Wählen Sie in Visual Studio Code die Option **Datei** > **Ordner öffnen...** .

1. Geben Sie im Textfeld **Ordner** den Pfad `C:\source\IoTEdgeAndMlSample\DeviceHarness` ein, und klicken Sie auf die Schaltfläche **Ordner auswählen**.

   Falls im Ausgabefenster OmniSharp-Fehler angezeigt werden, müssen Sie die C#-Erweiterung deinstallieren, VS Code schließen und wieder öffnen, die C#-Erweiterung installieren und das Fenster dann neu laden.

1. Da Sie auf diesem Computer zum ersten Mal Erweiterungen nutzen, werden einige Erweiterungen aktualisiert und die zugehörigen Abhängigkeiten installiert. Unter Umständen werden Sie aufgefordert, die Erweiterung zu aktualisieren. Wählen Sie in diesem Fall die Option **Fenster erneut laden**.

1. Sie werden aufgefordert, die erforderliche Ressourcen für „DeviceHarness“ hinzuzufügen. Wählen Sie **Ja**, um sie hinzuzufügen.

   * Es kann einige Sekunden dauern, bis die Benachrichtigung angezeigt wird.
   * Falls Sie diese Benachrichtigung verpasst haben, können Sie über das Glockensymbol in der unteren rechten Ecke darauf zugreifen.

   ![VS Code-Popup für Erweiterung](media/tutorial-machine-learning-edge-03-generate-data/add-required-assets.png)

1. Wählen Sie **Wiederherstellen**, um die Paketabhängigkeiten wiederherzustellen.

   ![VS Code-Eingabeaufforderung für Wiederherstellung](media/tutorial-machine-learning-edge-03-generate-data/restore-package-dependencies.png)

1. Überprüfen Sie, ob Ihre Umgebung richtig eingerichtet ist, indem Sie einen Buildvorgang auslösen. Verwenden Sie hierfür entweder `Ctrl + Shift + B` oder **Terminal** > **Buildtask ausführen**.

1. Sie werden aufgefordert, die auszuführende Buildtask auszuwählen. Wählen Sie **Build** aus.

1. Der Buildvorgang wird durchgeführt, und es wird eine Erfolgsmeldung ausgegeben.

   ![Ausgabenachricht für erfolgreichen Buildvorgang](media/tutorial-machine-learning-edge-03-generate-data/build-success.png)

1. Sie können diesen Buildvorgang mit der Standard-Buildtask durchführen, indem Sie **Terminal** > **Configure Default Build Task...** (Standard-Buildtask konfigurieren...) und dann in der Eingabeaufforderung die Option **Build** wählen.

## <a name="connect-to-iot-hub-and-run-deviceharness"></a>Herstellen einer Verbindung mit dem IoT-Hub und Ausführen von „DeviceHarness“

Da das Projekt jetzt erstellt wird, können Sie eine Verbindung mit Ihrem IoT-Hub herstellen, um auf die Verbindungszeichenfolge zuzugreifen und den Fortschritt bei der Datengenerierung zu überwachen.

### <a name="sign-in-to-azure-in-visual-studio-code"></a>Anmelden bei Azure in Visual Studio Code

1. Melden Sie sich an Ihrem Azure-Abonnement in Visual Studio Code an, indem Sie die Befehlspalette öffnen. Verwenden Sie hierfür `Ctrl + Shift + P` oder **Ansicht** > **Befehlspalette...** .

1. Suchen Sie an der Eingabeaufforderung nach **Azure: Anmelden**, und wählen Sie diese Option aus.

1. Ein Browserfenster wird geöffnet, und Sie werden zum Eingeben Ihrer Anmeldeinformationen aufgefordert. Sie können den Browser schließen, nachdem Sie auf eine Seite mit einer Erfolgsmeldung geleitet wurden.

### <a name="connect-to-your-iot-hub-and-retrieve-hub-connection-string"></a>Herstellen einer Verbindung mit Ihrem IoT-Hub und Abrufen der Hub-Verbindungszeichenfolge

1. Wählen Sie im unteren Abschnitt des Visual Studio Code-Explorers den Bereich **Azure IoT Hub-Geräte** aus, um ihn zu erweitern.

1. Klicken Sie im erweiterten Bereich auf **IoT-Hub auswählen**.

1. Wählen Sie bei entsprechender Aufforderung Ihr Azure-Abonnement und dann Ihren IoT-Hub aus.

1. Klicken Sie in den Bereich **Azure IoT Hub-Geräte** und dann auf **...** , um weitere Aktionen anzuzeigen. Wählen Sie die Option **Copy IoT Hub connection string** (IoT Hub-Verbindungszeichenfolge kopieren).

   ![Kopieren der IoT Hub-Verbindungszeichenfolge](media/tutorial-machine-learning-edge-03-generate-data/copy-hub-connection-string.png)

### <a name="run-the-deviceharness-project"></a>Ausführen des Projekts „DeviceHarness“

1. Wählen Sie **Ansicht** > **Terminal**, um das Visual Studio Code-Terminal zu öffnen.

   Drücken Sie die EINGABETASTE, wenn keine Eingabeaufforderung angezeigt wird.

1. Geben Sie im Terminal `dotnet run` ein.

1. Fügen Sie die Verbindungszeichenfolge ein, die Sie im vorherigen Schritt kopiert haben, wenn Sie zum Eingeben der IoT Hub-Verbindungszeichenfolge aufgefordert werden.

1. Klicken Sie im Bereich **Azure IoT Hub-Geräte** auf die Schaltfläche „Aktualisieren“.

   ![Aktualisieren der IoT Hub-Geräteliste](media/tutorial-machine-learning-edge-03-generate-data/refresh-hub-device-list.png)

1. Beachten Sie, dass Geräte dem IoT-Hub hinzugefügt und die Geräte grün angezeigt werden, um anzugeben, dass über das jeweilige Gerät Daten gesendet werden.

1. Sie können die an den Hub gesendeten Nachrichten anzeigen, indem Sie mit der rechten Maustaste auf ein Gerät klicken und **Start Monitoring Built-in Event Endpoint** (Überwachung des integrierten Ereignisendpunkts starten) wählen. Die Nachrichten werden in Visual Studio Code im Ausgabebereich angezeigt.

1. Halten Sie die Überwachung an, indem Sie in den Ausgabebereich **Azure IoT Hub-Toolkit** klicken und die Option **Stop Monitoring Built-in Event Endpoint** (Überwachung des integrierten Ereignisendpunkts beenden) wählen.

1. Warten Sie, bis die Ausführung der Anwendung abgeschlossen ist. Dies dauert einige Minuten.

## <a name="check-iot-hub-for-activity"></a>Überprüfen des IoT-Hubs auf Aktivität

Die von „DeviceHarness“ gesendeten Daten wurden auf Ihren IoT-Hub übertragen. Sie können über das Azure-Portal auf einfache Weise sicherstellen, dass die Daten Ihren Hub erreicht haben.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu Ihrem IoT-Hub.

1. Auf der Übersichtsseite sollte angezeigt werden, dass die Daten an den Hub gesendet wurden:  

   ![Anzeigen von Gerät-zu-Cloud-Nachrichten im IoT-Hub](media/tutorial-machine-learning-edge-03-generate-data/iot-hub-usage.png)

## <a name="validate-data-in-azure-storage"></a>Überprüfen von Daten in Azure Storage

Die Daten, die wir gerade an Ihren IoT-Hub gesendet haben, wurden an den Speichercontainer geleitet, den wir im vorherigen Artikel erstellt haben. Wir sehen uns nun die Daten in unserem Speicherkonto an.

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto.

1. Wählen Sie im Navigator des Speicherkontos die Option **Storage-Explorer (Vorschau)** .

1. Wählen Sie im Storage-Explorer die Option **Blobcontainer** und dann **devicedata**.

1. Klicken Sie im Inhaltsbereich auf den Ordner für den Namen des IoT-Hubs und dann auf das Jahr, den Monat, den Tag und die Stunde. Es werden mehrere Ordner angezeigt. Diese stehen für die Minuten, in denen der Schreibvorgang für die Daten durchgeführt wurde.

   ![Anzeigen von Ordnern im Blobspeicher](media/tutorial-machine-learning-edge-03-generate-data/confirm-data-storage-results.png)

1. Klicken Sie in einen dieser Ordner, um nach Datendateien mit den Bezeichnungen **00** und **01** für die Partition zu suchen.

1. Die Dateien sind im [Avro](https://avro.apache.org/)-Format geschrieben, aber beim Doppelklicken auf eine dieser Dateien wird eine weitere Browserregisterkarte geöffnet, und die Daten werden teilweise gerendert. Wenn Sie stattdessen aufgefordert werden, die Datei in einem Programm zu öffnen, können Sie VS Code auswählen. Die Daten werden richtig gerendert.

1. Es ist nicht erforderlich, die Daten jetzt zu lesen oder zu interpretieren. Dies erledigen wir im nächsten Artikel.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir ein .NET Core-Projekt zum Erstellen einer Reihe von virtuellen Geräten und Senden von Daten mit diesen Geräten über unseren IoT-Hub und in einen Azure Storage-Container verwendet. Mit diesem Projekt wird ein reales Szenario simuliert, bei dem physische Geräte Daten, z. B. Sensormesswerte, Betriebseinstellungen, Fehlersignale und -modi usw., an einen IoT-Hub und weiter an einen zusammengestellten Speicher senden. Nachdem genügend Daten erfasst wurden, verwenden wir diese zum Trainieren von Modellen, mit denen die Restlebensdauer (RUL) des Geräts vorhergesagt wird. Dies wird im nächsten Artikel veranschaulicht.

Fahren Sie mit dem nächsten Artikel fort, um ein Machine Learning-Modell mit den Daten zu trainieren.

> [!div class="nextstepaction"]
> [Trainieren und Bereitstellen eines Azure Machine Learning-Modells](tutorial-machine-learning-edge-04-train-model.md)
