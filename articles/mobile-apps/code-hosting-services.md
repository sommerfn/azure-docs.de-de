---
title: Hosten des Quellcodes Ihrer mobilen Anwendung in der Cloud mit GitHub und Azure DevOps
description: Erfahren Sie mehr über die Dienste zum Hosten des Codes Ihrer mobilen Anwendungen mit Microsoft-Diensten.
author: elamalani
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: f0af2ee5e62bbdbfb5d18cffc3e3ed62edec81f3
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795126"
---
# <a name="cloud-hosted-source-code-management-services"></a>In der Cloud gehostete Quellcode-Verwaltungsdienste
Wenn Sie Entwicklungsteams haben, in denen mehrere Teammitglieder an derselben Codebasis arbeiten, müssen Sie einen geeigneten Speicherort zum Hosten Ihres Codes finden. Speichern der Daten in der Cloud und Verwenden eines zentralisierten Repositorys ermöglicht es allen Beteiligten, die Codedateien hochladen, bearbeiten und verwalten zu können sowie mit anderen Entwicklern gemeinsam an Projekten zu arbeiten, und eröffnet die Möglichkeit, unabhängig davon, wo Sie sich befinden, jederzeit auf den Code zugreifen zu können, sofern Sie über eine Internetverbindung verfügen.

Hosten in der Cloud ist wesentlich einfacher als lokale Optionen, da es weniger Hardwarekonfiguration erfordert und Organisationen in die Lage versetzt, den Implementierungsprozess auf flexiblere Weise vorzunehmen.

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>Vorteile des Hostens von Quellcode in der Cloud
- **Zentralisierter Cloudspeicher**, um Ihre Daten von beliebiger Stelle aus anzuzeigen und zu verwalten.
- Ermöglicht **bessere Zusammenarbeit und konsistenteren Code** innerhalb von Teams, um Code nachzuverfolgen und Projekte zu verwalten, damit Continuous Delivery von exzellenter Software sichergestellt ist.
- **Beteiligen Sie sich**, und erhalten Sie die Möglichkeit, zu Ihren Projekten **auf einfache Weise beizutragen**.
- **Kürzerer Freigabezyklus**, weil Ihr Team schneller arbeiten kann und in die Lage versetzt wird, zu Ihren Projekten **problemlos beitragen** zu können.
- **Verringern der Kosten**, ohne sich Gedanken über die Wartung eigener Hardware, Server, VPN usw. machen zu müssen.

Verwenden Sie die folgenden Dienste, um Ihre Anwendungsdaten in der Cloud zu hosten.

