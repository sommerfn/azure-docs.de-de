---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 35f506235f698fbcf42308e6f0b0f400e925df29
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361266"
---
### <a name="bastion-tier"></a>Bastionschicht

Der Bastionhost ist eine optionale Komponente, die Sie als Sprungserver verwenden können, um auf die Anwendungs- und Datenbankinstanzen zuzugreifen. Der Bastionhost-VM kann eine öffentliche IP-Adresse zugewiesen sein. Wir empfehlen Ihnen aber, eine ExpressRoute-Leitung oder Site-to-Site-VPN-Verbindung mit Ihrem lokalen Netzwerk einzurichten, um für sicheren Zugriff zu sorgen. Darüber hinaus sollten nur Ports für SSH (Port 22, Linux) oder RDP (Port 3389, Windows Server) für eingehenden Datenverkehr geöffnet werden. Stellen Sie zur Erzielung von Hochverfügbarkeit einen Bastionhost in zwei Verfügbarkeitszonen oder in einer einzelnen Verfügbarkeitsgruppe bereit.

Sie können auf Ihren VMs auch die SSH-Agent-Weiterleitung aktivieren, damit Sie auf andere VMs im virtuellen Netzwerk zugreifen können, indem Sie die Anmeldeinformationen von Ihrem Bastionhost weiterleiten. Sie können auch das SSH-Tunneling nutzen, um auf andere Instanzen zuzugreifen.

Hier ist ein Beispiel für die Agent-Weiterleitung angegeben:

```
ssh -A -t user@BASTION_SERVER_IP ssh -A root@TARGET_SERVER_IP`
```

Mit diesem Befehl wird für den Bastionhost eine Verbindung hergestellt, und anschließend wird `ssh` sofort erneut ausgeführt, damit Sie auf der Zielinstanz ein Terminal erhalten. Unter Umständen müssen Sie einen anderen Benutzer als „root“ auf der Zielinstanz angeben, wenn Ihr Cluster anders konfiguriert ist. Mit dem Argument `-A` wird die Agent-Verbindung weitergeleitet, damit Ihr privater Schlüssel automatisch auf dem lokalen Computer verwendet wird. Beachten Sie, dass es sich bei der Agent-Weiterleitung um eine Kette handelt. Der zweite `ssh`-Befehl enthält also auch `-A`, sodass für alle nachfolgenden SSH-Verbindungen, die über die Zielinstanz initiiert werden, ebenfalls Ihr lokaler privater Schlüssel verwendet wird.