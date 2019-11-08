---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c2ce39c40ad535b4a66349fcbb4529333218555a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510231"
---
Bei SSD Premium wird das Bursting nur für Datenträgergrößen bis einschließlich 512 GiB (P20 oder niedriger) unterstützt. Diese Datenträgergrößen unterstützen das Bursting nach dem Prinzip des bestmöglichen Aufwands und nutzen ein Guthabensystem zum Verwalten von Bursts. Das Guthaben wird in einem Burstbucket gesammelt, wenn der Datenverkehr unter dem bereitgestellten Leistungsziel für die Datenträgergröße liegt, und verbraucht, wenn der Datenverkehr das Ziel übersteigt. Der Datenverkehr des Datenträgers wird auf IOPS und Bandbreite im bereitgestellten Ziel nachverfolgt.

Das Datenträgerbursting ist bei neuen Bereitstellungen der unterstützten Datenträgergrößen standardmäßig aktiviert. Bei vorhandenen Datenträgergrößen, die das Datenträgerbursting unterstützen, kann die Funktion durch eine der folgenden Methoden aktiviert werden:

- Trennen Sie den Datenträger, und fügen Sie ihn erneut an.
- Beenden Sie die VM, und starten Sie sie neu.

## <a name="burst-states"></a>Burstzustände

Alle für das Bursting infrage kommenden Datenträgergrößen werden mit einem vollen Burst-Guthaben-Bucket gestartet, wenn der Datenträger an einen virtuellen Computer abgeschlossen wird. Die maximale Dauer des Burstings wird durch die Größe des Burst-Guthaben-Buckets bestimmt. Sie können nicht verwendetes Guthaben nur bis zur Größe des Guthabenbuckets sammeln. Der Burst-Guthaben-Bucket Ihres Datenträgers weist zu jedem beliebigen Zeitpunkt einen der folgenden drei Zustände auf: 

- Anwachsend, wenn der Datenverkehr des Datenträgers weniger als das bereitgestellte Leistungsziel nutzt. Sie können Guthaben sammeln, wenn der Datenverkehr des Datenträgers unter den IOPS- oder Bandbreitenziele liegt. Sie können auch weiterhin E/A-Guthaben sammeln, wenn Sie die vollständige Datenträger-Bandbreite verbrauchen.  

- Abnehmend, wenn der Datenverkehr des Datenträgers mehr als das bereitgestellte Leistungsziel nutzt. Der Burstdatenverkehr beansprucht das Guthaben unabhängig von IOPS und Bandbreite. 

- Konstant, wenn der Datenverkehr des Datenträgers genau das bereitgestellte Leistungsziel nutzt. 

In der folgenden Tabelle werden die Datenträgergrößen zusammengefasst, die Bursting unterstützen. Außerdem werden die Spezifikationen für das Bursting angegeben.

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Derzeit ist Bursting nur in der Region „USA, Westen-Mitte“ verfügbar.

## <a name="disk-sizes"></a>Datenträgergrößen

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="example-scenarios"></a>Beispielszenarien

Im Folgenden finden Sie einige Beispielszenarien zum besseren Verständnis:

- Ein häufiges Szenario, bei dem das Datenträgerbursting Vorteile bietet, sind schnellere Starts von VMs und Anwendungen auf Betriebssystemdatenträgern. Gehen Sie als Beispiel von einer Linux-VM mit einem Betriebssystemimage mit 8 GiB aus. Wenn Sie einen P2-Datenträger als Betriebssystemdatenträger verwenden, ist das bereitgestellte Ziel 120 IOPS und 25 MB/s. Beim Starten des virtuellen Computers tritt aufgrund des Ladens der Startdateien eine Lesespitze auf dem Betriebssystemdatenträger auf. Durch die Verwendung von Bursting können Sie mit der maximalen Burstgeschwindigkeit von 3.500 IOPS und 170 MB/s lesen und so die Ladezeit um mindestens das 6-Fache beschleunigen. Nach dem Start des virtuellen Computers ist die Datenverkehrsrate auf dem Betriebssystemdatenträger normalerweise niedrig, da die meisten Datenvorgänge der Anwendung auf den angefügten Datenträgern erfolgen. Wenn der Datenverkehr unter das bereitgestellte Ziel fällt, sammeln Sie Guthaben an.

- Wenn Sie eine virtuelle Remote Virtual Desktop-Umgebung hosten, erhöht sich bei jedem Start einer Anwendung wie AutoCAD durch einen aktiven Benutzer der Datenverkehr auf dem Betriebssystemdatenträger erheblich. In diesem Fall beansprucht der Burstdatenverkehr das angesammelte Guthaben, sodass Sie das bereitgestellte Ziel überschreiten und die Anwendung wesentlich schneller starten können.

- Ein P1-Datenträger verfügt über ein bereitgestelltes Ziel von 120 IOPS und 25 MB/s. Wenn der tatsächliche Datenverkehr auf dem Datenträger im letzten 1-Sekunden-Intervall 100 IOPS und 20 MB/s war, werden die nicht genutzten 20 IOPS und 5 MB im Burstbucket des Datenträgers gutgeschrieben. Guthaben im Burstbucket kann später verwendet werden, wenn der Datenverkehr das bereitgestellte Ziel überschreitet (bis zum maximalen Burstlimit). Das maximale Burstlimit definiert die Obergrenze für Datenverkehr auf dem Datenträger (auch wenn Sie über Burstguthaben verfügen, das verbraucht werden kann). In diesem Fall kann ein P1-Datenträger selbst dann, wenn er über 10.000 IOPS im Guthabenbucket verfügt, nicht mehr als den maximalen Burstwert von 3.500 IOPS nutzen.  
