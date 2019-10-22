---
title: Erstellen eines Zeitplans für Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Zeitpläne für Classroom-Labs in Azure Lab Services erstellen, sodass virtuelle Computer in den Labs zu einem bestimmten Zeitpunkt gestartet und heruntergefahren werden.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 4e3cf302437c3e4954ac977ac3f4ff6b2021a760
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330520"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Erstellen und Verwalten von Zeitplänen für Classroom-Labs in Azure Lab Services 
Mithilfe von Zeitplänen können Sie ein Classroom-Lab so konfigurieren, dass VMs im Lab automatisch zu einem bestimmten Zeitpunkt gestartet und heruntergefahren werden. Sie können einen einmaligen Zeitplan oder einen sich wiederholenden Zeitplan definieren. Die folgenden Verfahren zeigen die Schritte zum Erstellen und Verwalten von Zeitplänen für ein Classroom-Lab: 

> [!IMPORTANT]
> Die geplante Ausführungszeit von virtuellen Computern wird nicht mit dem [Kontingent eines Benutzers](how-to-configure-student-usage.md#set-quotas-for-users) verrechnet. Das Kontingent dient für die Zeiten außerhalb der Stunden im Zeitplan, die ein Kursteilnehmer mit VMs verbringt. 

## <a name="set-a-schedule-for-the-lab"></a>Festlegen eines Zeitplans für das Lab
Erstellen Sie ein geplantes Ereignis für das Lab, damit VMs im Lab zu bestimmten Zeiten automatisch gestartet und beendet werden. Das von Ihnen zuvor angegebene Benutzerkontingent ist die zusätzliche Zeit, die jedem Benutzer außerhalb dieser geplanten Zeit zugewiesen ist. 

1. Wechseln Sie zur Seite **Zeitpläne**, und wählen Sie in der Symbolleiste die Option **Add scheduled event** (Geplantes Ereignis hinzufügen) aus. 

    ![Schaltfläche „Zeitplan hinzufügen“ auf der Seite „Zeitpläne“](../media/how-to-create-schedules/add-schedule-button.png)
2. Vergewissern Sie sich, dass unter **Ereignistyp** die Option **Standard** ausgewählt ist. Wählen Sie **Start only** (Nur starten) aus, um nur die Startzeit für die VMs anzugeben. Wählen Sie **Stop only** (Nur beenden) aus, um nur die Endzeit für die VMs anzugeben. 
7. Wählen Sie im Abschnitt **Wiederholen** den aktuellen Zeitplan aus. 

    ![Schaltfläche „Zeitplan hinzufügen“ auf der Seite „Zeitpläne“](../media/how-to-create-schedules/select-current-schedule.png)
5. Führen Sie im Dialogfeld **Wiederholen** die folgenden Schritte aus:
    1. Vergewissern Sie sich, dass für das Feld **Wiederholen** die Option **Wöchentlich** festgelegt ist. 
    3. Geben Sie das **Startdatum** an.
    4. Geben Sie die **Startzeit** an, zu der die virtuellen Computer gestartet werden sollen.
    5. Geben Sie die **Endzeit** an, zu der die virtuellen Computer heruntergefahren werden sollen. 
    6. Geben Sie die **Zeitzone** für die von Ihnen angegebene Start- und Endzeit an. 
    2. Wählen Sie die Tage aus, an denen der Zeitplan gelten soll. Im folgenden Beispiel ist „Montag bis Donnerstag“ ausgewählt. 
    8. Wählen Sie **Speichern** aus. 

        ![Festlegen des Zeitplans für die Wiederholung](../media/how-to-create-schedules/set-repeat-schedule.png)

3. Geben Sie nun auf der Seite **Add scheduled event** (Geplantes Ereignis hinzufügen) unter **Hinweise (optional)** ein beliebige Beschreibung bzw. Hinweise zum Zeitplan ein. 
4. Wählen Sie auf der Seite **Add scheduled event** (Geplantes Ereignis hinzufügen) die Option **Speichern** aus. 

    ![Wöchentlicher Zeitplan](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Anzeigen von Zeitplänen im Kalender
Die geplanten Datums- und Uhrzeitangaben werden in der Kalenderansicht hervorgehoben, wie in der folgenden Abbildung gezeigt:

![Zeitpläne in der Kalenderansicht](../media/how-to-create-schedules/schedules-calendar.png)

Wählen Sie die Schaltfläche **Heute** in der rechten oberen Ecke aus, um zum aktuellen Datum im Kalender zu wechseln. Wählen Sie den **Pfeil nach links** aus, um zur vorherigen Woche zu wechseln, und den **Pfeil nach rechts**, um zur nächsten Woche im Kalender zu wechseln. 

## <a name="edit-a-schedule"></a>Bearbeiten eines Zeitplans
Wenn Sie einen hervorgehobenen Zeitplan im Kalender auswählen, werden Schaltflächen zum **Bearbeiten** oder **Löschen** des Zeitplans angezeigt. 

![Seite „Zeitplan bearbeiten“](../media/how-to-create-schedules/schedule-edit-button.png)

Auf der Seite **Geplantes Ereignis bearbeiten** können Sie den Zeitplan aktualisieren und **Speichern** auswählen. 

## <a name="delete-a-schedule"></a>Löschen eines Zeitplans

1. Um einen Zeitplan zu löschen, wählen Sie einen hervorgehobenen Zeitplan im Kalender aus, und wählen Sie dann Die Papierkorbsymbol-Schaltfläche (Löschen) aus:

    ![Schaltfläche „Löschen“ auf der Symbolleiste](../media/how-to-create-schedules/schedule-delete-button.png)
2. Wählen Sie im Dialogfeld **Geplantes Ereignis löschen** **Ja** aus, um den Löschvorgang zu bestätigen. 



## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
