---
title: Erstellen von IoT Plug & Play-Geräten (Vorschauversion) unter Verwendung von Visual Studio und Visual Studio Code | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Visual Studio und Visual Studio Code verwenden, um die Erstellung von IoT Plug & Play-Gerätemodellen und die Implementierung des Gerätecodes zu beschleunigen.
author: liydu
ms.author: liydu
ms.date: 10/29/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 6ce5a93cdd44af7f199d59d459daa46b4adb0719
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748034"
---
# <a name="use-visual-studio-and-visual-studio-code-to-build-iot-plug-and-play-devices"></a>Erstellen von IoT Plug & Play-Geräten unter Verwendung von Visual Studio und Visual Studio Code

Azure IoT Tools für Visual Studio Code stellt eine integrierte Umgebung bereit, in der Sie Gerätefunktionsmodelle (Device Capability Models, DCMs) und Schnittstellen erstellen, in Modellrepositorys veröffentlichen und ein C-Codegerüst generieren können, um die Geräteanwendung zu implementieren.

In diesem Artikel erfahren Sie Folgendes:

- Sie generieren den Gerätecode und das Anwendungsprojekt.
- Sie verwenden den generierten Code in Ihrem Geräteprojekt.
- Sie durchlaufen den Code durch erneutes Generieren des Skeleton.

