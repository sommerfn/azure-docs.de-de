---
title: Verbinden eines IoT Plug & Play-Geräts mit IoT Central | Microsoft-Dokumentation
description: Für Geräteentwickler wird beschrieben, wie Visual Studio Code zum Erstellen und Testen eines IoT Plug & Play-Geräts verwendet wird, das eine Verbindung mit IoT Central herstellt.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 630f92bc8cf5fb89626bfe4de4e4a575045c961a
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879354"
---
# <a name="use-visual-studio-code-to-create-an-iot-plug-and-play-device-that-connects-to-iot-central"></a>Verwenden von Visual Studio Code zum Erstellen eines IoT Plug & Play-Geräts, das eine Verbindung mit IoT Central herstellt

In diesem Leitfaden wird Folgendes veranschaulicht:

* Als Bediener erfahren Sie, wie Sie Ihrer Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren.

* Als Geräteentwickler erfahren Sie, wie Sie Visual Studio Code zum Erstellen eines [IoT Plug & Play](https://aka.ms/iot-pnp-docs)-Geräts verwenden, das eine Verbindung mit Ihrer IoT Central-Anwendung herstellt.

Sie verwenden ein [Gerätefunktionsmodell](./concepts-architecture-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), um das Gerät zu definieren, das eine Verbindung mit IoT Central herstellt. In diesem Leitfaden verwenden Sie ein Modell, das ein Umgebungssensorgerät definiert.

Der Geräteentwickler verwendet das Modell, um den Clientcode des Geräts zu generieren, und der Ersteller verwendet das Modell, um eine [Gerätevorlage](./howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) in IoT Central zu erstellen. Das Modell fungiert als Vertrag zwischen Ihrem Gerät und Ihrer IoT Central-Anwendung.

Im Abschnitt dieses Leitfadens, der das Erstellen des generierten Codes beschreibt, wird vorausgesetzt, dass Sie Windows verwenden.

In diesem Artikel lernen Sie Folgendes:

* Importieren eines Gerätefunktionsmodells in eine Gerätevorlage in IoT Central
* Hinzufügen eines Dashboards zu der Vorlage, mit der Gerätetelemetrie angezeigt wird
* Hinzufügen eines echten Geräts aus der Vorlage
* Importieren eines Gerätefunktionsmodells in Visual Studio Code
* Generieren einer C-Geräteclientanwendung aus dem Modell
* Erstellen der C-Geräteclientanwendung und Verbinden mit IoT Central

## <a name="prerequisites"></a>Voraussetzungen

Zum Testen des Gerätecodes in diesem Leitfaden benötigen Sie eine IoT Central-Anwendung, die aus der Anwendungsvorlage **Preview** erstellt wurde. Falls Sie noch nicht über eine IoT Central-Anwendung verfügen, arbeiten Sie die Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) durch.

Die folgenden Voraussetzungen müssen erfüllt sein, um mit dem Gerätefunktionsmodell in diesem Leitfaden arbeiten zu können:

