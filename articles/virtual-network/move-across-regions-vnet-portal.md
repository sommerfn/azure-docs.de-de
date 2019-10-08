---
title: Verschieben eines virtuellen Azure-Netzwerks in eine andere Azure-Region im Azure-Portal.
description: Verschieben Sie ein virtuelles Azure-Netzwerk aus einer Azure-Region in eine andere, indem Sie eine Resource Manager-Vorlage und das Azure-Portal verwenden.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: d6f417e53e7d7a1a242a0c0dc56c2356f78f5344
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828953"
---
# <a name="move-an-azure-virtual-network-to-another-region-by-using-the-azure-portal"></a>Verschieben eines virtuellen Azure-Netzwerks in eine andere Region im Azure-Portal

Es gibt verschiedene Szenarien, in denen ein vorhandenes virtuelles Azure-Netzwerk aus einer Region in eine andere verschoben wird. Möglicherweise möchten Sie für Testzwecke oder Verfügbarkeitsprüfungen ein virtuelles Netzwerk erstellen, das dieselbe Konfiguration wie Ihr vorhandenes virtuelles Netzwerk aufweist. Oder Sie möchten im Rahmen der Planung der Notfallwiederherstellung ein virtuelles Produktionsnetzwerk in eine andere Region verschieben.

Sie können zum Verschieben des virtuellen Netzwerks in eine andere Region eine Azure Resource Manager-Vorlage verwenden. Dazu exportieren Sie das virtuelle Netzwerk in eine Vorlage, ändern die Parameter entsprechend der Zielregion und stellen die Vorlage dann in der neuen Region bereit. Weitere Informationen zu Resource Manager-Vorlagen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass sich das virtuelle Netzwerk in der Azure-Region befindet, aus der Sie es verschieben möchten.

- Damit Sie ein virtuelles Netzwerk exportieren und eine Vorlage bereitstellen können, um ein virtuelles Netzwerk in einer anderen Region zu erstellen, benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.

- Peerings virtueller Netzwerke werden nicht neu erstellt und schlagen fehl, wenn sie noch in der Vorlage vorhanden sind. Vor dem Exportieren der Vorlage müssen Sie alle Peerings virtueller Netzwerke entfernen. Sie können diese nach dem Verschieben des virtuellen Netzwerks wiederherstellen.

- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout schließt u. a. Load Balancers, Netzwerksicherheitsgruppen (NSGs) und öffentliche IP-Adressen ein.

- Vergewissern Sie sich, dass Sie mit Ihrem Azure-Abonnement virtuelle Netzwerke in der Zielregion erstellen können. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement über genügend Ressourcen verfügt, um das Hinzufügen virtueller Netzwerke für diesen Prozess zu unterstützen. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-for-the-move"></a>Vorbereiten der Verschiebung
In diesem Abschnitt bereiten Sie das virtuelle Netzwerk für die Verschiebung vor, indem Sie eine Resource Manager-Vorlage verwenden. Anschließend verschieben Sie das virtuelle Netzwerk mithilfe des Azure-Portals in die Zielregion.

Gehen Sie folgendermaßen vor, um das virtuelle Netzwerk zu exportieren und das virtuelle Zielnetzwerk mithilfe des Azure-Portals bereitzustellen:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann **Ressourcengruppen** aus.
1. Ermitteln Sie die Ressourcengruppe, die das virtuelle Netzwerk enthält, und wählen Sie sie aus.
1. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
1. Wählen Sie im Bereich **Vorlage exportieren** die Option **Bereitstellen** aus.
1. Wählen Sie **Vorlage** > **Parameter bearbeiten** aus, um die Datei *parameters.json* im Online-Editor zu öffnen.
1. Um den Parameter für den Namen des virtuellen Netzwerks zu bearbeiten, ändern Sie unter **parameters** die Eigenschaft **value**:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualNetworks_myVNET1_name": {
                "value": "<target-virtual-network-name>"
            }
        }
    }
    ```

1. Ändern Sie im Editor den Namenswert des virtuellen Quellnetzwerks in den Namen Ihrer Wahl für das virtuelle Zielnetzwerk. Schließen Sie den Namen unbedingt in Anführungszeichen ein.

1. Wählen Sie im Editor die Option **Speichern** aus.

1. Wählen Sie **Vorlage** > **Parameter bearbeiten** aus, um die Datei *template.json* im Online-Editor zu öffnen.

1. Zum Bearbeiten der Zielregion, in die das virtuelle Netzwerk verschoben wird, ändern Sie im Online-Editor unter **resources** die Eigenschaft **location**:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```

1. Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Regionsname ohne Leerzeichen (Beispiel: **Central US (USA, Mitte)**  = **centralus**).

1. (Optional) Sie können abhängig von Ihren Anforderungen auch andere Parameter in der Vorlage ändern:

    * **Adressraum**: Bevor Sie die Datei speichern, können Sie den Adressraum des virtuellen Netzwerks ändern, indem Sie den Abschnitt **resources** > **addressSpace** und die Eigenschaft **addressPrefixes** ändern:

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },

        ```

    * **Subnetz**: Sie können den Subnetznamen und den Adressraum des Subnetzes ändern oder hinzufügen, indem Sie den Abschnitt **subnets** der Vorlage ändern. Sie können den Namen des Subnetzes ändern, indem Sie die Eigenschaft **name** ändern. Sie können den Adressraum des Subnetzes ebenfalls ändern, indem Sie die Eigenschaft **addressPrefix** ändern:

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

        Um das Adresspräfix in der Datei *template.json* zu ändern, bearbeiten Sie es an zwei Stellen: im Code im vorherigen Abschnitt und im Abschnitt **type** des folgenden Codes. Ändern Sie die Eigenschaft **addressPrefix** im folgenden Code so, dass sie mit der Eigenschaft **addressPrefix** im Code im vorherigen Abschnitt übereinstimmt.

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

1. Wählen Sie im Online-Editor die Option **Speichern** aus.

1. Um das Abonnement auszuwählen, in dem das virtuelle Zielnetzwerk bereitgestellt wird, wählen Sie **Grundeinstellungen** > **Abonnement** aus.

1. Um die Ressourcengruppe auszuwählen, in der das virtuelle Zielnetzwerk bereitgestellt wird, wählen Sie **Grundeinstellungen** > **Ressourcengruppe** aus. 

    Wenn Sie eine neue Ressourcengruppe für das virtuelle Zielnetzwerk erstellen müssen, wählen Sie **Neu erstellen** aus. Stellen Sie sicher, dass der Name nicht mit dem Namen der Quellressourcengruppe im vorhandenen virtuellen Netzwerk übereinstimmt.

1. Vergewissern Sie sich, dass **Grundeinstellungen** > **Standort** auf den Zielstandort festgelegt ist, an dem das virtuelle Netzwerk bereitgestellt werden soll.

1. Stellen Sie unter **Einstellungen** sicher, dass der Name mit dem Namen übereinstimmt, den Sie zuvor im Parameter-Editor eingegeben haben.

1. Aktivieren Sie das Kontrollkästchen **Geschäftsbedingungen**.

1. Klicken Sie auf die Schaltfläche **Kaufen**, um das virtuelle Zielnetzwerk bereitzustellen.

## <a name="delete-the-target-virtual-network"></a>Löschen des virtuellen Zielnetzwerks

Wenn Sie das virtuelle Zielnetzwerk verwerfen möchten, löschen Sie die Ressourcengruppe, die das virtuelle Zielnetzwerk enthält. Gehen Sie dazu wie folgt vor:
1. Wählen Sie im Dashboard des Azure-Portals die Ressourcengruppe aus.
1. Wählen Sie oben im Bereich **Übersicht** die Option **Löschen** aus.

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und die Verschiebung des virtuellen Netzwerks abzuschließen, löschen Sie das virtuelle Quellnetzwerk oder die Ressourcengruppe. Gehen Sie dazu wie folgt vor:
1. Wählen Sie im Dashboard des Azure-Portals das virtuelle Netzwerk oder die Ressourcengruppe aus.
1. Wählen Sie oben in jedem Bereich die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mithilfe des Azure-Portals ein virtuelles Azure-Netzwerk aus einer Region in eine andere verschoben und dann die nicht benötigten Quellressourcen bereinigt. Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie in folgenden Artikeln:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben von virtuellen Azure-Computern in eine andere Azure-Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
