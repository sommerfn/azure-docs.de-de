---
title: Verstehen Sie sichere, von Azure verwaltete Arbeitsstationen- Azure Active Directory
description: Erfahren Sie mehr über sichere, von Azure verwaltete Arbeitsstationen und erfahren Sie, warum sie wichtig sind.
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
ms.openlocfilehash: 02a6ddef294c4872f2d7e50e8940ecbb4b4b7bc4
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491595"
---
# <a name="understand-secure-azure-managed-workstations"></a>Verstehen Sie sichere, von Azure verwaltete Arbeitsstationen

Gesicherte, isolierte Arbeitsstationen sind von entscheidender Bedeutung für die Sicherheit sensibler Rollen wie Administratoren, Entwickler und kritische Service-Betreiber. Wenn die Sicherheit der Client-Arbeitsstationen beeinträchtigt wird, können viele Sicherheitskontrollen und -zusicherungen fehlschlagen oder ineffektiv sein.

Dieses Dokument erklärt, was Sie für den Aufbau einer sicheren Arbeitsstation benötigen, die oft als Privileged Access Workstation (PAW) bezeichnet wird. Der Artikel enthält auch ausführliche Anweisungen zum Einrichten des ersten Sicherheitskontrollen. Diese Anleitung beschreibt, wie Cloud-basierte Technologie den Service verwalten kann. Es basiert auf Sicherheitsfunktionen, die in Windows 10RS5, Microsoft Defender Advanced Threat Protection (ATP), Azure Active Directory und Intune eingeführt wurden.

