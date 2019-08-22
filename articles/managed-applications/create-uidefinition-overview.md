---
title: Die Datei „CreateUiDefitinion.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie Benutzeroberflächendefinitionen für verwaltete Azure-Anwendungen erstellen.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: 292f2995e7ff1f56c306b8c9859bdb323f21762d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68847609"
---
# <a name="createuidefitinionjson-for-azure-managed-applications-create-experience"></a>Die Datei „CreateUiDefitinion.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen

In diesem Dokument werden die grundlegenden Konzepte der Datei **CreateUiDefinition.json** vorgestellt, die das Azure-Portal zum Definieren der Benutzeroberfläche zum Erstellen einer verwalteten Anwendung verwendet.

Die Vorlage sieht wie folgt aus:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { },
      "resourceTypes": [ ]
   }
}
```

Ein CreateUiDefinition-Element enthält immer drei Eigenschaften: 

* handler
* version
* parameters

Der Handler muss immer `Microsoft.Azure.CreateUIDef` lauten, und die neueste unterstützte Version ist `0.1.2-preview`.

Das Schema der parameters-Eigenschaft hängt von der Kombination aus den angegebenen Werten für „handler“ und „version“ ab. Für verwaltete Anwendungen lauten die unterstützten Eigenschaften `basics`, `steps` und `outputs`. Die Eigenschaften „basics“ und „steps“ enthalten die [Elemente ](create-uidefinition-elements.md) (wie Textfelder und Dropdownfelder), die im Azure-Portal angezeigt werden sollen. Mit der outputs-Eigenschaft werden die Ausgabewerte der angegebenen Elemente den Parametern der Azure Resource Manager-Bereitstellungsvorlage zugeordnet.

Die Aufnahme von `$schema` wird empfohlen, ist aber optional. Wenn ein Wert angegeben wird, muss der Wert für `version` der Version im `$schema`-URI entsprechen.

Sie können einen JSON-Editor zum Erstellen Ihrer Benutzeroberflächendefinition verwenden, und Sie können sie dann in der [Sandbox zum Erstellen der Benutzeroberflächendefinition](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) testen, um eine Vorschau davon anzuzeigen. Weitere Informationen zur Sandbox finden Sie unter [Testen Ihrer Portaloberfläche für Azure Managed Applications](test-createuidefinition.md).

## <a name="basics"></a>Grundlagen

Bei „basics“ handelt es sich um den ersten Schritt, der erstellt wird, wenn das Azure-Portal die Datei analysiert. Das Portal zeigt nicht nur die in `basics` angegebenen Elemente an, sondern fügt zusätzlich Elemente für Benutzer zum Auswählen des Abonnements, der Ressourcengruppe und des Standort für die Bereitstellung ein. Elemente, die bereitstellungsweite Parameter abfragen (wie den Namen eines Clusters oder Administratoranmeldeinformationen), sollten nach Möglichkeit in diesem Schritt enthalten sein.

Wenn das Verhalten eines Elements von dem Abonnement, der Ressourcengruppe oder dem Standort des Benutzers abhängig ist, kann dieses Element nicht in „basics“ verwendet werden. **Microsoft.Compute.SizeSelector** ist bei der Ermittlung der Liste verfügbarer Größen beispielsweise vom Abonnement und Standort des Benutzers abhängig. Daher kann **Microsoft.Compute.SizeSelector** nur in „steps“ verwendet werden. Im Allgemeinen können nur Elemente im **Microsoft.Common**-Namespace in „basics“ verwendet werden. Dennoch sind einige Elemente in anderen Namespaces (wie **Microsoft.Compute.Credentials**) zulässig, die nicht vom Kontext des Benutzers abhängig sind.

## <a name="steps"></a>Schritte

Die steps-Eigenschaft kann null oder mehr zusätzliche steps-Elemente für die Anzeige nach „basics“ enthalten, von denen jeder mindestens ein Element enthält. Ziehen Sie das Hinzufügen von steps-Elementen pro Rolle oder Ebene der bereitgestellten Anwendung in Betracht. Fügen Sie beispielsweise ein „steps“-Element für Masterknoteneingaben und ein „steps“-Element für die Workerknoten in einem Cluster hinzu.

## <a name="outputs"></a>Ausgaben

Mit der `outputs`-Eigenschaft ordnet das Azure-Portal Elemente aus `basics` und `steps` den Parametern der Azure Resource Manager-Bereitstellungsvorlage zu. Die Schlüssel dieses Wörterbuchs stellen die Namen der Vorlagenparameter dar, und die Werte sind Eigenschaften der Ausgabeobjekte von den referenzierten Elementen.

Um den Ressourcennamen der verwalteten Anwendung festzulegen, müssen Sie einen Wert mit dem Namen `applicationResourceName` in die Ausgabeeigenschaft eingeben. Wenn Sie diesen Wert nicht festlegen, weist die Anwendung eine GUID für den Namen zu. Sie können ein Textfeld in die Benutzeroberfläche aufnehmen, das einen Namen vom Benutzer anfordert.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="resource-types"></a>Ressourcentypen

Um die verfügbaren Standorte nach denen zu filtern, die die bereitzustellenden Ressourcentypen unterstützen, geben Sie ein Array der Ressourcentypen an. Wenn Sie mehrere Ressourcentypen angeben, werden nur die Standorte zurückgegeben, die alle Ressourcentypen unterstützen. Diese Eigenschaft ist optional.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
      "resourceTypes": ["Microsoft.Compute/disks"],
      "basics": [
        ...
```  

## <a name="functions"></a>Functions

„CreateUiDefinition“ stellt [Funktionen](create-uidefinition-functions.md) zum Arbeiten mit den Eingaben und Ausgaben von Elementen sowie Features wie konditionelle Abschnitte bereit. Diese Funktionen sind sowohl im Hinblick auf die Syntax als auch auf die Funktionalität den Vorlagenfunktionen von Azure Resource Manager ähnlich.

## <a name="next-steps"></a>Nächste Schritte

Die Datei „createUiDefinition.json“ besitzt selbst ein einfaches Schema. Die tatsächliche Tiefe ergibt sich aus allen unterstützten Elementen und Funktionen. Diese Elemente werden in den folgenden Dokumenten ausführlicher beschrieben:

- [CreateUiDefinition-Elemente](create-uidefinition-elements.md)
- [Funktionen](create-uidefinition-functions.md)

Ein aktuelles JSON-Schema für createUiDefinition ist unter folgendem Link verfügbar: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Ein Beispiel für eine Benutzeroberflächendatei finden Sie unter [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
