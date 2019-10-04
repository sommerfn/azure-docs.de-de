---
title: Verwenden des Azure-Portals zum Bereitstellen von Azure-Ressourcen | Microsoft Docs
description: Verwenden Sie das Azure-Portal und Azure Resource Manager zum Bereitstellen Ihrer Ressourcen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: tomfitz
ms.openlocfilehash: a171d9b4f054c942eebb08e7e11dd1164545f408
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460289"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal

Erfahren Sie, wie Sie mit dem [Azure-Portal](https://portal.azure.com) und [Azure Resource Manager](resource-group-overview.md) Ihre Azure-Ressourcen bereitstellen. Informationen zum Verwalten von Ressourcen finden Sie unter [Verwalten von Azure-Ressourcen unter Verwendung des Azure-Portals](manage-resources-portal.md).

Zur Bereitstellung von Azure-Ressourcen mit dem Azure-Portal sind normalerweise zwei Schritte notwendig:

- Erstellen Sie eine Ressourcengruppe.
- Bereitstellen von Ressourcen in der Ressourcengruppe.

Außerdem können Sie eine Azure Resource Manager-Vorlage erstellen, um Azure-Ressourcen bereitzustellen.

In diesem Artikel werden beide Methoden beschrieben.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

1. Wählen Sie **„Ressourcengruppen“** im [Azure-Portal](https://portal.azure.com), um eine neue Ressourcengruppe zu erstellen.

   ![Auswählen von Ressourcengruppen](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Wählen Sie unter „Ressourcengruppe“ die Option **Hinzufügen**.

   ![Hinzufügen von Ressourcengruppen](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Wählen Sie die folgenden Eigenschaftswerte aus, bzw. geben Sie sie ein:

    - **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    - **Ressourcengruppe**: Geben Sie der Ressourcengruppe einen Namen.
    - **Region**: Geben Sie einen Azure-Standort an. Dort speichert die Ressourcengruppe Metadaten über die Ressourcen. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.

   ![Festlegen von Gruppenwerten](./media/resource-group-template-deploy-portal/set-group-properties.png)

1. Klicken Sie auf **Überprüfen + erstellen**.
1. Überprüfen Sie die Werte, und wählen Sie dann **„Erstellen“** .
1. Wählen Sie **„Aktualisieren“** , damit die neue Ressourcengruppe in der Liste erscheint.

## <a name="deploy-resources-to-a-resource-group"></a>Bereitstellen von Ressourcen in einer Ressourcengruppe

Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie Ressourcen für die Gruppe über den Marketplace bereitstellen. Der Marketplace bietet vordefinierte Lösungen für gängige Szenarien.

1. Klicken Sie zum Starten einer Bereitstellung auf **„Ressource erstellen“** im [Azure-Portal](https://portal.azure.com).

   ![Neue Ressource](./media/resource-group-template-deploy-portal/new-resources.png)

1. Suchen Sie den Typ der Ressource, die Sie bereitstellen möchten. Die Ressource sind in Kategorien organisiert. Falls die gewünschte bereitzustellende Lösung nicht angezeigt wird, können Sie im Marketplace danach suchen. Im folgenden Screenshot ist der Ubuntu-Server ausgewählt.

   ![Ressourcentyp auswählen](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Je nach ausgewähltem Ressourcentyp müssen Sie einige relevante Eigenschaften festlegen, bevor die Bereitstellung beginnen kann. Für alle Typen müssen Sie eine Zielressourcengruppe auswählen. Die folgende Abbildung zeigt, wie Sie einen virtuellen Linux-Computer erstellen und in der erstellten Ressourcengruppe bereitstellen.

   ![Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/select-existing-group.png)

   Alternativ können Sie eine Ressourcengruppe beim Bereitstellen Ihrer Ressourcen erstellen. Wählen Sie **Neu erstellen** aus, und benennen Sie die Ressourcengruppe.

1. Die Bereitstellung wird gestartet. Die Bereitstellung kann mehrere Minuten dauern. Einige Ressourcen brauchen länger als andere. Nachdem die Bereitstellung abgeschlossen wurde, wird eine Benachrichtigung angezeigt. Wählen Sie **„Zu Ressource wechseln“** und öffnen sie es.

   ![Anzeigen einer Benachrichtigung](./media/resource-group-template-deploy-portal/view-notification.png)

1. Nach dem Bereitstellen Ihrer Ressourcen können Sie mit **Hinzufügen** weitere Ressourcen hinzufügen.

   ![Ressource hinzufügen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage

Wenn Sie eine Bereitstellung ausführen möchten, ohne eine der Vorlagen im Marketplace zu nutzen, können Sie eine angepasste Vorlage erstellen, mit der die Infrastruktur für Ihre Lösung definiert wird. Informationen zum Erstellen von Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

> [!NOTE]
> Für die Portalschnittstelle wird das Verweisen auf ein [Geheimnis aus einem Key Vault](resource-manager-keyvault-parameter.md) nicht unterstützt. Verwenden Sie stattdessen [PowerShell](resource-group-template-deploy.md) oder [Azure CLI](resource-group-template-deploy-cli.md), um Ihre Vorlage lokal oder über einen externen URI bereitzustellen.

1. Klicken Sie zum Bereitstellen einer benutzerdefinierten Vorlage über das Portal auf **„Ressource erstellen“** und suchen Sie nach **„Vorlage“** . Wählen Sie dann **„Vorlagenbereitstellung“** aus.

   ![Suchen der Vorlagenbereitstellung](./media/resource-group-template-deploy-portal/search-template.png)

1. Klicken Sie auf **Erstellen**.
1. Zum Erstellen einer Vorlage werden verschiedene Optionen angezeigt:

    - **Eigene Vorlage im Editor erstellen**: Erstellen Sie eine Vorlage mit dem Vorlageneditor des Portals.  Der Editor kann ein Vorlagenschema für eine Ressource hinzufügen.
    - **Gängige Vorlagen**: Es gibt vier gängige Vorlagen für das Erstellen von virtuellen Linux-Computern, virtuellen Windows-Computern, Webanwendungen und einer Azure SQL-Datenbank.
    - **GitHub-Schnellstartvorlage laden**: Verwenden Sie eine vorhandene [Schnellstartvorlage](https://azure.microsoft.com/resources/templates/).

   ![Anzeigen der Optionen](./media/resource-group-template-deploy-portal/see-options.png)

    Dieses Tutorial zeigt Ihnen, wie Sie eine Schnellstartvorlage laden.

1. Geben Sie unter **„GitHub-Schnellstartvorlage laden“** **„101-storage-account-create“** ein oder wählen Sie es aus.

    Sie haben zwei Möglichkeiten:

    - **Auswahl der Vorlage**: Stellen Sie die Vorlage bereit.
    - **Bearbeiten der Vorlage**: Bearbeiten Sie die Schnellstartvorlage, bevor Sie sie bereitstellen.

1. Wählen Sie **„Bearbeiten der Vorlage“** aus, um sich den Vorlageneditor des Portals anzusehen. Diese Vorlage wird in den Editor geladen. Bitte beachten Sie, dass es zwei Parameter gibt: **„storageAccountType“** und **„location“** .

   ![Erstellen der Vorlage](./media/resource-group-template-deploy-portal/show-json.png)

1. Nehmen Sie eine geringfügige Änderung an der Vorlage vor. Aktualisieren Sie zum Beispiel die Variabel **„storageAccountName“** auf:

    ```json
    "storageAccountName": "[concat('azstore', uniquestring(resourceGroup().id))]"
    ```

1. Wählen Sie **Speichern** aus. Jetzt sehen Sie die Bereitstellungsschnittstelle für Vorlagen im Portal. Sie sehen außerdem die zwei Parameter, die Sie in der Vorlage definiert haben.
1. Geben Sie die Eigenschaftswerte ein oder wählen Sie sie aus:

    - **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **„Neu erstellen“** , und vergeben Sie einen Namen.
    - **Standort**: Wählen Sie einen Azure-Standort aus.
    - **Speicherkontotyp**: Verwenden Sie den Standardwert.
    - **Standort**: Verwenden Sie den Standardwert.
    - **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.

1. Wählen Sie die Option **Kaufen**.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](./resource-group-audit.md).
- Informationen zum Beheben von Bereitstellungsfehlern finden Sie unter [Anzeigen von Bereitstellungsvorgängen](./resource-manager-deployment-operations.md).
- Informationen zum Exportieren einer Vorlage aus einer Bereitstellung oder Ressourcengruppe finden Sie unter [Exportieren von Azure Resource Manager-Vorlagen](./manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Informationen zum sicheren Rollout Ihres Dienst über mehrere Regionen finden Sie im Artikel zum [Azure-Bereitstellungs-Manager](./deployment-manager-overview.md).
