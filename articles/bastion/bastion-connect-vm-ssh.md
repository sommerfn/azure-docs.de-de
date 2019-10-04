---
title: Verbinden Sie sich mit einer Linux-VM mit Azure Bastion | Microsoft Docs
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung zu Linux Virtual Machine mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 69548541d16db95f633400808f72aebaf59cff08
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477779"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion-preview"></a>Verbinden Sie sich mit SSH über Azure Bastion (Vorschau) mit einer virtuellen Linux-Maschine

Dieser Artikel zeigt Ihnen, wie Sie sicher und nahtlos SSH zu Ihren Linux-VMs in einem virtuellen Azure-Netzwerk durchführen können. Sie können sich direkt vom Azure-Portal aus mit einer VM verbinden. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

Sie können Azure Bastion verwenden, um sich mit einer virtuellen Linux-Maschine über SSH zu verbinden. Sie können sowohl Benutzername/Passwort als auch SSH-Schlüssel für die Authentifizierung verwenden. Sie können sich mit SSH-Schlüsseln mit Ihrer VM verbinden, indem Sie beide verwenden:

* Ein privater Schlüssel, den Sie manuell eingeben
* Eine Datei, die den privaten Schlüssel enthält

Der private SSH-Schlüssel muss in einem Format vorliegen, das mit `"-----BEGIN RSA PRIVATE KEY-----"` beginnt und `"-----END RSA PRIVATE KEY-----"` endet.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Weitere Informationen finden Sie unter [Erstellen eines azurblauen Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM in diesem virtuellen Netzwerk zu verbinden. Wenn Sie in dieser Vorschau die Verbindung mit Bastion herstellen, wird davon ausgegangen, dass Sie RDP für die Verbindung mit einer Windows-VM und SSH für die Verbindung mit Ihren Linux-VMs verwenden.

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

## <a name="username"></a>Verbinden: Verwendung von Benutzername und Passwort


1.  Verwenden Sie [diesen Link](https://aka.ms/BastionHost), um die Vorschau-Portalseite für Azure Bastion zu öffnen. Navigieren Sie zu der virtuellen Maschine, mit der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte eine virtuelle Linux-Maschine sein, wenn Sie eine SSH-Verbindung verwenden.
1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt [Bastion konfigurieren](bastion-create-host-portal.md). Wenn Sie die **Bastion** nicht aufgelistet sehen, haben Sie das Vorschauportal nicht geöffnet. Öffnen Sie das Portal mit [diesen Link](https://aka.ms/BastionHost).

      ![VM-Verbindung](./media/bastion-connect-vm-ssh/bastion.png)

1. Geben Sie den Benutzernamen und das Passwort für SSH auf Ihrer virtuellen Maschine ein.
1. Klicken Sie auf **Verbinden** nach der Eingabe des Schlüssels.

## <a name="privatekey"></a>Verbinden: Geben Sie manuell einen privaten Schlüssel ein

1.  Verwenden Sie [diesen Link](https://aka.ms/BastionHost), um die Vorschau-Portalseite für Azure Bastion zu öffnen. Navigieren Sie zu der virtuellen Maschine, mit der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte eine virtuelle Linux-Maschine sein, wenn Sie eine SSH-Verbindung verwenden.
1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt [Bastion konfigurieren](bastion-create-host-portal.md). Wenn Sie die **Bastion** nicht aufgelistet sehen, haben Sie das Vorschauportal nicht geöffnet. Öffnen Sie das Portal mit [diesen Link](https://aka.ms/BastionHost).

      ![VM-Verbindung](./media/bastion-connect-vm-ssh/bastion.png)

1. Geben Sie den Benutzernamen ein und wählen Sie **SSH Private Key**.
1. Geben Sie Ihren privaten Schlüssel in das Textfeld **SSH Private Key** ein (oder fügen Sie ihn direkt ein).
1. Klicken Sie auf **Verbinden** nach der Eingabe des Schlüssels.

## <a name="ssh"></a>Verbinden: Verwendung einer privaten Schlüsseldatei

1.  Verwenden Sie [diesen Link](https://aka.ms/BastionHost), um die Vorschau-Portalseite für Azure Bastion zu öffnen. Navigieren Sie zu der virtuellen Maschine, mit der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte eine virtuelle Linux-Maschine sein, wenn Sie eine SSH-Verbindung verwenden.

    ![VM-Verbindung](./media/bastion-connect-vm-ssh/connect.png)

1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt [Bastion konfigurieren](bastion-create-host-portal.md). Wenn Sie die **Bastion** nicht aufgelistet sehen, haben Sie das Vorschauportal nicht geöffnet. Öffnen Sie das Portal mit [diesen Link](https://aka.ms/BastionHost).

    ![VM-Verbindung](./media/bastion-connect-vm-ssh/bastion.png)

1. Geben Sie den Benutzernamen ein und wählen Sie **SSH Private Key aus der lokalen Datei**.
1. Klicken Sie auf **Durchsuchen** (das Ordnersymbol in der lokalen Datei).
1. Suchen Sie nach der Datei, und klicken Sie dann **öffnen**.
1. Klicken Sie auf **Verbinden**, um die Verbindung mit dem virtuellen Computer herzustellen. Sobald Sie auf Verbinden klicken, wird SSH zu dieser virtuellen Maschine direkt im Azure-Portal geöffnet. Diese Verbindung erfolgt über HTML5 mit Port 443 auf dem Bastion-Dienst über die private IP Ihrer virtuellen Maschine.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [Bastion FAQs](bastion-faq.md)
