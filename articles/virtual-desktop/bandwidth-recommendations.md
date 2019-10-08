---
title: 'Bandbreitenempfehlungen für Remotesitzungen: Azure'
description: Verfügbare Bandbreitenempfehlungen für Remotesitzungen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: helohr
ms.openlocfilehash: 8352858b9fd6059257b5ba7637822f85c56eb070
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802616"
---
# <a name="bandwidth-recommendations-for-remote-sessions"></a>Bandbreitenempfehlungen für Remotesitzungen

Wenn Sie eine Windows-Remotesitzung verwenden, wirkt sich die verfügbare Bandbreite Ihres Netzwerks erheblich auf die Qualität ihrer Benutzererfahrung aus. Verschiedene Anwendungen und Anzeigeauflösungen erfordern unterschiedliche Netzwerkkonfigurationen, daher ist es wichtig, sicherzustellen, dass Ihr Netzwerk entsprechend Ihren Anforderungen konfiguriert ist.

>[!NOTE]
>Die folgenden Empfehlungen gelten für Netzwerke mit einem Verlust von weniger als 0,1 %.

## <a name="applications"></a>ANWENDUNGEN

Die folgende Tabelle listet die empfohlenen Mindestbandbreiten für eine störungsfreie Benutzererfahrung auf. 

|Workload        |Beispielanwendungen                                                                                           |Empfohlene Bandbreite|
|----------------|--------------------------------------------------------------------------------------------------------------|---------------------|
|Task Worker     |Microsoft Word, Outlook, Excel, Adobe Reader                                                                  |1,5&nbsp;MBit/s        |
|Office Worker   |Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint, Photo Viewer                                        |3&nbsp;MBit/s          |
|Knowledge Worker|Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint, Photo Viewer, Java                                  |5&nbsp;MBit/s          |
|Power Worker    |Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint, Photo Viewer, Java, CAD/CAM, Illustration/Veröffentlichung|15&nbsp;MBit/s         |

>[!NOTE]
>Diese Empfehlungen gelten unabhängig davon, wie viele Benutzer an der Sitzung teilnehmen.

Beachten Sie, dass die Belastung Ihres Netzwerks sowohl von der Ausgabeframerate Ihrer App-Workloads als auch von Ihrer Bildschirmauflösung abhängt. Wenn die Framerate oder die Bildschirmauflösung zunimmt, steigt auch die Bandbreitenanforderung. Beispielsweise erfordert eine einfache Workload mit einer hochauflösenden Anzeige mehr verfügbare Bandbreite als eine einfache Workload mit normaler oder niedriger Auflösung.

In anderen Szenarien können sich die Bandbreitenanforderungen abhängig von ihrer Verwendung ändern. Beispiele:

- Sprach- oder Videokonferenzen
- Echtzeitkommunikation
- Streaming von 4K-Videoinhalten

Stellen Sie sicher, dass Sie diese Szenarien in Ihrer Bereitstellung mit Simulationstools wie Login VSI Lasttests unterziehen. Verändern Sie die Lastgröße, führen Sie Stresstests durch, und testen Sie häufige Benutzerszenarien in Remotesitzungen, um die Anforderungen Ihres Netzwerks besser zu verstehen. 

## <a name="display-resolutions"></a>Bildschirmauflösungen

Unterschiedliche Bildschirmauflösungen erfordern unterschiedliche verfügbare Bandbreiten. Die folgende Tabelle listet die Bandbreiten auf, die für eine reibungslose Benutzererfahrung bei typischen Bildschirmauflösungen mit einer Bildfrequenz von 30 Frames pro Sekunde (FPS) empfohlen werden. Diese Empfehlungen gelten für Szenarien mit einem Benutzer oder mehreren Benutzern. Beachten Sie, dass Szenarien mit einer Bildfrequenz unter 30 FPS, wie das Lesen von statischem Text, eine geringere verfügbare Bandbreite erfordern. 

|Typische Bildschirmauflösungen bei 30 FPS    |Empfohlene Bandbreite|
|-----------------------------------------|---------------------|
|Etwa 1024 × 768 px                      |1,5 MBit/s             |
|Etwa 1280 × 720 px                      |3 MBit/s               |
|Etwa 1920 × 1080 px                     |5 MBit/s               |
|Etwa 3840 × 2160 px (4K)                |15 MBit/s              |

>[!NOTE]
>Diese Empfehlungen gelten unabhängig davon, wie viele Benutzer an der Sitzung teilnehmen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Die Azure-Region, in der Sie sich befinden, kann die Benutzererfahrung ebenso beeinflussen wie die Netzwerkbedingungen. Weitere Informationen finden Sie in der [Einschätzung der Servicequalität für Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/).
