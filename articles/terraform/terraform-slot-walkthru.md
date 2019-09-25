---
title: Terraform mit Bereitstellungsslots für Azure-Anbieter
description: Tutorial zur Verwendung von Terraform mit Bereitstellungsslots für Azure-Anbieter
services: terraform
ms.service: azure
keywords: Terraform, DevOps, virtueller Computer, Azure, Bereitstellungsslots
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: ec2ed1da46df2793a241c9c89d168a6c5d462b9d
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169829"
---
# <a name="use-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Verwenden von Terraform zur Bereitstellung einer Infrastruktur mit Azure-Bereitstellungsslots

Mit [Azure-Bereitstellungsslots](/azure/app-service/deploy-staging-slots) können Sie zwischen verschiedenen Versionen Ihrer App wechseln. Dadurch lassen sich die Auswirkungen fehlerhafter Bereitstellungen minimieren. 

Dieser Artikel enthält ein Beispiel für die Verwendung von Bereitstellungsslots sowie die Schritte zur Bereitstellung von zwei Apps über GitHub und Azure. Eine App wird in einem Produktionsslot gehostet. Die zweite App wird in einem Stagingslot gehostet. (Die Namen „Produktion“ und „Staging“ sind willkürlich gewählt. Sie können beliebige Namen festlegen, die besser zu Ihrem Szenario passen.) Nach dem Konfigurieren Ihrer Bereitstellungsslots können Sie mithilfe von Terraform nach Bedarf zwischen den beiden Slots wechseln.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **GitHub-Konto:** Zum Forken und Verwenden des GitHub-Testrepositorys wird ein [GitHub](https://www.github.com)-Konto benötigt.

## <a name="create-and-apply-the-terraform-plan"></a>Erstellen und Anwenden des Terraform-Plans

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Öffnen Sie [Azure Cloud Shell](/azure/cloud-shell/overview). Falls Sie zuvor noch keine Umgebung ausgewählt haben, wählen Sie **Bash** als Umgebung aus.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Wechseln Sie in das Verzeichnis `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Erstellen Sie ein Verzeichnis namens `deploy`.

    ```bash
    mkdir deploy
    ```

1. Erstellen Sie ein Verzeichnis namens `swap`.

    ```bash
    mkdir swap
    ```

1. Vergewissern Sie sich mithilfe des Bash-Befehls `ls`, dass beide Verzeichnisse erfolgreich erstellt wurden.

    ![Cloud Shell nach dem Erstellen von Verzeichnissen](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Wechseln Sie in das Verzeichnis `deploy`.

    ```bash
    cd deploy
    ```

1. Erstellen Sie mithilfe des [vi-Editors](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html) eine Datei namens `deploy.tf`. Diese Datei enthält die [Terraform-Konfiguration](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
        app_service_name    = "${azurerm_app_service.slotDemo.name}"
    }
    ```

1. Drücken Sie die ESC-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Überprüfen Sie nun den Inhalt der erstellten Datei.

    ```bash
    cat deploy.tf
    ```

1. Initialisieren Sie Terraform.

    ```bash
    terraform init
    ```

1. Erstellen Sie den Terraform-Plan.

    ```bash
    terraform plan
    ```

1. Stellen Sie die in der Konfigurationsdatei `deploy.tf` definierten Ressourcen bereit. (Bestätigen Sie die Aktion, indem Sie an der Eingabeaufforderung `yes` eingeben.)

    ```bash
    terraform apply
    ```

1. Schließen Sie das Cloud Shell-Fenster.

1. Klicken Sie im Hauptmenü des Azure-Portals auf **Ressourcengruppen**.

    ![Klicken auf „Ressourcengruppen“ im Portal](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. Klicken Sie auf der Registerkarte **Ressourcengruppen** auf **slotDemoResourceGroup**.

    ![Von Terraform erstellte Ressourcengruppe](./media/terraform-slot-walkthru/resource-group.png)

Daraufhin werden alle von Terraform erstellten Ressourcen angezeigt.

![Von Terraform erstellte Ressourcen](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Forken des Testprojekts

Bevor Sie die Erstellung testen und zwischen den Bereitstellungsslots wechseln können, müssen Sie das Testprojekt über GitHub forken.

1. Navigieren Sie zum [Repository „awesome-terraform“ auf GitHub](https://github.com/Azure/awesome-terraform).

1. Forken Sie das Repository **awesome-terraform**.

    ![Forken des Repositorys „awesome-terraform“ auf GitHub](./media/terraform-slot-walkthru/fork-repo.png)

1. Befolgen Sie alle Anweisungen zum Forken Ihrer Umgebung.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Bereitstellen über GitHub in Ihren Bereitstellungsslots

Konfigurieren Sie nach dem Forken des Repositorys mit dem Testprojekt die Bereitstellungsslots mithilfe der folgenden Schritte:

1. Klicken Sie im Hauptmenü des Azure-Portals auf **Ressourcengruppen**.

1. Klicken Sie auf **slotDemoResourceGroup**.

1. Klicken Sie auf **slotAppService**.

1. Klicken Sie auf **Bereitstellungsoptionen**.

    ![Bereitstellungsoptionen für eine App Service-Ressource](./media/terraform-slot-walkthru/deployment-options.png)

1. Klicken Sie auf der Registerkarte **Bereitstellungsoption** auf **Quelle auswählen** und dann auf **GitHub**.

    ![Auswählen der Bereitstellungsquelle](./media/terraform-slot-walkthru/select-source.png)

1. Wenn Azure die Verbindung hergestellt hat und alle Optionen anzeigt, klicken Sie auf **Autorisierung**.

1. Klicken Sie auf der Registerkarte **Autorisierung** auf **Autorisieren**, und geben Sie die Anmeldeinformationen ein, die Azure für den Zugriff auf Ihr GitHub-Konto benötigt. 

1. Nach der Überprüfung Ihrer GitHub-Anmeldeinformationen durch Azure wird eine Meldung mit dem Hinweis angezeigt, dass der Autorisierungsprozess abgeschlossen ist. Klicken Sie auf **OK**, um die Registerkarte **Autorisierung** zu schließen.

1. Klicken Sie auf **Organisation auswählen**, und wählen Sie Ihre Organisation aus.

1. Klicken Sie auf **Projekt auswählen**.

1. Wählen Sie auf der Registerkarte **Projekt auswählen** das Projekt **awesome-terraform** aus.

    ![Auswählen des Projekts „awesome-terraform“](./media/terraform-slot-walkthru/choose-project.png)

1. Klicken Sie auf **Verzweigung auswählen**.

1. Klicken Sie auf der Registerkarte **Verzweigung auswählen** auf **master**.

    ![Auswählen der Masterverzweigung](./media/terraform-slot-walkthru/choose-branch-master.png)

1. Klicken Sie auf der Registerkarte **Bereitstellungsoption** auf **OK**.

Nun haben Sie den Produktionsslot bereitgestellt. Führen Sie zum Bereitstellen des Stagingslots erneut die Schritte dieses Abschnitts aus – allerdings mit folgenden Anpassungen:

- Wählen Sie in Schritt 3 die Ressource **slotAppServiceSlotOne** aus.

- Wählen Sie in Schritt 13 anstelle der Masterverzweigung die Arbeitsverzweigung aus.

    ![Auswählen der Arbeitsverzweigung](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Testen der App-Bereitstellungen

In den vorherigen Abschnitten haben Sie zwei Slots (**slotAppService** und **slotAppServiceSlotOne**) für die Bereitstellung über verschiedene Verzweigungen in GitHub eingerichtet. Sehen wir uns eine Vorschau der Web-Apps an, um sicherzustellen, dass sie erfolgreich bereitgestellt wurden.

Führen Sie die folgenden Schritte zweimal aus: Wählen Sie in Schritt 3 beim ersten Mal **slotAppService** und beim zweiten Mal **slotAppServiceSlotOne** aus.

1. Klicken Sie im Hauptmenü des Azure-Portals auf **Ressourcengruppen**.

1. Klicken Sie auf **slotDemoResourceGroup**.

1. Wählen Sie entweder **slotAppService** oder **slotAppServiceSlotOne** aus.

1. Klicken Sie auf der Übersichtsseite auf **URL**.

    ![Auswählen von „URL“ auf der Registerkarte „Übersicht“ zum Rendern der App](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Es kann einige Minuten dauern, bis Azure die Site über GitHub erstellt und bereitgestellt hat.
>
>

Für die Web-App **slotAppService** wird eine blaue Seite mit dem Seitentitel **Slot Demo App 1** angezeigt. Für die Web-App **slotAppServiceSlotOne** wird eine grüne Seite mit dem Seitentitel **Slot Demo App 2** angezeigt.

![Anzeigen einer Vorschau der Apps, um zu testen, ob sie richtig bereitgestellt wurden](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>Wechseln zwischen den zwei Bereitstellungsslots

Wenn Sie das Wechseln zwischen den beiden Bereitstellungsslots testen möchten, führen Sie die folgenden Schritte aus:
 
1. Wechseln Sie zur Browserregisterkarte mit **slotAppService** (die App mit der blauen Seite). 

1. Öffnen Sie eine separate Registerkarte mit dem Azure-Portal.

1. Öffnen Sie Cloud Shell.

1. Wechseln Sie in das Verzeichnis **clouddrive/swap**.

    ```bash
    cd clouddrive/swap
    ```

1. Erstellen Sie mithilfe des vi-Editors eine Datei namens `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Drücken Sie die ESC-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Initialisieren Sie Terraform.

    ```bash
    terraform init
    ```

1. Erstellen Sie den Terraform-Plan.

    ```bash
    terraform plan
    ```

1. Stellen Sie die in der Konfigurationsdatei `swap.tf` definierten Ressourcen bereit. (Bestätigen Sie die Aktion, indem Sie an der Eingabeaufforderung `yes` eingeben.)

    ```bash
    terraform apply
    ```

1. Nachdem die Slots von Terraform gewechselt wurden, kehren Sie zu dem Browser zurück, in dem die Web-App **slotAppService** gerendert wird, und aktualisieren Sie die Seite. 

Mit der Web-App aus dem Stagingslot **slotAppServiceSlotOne** wurde in den Produktionsslot gewechselt, und sie wird nun grün gerendert. 

![Ausgetauschte Bereitstellungsslots](./media/terraform-slot-walkthru/slots-swapped.png)

Wenn Sie zur ursprünglichen Produktionsversion der App zurückkehren möchten, wenden Sie erneut den Terraform-Plan an, den Sie auf der Grundlage der Konfigurationsdatei `swap.tf` erstellt haben.

```bash
terraform apply
```

Nach dem Wechsel der App wird wieder die ursprüngliche Konfiguration angezeigt.