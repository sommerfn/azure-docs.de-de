---
title: Sicherheitsframework – Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Sicherheits-, Authentifizierungs- und Autorisierungsstandards, die bei der Entwicklung von Azure IoT Edge verwendet wurden und die Sie bei der Entwicklung Ihrer Lösung berücksichtigen sollten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 37d5288389c7b602eb0d13a736e289010d7e0f80
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208188"
---
# <a name="security-standards-for-azure-iot-edge"></a>Sicherheitsstandards für Azure IoT Edge

Azure IoT Edge deckt die Risikoszenarien ab, die mit dem Verschieben Ihrer Daten und Analysen zu Intelligent Edge verbunden sind. Die IoT Edge-Sicherheitsstandards gleichen die Flexibilität für verschiedene Bereitstellungsszenarien mit dem Schutz aus, den Sie von allen Azure-Diensten erwarten. 

Azure IoT Edge kann auf verschiedenen Hardwaremarken und -modellen ausgeführt werden, unterstützt mehrere Betriebssysteme und eignet sich für diverse Bereitstellungsszenarien. Das Risiko eines Bereitstellungsszenarios ist abhängig von Faktoren wie z.B. dem Besitz der Lösung, der geografischen Region für die Bereitstellung, der Vertraulichkeit der Daten, der Branche, in der die Anwendung eingesetzt wird, und gesetzlichen Vorgaben. Anstatt konkrete Lösungen für bestimmte Szenarien anzubieten, stellt IoT Edge ein erweiterbares Sicherheitsframework dar, das auf fundierten Prinzipien fußt und Skalierbarkeit bietet. 
 
Dieser Artikel bietet eine Übersicht über das IoT Edge-Sicherheitsframework. Weitere Informationen finden Sie unter [Sicherheit für Intelligent Edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standards

Standards fördern eine einfache Überprüfbarkeit und Implementierung – zwei Eckpfeiler der Sicherheit. Eine Sicherheitslösung sollte zur Überprüfbarkeit beitragen, um Vertrauen aufzubauen, und sie darf kein Hindernis bei der Bereitstellung sein. Das Design des Frameworks für den Schutz von Azure IoT Edge basiert auf Sicherheitsprotokollen, die über lange Zeit getestet wurden und sich in der Branche bewährt haben, und bietet somit Vertrautheit und Wiederverwendbarkeit. 

## <a name="authentication"></a>Authentication

Wenn Sie eine IoT-Lösung bereitstellen, muss gewährleistet sein, dass nur vertrauenswürdige Akteure, Geräte und Module Zugriff auf Ihre Lösung haben. Die zertifikatsbasierte Authentifizierung ist der primäre Authentifizierungsmechanismus für die Azure IoT Edge-Plattform. Dieser Mechanismus wurde aus einer Reihe von Standards abgeleitet, die die Public Key Infrastructure (PKiX) der Internet Engineering Task Force (IETF) regeln.     

Alle Geräte, Module und Akteure, die mit dem Azure IoT Edge-Gerät interagieren – physisch oder über eine Netzwerkverbindung –, müssen eindeutige Zertifikatidentitäten aufweisen. Weil sich nicht jedes Szenario oder jede Komponente für die zertifikatbasierte Authentifizierung eignet, bietet die Erweiterbarkeit des Sicherheitsframeworks sichere Alternativen. 

Weitere Informationen finden Sie unter [Azure IoT Edge-Zertifikatverwendung](iot-edge-certs.md).

## <a name="authorization"></a>Authorization

Das Prinzip der geringsten Rechte besagt, dass Benutzer und Komponenten eines Systems nur Zugriff auf den minimalen Satz von Ressourcen und Daten haben sollten, die zum Ausüben ihrer Rollen erforderlich sind. Geräte, Module und Akteure sollten nur auf Ressourcen und Daten in ihrem Berechtigungsbereich zugreifen – und auch nur dann, wenn die Architektur es zulässt. Einige Berechtigungen sind mit ausreichenden Rechten konfigurierbar, andere dagegen werden architekturbedingt erzwungen.  Beispielsweise können einige Module autorisiert werden, eine Verbindung mit Azure IoT Hub herzustellen. Es besteht jedoch kein Grund für ein Modul in einem IoT Edge-Gerät, auf den Zwilling eines Moduls in einem anderen IoT Edge-Gerät zuzugreifen.

Weitere Autorisierungskonzepte umfassen Rechte zum Signieren von Zertifikaten und die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). 

## <a name="attestation"></a>Nachweis

Der Nachweis stellt die Integrität von Softwarebits sicher, und dies ist wichtig für das Erkennen und Verhindern von Schadsoftware.  Das Azure IoT Edge-Sicherheitsframework klassifiziert Nachweise in drei Hauptkategorien:

* Statischer Nachweis
* Nachweis zur Laufzeit
* Softwarenachweis

### <a name="static-attestation"></a>Statischer Nachweis

Beim statischen Nachweis wird die Integrität aller Softwarebestandteile auf einem Gerät während des Starts überprüft, darunter die des Betriebssystems, aller Runtimes und der Konfigurationsinformationen. Da der statische Nachweis während des Starts erfolgt, wird er oft als „sicherer Start“ bezeichnet. Das Sicherheitsframework für IoT Edge-Geräte wird auch von Herstellern verwendet. Es schließt sichere Hardwarefeatures ein, die statische Nachweisprozesse sicherstellen. Zu diesen Prozessen gehören sicherer Gerätestart und sichere Firmwareupgrades.  Eine enge Zusammenarbeit mit Hardwareherstellern verhindert überflüssige Firmwareschichten und minimiert damit die Angriffsfläche. 

### <a name="runtime-attestation"></a>Nachweis zur Laufzeit

