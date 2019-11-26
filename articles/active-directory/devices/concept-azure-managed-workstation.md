---
title: Informationen zu sicheren, von Azure verwalteten Arbeitsstationen – Azure Active Directory
description: Lernen Sie sichere, von Azure verwaltete Arbeitsstationen kennen, und erfahren Sie, warum sie wichtig sind.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2abc5434f11bf00c6872775b1336694c04972e95
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73200223"
---
# <a name="understand-secure-azure-managed-workstations"></a>Informationen zu sicheren, von Azure verwalteten Arbeitsstationen

Gesicherte, isolierte Arbeitsstationen sind von entscheidender Bedeutung für die Sicherheit sensibler Rollen wie Administratoren, Entwickler und Operatoren kritischer Dienste. Wenn die Sicherheit der Client-Arbeitsstationen beeinträchtigt ist, können viele Sicherheitskontrollen und -zusicherungen fehlschlagen oder ineffektiv sein.

In diesem Dokument wird erläutert, was Sie zum Erstellen einer sicheren Arbeitsstation, häufig auch als Privileged Access Workstation (PAW) bezeichnet, benötigen. Der Artikel enthält außerdem ausführliche Anweisungen zum Einrichten erster Sicherheitskontrollen. In dieser Anleitung wird beschrieben, wie cloudbasierte Technologie den Dienst verwalten kann. Er basiert auf Sicherheitsfunktionen, die in Windows 10RS5, Microsoft Defender Advanced Threat Protection (ATP), Azure Active Directory und Intune eingeführt wurden.

> [!NOTE]
> In diesem Artikel wird das Konzept einer sicheren Arbeitsstation und deren Bedeutung beschrieben. Wenn Sie mit dem Konzept bereits vertraut sind und zur Bereitstellung springen möchten, wechseln Sie zu [Bereitstellung einer sicheren Workstation](howto-azure-managed-workstation.md).

## <a name="why-secure-workstation-access-is-important"></a>Warum ist ein sicherer Zugriff auf die Arbeitsstation wichtig?

Die schnelle Einführung von Clouddiensten und die Möglichkeit, überall arbeiten zu können, haben zu einer neuen Methode zur Ausnutzung von Schwachstellen geführt. Durch die Ausnutzung schwacher Sicherheitskontrollen auf Geräten, auf denen Administratoren arbeiten, können Angreifer Zugriff auf privilegierte Ressourcen erhalten.

