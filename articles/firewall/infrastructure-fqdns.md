---
title: Infrastruktur-FQDN für Azure Firewall
description: Erfahren Sie mehr über die Infrastruktur-FQDNs in Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 09/20/2019
ms.author: victorh
ms.openlocfilehash: 5b134058e1924bae030338411226a9a6aff46557
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130200"
---
# <a name="infrastructure-fqdns"></a>Infrastruktur-FQDNs

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. 

Die folgenden Dienste sind in der integrierten Regelsammlung enthalten:

- Computezugriff auf das Speicher-PIR (Platform Image Repository, PIR)
- Speicherzugriff auf den Status verwalteter Datenträger
- Azure-Diagnose und -Protokollierung (MDS)

## <a name="overriding"></a>Überschreiben 

Diese integrierte Infrastrukturregelsammlung kann außer Kraft gesetzt werden. Erstellen Sie hierzu eine Anwendungsregelsammlung vom Typ „Alle ablehnen“, die ganz zum Schluss verarbeitet wird. Sie wird immer vor der Infrastrukturregelsammlung verarbeitet. Alles, was sich nicht in der Infrastrukturregelsammlung befindet, wird standardmäßig abgelehnt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [Azure Firewall bereitstellen und konfigurieren](tutorial-firewall-deploy-portal.md).