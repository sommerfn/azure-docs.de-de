---
title: Azure Bastion – Problembehandlung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Problembehandlung für Azure Bastion ausführen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: cherylmc
ms.openlocfilehash: de112ff441bb53a0b3bc7f4ffa4456f1c241682c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510319"
---
# <a name="troubleshoot-azure-bastion"></a>Problembehandlung für Azure Bastion

In diesem Artikel erfahren Sie, wie Sie die Problembehandlung für Azure Bastion ausführen.

## <a name="nsg"></a>Fehler beim Erstellen einer NSG im AzureBastionSubnet

**F:** Beim Versuch, eine NSG im Azure Bastion-Subnetz zu erstellen, wird ein mit dem folgenden vergleichbarer Fehler angezeigt: *„Die Netzwerksicherheitsgruppe <NSG name> verfügt nicht über die erforderlichen Regeln für das Azure Bastion-Subnetz ‚AzureBastionSubnet‘“* .

**A:** Wenn Sie eine NSG erstellen und auf *AzureBastionSubnet* anwenden, stellen Sie sicher, dass Sie die folgenden Regeln in Ihrer NSG hinzugefügt haben. Wenn Sie diese Regeln nicht hinzufügen, schlägt die Erstellung/Aktualisierung der NSG fehl.

1. Konnektivität der Steuerungsebene – eingehend an 443 von GatewayManager
2. Diagnoseprotokollierung und andere – ausgehend an 443 an AzureCloud (Regionale Tags innerhalb dieses Diensttags werden noch nicht unterstützt.)
3. Ziel-VM – ausgehend für 3389 und 22 an VirtualNetwork

Ein Referenzbeispiel für die NSG-Regeln finden Sie in der [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azure-bastion).
Weitere Informationen finden Sie im [NSG-Leitfaden für Azure Bastion](bastion-nsg.md).

## <a name="sshkey"></a>Der SSH-Schlüssel kann nicht mit Azure Bastion verwendet werden.

**F:** Wenn ich versuche, meine SSH-Schlüsseldatei zu durchsuchen, wird ein mit dem folgenden vergleichbarer Fehler angezeigt: *„Der private SSH-Schlüssel muss mit ‚-----BEGIN RSA PRIVATE KEY-----‘ beginnen und mit ‚-----END RSA PRIVATE KEY-----‘ enden.“*

**A:** Azure Bastion unterstützt zu diesem Zeitpunkt nur RSA-SSH-Schlüssel. Stellen Sie sicher, dass Sie eine Schlüsseldatei durchsuchen, bei der es sich um einen privaten RSA-Schlüssel für SSH handelt, wobei der öffentliche Schlüssel auf der Ziel-VM bereitgestellt wird. 

Beispielsweise können Sie den folgenden Befehl verwenden, um einen neuen RSA-SSH-Schlüssel zu erstellen:

**ssh-keygen -t rsa -b 4096 -C "email@domain.com"

Ausgabe:

```
ashishj@dreamcatcher:~$ ssh-keygen -t rsa -b 4096 -C "email@domain.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ashishj/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ashishj/.ssh/id_rsa.
Your public key has been saved in /home/ashishj/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:c+SBciKXnwceaNQ8Ms8C4h46BsNosYx+9d+AUxdazuE email@domain.com
The key's randomart image is:
+---[RSA 4096]----+
|      .o         |
| .. ..oo+. +     |
|=.o...B==.O o    |
|==o  =.*oO E     |
|++ .. ..S =      |
|oo..   + =       |
|...     o o      |
|         . .     |
|                 |
+----[SHA256]-----+
```

## <a name="domain"></a>Anmeldung bei einem virtuellen, in die Domäne eingebundenen Windows-Computer nicht möglich

**F:** Ich kann keine Verbindung mit meinem virtuellen Windows-Computer herstellen, der in die Domäne eingebunden ist.

**A:** Azure Bastion unterstützt die Anmeldung bei virtuellen, in die Domäne eingebundenen Computern nur für Domänenanmeldungen, die auf dem Benutzernamen und Kennwort basieren. Wenn Sie die Domänenanmeldeinformationen im Azure-Portal angeben, verwenden Sie das UPN-Format (username@domain) anstelle des *domain\username*-Formats, um sich anzumelden. Dies wird für in die Domäne oder in Hybridumgebungen eingebundene virtuelle Computer unterstützt (die sowohl in die Domäne als auch in Azure AD eingebunden sind). Virtuelle Computer, die nur in Azure AD eingebunden sind, werden nicht unterstützt.

## <a name="filetransfer"></a>Probleme bei der Dateiübertragung

**F:** Wird die Dateiübertragung mit Azure Bastion unterstützt?

**A:** Die Dateiübertragung wird zurzeit nicht unterstützt. Wir arbeiten daran, die Unterstützung zu implementieren.

## <a name="blackscreen"></a>Schwarzer Bildschirm im Azure-Portal

**F:** Wenn ich versuche, eine Verbindung mithilfe von Azure Bastion herzustellen, erhalte ich einen schwarzen Bildschirm im Azure-Portal.

**A:** Dies geschieht, wenn ein Problem mit der Netzwerkverbindung zwischen dem Webbrowser und Azure Bastion vorliegt (beispielsweise kann Ihre Client-Internetfirewall den WebSockets-Datenverkehr blockieren) oder zwischen Azure Bastion und der Ziel-VM. In den meisten Fälle wurde auch eine NSG entweder auf AzureBastionSubnet oder auf das Subnetz der Ziel-VM angewendet, die den RDP-/SSH-Datenverkehr in Ihrem virtuellen Netzwerk blockiert. Lassen Sie WebSockets-Datenverkehr über die Client-Internetfirewall zu, und überprüfen Sie die NSGs für das Subnetz Ihrer Ziel-VM.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den [häufig gestellten Fragen zu Bastion](bastion-faq.md).