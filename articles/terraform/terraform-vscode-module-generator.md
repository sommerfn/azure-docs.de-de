---
title: Erstellen einer Terraform-Basisvorlage in Azure mithilfe von Yeoman
description: Hier erfahren Sie, wie Sie eine Terraform-Basisvorlage in Azure mithilfe von Yeoman erstellen.
services: terraform
ms.service: azure
keywords: Terraform, DevOps, virtueller Computer, Azure, Yeoman
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 7a628eb02170346a826cab19498d6fdf40cebddd
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173378"
---
# <a name="create-a-terraform-base-template-in-azure-using-yeoman"></a>Erstellen einer Terraform-Basisvorlage in Azure mithilfe von Yeoman

[Terraform](https://docs.microsoft.com/azure/terraform/
) bietet eine Möglichkeit, mühelos eine Infrastruktur in Azure zu erstellen. [Yeoman](https://yeoman.io/) erleichtert die Aufgabe des Modulentwicklers beim Erstellen von Terraform-Modulen erheblich und bietet gleichzeitig ein hervorragendes Framework *bewährter Methoden*.

In diesem Artikel erfahren Sie, wie Sie mit dem Yeoman-Modulgenerator eine Terraform-Basisvorlage erstellen. Anschließend erfahren Sie, wie Sie Ihre neue Terraform-Vorlage mithilfe von zwei unterschiedlichen Methoden testen:

- Ausführen Ihres Terraform-Moduls mithilfe einer Docker-Datei, die Sie in diesem Artikel erstellen
- Natives Ausführen Ihres Terraform-Moduls in Azure Cloud Shell

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- **Visual Studio Code**: Mit [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) untersuchen wir Dateien, die mit dem Yeoman-Generator erstellt werden. Sie können einen Code-Editor Ihrer Wahl verwenden.
- **Terraform**: Sie benötigen eine Installation von [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ), um das von Yeoman erstellte Modul auszuführen.
- **Docker**: Mit [Docker](https://www.docker.com/get-started) führen wir das vom Yeoman-Generator erstellte Modul aus. (Falls gewünscht, können Sie Ruby anstelle von Docker verwenden, um das Beispielmodul auszuführen.)
- **Programmiersprache Go**: Sie benötigen eine Installation von [Go](https://golang.org/), da die von Yeoman generierten Testfälle in Go geschrieben sind.

>[!NOTE]
>Bei den meisten in diesem Tutorial beschriebenen Verfahren spielen Befehlszeileneinträge eine Rolle. Die hier beschriebenen Schritte gelten für alle Betriebssysteme und Befehlszeilentools. In unseren Beispielen verwenden wir PowerShell für die lokale Umgebung und Git Bash für die Cloud Shell-Umgebung.

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

### <a name="install-nodejs"></a>Installieren von Node.js

Sie müssen [Node.js 6.0 oder höher](https://nodejs.org/en/download/) installieren, um Terraform in Cloud Shell zu verwenden.

>[!NOTE]
>Wenn Sie überprüfen möchten, ob Node.js installiert ist, öffnen Sie ein Terminalfenster, und geben Sie `node --version` ein.

### <a name="install-yeoman"></a>Installieren von Yeoman

Geben Sie an einer Eingabeaufforderung `npm install -g yo` ein.

![Installieren von Yeoman](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Installieren der Yeoman-Vorlage für das Terraform-Modul

Geben Sie an einer Befehlszeile `npm install -g generator-az-terra-module` ein.

![Installieren von generator-az-terra-module](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Um sicherzustellen, dass Yeoman installiert ist, geben Sie in einem Terminalfenster `yo --version` ein.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Erstellen eines leeren Ordners zum Speichern des von Yeoman generierten Moduls

Die Yeoman-Vorlage generiert Dateien im **aktuellen Verzeichnis**. Aus diesem Grund müssen Sie ein Verzeichnis erstellen.

>[!Note]
>Dieses leere Verzeichnis muss unter „$GOPATH/src“ erstellt werden. [Hier](https://github.com/golang/go/wiki/SettingGOPATH) finden Sie entsprechende Anweisungen.

Von einer Eingabeaufforderung aus:

1. Navigieren Sie zu dem übergeordneten Verzeichnis, dass das neue, leere Verzeichnis enthalten soll, das wir nun erstellen.
1. Geben Sie `mkdir <new-directory-name>` ein.

    > [!NOTE]
    > Ersetzen Sie `<new-directory-name>` durch den Namen Ihres neuen Verzeichnisses. In diesem Beispiel haben wir das neue Verzeichnis `GeneratorDocSample` genannt.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Wechseln Sie mit Eingabe von `cd <new directory's name>` in das neue Verzeichnis, und drücken Sie dann die **EINGABETASTE**.

    ![Navigieren Sie zu Ihrem neuen Verzeichnis.](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Um sicherzustellen, dass dieses Verzeichnis leer ist, geben Sie `ls` ein. Es sollten keine Dateien in der resultierenden Ausgabe dieses Befehls aufgelistet werden.

## <a name="create-a-base-module-template"></a>Erstellen einer Basismodulvorlage

Von einer Eingabeaufforderung aus:

1. Geben Sie `yo az-terra-module` ein.

1. Führen Sie die Anweisungen auf dem Bildschirm aus, um die folgenden Informationen anzugeben:

    - *Name des Terraform-Modulprojekts*

        ![Projektname](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >In diesem Beispiel haben wir `doc-sample-module` eingegeben.

    - *Möchten Sie die Docker-Imagedatei einbeziehen?*

        ![Docker-Imagedatei einbeziehen?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >Geben Sie `y` ein. Bei Auswahl von **n** unterstützt der generierte Modulcode nur die Ausführung im nativen Modus.

3. Geben Sie `ls` ein, um die resultierenden Dateien anzuzeigen, die erstellt werden.

    ![Erstellte Dateien auflisten](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Überprüfen des generierte Modulcodes

1. Starten Sie Visual Studio Code.

1. Wählen Sie in der Menüleiste **Datei > Ordner öffnen** aus, und wählen Sie den Ordner aus, den Sie erstellt haben.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Werfen wir einen Blick auf einige der Dateien, die vom Yeoman-Modulgenerator erstellt wurden.

>[!Note]
>In diesem Artikel verwenden wir die vom Yeoman-Modulgenerator erstellten Dateien „main.tf“, „variables.tf“ und „outputs.tf“. Wenn Sie allerdings Ihre eigenen Module erstellen, bearbeiten Sie diese Dateien, um die Funktionalität Ihres Terraform-Moduls anzupassen. Eine ausführliche Erläuterung dieser Dateien und ihrer Verwendung finden Sie unter [Terratest in Terraform Modules (Terratest in Terraform-Modulen)](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster).

### <a name="maintf"></a>main.tf

Definiert ein Modul namens *random-shuffle*. Die Eingabe ist eine *string_list*. Die Ausgabe ist die Anzahl der Permutationen.

### <a name="variablestf"></a>variables.tf

Definiert die vom Modul verwendeten Eingabe- und Ausgabevariablen.

### <a name="outputstf"></a>outputs.tf

Definiert, was das Modul ausgibt. Hier ist es der von **random_shuffle** zurückgegebene Wert, ein integriertes Terraform-Modul.

### <a name="rakefile"></a>Rakefile

Definiert die Schritte im Buildprozess. Diese Schritte umfassen:

- **build**: Überprüft die Formatierung der Datei „main.tf“.
- **unit**: Das generierte Modulgerüst enthält keinen Code für einen Komponententest. Wenn Sie ein Komponententestszenario angeben möchten, fügen Sie diesen Code hier hinzu.
- **e2e**: Führt einen End-to-End-Test des Moduls aus.

### <a name="test"></a>test

- Testfälle werden in Go geschrieben.
- Alle Codes im Test sind End-to-End-Tests.
- End-to-End-Tests versuchen, mit Terraform alle Elemente bereitzustellen, die unter **fixture** definiert sind, und dann die Ausgabe im **template_output.go**-Code mit den vordefinierten erwarteten Werten zu vergleichen.
- **Gopkg.lock** und **Gopkg.toml**: Dienen zum Definieren Ihrer Abhängigkeiten. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Testen Ihres neuen Terraform-Moduls mithilfe einer Docker-Datei

>[!NOTE]
>In unserem Beispiel führen wir das Modul als lokales Modul aus, sodass Azure nicht beteiligt ist.

### <a name="confirm-docker-is-installed-and-running"></a>Bestätigen, dass Docker installiert ist und ausgeführt wird

Geben Sie an einer Befehlszeile `docker version` ein.

![Docker-Version](media/terraform-vscode-module-generator/ymg-docker-version.png)

Die resultierende Ausgabe bestätigt, dass Docker installiert ist.

Um zu bestätigen, dass Docker wirklich ausgeführt wird, geben Sie `docker info` ein.

![Docker-Info](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Einrichten eines Docker-Containers

1. Geben Sie an einer Eingabeaufforderung Folgendes ein:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    Die Nachricht **Erfolgreich erstellt** wird angezeigt.

    ![Erfolgreich erstellt](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Geben Sie an der Eingabeaufforderung `docker image ls` ein.

    Ihr neu erstelltes Modul *terra-mod-example* ist aufgelistet.

    ![Repositoryergebnisse](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >Der Modulname *terra-mod-example* wurde in dem Befehl angegeben, den Sie oben in Schritt 1 eingegeben haben.

1. Geben Sie `docker run -it terra-mod-example /bin/sh` ein.

    Sie führen jetzt Docker aus und können die Datei durch Eingabe von `ls` auflisten.

    ![Docker-Datei auflisten](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Erstellen des Moduls

1. Geben Sie `bundle install` ein.

    Warten Sie, bis die Nachricht **Bundle abgeschlossen** angezeigt wird, und fahren Sie mit dem nächsten Schritt fort.

1. Geben Sie `rake build` ein.

    ![Rake build](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Durchführen des End-to-End-Tests

1. Geben Sie `rake e2e` ein.

1. Nach einigen Augenblicken wird die Meldung **ERFOLGREICH** angezeigt.

    ![ERFOLGREICH](media/terraform-vscode-module-generator/ymg-pass.png)

1. Geben Sie `exit` ein, um den End-to-End-Test abzuschließen und die Docker-Umgebung zu beenden.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Verwenden des Yeoman-Generators zum Erstellen und Testen eines Moduls in Cloud Shell

Im vorherigen Abschnitt haben Sie erfahren, wie Sie ein Terraform-Modul mithilfe einer Docker-Datei testen. In diesem Abschnitt verwenden Sie den Yeoman-Generator zum Erstellen und Testen eines Moduls in Cloud Shell.

Die Verwendung von Cloud Shell anstelle einer Docker-Datei vereinfacht den Prozess erheblich. Bei Verwendung von Cloud Shell:

- Sie müssen Node.js nicht installieren.
- Sie müssen Yeoman nicht installieren.
- Sie müssen Terraform nicht installieren.

Alle diese Elemente sind in Cloud Shell vorinstalliert.

### <a name="start-a-cloud-shell-session"></a>Starten einer Cloud Shell-Sitzung.

1. Starten Sie eine Azure Cloud Shell-Sitzung entweder über das [Azure-Portal](https://portal.azure.com/), über [shell.azure.com](https://shell.azure.com) oder über die [mobile Azure-App](https://azure.microsoft.com/features/azure-portal/mobile-app/).

1. Die Seite **Willkommen bei Azure Cloud Shell** wird geöffnet. Wählen Sie **Bash (Linux)** aus. (PowerShell wird nicht unterstützt.)

    ![Willkommen bei Azure Cloud Shell](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >In diesem Beispiel wurde „Bash“ (Linux) ausgewählt.

1. Wenn Sie noch kein Azure-Speicherkonto eingerichtet haben, wird folgender Bildschirm angezeigt. Klicken Sie auf **Speicher erstellen**.

    ![Für Sie wurde kein Speicher bereitgestellt.](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell wird in der Shell gestartet, die Sie zuvor ausgewählt haben, und zeigt Informationen zu dem Cloudlaufwerk an, das gerade für Sie erstellt wurde.

    ![Cloudlaufwerk wurde erstellt](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-folder-to-hold-your-terraform-module"></a>Vorbereiten eines Ordners für Ihr Terraform-Modul

1. Zu diesem Zeitpunkt wurde GOPATH in Ihren Umgebungsvariablen bereits von Cloud Shell für Sie konfiguriert. Geben Sie zum Anzeigen des Pfads `go env` ein.

1. Erstellen Sie den Ordner „$GOPATH“, falls dieser noch nicht vorhanden ist: Geben Sie `mkdir ~/go` ein.

1. Erstellen Sie einen Ordner im Ordner „$GOPATH“: Geben Sie `mkdir ~/go/src` ein. Mit diesem Ordner werden verschiedene von Ihnen erstellte Projektordner gespeichert und organisiert, z. B. der Ordner `<your-module-name>`, den Sie im nächsten Schritt erstellen.

1. Erstellen Sie einen Ordner für Ihr Terraform-Modul: Geben Sie `mkdir ~/go/src/<your-module-name>` ein.

    >[!NOTE]
    >In diesem Beispiel wurde als Ordnername `my-module-name` festgelegt.

1. Navigieren Sie zu Ihrem Modulordner: Geben Sie `cd ~/go/src/<your-module-name>` ein.

### <a name="create-and-test-your-terraform-module"></a>Erstellen und Testen Ihres Terraform-Moduls

1. Geben Sie `yo az-terra-module` ein, und befolgen Sie die Anweisungen des Assistenten.

    >[!NOTE]
    >Wenn Sie gefragt werden, ob Sie die Docker-Dateien erstellen möchten, können Sie `N` eingeben.

1. Geben Sie `bundle install` ein, um die Abhängigkeiten zu installieren.

    Warten Sie, bis die Nachricht **Bundle abgeschlossen** angezeigt wird, und fahren Sie mit dem nächsten Schritt fort.

1. Geben Sie `rake build` ein, um Ihr Modul zu erstellen.

    ![Rake build](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Geben Sie `rake e2e` ein, um den End-to-End-Test auszuführen.

1. Nach einigen Augenblicken wird die Meldung **ERFOLGREICH** angezeigt.

    ![ERFOLGREICH](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Installieren und Verwenden der Azure Terraform-Erweiterung für Visual Studio Code](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
