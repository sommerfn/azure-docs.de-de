---
title: Behandeln von Ressource Manager-Bereitstellungsproblemen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Ressource Manager-Bereitstellungen überwacht und Probleme damit behandelt werden.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/15/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: c889c3123160680d96889227d6964ff197dc41cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60388641"
---
# <a name="tutorial-troubleshoot-resource-manager-template-deployments"></a>Tutorial: Problembehandlung bei der Resource Manager-Vorlagenbereitstellung

Hier erfahren Sie, wie Fehler bei der Ressource Manager-Vorlagenbereitstellung behandelt werden. In diesem Tutorial erzeugen Sie zwei Fehler in einer Vorlage und lernen dann, wie die Probleme mithilfe der Aktivitätsprotokolle und des Bereitstellungsverlaufs behoben werden.

Im Zusammenhang mit der Vorlagenbereitstellung gibt es zwei Fehlertypen:

- **Überprüfungsfehler** entstehen durch Szenarien, die vor der Bereitstellung bestimmt werden können. Dazu gehören Syntaxfehler in Ihrer Vorlage oder Versuche, Ressourcen bereitzustellen, die Ihre Abonnementkontingente überschreiten würden. 
- **Bereitstellungsfehler** können durch Bedingungen verursacht werden, die während des Bereitstellungsprozesses auftreten. Dazu gehören Versuche, auf eine Ressource zuzugreifen, die parallel bereitgestellt wird.

Beide Fehlertypen geben einen Fehlercode zurück, der für die Problembehandlung für die Bereitstellung genutzt werden kann. Beide Fehlertypen werden im Aktivitätsprotokoll angezeigt. Überprüfungsfehler werden allerdings nicht im Bereitstellungsverlauf festgehalten, da die Bereitstellung tatsächlich nie gestartet wurde.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen einer problematischen Vorlage
> * Behandeln von Überprüfungsfehlern
> * Behandeln von Bereitstellungsfehlern
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- [Visual Studio Code](https://code.visualstudio.com/) mit der [Erweiterung „Azure Resource Manager-Tools“](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="create-a-problematic-template"></a>Erstellen einer problematischen Vorlage

Öffnen Sie über [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) eine Vorlage namens [Erstellen eines Standardspeicherkontos](https://azure.microsoft.com/resources/templates/101-storage-account-create/), und erzeugen Sie zwei Vorlagenprobleme.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Ändern Sie die Zeile **apiVersion** in folgende Zeile:

    ```json
    "apiVersion1": "2018-07-02",
    ```
    - **apiVersion1** ist ein ungültiger Elementname. Dies ist ein Überprüfungsfehler.
    - Die API-Version muss „2018-07-01“ sein.  Dies ist ein Bereitstellungsfehler.

5. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="troubleshoot-the-validation-error"></a>Behandeln des Überprüfungsfehlers

Informationen zum Bereitstellen der Vorlage Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

Sie erhalten von der Shell einen ähnlichen Fehler wie:

```
New-AzResourceGroupDeployment : 4:29:24 PM - Error: Code=InvalidRequestContent; Message=The request content was invalid and could not be deserialized: 'Could not find member 'apiVersion1' on object of type 'TemplateResource'. Path 'properties.template.resources[0].apiVersion1', line 36, position 24.'.
```

Die Fehlermeldung gibt an, dass das Problem durch **apiVersion1** verursacht wird.

Verwenden Sie Visual Studio Code, um das Problem zu beheben, indem Sie **apiVersion1** in **apiVersion** ändern und dann die Vorlage speichern.

## <a name="troubleshoot-the-deployment-error"></a>Behandeln des Bereitstellungsfehlers

Informationen zum Bereitstellen der Vorlage Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

Sie erhalten von der Shell einen ähnlichen Fehler wie:

```
New-AzResourceGroupDeployment : 4:48:50 PM - Resource Microsoft.Storage/storageAccounts 'storeqii7x2rce77dc' failed with message '{
  "error": {
    "code": "NoRegisteredProviderFound",
    "message": "No registered resource provider found for location 'centralus' and API version '2018-07-02' for type 'storageAccounts'. The supported api-versions are '2018-07-01, 2018-03-01-preview, 2018-02-01, 2017-10-01, 2017-06-01, 2016-12-01, 2016-05-01, 2016-01-01, 2015-06-15, 2015-05-01-preview'. The supported locations are 'eastus, eastus2, westus, westeurope, eastasia, southeastasia, japaneast, japanwest, northcentralus, southcentralus, centralus, northeurope, brazilsouth, australiaeast, australiasoutheast, southindia, centralindia, westindia, canadaeast, canadacentral, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth, francecentral'."
  }
}'
```

Der Bereitstellungsfehler lässt sich im Azure-Portal mit dem folgenden Verfahren finden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Öffnen Sie die Ressourcengruppe, indem Sie **Ressourcengruppen** und dann den Namen der Ressourcengruppe auswählen. Unter **Bereitstellung** sollte **1 Fehler** angezeigt werden.

    ![Resource Manager-Tutorial zur Problembehandlung](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error.png)
3. Wählen Sie **Fehlerdetails** aus.

    ![Resource Manager-Tutorial zur Problembehandlung](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error-details.png)

    Die Fehlermeldung ist identisch mit der zuvor gezeigten:

    ![Resource Manager-Tutorial zur Problembehandlung](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error-summary.png)

Sie können den Fehler auch in den Aktivitätsprotokollen finden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Überwachen** > **Aktivitätsprotokoll**.
3. Suchen Sie Protokoll mithilfe der Filter.

    ![Resource Manager-Tutorial zur Problembehandlung](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-activity-log.png)

Korrigieren Sie das Problem mithilfe von Visual Studio Code, und stellen Sie dann die Vorlage erneut bereit.

Eine Liste gängiger Fehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](./resource-manager-common-deployment-errors.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Fehler bei der Ressource Manager-Vorlagenbereitstellung behandelt werden.  Weitere Informationen finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](./resource-manager-common-deployment-errors.md).
