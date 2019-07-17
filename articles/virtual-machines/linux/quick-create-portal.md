---
title: 'Schnellstart: Erstellen eines virtuellen Linux-Computers im Azure-Portal | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Azure-Portals einen virtuellen Linux-Computer erstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/12/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f491a160e0f4f72f072dc43c46571f96dd19a297
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671048"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Schnellstart: Erstellen eines virtuellen Linux-Computers im Azure-Portal

Virtuelle Azure-Computer (VMs) können über das Azure-Portal erstellt werden. Das Azure-Portal ist eine browserbasierte Benutzeroberfläche, über die Sie virtuelle Computer und die dazugehörigen Ressourcen erstellen können. In dieser Schnellstartanleitung wird gezeigt, wie Sie über das Azure-Portal einen virtuellen Linux-Computer unter Ubuntu 16.04 LTS bereitstellen. Wenn Sie den virtuellen Computer in Aktion sehen möchten, stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her und installieren den NGINX-Webserver.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für diesen Schnellstart benötigen Sie ein SSH-Schlüsselpaar. Falls Sie bereits über ein SSH-Schlüsselpaar verfügen, können Sie diesen Schritt überspringen.

Öffnen Sie eine Bash-Shell, und verwenden [ssh-keygen](https://www.ssh.com/ssh/keygen/), um ein SSH-Schlüsselpaar zu erstellen. Sollte auf Ihrem Computer keine Bash-Shell zur Verfügung stehen, können Sie auch [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.  

```bash
ssh-keygen -t rsa -b 2048
```

Der obige Befehl generiert öffentliche und private Schlüssel mit dem Standardnamen `id_rsa` in `~/.ssh directory`. Der Befehl gibt den vollständigen Pfad des öffentlichen Schlüssels zurück. Verwenden Sie den Pfad des öffentlichen Schlüssels, um mithilfe von `cat` seinen Inhalt anzuzeigen.

```bash 
cat ~/.ssh/id_rsa.pub
```

Speichern Sie die Ausgabe dieses Befehls. Sie wird benötigt, wenn Sie Ihr Administratorkonto für die Anmeldung bei Ihrem virtuellen Computer konfigurieren.

Ausführlichere Informationen zum Erstellen von SSH-Schlüsselpaaren, u.a. zur Verwendung von PuTTy, finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](ssh-from-windows.md).

Wenn Sie Ihr SSH-Schlüsselpaar mithilfe von Cloud Shell erstellen, wird es in einer Azure-Dateifreigabe gespeichert, die [automatisch von Cloud Shell eingebunden](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage) wird. Wenn Sie diese Dateifreigabe oder das Speicherkonto löschen, bevor Sie Ihre Schlüssel abgerufen haben, können Sie nicht mehr auf den virtuellen Computer zugreifen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie links oben im Azure-Portal auf **Ressource erstellen**.

1. Suchen Sie im Suchfeld oberhalb der Liste der Azure Marketplace-Ressourcen nach **Ubuntu Server 16.04 LTS** von Canonical, und klicken Sie auf **Erstellen**.

1. Stellen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** sicher, dass das richtige Abonnement ausgewählt ist, und wählen Sie dann **Neu erstellen** unter **Ressourcengruppe**  aus. Geben Sie in dem Popup *myResourceGroup* als Namen der Ressourcengruppe ein, und wählen Sie dann **OK** aus. 

    ![Erstellen einer neuen Ressourcengruppe für Ihre VM](./media/quick-create-portal/project-details.png)

1. Geben Sie unter **Instanzdetails** *myVM* als **Namen des virtuellen Computers** ein, und wählen Sie *USA, Osten* als Ihre **Region** aus. Behalten Sie die restlichen Standardwerte bei.

    ![Abschnitt „Instanzdetails“](./media/quick-create-portal/instance-details.png)

1. Wählen Sie unter **Administratorkonto** die Option **Öffentlicher SSH-Schlüssel** aus, geben Sie Ihren Benutzernamen ein, und fügen Sie Ihren öffentlichen Schlüssel in das Textfeld ein. Entfernen Sie in Ihrem öffentlichen Schlüssel alle führenden bzw. nachgestellten Leerzeichen.

    ![Administratorkonto](./media/quick-create-portal/administrator-account.png)

1. Wählen Sie unter **Regeln für eingehende Ports** > **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** aus, und wählen Sie dann **SSH (22)** und **HTTP-(80)** aus der Dropdownliste aus. 

    ![Öffnen von Ports für RDP und HTTP](./media/quick-create-portal/inbound-port-rules.png)

1. Belassen Sie die übrigen Standardeinstellungen, und wählen Sie dann die Schaltfläche **Überprüfen + erstellen** am unteren Rand der Seite aus.

1. Auf der Seite zum **Erstellen eines virtuellen Computers** werden die Details des virtuellen Computers angezeigt, den Sie erstellen möchten. Klicken Sie auf **Erstellen**, wenn Sie so weit sind.

Die Bereitstellung des virtuellen Computers dauert ein paar Minuten. Fahren Sie nach Abschluss der Bereitstellung mit dem nächsten Abschnitt fort.

    
## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her.

1. Klicken Sie auf der Seite „Übersicht“ des virtuellen Computers auf die Schaltfläche **Verbinden**. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. Übernehmen Sie auf der Seite zum **Herstellen der Verbindung mit dem virtuellen Computer** die Standardoptionen, um basierend auf der IP-Adresse eine Verbindung über den Port 22 herzustellen. Unter **Login using VM local account** (Anmelden mit einem lokalen VM-Konto) wird ein Verbindungsbefehl angezeigt. Klicken Sie auf die Schaltfläche, um den Befehl zu kopieren. Das folgende Beispiel zeigt den Befehl für die SSH-Verbindung:

    ```bash
    ssh azureuser@10.111.12.123
    ```

3. Verwenden Sie die gleiche Bash-Shell, die Sie auch zum Erstellen Ihres SSH-Schlüsselpaars verwendet haben (also beispielsweise [Azure Cloud Shell](https://shell.azure.com/bash) oder Ihre lokale Bash-Shell), und fügen Sie den SSH-Verbindungsbefehl ein, um eine SSH-Sitzung zu erstellen. 

## <a name="install-web-server"></a>Installieren des Webservers

Wenn Sie den virtuellen Computer in Aktion sehen möchten, installieren Sie den NGINX-Webserver. Aktualisieren Sie über die SSH-Sitzung Ihre Paketquellen, und installieren Sie dann das aktuelle NGINX-Paket.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

Geben Sie abschließend `exit` ein, um die SSH-Sitzung zu verlassen.


## <a name="view-the-web-server-in-action"></a>Anzeigen des Webservers in Aktion

Verwenden Sie einen beliebigen Webbrowser, um die Standardwillkommensseite von NGINX anzuzeigen. Geben Sie die öffentliche IP-Adresse Ihres virtuellen Computers als Webadresse ein. Die öffentliche IP-Adresse finden Sie auf der Übersichtsseite für den virtuellen Computer sowie in der weiter oben verwendeten SSH-Verbindungszeichenfolge.

![NGINX-Standardwebsite](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe für den virtuellen Computer aus, klicken Sie auf **Löschen**, und bestätigen Sie den Namen der zu löschenden Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen virtuellen Computer bereitgestellt, eine Netzwerksicherheitsgruppe und eine Regel erstellt sowie einen einfachen Webserver installiert. Fahren Sie mit dem Tutorial für virtuelle Linux-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Tutorials für virtuelle Azure Linux-Computer](./tutorial-manage-vm.md)
