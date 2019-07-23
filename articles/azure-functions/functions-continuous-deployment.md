---
title: Continuous Deployment für Azure Functions | Microsoft Docs
description: Verwenden der Continuous Deployment-Funktionen von Azure App Service, um Ihre Funktionen zu veröffentlichen.
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
ms.openlocfilehash: 2dc5fab0966c2ead0276cd8b23ea764bd4f9ef59
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190473"
---
# <a name="continuous-deployment-for-azure-functions"></a>Continuous Deployment für Azure Functions

Azure Functions ermöglicht Ihnen das kontinuierliche Bereitstellen Ihres Codes über die [Integration der Quellcodeverwaltung](functions-deployment-technologies.md#source-control). Dies ermöglicht einen Workflow, bei dem Codeaktualisierungen eine Bereitstellung in Azure auslösen. Sollten Sie noch nicht mit Azure Functions vertraut sein, sehen Sie sich zuerst die [Übersicht zu Azure Functions](functions-overview.md) an.

Continuous Deployment ist hervorragend für Projekte geeignet, bei denen Sie häufig zahlreiche Beiträge integrieren. Außerdem können Sie eine Single Source of Truth (einzige Quelle der Wahrheit) für Ihren Funktionscode beibehalten. Sie können Continuous Deployment in Azure Functions von folgenden Speicherorten für Quellcode konfigurieren:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

Die Bereitstellungseinheit für Azure Functions ist die Funktions-App. Dies bedeutet, dass alle Funktionen in einer Funktions-App gleichzeitig bereitgestellt werden. Nach Aktivierung von Continuous Deployment wird der Zugriff auf Funktionscode im Azure-Portal als *schreibgeschützt* konfiguriert, da die Source of Truth (Quelle der Wahrheit) auf einen anderen Ort festgelegt ist.

## <a name="requirements-for-continuous-deployment"></a>Anforderungen für Continuous Deployment

Damit Continuous Deployment erfolgreich ausgeführt werden kann, muss Ihre Verzeichnisstruktur mit der folgenden grundlegenden Ordnerstruktur kompatibel sein, die von Azure Functions erwartet wird:

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Einrichten von Continuous Deployment

Gehen Sie wie folgt vor, um Continuous Deployment für eine vorhandene Funktionen-App zu konfigurieren. Die folgenden Schritte beziehen sich auf die Integration eines GitHub-Repositorys. Für Azure Repos und andere Quellcoderepositorys gelten jedoch ähnliche Schritte.

1. Wählen Sie in Ihrer Funktions-App im [Azure-Portal](https://portal.azure.com) die Optionen **Plattformfeatures** > **Bereitstellungscenter** aus.

    ![Öffnen des Bereitstellungscenters](./media/functions-continuous-deployment/platform-features.png)

2. Wählen Sie im **Bereitstellungscenter** die Option **GitHub** und dann **Autorisieren** aus. Falls Sie GitHub bereits autorisiert haben, wählen Sie **Weiter** aus. 

    ![Bereitstellungscenter](./media/functions-continuous-deployment/github.png)

3. Wählen Sie in GitHub **AzureAppService autorisieren** aus. 

    ![Autorisieren](./media/functions-continuous-deployment/authorize.png)
    
    Wählen Sie im **Bereitstellungscenter** im Azure-Portal **Weiter** aus.

4. Wählen Sie einen der folgenden Buildanbieter aus:

    * **App Service-Builddienst**: Optimal geeignet, wenn Sie keinen Build oder einen generischen Build benötigen.
    * **Azure-Pipelines (Vorschau)** : Optimal geeignet, wenn Sie mehr Kontrolle über den Build benötigen. Dieser Anbieter befindet sich derzeit in der Vorschauphase.

    ![Auswählen eines Buildanbieters](./media/functions-continuous-deployment/build.png)

5. Konfigurieren Sie Informationen, die für die von Ihnen angegebene Option der Quellcodeverwaltung spezifisch sind. Für GitHub müssen Sie die **Organisation**, das **Repository** und die **Verzweigung** angeben, in der sich Ihr Code befindet. Wählen Sie anschließend **Weiter** aus.

    ![Konfigurieren von GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Zum Schluss überprüfen Sie alle Details, und wählen Sie **Fertig stellen** aus, um die Bereitstellungskonfiguration abzuschließen.

    ![Zusammenfassung](./media/functions-continuous-deployment/summary.png)

Wenn der Prozess abgeschlossen ist, wird der gesamte Code aus der angegebenen Quelle Ihrer App bereitgestellt. An diesem Punkt lösen Änderungen in der Bereitstellungsquelle eine Bereitstellung dieser Änderungen in Ihrer Funktions-App in Azure aus.

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verschieben vorhandener Funktionen in Continuous Deployment

Wenn Sie bereits Funktionen im [Azure-Portal](https://portal.azure.com) geschrieben haben und den Inhalt Ihrer App vor dem Wechsel zu Continuous Deployment herunterladen möchten, navigieren Sie zur Registerkarte **Übersicht** Ihrer Funktions-App, und klicken Sie auf die Schaltfläche **App-Inhalt herunterladen**.

![Herunterladen von App-Inhalt](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Nach dem Konfigurieren von Continuous Integration können Sie Ihre Quelldateien im Functions-Portal nicht mehr bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für Azure Functions](functions-best-practices.md)
