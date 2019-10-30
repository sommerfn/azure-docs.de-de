---
title: Automatisieren des Bereitstellens und Veröffentlichens Ihrer mobilen Anwendungen mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über die Dienste, z. B. App Center, mit denen Sie eine Continuous Delivery-Pipeline für Ihre mobilen Anwendungen einrichten können.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 3b8305d02345abe98b674a8ed61626eaf6b5528d
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795118"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Automatisieren des Bereitstellens und Veröffentlichens Ihrer mobilen Anwendungen mit Continuous Delivery-Diensten

Als Entwickler schreiben Sie Code und checken diesen in das Coderepository ein, aber die in das Repository eingecheckten Commits sind möglicherweise nicht immer konsistent. Arbeiten mehrere Entwickler an demselben Projekt, treten Probleme mit der Integration auf, und das Team kann in Situationen geraten, in denen die Dinge nicht funktionieren, sich mehr und mehr Fehler ansammeln und die Projektentwicklung verzögert wird. Entwickler müssen warten, bis der gesamte Softwarecode erstellt und getestet ist, um nach Fehlern zu suchen, und dies macht den Prozess langsam und weniger iterativ.

Mit **Continuous Delivery** automatisieren Sie die Bereitstellung und Veröffentlichung Ihrer mobilen Anwendungen, unabhängig davon, ob Sie die jeweilige Anwendung an eine Gruppe von Testern oder Mitarbeitern des Unternehmens (für Betatests) oder den App Store (für die Produktion) verteilen. Continuous Delivery macht Bereitstellungen weniger riskant, fördert schnelle Iterationen und ermöglicht es Ihnen, neue Änderungen kontinuierlich für Ihre Kunden zu veröffentlichen.

## <a name="distribute-application-binaries-to-beta-testers"></a>Verteilen von Anwendungsbinärdateien an Betatester
Das Ausführen des Betatests Ihrer mobilen Anwendung ist einer der entscheidenden Schritte während des Anwendungsentwicklungsprozesses. Es hilft, Fehler und Probleme in Ihrer Anwendung frühzeitig zu finden, und bringt das Feedback, mit dem sich die Qualität Ihrer Anwendung verbessern lässt, um sie für den Einsatz in der Produktion vorzubereiten.

Mit den folgenden Diensten können Sie eine Continuous Delivery-Pipeline in Ihren mobilen Apps aktivieren.

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Distribute](/appcenter/distribution/) ist ein Tool für Entwickler, um schnell Builds auf Endbenutzergeräten veröffentlichen zu können. Mit einer vollständigen Installationsportalumgebung ist Distribute nicht nur eine leistungsstarke Lösung für die Verteilung an Tester von Beta-Apps, sondern auch eine bequeme Alternative zur Verteilung über die öffentlichen App Stores. Entwickler können ihren Verteilungsworkflow mit App Center Build und der Integration in öffentliche App Stores noch weiter automatisieren.

**Wichtige Features**
- **Verteilen Sie Ihre App an Betatester und Benutzer**, und stellen Sie sicher, dass alle Tester die neueste Version Ihrer Anwendung haben.
- **Benachrichtigen Sie Tester über neue Releases**, ohne dass Tester den Downloadvorgang erneut durchlaufen müssen.
- **Verwalten Sie Verteilergruppen** für verschiedene Versionen Ihrer Anwendung.
- **Verteilung an Stores** 
    - [Apple](/appcenter/distribution/stores/apple)
    - [Google Play](/appcenter/distribution/stores/googleplay)
    - [Intune](/appcenter/distribution/stores/intune)
- **Plattformunterstützung**: iOS, Android, macOS, tvOS, Xamarin, React Native, Unity, Cordova.
- Registrieren Sie iOS-Geräte automatisch in Ihrem Bereitstellungsprofil.

**Referenzen**
- [Registrieren für App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit App Center Distribute](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) ist ein voll funktionsfähiger Continuous Integration- (CI-) und Continuous Delivery-Dienst (CD-Dienst), der mit Ihrem bevorzugten Git-Anbieter zusammenarbeitet und in die meisten wichtigen Clouddienste bereitstellen kann, so auch in Azure-Dienste. Sie können mit Ihrem Code auf GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud oder Azure Repos starten. Anschließend können Sie das Erstellen, Testen und Bereitstellen Ihres Codes für Microsoft Azure, Google Cloud Platform oder Amazon Web Services automatisieren.

