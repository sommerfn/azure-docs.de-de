---
title: Verschieben einer Azure-Netzwerksicherheitsgruppe (NSG) in eine andere Azure-Region mit dem Azure-Portal
description: Verwenden einer Azure Resource Manager-Vorlage, um eine Azure-Netzwerksicherheitsgruppe über das Azure-Portal aus einer Azure-Region in eine andere zu verschieben.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: 839e608aa4bba26712ae5b0c160da40db279bbc9
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219189"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-the-azure-portal"></a>Verschieben einer Azure-Netzwerksicherheitsgruppe (NSG) in eine andere Region mit dem Azure-Portal

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen NSGs aus einer Region in eine andere verschieben möchten. Beispielsweise könnte es sein, dass Sie für Testzwecke eine NSG mit derselben Konfiguration und denselben Sicherheitsregeln erstellen möchten. Möglicherweise möchten Sie eine NSG auch im Rahmen der Planung einer Notfallwiederherstellung in eine andere Region verschieben.

Azure-Sicherheitsgruppen können nicht aus einer Region in eine andere verschoben werden. Sie können jedoch eine Azure Resource Manager-Vorlage verwenden, um die vorhandene Konfiguration und die vorhandenen Sicherheitsregeln einer NSG zu exportieren.  Anschließend können Sie die Ressource in einer anderen Region stagen, indem Sie die NSG in eine Vorlage exportieren, die Parameter so ändern, dass sie der Zielregion entsprechen, und die Vorlage dann in der neuen Region bereitstellen.  Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Voraussetzungen

- Vergewissern Sie sich, dass sich die Azure-Netzwerksicherheitsgruppe in der Azure-Region befindet, aus der Sie sie verschieben möchten.

- Azure-Netzwerksicherheitsgruppen können nicht zwischen Regionen verschoben werden.  Sie müssen die neue NSG Ressourcen in der Zielregion zuordnen.

- Damit Sie eine NSG-Konfiguration exportieren und eine Vorlage bereitstellen können, um eine NSG in einer anderen Region zu erstellen, benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.

- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout umfasst Load Balancer, Netzwerksicherheitsgruppen, öffentliche IP-Adressen und virtuelle Netzwerke, ohne darauf beschränkt zu sein.

- Vergewissern Sie sich, dass Sie mit Ihrem Azure-Abonnement NSGs in der verwendeten Zielregion erstellen können. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement genügend Ressourcen hat, um das Hinzufügen von NSGs für diesen Prozess zu unterstützen.  Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Vorbereiten und Verschieben
In den folgenden Schritten wird gezeigt, wie Sie eine Netzwerksicherheitsgruppe vorbereiten, damit deren Konfiguration und Sicherheitsregeln mit einer Resource Manager-Vorlage verschoben werden können, und wie Sie die NSG-Konfiguration und -Sicherheitsregeln über das Portal in die Zielregion verschieben.


### <a name="export-the-template-and-deploy-from-the-portal"></a>Exportieren der Vorlage und Bereitstellen im Portal

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) > **Ressourcengruppen** an.
2. Suchen Sie die Ressourcengruppe, die die Quell-NSG enthält, und klicken Sie darauf.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie **Bereitstellen** auf dem Blatt **Vorlage exportieren** aus.
5. Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.
6. Um den Parameter für den Namen der NSG zu bearbeiten, ändern Sie Eigenschaft **value** unter **parameters**:

    ```json
            {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "networkSecurityGroups_myVM1_nsg_name": {
               "value": "<target-nsg-name>"
                }
               }
            }
    ```

7. Ändern Sie den für die Quell-NSG angegebenen Namen im Editor in den Namen für die Ziel-NSG. Stellen Sie sicher, dass Sie den Namen in Anführungszeichen einschließen.

8.  Klicken Sie im Editor auf **Speichern**.

9.  Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.

10. Um die Zielregion zu bearbeiten, in die die NSG-Konfiguration und -Sicherheitsregeln verschoben werden, ändern Sie im Online-Editor die Eigenschaft **location** unter **resources**:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2019-06-01",
            "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
            "location": "<target-region>",
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78",
             }
            }
           ]

    ```

11. Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **USA, Mitte** = **centralus**.

12. Sie können wahlweise auch andere Parameter in der Vorlage ändern. Diese sind abhängig von Ihren Anforderungen optional:

    * **Sicherheitsregeln**: Sie können festlegen, welche Regeln in der Ziel-NSG bereitgestellt werden, indem Sie dem Abschnitt **securityRules** in der Datei **template.json** Regeln hinzufügen oder Regeln aus diesem Abschnitt entfernen:

        ```json
           "resources": [
            {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2019-06-01",
            "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
            "location": "<target-region>",
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78",
                "securityRules": [
                    {
                        "name": "RDP",
                        "etag": "W/\"c630c458-6b52-4202-8fd7-172b7ab49cf5\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "3389",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 300,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                             }
                    },
                ]
            }
        ```

      Um das Hinzufügen oder Entfernen von Regeln in der Ziel-NSG abzuschließen, müssen Sie außerdem die benutzerdefinierten Regeltypen am Ende der Datei **template.json** im Format des folgenden Beispiels bearbeiten:

      ```json
           {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('networkSecurityGroups_myVM1_nsg_name'), '/Port_80')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVM1_nsg_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "protocol": "*",
                "sourcePortRange": "*",
                "destinationPortRange": "80",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 310,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
      ```

13. Klicken Sie im Online-Editor auf **Speichern**.

14. Klicken Sie auf **GRUNDEINSTELLUNGEN** > **Abonnement**, um das Abonnement auszuwählen, in dem die Ziel-NSG bereitgestellt wird.

15. Klicken Sie auf **GRUNDEINSTELLUNGEN** > **Ressourcengruppe**, um die Ressourcengruppe auszuwählen, in der die Ziel-NSG bereitgestellt wird.  Sie können auf **Neu erstellen** klicken, um eine neue Ressourcengruppe für die Ziel-NSG zu erstellen.  Stellen Sie sicher, dass der Name nicht mit dem Namen der Quellressourcengruppe der vorhandenen NSG übereinstimmt.

16. Stellen Sie sicher, dass **GRUNDEINSTELLUNGEN** > **Standort** auf den Zielstandort festgelegt ist, an dem die NSG bereitgestellt werden soll.

17. Stellen Sie unter **EINSTELLUNGEN** sicher, dass der Name mit dem Namen übereinstimmt, den Sie oben im Parameter-Editor eingegeben haben.

18. Aktivieren Sie das Kontrollkästchen unter **GESCHÄFTSBEDINGUNGEN**.

19. Klicken Sie auf die Schaltfläche **Erwerben**, um die Zielnetzwerksicherheitsgruppe bereitzustellen.

## <a name="discard"></a>Verwerfen

Wenn Sie die Ziel-NSG verwerfen möchten, löschen Sie die Ressourcengruppe, die die Ziel-NSG enthält.  Wählen Sie hierzu im Portal die Ressourcengruppe im Dashboard aus, und klicken Sie oben auf der Übersichtsseite auf **Löschen**.

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und das Verschieben der NSG abzuschließen, löschen Sie die Quell-NSG oder -ressourcengruppe. Wählen Sie hierzu im Portal die Netzwerksicherheitsgruppe oder Ressourcengruppe im Dashboard aus, und klicken Sie oben auf jeder Seite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Netzwerksicherheitsgruppe aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
