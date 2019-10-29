---
title: Informationen zu Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Lab Services das Erstellen, Verwalten und Schützen von Labs mit virtuellen Computern vereinfachen können, die von Entwicklern, Testern, Lehrkräften, Kursteilnehmern und anderen genutzt werden können.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: 115320a8b4ee7afc6e420dcfa96612b91ea6d1a0
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790770"
---
# <a name="an-introduction-to-azure-lab-services"></a>Einführung in Azure Lab Services
Es gibt zwei Dienste in Azure, mit denen Laborumgebungen in der Cloud eingerichtet werden können. 

- **Azure DevTest Labs:** Mit diesem Dienst können Sie schnell eine Umgebung für Ihr Team (z. B. eine Entwicklungsumgebung oder Testumgebung in der Cloud) einrichten. Der Besitzer des Labs erstellt ein Lab, stellt virtuelle Windows- oder Linux-Computer bereit, installiert die erforderlichen Programme und Tools und stellt sie Lab-Benutzern zur Verfügung. Labbenutzer verbinden sich mit virtuellen Computern (VMs) im Lab und nutzen sie für ihre tägliche Arbeit und kurzzeitige Projekte. Sobald Benutzer die Ressourcen im Lab nutzen, kann ein Lab-Administrator die Kosten und Nutzung mehrerer Labs analysieren und übergreifende Richtlinien festlegen, um die Kosten Ihrer Organisation oder Ihres Teams zu optimieren.
- **Azure Lab Services:** Dieser Dienst ermöglicht es Ihnen, verwaltete Labtypen zu erstellen. Aktuell sind Classroom-Labs die einzige Art von verwalteten Labs, die von Azure Lab Services unterstützt werden. Der Dienst selbst übernimmt die gesamte Infrastrukturverwaltung für einen verwalteten Lab-Typ – vom Hochfahren der VMs über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Nachdem ein IT-Administrator ein Labkonto in Azure Lab Services erstellt hat, kann ein Kursleiter schnell ein Lab für seinen Kurs einrichten, die Anzahl und den Typ der virtuellen Computer angeben, die für Übungen im Kurs erforderlich sind, und Benutzer zum Kurs hinzufügen. Wenn sich ein Benutzer für den Kurs registriert hat, kann er auf den virtuellen Computer zugreifen, um Übungen für den Kurs auszuführen.  

## <a name="key-capabilities"></a>Wichtige Funktionen

Diese Dienste (Azure DevTest Labs und Azure Lab Services) unterstützen die folgenden Schlüsselfunktionen/-features:

- **Schnelle und flexible Einrichtung eines Labs**. Mithilfe von Azure Lab Services können Lab-Besitzer schnell ein Lab ihren Anforderungen entsprechend einrichten. Der Dienst bietet die Möglichkeit, alle Aufgaben im Zusammenhang mit der Azure-Infrastruktur für verwaltete Lab-Typen zu übernehmen oder es den Besitzern von Labs zu überlassen, die Infrastruktur im Rahmen ihres Abonnements selbst zu verwalten und anzupassen. Der Dienst bietet eine integrierte Skalierung und Resilienz von Infrastruktur für Labs, die er für Sie verwaltet.
- **Vereinfachte Umgebung für Lab-Benutzer**. In einem verwalteten Lab-Typ, z. B. einem Classroom-Lab, können sich Lab-Benutzer mit einem Registrierungscode registrieren und jederzeit auf das Lab zugreifen, um dessen Ressourcen zu nutzen. In einem Lab, das in DevTest Labs erstellt wurde, kann ein Lab-Besitzer Lab-Benutzern die Berechtigung erteilen, virtuelle Computer zu erstellen und darauf zuzugreifen, Datenträger zu verwalten und wiederzuverwenden und wiederverwendbare Geheimnisse einzurichten.  
- **Kostenoptimierung und -analyse**. Ein Lab-Besitzer kann Lab-Zeitpläne festlegen, gemäß denen virtuelle Computer automatisch herunter- und hochgefahren werden. Der Lab-Besitzer kann in einem Zeitplan die Zeitfenster festlegen, in denen die virtuellen Computer des Labs für Benutzer zugänglich sind, zur Kostenoptimierung Nutzungsrichtlinien lab- oder benutzerbezogen festlegen, um die Kosten zu optimieren, und Nutzungs- und Aktivitätstrends in einem Lab analysieren. Für verwaltete Lab-Typen, z. B. Classroom-Labs, steht derzeit eine kleinere Teilmenge von Optimierungs- und Analyseoptionen zur Verfügung.
- **Integrierte Sicherheit**. Ein Lab-Besitzer kann ein privates virtuelles Netzwerk und ein Subnetz für ein Lab einrichten und eine gemeinsame öffentliche IP-Adresse aktivieren. Lab-Benutzer können über virtuelle Netzwerke, die mit ExpressRoute oder Site-to-Site-VPNs konfiguriert sind, auf sichere Weise auf Ressourcen zugreifen. (derzeit in nur DevTest Labs verfügbar)
- **Integration in Ihre Workflows und Tools**. Azure Lab Services ermöglicht Ihnen, die Labs in die Website- und Verwaltungssysteme Ihres Unternehmens zu integrieren. Mithilfe Ihrer CI/CD-Tools (Continuous Integration/Continuous Deployment) können Sie Umgebungen automatisch bereitstellen. (derzeit in nur DevTest Labs verfügbar)

