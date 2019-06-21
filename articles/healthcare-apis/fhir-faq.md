---
title: Häufig gestellte Fragen (FAQ) zu FHIR-Diensten in Azure – Azure API for FHIR
description: In diesem Artikel werden häufig gestellte Fragen zu Azure API for FHIR beantwortet.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 50a610c3fb92b65b62e17a986ea1c0f70d0a0f1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753746"
---
# <a name="frequently-asked-questions-about-azure-api-for-fhir"></a>Häufig gestellte Fragen zu Azure API for FHIR

## <a name="storage-location"></a>Speicherort

**Werden die Daten hinter den FHIR&reg;-APIs in Azure gespeichert?** Ja. Die Daten werden in verwalteten Datenbanken in Azure gespeichert. Die Azure API for FHIR bietet keinen direkten Zugriff auf den zugrunde liegenden Datenspeicher.

## <a name="identity-providers"></a>Identitätsanbieter

Derzeit wird Microsoft Azure Active Directory als Identitätsanbieter unterstützt.

## <a name="supported-fhir-version"></a>Unterstützte FHIR-Version

Azure API for FHIR (PaaS): Version 3.0.1 wird unterstützt.

FHIR-Server für Azure (OSS): Zusätzlich zu Version 3.0.1 wird Version 4.0.0, die neueste Version der FHIR-Spezifikation, unterstützt.

Ausführliche Informationen finden Sie unter [Unterstützte Features](fhir-features-supported.md). Informationen zu den Änderungen zwischen Versionen finden Sie im [Versionsverlauf für HL7 FHIR](http://hl7.org/fhir/R4/history.html).

## <a name="oss-and-azure-api-for-fhir"></a>OSS und Azure API for FHIR

Worin besteht der Unterschied zwischen dem Open Source Microsoft FHIR-Server für Azure und Azure API for FHIR? Azure API for FHIR ist eine gehostete und verwaltete Version des OSS Microsoft FHIR-Servers für Azure. Im verwalteten Dienst stellt Microsoft die gesamte Wartung, Updates usw. zur Verfügung. Beim Ausführen des OSS FHIR-Servers für Azure haben Sie direkten Zugriff auf die zugrunde liegenden Dienste, sind aber auch für die Wartung, Updates des Servers und alle erforderlichen Compliance-Aufgaben zuständig, wenn Sie PHI-Daten speichern.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einige der häufig gestellten Fragen zu Azure API for FHIR gelesen. Weitere Informationen zu den unterstützten API-Features in Microsoft FHIR-Server für Azure.
 
>[!div class="nextstepaction"]
>[Unterstützte FHIR-Features](fhir-features-supported.md)