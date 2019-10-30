---
title: Automatisieren des Lebenszyklus Ihrer Apps mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über die Dienste, z. B. App Center, mit denen Sie kontinuierliche Erstellung und Continuous Integration für Ihre mobilen Anwendungen einrichten können.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 07e8e0edc4638c6c0bf9acc205175aa731c0f8bc
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795102"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Automatisieren des Lebenszyklus Ihrer Apps mit kontinuierlicher Erstellung und Continuous Integration

Als Entwickler schreiben Sie Code und checken diesen in das Coderepository ein, aber die in das Repository eingecheckten Commits sind eventuell nicht immer konsistent. Arbeiten mehrere Entwickler an demselben Projekt, treten Probleme mit der Integration auf, und das Team kann in Situationen geraten, in denen die Dinge nicht funktionieren, sich mehr und mehr Fehler ansammeln und die Projektentwicklung verzögert wird. Entwickler müssen warten, bis der gesamte Softwarecode erstellt und getestet ist, um nach Fehlern zu suchen, und dies macht den Prozess langsam und weniger iterativ. 

Mit **kontinuierliche Erstellung und Continuous Integration** können Entwickler die Builderstellung und das Testen Ihres Codes vereinfachen, indem sie die Änderungen nicht nur an das Quellcoderepository übermitteln, sondern Tests und Überprüfungen auch in die Buildumgebung einbringen, sodass Tests immer mit ihrem Code ausgeführt werden. Alle am Quellcode vorgenommenen Änderungen werden kontinuierlich in den Build übernommen, sobald sie in das Repository committet wurden. Bei jedem Einchecken überprüft der CI-Server alle vom Entwickler erstellten Tests und führt sie aus. Werden die Tests nicht bestanden, wird der Code für weitere Änderungen zurückgesendet. Dies ermöglicht es dem Entwickler, auf ein Unterbrechen der Builds, die erstellt werden, zu verzichten sowie nicht alle Tests auf dem lokalen Computer ausführen zu müssen, was wiederum zu einer Steigerung der Produktivität des Entwicklers führt. 

## <a name="key-benefits"></a>Hauptvorteile
- **Automatisierte** Build-, Test- und Bereitstellungspipeline.
- Frühzeitiges **Erkennen von Fehlern und Beheben von Problemen**, um kürzere Veröffentlichungsintervalle sicherzustellen.
- Häufigeres **Committen von Code** und schnelles Erstellen von Anwendungen.
- **Flexibilität**, Code schnell ohne Probleme zu ändern.
- **Kürzere Markteinführungszeit** stellt sicher, dass nur Code mit guter Qualität den gesamten Prozess durchläuft.
- **Kleine Codeänderungen**, da die Möglichkeit besteht, kleine Codeelemente gleichzeitig zu integrieren.
- **Verbesserte Teamtransparenz und -verantwortlichkeit** führt dazu, dass Sie **kontinuierliches Feedback** nicht nur von ihren Kunden, sondern auch von Ihrem Team erhalten.

Mit den folgenden Diensten können Sie eine Continuous Integration-Pipeline in Ihren mobilen Apps aktivieren.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
Der [App Center Build](/appcenter/build/)-Dienst unterstützt Sie bei der Erstellung nativer und plattformübergreifender Anwendungen, an denen Ihr Team arbeitet, wozu eine sichere Cloudinfrastruktur verwendet wird. Sie können Ihr Repository problemlos in App Center verbinden und mit der Erstellung Ihrer App in der Cloud bei jedem Commit beginnen, ohne sich Gedanken über die lokale Konfiguration der Buildserver, komplizierte Konfigurationen und Code machen zu müssen, der nicht auf Ihrem Computer, sondern auf dem Computer eines Kollegen erstellt wird.

