---
title: Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center | Microsoft-Dokumentation
description: In diesem Thema werden die in Azure Security Center verfügbaren Warnungen für die Azure-Dienstebene vorgestellt.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 8c1733877834f82d9ee2524cf8bf54f532e7d9c4
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571328"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center

In diesem Thema werden die Security Center-Warnungen vorgestellt, die bei der Überwachung der folgenden Azure-Dienstebenen zur Verfügung stehen:

* [Azure-Netzwerkebene](#network-layer)
* [Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)](#management-layer)

>[!NOTE]
>Die folgenden Analysen basieren auf den Telemetriedaten, die Security Center aus den internen Feeds von Azure bezieht, und gelten für alle Ressourcentypen.

## Azure-Netzwerkebene<a name="network-layer"></a>

Die Netzwerkebenenanalysen von Security Center basieren auf exemplarischen [IPFIX-Daten](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) (durch Azure-Kernrouter erfasste Paketheader). Auf der Grundlage dieses Datenfeeds werden schädliche Datenverkehrsaktivitäten durch Machine Learning-Modelle von Security Center identifiziert und gekennzeichnet. Zur Anreicherung von IP-Adressen nutzt Security Center die Threat Intelligence-Datenbank von Microsoft.

> [!div class="mx-tableFixed"]

|Warnung|Beschreibung|
|---|---|
|**Verdächtige ausgehende RDP-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende RDP-Kommunikation (Remote Desktop Protocol) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt als anormal für diese Umgebung und kann darauf hindeuten, dass Ihre Ressource kompromittiert wurde und nun für einen Brute-Force-Angriff auf den externen RDP-Endpunkt verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende RDP-Netzwerkaktivität für mehrere Ziele**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende RDP-Kommunikation (Remote Desktop Protocol) mit mehreren Zielen erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt als anormal für diese Umgebung und kann darauf hindeuten, dass Ihre Ressource kompromittiert wurde und nun für einen Brute-Force-Angriff auf externe RDP-Endpunkte verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende SSH-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende SSH-Kommunikation (Secure Shell) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt als anormal für diese Umgebung und kann darauf hindeuten, dass Ihre Ressource kompromittiert wurde und nun für einen Brute-Force-Angriff auf den externen SSH-Endpunkt verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende SSH-Netzwerkaktivität für mehrere Ziele**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende SSH-Kommunikation (Secure Shell) mit mehreren Zielen erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt als anormal für diese Umgebung und kann darauf hindeuten, dass Ihre Ressource kompromittiert wurde und nun für einen Brute-Force-Angriff auf externe SSH-Endpunkte verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|
|**Verdächtige eingehende SSH-Netzwerkaktivität mit mehreren Quellen**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende SSH-Kommunikation mehrerer Quellen mit einer Ressource in Ihrer Bereitstellung erkannt. Der Umstand, dass verschiedene individuelle IP-Adressen eine Verbindung mit Ihrer Ressource herstellen, wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle mit mehreren Hosts (Botnet) hindeuten.|
|**Verdächtige eingehende SSH-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende SSH-Kommunikation mit einer Ressource in Ihrer Bereitstellung erkannt. Eine relativ hohe Anzahl eingehender Verbindungen für Ihre Ressource wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle hindeuten.
|**Verdächtige eingehende RDP-Netzwerkaktivität mit mehreren Quellen**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende RDP-Kommunikation mehrerer Quellen mit einer Ressource in Ihrer Bereitstellung erkannt. Der Umstand, dass verschiedene individuelle IP-Adressen eine Verbindung mit Ihrer Ressource herstellen, wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre RDP-Schnittstelle mit mehreren Hosts (Botnet) hindeuten.|
|**Verdächtige eingehende RDP-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende RDP-Kommunikation mit einer Ressource in Ihrer Bereitstellung erkannt. Eine relativ hohe Anzahl eingehender Verbindungen für Ihre Ressource wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle hindeuten.|

Unter [Heuristic DNS detections in Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/) (Heuristische DNS-Erkennungen in Azure Security Center) erfahren Sie, wie Security Center netzwerkbezogene Signale für den Bedrohungsschutz verwenden kann.
## Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)<a name ="management-layer"></a>

>[!NOTE]
>Die auf Azure Resource Manager basierende Sicherheitsstufe von Security Center befindet sich derzeit in der Vorschauphase.

Security Center nutzt Azure Resource Manager-Ereignisse (also gewissermaßen die Steuerungsebene von Azure), um eine zusätzliche Schutzebene zu bieten. Durch die Analyse der Azure Resource Manager-Datensätze erkennt Security Center ungewöhnliche bzw. potenziell schädliche Vorgänge in der Azure-Abonnementumgebung.

> [!div class="mx-tableFixed"]

|Warnung|Beschreibung|
|---|---|
|**MicroBurst toolkit run** (MicroBurst-Toolkitausführung)|In Ihrer Umgebung wurde die Ausführung eines bekannten Reconnaissance-Toolkits für Cloudumgebungen erkannt. Das Tool „MicroBurst“ (siehe https://github.com/NetSPI/MicroBurst) kann von einem Angreifer (oder Penetrationtester) verwendet werden, um Ihre Abonnementressourcen zu erfassen, unsichere Konfigurationen ausfindig zu machen und vertrauliche Informationen offenzulegen.|
|**Azurite toolkit run** (Azurite-Toolkitausführung)|In Ihrer Umgebung wurde die Ausführung eines bekannten Reconnaissance-Toolkits für Cloudumgebungen erkannt. Das Tool „Azurite“ (siehe https://github.com/mwrlabs/Azurite) kann von einem Angreifer (oder Penetrationtester) verwendet werden, um Ihre Abonnementressourcen zu erfassen und unsichere Konfigurationen ausfindig zu machen.|
|**Suspicious management session using an inactive account** (Verdächtige Verwaltungssitzung mit einem inaktiven Konto)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde eine verdächtige Vorgehensweise erkannt. Ein Prinzipal, der längere Zeit nicht verwendet wurde, führt nun Aktionen aus, mit denen sich ein Angreifer dauerhaft Zugriff verschaffen kann.|
|**Suspicious management session using PowerShell** (Verdächtige Verwaltungssitzung mit PowerShell)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde eine verdächtige Vorgehensweise erkannt. Ein Prinzipal, der nicht regelmäßig PowerShell verwendet, um die Abonnementumgebung zu verwalten, verwendet nun PowerShell und führt Aktionen aus, mit denen sich ein Angreifer dauerhaft Zugriff verschaffen kann.|
|**Use of advanced Azure persistence techniques** (Verwendung erweiterter Azure-Persistenzmethoden)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde eine verdächtige Vorgehensweise erkannt. Benutzerdefinierten Rollen wurden legitimierte Identitätsentitäten zugewiesen. Dadurch kann sich ein Angreifer unter Umständen dauerhaft Zugriff auf eine Azure-Kundenumgebung verschaffen.|
|**Aktivität aus selten verwendetem Land**|Es wurde eine Aktivität über einen Standort ausgeführt, der schon länger nicht mehr oder noch nie von einem Benutzer in der Organisation verwendet wurde.<br/>Bei dieser Erkennungsmethode werden anhand von in der Vergangenheit verwendeten Aktivitätsstandorten neue und selten verwendete Standorte ermittelt. Die Anomalieerkennungsengine speichert Informationen zu Standorten, die Benutzer der Organisation in der Vergangenheit verwendet haben. 
|**Aktivitäten von anonymen IP-Adressen**|Es wurde eine Benutzeraktivität über eine IP-Adresse erkannt, die als anonyme Proxy-IP-Adresse identifiziert wurde. <br/>Diese Proxys werden von Personen verwendet, die die IP-Adresse ihres Geräts verbergen möchten, und können in böswilliger Absicht eingesetzt werden. Die Erkennung nutzt einen Machine Learning-Algorithmus, um falsch positive Ergebnisse wie etwa falsch gekennzeichnete IP-Adressen, die regelmäßig von anderen Benutzern in der Organisation verwendet werden, zu reduzieren.|
|**Impossible travel detected** (Unmöglicher Ortswechsel erkannt)|Es wurden zwei Benutzeraktivitäten (in einer einzelnen Sitzung oder in mehreren Sitzungen) festgestellt, die von unterschiedlichen geografischen Standorten stammen und in einem Zeitraum liegen, der kürzer ist als die Zeit, die der Benutzer benötigt, um von einem Ort zum anderen zu gelangen. Dies deutet darauf hin, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet. <br/>Bei dieser Erkennung wird ein Machine Learning-Algorithmus verwendet, der offensichtlich falsch positive Ergebnisse ignoriert, die den Eindruck eines unmöglichen Ortswechsels erwecken. Hierzu zählen beispielsweise VPNs sowie Standorte, die regelmäßig von anderen Benutzern der Organisation verwendet werden. Die Erkennung hat eine anfängliche Lernphase von sieben Tagen, in der sie sich mit dem Aktivitätsmuster eines neuen Benutzers vertraut macht.|

>[!NOTE]
> Einige der oben aufgeführten Analyse basieren auf Microsoft Cloud App Security (MCAS). Für die Verwendung dieser Analysen ist eine aktivierte MCAS-Lizenz erforderlich. Wenn Sie über eine MCAS-Lizenz verfügen, sind diese Warnungen standardmäßig aktiviert. So können Sie sie deaktivieren:
>
> 1. Klicken Sie auf dem Security Center-Blatt auf **Sicherheitsrichtlinie**. Klicken Sie für das Abonnement, das Sie ändern möchten, auf **Einstellungen bearbeiten**.
> 2. Klicken Sie auf **Bedrohungserkennung**.
> 3. Deaktivieren Sie unter **Enable integrations** (Integrationen aktivieren) das Kontrollkästchen **Allow Microsoft Cloud App Security to access my data** (Microsoft Cloud App Security Zugriff auf meine Daten gewähren), und klicken Sie anschließend auf **Speichern**.

>[!NOTE]
>Azure Security Center speichert sicherheitsbezogene Kundendaten im gleichen geografischen Raum, in dem sich auch die Ressource befindet. Falls Azure Security Center noch nicht im geografischen Raum der Ressource bereitgestellt wurde, werden die Daten in Nordamerika gespeichert. Wenn Microsoft Cloud App Security (MCAS) aktiviert ist, werden diese Informationen gemäß den MCAS-Regeln für geografische Standorte gespeichert. Weitere Informationen finden Sie unter [Data storage for non-regional services](http://azuredatacentermap.azurewebsites.net/) (Datenspeicherung für nicht regionale Dienste).
