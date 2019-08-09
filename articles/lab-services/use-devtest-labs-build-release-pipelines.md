---
title: Verwenden von DevTest Labs in Build- und Releasepipelines in Azure DevOps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure DevTest Labs in Build- und Releasepipelines in Azure DevOps verwenden.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2019
ms.author: spelluru
ms.openlocfilehash: 606563cd7d7adcdd79bf9561876eb0640fb68b21
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620622"
---
# <a name="use-devtest-labs-in-azure-devops-build-and-release-pipelines"></a>Verwenden von DevTest Labs in Build- und Releasepipelines in Azure DevOps
Dieser Artikel enthält Informationen dazu, wie DevTest Labs in Build- und Releasepipelines in Azure DevOps verwendet werden kann. 

## <a name="overall-flow"></a>Allgemeiner Ablauf
Der grundlegende Ablauf umfasst die Einrichtung einer **Buildpipeline**, über die die folgenden Aufgaben ausgeführt werden:

1. Erstellen des Anwendungscodes
1. Erstellen der Basisumgebung in DevTest Labs
1. Aktualisieren der Umgebung mit benutzerdefinierten Informationen
1. Bereitstellen der Anwendung in der DevTest Labs-Umgebung
1. Testen des Codes 

Nach dem erfolgreichen Erstellen des Builds verwendet die **Releasepipeline** die Buildartefakte für die Bereitstellung in der Staging- oder Produktionsumgebung. 

Eine der notwendigen Voraussetzungen besteht darin, dass alle Informationen, die zum Neuerstellen des getesteten Ökosystems erforderlich sind, innerhalb der Buildartefakte verfügbar sind, einschließlich der Konfiguration der Azure-Ressourcen. Da für die Verwendung von Azure-Ressourcen Kosten anfallen, möchten Unternehmen die Nutzung dieser Ressourcen kontrollieren oder nachverfolgen. In einigen Fällen können die zum Erstellen und Konfigurieren der Ressourcen verwendeten Azure Resource Manager-Vorlagen in einer anderen Abteilung verwaltet werden, beispielsweise der IT-Abteilung. Außerdem können diese Vorlagen in einem anderen Repository gespeichert werden. Dies kann zu dem interessanten Szenario führen, in dem ein Build erstellt und getestet wird und sowohl der Code als auch die Konfiguration innerhalb der Buildartefakte gespeichert werden müssen, damit das System in der Produktionsumgebung ordnungsgemäß neu erstellt wird. 

Durch Verwendung von DevTest Labs in der Build- und Testphase können Sie den Buildquellen Azure Resource Manager-Vorlagen und Unterstützungsdateien hinzufügen, sodass während der Releasephase die genaue zum Testen verwendete Konfiguration auch in der Produktionsumgebung bereitgestellt wird. Mit der Aufgabe **Create Azure DevTest Labs Environment** (Azure DevTest Labs-Umgebung erstellen) mit der richtigen Konfiguration werden die Resource Manager-Vorlagen in den Buildartefakten gespeichert. In diesem Beispiel verwenden Sie den Code aus [Tutorial: Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure App Service](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md) zum Bereitstellen und Testen der Web-App in Azure.

![Allgemeiner Ablauf](./media/use-devtest-labs-build-release-pipelines/overall-flow.png)

## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen
Verschiedene Elemente müssen im Voraus erstellt werden:

- Zwei Repositorys: Das erste mit dem Code aus dem Tutorial und einer Resource Manager-Vorlage mit zwei zusätzlichen virtuellen Computern. Das zweite enthält die Azure Resource Manager-Basisvorlage (vorhandene Konfiguration).
- Eine Ressourcengruppe für die Bereitstellung des Produktionscodes und der Produktionskonfiguration.
- Ein Lab muss mit einer [Verbindung mit dem Konfigurationsrepository](devtest-lab-create-environment-from-arm.md) für die Buildpipeline eingerichtet werden. Die Resource Manager-Vorlage muss als „azuredeploy.json“ mit „metadata.json“ im Konfigurationsrepository eingecheckt werden, damit die Vorlage in DevTest Labs erkannt und bereitgestellt werden kann.

Mit der Buildpipeline wird eine DevTest Labs-Umgebung erstellt und der Code zum Testen bereitgestellt.

## <a name="set-up-a-build-pipeline"></a>Einrichten einer Buildpipeline
Erstellen Sie in Azure DevOps eine Buildpipeline mit dem Code aus [Tutorial: Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure App Service](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md). Verwenden Sie die **ASP.NET Core**-Vorlage, über die die erforderliche Aufgabe zum Erstellen, Testen und Veröffentlichen des Codes gefüllt wird.

![Auswählen der ASP.NET-Vorlage](./media/use-devtest-labs-build-release-pipelines/select-asp-net.png)

Sie müssen drei weitere Aufgaben hinzufügen, um die Umgebung in DevTest Labs zu erstellen und die Bereitstellung in der Umgebung durchzuführen.

