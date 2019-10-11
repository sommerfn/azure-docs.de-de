---
title: Verfügbare unterstützte Features in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste der von Azure Security Center unterstützten Dienste.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 870ebc8d-1fad-435b-9bf9-c477f472ab17
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: d756f9dfbd0012f884bb0c4a1e27efc76d613234
ms.sourcegitcommit: 387da88b8262368c1b67fffea58fe881308db1c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "71982836"
---
# <a name="supported-features-available-in-azure-security-center"></a>Verfügbare unterstützte Features in Azure Security Center

> [!NOTE]
>Einige Features sind nur beim Standardtarif verfügbar. Wenn Sie sich noch nicht für den Security Center-Standardtarif registriert haben, können Sie einen kostenlosen Testzeitraum nutzen. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Die folgenden Abschnitte enthalten Security Center-Features, die für die [unterstützten Plattformen](security-center-os-coverage.md) verfügbar sind.

* [Virtuelle Computer/Server](#vm-server-features)
* [PaaS-Dienste](#paas-services)


## Virtuelle Computer/Server – unterstützte Features <a name="vm-server-features"></a>

> [!div class="mx-tableFixed"]

|Server|Windows|||Linux|||Preise|
|----|----|----|----|----|----|----|----|
|**Umgebung**|**Azure**||**Nicht-Azure**|**Azure**||**Nicht-Azure**||
||**Virtueller Computer**|**VM-Skalierungsgruppe**||**Virtueller Computer**|**VM-Skalierungsgruppe**|
|Warnungen zur Bedrohungserkennung für Virtual Machine-Verhaltensanalysen|✔|✔|✔|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|Empfehlungen (kostenlos) Bedrohungserkennung (Standard)|
|Warnungen der netzwerkbasierten Bedrohungserkennung|✔|✔|X|✔|✔|X|Standard|
|Microsoft Defender ATP-Integration|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|X|X|X|Standard|
|Fehlende Patches|✔|✔|✔|✔|✔|✔|Kostenlos|
|Sicherheitskonfigurationen|✔|✔|✔|✔|✔|✔|Kostenlos|
|Bewertung von Endpoint Protection|✔|✔|✔|X|X|X|Kostenlos|
|Just-in-Time-VM-Zugriff|✔|X|X|✔|X|X|Standard|
|Adaptive Anwendungssteuerungen|✔|X|✔|✔|X|✔|Standard|
|Dateiintegritätsüberwachung|✔|✔|✔|✔|✔|✔|Standard|
|Bewertung der Datenträgerverschlüsselung|✔|✔|X|✔|✔|X|Kostenlos|
|Drittanbieterbereitstellung|✔|X|X|✔|X|X|Kostenlos|
|NSG-Bewertung|✔|✔|X|✔|✔|X|Kostenlos|
|Erkennung von Fileless-Bedrohungen|✔|✔|✔|X|X|X|Standard|
|Netzwerkübersicht|✔|✔|X|✔|✔|X|Standard|
|Adaptives Erhöhen des Netzwerkschutzes|✔|X|X|✔|X|X|Standard|
|Adaptive Netzwerksteuerungen|✔|✔|X|✔|✔|X|Standard|
|Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen|✔|✔|✔|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungserkennung für in Docker gehostete IaaS-Container|X|X|X|✔|✔|✔|Standard|

### Unterstützte Endpoint Protection-Lösungen <a name="endpoint-supported"></a>

In der folgenden Tabelle finden Sie eine Matrix zu folgenden Fragen:

 - Ob Sie mit Azure Security Center jede Lösung für sich installieren können.
 - Welche Endpunktschutz-Lösungen Security Center erkennen kann. Wenn eine dieser Endpunktschutz-Lösungen ermittelt wird, empfiehlt Security Center nicht deren Installation.

Informationen darüber, wann Empfehlungen für die einzelnen Schutzfunktionen generiert werden, finden Sie unter [Endpoint Protection: Bewertung und Empfehlungen](security-center-endpoint-protection.md).

| Endpoint Protection| Plattformen | Security Center-Installation | Security Center-Ermittlung |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| Nein, in Betriebssystem integriert| Ja |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (siehe Hinweis unten) | Per Erweiterung | Ja |
| Trend Micro – Alle Versionen* | Windows Server-Familie  | Nein | Ja |
| Symantec v12.1.1100+| Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Windows Server-Familie  | Nein | Ja |
| McAfee v10+ | Linux-Serverfamilie  | Nein | Ja **\*** |
| Sophos V9+| Linux-Serverfamilie  | Nein | Ja **\***  |

 **\*** Der Abdeckungsstand und die unterstützenden Daten sind zurzeit nur im Log Analytics-Arbeitsbereich verfügbar, der Ihren geschützten Abonnements zugeordnet ist, und werden nicht im Azure Security Center-Portal widergespiegelt.

> [!NOTE]
>
> - Für die Erkennung von System Center Endpoint Protection (SCEP) auf einem virtuellen Computer mit Windows Server 2008 R2 muss SCEP nach PowerShell 3.0 (oder einer höheren Version) installiert werden.
> - Die Erkennung von Trend Micro-Schutz wird für Deep Security-Agents unterstützt.  OfficeScan-Agents werden nicht unterstützt.


## PaaS-Dienste – unterstützte Features <a name="paas-services"> </a>

Die folgenden PaaS-Ressourcen werden von Azure Security Center unterstützt:

|Dienst|Empfehlungen (kostenlos)|Bedrohungserkennung (Standard)|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|CosmosDB*|X| ✔|
|Blob Storage|✔| ✔|
|Speicherkonto|✔| Nicht verfügbar|
|App Service|✔| ✔|
|Funktion|✔| X|
|Clouddienst|✔| X|
|VNet|✔| Nicht verfügbar|
|Subnet|✔| Nicht verfügbar|
|NIC|✔| Nicht verfügbar|
|NSG|✔| Nicht verfügbar|
|Subscription|✔ **| ✔|
|Batch-Konto|✔| X|
|Service Fabric-Konto|✔| X|
|Automation-Konto|✔| X|
|Load Balancer|✔| X|
|Suchen,|✔| X|
|Service Bus-Namespace|✔| X|
|Stream Analytics|✔| X|
|Event Hub-Namespace|✔| X|
|Logik-Apps|✔| X|
|Redis|✔| Nicht verfügbar|
|Data Lake Analytics|✔| X|
|Data Lake Store|✔| X|
|Schlüsseltresor|✔| X|

\* Diese Features werden derzeit in der öffentlichen Vorschau unterstützt.

\*\* Empfehlungen zu Azure Active Directory (Azure AD) sind nur für Standardabonnements verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Datenerfassung in Security Center und den Log Analytics-Agent](security-center-enable-data-collection.md).
- Erfahren Sie, wie [Daten von Security Center verwaltet und geschützt werden](security-center-data-security.md).
- Hier erfahren Sie, wie Sie [die Entwurfsaspekte in Bezug auf die Einführung von Azure Security Center planen und verstehen](security-center-planning-and-operations-guide.md).
- Informieren Sie sich über die [Plattformen, die Security Center unterstützen](security-center-os-coverage.md).
- Erfahren Sie mehr über die [Bedrohungserkennung für virtuelle Computer und Server in Azure Security Center](security-center-alerts-iaas.md).
- Lesen Sie [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).
- Lesen Sie [Blogbeiträge zur Sicherheit und Compliance von Azure](https://blogs.msdn.com/b/azuresecurity/).
