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
ms.openlocfilehash: 2ce9549765f6a912b3e95f99d11da20347b82ad8
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326461"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect-Bereitstellungs-Agent: Verlauf der Versionsveröffentlichungen
In diesem Artikel werden die veröffentlichten Versionen und Features des Azure Active Directory Connect-Bereitstellungs-Agent aufgeführt. Das Azure AD-Team aktualisiert den Bereitstellungs-Agent regelmäßig mit neuen Features und Funktionen. Der Bereitstellungs-Agent wird automatisch aktualisiert, wenn eine neue Version veröffentlicht wird. 

Wir empfehlen die Aktivierung der automatischen Aktualisierung für Ihre Agents, um sicherzustellen, dass Sie über die neuesten Features und Fehlerbehebungen verfügen. Microsoft stellt direkte Unterstützung für die neueste Agent-Version und die unmittelbare Vorgängerversion bereit.

## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Releasestatus

9\. September 2019: Veröffentlichung für automatisches Update

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Möglichkeit zum Konfigurieren zusätzlicher Ablaufverfolgung und Protokollierung für das Debuggen des Bereitstellungs-Agent
* Möglichkeit zum Abrufen nur jener Azure AD-Attribute, die in der Zuordnung konfiguriert sind, um die Synchronisierungsleistung zu verbessern

### <a name="fixed-issues"></a>Behobene Probleme

* Ein Fehler wurde behoben, aufgrund dessen der Agent nicht mehr reagierte, wenn Probleme durch Azure AD-Verbindungsfehler aufgetreten sind.
* Ein Fehler wurde behoben, der beim Lesen von Binärdaten aus Azure Active Directory Probleme verursacht hat.
* Ein Fehler wurde behoben, aufgrund dessen Agent die Vertrauensstellung mit dem Cloud-Hybrididentitätsdienst nicht erneuern konnte.

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Releasestatus

23. Januar 2019: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Bereitstellungs-Agent und Connector-Architektur wurden für bessere Leistung, Stabilität und Zuverlässigkeit überarbeitet 
* Die Konfiguration des Bereitstellungs-Agent wurde durch Verwendung eines Installationsassistenten auf der Benutzeroberfläche vereinfacht 
* Unterstützung für automatische Agent-Updates hinzugefügt