* [Visual Studio Code](https://code.visualstudio.com/download): Visual Studio Code ist für mehrere Plattformen verfügbar.
* Azure IoT Device Workbench-Erweiterung für Visual Studio Code. Führen Sie die folgenden Schritte aus, um die Azure IoT Device Workbench-Erweiterung in VS Code zu installieren:

    1. Wählen Sie in VS Code die Registerkarte **Erweiterungen**.
    1. Suchen Sie nach **Azure IoT Device Workbench**.
    1. Wählen Sie **Installieren** aus.

    > [!NOTE]
    > Die aktuelle Version des Codegenerators in der Erweiterung unterstützt nicht die Schematypen **Geopunkt** und **Vektor** oder die semantischen Typen **Beschleunigung**, **Geschwindigkeit** und **Standort**. Diese Schema- und semantischen Typen werden von IoT Central unterstützt.

    > [!NOTE]
    > Für die Zusammenarbeit mit IoT Central muss das Gerätefunktionsmodell über alle definierten Schnittstellen inline in derselben Datei verfügen.

Zum Erstellen des generierten C-Codes unter Windows in diesem Leitfaden benötigen Sie Folgendes:

* [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/): Stellen Sie sicher, dass Sie die Komponente **NuGet-Paket-Manager** und die Workload **Desktopentwicklung mit C++** aktivieren, wenn Sie Visual Studio installieren.
* [Git-Client](https://git-scm.com/download)
* [CMake](https://cmake.org/download/): Wählen Sie beim Installieren von **CMake** die Option **Add CMake to the system PATH** (CMake dem Systempfad hinzufügen).
* Eine lokale Kopie des Azure IoT C SDK:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

Damit Sie die Anweisungen für die Gerätepriorisierung am Ende dieses Leitfadens befolgen können, müssen Sie außerdem Folgendes installieren:

* [Node.js](https://nodejs.org)
* Das Tool [dps-keygen](https://www.npmjs.com/package/dps-keygen):

    ```cmd/sh
    npm i -g dps-keygen
    ```

## <a name="create-device-template"></a>Erstellen einer Gerätevorlage

Erstellen Sie einen Ordner mit dem Namen **pnp_app** in dem Ordner **azure-iot-sdk-c**, der das von Ihnen geklonte Azure IoT C SDK enthält. Laden Sie das Gerätefunktionsmodell [EnvironmentalSensor.capabilitymodel.json](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) aus GitHub herunter, und speichern Sie die Datei im Ordner **pnp_app**. Ersetzen Sie die beiden Vorkommen von `<YOUR_COMPANY_NAME_HERE>` durch Ihren Namen. Verwenden Sie nur die Zeichen a - z, A - Z, 0 - 9 und Unterstrich. Notieren Sie sich den vollständigen Wert des Felds `"@id"`, das Ihr Gerätefunktionsmodell eindeutig identifiziert. Sie benötigen diesen Wert später. Dieses Modell enthält zwei Schnittstellen:

* Die gemeinsame Schnittstelle **DeviceManagement**.
* Die benutzerdefinierte Schnittstelle **EnvironmentalSensor**.

Zum Erstellen einer Gerätevorlage für das Umgebungssensorgerät in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie zur Seite **Gerätevorlagen**, und wählen Sie **+ Neu** aus. Wählen Sie dann **Benutzerdefiniert** aus.

1. Geben Sie **Umgebungssensor** als Namen für Ihre Gerätevorlage ein.

1. Wählen Sie **Funktionsmodell importieren** aus. Suchen Sie dann nach der von Ihnen erstellten Datei **EnvironmentalSensor.capabilitymodel.json**, und wählen Sie **Öffnen** aus. Die beiden Schnittstellen **Geräteinformationen** und **Umgebungssensor** werden im **Umgebungssensor-Funktionsmodell** angezeigt.

1. Wählen Sie **Ansichten** und dann **Gerät visualisieren** aus.

1. Wählen Sie in der Liste der Felder, die Sie dem Dashboard hinzufügen können, die beiden Werte für **Telemetrie**, **Luftfeuchtigkeit** und **Temperatur**, aus, und klicken Sie auf **Kombinieren**. Klicken Sie auf **Speichern**.

Sie verfügen nun über eine Gerätevorlage, die das Modell **Umgebungssensor** verwendet und ein einfaches Dashboard zum Anzeigen der Telemetriedaten von einem Gerät aufweist.

## <a name="publish-the-template-and-add-a-real-device"></a>Veröffentlichen der Vorlage und Hinzufügen eines echten Geräts

Zum Veröffentlichen der Vorlage und Hinzufügen eines echten Geräts wechseln Sie zur Seite **Gerätevorlagen**, und wählen Sie die Gerätevorlage **Umgebungssensor** aus. Wählen Sie **Veröffentlichen**, überprüfen Sie die Informationen,und klicken Sie auf **Veröffentlichen**.

Bevor Sie eine Clientanwendung verbinden, müssen Sie auf der Seite **Geräte** ein Gerät hinzufügen und die zugehörigen Verbindungsinformationen abrufen:

1. Navigieren Sie zur Seite **Geräte**, und wählen Sie **Umgebungssensor** aus. Auf der Seite **Geräte** können Sie die Geräte verwalten, die eine Verbindung mit Ihrer Anwendung herstellen können.

1. Zum Hinzufügen eines echten Geräts wählen Sie **+ Neu** aus, ändern die Geräte-ID in **sensor01** und klicken auf **Erstellen**. Stellen Sie sicher, dass **Simuliert** auf **Aus** festgelegt ist.

1. Wählen Sie in der Liste der Geräte Ihren **Umgebungssensor** aus. Wählen Sie dann **Verbinden** aus.

1. Notieren Sie sich die **Bereichs-ID**, die **Geräte-ID** und den **Primärschlüssel**. Klicken Sie dann auf **Schließen**.

## <a name="generate-a-device-client-application"></a>Generieren einer Geräteclientanwendung

Mit Visual Studio Code können Sie ein Gerüst einer Geräteclientanwendung aus dem Gerätefunktionsmodell generieren:

1. Starten Sie Visual Studio Code, und öffnen Sie den Ordner **pnp_app**.

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann **Generate Device Code Stub** (Gerätecode-Stub generieren) aus.

1. Wählen Sie die Datei für das Gerätefunktionsmodell **EnvironmentalSensor.capabilitymodel.json** aus.

1. Geben Sie **sensor_app** als Projektnamen ein.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie **CMake-Projekt** als Ziel aus.

1. Wählen Sie als Verbindungsmethode die Option **Via DPS (Device Provisioning Service) symmetric key** (Mit symmetrischem Schlüssel per Device Provisioning-Dienst).

Visual Studio Code öffnet ein neues Fenster mit dem generierten C-Code im Ordner **sensor_app**.

## <a name="add-connection-details-to-the-device-client"></a>Hinzufügen von Verbindungsdetails zum Geräteclient

Um Ihrem Geräteclient die Verbindungsinformationen hinzuzufügen, öffnen Sie **main.c** in dem Ordner, der den generierten Code enthält:

1. Ersetzen Sie `[DPS Id Scope]` durch den Wert für **Bereichs-ID**, den Sie sich zuvor notiert hatten.

1. Ersetzen Sie `[DPS symmetric key]` durch den Wert für **Primärschlüssel**, den Sie sich zuvor notiert hatten.

1. Ersetzen Sie `[device registration Id]` durch den Wert für **Geräte-ID**, den Sie sich zuvor notiert hatten.

1. Speichern Sie die Änderungen.

## <a name="build-and-run-the-client"></a>Erstellen und Ausführen des Clients

Zum Erstellen und Ausführen des Clients müssen Sie den Build für das Azure IoT C SDK anpassen:

1. Öffnen Sie die Datei **CMakeLists.txt** im Stammverzeichnis des Ordners **azure-iot-sdk-c**.

1. Fügen Sie am Ende dieser Datei die folgende Zeile hinzu, um die neue Clientanwendung in den Build aufzunehmen:

    ```txt
    add_subdirectory(pnp_app/sensor_app)
    ```

1. Speichern Sie die Änderungen.

1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner **azure-iot-sdk-c**.

1. Führen Sie zum Erstellen der Anwendung die folgenden Befehle aus:

    ```cmd
    mkdir cmake
    cd cmake
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -Dskip_samples:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

1. Zum Ausführen der Anwendungen führen Sie an derselben Eingabeaufforderung den folgenden Befehl aus:

    ```cmd
    pnp_app\sensor_app\Release\sensor_app.exe
    ```

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Nach einigen Minuten können Sie die Telemetriedaten vom Gerät auf dem Gerätedashboard in Ihrer IoT Central-Anwendung sehen.

## <a name="connect-device-first"></a>Verbinden mit Gerätepriorisierung

Sie können ein IoT Plug & Play-Gerät über eine gerätepriorisierende Verbindung anschließen, wie es unter [Konnektivität](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) beschrieben ist. Der Ermittlungsprozess erfolgt in dieser Reihenfolge:

1. Wird der Gerätevorlage zugeordnet, wenn sie bereits in der IoT Central-Anwendung veröffentlicht wurde.

1. Ruft das Funktionsmodell aus dem [öffentlichen Repository](https://aka.ms/ACFI) veröffentlichter und zertifizierter Funktionsmodelle ab.

Mithilfe von Visual Studio Code und dem oben erwähnten Befehl **Generate Device Code Stub** (Gerätecode-Stub generieren) können Sie die folgenden Schritte ausführen, um eine gerätepriorisierende Verbindung herzustellen und das veröffentlichte Gerätefunktionsmodell automatisch aus dem öffentlichen Repository in IoT Central einzubinden:

1. Verwenden Sie ein vorhandenes Gerätefunktionsmodell, das im öffentlichen Repository veröffentlicht wurde. Sie benötigen das vollständige Gerätefunktionsmodell und müssen sich den URN dieses Modells notieren.

1. Führen Sie die oben beschriebenen Schritte zum [Generieren einer Geräteclientanwendung](#generate-a-device-client-application) aus, um Visual Studio Code zu verwenden und den Gerätecode zu generieren.

1. Wechseln Sie in Ihrer Azure IoT Central-Anwendung zur Registerkarte **Verwaltung**, und wählen Sie den Abschnitt **Geräteverbindung** aus. Notieren Sie sich die Werte für **Bereichs-ID** und **Primärschlüssel** für Ihre Anwendung.

    > [!NOTE]
    > Der auf dieser Seite gezeigte **Primärschlüssel** ist eine Shared Access Signature (SAS) für Gruppen, mit der Sie mehrere Geräteschlüssel für Ihre Anwendung generieren können.

1. Verwenden Sie das Tool [dps-keygen](https://www.npmjs.com/package/dps-keygen), um einen Geräteschlüssel aus dem Primärschlüssel zu generieren, den Sie sich zuvor notiert hatten. Zum Generieren des Geräteschlüssels führen Sie den folgenden Befehl aus:

    ```cmd/sh
    dps-keygen -mk:<Primary_Key> -di:<device_id>
    ```

1. Führen Sie die Schritte im vorherigen Abschnitt [Hinzufügen von Verbindungsdetails zum Geräteclient](#add-connection-details-to-the-device-client) aus, um die **Bereichs-ID**, den **Primärschlüssel** und die **Geräte-ID** hinzuzufügen.

1. Führen Sie die Schritte im vorherigen Abschnitt zum [Erstellen und Ausführen des Clients](#build-and-run-the-client) aus.

1. Sie sehen nun, dass das Gerät eine Verbindung mit Ihrer IoT Central-Anwendung herstellt und automatisch das Gerätefunktionsmodell aus dem öffentlichen Repository als Gerätevorlage einbindet.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie ein echtes Gerät mit IoT Central verbinden, wird als nächster Schritt empfohlen, mehr über Gerätevorlagen unter [Einrichten einer Gerätevorlage](howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) zu erfahren.
