---
title: Onboarding von Ressourcen mit benutzerdefinierten Azure-Anbietern
description: Das Onboarding von Ressourcen durch benutzerdefinierte Anbieter ermöglicht es Ihnen, bestehende Azure-Ressourcen zu ändern und zu erweitern.
services: managed-applications
ms.service: managed-applications
ms.topic: tutorial
ms.author: jobreen
author: jjbfour
ms.date: 09/17/2019
ms.openlocfilehash: 45086987d5ba3a619028ced798712f2870c6d487
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73826781"
---
# <a name="tutorial-resource-onboarding-with-azure-custom-providers"></a>Tutorial: Onboarding von Ressourcen mit benutzerdefinierten Azure-Anbietern

In diesem Tutorial werden Sie für Azure einen benutzerdefinierten Ressourcenanbieter bereitstellen, der die Azure Resource Manager-API um den Ressourcentyp „Microsoft.CustomProviders/associations“ erweitert. In diesem Tutorial wird gezeigt, wie Sie bestehende Ressourcen, die sich außerhalb der Ressourcengruppe befinden, in der sich die benutzerdefinierte Anbieterinstanz befindet, erweitern können. In diesem Tutorial wird der benutzerdefinierte Ressourcenanbieter von einer Azure-Logik-App unterstützt, aber Sie können jeden öffentlichen API-Endpunkt verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial müssen Sie mit Folgendem vertraut sein:

* Funktionen von [benutzerdefinierten Azure-Anbietern](custom-providers-overview.md).
* Grundlegende Informationen zum [Onboarding von Ressourcen mit benutzerdefinierten Anbietern](concepts-custom-providers-resourceonboarding.md).

## <a name="get-started-with-resource-onboarding"></a>Erste Schritte für das Onboarding von Ressourcen

In diesem Tutorial gibt es zwei Komponenten, die bereitgestellt werden müssen: der benutzerdefinierte Anbieter und die Zuordnung. Sie können zur Vereinfachung des Prozesses optional eine einzelne Vorlage verwenden, die beides bereitstellt.

Die Vorlage verwendet folgende Ressourcen:

* Microsoft.CustomProviders/resourceProviders
* Microsoft.Logic/workflows
* Microsoft.CustomProviders/associations

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "allowedValues": [
                "australiaeast",
                "eastus",
                "westeurope"
            ],
            "metadata": {
                "description": "Location for the resources."
            }
        },
        "logicAppName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the logic app to be created."
            }
        },
        "customResourceProviderName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the custom provider to be created."
            }
        },
        "customResourceProviderId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The resource ID of an existing custom provider. Provide this to skip deployment of new logic app and custom provider."
            }
        },
        "associationName": {
            "type": "string",
            "defaultValue": "myAssociationResource",
            "metadata": {
                "description": "Name of the custom resource that is being created."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2017-05-10",
            "condition": "[empty(parameters('customResourceProviderId'))]",
            "name": "customProviderInfrastructureTemplate",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "logicAppName": {
                            "type": "string",
                            "defaultValue": "[parameters('logicAppName')]"
                        }
                    },
                    "resources": [
                        {
                            "type": "Microsoft.Logic/workflows",
                            "apiVersion": "2017-07-01",
                            "name": "[parameters('logicAppName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "state": "Enabled",
                                "definition": {
                                    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
                                    "actions": {
                                        "Switch": {
                                            "cases": {
                                                "Case": {
                                                    "actions": {
                                                        "CreateCustomResource": {
                                                            "inputs": {
                                                                "body": {
                                                                    "properties": "@addProperty(triggerBody().Body['properties'], 'myDynamicProperty', 'myDynamicValue')"
                                                                },
                                                                "statusCode": 200
                                                            },
                                                            "kind": "Http",
                                                            "type": "Response"
                                                        }
                                                    },
                                                    "case": "CREATE"
                                                }
                                            },
                                            "default": {
                                                "actions": {
                                                    "DefaultHttpResponse": {
                                                        "inputs": {
                                                            "statusCode": 200
                                                        },
                                                        "kind": "Http",
                                                        "type": "Response"
                                                    }
                                                }
                                            },
                                            "expression": "@triggerBody().operationType",
                                            "type": "Switch"
                                        }
                                    },
                                    "contentVersion": "1.0.0.0",
                                    "outputs": {},
                                    "parameters": {},
                                    "triggers": {
                                        "CustomProviderWebhook": {
                                            "inputs": {
                                                "schema": {}
                                            },
                                            "kind": "Http",
                                            "type": "Request"
                                        }
                                    }
                                }
                            }
                        },
                        {
                            "type": "Microsoft.CustomProviders/resourceProviders",
                            "apiVersion": "2018-09-01-preview",
                            "name": "[parameters('customResourceProviderName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "resourceTypes": [
                                    {
                                        "name": "associations",
                                        "mode": "Secure",
                                        "routingType": "Webhook,Cache,Extension",
                                        "endpoint": "[[listCallbackURL(concat(resourceId('Microsoft.Logic/workflows', parameters('logicAppName')), '/triggers/CustomProviderWebhook'), '2017-07-01').value]"
                                    }
                                ]
                            }
                        }
                    ],
                    "outputs": {
                        "customProviderResourceId": {
                            "type": "string",
                            "value": "[resourceId('Microsoft.CustomProviders/resourceProviders', parameters('customResourceProviderName'))]"
                        }
                    }
                }
            }
        },
        {
            "type": "Microsoft.CustomProviders/associations",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('associationName')]",
            "location": "global",
            "properties": {
                "targetResourceId": "[if(empty(parameters('customResourceProviderId')), reference('customProviderInfrastructureTemplate').outputs.customProviderResourceId.value, parameters('customResourceProviderId'))]",
                "myCustomInputProperty": "myCustomInputValue",
                "myCustomInputObject": {
                    "Property1": "Value1"
                }
            }
        }
    ],
    "outputs": {
        "associationResource": {
            "type": "object",
            "value": "[reference(parameters('associationName'), '2018-09-01-preview', 'Full')]"
        }
    }
}
```

### <a name="deploy-the-custom-provider-infrastructure"></a>Bereitstellen der Infrastruktur für benutzerdefinierte Anbieter

Der erste Teil der Vorlage stellt die Infrastruktur des benutzerdefinierten Anbieters bereit. Diese Infrastruktur definiert die Auswirkung der Zuordnungsressource. Wenn Sie mit benutzerdefinierten Anbietern nicht vertraut sind, finden Sie weitere Informationen unter [Grundlagen zu benutzerdefinierten Anbietern](custom-providers-overview.md).

Lassen Sie uns die Infrastruktur des benutzerdefinierten Anbieters bereitstellen. Sie können die vorherige Vorlage entweder kopieren, speichern und bereitstellen oder mithilfe des Azure-Portals die Infrastruktur mitverfolgen und bereitstellen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Suchen Sie in **Alle Dienste** nach **Vorlagen**, oder verwenden Sie das Hauptsuchfeld:

   ![Suchen nach Vorlagen](media/custom-providers-resource-onboarding/templates.png)

3. Wählen Sie im Bereich **Vorlagen** die Option **Hinzufügen** aus:

   ![Auswahl von „Hinzufügen“](media/custom-providers-resource-onboarding/templatesadd.png)

4. Geben Sie unter **Allgemein** einen **Namen** und eine **Beschreibung** für die neue Vorlage ein:

   ![Vorlagenname und -beschreibung](media/custom-providers-resource-onboarding/templatesdescription.png)

5. Erstellen Sie die Resource Manager-Vorlage, indem Sie sie in die JSON-Vorlage aus dem Abschnitt „Erste Schritte für das Onboarding von Ressourcen“ in diesem Artikel kopieren:

   ![Erstellen einer Resource Manager-Vorlage](media/custom-providers-resource-onboarding/templatesarmtemplate.png)

6. Wählen Sie **Hinzufügen** aus, um die Vorlage zu erstellen. Wenn die neue Vorlage nicht angezeigt wird, wählen Sie **Aktualisieren** aus.

7. Wählen Sie die neu erstellte Vorlage und anschließend **Bereitstellen** aus:

   ![Auswählen der neu erstellten Vorlage und von „Bereitstellen“](media/custom-providers-resource-onboarding/templateselectspecific.png)

8. Geben Sie die Einstellungen für die erforderlichen Felder ein, und wählen Sie dann das Abonnement und die Ressourcengruppe aus. Sie können das Feld für die **ID des benutzerdefinierten Ressourcenanbieters** leer lassen.

   | Einstellungsname | Erforderlich? | BESCHREIBUNG |
   | ------------ | -------- | ----------- |
   | Location | Ja | Der Standort für die Ressourcen in der Vorlage. |
   | Logik-App-Name | Nein | Der Name der Logik-App. |
   | Name des benutzerdefinierten Ressourcenanbieters | Nein | Der Name des benutzerdefinierten Ressourcenanbieters. |
   | ID des benutzerdefinierten Ressourcenanbieters | Nein | Ein vorhandener benutzerdefinierter Ressourcenanbieter, der die Zuordnungsressource unterstützt. Wenn Sie hier einen Wert angeben, werden die Logik-App und die benutzerdefinierte Anbieterbereitstellung übersprungen. |
   | Zuordnungsname | Nein | Der Name der Zuordnungsressource. |

   Beispielparameter:

   ![Geben Sie Vorlagenparameter ein.](media/custom-providers-resource-onboarding/templatescustomprovider.png)

9. Wechseln Sie zur Bereitstellung, und warten Sie, bis sie abgeschlossen ist. Es sollte ein Screenshot angezeigt werden, der in etwa wie folgt aussieht. Die neue Zuordnungsressource sollte als Ausgabe angezeigt werden:

   ![Erfolgreiche Bereitstellung](media/custom-providers-resource-onboarding/customproviderdeployment.png)

   Hier ist die Ressourcengruppe mit der Auswahl **Ausgeblendete Typen anzeigen**:

   ![Bereitstellung eines benutzerdefinierten Anbieters](media/custom-providers-resource-onboarding/showhidden.png)

10. Untersuchen Sie die Registerkarte **Ausführungsverlauf** der Logik-App, um die Aufrufe für die Erstellung der Zuordnung anzuzeigen:

    ![Ausführungsverlauf der Logik-App](media/custom-providers-resource-onboarding/logicapprun.png)

## <a name="deploy-additional-associations"></a>Bereitstellen zusätzlicher Zuordnungen

Nachdem Sie die Infrastruktur des benutzerdefinierten Anbieters eingerichtet haben, können Sie problemlos weitere Zuordnungen bereitstellen. Die Ressourcengruppe für zusätzliche Zuordnungen muss nicht mit der Ressourcengruppe übereinstimmen, in der Sie die Infrastruktur des benutzerdefinierten Anbieters bereitgestellt haben. Um eine Zuordnung zu erstellen, benötigen Sie die Berechtigungen „Microsoft.CustomProviders/resourceproviders/write“ für die angegebene ID des benutzerdefinierten Ressourcenanbieters.

1. Wechseln Sie zur Ressource des benutzerdefinierten Anbieters **Microsoft.CustomProviders/resourceProviders** in der Ressourcengruppe der vorherigen Bereitstellung. Sie müssen das Kontrollkästchen **Ausgeblendete Typen anzeigen** aktivieren:

   ![Wechsel zur Ressource](media/custom-providers-resource-onboarding/showhidden.png)

2. Kopieren Sie die Ressourcen-ID-Eigenschaft des benutzerdefinierten Anbieters.

3. Suchen Sie in **Alle Dienste** nach **Vorlagen**, oder verwenden Sie das Hauptsuchfeld:

   ![Suchen nach Vorlagen](media/custom-providers-resource-onboarding/templates.png)

4. Wählen Sie die zuvor erstellte Vorlage und anschließend **Bereitstellen** aus:

   ![Zuvor erstellte Vorlage und anschließend „Bereitstellen“ auswählen](media/custom-providers-resource-onboarding/templateselectspecific.png)

5. Geben Sie die Einstellungen für die erforderlichen Felder ein, und wählen Sie dann das Abonnement und eine andere Ressourcengruppe aus. Geben Sie für die Einstellung für die **ID des benutzerdefinierten Ressourcenanbieters** die Ressourcen-ID ein, die Sie von dem benutzerdefinierten und zuvor bereitgestellten Anbieter kopiert haben.

6. Wechseln Sie zur Bereitstellung, und warten Sie, bis sie abgeschlossen ist. Es sollte jetzt nur noch die neue Zuordnungsressource bereitgestellt werden:

   ![Neue Zuordnungsressource](media/custom-providers-resource-onboarding/createdassociationresource.png)

Bei Bedarf können Sie zum **Ausführungsverlauf** der Logik-App zurückkehren und feststellen, dass ein weiterer Aufruf an die Logik-App erfolgt ist. Sie können die Logik-App aktualisieren, um zusätzliche Funktionen für jede erstellte Zuordnung zu erweitern.

## <a name="getting-help"></a>Hilfe

Wenn Sie Fragen zu benutzerdefinierten Azure-Anbietern haben, stellen Sie sie auf [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-custom-providers). Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag `azure-custom-providers` hinzu, um schnell eine Antwort zu erhalten!