Privilegierte Missbrauchs- und Lieferkettenangriffe gehören zu den fünf häufigsten Methoden, mit denen Angreifer gegen Unternehmen vorgehen. Sie zählen auch zu den am zweithäufigsten erkannten Taktiken bei Vorfällen, die 2018 gemäß dem [Verizon Threat Report](https://enterprise.verizon.com/resources/reports/dbir/) und dem [Security Intelligence Report](https://aka.ms/sir) gemeldet wurden.

Die meisten Angreifer gehen wie folgt vor:

1. Erkundung einer Einfallsmöglichkeit, oft branchenspezifisch.
1. Analyse zum Sammeln von Informationen und Identifizieren der besten Infiltrierungsmöglichkeit bei einer Arbeitsstation, die als geringwertig betrachtet wird.
1. Hartnäckigkeit bei der Suche nach einer Möglichkeit für [laterale](https://en.wikipedia.org/wiki/Network_Lateral_Movement) Angriffe.
1. Abschöpfen von vertraulichen und sensiblen Daten.

Bei der Erkundung infiltrieren Angreifer häufig Geräte, die als weniger risikobehaftet bzw. wichtig betrachtet werden. Mithilfe dieser anfälligen Geräte suchen sie nach einer Möglichkeit für laterale Angriffe und nach administrativen Benutzern und Geräten. Nachdem sie Zugriff auf privilegierte Benutzerrollen erhalten haben, identifizieren Angreifer hochwertige Daten und schöpfen diese erfolgreich ab.

![Typisches Kompromittierungsmuster](./media/concept-azure-managed-workstation/typical-timeline.png)

In diesem Dokument wird eine Lösung beschrieben, die Ihre Computergeräte vor diesen lateralen Angriffen schützen kann. Die Lösung isoliert Verwaltung und Dienste von weniger wertvollen Produktionsgeräten und unterbricht die Kette, bevor das Gerät, das Zugriff auf sensible Cloud-Ressourcen hat, infiltriert werden kann. Die Lösung verwendet native Azure-Dienste, die Bestandteil des Microsoft 365 Enterprise-Pakets sind:

* Intune für die Geräteverwaltung und eine sichere Liste von Anwendungen und URLs
* Autopilot für die Einrichtung, Bereitstellung und Aktualisierung von Geräten
* Azure AD für Benutzerverwaltung, bedingten Zugriff und mehrstufige Authentifizierung
* Windows 10 (aktuelle Version) für Integritätsnachweis für Geräte und Benutzerfreundlichkeit
* Defender ATP für Cloud-basierten Endpunktschutz, -erkennung und -reaktion
* Azure AD PIM zur Verwaltung von Autorisierung und privilegiertem Just-in-time (JIT)-Zugriff auf Ressourcen

## <a name="who-benefits-from-a-secure-workstation"></a>Wer profitiert von einer sicheren Arbeitsstation?

Alle Benutzer und Operatoren profitieren von der Verwendung einer sicheren Arbeitsstation. Ein Angreifer, der einen PC oder ein Gerät gefährdet, kann die Identität aller zwischengespeicherten Konten annehmen. Wenn er auf dem Gerät angemeldet ist, kann er auch Anmeldeinformationen und Token verwenden. Durch dieses Risiko ist es wichtig, Geräte zu sichern, die für privilegierte Rollen (einschließlich Administratorrechte) verwendet werden. Geräte mit privilegierten Konten sind Ziele für laterale Angriffe und Rechteausweitungsangriffe. Diese Konten können für eine Vielzahl von Ressourcen verwendet werden:

* Administratoren von lokalen und cloudbasierten Systemen
* Entwicklerarbeitsstationen für wichtige Systeme
* Administratoren von Social Media-Konten mit hohem Gefährdungsgrad
* Hochsensible Arbeitsstationen, z.B. SWIFT-Bezahlterminals
* Arbeitsstationen, auf denen Geschäftsgeheimnisse verarbeitet werden

Zur Risikominimierung sollten Sie erhöhte Sicherheitskontrollen für privilegierte Arbeitsstationen implementieren, die diese Konten verwenden. Weitere Informationen finden Sie im [Leitfaden zur Bereitstellung von Azure Active Directory-Funktionen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2) sowie unter [Office 365-Roadmap](https://aka.ms/o365secroadmap) und [Sichern des privilegierten Zugriffs (Roadmap)](https://aka.ms/sparoadmap).

## <a name="why-use-dedicated-workstations"></a>Warum sollen dedizierte Arbeitsstationen verwendet werden?

Es ist zwar möglich, ein bestehendes Gerät abzusichern, aber es ist besser, direkt mit einem sicheren Fundament zu beginnen. Damit Ihr Unternehmen bestmöglich ein hohes Sicherheitsniveau gewährleisten kann, beginnen Sie mit einem Gerät, von dem Sie wissen, dass es sicher ist, und implementieren eine Reihe von bekannten Sicherheitskontrollen.

Eine wachsende Anzahl von Angriffsvektoren durch E-Mail und Webbrowsing macht es immer schwieriger, sicherzustellen, dass einem Gerät vertraut werden kann. Dieser Leitfaden geht davon aus, dass eine dedizierte Arbeitsstation von Standardproduktivität, Surfen im Internet und E-Mail isoliert ist. Das Entfernen von Produktivität, Surfen im Internet und E-Mail von einem Gerät kann sich negativ auf die Produktivität auswirken. Diese Sicherheitsmaßnahme ist jedoch in der Regel bei Szenarien akzeptabel, in denen die Arbeitsaufgaben diese Funktionen nicht explizit erfordern und das Sicherheitsrisiko hoch ist.

> [!NOTE]
> Das Surfen im Internet bezieht sich hier auf den allgemeinen Zugriff auf beliebige Websites, der eine risikoreiche Aktivität darstellen kann. Ein solches Surfen unterscheidet sich deutlich von der Verwendung eines Webbrowsers für den Zugriff auf eine kleine Anzahl bekannter administrativer Websites für Dienste wie Azure, Office 365, andere Cloud-Anbieter und SaaS-Anwendungen.

Eindämmungsstrategien stärken die Sicherheit, indem sie die Anzahl und Art der Kontrollen erhöhen, die einen Angreifer davon abhalten, auf sensible Daten zuzugreifen. Das in diesem Artikel beschriebene Modell verwendet ein Design mit mehrstufigen Berechtigungen und schränkt Administratorrechte auf bestimmte Geräte ein.

## <a name="supply-chain-management"></a>Lieferkettenverwaltung

Wesentlich für eine sichere Arbeitsstation ist eine Lieferkettenlösung, bei der Sie eine vertrauenswürdige Arbeitsstation verwenden, die als „root of trust“ (Vertrauensgrundlage) bezeichnet wird. Bei dieser Lösung wird als Vertrauensgrundlage die [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot)-Technologie genutzt. Um eine Arbeitsstation zu sichern, können Sie mit Autopilot OEM-optimierte Microsoft Windows 10-Geräte nutzen. Diese Geräte werden vom Hersteller in einem bekannt guten Zustand geliefert. Anstatt ein Reimaging bei einem potenziell unsicheren Gerät auszuführen, kann Autopilot ein Windows-Gerät in einen „geschäftsbereiten“ Zustand versetzen. Autopilot wendet Einstellungen und Richtlinien an, installiert Apps und ändert sogar die Edition von Windows 10. So kann Autopilot beispielsweise die Windows-Installation eines Geräts von Windows 10 Pro in Windows 10 Enterprise ändern, um erweiterte Funktionen zu nutzen.

![Ebenen von sicheren Arbeitsstationen](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Geräterollen und -profile

Diese Anleitung verweist auf mehrere Sicherheitsprofile und -rollen, die Ihnen helfen können, sicherere Lösungen für Benutzer, Entwickler und IT-Mitarbeiter zu erstellen. Diese Profile wägen Benutzerfreundlichkeit und Risiken für normale Benutzer gegeneinander ab, die so von einer verbesserten oder sicheren Arbeitsstation profitieren können. Die hier angebotenen Einstellungskonfigurationen basieren auf branchenüblichen Standards. Diese Anleitung zeigt, wie Sie Windows 10 absichern und die mit Geräte- oder Benutzergefährdungen verbundenen Risiken reduzieren können. Dies geschieht durch den Einsatz von Richtlinien und Technologien, um die Verwaltung von Sicherheitsmerkmalen und -risiken zu unterstützen.
![Ebenen von sicheren Arbeitsstationen](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Niedrige Sicherheit**: Eine verwaltete Standardarbeitsstation bietet einen guten Ausgangspunkt für die meisten Privatanwender und kleine Unternehmen. Diese Geräte sind in Azure AD registriert und werden mit Intune verwaltet. Das Profil ermöglicht Benutzern das Ausführen von beliebigen Anwendungen und das Surfen zu beliebigen Websites. Es sollte eine Antischadsoftware-Lösung wie [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) aktiviert werden.

* **Erweiterte Sicherheit**: Diese geschützte Einstiegslösung ist geeignet für Privatanwender, kleine Unternehmen und allgemeine Entwickler.

   Die erweiterte Arbeitsstation ist eine richtlinienbasierte Möglichkeit, die Sicherheit des niedrigen Sicherheitsprofils zu erhöhen. Sie bietet eine sichere Möglichkeit für das Arbeiten mit Kundendaten und die gleichzeitige Nutzung von Produktivitätstools wie E-Mail und Webbrowsing. Sie können Überwachungsrichtlinien und Intune verwenden, um eine erweiterte Arbeitsstation auf Benutzerverhalten und Profilnutzung zu überwachen. Sie stellen das erweiterte Arbeitsstationsprofil mit dem Skript „Windows10 (1809)“ bereit. Dadurch kommen mit [Advanced Threat Protection (ATP)](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) die Vorteile des erweiterten Antischadsoftwareschutzes zur Anwendung.

* **Hohe Sicherheit**: Das effektivste Mittel zum Verringern der Angriffsfläche einer Arbeitsstation besteht darin, für die Arbeitsstation die Möglichkeit zur selbstständigen Verwaltung zu beseitigen. Das Entfernen lokaler Administrationsrechte ist ein Schritt, der die Sicherheit verbessert, kann aber bei falscher Implementierung die Produktivität beeinträchtigen. Das Hochsicherheitsprofil baut mit einer wesentlichen Änderung, nämlich dem Wegfall des lokalen Administrators, auf dem erweiterten Sicherheitsprofil auf. Dieses Profil richtet sich an wichtige Benutzer wie Führungskräfte, Lohnbuchhaltung und Nutzer sensibler Daten, genehmigende Personen für Dienstleistungen und Prozesse.

   Der Hochsicherheitsbenutzer verlangt nach einer kontrollierteren Umgebung und kann gleichzeitig Aktivitäten wie E-Mail und Webbrowsing in einer benutzerfreundlichen Umgebung durchführen. Die Benutzer erwarten, dass Features wie Cookies, Favoriten und andere Verknüpfungen reibungslos funktionieren. Diese Benutzer benötigen jedoch möglicherweise nicht die Möglichkeit, ihr Gerät zu modifizieren oder zu debuggen. Sie müssen auch keine Treiber installieren. Das Hochsicherheitsprofil wird mit dem Skript „High Security – Windows10 (1809)“ bereitgestellt.

* **Spezialisiert**: Entwickler und IT-Administratoren sind ein attraktives Ziel für Angreifer, da sich mit diesen Rollen Systeme im Sinne des Angreifers ändern lassen. Die spezialisierte Arbeitsstation erweitert die Richtlinien der Hochsicherheitsarbeitsstation, indem sie lokale Anwendungen verwaltet und Websites begrenzt. Sie schränkt auch risikoreiche Produktivitätsfunktionen wie ActiveX, Java, Browser-Plugins und andere Windows-Steuerelemente ein. Sie stellen dieses Profil mit dem Skript „DeviceConfiguration_NCSC - Windows 10 (Version 1803) SecurityBaseline“ bereit.

* **Geschützt**: Ein Angreifer, der ein administratives Konto gefährdet, kann durch Datendiebstahl, Datenänderung oder Dienstunterbrechung erheblichen geschäftlichen Schaden verursachen. In diesem geschützten Zustand ermöglicht die Arbeitsstation alle Sicherheitskontrollen und Richtlinien, die die direkte Kontrolle der lokalen Anwendungsverwaltung einschränken. Eine geschützte Arbeitsstation verfügt über keine Produktivitätstools, so dass das Gerät schwieriger zu kompromittieren ist. Sie blockiert den häufigsten Vektor für Phishing-Angriffe: E-Mail und Social Media.  Die geschützte Arbeitsstation kann mit dem Skript „Secure Workstation – Windows10 (1809) SecurityBaseline“ bereitgestellt werden.

   ![Geschützte Arbeitsstation](./media/concept-azure-managed-workstation/secure-workstation.png)

   Eine geschützte Arbeitsstation bietet einem Administrator eine abgesicherte Arbeitsstation mit klarer Anwendungskontrolle und Anwendungsschutz. Die Arbeitsstation verwendet Credential Guard, Device Guard und Exploit Guard, um den Host vor bösartigem Verhalten zu schützen. Außerdem sind alle lokalen Festplatten mit BitLocker verschlüsselt.

* **Isoliert**: Dieses benutzerdefinierte Offline-Szenario stellt das äußerste Ende des Spektrums dar. Für diesen Fall gibt es keine Installationsskripts. Möglicherweise müssen Sie eine geschäftskritische Funktion verwalten, die ein nicht unterstütztes oder nicht gepatchtes Legacy-Betriebssystem erfordert, zum Beispiel eine hochwertige Produktionslinie oder ein Lebenserhaltungssystem. Da die Sicherheit von großer Bedeutung ist und Clouddienste nicht verfügbar sind, können Sie diese Computer manuell oder mit einer isolierten Active Directory-Gesamtstruktur wie Enhanced Security Admin Environment (ESAE) verwalten und aktualisieren. Erwägen Sie unter diesen Umständen, alle Zugriffe mit Ausnahme der grundlegenden Intune- und ATP-Integritätsprüfungen zu entfernen.

  * [Anforderungen und Bandbreite: Intune-Netzwerkkonfiguration](https://docs.microsoft.com/intune/network-bandwidth-use)
  * [Konfigurieren von Endpunktproxy- und Internetkonnektivitätseinstellungen für Ihren Azure ATP-Sensor](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen einer sicheren, von Azure verwalteten Arbeitsstation](howto-azure-managed-workstation.md)