> [!NOTE]
> Azure Lab Services unterstützt zurzeit nur virtuelle Computer, die aus Azure Marketplace-Images erstellt werden. Wenn Sie benutzerdefinierte Images verwenden oder andere PaaS-Ressourcen in einer Laborumgebung erstellen möchten, verwenden Sie DevTest Labs. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Images in DevTest Labs](devtest-lab-create-custom-image-from-vm-using-portal.md) und [Erstellen von Laborumgebungen mithilfe von Resource Manager-Vorlagen](devtest-lab-create-environment-from-arm.md).

## <a name="scenarios"></a>Szenarien

Hier sind einige der Szenarien aufgeführt, die Azure DevTest Labs und Azure Lab Services unterstützen:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Einrichtung eines Computerlabors mit anpassbarer Größe in der Cloud für Ihren Classroom  

- Erstellen Sie ein verwaltetes Classroom-Lab. Sie geben dem Dienst einfach genau das an, was Sie brauchen. Daraufhin wird die Infrastruktur des Labs für Sie erstellt und verwaltet, sodass Sie sich auf das Unterrichten konzentrieren können, ohne auf die technischen Details eines Labs achten zu müssen.
- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die genau für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von Stunden für die Nutzung der VMs für die Unterrichtsarbeit zu.  
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die Anzahl der VMs automatisch nur bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben.
- Löschen Sie das Lab mit einem einzigen Mausklick, sobald Sie fertig sind.

### <a name="use-devtest-labs-for-development-environments"></a>Verwenden von DevTest Labs für Entwicklungsumgebungen

Azure DevTest Labs kann zum Implementieren vieler wichtiger Szenarien verwendet werden. Eines der primären Szenarien stellt aber die Verwendung von DevTest Labs zum Hosten von Entwicklungscomputern für Entwickler dar. In diesem Szenario bietet DevTest Labs folgende Vorteile:

- Ermöglichen, dass Entwickler ihre Entwicklungscomputer nach Bedarf bereitstellen können.
- Bereitstellen von Linux- und Windows-Umgebungen mithilfe wiederverwendbarer Vorlagen und Artefakte.
- Entwickler können ihre Entwicklungscomputer bei Bedarf einfach anpassen.
- Administratoren können die Kosten kontrollieren, indem sie sicherstellen, dass Entwickler nicht mehr VMs erhalten, als sie für die Entwicklung benötigen, und VMs heruntergefahren werden, sobald sie nicht verwendet werden.

Weitere Informationen finden Sie unter [Verwenden von DevTest Labs für die Entwicklung](devtest-lab-developer-lab.md).

### <a name="use-devtest-labs-for-test-environments"></a>Verwenden von DevTest Labs für Testumgebungen

Mit Azure DevTest Labs lassen sich zahlreiche Schlüsselszenarien implementieren. Ein primäres Szenario ist jedoch das Hosten von Computern für Tester. In diesem Szenario bietet DevTest Labs folgende Vorteile:

- Tester können schnell Windows- und Linux-Umgebungen mit wiederverwendbaren Vorlagen und Artefakten bereitstellen und so die neueste Version ihrer Anwendung testen.
- Tester können ihre Auslastungstests durch Bereitstellen mehrerer Test-Agents zentral hochskalieren.
- Administratoren können die Kosten kontrollieren, indem sie sicherstellen, dass Tester nicht mehr VMs erhalten, als sie für Tests benötigen, und VMs heruntergefahren werden, sobald sie nicht verwendet werden.

Weitere Informationen finden Sie unter [Verwenden von DevTest Labs für Tests](devtest-lab-test-env.md).

## <a name="types-of-labs"></a>Arten von Labs
Sie können zwei Arten von Labs erstellen: **verwaltete Lab-Typen** mit Azure Lab Services und **Labs** mit Azure Lab Services. Wenn Sie lediglich Ihre Lab-Anforderungen eingeben und die Einrichtung und Verwaltung der dafür erforderlichen Infrastruktur dem Dienst überlassen möchten, können Sie einen der **verwalteten Lab-Typen** wählen. Derzeit können mit Azure Lab Services nur verwaltete Lab-Typen der Art **Classroom-Lab** erstellt werden. Wenn Sie Ihre eigene Infrastruktur verwalten möchten, können Sie mithilfe von **Azure DevTest Labs** ein Lab erstellen.

Ausführlichere Informationen zu diesen Labs finden Sie in den folgenden Abschnitten. 

## <a name="managed-lab-types"></a>Verwaltete Labtypen
Mit Azure Lab Services können Sie Labs erstellen, deren Infrastruktur von Azure verwaltet wird. In diesem Artikel werden sie als verwaltete Lab-Typen bezeichnet. Verwaltete Lab-Typen umfassen verschiedene Arten von Labs für Ihre individuellen Anforderungen. Derzeit wird das **Classroom-Lab** als einziger verwalteter Lab-Typ unterstützt. 

