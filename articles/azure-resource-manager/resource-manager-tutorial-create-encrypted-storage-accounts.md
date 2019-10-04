---
title: Nutzen der Azure Resource Manager-Vorlagenreferenz | Microsoft-Dokumentation
description: Nutzen Sie die Azure Resource Manager-Vorlagenreferenz, um eine Vorlage zum Bereitstellen eines verschlüsselten Speicherkontos zu erstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 81c02c5e39e23b49291561821b0732227feb5c05
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103644"
---
# <a name="tutorial-utilize-the-azure-resource-manager-template-reference"></a>Tutorial: Nutzen der Azure Resource Manager-Vorlagenreferenz

Hier erfahren Sie, wie Sie die Vorlagenschemainformationen ermitteln und anhand der Informationen Azure Resource Manager-Vorlagen erstellen.

In diesem Tutorial verwenden Sie eine Basisvorlage aus Azure-Schnellstartvorlagen. Anhand der Vorlagenreferenzdokumentation passen Sie die Vorlage an, um ein verschlüsseltes Speicherkonto zu erstellen.

![Resource Manager: Vorlagenreferenz, Bereitstellen eines verschlüsselten Speicherkontos](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-tutorial-deploy-encrypted-storage-account.png)

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Kennenlernen der Vorlage
> * Suchen der Vorlagenreferenz
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der [Erweiterung „Azure Resource Manager-Tools“](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in dieser Schnellstartanleitung verwendete Vorlage heißt [Standardspeicherkonto erstellen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Die Vorlage definiert eine Azure Storage-Kontoressource.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="understand-the-schema"></a>Grundlegendes zum Schema

1. Reduzieren Sie in VS Code die Vorlage auf die Stammebene. Sie besitzen die einfachste Struktur mit den folgenden Elementen:

    ![Einfachste Struktur der Resource Manager-Vorlage](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

    * **$schema**: Geben Sie den Speicherort der JSON-Schemadatei an, die die Version der Vorlagensprache beschreibt.
    * **contentVersion**: Geben Sie einen beliebigen Wert für dieses Element an, um wichtige Änderungen an der Vorlage zu dokumentieren.
    * **parameters**: Geben Sie die Werte an, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anzupassen.
    * **variables**: Geben Sie die Werte an, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen.
    * **resources**: Geben Sie die Ressourcentypen an, die in einer Ressourcengruppe bereitgestellt oder aktualisiert werden.
    * **outputs**: Geben Sie die Werte an, die nach der Bereitstellung zurückgegeben werden.

2. Erweitern Sie **Ressourcen**. Dort ist eine Ressource vom Typ `Microsoft.Storage/storageAccounts` definiert. Die Vorlage erstellt ein nicht verschlüsseltes Storage-Konto.

    ![Resource Manager-Vorlage, Speicherkontodefinition](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resource.png)

## <a name="find-the-template-reference"></a>Suchen der Vorlagenreferenz

1. Navigieren Sie zu [Azure-Vorlagenreferenz](https://docs.microsoft.com/azure/templates/).
2. Geben Sie **Speicherkonten** im Feld **Filtern nach Titel** ein.
3. Wählen Sie **Referenz/Vorlagenreferenz/Speicher/&lt;Version>/Speicherkonten** wie im folgenden Screenshot gezeigt aus:

    ![Resource Manager: Vorlagenreferenz -> Speicherkonto](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts.png)

    Verwenden Sie die aktuellste Version, falls Sie nicht wissen, welche Version Sie wählen sollen.

4. Suchen Sie die verschlüsselungsbezogenen Definitionsinformationen.  

    ```json
    "encryption": {
      "services": {
        "blob": {
          "enabled": boolean
        },
        "file": {
          "enabled": boolean
        }
      },
      "keySource": "string",
      "keyvaultproperties": {
        "keyname": "string",
        "keyversion": "string",
        "keyvaulturi": "string"
      }
    },
    ```

    Auf der gleichen Webseite wird mit der folgenden Beschreibung bestätigt, dass das Objekt `encryption` zum Erstellen eines verschlüsselten Speicherkontos verwendet wird.

    ![Resource Manager: Vorlagenreferenz > Speicherkontoverschlüsselung](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption.png)

    Es gibt zwei Möglichkeiten zum Verwalten des Verschlüsselungsschlüssels. Mit Storage Service Encryption können von Microsoft verwaltete oder eigene Verschlüsselungsschlüssel verwendet werden. In diesem Tutorial verwenden Sie der Einfachheit halber die Option `Microsoft.Storage`, damit Sie keine Azure Key Vault-Instanz erstellen müssen.

    ![Resource Manager: Vorlagenreferenz > Speicherkonto-Verschlüsselungsobjekt](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption-object.png)

    Ihr Verschlüsselungsobjekt sollte wie folgt aussehen:

    ```json
    "encryption": {
        "services": {
            "blob": {
                "enabled": true
            },
            "file": {
              "enabled": true
            }
        },
        "keySource": "Microsoft.Storage"
    }
    ```

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Ändern Sie die Vorlage in Visual Studio Code, sodass das Ressourcenelement wie folgt aussieht:

![Verschlüsselte Speicherkontoressourcen der Resource Manager-Vorlage](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie in der Visual Studio Code-Schnellstartanleitung im Abschnitt [Bereitstellen der Vorlage](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

Der folgende Screenshot zeigt den CLI-Befehl zum Auflisten des neu erstellten Speicherkontos, um zu sehen, dass die Verschlüsselung für den Blobspeicher aktiviert wurde.

![Verschlüsseltes Azure Resource Manager-Speicherkonto](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die Vorlagenreferenz zum Anpassen einer vorhandenen Vorlage verwenden. Wie Sie mehrere Speicherkontoinstanzen erstellen, erfahren Sie unter:

> [!div class="nextstepaction"]
> [Erstellen mehrerer Instanzen](./resource-manager-tutorial-create-multiple-instances.md)