Dank der zusätzlichen Leistungsfähigkeit der App Center-Dienste können Sie Ihren Workflow weiter automatisieren. Mit App Center Distribute können Sie Builds automatisch für Tester und in öffentlichen App Stores veröffentlichen, oder mit App Center Test können Sie automatisierte UI-Tests für Tausende von echten Geräte- und Betriebssystemkonfigurationen in der Cloud ausführen.

**Wichtige Features**
- **Einrichten von Continuous Integration** in Minuten und häufigeres und schnelleres Erstellen von Anwendungen.
- Integration in **GitHub, Bitbucket, Azure DevOps und GitLab**.
- **Schnelle und sichere Builds** auf verwalteten, in der Cloud gehosteten Computern.
- **Aktivieren Sie für Ihre Builds das „Starten von Tests“** , und überprüfen Sie, ob die App auf echten iOS- und Android-Geräten erstellt wird.
- **Native Plattform- und plattformübergreifende Unterstützung**: iOS, Android, macOS, Windows, Xamarin, React Native.
- **Anpassen Ihrer Builds**, indem Postklonen-, Präbuild- und Postbuild-Skripts hinzugefügt werden.

**Referenzen**
- [Registrieren für App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit App Center Build](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines
 [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), ein Dienst in Azure DevOps, ist ein voll funktionsfähiger Continuous Integration- (CI-) und Continuous Delivery-Dienst (CD-Dienst), der mit Ihrem bevorzugten Git-Anbieter zusammenarbeitet und in die meisten wichtigen Clouddienste bereitstellen kann, so auch in Azure. Sie können mit Ihrem Code auf GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud oder Azure Repos starten. Anschließend können Sie das Erstellen, Testen und Bereitstellen Ihres Codes für Microsoft Azure, Google Cloud Platform oder Amazon Web Services automatisieren.

**Wichtige Features**
- Vereinfachte aufgabenbasierte Umgebung für das Einrichten eines CI-Servers für native (Android, iOS und Windows) und plattformübergreifende (Xamarin, Cordova und React Native) mobile Anwendungen zusätzlich zu Servertechnologien von Microsoft und anderen Anbietern (Node.js, Java).
- **Alle Sprachen, Plattformen und Clouds**: Erstellen, Testen und Bereitstellen von Node.js-, Python-, Java-, PHP-, Ruby-, Go-, C/C++-, C#-, Android- und iOS-Anwendungen. Nutzen Sie die parallele Ausführung unter Linux, macOS und Windows. Stellen Sie in Cloudanbietern wie Azure, AWS und GCP bereit. Verteilen Sie mobile Anwendungen über Betakanäle und App Stores.
- **Native Containerunterstützung**: Erstellen Sie neue Container im Handumdrehen, und stellen Sie diese in einer beliebigen Registrierung bereit. Stellen Sie Container auf unabhängigen Hosts oder Kubernetes bereit.
- **Erweiterte Workflows**: Einfache Buildverkettung und mehrstufige Builds. Profitieren Sie von YAML-Unterstützung, Testintegration, Releasegates, Berichterstellung und vielem mehr.
- **Erweiterbar**: Verwenden Sie eine Reihe von Build-, Test-und Bereitstellungsaufgaben, die von der Community erstellt wurden – Hunderte von Erweiterungen von Slack bis SonarCloud. Sie können sogar aus anderen CI-Systeme bereitstellen, z. B. Jenkins, und Webhooks und REST-APIs verwenden, um Sie bei der Integration zu unterstützen.
- **Kostenlose in der Cloud gehostete Builds** für öffentliche und private Repositorys.
- **Unterstützung für Bereitstellungen über andere Cloudanbieter** wie Amazon Web Services, Google Cloud Platform usw.

**Referenzen**
- [Erste Schritte mit Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Erste Schritte mit Azure DevOps](https://app.vsaex.visualstudio.com/signup/) 
- [Schnellstarts](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

Damit Sie den richtigen Dienst für Ihre Anwendungsbuilds auswählen, lesen Sie den Artikel, in dem [App Center Build und Azure Pipelines](/appcenter/build/choose-between-services) verglichen werden.
