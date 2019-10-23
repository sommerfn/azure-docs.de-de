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
ms.openlocfilehash: 275598aa50c252512348f4a04543e1beaf538626
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529482"
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
|[Microsoft Defender ATP-Integration](https://docs.microsoft.com/azure/security-center/security-center-wdatp)|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|X|X|X|Standard|
|[Warnungen zur Bedrohungserkennung für Virtual Machine-Verhaltensanalysen](https://docs.microsoft.com/azure/security-center/security-center-alerts-iaas)|✔|✔|✔|✔ (für unterstützte Versionen)|✔ (für unterstützte Versionen)|✔|Empfehlungen (kostenlos) Bedrohungserkennung (Standard)|
|[Warnungen der Erkennung von Fileless-Bedrohungen](https://docs.microsoft.com/azure/security-center/security-center-alerts-iaas#fileless-attack-detection-)|✔|✔|✔|X|X|X|Standard|
|[Warnungen der netzwerkbasierten Bedrohungserkennung](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer#azure-network-layer)|✔|✔|X|✔|✔|X|Standard|
|[Just-in-Time-VM-Zugriff](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)|✔|X|X|✔|X|X|Standard|
|[Dateiintegritätsüberwachung](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)|✔|✔|✔|✔|✔|✔|Standard|
|[Adaptive Anwendungssteuerungen](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)|✔|X|✔|✔|X|✔|Standard|
|[Netzwerkübersicht](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations#network-map)|✔|✔|X|✔|✔|X|Standard|
|[Adaptives Erhöhen des Netzwerkschutzes](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)|✔|X|X|✔|X|X|Standard|
|Adaptive Netzwerksteuerungen|✔|✔|X|✔|✔|X|Standard|
|[Dashboard und Berichte für die Einhaltung gesetzlicher Bestimmungen](https://docs.microsoft.com/azure/security-center/security-center-compliance-dashboard)|✔|✔|✔|✔|✔|✔|Standard|
|Empfehlungen und Bedrohungserkennung für in Docker gehostete IaaS-Container|X|X|X|✔|✔|✔|Standard|
|Fehlende Bewertung von BS-Patches|✔|✔|✔|✔|✔|✔|Kostenlos|
|Bewertung von Sicherheitsfehlkonfigurationen|✔|✔|✔|✔|✔|✔|Kostenlos|
|[Endpoint Protection-Bewertung](https://docs.microsoft.com/azure/security-center/security-center-services#supported-endpoint-protection-solutions-)|✔|✔|✔|X|X|X|Kostenlos|
|Bewertung der Datenträgerverschlüsselung|✔|✔|X|✔|✔|X|Kostenlos|
|Sicherheitsrisikobewertung durch Drittanbieter|✔|X|X|✔|X|X|Kostenlos|
|[Netzwerksicherheitsbewertung](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)|✔|✔|X|✔|✔|X|Kostenlos|

### Unterstützte Endpoint Protection-Lösungen <a name="endpoint-supported"></a>

In der folgenden Tabelle finden Sie eine Matrix zu folgenden Fragen:

 - Ob Sie mit Azure Security Center jede Lösung für sich installieren können.
 - Welche Endpunktschutz-Lösungen Security Center erkennen kann. Wenn eine Endpoint Protection-Lösungen aus dieser Liste ermittelt wird, empfiehlt Security Center nicht deren Installation.

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

 **\*** Der Abdeckungsstand und die unterstützenden Daten sind zurzeit nur im Log Analytics-Arbeitsbereich verfügbar, der Ihren geschützten Abonnements zugeordnet ist. Sie spiegeln sich nicht im Azure Security Center-Portal wider.

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
|Speicherkonto|✔| X|
|App Service|✔| ✔|
|Funktion|✔| X|
|Clouddienst|✔| X|
|VNet|✔| X|
|Subnet|✔| X|
|NIC|✔| X|
|NSG|✔| X|
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
|Redis|✔| X|
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