## <a name="github"></a>GitHub
[GitHub](https://github.com/) ist ein Open-Source-Repository-Hostingdienst, der Quellcodeprojekte in unterschiedlichen Programmiersprachen hostet und die verschiedenen bei jeder Iteration vorgenommenen Änderungen nachverfolgt.

**Wichtige Features**
- **Hosten von Code** mit Ihrem gesamten Code an einem Ort. Alle Repositorys, egal ob privat, öffentlich oder Open Source, sind mit Tools ausgestattet, mit denen Sie Code hosten, versionieren und freigeben können.
- Nahtloser **Code Review** und integrierte Reviewtools bedingen, dass Codeüberprüfung ein wesentlicher Bestandteil jedes in einem Team erledigten Prozesses ist.
    - Schützen Sie Verzweigungen, schlagen Sie Änderungen vor, und fordern Sie Überprüfungen an.
    - Sehen Sie sich die Unterschiede und Kommentare im Kontext an, und erhalten Sie klares Feedback.
- Koordinieren Sie frühzeitig, bleiben Sie Ihrer Linie treu, und schaffen Sie mehr mit den **Projektverwaltungstools** von GitHub.
    - Sehen Sie das große Bild des Projekts.
    - **Task Boards**, die sich unmittelbar neben Ihrem Code in GitHub befinden.
    - **Ablegen von Karten über Drag & Drop**, damit Sie Teammitgliedern Probleme oder Pull Requests zuweisen können.
    - **Meilensteine** zum Organisieren und Nachverfolgen des Fortschritts.
    - **Hinweise**, um Ideen zu erfassen, die nützlich sein können, aber nicht zu einem bestimmten Problem oder Pull Request gehören.
- Einfaches Erkennen und Auswählen der richtigen Tools für eine bessere Kommunikation und Automatisierung der Arbeit, indem Sie Anwendungen über **[GitHub Marketplace](https://github.com/marketplace)** kaufen.
- **Teammanagement**, um Teams einfach zu verwalten und zu erweitern, **Benutzerrollen**, um Teams und Zugriffsberechtigungen zu verwalten, Tools für **Diskussionsthreads**, um Konversationen themen- und teambezogen zu führen und **Communityrichtlinien**, um neue Teammitglieder schnell mit einem Konto einzurichten.
- **Durchsuchen** und **Favorisieren** beliebter Projekte, um diesen zu folgen.
- **Zusammenarbeit mit und Lernen** von anderen Personen in der Branche.

**Referenzen**
- [GitHub](https://github.com/)
- [GitHub Guides](https://guides.github.com/)
- [GitHub Community Forum](https://github.community/)
- [GitHub Marketplace](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps
[Azure DevOps](https://azure.microsoft.com/services/devops/) unterstützt [Azure Repos](https://azure.microsoft.com/services/devops/repos/) und [Team Foundation-Versionskontrolle (TFVC)](https://docs.microsoft.com/azure/devops/repos/tfvc/index?view=azure-devops) als Quellcodeverwaltungsoptionen. Azure DevOps hat unbegrenzt viele kostenlose private Repositorys mit kollaborativen Codeüberprüfungen (Code Reviews), erweiterte Dateiverwaltung, Codesuche und Branchrichtlinien, um qualitativ hochwertigen Code sicherzustellen. Azure Repos eignet sich hervorragend für kleine Projekte sowie für große Organisationen, die native AAD-Unterstützung und erweiterte Richtlinien benötigen.
    
**Wichtige Features**
- **Unbegrenztes, in der Cloud gehostetes Git-Quellcoderepository** für ihre öffentlichen und privaten Repositorys
    - Unterstützung aller Git-Clients
    - Webhooks und API-Integration
- **Starten Ihres nächsten Builds** aus einem Repos-Pull Request
    - Zusammenarbeit, um besseren Code zu erstellen, indem threadgeführte Diskussionen und Continuous Integration verwendet werden
    - Einrichten von Continuous Integration/Continuous Delivery (CI/CD), um automatisch Builds, Tests und Bereitstellungen mit jedem abgeschlossenen Pull Request über Azure Pipelines oder Ihre Tools auszulösen
    - Schutz der Codequalität durch Branchrichtlinien
- **Zentrale Versionskontrolle mit Team Foundation-Versionskontrolle**, einschließlich Code Review
- **Verbindung zu Ihrem Code** über Xcode, Eclipse, IntelliJ, Android Studio, Visual Studio, Visual Studio Code usw.
- **Leistungsstarke semantische Codesuche**
- **Unternehmensgeeignet**, weil es eine native Integration in Azure Active Directory (AD) hat, wodurch das Verwalten des Zugriffs auf Ihre Coderepositorys vereinfacht wird
- **Sicherstellen der Codequalität** mit Branchrichtlinien, etwa Mindestanzahl von Code Reviews, Erfordern erfolgreicher Builds und Erzwingen von Git-Zusammenführungsstrategien
- **Verbinden mit der bevorzugten Entwicklungsumgebung**, um auf Repositorys zuzugreifen und die Arbeit zu verwalten

**Referenzen**
- [Erste Schritte mit Azure Repos](https://azure.microsoft.com/services/devops/repos/) 
- [Dokumentation zu Azure Repos](/azure/devops/repos/?view=azure-devops)