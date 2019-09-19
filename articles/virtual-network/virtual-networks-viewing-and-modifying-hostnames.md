---
title: Anzeigen und Ändern von Hostnamen | Microsoft Docs
description: Anzeigen und Ändern von Hostnamen für virtuelle Computer sowie Web- und Workerrollen in Azure für die Namensauflösung
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: cce248e2906f4a36737388e8cc7124b1bb19fbae
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71058673"
---
# <a name="viewing-and-modifying-hostnames"></a>Anzeigen und Ändern von Hostnamen
Damit auf Ihre Rolleninstanzen mit dem Hostnamen verwiesen werden kann, müssen Sie den Wert für den Hostnamen in der Dienstkonfigurationsdatei für jede Rolle festlegen. Hierzu fügen Sie den gewünschten Hostnamen dem **vmName**-Attribut des **Role**-Elements hinzu. Der Wert des **vmName** -Attributs wird als Grundlage für den Hostnamen der einzelnen Rolleninstanzen verwendet. Wenn **vmName** beispielsweise *webrole* lautet und drei Instanzen dieser Rolle vorhanden sind, lauten die Hostnamen der Instanzen *webrole0*, *webrole1* und *webrole2*. Sie müssen keinen Hostnamen für virtuelle Computer in der Konfigurationsdatei angeben, da der Hostname für einen virtuellen Computer auf Grundlage des Namens des virtuellen Computers erstellt wird. Weitere Informationen zum Konfigurieren eines Microsoft Azure-Diensts finden Sie unter [Azure-Dienstkonfigurationsschema (.cscfg-Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Anzeigen von Hostnamen
Sie können die Hostnamen von virtuellen Computern und Rolleninstanzen in einem Clouddienst mithilfe der folgenden Tools anzeigen.

### <a name="service-configuration-file"></a>Dienstkonfigurationsdatei
Sie können die Dienstkonfigurationsdatei für einen bereitgestellten Dienst auf dem Blatt **Konfigurieren** für den Dienst im Azure-Portal herunterladen. Dann können Sie das **vmName**-Attribut für das **Role name**-Element suchen, um den Hostnamen anzuzeigen. Beachten Sie, dass dieser Hostname als Grundlage für den Hostnamen der einzelnen Rolleninstanzen verwendet wird. Wenn **vmName** beispielsweise *webrole* lautet und drei Instanzen dieser Rolle vorhanden sind, lauten die Hostnamen der Instanzen *webrole0*, *webrole1* und *webrole2*.

### <a name="remote-desktop"></a>Remotedesktop
Nach der Aktivierung von Verbindungen zu den virtuellen Computern oder Rolleninstanzen über Remotedesktop (Windows), Windows PowerShell-Remoting (Windows) oder SSH (Linux und Windows) haben Sie mehrere Möglichkeiten, um den Hostnamen aus einer aktiven Remotedesktopverbindung anzuzeigen:

* Geben Sie an der Eingabeaufforderung oder im SSH-Terminal "hostname" ein.
* Geben Sie "ipconfig /all" an der Eingabeaufforderung ein (nur Windows).
* Zeigen Sie den Computernamen in den Systemeinstellungen an (nur Windows).

### <a name="azure-service-management-rest-api"></a>Azure-Dienstverwaltungs-REST-API
Gehen Sie auf einem REST-Client wie folgt vor:

1. Stellen Sie sicher, dass Sie über ein Clientzertifikat für die Verbindung zum Azure-Portal verfügen. Um ein Clientzertifikat zu erhalten, befolgen Sie die Schritte in [How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Legen Sie einen Headereintrag mit dem Namen "x-ms-version" mit einem Wert von "2013-11-01" fest.
3. Senden Sie eine Anforderung im folgenden Format:\//management.core.windows.net/\<Abonnement-ID\>/services/hostedservices/\<Dienstname\>?embed-detail=true
4. Suchen Sie das **HostName**-Element für jedes **RoleInstance**-Element.

> [!WARNING]
> Sie können auch das interne Domänensuffix für Ihren Clouddienst über die Antwort auf den REST-Aufruf anzeigen, indem Sie das **InternalDnsSuffix**-Element überprüfen. Alternativ können Sie „ipconfig /all“ an einer Eingabeaufforderung in einer Remotedesktopsitzung (Windows) oder „cat /etc/resolv.conf“ in einem SSH-Terminal ausführen (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Ändern eines Hostnamens
Sie können den Hostnamen für einen virtuellen Computer oder eine Rolleninstanz ändern, indem Sie eine geänderte Dienstkonfigurationsdatei hochladen oder den Computer in einer Remotedesktopsitzung umbenennen.

## <a name="next-steps"></a>Nächste Schritte
[Namensauflösung (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure-Dienstkonfigurationsschema (CSCFG)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Konfigurationsschema für virtuelle Azure-Netzwerke](https://go.microsoft.com/fwlink/?LinkId=248093)

[Angeben von DNS-Einstellungen mit Netzwerkkonfigurationsdateien](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