Verwaltete Lab-Typen zeichnen sich durch ihren geringen Einrichtungsaufwand aus und sind schnell einsatzbereit. Der Dienst übernimmt die gesamte Infrastrukturverwaltung für das Lab – vom Hochfahren der virtuellen Computer über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Wenn Sie einen verwalteten Lab-Typ, z. B. ein Classroom-Lab, erstellen möchten, müssen Sie zunächst ein Lab-Konto für Ihre Organisation erstellen. Das Lab-Konto fungiert als zentrales Konto, unter dem alle Labs der Organisation verwaltet werden. 

Wenn Sie Azure-Ressourcen unter diesen verwalteten Lab-Typen erstellen und verwenden, erstellt und verwaltet der Dienst Ressourcen in internen Microsoft-Abonnements. Sie werden nicht in Ihrem eigenen Azure-Abonnement erstellt. Der Dienst überwacht die Nutzung dieser Ressourcen in internen Microsoft-Abonnements. Diese Nutzung wird dann über Ihr Azure-Abonnement abgerechnet, dem das Lab-Konto angehört.   

Im Anschluss finden Sie einige **Anwendungsfälle für verwaltete Lab-Typen**: 

- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die exakt für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von VM-Stunden für Hausaufgaben oder persönliche Projekte zu.
- Richten Sie einen Pool mit hochleistungsfähigen virtuellen Computern für rechen- oder grafikintensive Untersuchungen ein. Führen Sie die virtuellen Computer nach Bedarf aus, und bereinigen Sie sie anschließend. 
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die VM-Anzahl automatisch bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben.  
- Stellen Sie schnell ein Lab mit virtuellen Computern für einen Hackathon bereit. Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 


## <a name="devtest-labs"></a>DevTest Labs
In bestimmten Szenarien kann es wünschenswert sein, die gesamte Infrastruktur und Konfiguration selbst innerhalb des eigenen Abonnements zu verwalten. Zu diesem Zweck können Sie über das Azure-Portal mithilfe von Azure DevTest Labs ein Lab erstellen. Für diese Labs muss kein Lab-Konto erstellt werden. Diese Labs werden nicht im Lab-Konto für die verwalteten Lab-Typen angezeigt.  

Im Anschluss finden Sie einige **Anwendungsfälle für DevTest Labs**: 

- Stellen Sie schnell ein Lab mit virtuellen Computern für einen Hackathon oder eine Praxiseinheit für eine Konferenz bereit. Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 
- Erstellen Sie einen Pool mit virtuellen Computern, die mit Ihrer Anwendung konfiguriert sind, und ermöglichen Sie Ihrem Team ganz einfach die Verwendung eines virtuellen Computers für die Fehlerbeseitigung.  
- Stellen Sie Entwicklern virtuelle Computer zur Verfügung, die mit allen benötigten Tools konfiguriert sind. Planen Sie das automatische Starten und Herunterfahren, um die Kosten zu minimieren. 
- Erstellen Sie im Rahmen Ihrer Bereitstellung wiederholt ein Lab mit Testcomputern. Testen Sie Ihre neuesten Komponenten, und bereinigen Sie die Testcomputer, wenn Sie sie nicht mehr benötigen. 
- Richten Sie mehrere unterschiedlich konfigurierte virtuelle Computer und mehrere Test-Agents für Skalierungs- und Leistungstests ein. 
- Verwenden Sie ein Lab, das mit der neuesten Version Ihres Produkts konfiguriert ist, um Schulungen für Kunden anzubieten. Geben Sie jedem Kunden ein begrenztes Zeitkontingent für die Lab-Verwendung. 


## <a name="managed-lab-types-vs-devtest-labs"></a>Verwaltete Labtypen im Vergleich zu DevTest Labs
In der folgenden Tabelle werden die beiden Arten von Labs verglichen, die von Azure Lab Services unterstützt werden: 

| Features | Verwaltete Labtypen | DevTest Labs |
| -------- | ----------------- | ---------- |
| Verwaltung der Azure-Infrastruktur im Lab |  Automatisch durch den Dienst | Manuelle Verwaltung  |
| Integrierte Resilienz gegen Infrastrukturprobleme | Automatisch durch den Dienst | Manuelle Verwaltung  |
| Abonnementverwaltung | Der Dienst übernimmt die Zuteilung von Ressourcen in Microsoft-Abonnements, die dem Dienst zugrunde liegen. Die Skalierung wird automatisch vom Dienst durchgeführt. | Die Verwaltung muss manuell in Ihrem eigenen Azure-Abonnement durchgeführt werden. Abonnements werden nicht automatisch skaliert. |
| Azure Resource Manager-Bereitstellung innerhalb des Labs | Nicht verfügbar | Verfügbar |

## <a name="next-steps"></a>Nächste Schritte

Entsprechende Informationen finden Sie in den folgenden Artikeln: 

- [Informationen zu Classroom-Labs](./classroom-labs/classroom-labs-overview.md)
- [Informationen zu DevTest Labs](devtest-lab-overview.md)
