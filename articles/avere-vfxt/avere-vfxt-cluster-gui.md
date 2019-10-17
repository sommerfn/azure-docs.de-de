---
title: Zugreifen auf die Avere vFXT-Systemsteuerung – Azure
description: Herstellen einer Verbindung mit dem vFXT-Cluster und mit der browserbasierten Avere-Systemsteuerung zum Konfigurieren von Avere vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: rohogue
ms.openlocfilehash: 098ed98c1680fa2ea38c377e9e34719ba778b175
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255038"
---
# <a name="access-the-vfxt-cluster"></a>Zugreifen auf den vFXT-Cluster

Verwenden Sie die Avere-Systemsteuerung, um Einstellungen zu ändern und den Avere vFXT-Cluster zu überwachen. Die Avere-Systemsteuerung ist eine browserbasierte grafische Oberfläche für den Cluster.

Da sich der vFXT-Cluster in einem privaten virtuellen Netzwerk befindet, müssen Sie einen SSH-Tunnel erstellen oder eine andere Methode verwenden, um die IP-Adresse für die Clusterverwaltung zu erreichen. Es gibt zwei grundlegende Schritte: 

1. Erstellen einer Verbindung zwischen Ihrem Arbeitsplatz und dem privaten VNet 
1. Laden der Systemsteuerung des Clusters in einem Webbrowser 

> [!NOTE] 
> In diesem Artikel wird davon ausgegangen, dass Sie eine öffentliche IP-Adresse auf dem Clustercontroller oder auf einem anderen virtuellen Computer innerhalb des virtuellen Netzwerks Ihres Clusters festgelegt haben. Dieser Artikel beschreibt, wie Sie diesen virtuellen Computer als Host für den Zugriff auf den Cluster verwenden. Wenn Sie ein VPN oder ExpressRoute für den VNet-Zugriff verwenden, wechseln Sie zu [Herstellen einer Verbindung mit der Avere-Systemsteuerung](#connect-to-the-avere-control-panel-in-a-browser).

Stellen Sie vor dem Herstellen der Verbindung sicher, dass das SSH-Schlüsselpaar „public/private“, das Sie beim Erstellen des Clustercontrollers verwendet haben, auf Ihrem lokalen Computer installiert ist. Lesen Sie die Dokumentation zu SSH-Schlüsseln für [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) oder für [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys), wenn Sie Hilfe benötigen. (Wenn Sie ein Kennwort anstelle eines öffentlichen Schlüssels verwendet haben, werden Sie beim Verbinden aufgefordert, dieses einzugeben.) 

## <a name="create-an-ssh-tunnel"></a>Erstellen eines SSH-Tunnels 

Sie können einen SSH-Tunnel über die Befehlszeile eines Linux-basierten oder Windows 10-Clientsystems erstellen. 

Verwenden einen SSH-Tunneling-Befehl mit dem folgenden Format: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*\@*controller_public_IP*

Dieser Befehl stellt über die IP-Adresse des Clustercontrollers eine Verbindung mit der IP-Adresse für die Clusterverwaltung her.

Beispiel:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Die Authentifizierung erfolgt automatisch, wenn Sie Ihren öffentlichen SSH-Schlüssel zur Erstellung des Clusters verwendet haben und der passende Schlüssel auf dem Clientsystem installiert ist. Wenn Sie ein Kennwort verwendet haben, fordert das System Sie zur Eingabe auf.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Herstellen einer Verbindung mit der Avere-Systemsteuerung in einem Browser

Dieser Schritt verwendet einen Webbrowser, um eine Verbindung mit dem Konfigurationsprogramm herzustellen, das auf dem vFXT-Cluster ausgeführt wird.

* Für eine SSH-Tunnelverbindung öffnen Sie Ihren Webbrowser, und navigieren Sie zu `https://127.0.0.1:8443`. 

  Sie haben bei der Erstellung des Tunnels eine Verbindung mit der IP-Adresse des Clusters hergestellt, daher müssen Sie nur die localhost-IP-Adresse im Browser verwenden. Wenn Sie einen anderen lokalen Port als 8443 verwendet haben, verwenden Sie stattdessen Ihre Portnummer.

* Wenn Sie ein VPN oder ExpressRoute verwenden, um den Cluster zu erreichen, navigieren Sie in Ihrem Browser zur IP-Adresse der Clusterverwaltung. Beispiel: ``https://203.0.113.51``

Je nach Browser müssen Sie möglicherweise auf **Erweitert** klicken und überprüfen, ob die Weiterleitung zur Seite sicher ist.

Geben Sie den Benutzernamen `admin` und das Administratorkennwort ein, das Sie beim Erstellen des Clusters angegeben haben.

![Screenshot der Avere-Anmeldeseite, die mit dem Benutzernamen „admin“ und einem Kennwort gefüllt ist](media/avere-vfxt-gui-login.png)

Klicken Sie auf **Anmelden**, oder drücken Sie die EINGABETASTE auf Ihrer Tastatur.

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie jetzt, da Sie auf den Cluster zugreifen können, die Option [Support](avere-vfxt-enable-support.md).