Weitere Informationen zur Entwicklung von IoT-Geräten unter Verwendung von VS Code finden Sie hier unter [https://github.com/microsoft/vscode-iot-workbench](https://github.com/microsoft/vscode-iot-workbench).

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie [Visual Studio Code](https://code.visualstudio.com/).

Führen Sie die folgenden Schritte aus, um das Erweiterungspaket in VS Code zu installieren:

1. Wählen Sie in VS Code die Registerkarte **Erweiterungen** aus.
1. Suchen Sie im Marketplace nach **Azure IoT Tools**, und installieren Sie diese Erweiterung.

## <a name="generate-device-code-and-project"></a>Generieren von Gerätecode und Projekt

Öffnen Sie die Befehlspalette in VS Code mit **STRG+UMSCHALT+P**. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Gerätecodestub generieren** aus, um den Skeletoncode und Projekttyp zu konfigurieren. Die folgende Liste beschreibt jeden Schritt im Detail:

- **DCM-Datei, die zum Generieren des Codes verwendet werden soll**. Um den Skeletoncode für ein Gerät zu generieren, öffnen Sie den Ordner mit den DCM- und Schnittstellendateien, die dieser Code implementiert. Wenn der Code-Generator keine Schnittstelle findet, die ein DCM erfordert, lädt er diese bei Bedarf aus dem Modellrepository herunter.

- **Sprache des Gerätecodes**. Derzeit unterstützt der Code-Generator nur ANSI-C.

- **Projektname**. Der Projektname wird als Ordnername für den generierten Code und andere Projektdateien verwendet. Der Ordner wird standardmäßig neben der DCM-Datei gespeichert. Um zu vermeiden, dass Sie den Ordner für den generierten Code jedes Mal manuell kopieren müssen, wenn Sie Ihr DCM aktualisieren und den Gerätecode erneut generieren, speichern Sie die DCM-Datei im gleichen Ordner wie den Projektordner.

- **Methode zum Herstellen einer Verbindung mit Azure IoT**. Die generierten Dateien enthalten auch Code, mit dem das Gerät zum Herstellen einer Verbindung mit Azure IoT Hub konfiguriert wird. Sie können eine direkte Verbindung mit [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) herstellen oder den [Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps) verwenden.

    - **Über die Verbindungszeichenfolge für ein IoT Hub-Gerät**: Geben Sie die Verbindungszeichenfolge für das Gerät an, damit die Geräteanwendung direkt eine Verbindung mit IoT Hub herstellen kann.
    - **Über einen symmetrischen DPS-Schlüssel**: Geben Sie die Werte für **Bereichs-ID**, **Symmetrischer Schlüssel** und **Geräte-ID** für die Geräteanwendung an, die erforderlich sind, um eine Verbindung mit IoT Hub oder IoT Central über DPS herzustellen.

- **Projekttyp**. Der Code-Generator generiert auch ein CMake-oder Arduino-Projekt. Derzeit werden folgende Projekttypen unterstützt:

    - **CMake-Projekt unter Windows**: für ein Geräteprojekt, das [CMake](https://cmake.org/) als Buildsystem unter Windows verwendet. Mit dieser Option wird `CMakeLists.txt` mit Geräte-SDK-Konfigurationen in demselben Ordner wie der C-Code generiert.
    - **CMake-Projekt unter Linux**: für ein Geräteprojekt, das [CMake](https://cmake.org/) als Buildsystem unter Linux verwendet. Mit dieser Option wird `CMakeLists.txt` mit Geräte-SDK-Konfigurationen in demselben Ordner wie der C-Code generiert.
    - **MXChip IoT DevKit-Projekt**: Für ein Geräteprojekt, das auf einem [MXChip IoT DevKit](https://aka.ms/iot-devkit)-Gerät ausgeführt wird. Diese Option generiert ein Arduino-Projekt, das Sie [in VS Code](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) oder in der Arduino-IDE verwenden können, um Code auf einem IoT DevKit-Gerät zu kompilieren und auszuführen.

- **Geräte-SDK-Typ**. Wenn Sie „CMake“ als Projekttyp auswählen, konfigurieren Sie mit diesem Schritt, wie generierter Code das Azure IoT C-Geräte-SDK in `CMakeLists.txt` integrieren soll:

    - **Über Quellcode**: Der generierte Code basiert auf dem [Geräte-SDK-Quellcode](https://github.com/Azure/azure-iot-sdk-c), der integriert und zum Kompilieren verwendet werden soll. Dies wird empfohlen, wenn Sie den Geräte-SDK-Quellcode angepasst haben.
    - **Über Vcpkg**: Der generierte Code basiert auf dem [Geräte-SDK-Vcpkg](https://github.com/microsoft/vcpkg/tree/master/ports/azure-iot-sdk-c), das integriert und zum Kompilieren verwendet werden soll. Dies ist die empfohlene Vorgehensweise für Windows-, Linux- oder macOS-Geräte.

    > [!NOTE]
    > Die macOS-Unterstützung für das Azure IoT C-Geräte-SDK-Vcpkg wird gerade ausgeführt.

Der Code-Generator versucht, die DCM- und Schnittstellendateien im lokalen Ordner zu verwenden. Wenn sich die Schnittstellendateien nicht im lokalen Ordner befinden, sucht der Code-Generator im öffentlichen Modellrepository oder im Unternehmensmodellrepository danach. [Allgemeine Schnittstellendateien](./concepts-common-interfaces.md) sind im öffentlichen Modellrepository gespeichert.

Wenn die Codegenerierung abgeschlossen ist, öffnet die Erweiterung ein neues VS Code-Fenster mit dem Code. Wenn Sie eine generierte Datei wie **main.c** öffnen, meldet IntelliSense möglicherweise, dass die C SDK-Quelldateien nicht geöffnet werden können. Um die korrekten IntelliSense-Einstellungen und eine ordnungsgemäße Codenavigation zu ermöglichen, beziehen Sie die C SDK-Quelle mithilfe der folgenden Schritte ein:

1. Drücken Sie in VS Code die Tastenkombination **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen. Wählen Sie **C/C++: Konfigurationen bearbeiten (JSON)** aus, um die Datei **c_cpp_properties.json** zu öffnen.

1. Fügen Sie den Pfad des Geräte-SDKs im Abschnitt `includePath` ein:

    ```json
    "includePath": [
        "${workspaceFolder}/**",
        "{path_of_device_c_sdk}/**"
    ]
    ```

1. Speichern Sie die Datei .

## <a name="use-the-generated-code"></a>Verwenden des generierten Codes

Die folgenden Anweisungen beschreiben, wie Sie den generierten Code in Ihrem eigenen Geräteprojekt auf verschiedenen Computerplattformen für die Entwicklung verwenden. Hierbei wird davon ausgegangen, dass Sie eine IoT Hub-Geräteverbindungszeichenfolge mit dem generierten Code verwenden:

### <a name="linux"></a>Linux

Wenn Sie den Gerätecode zusammen mit dem Geräte-C SDK-Vcpkg kompilieren möchten, verwenden Sie CMake in einer Linux-Umgebung wie Ubuntu oder Debian:

1. Öffnen Sie eine Terminalanwendung.

1. Installieren Sie **GCC**, **Git**, `cmake` und alle Abhängigkeiten mithilfe des Befehls `apt-get`:

    ```bash
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    Überprüfen Sie, ob die Version von `cmake` höher als **2.8.12** und die Version von **GCC** höher als **4.4.7** ist.

    ```bash
    cmake --version
    gcc --version
    ```

1. Installieren Sie Vcpkg:

    ```bash
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    ./bootstrap-vcpkg.sh
    ```

    Führen Sie dann Folgendes aus, um die benutzerweite [Integration](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md) zu verbinden:

    ```bash
    ./vcpkg integrate install
    ```

1. Installieren Sie das Azure IoT C-Geräte-SDK-Vcpkg:

    ```bash
    ./vcpkg install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

1. Erstellen Sie in dem Ordner, der den generierten Codestub enthält, ein `cmake`-Unterverzeichnis, und navigieren Sie zu diesem Ordner:

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Codestub mithilfe von CMake zu kompilieren:

    ```bash
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}/scripts/buildsystems/vcpkg.cmake"

    cmake --build .
    ```

1. Führen Sie die Anwendung nach erfolgreicher Kompilierung aus, und geben Sie dabei die IoT Hub-Geräteverbindungszeichenfolge als Parameter an.

    ```bash
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

### <a name="windows"></a>Windows

Informationen dazu, wie Sie CMake und die Visual Studio C/C++-Compiler an der Befehlszeile verwenden, um den Gerätecode zusammen mit dem C-SDK für das Gerät unter Windows zu kompilieren, finden Sie hier: [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts](./quickstart-create-pnp-device.md). Die folgenden Schritte zeigen, wie Sie den Gerätecode zusammen mit dem Geräte-C SDK-Vcpkg als CMake-Projekt in Visual Studio kompilieren.

1. Führen Sie die Schritte im [Schnellstart](https://docs.microsoft.com/azure/iot-pnp/quickstart-create-pnp-device#prepare-the-development-environment) aus, um das Azure IoT-Geräte-SDK für C über Vcpkg zu installieren.

1. Installieren Sie [Visual Studio 2019 (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/): Stellen Sie sicher, dass Sie die Komponente **NuGet-Paket-Manager** und die Workload **Desktopentwicklung mit C++** einschließen.

1. Öffnen Sie Visual Studio, und wählen Sie **Datei > Öffnen > CMake...** aus, um `CMakeLists.txt` in dem Ordner zu öffnen, der generierten Code enthält.

1. Suchen Sie in der Symbolleiste **Allgemein** das Dropdownmenü **Konfiguration**. Wählen Sie **Konfiguration verwalten** aus, um die CMake-Einstellung für Ihr Projekt hinzuzufügen.

    ![Konfiguration verwalten](media/howto-develop-with-vs-vscode/vs-manage-config.png)

1. Fügen Sie in den **CMake-Einstellungen** eine neue Konfiguration hinzu, und wählen Sie **x86-Debug** als Ziel aus.

1. Fügen Sie in den **CMake-Befehlsargumenten** folgende Zeile hinzu:

    ```txt
    -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    ```

1. Speichern Sie die Datei .

1. Wechseln Sie im Dropdown **Konfigurationen** zu **x86-Debug**. Es dauert ein paar Sekunden, bis CMake den Cache dafür generiert hat. Zeigen Sie das Ausgabefenster an, um den Fortschritt sehen zu können.

    ![CMake-Ausgabe](media/howto-develop-with-vs-vscode/vs-cmake-output.png)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf `CMakeLists.txt` im Stammordner, und wählen Sie im Kontextmenü die Option **Kompilieren** aus, um den generierten Codestub mit dem Geräte-SDK zu kompilieren.

1. Führen Sie die Anwendung nach erfolgreicher Kompilierung an der Eingabeaufforderung aus, und eben Sie dabei die IoT Hub-Geräteverbindungszeichenfolge als Parameter an.

    ```cmd
    .\out\build\x86-Debug\{generated_code_project_name}.exe "[IoT Hub device connection string]"
    ```

> [!TIP]
> Weitere Informationen zur Verwendung von CMake in Visual Studio finden Sie unter [Erstellen von CMake-Projekten](https://docs.microsoft.com/cpp/build/cmake-projects-in-visual-studio?view=vs-2019#building-cmake-projects).

### <a name="macos"></a>macOS

Die folgenden Schritte zeigen, wie Sie den Gerätecode zusammen mit dem Geräte-C SDK-Quellcode mithilfe von CMake unter macOS kompilieren:

1. Öffnen Sie eine Terminalanwendung.

1. Verwenden Sie [Homebrew](https://homebrew.sh), um alle Abhängigkeiten zu installieren:

    ```bash
    brew update
    brew install git cmake pkgconfig openssl ossp-uuid
    ```

1. Überprüfen Sie, ob [CMake](https://cmake.org/) mindestens in Version **2.8.12** vorliegt:

    ```bash
    cmake --version
    ```

1. [Patchen Sie die CURL](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#upgrade-curl-on-mac-os) auf die neueste verfügbare Version.

1. Klonen Sie in dem Ordner, der den generierten Code enthält, das Repository [Azure IoT C-SDK](https://github.com/Azure/azure-iot-sdk-c):

    ```bash
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

1. Erstellen Sie unter dem Ordner, der den generierten Code enthält, einen Ordner namens `cmake`, und navigieren Sie zu diesem Ordner.

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Codestub mithilfe von CMake zu kompilieren:

    ```bash
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DOPENSSL_ROOT_DIR:PATH=/usr/local/opt/openssl ..
    cmake --build .
    ```

1. Führen Sie die Anwendung nach erfolgreicher Kompilierung aus, und geben Sie dabei die IoT Hub-Geräteverbindungszeichenfolge als Parameter an.

    ```bash
    cd {generated_code_folder_name}/cmake/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

## <a name="iterate-by-regenerating-the-skeleton-code"></a>Durchlaufen des Codes durch erneutes Generieren des Skeleton

Der Code-Generator kann den Code erneut generieren, wenn Sie Ihre DCM- oder Schnittstellendateien aktualisieren. Wenn Sie Ihren Gerätecode bereits aus einer DCM-Datei generiert haben, gehen Sie folgendermaßen vor, um den Code erneut zu generieren:

1. Drücken Sie bei geöffnetem Ordner mit den DCM-Dateien die Tastenkombination **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Gerätecodestub generieren** aus.

1. Wählen Sie die DCM-Datei aus, die Sie aktualisiert haben.

1. Klicken Sie auf **Code für „{Projektname}“ neu generieren**.

1. Der Code-Generator verwendet die vorherige von Ihnen konfigurierte Einstellung und generiert den Code neu. Dateien, die Benutzercode enthalten können, wie z. B. `main.c` und `{project_name}_impl.c`, werden jedoch nicht überschrieben.

> [!NOTE]
> Wenn Sie die URN-ID in Ihrer Schnittstellendatei aktualisieren, wird diese als neue Schnittstelle behandelt. Wenn Sie den Code neu generieren, generiert der Code-Generator den Code für die Schnittstelle, überschreibt aber nicht den ursprünglichen Code in der Datei `{project_name}_impl.c`.

## <a name="problems-and-feedback"></a>Probleme und Feedback

Bei Azure IoT Tools handelt es sich um ein Open-Source-Projekt auf GitHub. Wenn Probleme auftreten oder Sie ein Feature anfordern möchten, können Sie [ein Issue auf GitHub erstellen](https://github.com/microsoft/vscode-azure-iot-tools/issues/new).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie mit Visual Studio und Visual Studio Code ein C-Codegerüst generieren, um die Geräteanwendung zu implementieren. Als Nächstes empfehlen wir, sich darüber zu informieren, wie Sie das Tool [Azure-IoT-Explorer installieren und verwenden](./howto-install-iot-explorer.md).
