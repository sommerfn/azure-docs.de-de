---
title: Verwenden von von Azure verwalteten Identitäten zum Erstellen von Umgebungen in DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie verwaltete Identitäten in Azure verwenden, um Umgebungen in einem Lab in Azure DevTest Labs bereitzustellen.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: spelluru
ms.openlocfilehash: a4ba4206c01e492f2ae980c5806de1e72c7051c3
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931153"
---
# <a name="use-azure-managed-identities-to-deploy-environments-in-a-lab"></a>Verwenden von verwalteten Azure-Identitäten zum Bereitstellen von Umgebungen in einem Lab 
Als Lab-Besitzer können Sie eine verwaltete Identität verwenden, um Umgebungen in einem Lab bereitzustellen. Diese Funktion ist in Szenarien hilfreich, in denen die Umgebung Verweise auf Azure-Ressourcen wie Schlüsseltresore, freigegebene Imagekataloge und Netzwerke enthält oder besitzt, die außerhalb der Ressourcengruppe der Umgebung liegen. Sie ermöglicht die Erstellung von Sandboxumgebungen, die nicht auf die Ressourcengruppe dieser Umgebung beschränkt sind.

> [!NOTE]
> Zurzeit wird eine einzige vom Benutzer zugewiesene Identität pro Lab unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen
- [Erstellen, Auflisten, Löschen oder Zuweisen einer Rolle zu einer vom Benutzer zugewiesenen verwalteten Identität über das Azure-Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). 

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt verwenden Sie als Lab-Besitzer das Azure-Portal, um dem Lab eine vom Benutzer verwaltete Identität hinzuzufügen. 

1. Wählen Sie auf der Seite des Labs die Option **Konfiguration und Richtlinien** aus. 
1. Wählen Sie im Abschnitt **Einstellungen** die Option **Identität** aus.
1. Wählen Sie auf der Symbolleiste **Hinzufügen** aus, um eine vom Benutzer zugewiesene Identität hinzuzufügen. 
1. Wählen Sie eine **Identität** aus einer voraufgefüllten Dropdownliste aus.
1. Klicken Sie auf **OK**.

    ![Hinzufügen einer vom Benutzer verwalteten Identität](./media/use-managed-identities-environments/add-user-managed-identity.png)
2. Die hinzugefügte vom Benutzer verwaltete Identität wird in der Liste angezeigt. 

    ![Vom Benutzer verwaltete Identität in der Liste](./media/use-managed-identities-environments/identity-in-list.png)

Nach dem Speichern verwendet das Lab diese Identität während der Bereitstellung aller Lab-Umgebungen. Sie können auch auf die Identitätsressource in Azure zugreifen, indem Sie die Identität in der Liste auswählen. 

Der Lab-Besitzer muss beim Bereitstellen einer Umgebung nichts Besonderes tun, solange die dem Lab hinzugefügte Identität über Berechtigungen für die externen Ressourcen verfügt, auf die die Umgebung zugreifen muss. 

Wenn Sie die dem Lab zugewiesene vom Benutzer verwaltete Identität ändern möchten, entfernen Sie zuerst die dem Lab angefügte Identität, und fügen Sie dann dem Lab eine weitere hinzu. Um eine dem Lab angefügte Identität zu entfernen, wählen Sie **... (Auslassungszeichen)** aus, und klicken Sie auf **Entfernen**. 

![Vom Benutzer verwaltete Identität in der Liste](./media/use-managed-identities-environments/replace-identity.png)  

## <a name="use-api"></a>Verwenden der API

1. Notieren Sie sich nach dem Erstellen einer Identität die Ressourcen-ID dieser Identität. Sie sollte in etwa wie das folgende Beispiel aussehen: 

    `/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`.
1. Führen Sie eine PUT HTTPS-Methode aus, um dem Lab eine neue `ServiceRunner`-Ressource hinzuzufügen, ähnlich wie im folgenden Beispiel. Die Service Runner-Ressource ist eine Proxyressource zum Verwalten und Steuern verwalteter Identitäten in DevTest Labs. Der Name des Service Runners kann ein beliebiger gültiger Name sein, aber es wird empfohlen, dass Sie den Namen der verwalteten Identitätsressource verwenden. 
 
    ```json
    PUT https://management.azure.com/subscriptions/{subId}/resourceGroups/{rg}/providers/Microsoft.Devtestlab/labs/{yourlabname}/serviceRunners/{serviceRunnerName}

    {
        "location": "{location}",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "[userAssignedIdentityResourceId]":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
          
    }
    ```
 
    Hier sehen Sie ein Beispiel: 

    ```json
    PUT https://management.azure.com/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.Devtestlab/labs/mylab/serviceRunners/sampleuseridentity

    {
        "location": "eastus",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/sampleuseridentity":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
    }
    ```
 
Sobald die vom Benutzer zugewiesene Identität dem Lab hinzugefügt wurde, wird Sie vom Azure DevTest Labs-Dienst bei der Bereitstellung von Azure Resource Manager-Umgebungen verwendet. Wenn z. B. Ihre Resource Manager-Vorlage auf ein Image aus einem externen freigegebenen Imagekatalog zugreifen muss, stellen Sie sicher, dass die Identität, die Sie dem Lab hinzugefügt haben, über mindestens erforderliche Berechtigungen für die freigegebene Imagekatalogressource verfügt. 
