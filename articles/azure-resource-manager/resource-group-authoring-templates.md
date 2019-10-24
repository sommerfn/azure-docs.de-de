---
title: 'Azure Resource Manager-Vorlagen: Struktur und Syntax | Microsoft-Dokumentation'
description: Beschreibt die Struktur und die Eigenschaften der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: tomfitz
ms.openlocfilehash: e5ef3dcd7c2eec08237d5eb31fb95a0e450d9ac9
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286716"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen

In diesem Artikel wird die Struktur einer Azure Resource Manager-Vorlage beschrieben. Er zeigt die verschiedenen Abschnitte einer Vorlage und die Eigenschaften, die in diesen Abschnitten verfügbar sind.

Dieser Artikel richtet sich an Benutzer, die bereits Vorkenntnisse zu Resource Manager-Vorlagen haben. Er bietet detaillierte Informationen zur Struktur der Vorlage. Eine Einführung in die Erstellung einer Vorlage finden Sie unter [Azure Resource Manager-Vorlagen](template-deployment-overview.md).

## <a name="template-format"></a>Vorlagenformat

In der einfachsten Struktur weist eine Vorlage die folgenden Elemente auf:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| $schema |Ja |Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt.<br><br> Verwenden Sie Folgendes für Bereitstellungen von Ressourcengruppen: `https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`.<br><br>Verwenden Sie Folgendes für Bereitstellungen von Abonnements: `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`. |
| contentVersion |Ja |Version der Vorlage (z. B. 1.0.0.0). Sie können einen beliebigen Wert für dieses Element resources. Mit diesem Wert können Sie wichtige Änderungen in der Vorlage dokumentieren. Bei der Bereitstellung von Ressourcen mithilfe der Vorlage kann mit diesem Wert sichergestellt werden, dass die richtige Vorlage verwendet wird. |
| apiProfile |Nein | Eine API-Version, die als Sammlung von API-Versionen für Ressourcentypen dient. Verwenden Sie diesen Wert, um zu vermeiden, dass Sie API-Versionen für jede Ressource in der Vorlage angeben müssen. Wenn Sie eine API-Profilversion aber keine API-Version für den Ressourcentyp angeben, verwendet Resource Manager die API-Version für diesen Ressourcentyp, der im Profil bestimmt wurde.<br><br>Die API-Profileigenschaft ist besonders hilfreich, wenn Sie eine Vorlage in verschiedenen Umgebungen wie Azure Stack und der globalen Azure-Umgebung bereitstellen. Verwenden Sie die API-Profilversion, um sicherzustellen, dass Ihre Vorlage automatisch Versionen verwendet, die in beiden Umgebungen unterstützt werden. Eine Liste der im Profil definierten aktuellen API-Profilversionen und Ressourcen-API-Versionen finden Sie unter [API Profile (API-Profil)](https://github.com/Azure/azure-rest-api-specs/tree/master/profile).<br><br>Weitere Informationen finden Sie unter [Nachverfolgen von Versionen mithilfe von API-Profilen](templates-cloud-consistency.md#track-versions-using-api-profiles). |
| [parameters](#parameters) |Nein |Werte, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anpassen. |
| [variables](#variables) |Nein |Werte, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen. |
| [functions](#functions) |Nein |Benutzerdefinierte Funktionen, die in der Vorlage verfügbar sind. |
| [resources](#resources) |Ja |Ressourcentypen, die in einer Ressourcengruppe oder einem Abonnement bereitgestellt oder aktualisiert werden. |
| [outputs](#outputs) |Nein |Werte, die nach der Bereitstellung zurückgegeben werden. |

Jedes Element weist Eigenschaften auf, die Sie festlegen können. In diesem Artikel werden die Abschnitte der Vorlage ausführlicher beschrieben.

## <a name="parameters"></a>Parameter

Im Abschnitt „Parameter“ der Vorlage geben Sie an, welche Werte Sie beim Bereitstellen der Ressourcen eingeben können. Die Anzahl der Parameter in einer Vorlage ist auf 256 beschränkt. Sie können die Anzahl der Parameter verringern, indem Sie Objekte verwenden, die mehrere Eigenschaften enthalten.

Folgende Eigenschaften sind für einen Parameter verfügbar:

```json
"parameters": {
  "<parameter-name>" : {
    "type" : "<type-of-parameter-value>",
    "defaultValue": "<default-value-of-parameter>",
    "allowedValues": [ "<array-of-allowed-values>" ],
    "minValue": <minimum-value-for-int>,
    "maxValue": <maximum-value-for-int>,
    "minLength": <minimum-length-for-string-or-array>,
    "maxLength": <maximum-length-for-string-or-array-parameters>,
    "metadata": {
      "description": "<description-of-the parameter>"
    }
  }
}
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| parameter-name |Ja |Der Name des Parameters. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| type |Ja |Der Typ des Parameterwerts. Die zulässigen Typen und Werte sind **string**, **securestring**, **int**, **bool**, **object**, **secureObject** und **array**. Siehe [Datentypen](#data-types). |
| defaultValue |Nein |Der Standardwert für den Parameter, wenn kein Wert für den Parameter angegeben wird. |
| allowedValues |Nein |Ein Array der zulässigen Werte für den Parameter, um sicherzustellen, dass der richtige Wert angegeben wird. |
| minValue |Nein |Der Mindestwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| maxValue |Nein |Der Höchstwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| minLength |Nein |Die Mindestlänge der Parameter „string“, „securestring“ und „array“, einschließlich des angegebenen Werts. |
| maxLength |Nein |Die Höchstlänge der Parameter „string“, „securestring“ und „array“, einschließlich des angegebenen Werts. |
| description |Nein |Beschreibung des Parameters, der Benutzern im Portal angezeigt wird. Weitere Informationen finden Sie unter [Kommentare in Vorlagen](#comments). |

Beispiele für die Verwendung von Parametern finden Sie unter [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).

### <a name="data-types"></a>Datentypen

Für Integer, die als Inlineparameter übergeben werden, ist der Wertebereich möglicherweise durch das SDK oder Befehlszeilentool, das Sie zur Bereitstellung verwenden, eingeschränkt. Wenn Sie beispielsweise PowerShell zum Bereitstellen einer Vorlage verwenden, können Integertypen im Bereich von -2147483648 bis 2147483647 liegen. Um diese Einschränkung zu vermeiden, geben Sie große Werte in einer [Parameterdatei](resource-manager-parameter-files.md) an. Ressourcentypen wenden ihre eigenen Grenzwerte für Integereigenschaften an.

Schließen Sie Boolesche und Integerwerte in Ihrer Vorlage nicht in Anführungszeichen ein. Beginnen und beenden Sie Zeichenfolgenwerte mit doppelten Anführungszeichen.

Objekte beginnen mit einer linken geschweiften Klammer und enden mit einer rechten geschweiften Klammer. Arrays beginnen mit einer linken eckigen Klammer und enden mit einer rechten eckigen Klammer.

Sichere Zeichenfolgen und sichere Objekte können nach der Bereitstellung der Ressource nicht mehr gelesen werden.

Beispiele zum Formatieren von Datentypen finden Sie unter [Parametertypformate](resource-manager-parameter-files.md#parameter-type-formats).

## <a name="variables"></a>Variables

Im Abschnitt „variables“ erstellen Sie Werte, die in der ganzen Vorlage verwendet werden können. Sie müssen nicht unbedingt Variablen definieren, aber häufig bewirken sie eine Vereinfachung Ihrer Vorlage, indem komplexe Ausdrücke reduziert werden.

Im folgenden Beispiel werden die verfügbaren Optionen zum Definieren einer Variable angezeigt:

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": {
    <variable-complex-type-value>
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

Informationen zur Verwendung von `copy` zum Erstellen mehrerer Werte für eine Variable finden Sie unter [Variableniteration](resource-group-create-multiple.md#variable-iteration).

Beispiele für die Verwendung von Variablen finden Sie unter [Variablen in einer Azure Resource Manager-Vorlage](template-variables.md).

## <a name="functions"></a>Functions

In Ihrer Vorlage können Sie Ihre eigenen Funktionen erstellen. Diese Funktionen stehen dann zur Verwendung in der Vorlage zur Verfügung. Normalerweise definieren Sie komplexe Ausdrücke, die in der Vorlage nicht wiederholt werden sollen. Sie erstellen die benutzerdefinierten Funktionen aus Ausdrücken und [Funktionen](resource-group-template-functions.md), die in Vorlagen unterstützt werden.

Beim Definieren einer benutzerdefinierten Funktion gelten einige Einschränkungen:

* Die Funktion kann nicht auf Variablen zugreifen.
* Die Funktion kann nur Parameter verwenden, die in der Funktion definiert sind. Bei Verwendung der [parameters-Funktion](resource-group-template-functions-deployment.md#parameters) innerhalb einer benutzerdefinierten Funktion sind Sie auf die Parameter für diese Funktion beschränkt.
* Die Funktion kann keine anderen benutzerdefinierten Funktionen aufrufen.
* Die Funktion kann nicht die [Referenzfunktion](resource-group-template-functions-resource.md#reference) verwenden.
* Für die Parameter der Funktion können keine Standardwerte verwendet werden.

```json
"functions": [
  {
    "namespace": "<namespace-for-functions>",
    "members": {
      "<function-name>": {
        "parameters": [
          {
            "name": "<parameter-name>",
            "type": "<type-of-parameter-value>"
          }
        ],
        "output": {
          "type": "<type-of-output-value>",
          "value": "<function-return-value>"
        }
      }
    }
  }
],
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| Namespace |Ja |Namespace für die benutzerdefinierten Funktionen. Damit können Sie Namenskonflikte mit Vorlagenfunktionen vermeiden. |
| function-name |Ja |Name der benutzerdefinierten Funktion. Wenn Sie die Funktion aufrufen, kombinieren Sie den Funktionsnamen mit dem Namespace. Verwenden Sie z. B. `"[contoso.uniqueName()]"`, um die Funktion „uniqueName“ im Namespace „contoso“ aufzurufen. |
| parameter-name |Nein |Name des Parameters, der in der benutzerdefinierten Funktion verwendet werden soll |
| parameter-value |Nein |Der Typ des Parameterwerts. Die zulässigen Typen und Werte sind **string**, **securestring**, **int**, **bool**, **object**, **secureObject** und **array**. |
| output-type |Ja |Der Typ des Ausgabewerts. Ausgabewerte unterstützen dieselben Typen wie die Eingabeparameter der Funktion. |
| output-value |Ja |Vorlagensprachausdruck, der ausgewertet und von der Funktion zurückgegeben wird |

Beispiele für die Verwendung von benutzerdefinierten Funktionen finden Sie unter [Benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage](template-user-defined-functions.md).

## <a name="resources"></a>Ressourcen

Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden.

Sie definieren Ressourcen mit der folgenden Struktur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| condition | Nein | Boolescher Wert, der angibt, ob die Ressource während dieser Bereitstellung bereitgestellt wird. Wenn der Wert `true` lautet, wird die Ressource während der Bereitstellung erstellt. Wenn der Wert `false` lautet, wird die Ressource für diese Bereitstellung ausgelassen. Weitere Informationen finden Sie unter [Bedingung](conditional-resource-deployment.md). |
| apiVersion |Ja |Version der REST-API zum Erstellen der Ressource. Informationen zum Bestimmen verfügbarer Werte finden Sie in der [Vorlagenreferenz](/azure/templates/). |
| type |Ja |Der Typ der Ressource. Dieser Wert ist eine Kombination aus dem Namespace des Ressourcenanbieters und dem Ressourcentyp (z.B. **Microsoft.Storage/storageAccounts**). Informationen zum Bestimmen verfügbarer Werte finden Sie in der [Vorlagenreferenz](/azure/templates/). Für eine untergeordnete Ressource hängt das Format des Typs davon ab, ob sie innerhalb der übergeordneten Ressource geschachtelt oder außerhalb der übergeordneten Ressource definiert ist. Weitere Informationen finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen](child-resource-name-type.md). |
| name |Ja |Der Name der Ressource. Der Name muss die Einschränkungen für URI-Komponenten laut Definition in RFC3986 erfüllen. Azure-Dienste, die externen Parteien den Ressourcennamen verfügbar machen, überprüfen den Namen, um sicherzustellen, dass es sich nicht um einen Versuch handelt, eine andere Identität vorzutäuschen. Für eine untergeordnete Ressource hängt das Format des Namens davon ab, ob sie innerhalb der übergeordneten Ressource geschachtelt oder außerhalb der übergeordneten Ressource definiert ist. Weitere Informationen finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen](child-resource-name-type.md). |
| location |Varies |Unterstützte Standorte der angegebenen Ressource Wählen Sie einen der verfügbaren Standorte. In der Regel ist es jedoch sinnvoll, einen in der Nähe der Benutzer zu wählen. Normalerweise ist es auch sinnvoll, Ressourcen, die miteinander interagieren, in der gleichen Region zu platzieren. Die meisten Ressourcentypen benötigen einen Speicherort, andere Typen (z.B. eine Rollenzuordnung) jedoch nicht. Weitere Informationen finden Sie unter [Festlegen des Ressourcenspeicherorts](resource-location.md). |
| tags |Nein |Markierungen, die der Ressource zugeordnet sind Verwenden Sie Tags zum logischen Organisieren der Ressourcen in Ihrem Abonnement. |
| Kommentare |Nein |Ihre Notizen zur Dokumentierung der Ressourcen in Ihrer Vorlage. Weitere Informationen finden Sie unter [Kommentare in Vorlagen](resource-group-authoring-templates.md#comments). |
| copy |Nein |Wenn mehr als eine Instanz erforderlich ist, die Anzahl der zu erstellenden Ressourcen. Der Standardmodus ist parallel. Geben Sie den seriellen Modus an, wenn nicht alle Ressourcen gleichzeitig bereitgestellt werden sollen. Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nein |Ressourcen, die bereitgestellt werden müssen, bevor diese Ressource bereitgestellt wird. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der richtigen Reihenfolge bereit. Wenn Ressourcen nicht voneinander abhängig sind, werden sie parallel bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern sein. Es werden nur Ressourcen aufgelistet, die in dieser Vorlage bereitgestellt werden. Ressourcen, die nicht in dieser Vorlage definiert sind, müssen bereits vorhanden sein. Vermeiden Sie das Hinzufügen unnötiger Abhängigkeiten, da diese die Bereitstellung verlangsamen und Ringabhängigkeiten schaffen können. Tipps für das Festlegen von Abhängigkeiten finden Sie unter [Definieren von Abhängigkeiten in Azure Resource Manager-Vorlagen](resource-group-define-dependencies.md). |
| properties |Nein |Ressourcenspezifische Konfigurationseinstellungen. Die Werte für die Eigenschaften sind mit den Werten identisch, die Sie im Anforderungstext für den REST-API-Vorgang (PUT-Methode) angegeben haben, um die Ressource zu erstellen. Sie können auch ein Kopierarray angeben, um mehrere Instanzen einer Eigenschaft zu erstellen. Informationen zum Bestimmen verfügbarer Werte finden Sie in der [Vorlagenreferenz](/azure/templates/). |
| sku | Nein | Einige Ressourcen lassen Werte zu, die die bereitzustellende SKU definieren. Beispielsweise können Sie den Typ der Redundanz für ein Speicherkonto angeben. |
| kind | Nein | Einige Ressourcen lassen einen Wert zu, der den Typ der Ressource definiert, die Sie bereitstellen. Beispielsweise können Sie den Typ der zu erstellenden Cosmos DB angeben. |
| Tarif | Nein | Einige Ressourcen lassen Werte zu, die den bereitzustellenden Tarif definieren. Beispielsweise können Sie das Marketplace-Image für einen virtuellen Computer angeben. |
| ressourcen |Nein |Untergeordnete Ressourcen, die von der definierten Ressource abhängig sind. Stellen Sie nur Ressourcentypen bereit, die laut Schema der übergeordneten Ressource zulässig sind. Eine Abhängigkeit von der übergeordneten Ressource ist nicht impliziert. Sie müssen diese Abhängigkeit explizit definieren. Weitere Informationen finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen](child-resource-name-type.md). |

## <a name="outputs"></a>Ausgaben

Im Ausgabenabschnitt legen Sie Werte fest, die von der Bereitstellung zurückgegeben werden. In der Regel geben Sie Werte aus bereitgestellten Ressourcen zurück.

Das folgende Beispiel zeigt die Struktur einer Ausgabedefinition:

```json
"outputs": {
  "<output-name>" : {
    "condition": "<boolean-value-whether-to-output-value>",
    "type" : "<type-of-output-value>",
    "value": "<output-value-expression>"
  }
}
```

| Elementname | Erforderlich | BESCHREIBUNG |
|:--- |:--- |:--- |
| output-name |Ja |Name des Ausgabewerts. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| condition |Nein | Boolescher Wert, der angibt, ob dieser Ausgabewert zurückgegeben wird. Wenn `true`, wird der Wert in die Ausgabe für die Bereitstellung einbezogen. Wenn `false`, wird der Ausgabewert für diese Bereitstellung ausgelassen. Wenn keine Angabe erfolgt, lautet der Standardwert `true`. |
| type |Ja |Der Typ des Ausgabewerts. Ausgabewerte unterstützen dieselben Typen wie Vorlagen-Eingabeparameter. Bei Angabe von **securestring** für den Ausgabetyp wird der Wert nicht im Bereitstellungsverlauf angezeigt und kann nicht aus einer anderen Vorlage abgerufen werden. Um einen geheimen Wert in mehreren Vorlagen zu verwenden, speichern Sie das Geheimnis in einer Key Vault-Instanz, und verweisen Sie in der Parameterdatei auf das Geheimnis. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](resource-manager-keyvault-parameter.md). |
| value |Ja |Vorlagensprachausdruck, der ausgewertet und als Ausgabewert zurückgegeben wird. |

Beispiele für die Verwendung von Ausgaben finden Sie unter [Ausgaben in einer Azure Resource Manager-Vorlage](template-outputs.md).

<a id="comments" />

## <a name="comments-and-metadata"></a>Kommentare und Metadaten

Es gibt mehrere Möglichkeiten, um Kommentare und Metadaten in Ihrer Vorlage hinzuzufügen.

Sie können ein `metadata`-Objekt fast überall in Ihrer Vorlage hinzufügen. Resource Manager ignoriert das Objekt, aber Sie werden von Ihrem JSON-Editor möglicherweise gewarnt, dass die Eigenschaft nicht gültig ist. Definieren Sie im Objekt die erforderlichen Eigenschaften.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

Fügen Sie für **Parameter** ein `metadata`-Objekt mit einer `description`-Eigenschaft hinzu.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

Wenn Sie die Vorlage über das Portal bereitstellen, wird der Text, den Sie in der Beschreibung angeben, automatisch als Tipp für diesen Parameter verwendet.

![Parametertipp anzeigen](./media/resource-group-authoring-templates/show-parameter-tip.png)

Fügen Sie für **Ressourcen** ein `comments`-Element oder ein Metadatenobjekt hinzu. Das folgende Beispiel zeigt sowohl ein Kommentarelement als auch ein Metadatenobjekt.

```json
"resources": [
  {
    "comments": "Storage account used to store VM disks",
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

Fügen Sie für **Ausgaben** dem Ausgabewert ein Metadatenobjekt hinzu.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

Sie können ein Metadatenobjekt nicht zu benutzerdefinierten Funktionen hinzufügen.

Für Inlinekommentare können Sie entweder `//` oder `/* ... */` verwenden, aber diese Syntax funktioniert nicht mit allen Tools. Sie können nicht den Vorlagen-Editor des Portals verwenden, um Vorlagen mit Inlinekommentaren zu bearbeiten. Wenn Sie diese Art von Kommentar hinzufügen, stellen Sie sicher, dass die von Ihnen verwendeten Tools JSON-Inlinekommentare unterstützen.

> [!NOTE]
> Wenn Sie Vorlagen mit Kommentaren mithilfe der Azure CLI bereitstellen möchten, müssen Sie den Switch `--handle-extended-json-format` verwenden.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  "dependsOn": [ /* storage account and network interface must be deployed first */
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Im VS-Code können Sie den Sprachmodus auf „JSON mit Kommentaren“ festlegen. Die Inlinekommentare werden nicht mehr als ungültig markiert. So ändern Sie den Modus

1. Öffnen Sie die Sprachmodusauswahl (STRG+K M).

1. Wählen Sie **JSON mit Kommentaren** aus.

   ![Auswählen des Sprachmodus](./media/resource-group-authoring-templates/select-json-comments.png)

## <a name="multi-line-strings"></a>Mehrzeilige Zeichenfolgen

Sie können eine Zeichenfolge in mehrere Zeilen unterteilen. Zum Beispiel die Standorteigenschaft und einen der Kommentare im folgenden JSON-Beispiel.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Wenn Sie Vorlagen mit mehrzeiligen Zeichenfolgen mithilfe der Azure CLI bereitstellen möchten, müssen Sie den Switch `--handle-extended-json-format` verwenden.

## <a name="next-steps"></a>Nächste Schritte

* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager template best practices (Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen)](template-best-practices.md).
* Weitere Empfehlungen zum Erstellen von Resource Manager-Vorlagen, die Sie in allen Azure-Umgebungen und in Azure Stack verwenden können, finden Sie unter [Entwickeln von Azure Resource Manager-Vorlagen für cloudübergreifende Konsistenz](templates-cloud-consistency.md).
