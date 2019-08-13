---
title: Erstellen einer Definition für eine verwaltete Azure-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: 7682670131b0ef50a1480285bc379b634169e49e
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840624"
---
# <a name="publish-an-azure-managed-application-definition"></a>Veröffentlichen einer Definition für eine verwaltete Azure-Anwendung

Diese Schnellstartanleitung enthält eine Einführung in die Verwendung von verwalteten Anwendungen. Sie fügen einem internen Katalog für die Benutzer Ihrer Organisation eine Definition für die verwaltete Anwendung hinzu. Wir haben die Dateien für Ihre verwaltete Anwendung bereits erstellt, um die Einführung zu vereinfachen. Diese Dateien sind über GitHub verfügbar. Im Tutorial [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md) erfahren Sie, wie Sie diese Dateien erstellen.

Wenn Sie fertig sind, verfügen Sie über eine Ressourcengruppe mit dem Namen **appDefinitionGroup**, die die Definition für die verwaltete Anwendung enthält.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Erstellen einer Ressourcengruppe für die Definition

Die Definition Ihrer verwalteten Anwendung ist in einer Ressourcengruppe enthalten. Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe zu erstellen:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Beim Definieren der verwalteten Anwendung wählen Sie einen Benutzer, eine Gruppe oder eine Anwendung aus, um die Ressourcen für den Consumer zu verwalten. Diese Identität verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Normalerweise erstellen Sie eine Azure Active Directory-Gruppe, um die Ressourcen zu verwalten. Verwenden Sie für diesen Artikel aber Ihre eigene Identität.

Geben Sie im folgenden Befehl Ihren Benutzerprinzipalnamen an, um die Objekt-ID Ihrer Identität zu erhalten:

```azurecli-interactive
userid=$(az ad user show --id example@contoso.org --query objectId --output tsv)
```

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer gewähren möchten. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung. Die verwaltete Anwendung enthält nur ein Speicherkonto.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"
```

Nachdem die Ausführung des Befehls abgeschlossen ist, verfügen Sie in Ihrer Ressourcengruppe über eine Definition für die verwaltete Anwendung. 

Im vorigen Beispiel werden u.a. folgende Parameter verwendet:

* **resource-group**: Der Name der Ressourcengruppe, in der die Definition für die verwaltete Anwendung erstellt wird.
* **lock-level**: Der Typ der Sperre, der auf die verwaltete Ressourcengruppe angewendet wird. Er verhindert, dass der Kunde unerwünschte Vorgänge für diese Ressourcengruppe durchführt. Derzeit ist „ReadOnly“ die einzige unterstützte Sperrebene. Wenn „ReadOnly“ festgelegt wird, sind die in der verwalteten Ressourcengruppe vorhandenen Ressourcen für den Kunden schreibgeschützt. Dies gilt nicht für Herausgeberidentitäten, denen Zugriff auf die verwaltete Ressourcengruppe gewährt wird.
* **authorizations**: Beschreibt die Prinzipal-ID und die Rollendefinitions-ID, die zum Erteilen von Berechtigungen für die verwaltete Ressourcengruppe verwendet werden. Diese ist im Format `<principalId>:<roleDefinitionId>` angegeben. Wenn mehr als ein Wert erforderlich ist, können Sie diese in der Form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>` angeben. Die Werte werden hierbei durch ein Leerzeichen voneinander getrennt.
* **package-file-uri**: Der Speicherort eines ZIP-Pakets, das die erforderlichen Dateien enthält. Das Paket muss die Dateien **mainTemplate.json** und **createUiDefinition.json** enthalten. Die Datei **mainTemplate.json** definiert die Azure-Ressourcen, die als Teil der verwalteten Anwendung erstellt werden. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen. Die Datei **createUiDefinition.json** generiert die Benutzeroberfläche für Benutzer, die die verwaltete Anwendung über das Portal erstellen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Definition für die verwaltete Anwendung veröffentlicht. Als Nächstes erfahren Sie, wie Sie eine Instanz dieser Definition bereitstellen.

> [!div class="nextstepaction"]
> [Schnellstart: Bereitstellen einer Dienstkatalog-App](deploy-service-catalog-quickstart.md)