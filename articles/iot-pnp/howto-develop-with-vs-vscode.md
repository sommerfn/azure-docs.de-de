---
title: Erstellen von IoT Plug & Play-Geräten (Vorschauversion) unter Verwendung von Visual Studio und Visual Studio Code | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Visual Studio und Visual Studio Code verwenden, um die Erstellung von IoT Plug & Play-Gerätemodellen und die Implementierung des Gerätecodes zu beschleunigen.
author: liydu
ms.author: liydu
ms.date: 09/10/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d68a3f99096ca64b357239f61cf7ff17d6fc3f83
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935527"
---
# <a name="use-azure-iot-tools-for-visual-studio-code"></a>Verwenden von Azure IoT Tools für Visual Studio Code

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

- **Projekttyp**. Der Code-Generator generiert auch eine Projektdatei, sodass Sie den Code in Ihr eigenes Projekt oder in das Projekt für das Geräte-SDK integrieren können. Derzeit werden folgende Projekttypen unterstützt:

    - **CMake-Projekt**: Für ein Geräteprojekt, das [CMake](https://cmake.org/) als Buildsystem verwendet. Diese Option generiert eine `CMakeLists.txt`-Datei im gleichen Ordner wie der C-Code.
    - **MXChip IoT DevKit-Projekt**: Für ein Geräteprojekt, das auf einem [MXChip IoT DevKit](https://aka.ms/iot-devkit)-Gerät ausgeführt wird. Diese Option generiert ein Arduino-Projekt, das Sie [in VS Code](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) oder in der Arduino-IDE verwenden können, um Code auf einem IoT DevKit-Gerät zu kompilieren und auszuführen.

- **Methode zum Herstellen einer Verbindung mit Azure IoT**. Die generierten Dateien enthalten auch Code, mit dem das Gerät zum Herstellen einer Verbindung mit Azure IoT Hub konfiguriert wird. Sie können eine direkte Verbindung mit [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) herstellen oder den [Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps) verwenden.

    - **Über die Verbindungszeichenfolge für ein IoT Hub-Gerät**: Geben Sie die Verbindungszeichenfolge für das Gerät an, damit die Geräteanwendung direkt eine Verbindung mit IoT Hub herstellen kann.
    - **Über einen symmetrischen DPS-Schlüssel**: Geben Sie die Werte für **Bereichs-ID**, **Registrierungs-ID** und **SaS-Schlüssel** für die Geräteanwendung an, die erforderlich, um eine Verbindung mit IoT Hub oder IoT Central über DPS herzustellen.

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

Um den Gerätecode zusammen mit dem C-SDK des Geräts zu kompilieren, verwenden Sie CMake in einer Linux-Umgebung wie Ubuntu oder Debian:

1. Öffnen Sie eine Terminalanwendung.

1. Installieren Sie **GCC**, **Git**, `cmake` und alle Abhängigkeiten mithilfe des Befehls `apt-get`:

    ```sh
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    Überprüfen Sie, ob die Version von `cmake` höher als **2.8.12** und die Version von **GCC** höher als **4.4.7** ist.

    ```sh
    cmake --version
    gcc --version
    ```

1. Klonen Sie das Repository [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c):

    ```sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

1. Kopieren Sie den Ordner, der den generierten Code enthält, in den Stammordner des Geräte-SDKs.

1. Öffnen Sie in VS Code die Datei `CMakeLists.txt` im Stammordner des Geräte-SDKs.

1. Fügen Sie die folgende Zeile am Ende der Datei `CMakeLists.txt` hinzu, um beim Kompilieren des SDKs den Ordner mit dem Gerätecodestub einzubinden:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. Erstellen Sie im Stammordner des Geräte-SDKs einen Ordner namens `cmake`, und navigieren Sie zu diesem Ordner.

    ```sh
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Codestub mithilfe von CMake zu kompilieren:

    ```cmd\sh
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON ..
    cmake --build .
    ```

1. Führen Sie die Anwendung nach erfolgreicher Kompilierung aus, und geben Sie dabei die IoT Hub-Geräteverbindungszeichenfolge als Parameter an.

    ```cmd\sh
    cd azure-iot-sdk-c/cmake/{generated_code_folder_name}/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

### <a name="windows"></a>Windows

Informationen dazu, wie Sie CMake und die Visual Studio C/C++-Compiler an der Befehlszeile verwenden, um den Gerätecode zusammen mit dem C-SDK für das Gerät unter Windows zu kompilieren, finden Sie hier: [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts](./quickstart-create-pnp-device.md). Die folgenden Schritte zeigen, wie Sie den Gerätecode zusammen mit dem C-SDK für das Gerät als CMake-Projekt in Visual Studio kompilieren.

1. Installieren Sie [Visual Studio 2019 (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/): Stellen Sie sicher, dass Sie die Komponente **NuGet-Paket-Manager** und die Workload **Desktopentwicklung mit C++** einschließen.

1. Öffnen Sie Visual Studio, und wählen Sie auf der Seite **Erste Schritte** die Option **Code klonen oder auschecken** aus:

1. Klonen Sie unter **Repositoryspeicherort** das Repository [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c):

    ```txt
    https://github.com/Azure/azure-iot-sdk-c
    ```

    Sie sollten davon ausgehen, dass dieser Vorgang einige Minuten dauert. Im Bereich **Team Explorer** können Sie den Fortschritt anzeigen.

1. Öffnen Sie das Repository **azure-iot-sdk-c** im **Team Explorer**, wählen Sie **Branches** aus, suchen Sie nach dem Branch **public-preview**, und checken Sie diesen aus.

    ![Öffentliche Vorschau](media/howto-develop-with-vs-vscode/vs-public-preview.png)

1. Kopieren Sie den Ordner, der den generierten Code enthält, in den Stammordner des Geräte-SDKs.

1. Öffnen Sie den Ordner `azure-iot-sdk-c` in VS.

1. Öffnen Sie die Datei `CMakeLists.txt` im Stammordner des Geräte-SDKs.

1. Fügen Sie die folgende Zeile am Ende der Datei `CMakeLists.txt` hinzu, um beim Kompilieren des SDKs den Ordner mit dem Gerätecodestub einzubinden:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. Suchen Sie in der Symbolleiste **Allgemein** das Dropdownmenü **Konfiguration**. Wählen Sie **Konfiguration verwalten** aus, um die CMake-Einstellung für Ihr Projekt hinzuzufügen.

    ![Konfiguration verwalten](media/howto-develop-with-vs-vscode/vs-manage-config.png)

1. Fügen Sie in den **CMake-Einstellungen** eine neue Konfiguration hinzu, und wählen Sie **x64-Release** als Ziel aus.

1. Fügen Sie in den **CMake-Befehlsargumenten** folgende Zeile hinzu:

    ```txt
    -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    ```

1. Speichern Sie die Datei .

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei `CMakeLists.txt` im Stammordner, und wählen Sie im Kontextmenü die Option **Kompilieren** aus, um das Geräte-SDK und den generierten Codestub zu kompilieren.

1. Führen Sie die Anwendung nach erfolgreicher Kompilierung an der Eingabeaufforderung aus, und eben Sie dabei die IoT Hub-Geräteverbindungszeichenfolge als Parameter an.

    ```cmd
    cd %USERPROFILE%\CMakeBuilds\{workspaceHash}\build\x64-Release\{generated_code_folder_name}\
    {generated_code_project_name}.exe "[IoT Hub device connection string]"
    ```

> [!TIP]
> Weitere Informationen zur Verwendung von CMake in Visual Studio finden Sie unter [Erstellen von CMake-Projekten](https://docs.microsoft.com/cpp/build/cmake-projects-in-visual-studio?view=vs-2019#building-cmake-projects).

### <a name="macos"></a>macOS

Die folgenden Schritte zeigen, wie Sie den Gerätecode zusammen mit dem C-SDK für das Gerät mit CMake unter macOS kompilieren:

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

1. Klonen Sie das Repository [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c):

    ```bash
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

1. Kopieren Sie den Ordner, der den generierten Code enthält, in den Stammordner des Geräte-SDKs.

1. Öffnen Sie in VS Code die Datei `CMakeLists.txt` im Stammordner des Geräte-SDKs.

1. Fügen Sie die folgende Zeile am Ende der Datei `CMakeLists.txt` hinzu, um beim Kompilieren des SDKs den Ordner mit dem Gerätecodestub einzubinden:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. Erstellen Sie im Stammordner des Geräte-SDKs einen Ordner namens `cmake`, und navigieren Sie zu diesem Ordner.

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
    cd azure-iot-sdk-c/cmake/{generated_code_folder_name}/
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
