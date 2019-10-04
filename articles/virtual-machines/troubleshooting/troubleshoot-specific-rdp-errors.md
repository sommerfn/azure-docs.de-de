---
title: Bestimmte RDP-Fehlermeldungen für Azure-VMs | Microsoft Docs
description: Grundlegendes zu Fehlermeldungen, die möglicherweise angezeigt werden, wenn Sie versuchen, eine Remotedesktopverbindung zu einem Windows-Computer in Azure zu verwenden
keywords: Remotedesktop-Fehler,Remotedesktop-Verbindungsfehler,Verbindung mit virtuellem Computer nicht möglich,Remotedesktop-Problembehandlung
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 851c5eb4ebfee4e4a4836a07b51578dd2b0c68cd
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71088186"
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Problembehandlung bei bestimmten RDP-Fehlermeldungen in einer Windows-VM in Azure
Sie erhalten möglicherweise eine bestimmte Fehlermeldung bei Verwendung einer Remotedesktopverbindung mit einem virtuellen Windows-Computer (VM) in Azure. In diesem Artikel werden einige der häufigeren Fehlermeldungen beschrieben, zusammen mit Problemlösungsschritten zur Fehlerbehebung. Wenn beim Herstellen einer Verbindung mit Ihrem virtuellen Computer mit RDP keine bestimmte Fehlermeldung auftritt, finden Sie Informationen im [Handbuch zur Problembehandlung bei Remotedesktopfehlern](troubleshoot-rdp-connection.md).

Hinweise auf Informationen zu bestimmten Fehlermeldungen:

* [Die Verbindung mit der Remotesitzung wurde getrennt, da keine Lizenzserver für Remotedesktop vorhanden sind](#rdplicense).
* [Remotedesktop kann den Computer „Name“ nicht finden.](#rdpname)
* [Authentifizierungsfehler. Die lokale Sicherheitsautorität (LSA) ist nicht erreichbar](#rdpauth).
* [Windows-Sicherheitsfehler: Ihre Anmeldeinformationen haben nicht funktioniert](#wincred).
* [Dieser Computer kann keine Verbindung mit dem Remotecomputer herstellen.](#rdpconnect)

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Die Verbindung mit der Remotesitzung wurde getrennt, da keine Lizenzserver für Remotedesktop vorhanden sind.
Ursache: Die 120-Tage-Kulanzfrist für die Rolle „Remotedesktopserver“ ist abgelaufen, und Sie müssen Lizenzen installieren.

Als Problemumgehung speichern Sie eine lokale Kopie der RDP-Datei aus dem Portal, und führen Sie diesen Befehl an einer PowerShell-Eingabeaufforderung aus, um eine Verbindung herzustellen. Hiermit wird die Lizenz nur für diese Verbindung deaktiviert:

        mstsc <File name>.RDP /admin

Wenn Sie eigentlich nicht mehr als zwei gleichzeitige Remotedesktopverbindungen mit dem virtuellen Computer benötigen, können Sie mit Server-Manager die Rolle „Remotedesktopserver“ entfernen.

Weitere Informationen finden Sie im Blogbeitrag [Azure VM fails with „No Remote Desktop License Servers available“](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/)(Azure-VM schlägt fehl mit „Keine Lizenzserver für Remotedesktop verfügbar“).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Remotedesktop kann den Computer „Name“ nicht finden.
Ursache: Der Remotedesktopclient auf Ihrem Computer kann den Namen des Computers, der in den Einstellungen der RDP-Datei angegeben ist, nicht auflösen.

Lösungsvorschläge:

* Wenn Sie sich im Intranet einer Organisation befinden, sollten Sie sicherstellen, dass Ihr Computer Zugriff auf den Proxyserver hat und diesem HTTPS-Datenverkehr senden kann.
* Wenn Sie eine lokal gespeicherte RDP-Datei verwenden, können Sie versuchen, die vom Portal generierte Datei zu verwenden. Dadurch wird sichergestellt, dass Sie den richtigen DNS-Namen für den virtuellen Computer oder den Clouddienst und den Endpunktport des virtuellen Computers verwenden. Dies ist eine RDP-Beispieldatei, die vom Portal generiert wurde:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Der Adressteil dieser RDP-Datei besteht aus:

* dem vollqualifizierten Domänennamen des Clouddiensts, der den virtuellen Computer enthält (in diesem Beispiel „tailspin-azdatatier.cloudapp.net“)
* dem externen TCP-Port des Endpunkts für den Remotedesktop-Datenverkehr (55919)

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Authentifizierungsfehler. Die lokale Sicherheitsautorität (LSA) ist nicht erreichbar.
Ursache: Der virtuelle Zielcomputer kann die Sicherheitsautorität im Benutzernamensteil Ihrer Anmeldeinformationen nicht finden.

Wenn Ihr Benutzername das Format *Sicherheitsautorität*\\*Benutzername* (Beispiel: „CORP\User1“) aufweist, ist *Sicherheitsautorität* entweder der Computername des virtuellen Computers (für die lokale Sicherheitsautorität) oder der Name einer Active Directory-Domäne.

Lösungsvorschläge:

* Wenn das Konto lokal für den virtuellen Computer gilt, sollten Sie sicherstellen, dass der Name richtig geschrieben ist.
* Wenn sich das Konto in einer Active Directory-Domäne befindet, sollten Sie die richtige Schreibweise des Domänennamens überprüfen.
* Falls es ein Active Directory-Domänenkonto ist und der Domänenname richtig geschrieben ist, sollten Sie sicherstellen, dass in der Domäne ein Domänencontroller verfügbar ist. In virtuellen Azure-Netzwerken, die Domänencontroller enthalten, tritt häufig das Problem auf, dass ein Domänencontroller nicht verfügbar ist, weil er nicht gestartet wurde. Um dieses Problem zu umgehen, können Sie anstelle eines Domänenkontos ein lokales Administratorkonto verwenden.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows-Sicherheitsfehler: Ihre Anmeldeinformationen haben nicht funktioniert.
Ursache: Der virtuelle Zielcomputer kann Ihren Kontonamen und das Kennwort nicht überprüfen.

Ein Windows-basierter Computer kann die Anmeldeinformationen eines lokalen Kontos oder eines Domänenkontos überprüfen.

* Verwenden Sie für lokale Konten die Syntax *Computername*\\*Benutzername* (Beispiel: SQL1\Admin4798).
* Verwenden Sie für Domänenkonten die Syntax *Domänenname*\\*Benutzername* (Beispiel: CONTOSO\peterodman).

Bei virtuellen Computern, die Sie in einer neuen Active Directory-Gesamtstruktur zu Domänencontrollern heraufgestuft haben, wird das lokale Administratorkonto, mit dem Sie sich angemeldet haben, in der neuen Gesamtstruktur und Domäne in ein entsprechendes Konto mit demselben Kennwort konvertiert. Das lokale Konto wird gelöscht.

Wenn Sie sich beispielsweise mit dem lokalen Konto „DC1\DCAdmin“ angemeldet und dann den virtuellen Computer zu einem Domänencontroller in einer neuen Gesamtstruktur für die Domäne „corp.contoso.com“ heraufgestuft haben, wird das lokale Konto „DC1\DCAdmin“ gelöscht und ein neues Domänenkonto („CORP\DCAdmin“) mit demselben Kennwort erstellt.

Stellen Sie sicher, dass der Kontoname ein Name ist, der vom virtuellen Computer als gültiges Konto bestätigt werden kann, und dass das Kennwort korrekt ist.

Wenn Sie das Kennwort für das lokale Administratorkonto ändern müssen, finden Sie hierzu weitere Informationen unter [Zurücksetzen eines Kennworts oder des Remotedesktopdiensts für virtuelle Computer in Windows](reset-rdp.md).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Dieser Computer kann keine Verbindung mit dem Remotecomputer herstellen.
Ursache: Das Konto zum Herstellen der Verbindung verfügt nicht über Remotedesktop-Anmelderechte.

Jeder Windows-Computer hat eine lokale Gruppe von Remotedesktopbenutzern, in der die Konten und Gruppen enthalten sind, die sich per Remoteverbindung anmelden können. Auch Mitglieder der lokalen Gruppe „Administratoren“ haben Zugriff, obwohl diese Konten nicht in der lokalen Gruppe „Remotedesktopbenutzer“ aufgelistet sind. Bei in einer Domäne eingebundenen Computern enthält die lokale Gruppe „Administratoren“ auch die Domänenadministratoren für die Domäne.

Stellen Sie sicher, dass das Konto, das Sie zum Herstellen der Verbindung verwenden, über Remotedesktop-Anmelderechte verfügt. Verwenden Sie als Problemumgehung ein Domänenkonto oder lokales Administratorkonto, um per Remotedesktop eine Verbindung herzustellen. Um das gewünschte Konto zur lokalen Remotedesktopbenutzer-Gruppe hinzuzufügen, verwenden Sie das Microsoft Management Console-Snap-In (**Systemprogramme > Lokale Benutzer und Gruppen > Gruppen > Remotedesktopbenutzer**).

## <a name="next-steps"></a>Nächste Schritte
Wenn keiner dieser Fehler aufgetreten ist, und Sie einen unbekannten Fehler beim Herstellen einer Verbindung über RDP haben, finden Sie Informationen im [Handbuch zur Problembehandlung bei Remotedesktopfehlern](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Informationen zum Behandeln von Problemen beim Zugreifen auf Anwendungen, die auf einem virtuellen Computer ausgeführt werden, finden Sie unter [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Informationen zu Problemen beim Herstellen einer SSH-Verbindung (Secure Shell) mit einem virtuellen Linux-Computer in Azure finden Sie unter [Behandeln von Problemen, Fehlern oder Ablehnungen im Zusammenhang mit der SSH-Verbindung mit einem virtuellen Azure Linux-Computer](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

