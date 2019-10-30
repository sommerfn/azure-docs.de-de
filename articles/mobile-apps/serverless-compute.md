---
title: Erstellen eines serverlosen Back-Ends für eine mobile Anwendung mit Azure Functions und anderen Diensten
description: Erfahren Sie mehr über die Compute-Dienste, um ein robustes serverloses Back-End für eine mobile Anwendung zu erstellen.
author: elamalani
manager: elamalani
ms.service: vs-appcenter
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: b8378be675b86ada10b8442c40a54b635ee115a9
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795198"
---
# <a name="build-mobile-backend-components-with-compute-services"></a>Erstellen von mobilen Back-End-Komponenten mit Compute-Diensten
Jede mobile Anwendung benötigt ein Back-End, das für die Datenspeicherung, Geschäftslogik und Sicherheit zuständig ist. Das Verwalten der Infrastruktur, um Back-End-Code zu hosten und auszuführen, erfordert, dass Sie eine Reihe von Servern dimensionieren, bereitstellen und skalieren, Betriebssystemupdates und betroffene Hardwarekomponenten verwalten, Sicherheitspatches anwenden und schließlich alle diese Infrastrukturkomponenten hinsichtlich Leistung, Verfügbarkeit und Fehlertoleranz überwachen. An diesem Punkt erweist sich die serverlose Architektur als nützlich, da Entwickler weder Server noch ein Betriebssystem noch zugehörige Software-/Hardwareupdates verwalten müssen. Dadurch werden viel Entwicklerzeit und viele Entwicklerkosten gespart, was eine schnellere Markteinführungszeit und eine Fokussierung auf das Entwickeln von Anwendungen bedeutet.

## <a name="benefits-of-compute"></a>Die Vorteile von Compute
- Abstraktion von Servern: Es ist nicht erforderlich, sich Gedanken über Hosting, Patching und Sicherheit zu machen, sodass sich Entwickler einfach auf den Code konzentrieren können.
- Sofortige und effiziente Skalierung stellt sicher, dass Ressourcen automatisch oder nach Bedarf in jeder benötigten Größenordnung bereitgestellt werden.
- Hochverfügbarkeit und Fehlertoleranz.
- Durch Mikroabrechnung ist sichergestellt, dass für Sie nur dann Kosten anfallen, wenn Ihr Code tatsächlich ausgeführt wird.
- Schreiben Sie Code, der in der Cloud ausgeführt wird, in der von Ihnen gewünschten Sprache.

Verwenden Sie die folgenden Dienste, um serverlose Compute-Funktionalität in ihren mobilen Apps zu ermöglichen.