**Wichtige Features**
- Vereinfachte aufgabenbasierte Umgebung für das Einrichten eines CI-Servers für **native (Android, iOS und Windows) und plattformübergreifende (Xamarin, Cordova und React Native) mobile Anwendungen**.
- **Alle Sprachen, Plattformen und Clouds**: Erstellen, Testen und Bereitstellen von Node.js-, Python-, Java-, PHP-, Ruby-, Go-, C/C++-, C#-, Android- und iOS-Apps. Nutzen Sie die parallele Ausführung unter Linux, macOS und Windows. Stellen Sie in Cloudanbietern wie Azure, AWS und GCP bereit. Verteilen Sie mobile Anwendungen über Betakanäle und App Stores.
- **Native Containerunterstützung**: Erstellen Sie neue Container im Handumdrehen, und stellen Sie diese in einer beliebigen Registrierung bereit. Stellen Sie Container auf unabhängigen Hosts oder Kubernetes bereit.
- **Erweiterte Workflows und Features**: Einfache Buildverkettung und mehrstufige Builds. Profitieren Sie von YAML-Unterstützung, Testintegration, Releasegates, Berichterstellung und vielem mehr.
- **Erweiterbar**: Verwenden Sie eine Reihe von Build-, Test-und Bereitstellungsaufgaben, die von der Community erstellt wurden – Hunderte von Erweiterungen von Slack bis SonarCloud. Sie haben sogar die Möglichkeit, aus anderen CI-Systemen, etwa Jenkins, bereitzustellen. Webhooks und Rest-APIs unterstützen Sie bei der Integration.
- **Kostenlose in der Cloud gehostete Builds** für öffentliche und private Repositorys.
- **Unterstützt die Bereitstellung über andere Cloudanbieter** wie AWS, GCP usw.

**Referenzen**
- [Erste Schritte mit Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Erste Schritte mit Azure DevOps](https://app.vsaex.visualstudio.com/signup/) 
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Verteilen Ihrer Anwendung direkt an App Stores
Sobald Ihre Anwendung für die Verwendung in der Produktionsumgebung bereit ist und öffentlich verwendet werden soll, muss Sie an App Stores übermittelt werden, wo Sie von Kunden heruntergeladen werden kann. Es gibt mehrere Möglichkeiten, Ihre Anwendung direkt an App Stores zu verteilen. 

### <a name="visual-studio-app-center"></a>Visual Studio App Center
Mit dem [App Center Distribute](/appcenter/distribution/stores/)-Dienst können Sie Ihre mobilen Anwendungen direkt in App Stores veröffentlichen. Sobald Ihre Anwendung für den Download durch Endbenutzer bereit ist, können Sie Ihre Anwendungsbinärdateien direkt aus dem App Center-Portal veröffentlichen.  

Sie können direkt an folgende Ziele verteilen:
- [Apple App Store](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)
    
### <a name="apple-app-store"></a>Apple App Store
Der von Apple entwickelte und betreute App Store ermöglicht Benutzern das Suchen nach und Herunterladen von Anwendungen, die für iOS-, MacOS-, WatchOS- oder tvOS-Geräte entwickelt wurden. Entwickler müssen ihre iOS-Apps, wenn sie öffentlich verwendet werden sollen, an Apple Store übermitteln.

### <a name="google-play"></a>Google Play

Google Play ist der offizielle App Store für Android-Apps und ermöglicht Benutzern das Suchen nach und Herunterladen von Anwendungen, die für Android-Geräte entwickelt und über Google veröffentlicht wurden.

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) ist ein cloudbasierter Dienst im Mobilitätsmanagementbereich für Unternehmen, der es Ihren Mitarbeitern ermöglicht, produktiv zu sein, während sie Ihre Unternehmensdaten geschützt halten. Mit Intune haben Sie folgende Möglichkeiten: 
- Verwalten der mobilen Geräte, mit denen Ihre Mitarbeiter auf Unternehmensdaten zugreifen
- Verwalten der mobilen Anwendungen, die Ihre Mitarbeiter verwenden
- Schützen Ihrer Unternehmensdaten, indem die Art und Weise kontrolliert wird, wie Ihre Mitarbeiter auf die Daten zugreifen und diese freigeben
- Sicherstellen, dass Geräte und Anwendungen mit den Sicherheitsanforderungen des Unternehmens kompatibel sind
    
## <a name="deploy-updates-directly-to-users-devices"></a>Bereitstellen von Updates direkt auf den Geräten der Benutzer

### <a name="codepush"></a>CodePush
Der [CodePush](/appcenter/distribution/codepush/)-Dienst in App Center ermöglicht es Apache Cordova- und React Native-Entwicklern, Updates für mobile Anwendungen direkt auf den Geräten ihrer Benutzer bereitzustellen. Der Dienst fungiert dazu als zentrales Repository, in dem Entwickler bestimmte Updates veröffentlichen können (z. B. JS-, HTML-, CSS- und Bildänderungen). Anwendungen können das Repository dann über die bereitgestellten Client-SDKs auf Updates abfragen. Dies ermöglicht Ihnen ein besser bestimmbares und direkteres Interaktionsmodell für Ihre Endbenutzer, wenn Sie Fehler beheben oder kleine Funktionen hinzufügen, ohne dass Sie eine Binärdatei neu erstellen oder diese an alle öffentlichen App-Stores neu verteilen müssen.

**Wichtige Features**
- Cordova- und React Native-Entwickler erhalten die Möglichkeit, Updates für mobile Anwendungen direkt auf den Geräten ihrer Benutzer bereitzustellen, ohne eine Veröffentlichung über einen Store vorzunehmen.
- Dies ist nützlich für das Beheben von Fehlern oder Hinzufügen/Entfernen von kleinen Funktionen, für die kein erneutes Erstellen der Binärdateien und eine erneute Veröffentlichung über die entsprechenden Stores erforderlich ist.

**Referenzen**
- [Registrieren für App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit CodePush in App Center](/appcenter/distribution/codepush/)
- [CodePush-CLI](/appcenter/distribution/codepush/cli)