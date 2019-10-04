---
title: Energieoptionen der seriellen Azure-Konsole | Microsoft-Dokumentation
description: Verfügbare Energieoptionen für virtuelle Computer in der seriellen Azure-Konsole
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/6/2019
ms.author: alsin
ms.openlocfilehash: f17b96ad880742cf1232b074e4398f3b1d15e5ba
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129442"
---
# <a name="power-options-available-from-the-azure-serial-console"></a>Verfügbare Energieoptionen in der seriellen Azure-Konsole

Die serielle Azure-Konsole umfasst verschiedene leistungsstarke Tools für die Energieverwaltung des virtuellen Computers oder der VM-Skalierungsgruppe. Diese Energieverwaltungsoptionen können für einige Benutzer verwirrend sein. Daher bietet dieses Dokument eine Übersicht über die einzelnen Tools und den jeweiligen vorgesehenen Anwendungsfall.

Funktion in der seriellen Konsole | BESCHREIBUNG | Anwendungsfall
:----------------------|:------------|:---------
VM neu starten | Ordnungsgemäßer Neustart des virtuellen Computers oder der VM-Skalierungsgruppeninstanz. Dieser Vorgang entspricht dem Aufrufen der auf der Übersichtsseite verfügbaren Neustartfunktion. | In den meisten Fällen sollten Sie diese Option als erstes Tool zum Neustarten des virtuellen Computers verwenden. Die Verbindung der seriellen Konsole wird kurz unterbrochen und nach dem Neustart des virtuellen Computers automatisch wiederhergestellt.
Zurücksetzen eines virtuellen Computers | Erzwungenes Aus- und Einschalten des virtuellen Computers oder der VM-Skalierungsgruppe über die Azure-Plattform. | Diese Option wird verwendet, um das Betriebssystem unabhängig vom aktuellen Status sofort neu zu starten. Da es sich bei diesem Vorgang nicht um einen ordnungsgemäßen Neustart handelt, besteht das Risiko, dass Daten verloren gehen oder beschädigt werden. Die Verbindung der seriellen Konsole wird nicht unterbrochen. Dies kann nützlich sein, um Befehle zu einem frühen Zeitpunkt im Startvorgang zu senden (z. B. für den GRUB-Zugriff auf einem virtuellen Linux-Computer oder den abgesicherten Modus auf einem virtuellen Windows-Computer).
SysRq - Reboot (b) | Systemanforderung zum Erzwingen des Neustarts eines Gasts. | Diese Funktion gilt nur für Linux-Betriebssysteme und erfordert, dass im Betriebssystem [SysRq aktiviert ist](./serial-console-nmi-sysrq.md#system-request-sysrq). Wenn das Betriebssystem ordnungsgemäß für SysRq konfiguriert ist, wird über diesen Befehl das Betriebssystem neu gestartet.
NMI (Nicht maskierbarer Interrupt) | Interruptbefehl, der an das Betriebssystem übermittelt wird. | Dieser Vorgang ist für virtuelle [Windows](./serial-console-windows.md#use-the-serial-console-for-nmi-calls)- und [Linux](./serial-console-nmi-sysrq.md#non-maskable-interrupt-nmi)-Computer verfügbar und erfordert, dass NMI aktiviert ist. Beim Senden eines NMI-Befehls stürzt das Betriebssystem in der Regel ab. Sie können das Betriebssystem so konfigurieren, dass eine Sicherungsdatei erstellt und dann das Betriebssystem nach dem Empfang des NMI-Befehls neu gestartet wird. Dies kann beim Low-Level-Debuggen nützlich sein.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur [seriellen Azure-Konsole für virtuelle Linux-Computer](./serial-console-linux.md)
* Weitere Informationen zur [seriellen Azure-Konsole für virtuelle Windows-Computer](./serial-console-windows.md)