## <a name="azure-functions"></a>Azure-Funktionen
[Azure Functions](https://azure.microsoft.com/services/functions/) ist eine ereignisgesteuerte Computelösung, in der Sie Code ausführen können, der in einer Programmiersprache Ihrer Wahl geschrieben wurde, ohne dass Sie sich Gedanken über Server machen müssen. Entwickler müssen die Anwendung oder die Infrastruktur nicht verwalten, um sie auf ihr auszuführen. Functions wird nach Bedarf skaliert, und bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird. Azure Functions sind eine großartige Möglichkeit, eine API für eine mobile Anwendung zu implementieren, da sie sehr einfach zu implementieren und zu warten und über HTTP zugänglich sind.

**Wichtige Features**
- **Ereignisgesteuert und skalierbar**, wobei Entwickler **Trigger und Bindungen** verwenden können, um zu definieren, wann eine Funktion aufgerufen und mit welchen Daten sie verbunden wird.
- **Eigene Abhängigkeiten** : Functions unterstützt NuGet und NPM, sodass Sie Ihre bevorzugten Bibliotheken verwenden können.
- **Integrierte Sicherheit** ermöglicht es Ihnen, per HTTP ausgelöste Funktionen mit OAuth-Anbietern wie Azure Active Directory, Facebook, Google, Twitter und Microsoft-Konto zu schützen.
- **Vereinfachte Integration** mit unterschiedlichen [Azure-Diensten](/azure/azure-functions/functions-overview#integrations) und SaaS-Angeboten (Software-as-a-Service).
- **Flexible Entwicklung** ermöglicht es Ihnen, Ihre Funktionen direkt im Portal zu codieren oder Continuous Integration einzurichten und Ihren Code über GitHub, Azure DevOps Services und andere unterstützte Entwicklungstools bereitzustellen.
- Die Functions-Runtime ist **Open Source**-Software und auf [GitHub](https://github.com/azure/azure-webjobs-sdk-script) verfügbar.
-  **Verbesserte Entwicklungsoberfläche**, in der Entwickler lokal mit ihrem bevorzugten Editor oder einer benutzerfreundlichen Webschnittstelle codieren, testen und debuggen können, samt Überwachung mit integrierten Tools und DevOps-Funktionen.
- **Eine Vielzahl von Programmiersprachen und Hostingoptionen**: Entwickeln Sie mit C#, Node.js, Java, Javascript oder Python.
- **Preismodell mit nutzungsbasierter Bezahlung** : Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird.

**Referenzen**
- [Azure-Portal](https://portal.azure.com)
- [Documentation](/azure/azure-functions/)
- [Azure Functions: Entwicklerhandbuch](/azure/azure-functions/functions-reference)
- [Schnellstarts](/azure/azure-functions/functions-create-first-function-vs-code)
- [Beispiele](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="app-service"></a>App Service
[Azure App Service](https://azure.microsoft.com/services/app-service/) ermöglicht das Erstellen und Hosten von Web-Apps und RESTful-APIs in der Programmiersprache Ihrer Wahl, ohne eine Infrastruktur verwalten zu müssen. Der Dienst bietet automatische Skalierung und Hochverfügbarkeit, unterstützt Windows und Linux und ermöglicht automatisierte Bereitstellungen über GitHub, Azure DevOps oder ein anderes beliebiges Git-Repository.

**Wichtige Features**
- **Mehrere Sprachen und Frameworks**: Unterstützung für ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP oder Python. Sie können PowerShell- und weitere Skripts oder ausführbare Dateien auch als Hintergrunddienst ausführen.
- **DevOps-Optimierung**: Richten Sie mit Azure DevOps, GitHub, BitBucket, Docker Hub oder Azure Container Registry die fortlaufende Integration und Bereitstellung ein. Verwalten Sie Ihre Apps in App Service mithilfe von Azure PowerShell oder der plattformübergreifenden Befehlszeilenschnittstelle (Command-Line Interface, CLI).
- **Globale Skalierung mit Hochverfügbarkeit**, um manuell oder automatisch zentral hochzuskalieren oder zu erweitern.
- **Verbindungen mit SaaS-Plattformen und lokalen Daten**, um aus über 50 Connectors für Unternehmenssysteme (z. B. SAP), SaaS-Dienste (z. B. Salesforce) sowie Internetdienste (z. B. Facebook) zu wählen. Greifen Sie über Hybrid Connections und Azure Virtual Networks auf lokale Daten zu.
- **Sicherheit und Konformität**: Azure App Service ist ISO-, SOC- und PCI-konform. Authentifizieren Sie Benutzer mit Azure Active Directory oder mit Anmeldungen für soziale Netzwerke (Google, Facebook, Twitter und Microsoft). Erstellen Sie IP-Adresseinschränkungen, und verwalten Sie Dienstidentitäten.
- **Anwendungsvorlagen**, um im Azure Marketplace aus zahlreichen Anwendungsvorlagen wie WordPress, Joomla und Drupal zu wählen.
- **Visual Studio-Integration** mit dedizierten Tools in Visual Studio optimiert das Erstellen, Bereitstellen und Debuggen.

**Referenzen**
- [Azure-Portal](https://portal.azure.com/)
- [Documentation](/azure/app-service/)
- [Schnellstarts](/azure/app-service/app-service-web-get-started-dotnet)
- [Beispiele](/azure/app-service/samples-cli)
- [Lernprogramme](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)
[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) verwaltet Ihre gehostete Kubernetes-Umgebung und ermöglicht so die schnelle und einfache Bereitstellung und Verwaltung von Containeranwendungen – ganz ohne Kenntnisse im Zusammenhang mit Containerorchestrierung. Darüber hinaus übernimmt die Lösung die Betreuung laufender Vorgänge und Wartungsaufgaben für Sie, indem sie Ressourcen ganz nach Bedarf bereitstellt, aktualisiert und skaliert, ohne Ihre Anwendungen offline zu schalten. 

**Wichtige Features**
- **Einfaches Migrieren vorhandener Anwendungen** zu Containern und Ausführen in AKS.
- **Vereinfachen der Bereitstellung und Verwaltung** von auf Microservices basierenden Anwendungen.
- **Sichere DevOps für AKS**, um ein ausgewogenes Verhältnis zwischen Geschwindigkeit und Sicherheit zu erreichen und Code schneller bedarfsgerecht bereitzustellen.
- **Müheloses Skalieren mit AKS und ACI**, um Pods in ACI bereitzustellen, die in Sekunden starten.
- **Bereitstellung und Verwaltung von IoT-Geräten** nach Bedarf.
- **Trainieren von Machine Learning-Modellen** mit Tools wie TensorFlow und KubeFlow.

**Referenzen**
- [Azure-Portal](https://portal.azure.com/)
- [Documentation](/azure/aks/)
- [Schnellstarts](/azure/aks/kubernetes-walkthrough-portal)
- [Lernprogramme](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure Container Instances
[Azure Container Instances (ACI)](https://azure.microsoft.com/services/container-instances/) ist eine großartige Lösung für jedes Szenario, das für die Verwendung isolierter Container geeignet ist. Hierzu zählen unter anderem einfache Anwendungen, Aufgabenautomatisierung und Erstellungsaufträge. Entwickeln Sie Apps schnell, ohne virtuelle Computer zu verwalten.

**Wichtige Features**
- **Kurze Startzeiten**, weil ACI Container in Azure in Sekundenschnelle starten kann, ohne virtuelle Computer bereitstellen und verwalten zu müssen.
- **Öffentliche IP-Konnektivität und benutzerdefinierter DNS-Name**.
- **Sicherheit auf Hypervisorebene**, die gewährleistet, dass Ihre Anwendung in einem Container isoliert ist – genau wie bei einem virtuellen Computer.
- **Angepasste Größen** für optimale Nutzung, indem exakte Angaben für CPU-Kerne und Arbeitsspeicher möglich sind. Dank sekundengenauer Abrechnung können Sie Ihre Ausgaben auf der Grundlage Ihres tatsächlichen Bedarfs präzise optimieren.
- **Permanenter Speicher**, um den Zustand abzurufen und zu speichern. Damit bietet ACI direktes Einbinden von Azure Files-Freigaben.
- **Linux-und Windows-Container**, die mit derselben API geplant werden.

**Referenzen**
- [Azure-Portal](https://portal.azure.com/)
- [Documentation](/azure/container-instances/)
- [Schnellstarts](/azure/container-instances/container-instances-quickstart-portal)
- [Beispiele](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Lernprogramme](/azure/container-instances/container-instances-tutorial-prepare-app)