---
title: Überwachen von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Informationen zur Überwachung von SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0aea4dddef65600fe30f36499d4ad2a4f461245
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70077934"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Überwachen von SAP HANA in Azure (große Instanzen)

SAP HANA in Azure (große Instanzen) unterscheidet sich nicht von anderen IaaS-Bereitstellungen, d.h., Sie müssen die Aktivitäten von Betriebssystem und Anwendungen und deren Belegung der folgenden Ressourcenklassen überwachen:

- CPU
- Arbeitsspeicher
- Netzwerkbandbreite
- Speicherplatz

Bei Azure Virtual Machines müssen Sie ermitteln, ob die genannten Ressourcenklassen ausreichend Kapazität bieten oder irgendwann erschöpft werden. Nachstehend werden die verschiedenen Klassen detailliert beschrieben:

**CPU-Ressourcenauslastung:** Das Verhältnis, das SAP für eine bestimmte Workload für HANA definiert hat, wird erzwungen, um sicherstellen, dass genügend CPU-Ressourcen verfügbar sind, um die im Arbeitsspeicher gespeicherten Daten zu verarbeiten. Trotzdem kann es Fälle geben, in denen HANA bei der Ausführung von Abfragen aufgrund fehlender Indizes oder ähnlicher Probleme viele CPU-Ressourcen belegt. Dies bedeutet, dass Sie die CPU-Ressourcenauslastung der HANA (große Instanzen)-Einheit sowie CPU-Ressourcen überwachen sollten, die von den verschiedenen HANA-Diensten genutzt werden.

**Arbeitsspeicherauslastung:** Hierfür muss eine Überwachung sowohl innerhalb als auch außerhalb von HANA erfolgen. Überwachen Sie in HANA, wie die Daten für HANA reservierten Arbeitsspeicher nutzen, um die von SAP angeforderten Größenrichtlinien zu erfüllen. Sie sollten außerdem die Arbeitsspeichernutzung auf Ebene der „großen Instanz“ überwachen, um sicherzustellen, dass installierte Nicht-HANA-Software nicht zu viel Arbeitsspeicher belegt und aufgrund dessen mit HANA um Arbeitsspeicher konkurriert.

**Netzwerkbandbreite:** Die Bandbreite des Azure VNET-Gateways ist hinsichtlich der Bandbreite für bei dem Azure VNET eingehende Daten begrenzt. Deshalb ist es hilfreich, die von allen virtuellen Azure-Computern in einem VNET empfangenen Daten zu überwachen, um zu bestimmen, wie nahe Sie an die Grenzwerte der ausgewählten Azure-Gateway-SKU herankommen. Auf der HANA (große Instanz)-Einheit ist es sinnvoll, auch den ein- und ausgehenden Netzwerkdatenverkehr zu überwachen und die Volumes nachzuverfolgen, die mit der Zeit verarbeitet werden.

**Speicherplatz:** Die Speicherplatzbelegung steigt in der Regel mit der Zeit. Die wichtigsten Gründe dafür sind: Zunahme der Datenmenge, Erstellung von Sicherungen von Transaktionsprotokollen, Speicherung von Ablaufverfolgungsdateien und Erstellung von Speichermomentaufnahmen. Aus diesem Grund ist es wichtig, die Speicherplatzbelegung zu überwachen und den Speicherplatz zu verwalten, der der HANA (große Instanz)-Einheit zugeordnet ist.

Bei **SKUs von Typ II** großer HANA-Instanzen sind bereits Systemdiagnosetools auf dem Server geladen. Mit diesen Diagnosetools können Sie die Systemintegritätsprüfung durchführen. Führen Sie den folgenden Befehl aus, um die Systemdiagnose-Protokolldatei unter „/var/log/health_check“ zu generieren.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Wenn Sie zur Behandlung eines Problems das Microsoft-Supportteam zurate ziehen, werden Sie eventuell auch dazu aufgefordert, die Protokolldateien mithilfe dieser Diagnosetools zu übermitteln. Sie können die Datei mithilfe des folgenden Befehls komprimieren:
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Nächste Schritte**

- Lesen Sie [Überwachen von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot).
