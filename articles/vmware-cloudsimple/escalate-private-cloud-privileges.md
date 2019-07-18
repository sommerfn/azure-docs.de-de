---
title: Ausweiten von Berechtigungen für die private Cloud – Azure-VMware-Lösung von CloudSimple
description: Beschreibt, wie Sie Berechtigungen in Ihrer privaten Cloud für administrative Funktionen in vCenter ausweiten
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d11c88b91b13cca13120a9203e376fdc2c3d6d8d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332432"
---
# <a name="escalate-private-cloud-vcenter-privileges-from-the-cloudsimple-portal"></a>Ausweiten von Berechtigungen für das vCenter Ihrer privaten Cloud über das CloudSimple-Portal 

Sie können Ihre CloudSimple-Berechtigungen temporär für den administrativen Zugriff auf das vCenter Ihrer privaten Cloud ausweiten.  Mithilfe von erhöhten Rechten können Sie VMware-Lösungen installieren, Identitätsquellen hinzufügen und Benutzer verwalten.

Neue Benutzer können Sie in der vCenter-SSO-Domäne erstellen und diesen Zugriff auf vCenter gewähren.  Wenn Sie neue Benutzer erstellen, fügen Sie diese für den Zugriff auf vCenter zu den in CloudSimple integrierten Gruppen hinzu.  Weitere Informationen finden Sie unter [CloudSimple Private Cloud permission model of VMware vCenter (Berechtigungsmodell für das VMware-vCenter der privaten Cloud von CloudSimple)](https://docs.azure.cloudsimple.com/learn-private-cloud-permissions/).

> [!CAUTION]
> Ändern Sie die Konfigurationen für Verwaltungskomponenten nicht. Aktionen, die während des ausgeweiteten Berechtigungszustands durchgeführt werden, können sich negativ auf Ihr System auswirken oder dazu führen, dass Ihr System nicht mehr verfügbar ist.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="escalate-privileges"></a>Eskalieren von Berechtigungen

1. Greifen Sie auf das [CloudSimple-Portal](access-cloudsimple-portal.md) zu.

2. Öffnen Sie die Seite **Resources** (Ressourcen), und wählen Sie die private Cloud aus, für die Sie die Berechtigungen ausweiten möchten.

3. Klicken Sie am unteren Rand der Seite „Summary“ (Zusammenfassung) unter **Change vSphere privileges** (vSphere-Berechtigungen ändern) auf **Escalate** (Ausweiten).

    ![Ändern der vSphere-Berechtigung](media/escalate-private-cloud-privilege.png)

4. Wählen Sie den vSphere-Benutzertyp aus.  Nur der lokale Benutzer **CloudOwner@cloudsimple.local** kann ausgeweitet werden.

5. Wählen Sie das Zeitintervall zum Ausweiten aus der Dropdownliste aus. Wählen Sie den kürzesten Zeitraum aus, in dem Sie die Aufgabe erledigen können.

6. Aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie die Risiken kennen.

    ![Dialogfeld zum Ausweiten von Berechtigungen](media/escalate-private-cloud-privilege-dialog.png)

7. Klicken Sie auf **OK**.

8. Der Ausweitungsvorgang kann einige Minuten dauern. Klicken Sie zum Abschluss auf **OK**.

Die Rechteausweitung beginnt und dauert bis zum Ende des ausgewählten Intervalls.  Sie können sich beim vCenter Ihrer privaten Cloud anmelden, um administrative Aufgaben zu erledigen.

> [!IMPORTANT]
> Nur ein Benutzer kann über ausgeweitete Berechtigungen verfügen.  Sie müssen die Berechtigungen des Benutzers wieder einschränken, bevor Sie die Berechtigungen eines anderen Benutzers ausweiten können.

## <a name="extend-privilege-escalation"></a>Erweitern der Rechteausweitung

Wenn Sie zusätzliche Zeit zum Erledigen Ihrer Aufgaben benötigen, können Sie den Zeitraum der Rechteausweitung erweitern.  Wählen Sie das zusätzliche Zeitintervall für die Ausweitung aus, das Sie für die Erledigung der administrativen Aufgaben benötigen.

1. Wählen Sie im CloudSimple-Portal unter **Resources** > **Private Clouds** (Ressourcen > Private Cloud) die private Cloud aus, für die Sie die Rechteausweitung erweitern möchten.

2. Klicken Sie am unteren Rand der Registerkarte „Summary“ (Zusammenfassung) auf **Extend privilege escalation** (Rechteausweitung erweitern).

    ![Erweitern der Rechteausweitung](media/de-escalate-private-cloud-privilege.png)

3. Wählen Sie ein Zeitintervall zum Ausweiten aus der Dropdownliste aus. Überprüfen Sie die neue Endzeit der Ausweitung.

4. Klicken Sie auf **Save** (Speichern), um das Intervall zu erweitern.

## <a name="de-escalate-privileges"></a>Einschränken von Berechtigungen

Nachdem Sie Ihre administrativen Aufgaben abgeschlossen haben, sollten Sie Ihre Berechtigungen wieder einschränken.  

1. Wählen Sie im CloudSimple-Portal unter **Resources** > **Private Clouds** (Ressourcen > Private Cloud) die private Cloud aus, für die Sie die Berechtigungen wieder einschränken möchten.

2. Klicken Sie auf **De-escalate** (Einschränken).

3. Klicken Sie auf **OK**.

> [!IMPORTANT]
> Melden Sie sich im vCenter ab und nach dem Einschränken der Berechtigungen wieder an, um Fehler zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

* [Set up vCenter identity sources to use Active Directory (Einrichten von vCenter-Identitätsquellen für die Verwendung von Active Directory)](https://docs.azure.cloudsimple.com/set-vcenter-identity/)
* Installieren einer Sicherungslösung zum [Sichern von Workload-VMs](https://docs.azure.cloudsimple.com/backup-workloads-veeam/)