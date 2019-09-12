---
title: Physische HSM-Sicherheit – Dediziertes Azure-HSM | Microsoft-Dokumentation
description: Informationen zur physischen Sicherheit dedizierter Azure-HSM-Geräte in Rechenzentren
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: ede1af4625d06af4e280eda86d09ae1db3dfdfd1
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70881031"
---
# <a name="azure-dedicated-hsm-physical-security"></a>Azure-Dienst für dedizierte HSMs – physische Sicherheit

Mit dem Azure-Dienst für dedizierte HSMs erfüllen Sie die erweiterten Sicherheitsanforderungen zum Speichern von Schlüsseln. Sie werden während des gesamten Lebenszyklus mit den folgenden strengen Sicherheitsmaßnahmen verwaltet, um Kundenanforderungen zu erfüllen.

## <a name="security-through-procurement"></a>Sicherheit durch Beschaffung

Microsoft hält einen sicheren Beschaffungsprozess ein. Wir verwalten die Verwahrungskette und stellen sicher, dass das jeweilige bestellte und ausgelieferte Gerät auch das Gerät ist, das bei unseren Rechenzentren eintrifft. Die Geräte befinden sich in manipulationssicheren Kunststoffbeuteln. Sie werden in einem sicheren Bereich gelagert, bis sie im Datenkatalog des Rechenzentrums bearbeitet werden.  Die Racks, in denen sich die HSM-Geräte befinden, gelten als Geräte mit erheblichen Geschäftsauswirkungen (HBI, High Business Impact). Die Geräte sind gesperrt und stehen jederzeit an Vorder- und Rückseite unter Videoüberwachung.

## <a name="security-through-deployment"></a>Sicherheit durch Bereitstellung

HSMs werden zusammen mit zugehörigen Netzwerkkomponenten in Racks installiert. Nach der Installation müssen sie konfiguriert werden, bevor sie als Teil des Azure-Diensts für dedizierte HSMs zur Verfügung gestellt werden. Diese Konfigurationsaktivität wird von Microsoft-Mitarbeitern ausgeführt, die einer Sicherheitsprüfung unterzogen wurden. „Just In Time“-Verwaltung (JIT) wird verwendet, um den Zugriff auf die richtigen Mitarbeiter und die benötigte Zugriffszeit zu beschränken. Die verwendeten Verfahren und Systeme stellen auch sicher, dass alle Aktivitäten im Zusammenhang mit den HSM-Geräten protokolliert werden.

## <a name="security-in-operations"></a>Sicherheit im Betrieb

Da HSMs Hardwareappliances sind (das eigentliche HSM ist eine PCI-Karte innerhalb der Appliance), können Probleme auf Komponentenebene auftreten. Potenzielle Probleme können bei Lüfter und Stromversorgung auftreten, sind jedoch nicht darauf beschränkt. Dieser Ereignistyp erfordert Wartungs- oder Reparaturaktivitäten zum Ersatz austauschbarer Komponenten.

### <a name="component-replacement"></a>Komponentenaustausch

Nachdem ein Gerät bereitgestellt wurde und unter Kundenverwaltung steht, ist die im laufenden Betrieb austauschbare Stromversorgung die einzige Komponente, die ersetzt würde. Diese Komponente befindet sich außerhalb der Sicherheitsbegrenzung und bietet keinen Angriffspunkt für eine Manipulation. Mit einem Ticketsystem werden Microsoft-Techniker autorisiert, auf die Rückseite des HBI-Racks zuzugreifen. Wenn das Ticket verarbeitet wird, wird ein temporärer physischer Schlüssel ausgegeben. Dieser Schlüssel ermöglicht den Technikerzugriff auf das Gerät und den Austausch der betroffenen Komponente. Jeder andere Zugriff (d.h. potenzielle Manipulation) wird ausgeführt, wenn ein Gerät nicht einem Kunden zugewiesen ist, um Sicherheit und Verfügbarkeitsrisiko zu minimieren.  

### <a name="device-replacement"></a>Geräteaustausch

Bei einem Gerätetotalausfall wird ein Prozess eingehalten, der dem Prozess bei einem Komponentenausfall ähnelt. Wenn ein Kunde das Gerät nicht löschen kann, oder das Gerät sich in einem unbekannten Zustand befindet, werden die Geräte, die die Daten enthalten, entfernt und in einem Zerstörungsbehälter im Rack abgelegt. Geräte, die im Zerstörungsbehälter platziert werden, werden auf kontrollierte und sichere Weise zerstört. Keine Daten enthaltenden Geräte aus einem HBI-Rack verlassen ein Microsoft-Rechenzentrum.

### <a name="other-rack-access-activities"></a>Andere Rackzugriffsaktivitäten

Wenn ein Microsoft-Techniker auf das von HSM-Geräten verwendete Rack zugreifen muss (z.B. zur Netzwerkgeräte-Wartung), erfolgt der Zugriff auf das sichere HBI-Rack mit den Standardsicherheitsprozeduren. Alle Zugriffe erfolgen unter Videoüberwachung. Die HSM-Geräte werden auf [FIPS 140-2 Level 3](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf) überprüft, sodass unberechtigte Zugriffe auf die HSM-Geräte dem Kunden signalisiert und Daten gelöscht werden.

## <a name="logical-level-security-considerations"></a>Überlegungen zur Sicherheit auf logischer Ebene

HSMs werden für ein vom Kunden erstelltes virtuelles Netzwerk bereitgestellt. Dies ist der private IUP-Adressraum eines Kunden.  Diese Konfiguration bietet eine wertvolle Isolierung auf logischer Netzwerkebene und gewährleistet den ausschließlichen Zugriff durch den Kunden. Dies bedeutet, dass alle Sicherheitskontrollen auf logischer Ebene in der Verantwortung des Kunden liegen.

## <a name="next-steps"></a>Nächste Schritte

Der Benutzer sollte vor der Bereitstellung von Geräten, dem Anwendungsentwurf und der Bereitstellung mit allen Schlüsselkonzepten des Diensts wie hohe Verfügbarkeit, Sicherheit und Unterstützbarkeit gut vertraut sein.

* [Hochverfügbarkeit](high-availability.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)
* [Bereitstellungsarchitektur](deployment-architecture.md)