---
title: 'Azure VMware-Lösung von CloudSimple: Eskalieren der CloudSimple-Berechtigungen'
description: Beschreibt, wie die CloudSimple-Berechtigungen zum Ausführen von Verwaltungsfunktionen in vCenter der privaten Cloud ausgeweitet werden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 524772578ad724e969bbeab0be0a3edcf32a845f
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69619483"
---
# <a name="escalate-cloudsimple-privileges-to-perform-administrative-functions-in-private-cloud-vcenter"></a>Eskalieren der CloudSimple-Berechtigungen zum Ausführen von Verwaltungsfunktionen in vCenter in der privaten Cloud

Der Berechtigungsansatz von CloudSimple ist so konzipiert, dass vCenter-Benutzer die Berechtigungen erhalten, die sie für die Durchführung normaler Vorgänge benötigen. In einigen Fällen benötigt ein Benutzer möglicherweise zusätzliche Berechtigungen, um eine bestimmte Aufgabe auszuführen.  Sie können die Berechtigungen eines vCenter-SSO-Benutzers für einen begrenzten Zeitraum ausweiten.

Gründe für die Eskalation von Berechtigungen können Folgendes umfassen:

* Konfiguration von Identitätsquellen
* Benutzerverwaltung
* Löschen der verteilten Portgruppe
* Installieren von vCenter-Lösungen (z.B. Sicherungs-Apps)
* Erstellen von Dienstkonten

> [!WARNING]
> Aktionen, die während des ausgeweiteten Berechtigungszustands durchgeführt werden, können sich negativ auf Ihr System auswirken und dazu führen, dass Ihr System nicht mehr verfügbar ist. Führen Sie während des Eskalationszeitraums nur die erforderlichen Aktionen aus.

[Eskalieren Sie im CloudSimple-Portal die Berechtigungen](escalate-private-cloud-privileges.md) für den lokalen Benutzer „CloudOwner“ für das einmalige Anmelden (SSO) von vCenter.  Sie können die Berechtigung des Remotebenutzers nur ausweiten, wenn ein zusätzlicher Identitätsanbieter für vCenter konfiguriert ist.  Die Ausweitung von Berechtigungen umfasst das Hinzufügen des ausgewählten Benutzers zur integrierten Gruppe „Administratoren“ von vSphere.  Nur ein Benutzer kann über ausgeweitete Berechtigungen verfügen.  Wenn Sie die Berechtigungen eines anderen Benutzers eskalieren müssen, schränken Sie zunächst die Berechtigungen der aktuellen Benutzer ein.

Benutzer aus zusätzlichen Identitätsquellen müssen als Mitglieder der Gruppe „CloudOwner“ hinzugefügt werden.

Während des Eskalationszeitraums verwendet CloudSimple die automatisierte Überwachung mit zugehörigen Warnungsbenachrichtigungen, um unbeabsichtigte Änderungen an der Umgebung zu identifizieren.
