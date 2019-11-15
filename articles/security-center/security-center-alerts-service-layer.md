---
title: 'Bedrohungserkennung für die Azure-Dienstebene: Azure Security Center'
description: In diesem Thema werden die in Azure Security Center verfügbaren Warnungen für die Azure-Dienstebene vorgestellt.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 08/25/2019
ms.author: memildin
ms.openlocfilehash: b82eab9d20966ddd0678c9213bf25a14b5313f58
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686456"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center

In diesem Thema werden die Azure Security Center-Warnungen vorgestellt, die bei der Überwachung der folgenden Azure-Dienstebenen zur Verfügung stehen:

* [Azure-Netzwerkebene](#network-layer)
* [Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)](#management-layer)

>[!NOTE]
>Die folgenden Analysen gelten für alle Ressourcentypen. Dafür werden die Telemetriedaten genutzt, die Security Center anhand der internen Azure-Feeds bereitstellt.

## Azure-Netzwerkebene<a name="network-layer"></a>

Die Netzwerkebenenanalysen von Security Center basieren auf exemplarischen [IPFIX-Daten](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) (durch Azure-Kernrouter erfasste Paketheader). Auf der Grundlage dieses Datenfeeds werden schädliche Datenverkehrsaktivitäten durch Machine Learning-Modelle von Security Center identifiziert und gekennzeichnet. Zur Anreicherung von IP-Adressen nutzt Security Center die Threat Intelligence-Datenbank von Microsoft.

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Verdächtige ausgehende RDP-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende RDP-Kommunikation (Remote Desktop Protocol) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt für diese Umgebung als abweichend. Dies weist möglicherweise darauf hin, dass Ihre Ressource kompromittiert wurde und jetzt für Brute-Force-Angriffe auf einen externen RDP-Endpunkt verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Stellen als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende RDP-Netzwerkaktivität für mehrere Ziele**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende RDP-Kommunikation mit mehreren Zielen erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt für diese Umgebung als abweichend. Dies weist möglicherweise darauf hin, dass Ihre Ressource kompromittiert wurde und jetzt für Brute-Force-Angriffe auf externe RDP-Endpunkte verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Stellen als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende SSH-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende SSH-Kommunikation (Secure Shell) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt für diese Umgebung als abweichend. Dies weist möglicherweise darauf hin, dass Ihre Ressource kompromittiert wurde und jetzt für Brute-Force-Angriffe auf einen externen SSH-Endpunkt verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Stellen als schädlich gekennzeichnet wird.|
|**Verdächtige ausgehende SSH-Netzwerkaktivität für mehrere Ziele**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende SSH-Kommunikation mit mehreren Zielen erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt für diese Umgebung als abweichend. Dies weist möglicherweise darauf hin, dass Ihre Ressource kompromittiert wurde und jetzt für Brute-Force-Angriffe auf externe SSH-Endpunkte verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Stellen als schädlich gekennzeichnet wird.|
|**Verdächtige eingehende SSH-Netzwerkaktivität mit mehreren Quellen**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende SSH-Kommunikation mehrerer Quellen mit einer Ressource in Ihrer Bereitstellung erkannt. Der Umstand, dass verschiedene individuelle IP-Adressen eine Verbindung mit Ihrer Ressource herstellen, wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle von mehreren Hosts (Botnet) hindeuten.|
|**Verdächtige eingehende SSH-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende SSH-Kommunikation mit einer Ressource in Ihrer Bereitstellung erkannt. Eine relativ hohe Anzahl eingehender Verbindungen für Ihre Ressource wird bei dieser Umgebung als anomal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle hindeuten.
|**Verdächtige eingehende RDP-Netzwerkaktivität mit mehreren Quellen**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende RDP-Kommunikation mehrerer Quellen mit einer Ressource in Ihrer Bereitstellung erkannt. Der Umstand, dass verschiedene individuelle IP-Adressen eine Verbindung mit Ihrer Ressource herstellen, wird bei dieser Umgebung als anormal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre RDP-Schnittstelle von mehreren Hosts (Botnet) hindeuten.|
|**Verdächtige eingehende RDP-Netzwerkaktivität**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche eingehende RDP-Kommunikation mit einer Ressource in Ihrer Bereitstellung erkannt. Eine relativ hohe Anzahl eingehender Verbindungen für Ihre Ressource wird bei dieser Umgebung als anomal eingestuft. Diese Aktivität kann auf einen versuchten Brute-Force-Angriff auf Ihre SSH-Schnittstelle hindeuten.|
|**Netzwerkkommunikation mit einer schädlichen Adresse erkannt**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine Kommunikation erkannt, die von einer Ressource in Ihrer Bereitstellung mit möglicherweise einem C&C-Server (Command-and-Control) stammt. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|

Unter [Heuristic DNS detections in Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/) (Heuristische DNS-Erkennungen in Azure Security Center) erfahren Sie, wie Security Center netzwerkbezogene Signale für den Bedrohungsschutz verwenden kann.

>[!NOTE]
>Warnungen zur Bedrohungserkennung auf Azure-Netzwerkebene werden in Azure Security Center nur auf virtuellen Computern generiert, denen für die gesamte Stunde, in der eine verdächtige Kommunikation stattfindet, dieselbe IP-Adresse zugewiesen wurde. Dies gilt für virtuelle Computer – auch für solche, die als Teil eines verwalteten Diensts (z. B. AKS, Databricks) im Abonnement des Kunden erstellt werden.

## Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)<a name ="management-layer"></a>

>[!NOTE]
>Die auf Azure Resource Manager basierende Sicherheitsstufe von Security Center befindet sich derzeit in der Vorschauphase.

Security Center nutzt Azure Resource Manager-Ereignisse (also gewissermaßen die Steuerungsebene von Azure), um eine zusätzliche Schutzebene zu bieten. Durch die Analyse der Azure Resource Manager-Datensätze erkennt Security Center ungewöhnliche bzw. potenziell schädliche Vorgänge in der Azure-Abonnementumgebung.

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**MicroBurst toolkit run** (MicroBurst-Toolkitausführung)|In Ihrer Umgebung wurde die Ausführung eines bekannten Reconnaissance-Toolkits für Cloudumgebungen erkannt. Das Tool [MicroBurst](https://github.com/NetSPI/MicroBurst) kann von einem Angreifer (oder Penetrationstester) verwendet werden, um Ihre Abonnementressourcen zu erfassen, unsichere Konfigurationen zu ermitteln und vertrauliche Informationen offenzulegen.|
|**Azurite toolkit run** (Azurite-Toolkitausführung)|In Ihrer Umgebung wurde die Ausführung eines bekannten Reconnaissance-Toolkits für Cloudumgebungen erkannt. Das Tool [Azurite](https://github.com/mwrlabs/Azurite) kann von einem Angreifer (oder Penetrationstester) verwendet werden, um Ihre Abonnementressourcen zu erfassen und unsichere Konfigurationen zu ermitteln.|
|**Suspicious management session using an inactive account** (Verdächtige Verwaltungssitzung mit einem inaktiven Konto)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde verdächtiges Verhalten erkannt. Ein Prinzipal, der längere Zeit nicht verwendet wurde, führt nun Aktionen aus, mit denen sich ein Angreifer dauerhaft Zugriff verschaffen kann.|
|**Suspicious management session using PowerShell** (Verdächtige Verwaltungssitzung mit PowerShell)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde verdächtiges Verhalten erkannt. Ein Prinzipal, der nicht regelmäßig PowerShell verwendet, um die Abonnementumgebung zu verwalten, verwendet nun PowerShell und führt Aktionen aus, mit denen sich ein Angreifer dauerhaft Zugriff verschaffen kann.|
|**Use of advanced Azure persistence techniques** (Verwendung erweiterter Azure-Persistenzmethoden)|Bei der Analyse der Abonnementaktivitätsprotokolle wurde verdächtiges Verhalten erkannt. Benutzerdefinierten Rollen wurden legitimierte Identitätsentitäten zugewiesen. Dadurch kann sich ein Angreifer unter Umständen dauerhaft Zugriff auf eine Azure-Kundenumgebung verschaffen.|
|**Aktivität aus selten verwendetem Land**|Es wurde eine Aktivität über einen Standort ausgeführt, der schon länger nicht mehr oder noch nie von einem Benutzer in der Organisation verwendet wurde.<br/>Bei dieser Erkennungsmethode werden anhand von in der Vergangenheit verwendeten Aktivitätsstandorten neue und selten verwendete Standorte ermittelt. Die Anomalieerkennungsengine speichert Informationen zu Standorten, die Benutzer der Organisation in der Vergangenheit verwendet haben. 
|**Aktivitäten von anonymen IP-Adressen**|Es wurde eine Benutzeraktivität über eine IP-Adresse erkannt, die als anonyme Proxy-IP-Adresse identifiziert wurde. <br/>Diese Proxys werden von Personen, die die IP-Adresse ihres Geräts verbergen möchten, möglicherweise auch in böswilliger Absicht verwendet. Die Erkennung nutzt einen Algorithmus für maschinelles Lernen, um falsch positive Ergebnisse wie etwa falsch gekennzeichnete IP-Adressen zu reduzieren, die regelmäßig von anderen Benutzern in der Organisation verwendet werden.|
|**Impossible travel detected** (Unmöglicher Ortswechsel erkannt)|Es sind zwei Benutzeraktivitäten (in einer oder mehreren Sitzungen) aufgetreten, die von geografisch entfernten Standorten stammen. Sie erfolgten innerhalb eines kürzeren Zeitraums als der Zeit, die der Benutzer benötigt hätte, um vom ersten Standort zum zweiten zu reisen. Dies deutet darauf hin, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet. <br/>Bei dieser Erkennung wird ein Algorithmus des maschinellen Lernens verwendet, der offensichtlich falsch positive Ergebnisse ignoriert, die den Eindruck eines unmöglichen Ortswechsels erwecken. Hierzu zählen beispielsweise VPNs sowie Standorte, die regelmäßig von anderen Benutzern der Organisation verwendet werden. Die Erkennung hat eine anfängliche Lernphase von sieben Tagen, in der sie sich mit dem Aktivitätsmuster eines neuen Benutzers vertraut macht.|

>[!NOTE]
> Einige der oben aufgeführten Analysen basieren auf Microsoft Cloud App Security. Um von diesen Analysen zu profitieren, müssen Sie eine Cloud App Security-Lizenz aktivieren. Wenn Sie über eine Cloud App Security-Lizenz verfügen, sind diese Warnungen standardmäßig aktiviert. So können Sie sie deaktivieren:
>
> 1. Wählen Sie auf dem Blatt **Security Center** die Option **Sicherheitsrichtlinie** aus. Wählen Sie für das Abonnement, das Sie ändern möchten, **Einstellungen bearbeiten** aus.
> 2. Wählen Sie **Bedrohungserkennung** aus.
> 3. Deaktivieren Sie unter **Integrationen aktivieren** das Kontrollkästchen **Microsoft Cloud App Security Zugriff auf meine Daten erteilen**, und wählen Sie anschließend **Speichern** aus.

>[!NOTE]
>Security Center speichert sicherheitsbezogene Kundendaten im gleichen geografischen Raum, in dem sich auch die Ressource befindet. Falls Security Center von Microsoft noch nicht im geografischen Raum der Ressource bereitgestellt wurde, werden die Daten in den USA gespeichert. Wenn Cloud App Security aktiviert ist, werden diese Informationen gemäß den Regeln für geografische Standorte von Cloud App Security gespeichert. Weitere Informationen finden Sie unter [Data storage for non-regional services](https://azuredatacentermap.azurewebsites.net/) (Datenspeicherung für nicht regionale Dienste).
