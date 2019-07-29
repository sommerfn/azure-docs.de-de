---
title: Continuous Deployment für Azure Functions | Microsoft Docs
description: Hier erfahren Sie, wie Sie Ihre Funktionen mithilfe der Continuous Deployment-Features von Azure App Service verwenden veröffentlichen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: dd1605aa2f5fc9e0b4bc790bae2a1c20cb3ea048
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594529"
---
# <a name="continuous-deployment-for-azure-functions"></a>Continuous Deployment für Azure Functions

Mit Azure Functions können Sie Ihren Code mithilfe der [Integration der Quellcodeverwaltung](functions-deployment-technologies.md#source-control) kontinuierlich bereitstellen. Die Integration der Quellcodeverwaltung ermöglicht einen Workflow, bei dem eine Codeaktualisierung eine Bereitstellung in Azure auslöst. Sollten Sie noch nicht mit Azure Functions vertraut sein, sehen Sie sich zunächst die [Einführung in Azure Functions](functions-overview.md) an.

Continuous Deployment eignet sich hervorragend für Projekte mit häufiger Integration zahlreicher Beiträge. Bei Verwendung von Continuous Deployment wird für Ihren Code eine einzelne alleingültige Quelle (Single Source Of Truth, SSOT) gepflegt, wodurch Teams problemlos zusammenarbeiten können. Sie können Continuous Deployment in Azure Functions von folgenden Speicherorten für Quellcode konfigurieren:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

Die Bereitstellungseinheit für Funktionen in Azure ist die Funktions-App. Alle Funktionen in einer Funktions-App werden gleichzeitig bereitgestellt. Nach Aktivierung von Continuous Deployment wird der Zugriff auf Funktionscode im Azure-Portal als *schreibgeschützt* konfiguriert, da die Source of Truth auf einen anderen Ort festgelegt ist.

## <a name="requirements-for-continuous-deployment"></a>Anforderungen für Continuous Deployment

Damit Continuous Deployment erfolgreich ausgeführt werden kann, muss Ihre Verzeichnisstruktur mit der grundlegenden Ordnerstruktur kompatibel sein, die von Azure Functions erwartet wird.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Einrichten von Continuous Deployment

Gehen Sie wie folgt vor, um Continuous Deployment für eine vorhandene Funktions-App zu konfigurieren. Die Schritte zeigen die Integration eines GitHub-Repositorys. Die Vorgehensweise für Azure Repos und andere Quellcoderepositorys ist jedoch ähnlich.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) unter Ihrer Funktions-App die Optionen **Plattformfeatures** > **Bereitstellungscenter** aus.

    ![Öffnen des Bereitstellungscenters](./media/functions-continuous-deployment/platform-features.png)

2. Wählen Sie im **Bereitstellungscenter** die Option **GitHub** und anschließend **Autorisieren** aus. Falls Sie GitHub bereits autorisiert haben, wählen Sie **Weiter** aus. 

    ![Azure App Service-Bereitstellungscenter](./media/functions-continuous-deployment/github.png)

3. Wählen Sie auf GitHub die Schaltfläche **Authorize AzureAppService** (AzureAppService autorisieren) aus. 

    ![Autorisieren von Azure App Service](./media/functions-continuous-deployment/authorize.png)
    
    Wählen Sie im Azure-Portal im **Bereitstellungscenter** die Option **Weiter** aus.

4. Wählen Sie einen der folgenden Buildanbieter aus:

    * **App Service-Builddienst**: Empfiehlt sich, wenn Sie keinen Build oder einen generischen Build benötigen.
    * **Azure-Pipelines (Vorschau)** : Empfiehlt sich, wenn Sie mehr Kontrolle über den Build benötigen. Dieser Anbieter befindet sich derzeit in der Vorschauphase.

    ![Auswählen eines Buildanbieters](./media/functions-continuous-deployment/build.png)

5. Konfigurieren Sie Informationen, die für die von Ihnen angegebene Option der Quellcodeverwaltung spezifisch sind. Für GitHub müssen Werte für **Organisation**, **Repository** und **Branch** eingegeben bzw. ausgewählt werden. Die Werte basieren auf dem Ort, an dem sich Ihr Code befindet. Wählen Sie anschließend **Weiter** aus.

    ![Konfigurieren von GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Überprüfen Sie alle Angaben, und wählen Sie anschließend **Fertig stellen** aus, um die Bereitstellungskonfiguration abzuschließen.

    ![Zusammenfassung](./media/functions-continuous-deployment/summary.png)

Nach Abschluss des Prozesses wird der gesamte Code aus der angegebenen Quelle für Ihre App bereitgestellt. An diesem Punkt lösen Änderungen in der Bereitstellungsquelle eine Bereitstellung dieser Änderungen in Ihrer Funktions-App in Azure aus.

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Verschieben vorhandener Funktionen in Continuous Deployment

Wenn Sie bereits Funktionen im [Azure-Portal](https://portal.azure.com) geschrieben haben und den Inhalt Ihrer App vor dem Wechsel zu Continuous Deployment herunterladen möchten, navigieren Sie zur Registerkarte **Übersicht** Ihrer Funktions-App. Wählen Sie die Schaltfläche **App-Inhalt herunterladen** aus.

![Herunterladen des App-Inhalts](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Nach dem Konfigurieren von Continuous Integration können Sie Ihre Quelldateien im Functions-Portal nicht mehr bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für Azure Functions](functions-best-practices.md)
