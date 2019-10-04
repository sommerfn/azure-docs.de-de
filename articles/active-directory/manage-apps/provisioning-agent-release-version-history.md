---
title: 'Azure AD Connect-Bereitstellungs-Agent: Versionsverlauf | Microsoft-Dokumentation'
description: In diesem Artikel werden alle Versionen des Azure AD Connect-Bereitstellungs-Agent aufgeführt sowie neue Features und behobene Probleme beschrieben.
services: active-directory
documentationcenter: ''
author: cmmdesai
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.subservice: app-mgmt
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: de6ef5dedb0779536cfbc1e6a3440e748ff78524
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861989"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect-Bereitstellungs-Agent: Verlauf der Versionsveröffentlichungen
In diesem Artikel werden die veröffentlichten Versionen und Features des Azure AD Connect-Bereitstellungs-Agent aufgeführt. Das Azure AD-Team aktualisiert den Bereitstellungs-Agent regelmäßig mit neuen Features und Funktionen. Bereitstellungs-Agents werden automatisch aktualisiert, wenn eine neue Version veröffentlicht wird. 

Wir empfehlen die Aktivierung der automatischen Aktualisierung für Ihre Agents, um sicherzustellen, dass Sie über die neuesten Features und Fehlerbehebungen verfügen. Microsoft stellt direkte Unterstützung für die neueste Agent-Version und die unmittelbare Vorgängerversion bereit.

## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Releasestatus

9\. September 2019: Veröffentlichung für automatisches Update

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Möglichkeit zum Konfigurieren zusätzlicher Ablaufverfolgung und Protokollierung für das Debuggen des Bereitstellungs-Agent
* Möglichkeit zum Abrufen nur jener AD-Attribute, die in der Zuordnung konfiguriert sind, um die Synchronisierung zu verbessern

### <a name="fixed-issues"></a>Behobene Probleme

* Ein Fehler wurde behoben, aufgrund dessen der Agent nicht mehr reagierte, wenn Probleme durch AD-Verbindungsfehler aufgetreten sind.
* Ein Fehler wurde behoben, der beim Lesen von Binärdaten aus Active Directory Probleme verursacht hat.
* Ein Fehler wurde behoben, aufgrund dessen Agent die Vertrauensstellung mit dem Cloud-Hybrididentitätsdienst nicht erneuern konnte.

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Releasestatus

23. Januar 2019: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Bereitstellungs-Agent und Connector-Architektur für bessere Leistung, Stabilität und Zuverlässigkeit überarbeitet 
* Konfiguration des Bereitstellungs-Agent durch Verwendung eines Installationsassistenten auf der Benutzeroberfläche vereinfacht 
* Unterstützung für automatische Agent-Updates hinzugefügt

