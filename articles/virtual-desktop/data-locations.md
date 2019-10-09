---
title: Datenstandorte für Windows Virtual Desktop – Azure
description: Eine kurze Übersicht der Standorte, an denen die Daten und Metadaten von Windows Virtual Desktop gespeichert werden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: helohr
ms.openlocfilehash: 5a634f3449d88e2c1885f4a32ae2662c93811c63
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349942"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Datenstandorte für Windows Virtual Desktop

Windows Virtual Desktop ist zurzeit für alle geografischen Standorte verfügbar. Anfänglich können Dienstmetadaten nur in den Regionen in den USA gespeichert werden. Administratoren können den Standort zum Speichern von Benutzerdaten auswählen, wenn sie die virtuellen Computer des Hostpools und die zugehörigen Dienste wie Dateiserver erstellen. Weitere Informationen zu Azure-Regionen erhalten Sie in der [Karte der Azure-Rechenzentren](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft kontrolliert oder beschränkt nicht die Regionen, in denen Sie oder Ihre Benutzer auf Ihre benutzer- und App-spezifischen Daten zugreifen können.

>[!IMPORTANT]
>Windows Virtual Desktop speichert globale Metadateninformationen wie Mandantennamen, Hostpoolnamen, App-Gruppennamen und Benutzerprinzipalnamen in einem Rechenzentrum in den USA. Die gespeicherten Metadaten werden im Ruhezustand verschlüsselt, und georedundante Spiegel werden innerhalb der USA verwaltet. Alle Kundendaten wie App-Einstellungen und Benutzerdaten befinden sich an dem vom Kunden ausgewählten Standort und werden nicht vom Dienst verwaltet.

Dienstmetadaten werden für die Notfallwiederherstellung in die USA repliziert.