Nachdem ein System einen sicheren Startprozess abgeschlossen hat, sollten sorgfältig entwickelte Sicherheitssysteme Versuche der Einschleusung von Schadsoftware erkennen und entsprechende Gegenmaßnahmen einleiten. Angriffe durch Schadsoftware können etwa auf die Ports und Schnittstellen des Systems abzielen. Wenn böswillige Akteure physischen Zugriff auf ein Gerät haben, können sie das Gerät auf ähnliche Weise direkt manipulieren oder über Seitenkanalangriffe Zugriff erhalten. Unabhängig davon, ob es sich bei solchen schädlichen Inhalten um Schadsoftware oder nicht autorisierte Konfigurationsänderungen handelt, kann ein statischer Nachweis sie nicht erkennen, weil sie erst nach dem Startvorgang eingeschleust werden. Gegenmaßnahmen, die von der Gerätehardware angeboten oder erzwungen werden, tragen zur Abwehr solcher Bedrohungen bei.  Das Sicherheitsframework für IoT Edge fordert ausdrücklich Erweiterungen zur Bekämpfung von Laufzeitbedrohungen.  

### <a name="software-attestation"></a>Softwarenachweis

Alle fehlerfreien Systeme, einschließlich Intelligent Edge-Systemen, benötigen Patches und Upgrades.  Sicherheit ist wichtig für Updateprozesse, da diese sonst potenzielle Bedrohungsvektoren sein können.  Das Sicherheitsframework für IoT Edge fordert, dass Updates über bewertete und signierte Pakete erfolgen, um die Integrität der Paketquellen sicherzustellen und zu authentifizieren.  Dieser Standard gilt für alle Betriebssysteme und Bestandteile von Softwareanwendungen. 

## <a name="hardware-root-of-trust"></a>Vertrauensanker in Hardware

Bei vielen Intelligent Edge-Geräten – insbesondere bei Geräten, auf die durch potenzielle böswillige Akteure physisch zugegriffen werden kann, – ist Hardwaresicherheit die letzte Schutzmaßnahme. Manipulationssichere Hardware ist für solche Bereitstellungen ausschlaggebend. Azure IoT Edge ermuntert Anbieter von sicherer Hardware, verschiedene Arten von Hardware-Vertrauensankern für unterschiedliche Risikoprofile und Bereitstellungsszenarien anzubieten. Das Vertrauen in Hardware kann von allgemeinen Sicherheitsprotokollstandards wie dem Trusted Platform Module (ISO/IEC 11889) oder der Device Identifier Composition Engine (DICE) der Trusted Computing Group stammen. Auch sichere Enklaventechnologien wie TrustZones und Software Guard Extensions (SGX) stellen Vertrauen in Hardware her. 

## <a name="certification"></a>Zertifizierung

Damit Kunden beim Erwerb von Azure IoT Edge-Geräten fundierte Entscheidungen für ihre Bereitstellung treffen können, enthält das IoT Edge-Framework Zertifizierungsanforderungen.  Die Grundlage dieser Anforderungen sind Zertifizierungen in Bezug auf Sicherheitserfordernisse und Zertifizierungen in Bezug auf die Validierung der Sicherheitsimplementierung.  Beispielsweise bedeutet eine Zertifizierung in Bezug auf Sicherheitserfordernisse, dass für das IoT Edge-Gerät sichere Hardware eingesetzt wird, die Startangriffe bekanntermaßen abwehrt. Eine Validierungszertifizierung bedeutet, dass die sichere Hardware ordnungsgemäß implementiert wurde, sodass dieses Feature im Gerät wirklich genutzt werden kann.  Im Einklang mit dem Prinzip der Einfachheit ist das Framework darauf ausgelegt, den Aufwand für Zertifizierungen so gering wie möglich zu halten.   

## <a name="extensibility"></a>Erweiterbarkeit

Da die IoT-Technologie verschiedene Formen von geschäftlichen Transformationen fördert, sollten Sicherheitsmaßnahmen parallel weiterentwickelt werden, um neue Szenarien abzudecken.  Das Azure IoT Edge-Sicherheitsframework startet mit einem soliden Fundament, auf dem die Erweiterbarkeit in verschiedene Dimensionen aufgebaut wird, damit folgende Aspekte berücksichtigt werden: 

* Sicherheitsdienste des Erstanbieters, wie etwa den Device Provisioning-Dienst für Azure IoT Hub.
* Drittanbieterdienste wie verwaltete Sicherheitsdienste für verschiedene Branchen (z.B. Industrie oder Gesundheitswesen) oder technologische Fokusbereiche (z.B. Sicherheitsüberwachung in Cloudnetzwerken oder Sicherheitshardware-Nachweisdienste) über ein umfassendes Partnernetzwerk.
* Ältere Systeme, die mit alternativen Sicherheitsstrategien umgerüstet werden müssen, indem z.B. eine andere Sicherheitstechnologie als Zertifikate für die Authentifizierung und Identitätsverwaltung verwendet wird.
* Sichere Hardware für die Einführung neuer sicherer Hardwaretechnologien und Beiträge von Hardwarepartnern.

Schließlich erfordert das Sichern von Intelligent Edge gemeinsame Beiträge aus einer offenen Community, die vom gemeinsamen Interesse am Sichern von IoT angetrieben wird.  Bei diesen Beiträgen kann es sich um sichere Technologien oder sichere Dienste handeln.  Das Azure IoT Edge-Sicherheitsframework bietet ein solides Fundament für die Sicherheit, das erweiterbar ist, um im intelligenten Edge das gleiche Maß an Vertrauen und Integrität bereitzustellen wie in der Azure-Cloud.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure IoT Edge unter [Sicherheit für Intelligent Edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
