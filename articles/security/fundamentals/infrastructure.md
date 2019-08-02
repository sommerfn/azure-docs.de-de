---
title: Sicherheit der Azure-Infrastruktur | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Microsoft die Sicherheit seiner Azure-Rechenzentren gewährleistet.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 1dd9d9d4fc47f13f275e3be87ad282640840f873
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68611621"
---
# <a name="azure-infrastructure-security"></a>Sicherheit der Azure-Infrastruktur
Microsoft Azure wird in Rechenzentren ausgeführt, die von Microsoft verwaltet und betrieben werden. Diese geografisch voneinander getrennten Rechenzentren erfüllen die wichtigsten Branchenstandards, wie etwa ISO/IEC 27001:2013 und NIST SP 800-53, bezüglich Sicherheit und Zuverlässigkeit. Die Rechenzentren werden von Microsoft-Betriebspersonal verwaltet und überwacht. Das Betriebspersonal verfügt über langjährige Erfahrung in der Bereitstellung der weltweit größten Onlinedienste, die jeden Tag rund um die Uhr verfügbar sind.

Diese Artikelreihe enthält Informationen zu den Maßnahmen, die Microsoft zum Schutz der Azure-Infrastruktur ergreift. Die Artikel behandeln folgende Themen:

- [Physische Sicherheit](physical-security.md)
- [Verfügbarkeit](infrastructure-availability.md)
- [Komponenten und Grenzen](infrastructure-components.md)
- [Netzwerkarchitektur](infrastructure-network.md)
- [Produktionsnetzwerk](production-network.md)
- [SQL-Datenbank](infrastructure-sql.md)
- [Vorgänge](infrastructure-operations.md)
- [Überwachung](infrastructure-monitoring.md)
- [Integrität](infrastructure-integrity.md)
- [Datenschutz](protection-customer-data.md)

## <a name="shared-responsibility-model"></a>Modell der gemeinsamen Zuständigkeit
Es ist wichtig, die Aufteilung der Zuständigkeit zwischen Ihnen und Microsoft zu verstehen. In einer lokalen Umgebung sind Sie für den gesamten Stapel zuständig. Bei einem Wechsel in die Cloud wird dagegen ein Teil der Zuständigkeit auf Microsoft übertragen. Die folgende Abbildung veranschaulicht die Zuständigkeitsbereiche abhängig von der Art der Bereitstellung Ihres Stapels (Software-as-a-Service (SaaS), Platform as a Service (PaaS), Infrastructure-as-a-Service (IaaS) und lokal).

![Abbildung zu den Zuständigkeiten](./media/infrastructure/responsibility-zones.png)

Sie tragen stets die Verantwortung für die folgenden Elemente, unabhängig von der Art der Bereitstellung:

- Daten
- Endpunkte
- Konto
- Zugriffsverwaltung

Vergewissern Sie sich, dass Sie die Aufteilung der Zuständigkeit zwischen Ihnen und Microsoft in einer SaaS-, PaaS- und IaaS-Bereitstellung verstanden haben. Weitere Informationen dazu finden Sie unter [Gemeinsame Verantwortung für das Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie mehr über den Schutz der Azure-Infrastruktur durch Microsoft:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](infrastructure-availability.md)
- [Komponenten und Grenzen des Azure-Informationssystems](infrastructure-components.md)
- [Azure-Netzwerkarchitektur](infrastructure-network.md)
- [Azure-Produktionsnetzwerk](production-network.md)
- [Sicherheitsfeatures für Azure SQL-Datenbank](infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](infrastructure-operations.md)
- [Azure-Infrastrukturüberwachung](infrastructure-monitoring.md)
- [Integrität der Azure-Infrastruktur](infrastructure-integrity.md)
- [Schutz der Azure-Kundendaten](protection-customer-data.md)


