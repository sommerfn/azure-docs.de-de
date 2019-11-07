---
title: Verwenden des Hunting-Livestreams in Azure Sentinel zum Erkennen von Bedrohungen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie der Hunting-Livestream in Azure Sentinel verwendet werden kann, um den Überblick über Daten zu behalten.
services: sentinel
documentationcenter: na
author: cabailey
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: cabailey
ms.openlocfilehash: d1d048507946850192c1c6f8ca9024c6f9ddf8ca
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510987"
---
# <a name="use-hunting-livestream-in-azure-sentinel-to-detect-threats"></a>Verwenden des Hunting-Livestreams in Azure Sentinel zum Erkennen von Bedrohungen

> [!IMPORTANT]
> Der Hunting-Livestream in Azure Sentinel ist derzeit als öffentliche Vorschau verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Verwenden Sie den Hunting-Livestream, um interaktive Sitzungen zu erstellen, mit denen Sie neu erstellte Abfragen beim Eintreten von Ereignissen testen, Benachrichtigungen von den Sitzungen bei einer Übereinstimmung erhalten und bei Bedarf Untersuchungen starten können. Sie können schnell eine Livestreamsitzung erstellen, indem Sie eine beliebige Log Analytics-Abfrage verwenden.

- **Testen neu erstellter Abfragen, wenn Ereignisse auftreten**
    
    Sie können Abfragen ohne Konflikte mit aktuellen Regeln testen und anpassen, die aktiv auf Ereignisse angewendet werden. Nachdem Sie bestätigt haben, dass diese neuen Abfragen erwartungsgemäß funktionieren, ist es einfach, sie zu benutzerdefinierten Warnungsregeln höher zu stufen, indem Sie eine Option auswählen, die die Sitzung zu einer Warnung erhöht.

- **Benachrichtigung beim Auftreten von Bedrohungen erhalten**
    
    Sie können Bedrohungsdatenfeeds mit aggregierten Protokolldaten vergleichen und eine Benachrichtigung erhalten, wenn eine Entsprechung auftritt. Bedrohungsdatenfeeds sind fortlaufende Datenströme, die sich auf potenzielle oder aktuelle Bedrohungen beziehen, sodass die Benachrichtigung möglicherweise auf eine potenzielle Bedrohung für Ihre Organisation hinweist. Erstellen Sie eine Livestreamsitzung anstelle einer benutzerdefinierten Warnungsregel, wenn Sie über ein mögliches Problem benachrichtigt werden möchten, ohne dass Mehraufwand für die Verwaltung einer benutzerdefinierten Warnungsregel anfällt.

- **Starten von Untersuchungen**
    
    Wenn eine aktive Untersuchung durchgeführt wird, die eine Ressource wie einen Host oder einen Benutzer beinhaltet, können Sie bestimmte (oder beliebige) Aktivitäten in den Protokolldaten anzeigen, während diese Aktivitäten für die Ressource auftreten. Sie können eine Benachrichtigung erhalten, wenn diese Aktivität auftritt.


## <a name="create-a-livestream-session"></a>Erstellen einer Livestreamsitzung

Sie können eine Livestreamsitzung aus einer vorhandenen Hunting-Abfrage oder eine Sitzung von Grund auf neu erstellen.

1. Navigieren Sie im Azure-Portal zu **Sentinel** > **Bedrohungsverwaltung** > **Hunting**.

2. So erstellen Sie eine Livestreamsitzung aus einer Hunting-Abfrage:
    
    1. Ermitteln Sie auf der Registerkarte **Abfragen** die zu verwendende Hunting-Abfrage.
    2. Klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **Zu Livestream hinzufügen** aus. Beispiel:
    
    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Livestreamsitzung aus der Hunting-Abfrage von Azure Sentinel](./media/livestream/livestream-from-query.png)

3. So erstellen Sie eine Livestreamsitzung von Grund auf neu: 
    
    1. Wählen Sie die Registerkarte **Livestream** aus.
    2. Wählen Sie **Zu Livestream wechseln** aus.
    
4. Gehen Sie auf dem Blatt **Livestream** folgendermaßen vor:
    
    - Wenn Sie Livestream aus einer Abfrage gestartet haben, überprüfen Sie die Abfrage, und nehmen Sie die gewünschten Änderungen vor.
    - Wenn Sie Livestream von Grund auf neu gestartet haben, erstellen Sie Ihre Abfrage. 

5. Wählen Sie in der Befehlsleiste **Wiedergeben** aus.
    
    Die Statusleiste unter der Befehlsleiste gibt an, ob der Livestream ausgeführt wird oder angehalten ist. Im folgenden Beispiel wird die Sitzung ausgeführt:
    
    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Livestreamsitzung aus der Hunting-Abfrage von Azure Sentinel](./media/livestream/livestream-session.png)

6. Wählen Sie in der Befehlsleiste **Speichern** aus.
    
    Wenn Sie nicht **Anhalten** auswählen, wird die Sitzung fortgesetzt, bis Sie sich vom Azure-Portal abgemeldet haben.

## <a name="view-your-livestream-sessions"></a>Anzeigen von Livestreamsitzungen

1. Navigieren Sie im Azure-Portal zur Registerkarte **Sentinel** > **Bedrohungsverwaltung** > **Hunting** > **Livestream**.

2. Wählen Sie die Livestreamsitzung aus, die Sie anzeigen oder bearbeiten möchten. Beispiel:
    
    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Livestreamsitzung aus der Hunting-Abfrage von Azure Sentinel](./media/livestream/livestream-tab.png)
    
    Ihre ausgewählte Livestreamsitzung wird geöffnet, damit Sie sie wiedergeben, anhalten, bearbeiten usw. können.

## <a name="receive-notifications-when-new-events-occur"></a>Empfangen von Benachrichtigungen, wenn neue Ereignisse auftreten

Da Livestreambenachrichtigungen für neue Ereignisse Benachrichtigungen des Azure-Portals verwenden, werden diese Benachrichtigungen immer dann angezeigt, wenn Sie das Azure-Portal verwenden. Beispiel:

![Benachrichtigung des Azure-Portals für Livestream](./media/livestream/notification.png)

Wählen Sie die Benachrichtigung aus, um das Blatt **Livestream** zu öffnen.
 
## <a name="elevate-a-livestream-session-to-an-alert"></a>Erhöhen einer Livestreamsitzung zu einer Warnung

Sie können eine Livestreamsitzung in eine neue Warnung höher stufen, indem Sie in der Befehlsleiste der relevanten Livestreamsitzung **Auf Warnung höherstufen** auswählen:

> [!div class="mx-imgBorder"]
> ![Erhöhen einer Livestreamsitzung zu einer Warnung](./media/livestream/elevate-to-alert.png)

Mit dieser Aktion wird der Regelerstellungs-Assistent geöffnet, der mit der Abfrage, die der Livestreamsitzung zugeordnet ist, bereits aufgefüllt ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie der Hunting-Livestream in Azure Sentinel verwendet wird. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:


- [Proactively hunt for threats (Proaktive Ermittlung von Bedrohungen)](hunting.md)
- [Verwenden von Notebooks zur Ermittlung von Anomalien](notebooks.md)
