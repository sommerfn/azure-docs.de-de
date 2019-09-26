---
title: Verschieben eines virtuellen Azure-Netzwerks in eine andere Azure-Region im Azure-Portal
description: Verwenden einer Azure Resource Manager-Vorlage, um ein virtuelles Azure-Netzwerk im Azure-Portal aus einer Azure-Region in eine andere zu verschieben.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: 2a1ee358a6c97b721ec6f0da3eb70269239b0737
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077958"
---
# <a name="move-azure-virtual-network-to-another-region-using-the-azure-portal"></a>Verschieben eines virtuellen Azure-Netzwerks in eine andere Region im Azure-Portal

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen virtuellen Azure-Netzwerke (VNets) aus einer Region in eine andere verschieben möchten. Möglicherweise möchten Sie ein virtuelles Netzwerk mit der gleichen Konfiguration für Tests der Verfügbarkeit Ihres bestehenden virtuellen Netzwerks erstellen. Möglicherweise möchten Sie auch im Rahmen der Planung der Notfallwiederherstellung ein virtuelles Produktionsnetzwerk in eine andere Region verschieben.

Sie können zum Verschieben des virtuellen Netzwerks in eine andere Region eine Azure Resource Manager-Vorlage verwenden. Dazu exportieren Sie das virtuelle Netzwerk in eine Vorlage, ändern die Parameter entsprechend der Zielregion und stellen die Vorlage dann in der neuen Region bereit.  Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass sich das virtuelle Azure-Netzwerk in der Azure-Region befindet, aus der Sie es verschieben möchten.

- Damit Sie ein virtuelles Netzwerk exportieren und eine Vorlage bereitstellen können, um ein virtuelles Netzwerk in einer anderen Region zu erstellen, benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.

- Peerings virtueller Netzwerke werden nicht neu erstellt und schlagen fehl, wenn sie noch in der Vorlage vorhanden sind.  Sie müssen alle Peers virtueller Netzwerke vor dem Export der Vorlage entfernen und Peers nach dem Verschieben des virtuellen Netzwerks erneut einrichten.
    
- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout schließt u. a. Load Balancers, Netzwerksicherheitsgruppen (NSGs) und öffentliche IP-Adressen ein.

- Vergewissern Sie sich, dass Sie mit Ihrem Azure-Abonnement virtuelle Netzwerke in der gewählten Zielregion erstellen können. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement über genügend Ressourcen verfügt, um das Hinzufügen virtueller Netzwerke für diesen Prozess zu unterstützen.  Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Vorbereiten und Verschieben
In den folgenden Schritten wird gezeigt, wie Sie das virtuelle Netzwerk auf das Verschieben mit einer Resource Manager-Vorlage vorbereiten und das virtuelle Netzwerk dann im Portal in die Zielregion verschieben.

### <a name="export-the-template-and-deploy-from-the-portal"></a>Exportieren der Vorlage und Bereitstellen im Portal

1. Melden Sie sich bei [Azure-Portal](http://portal.azure.com) > **Ressourcengruppen** an.
2. Wechseln Sie zur Ressourcengruppe, die das virtuelle Netzwerk enthält, und klicken Sie darauf.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie auf dem Blatt **Vorlage exportieren** die Option **Bereitstellen**.
5. Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.
6. Um den Parameter für den Namen des virtuellen Netzwerks zu bearbeiten, ändern Sie unter **parameters** die Eigenschaft **value**:
    
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
7. Ändern Sie den Wert für den Namen des virtuellen Netzwerks der Quelle im Editor in den Namen Ihrer Wahl für das Ziel-VNet. Stellen Sie sicher, dass Sie den Namen in Anführungszeichen einschließen.

8.  Klicken Sie im Editor auf **Speichern**.

9.  Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen. 

10. Zum Bearbeiten der Zielregion, in die das VNet verschoben wird, ändern Sie unter **resources** die Eigenschaft **location**:

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
 
11. Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **Central US** = **centralus**.
 
12. Sie können wahlweise auch andere Parameter in der Vorlage ändern, die abhängig von Ihren Anforderungen optional sind:
    
    * **Adressraum**: Der Adressraum des VNet kann vor dem Speichern geändert werden, indem der Abschnitt **resources** > **addressSpace** und die Eigenschaft **addressPrefixes** in der Datei **template.json** geändert werden:

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

    * **Subnetz**: Der Subnetzname und der Subnetzadressraum können geändert oder hinzugefügt werden, indem Sie den Abschnitt **subnets** der Datei **template.json** ändern. Der Name des Subnetzes kann geändert werden, indem Sie die Eigenschaft **name** ändern. Der Subnetzadressraum kann geändert werden, indem Sie die Eigenschaft **addressPrefix** in der Datei **template.json** ändern:

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

         Die Datei **template.json** muss zum Ändern des Adresspräfixes an zwei Stellen bearbeitet werden: im oben aufgeführten Abschnitt und im unten aufgeführten Abschnitt **type**.  Ändern Sie die Eigenschaft **addressPrefix** so, dass Sie mit der oben genannten Eigenschaft übereinstimmt:

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

13. Klicken Sie im Online-Editor auf **Speichern**.

14. Klicken Sie auf **GRUNDLAGEN** > **Abonnement**,um das Abonnement auszuwählen, in dem das Ziel-VNet bereitgestellt wird.

15. Klicken Sie auf **GRUNDLAGEN** > **Ressourcengruppe**,um die Ressourcengruppe auszuwählen, in der das Ziel-VNet bereitgestellt wird.  Sie können auf **Neu erstellen** klicken, um eine neue Ressourcengruppe für das Ziel-VNET zu erstellen.  Stellen Sie sicher, dass der Name nicht mit dem Namen der Quellressourcengruppe des vorhandenen VNet übereinstimmt. 

16. Stellen Sie sicher, dass **GRUNDLAGEN** > **Standort** auf den Zielstandort festgelegt ist, an dem das VNet bereitgestellt werden soll.

17. Stellen Sie unter **EINSTELLUNGEN** sicher, dass der Name mit dem Namen übereinstimmt, den Sie oben im Parameter-Editor eingegeben haben.

18. Aktivieren Sie das Kontrollkästchen unter **NUTZUNGSBEDINGUNGEN**.

19. Klicken Sie auf die Schaltfläche **Kaufen**, um das virtuelle Zielnetzwerk bereitzustellen.

## <a name="discard"></a>Verwerfen 

Wenn Sie das virtuelle Netzwerk des Ziels verwerfen möchten, löschen Sie die Ressourcengruppe, die das virtuelle Netzwerk des Ziels enthält.  Wählen Sie hierzu im Portal die Ressourcengruppe im Dashboard aus, und klicken Sie oben auf der Übersichtsseite auf **Löschen**.

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und das Verschieben des virtuellen Netzwerks abzuschließen, löschen Sie das virtuelle Netzwerk oder die Ressourcengruppe der Quelle. Wählen Sie hierzu im Portal das virtuelle Netzwerk oder die Ressourcengruppe im Dashboard aus, und klicken Sie oben auf jeder Seite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein virtuelles Azure-Netzwerk aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
