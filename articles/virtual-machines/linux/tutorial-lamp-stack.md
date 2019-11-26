---
title: 'Tutorial: Bereitstellen von LAMP auf einem virtuellen Linux-Computer in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie den LAMP-Stack auf einem virtuellen Linux-Computer in Azure installieren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: 3b1f4ef9d4e36c35cc72716125392aaff05eab6d
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034470"
---
# <a name="tutorial-install-a-lamp-web-server-on-a-linux-virtual-machine-in-azure"></a>Tutorial: Installieren eines LAMP-Webservers auf einem virtuellen Linux-Computer in Azure

In diesem Artikel werden Sie durch die Bereitstellung eines Apache-Webservers sowie von MySQL und PHP (LAMP-Stack) auf einem virtuellen Ubuntu-Computer in Azure geführt. Wenn Sie den NGINX-Webserver bevorzugen, finden Sie entsprechende Informationen im Tutorial zum [LEMP-Stack](tutorial-lemp-stack.md). Um den LAMP-Server in Aktion zu sehen, können Sie optional eine WordPress-Website installieren und konfigurieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Ubuntu-Computers („L“ im LAMP-Stack)
> * Öffnen von Port 80 für Webdatenverkehr
> * Installieren von Apache, MySQL und PHP
> * Überprüfen der Installation und Konfiguration
> * Installieren von WordPress auf dem LAMP-Server

Dieses Setup ist für schnelle Tests oder Proof of Concept gedacht. Weitere Informationen zum LAMP-Stack, einschließlich Empfehlungen für eine Produktionsumgebung, finden Sie in der [Ubuntu-Dokumentation](https://help.ubuntu.com/community/ApacheMySQLPHP).

Dieses Tutorial verwendet die CLI innerhalb des Diensts [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), der ständig auf die neueste Version aktualisiert wird. Wählen Sie zum Öffnen von Cloud Shell oben in einem Codeblock die Option **Ausprobieren** aus.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.30 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Installieren von Apache, MySQL und PHP

Führen Sie den folgenden Befehl aus, um die Ubuntu-Paketquellen zu aktualisieren und Apache, MySQL und PHP zu installieren. Beachten Sie das Caretzeichen (^) am Ende des Befehls, das Teil des Paketnamens `lamp-server^` ist. 


```bash
sudo apt update && sudo apt install lamp-server^
```

Sie werden aufgefordert, die Pakete und andere Abhängigkeiten zu installieren. Dieser Prozess installiert die PHP-Erweiterungen, die mindestens für die Verwendung von PHP mit MySQL erforderlich sind.  

## <a name="verify-installation-and-configuration"></a>Überprüfen der Installation und Konfiguration


### <a name="verify-apache"></a>Überprüfen von Apache

Überprüfen Sie die Version von Apache mit dem folgenden Befehl:
```bash
apache2 -v
```

Nachdem Apache installiert und Port 80 für den virtuellen Computer geöffnet wurde, ist der Zugriff auf den Webserver über das Internet möglich. Öffnen Sie zum Anzeigen der Apache2 Ubuntu-Standardseite einen Webbrowser, und geben Sie die öffentliche IP-Adresse des virtuellen Computers ein. Verwenden Sie die öffentliche IP-Adresse, die Sie zum Herstellen einer SSH-Verbindung mit dem virtuellen Computer verwendet haben:

![Apache-Standardseite][3]


### <a name="verify-and-secure-mysql"></a>Überprüfen und Sichern von MySQL

Überprüfen Sie die Version von MySQL mit dem folgenden Befehl (beachten Sie die Großschreibung des `V`-Parameters):

```bash
mysql -V
```

Führen Sie das Skript `mysql_secure_installation` aus, um die Installation von MySQL zu sichern, u.a. durch Festlegen eines Stammkennworts. 

```bash
sudo mysql_secure_installation
```

Sie können optional das Plug-In zur Kennwortüberprüfung einrichten (empfohlen). Legen Sie dann ein Kennwort für den MySQL-Root-Benutzer fest, und konfigurieren Sie die übrigen Sicherheitseinstellungen für Ihre Umgebung. Es wird empfohlen, auf alle Fragen mit „Y“ (ja) zu antworten.

Wenn Sie MySQL-Features (MySQL-Datenbank erstellen, Benutzer hinzufügen oder Konfigurationseinstellungen ändern) ausprobieren möchten, melden Sie sich bei MySQL an. Dieser Schritt ist für den Abschluss des Tutorials nicht erforderlich.

```bash
sudo mysql -u root -p
```

Beenden Sie anschließend die MySQL-Eingabeaufforderung durch Eingabe von `\q`.

### <a name="verify-php"></a>Überprüfen von PHP

Überprüfen Sie die Version von PHP mit dem folgenden Befehl:

```bash
php -v
```

Wenn Sie weitere Tests durchführen möchten, erstellen Sie schnell eine PHP-Infoseite zum Anzeigen in einem Browser. Die PHP-Infoseite wird mit dem folgenden Befehl erstellt:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Nun können Sie die erstellte PHP-Infoseite überprüfen. Öffnen Sie einen Browser, und wechseln Sie zu `http://yourPublicIPAddress/info.php`. Ersetzen Sie die öffentliche IP-Adresse Ihres virtuellen Computers. Die Seite sollte ähnlich wie diese Abbildung aussehen.

![PHP-Infoseite][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen LAMP-Server in Azure bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Ubuntu-Computers
> * Öffnen von Port 80 für Webdatenverkehr
> * Installieren von Apache, MySQL und PHP
> * Überprüfen der Installation und Konfiguration
> * Installieren von WordPress auf dem LAMP-Server

Im nächsten Tutorial erfahren Sie, wie Sie Webserver mit SSL-Zertifikaten sichern.

> [!div class="nextstepaction"]
> [Sichern von Webservern mit SSL](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png