![Pipeline mit drei Aufgaben](./media/use-devtest-labs-build-release-pipelines/pipeline-tasks.png)

### <a name="create-environment-task"></a>Aufgabe zum Erstellen der Umgebung
Wählen Sie in der Aufgabe zum Erstellen der Umgebung (Aufgabe **Azure DevTest Labs Create Environment** (Azure DevTest Labs – Umgebung erstellen)) die folgenden Werte in den Dropdownlisten aus:

- Azure-Abonnement
- Name des Labs
- Name des Repositorys
- Name der Vorlage (die den Ordner anzeigt, in dem die Umgebung gespeichert ist) 

Es wird empfohlen, dass Sie die Dropdownlisten auf der Seite verwenden, anstatt die Informationen manuell einzugeben. Wenn Sie die Informationen manuell eingeben, geben Sie die vollqualifizierten Azure-Ressourcen-IDs ein. In der Aufgabe werden die Anzeigenamen anstelle der Ressourcen-IDs angezeigt. 

Der Umgebungsname ist der in DevTest Labs verwendete Anzeigename. Es muss sich für jeden Build um einen eindeutigen Namen handeln. Beispiel:  **TestEnv$(Build.BuildId)** . 

Sie können entweder eine Parameterdatei oder Parameter angeben, um die Informationen an die Resource Manager-Vorlage zu übergeben. 

Wählen Sie die Option **Create output variables based on the environment template output** (Ausgabevariablen auf der Grundlage der Ausgabe der Umgebungsvorlage erstellen) aus, und geben Sie einen Verweisnamen ein. Geben Sie in diesem Beispiel **BaseEnv** als Verweisnamen ein. Diesen Namen verwenden Sie beim Konfigurieren der nächsten Aufgabe. 

![Aufgabe zum Erstellen der Azure DevTest Labs-Umgebung](./media/use-devtest-labs-build-release-pipelines/create-environment.png)

### <a name="populate-environment-task"></a>Aufgabe zum Auffüllen der Umgebung
Mit der zweiten Aufgabe (Aufgabe **Azure DevTest Labs Populate Environment** (Azure DevTest Labs – Umgebung auffüllen)) wird die vorhandene DevTest Labs-Umgebung aktualisiert. Die Aufgabe zum Erstellen der Umgebung gibt **BaseEnv.environmentResourceId** aus, die zum Konfigurieren des Umgebungsnamens für diese Aufgabe verwendet wird. Die Resource Manager-Vorlage für dieses Beispiel umfasst zwei Parameter: **adminUserName** und **adminPassword**. 

![Aufgabe zum Auffüllen der Azure DevTest Labs-Umgebung](./media/use-devtest-labs-build-release-pipelines/populate-environment.png)

## <a name="app-service-deploy-task"></a>Aufgabe zur App Service-Bereitstellung
Bei der dritten Aufgabe handelt es sich um die Aufgabe **Azure App Service-Bereitstellung**. Der App-Typ ist auf **Web-App** festgelegt und der App Service-Name auf **$(WebSite)** .

![Aufgabe zur App Service-Bereitstellung](./media/use-devtest-labs-build-release-pipelines/app-service-deploy.png)

## <a name="setup-release-pipeline"></a>Einrichten der Releasepipeline
Sie erstellen eine Releasepipeline mit zwei Aufgaben: **Azure-Bereitstellung: Ressourcengruppe erstellen oder aktualisieren** und **Azure App Service-Bereitstellung**. 

Geben Sie für die erste Aufgabe den Namen und den Speicherort der Ressourcengruppe an. Der Speicherort der Vorlage ist ein verknüpftes Artefakt. Wenn die Resource Manager-Vorlage verknüpfte Vorlagen enthält, muss eine benutzerdefinierte Ressourcengruppenbereitstellung implementiert werden. Die Vorlage befindet sich im veröffentlichten Ablageartefakt. Überschreiben Sie die Vorlagenparameter für die Resource Manager-Vorlage. Sie können die verbleibenden Einstellungen mit den Standardwerten beibehalten. 

Geben Sie für die zweite Aufgabe **Azure App Service-Bereitstellung** das Azure-Abonnement an, und wählen Sie **Web-App** als **App-Typ** und **$(WebSite)** als **App Service-Name** aus. Sie können die verbleibenden Einstellungen mit den Standardwerten beibehalten. 

## <a name="test-run"></a>Testlauf
Nachdem Sie beide Pipelines eingerichtet haben, können Sie einen Build manuell in die Warteschlange einreihen und prüfen, wie er funktioniert. Im nächsten Schritt wird der entsprechende Trigger für den Build eingerichtet und der Build mit der Releasepipeline verbunden.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Integrieren von Azure DevTest Labs in Ihre Azure DevOps-Pipeline für Continuous Integration und Continuous Delivery](devtest-lab-integrate-ci-cd-vsts.md)
- [Integrieren von Umgebungen in Ihre Azure DevOps-CI/CD-Pipelines](integrate-environments-devops-pipeline.md)
