---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 8106b1a77f11874b1b13f41bfc9e62ea800afc98
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361262"
---
### <a name="database-tier"></a>Datenbankschicht

Die Datenbankschicht enthält die Datenbankinstanzen für die Anwendung. Bei der Datenbank kann es sich um ein Oracle DB-, Oracle RAC- oder Oracle Exadata Database-System handeln. 

Wenn die Wahl auf Oracle DB fällt, kann die Datenbankinstanz in Azure über die Oracle DB-Images bereitgestellt werden, die auf dem Azure Marketplace verfügbar sind. Alternativ können Sie auch die Verbindung zwischen Azure und der OCI verwenden, um Oracle DB in einem PaaS-Modell in der OCI bereitzustellen.

Oracle RAC können Sie unter Azure CloudSimple per IaaS-Modell oder in der OCI per PaaS-Modell bereitstellen. Wir empfehlen Ihnen, ein RAC-System mit zwei Knoten zu verwenden. 

Verwenden Sie für Exadata-Systeme die OCI-Verbindung, und stellen Sie das Exadata-System in der OCI bereit. Im obigen Architekturdiagramm ist ein Exadata-System dargestellt, das in der OCI in zwei Subnetzen bereitgestellt wurde.

Stellen Sie für Produktionsszenarien mehrere Instanzen der Datenbank in zwei Verfügbarkeitszonen (bei Bereitstellung in Azure) bzw. zwei Verfügbarkeitsdomänen (in der OCI) bereit. Nutzen Sie Oracle Active Data Guard, um die primäre Datenbank und die Standbydatenbank zu synchronisieren.

Die Datenbankebene empfängt nur Anforderungen von der mittleren Ebene. Wir empfehlen Ihnen, eine Netzwerksicherheitsgruppe einzurichten (Sicherheitsliste bei Bereitstellung der Datenbank in der OCI), um nur Anforderungen über Port 1521 von der mittleren Ebene und Port 22 vom Bastionsserver zu Verwaltungszwecken zuzulassen.

Für in der OCI bereitgestellte Datenbanken muss ein separates virtuelles Cloudnetzwerk mit einem Gateway für dynamisches Routing (Dynamic Routing Gateway, DRG) eingerichtet werden, das mit Ihrer FastConnect-Leitung verbunden ist.