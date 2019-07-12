---
title: include file
description: include file
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4f3d31fa25ea5781bc3af7297719b77723597e17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444593"
---
## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Visual Studio Code ermöglicht die Veröffentlichung Ihres Funktionsprojekts direkt in Azure. Bei diesem Vorgang erstellen Sie eine Funktions-App und zugehörige Ressourcen in Ihrem Azure-Abonnement. Die Funktions-App bietet einen Ausführungskontext für Ihre Funktionen. Das Projekt wird verpackt und in der neuen Funktions-App in Ihrem Azure-Abonnement bereitgestellt.

Standardmäßig erstellt Visual Studio alle Azure-Ressourcen, die für die Erstellung Ihrer Funktions-App erforderlich sind. Die Namen dieser Ressourcen basieren auf dem von Ihnen gewählten Name der Funktions-App. Wenn Sie die volle Kontrolle über die erstellten Ressourcen haben möchten, können Sie stattdessen [mit erweiterten Optionen veröffentlichen](../articles/azure-functions/functions-develop-vs-code.md#enabled-publishing-with-advanced-create-options).

In diesem Abschnitt wird davon ausgegangen, dass Sie eine neue Funktions-App in Azure erstellen.

> [!IMPORTANT]
> Beim Veröffentlichen in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Deploy to function app...`, und wählen Sie ihn aus.

1. Wenn Sie nicht angemeldet sind, werden Sie aufgefordert, **sich bei Azure anzumelden**. Sie können auch ein **kostenloses Azure-Konto erstellen**. Nach der erfolgreichen Anmeldung aus dem Browser wechseln Sie zurück zu Visual Studio Code. 

1. Wenn Sie mehrere Abonnements besitzen, **wählen Sie ein Abonnement für die Funktions-App aus**, und wählen Sie dann **+ Neue Funktions-App erstellen** aus.

1. Geben Sie einen global eindeutigen Namen ein, der Ihre Funktions-App identifiziert, und drücken Sie die EINGABETASTE. Gültige Zeichen für den Namen einer Funktions-App sind `a-z`, `0-9` und `-`.

    Wenn Sie die EINGABETASTE drücken, werden die folgenden Azure-Ressourcen in Ihrem Abonnement erstellt:

    * **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)** : Enthält alle erstellten Azure-Ressourcen. Der Name basiert auf dem Namen Ihrer Funktions-App.
    * **[Speicherkonto](../articles/storage/common/storage-quickstart-create-account.md)** : Ein Standardspeicherkonto wird mit einem eindeutigen Namen erstellt, der auf dem Namen Ihrer Funktions-App basiert.
    * **[Hostingplan](../articles/azure-functions/functions-scale.md)** : Ein Verbrauchsplan wird in der Region „USA, Westen“ erstellt, um Ihre serverlose Funktions-App zu hosten.
    * **Funktions-App**: Ihr Projekt wird in dieser neuen Funktions-App bereitgestellt, wo es auch ausgeführt wird.

    Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen.

1. Zurück im Bereich **Azure: Funktionen** erweitern Sie die neue Funktions-App unter Ihrem Abonnement. Erweitern Sie **Funktionen**, klicken Sie mit der rechten Maustaste auf **HttpTrigger**, und wählen Sie dann **Funktions-URL kopieren** aus.

    ![Kopieren Sie die Funktions-URL für den neuen HTTP-Trigger.](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