> [!NOTE]
> Dieser Artikel beschreibt das Konzept der einer sicheren Arbeitsstation und deren Bedeutung. Wenn Sie mit dem Konzept bereits vertraut sind und die Bereitstellung überspringen möchten, besuchen Sie [Deploy a Secure Workstation](https://docs.microsoft.com/azure/active-directory/devices/howto-azure-managed-workstation).

## <a name="why-secure-workstation-access-is-important"></a>Warum ein sicherer Zugriff auf die Arbeitsstation wichtig ist

Die schnelle Einführung von Cloud-Diensten und die Möglichkeit, von überall aus zu arbeiten, haben eine neue Ausbeutungsmethoden geschaffen. Durch die Ausnutzung schwacher Sicherheitskontrollen auf Geräten, auf denen Administratoren arbeiten, können Angreifer Zugriff auf privilegierte Ressourcen erhalten.

Privilegierte Missbrauchs- und Lieferkettenangriffe gehören zu den fünf häufigsten Methoden, mit denen Angreifer gegen Unternehmen vorgehen. Sie sind auch die am zweithäufigsten erkannte Taktik bei Vorfällen, die 2018 gemäß dem [Verizon Threat Report](https://enterprise.verizon.com/resources/reports/dbir/) und dem [Security Intelligence Report](https://aka.ms/sir) gemeldet wurden.

Die meisten Angreifer gehen wie folgt vor:

1. Aufklärung, um einen Weg hinein zu finden, oft branchenspezifisch.
1. Analyse zur Sammlung von Informationen und Identifizierung des besten Weges, um eine Arbeitsstation zu infiltrieren, die als minderwertig angesehen wird.
1. Hartnäckigkeit bei der Suche nach einer Möglichkeit zur [lateralen](https://en.wikipedia.org/wiki/Network_Lateral_Movement) Bewegung.
1. Exfiltration von vertraulichen und sensiblen Daten.

Während der Aufklärung infiltrieren Angreifer häufig Geräte, die als risikoarm oder unterbewertet erscheinen. Sie nutzen diese anfälligen Geräte, um eine Möglichkeit zur lateralen Bewegung zu finden und administrative Benutzer und Geräte aufzudecken. Nachdem sie Zugriff auf privilegierte Benutzerrollen erhalten haben, identifizieren Angreifer hochwertige Daten und filtern diese erfolgreich aus.

![Typisches Kompromittierungsmuster](./media/concept-azure-managed-workstation/typical-timeline.png)

Dieses Dokument beschreibt eine Lösung, die Ihre Geräte vor diesen lateralen Angriffen schützen kann. Die Lösung isoliert Management und Services von weniger wertvollen Produktivitätsgeräten und unterbricht die Kette, bevor das Gerät, das Zugriff auf sensible Cloud-Ressourcen hat, infiltriert werden kann. Die Lösung verwendet native Azure-Dienste, die Teil des Microsoft 365 Enterprise Stacks sind:

* Intune für die Geräteverwaltung und eine sichere Liste von Anwendungen und URLs
* Autopilot für die Einrichtung, Bereitstellung und Aktualisierung von Geräten
* Azure AD für Benutzerverwaltung, bedingten Zugriff und mehrstufige Authentifizierung
* Windows 10 (aktuelle Version) für Integritätsnachweis für Geräte und Benutzerfreundlichkeit
* Defender ATP für Cloud-basierten Endpunktschutz, -erkennung und -reaktion
* Azure AD PIM zur Verwaltung von Autorisierung und Just-in-time (JIT) privilegiertem Zugriff auf Ressourcen

## <a name="who-benefits-from-a-secure-workstation"></a>Wer profitiert von der Verwendung einer sicheren Arbeitsstation?

Alle Benutzer und Betreiber profitieren von der Verwendung einer sicheren Arbeitsstation. Ein Angreifer, der einem PC oder Gerät gefährdet, kann alle zwischengespeicherte Konten imitieren. Wenn Sie auf dem Gerät angemeldet sind, können sie auch Anmeldeinformationen und Token verwenden. Dieses Risiko macht es wichtig, Geräte zu sichern, die für privilegierte Rollen verwendet werden, einschließlich Administratorrechte. Geräte mit privilegierten Konten sind Ziele für laterale Angriffe und Berichtigungsausweitung. Diese Konten können für viele verschiedene Ressourcen verwendet werden, z. B.:

* Administratoren von lokalen und cloudbasierten Systemen
* Entwicklerarbeitsstationen für wichtige Systeme
* Administratoren von Social Media-Konten mit hohem Gefährdungsgrad
* Sehr sensible Arbeitsstationen, z. B. SWIFT-Bezahlterminals
* Arbeitsstationen, die Geschäftsgeheimnisse bewahren

Um das Risiko zu verringern, sollten Sie erhöhte Sicherheitskontrollen für privilegierte Arbeitsstationen implementieren, die diese Konten verwenden. Weitere Informationen finden Sie im Leitfaden zur Bereitstellung von [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2)-Funktionen, in der [Office 365-Roadmap](https://aka.ms/o365secroadmap) und in der [Securing Privileged Access-Roadmap ](https://aka.ms/sparoadmap)).

## <a name="why-use-dedicated-workstations"></a>Warum dedizierte Arbeitsstationen verwenden?

Es ist zwar möglich, ein bestehendes Gerät abzusichern, aber es ist besser, direkt mit einem sicheren Fundament zu beginnen. Um Ihr Unternehmen in die beste Position zu versetzen, um ein hohes Sicherheitsniveau aufrechtzuerhalten, beginnen Sie mit einem Gerät, von dem Sie wissen, dass es sicher ist, und implementieren Sie eine Reihe von bekannten Sicherheitskontrollen.

Eine wachsende Anzahl von Angriffsvektoren durch E-Mail und Webbrowsing macht es immer schwieriger, sicherzustellen, dass einem Gerät vertraut werden kann. Dieser Leitfaden geht davon aus, dass eine dedizierte Arbeitsstation von Standardproduktivität, Browsing und E-Mail isoliert ist. Die Entfernung von Produktivität, Webbrowsing und E-Mail von einem Gerät kann sich negativ auf die Produktivität auswirken. Diese Sicherung ist jedoch typischerweise für Szenarien akzeptabel, in denen die Jobaufgaben dies nicht explizit erfordern und das Risiko eines Sicherheitsvorfalls hoch ist.

> [!NOTE]
> Das Surfen im Internet bezieht sich hier auf den allgemeinen Zugriff auf beliebige Websites, was eine risikoreiche Aktivität darstellen kann. Ein solches Surfen unterscheidet sich deutlich von der Verwendung eines Webbrowsers für den Zugriff auf eine kleine Anzahl bekannter administrativer Websites für Dienste wie Azure, Office 365, andere Cloud-Anbieter und SaaS-Anwendungen.

Eindämmungsstrategien verschärfen die Sicherheit, indem sie die Anzahl und Art der Kontrollen erhöhen, die einen Angreifer davon abhalten, auf sensible Daten zuzugreifen. DDas in diesem Artikel beschriebene Modell verwendet ein mehrstufiges Berechtigungsdesign und beschränkt die Administratorrechte auf bestimmte Geräte.

## <a name="supply-chain-management"></a>Lieferkettenverwaltung

Wesentlich für eine sichere Arbeitsstation ist eine Lieferkettenlösung, bei der Sie eine vertrauenswürdige Arbeitsstation verwenden, die als „root of trust“ bezeichnet wird. Bei dieser Lösung wird für die Vertrauensgrundlage die [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot)-Technologie genutzt. Um eine Arbeitsstationzu sichern, können Sie mit Autopilot Microsoft OEM-optimierte Windows 10-Geräte nutzen. Diese Geräte werden vom Hersteller in einem bekannt guten Zustand geliefert. Anstatt ein Reimaging bei einem potenziell unsicheren Gerät auszuführen kann Autopilot ein Windows-Gerät in einen "geschäftsbereiten" Zustand versetzen. Es wendet Einstellungen und Richtlinien an, installiert Apps und ändert sogar die Edition von Windows 10. So kann Autopilot beispielsweise die Windows-Installation eines Geräts von Windows 10 Pro in Windows 10 Enterprise ändern, so dass es erweiterte Funktionen nutzen kann.

![Ebenen von sicheren Arbeitsstationen](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Geräterollen und -profile

Diese Anleitung verweist auf mehrere Sicherheitsprofile und -rollen, die Ihnen helfen können, sicherere Lösungen für Benutzer, Entwickler und IT-Mitarbeiter zu erstellen. DiDiese Profile gleichen Benutzerfreundlichkeit und Risiken für normale Benutzer aus, die so von eine verbesserten oder sicheren Arbeitsstation profitieren können. Die hier angebotenen Einstellungskonfigurationen basieren auf branchenüblichen Standards. Diese Anleitung zeigt, wie Sie Windows 10 absichern und die mit Geräte- oder Benutzergefährdungen verbundenen Risiken reduzieren können. Dies geschieht durch den Einsatz von Richtlinien und Technologien, um das Management von Sicherheitsmerkmalen und -risiken zu unterstützen.
![Ebenen von sicheren Arbeitsstationen](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Niedrige Sicherheit**: Eine verwaltete standardisierte Arbeitsstation bietet einen guten Ausgangspunkt für die meisten Heim- und Kleinunternehmen. Diese Geräte sind in Azure AD registriert und werden mit Intune verwaltet. Das Profil ermöglicht Benutzern das Ausführen von beliebigen Anwendungen und das Browsen auf beliebigen Websites. Eine Antischadsoftware-Lösung wie [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) sollte aktiviert werden.

* **Erweiterte Sicherheit**: Diese geschützte Einstiegslösung ist geeignet für Privatanwender, kleine Unternehmen und allgemeine Entwickler.

   Die erweiterte Arbeitsstation ist eine richtlinienbasierte Möglichkeit, die Sicherheit des niedrigen Sicherheitsprofils zu erhöhen. Es bietet eine sichere Möglichkeit, mit Kundendaten zu arbeiten und gleichzeitig Produktivitätstools wie E-Mail und Webbrowsing zu nutzen. Sie können Überwachungsrichtlinien und Intune verwenden, um eine erweiterte Arbeitsstation auf Benutzerverhalten und Profilnutzung zu überwachen. Sie stellen das erweiterte Arbeitsstationsprofil mit dem Skript Windows10 (1809) bereit, und es nutzt die Vorteile des erweiterten Malwareschutzes mit [Advanced Threat Protection (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp).

* **Hohe Sicherheit**: Das effektivste Mittel zum Verringern der Angriffsfläche einer Arbeitsstation besteht darin, für die Arbeitsstation die Möglichkeit zur selbstständigen Verwaltung zu beseitigen. Das Entfernen lokaler Administrationsrechte ist ein Schritt, der die Sicherheit verbessert, kann aber bei falscher Implementierung die Produktivität beeinträchtigen. Das Hochsicherheitsprofil baut auf dem erhöhten Sicherheitsprofil mit einer wesentlichen Änderung auf: dem Wegfall des lokalen Administrators. Dieses Profil richtet sich an wichtige Benutzer: Führungskräfte, Gehaltsabrechnung und Nutzer vertraulicher Daten, Genehmigende für Dienstleistungen und Prozesse.

   Der Hochsicherheitsbenutzer verlangt eine kontrolliertere Umgebung und kann gleichzeitig Aktivitäten wie E-Mail und Webbrowsing in einer benutzerfreundlichen Umgebung durchführen. Die Benutzer erwarten, dass Features, z. B. Cookies, Favoriten und andere Verknüpfungen, reibungslos funktionieren. Diese Benutzer benötigen jedoch möglicherweise nicht die Möglichkeit, ihr Gerät zu modifizieren oder zu debuggen. Sie müssen auch keine Treiber installieren. Das Hochsicherheitsprofil wird mit dem Skript Hochsicherheit – Windows10 (1809) bereitgestellt.

* **Spezialisiert**: Entwickler und IT-Administratoren sind ein attraktives Ziel für Angreifer, da sich mit diesen Rollen Systeme im Sinne des Angreifers ändern lassen. Die spezialisierte Arbeitsstation erweitert die Richtlinien der Hochsicherheitsarbeitsstation, indem sie lokale Anwendungen verwaltet und Websites begrenzt. Es beschränkt auch risikoreiche Produktivitätsfunktionen wie ActiveX, Java, Browser-Plugins und andere Windows-Steuerelemente. Sie stellen dieses Profil mit den DeviceConfiguration_NCSC - Skript für Windows 10 (Version 1803) SecurityBaseline bereit.

* **Geschützt** – Ein Angreifer, der ein administratives Konto gefährdet, kann erhebliche Geschäftsschäden durch Datendiebstahl, Datenänderung oder Dienstunterbrechung verursachen. In diesem gesicherten Zustand ermöglicht die Arbeitsstation alle Sicherheitskontrollen und Richtlinien, die die direkte Kontrolle des lokalen Anwendungsmanagements einschränken. Eine gesichertere Arbeitsstation hat keine Produktivitätstools, so dass das Gerät schwieriger zu kompromittieren ist. Es blockiert den häufigsten Vektor für Phishing-Angriffe: E-Mail und Social Media.  Die gesicherte Arbeitsstation kann mit dem Secure Workstation - Windows10 (1809) SecurityBaseline Skript bereitgestellt werden.

   ![Geschützte Arbeitsstation](./media/concept-azure-managed-workstation/secure-workstation.png)

   Eine sichere Arbeitsstation bietet einem Administrator eine abgesicherte Arbeitsstation mit klarer Anwendungskontrolle und Anwendungsschutz. Die Arbeitsstation verwendet Credential Guard, Device Guard und Exploit Guard, um den Host vor bösartigem Verhalten zu schützen. Alle lokalen Festplatten sind ebenfalls mit BitLocker verschlüsselt.

* **Isoliert** – Dieses benutzerdefinierte Offline-Szenario stellt das äußerste Ende des Spektrums dar. In diesem Fall gibt es keine Installationsskripts. Möglicherweise müssen Sie eine geschäftskritische Funktion verwalten, die ein nicht unterstütztes oder nicht gepatchtes Legacy-Betriebssystem erfordert. Zum Beispiel eine hochwertige Produktionslinie oder ein lebenserhaltendes System. Da die Sicherheit von großer Bedeutung ist und Cloud-Services nicht verfügbar sind, können Sie diese Computer manuell oder mit einer isolierten Active Directory-Waldarchitektur wie der Enhanced Security Admin Environment (ESAE) verwalten und aktualisieren. Erwägen Sie unter diesen Umständen, alle Zugriffe mit Ausnahme der grundlegenden Intune- und ATP-Integritätsüberprüfungen zu entfernen.

  * [Anforderungen und Bandbreite: Intune-Netzwerkkonfiguration](https://docs.microsoft.com/intune/network-bandwidth-use)
  * [Konfigurieren von Endpunktproxy- und Internetkonnektivitätseinstellungen für Ihren Azure ATP-Sensor](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Nächste Schritte

[Stellen Sie eine sichere, von Azure verwaltete Arbeitsstation bereit](howto-azure-managed-workstation.md).
