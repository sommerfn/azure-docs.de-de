---
title: Verwenden von Visual Studio Code für die Erstellung von Azure Resource Manager-Vorlagen
description: Erfahren Sie, wie Sie Visual Studio Code und die Erweiterung „Azure Resource Manager-Tools“ installieren und verwenden können.
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: jgao
ms.openlocfilehash: 8d2b818846eeae44ffe05b4546a333510c638621
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196872"
---
# <a name="use-visual-studio-code-to-create-azure-resource-manager-templates"></a>Verwenden von Visual Studio Code für die Erstellung von Azure Resource Manager-Vorlagen

Visual Studio Code ist ein schlanker Open-Source-Editor für mehrere Plattformen. Die Erweiterung „Azure Resource Manager-Tools“ ist ein Plug-In für die Entwicklung von Resource Manager-Vorlagen. Die Erweiterung fügt Sprachunterstützung für Vorlagen hinzu, um Ihnen IntelliSense, Syntaxhervorhebung, Inlinehilfe und viele andere Sprachfunktionen bereitstellen zu können. In dieser Kombination ergeben sie die empfohlene Umgebung für Vorlagenentwicklung.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Visual Studio Code unterstützt MacOS, Windows und Linux.  Es kann von der Website für [Visual Studio Code](https://code.visualstudio.com/) installiert werden.

## <a name="install-resource-manager-tools-extension"></a>Installieren der Azure Resource Manager-Tools-Erweiterung

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie im linken Menü die Option **Erweiterungen** aus. Oder wählen Sie im Menü **Ansicht** die Option **Erweiterungen** aus, um den Bereich „Erweiterungen“ zu öffnen.

    ![Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code installieren](./media/resource-manager-tools-vs-code/resource-manager-visual-studio-code-tools-extension.png)
1. Suchen Sie nach **Resource Manager**.
1. Wählen Sie **Installieren** unter **Azure Resource Manager-Tools** aus.

## <a name="the-extension-features"></a>Die Merkmale der Erweiterung

### <a name="colorization-for-template-language-expressions"></a>Farbgebung für Sprachausdrücke einer Vorlage

Parameter, Variablen, Funktionen, Namen und Ausdrücke werden farblich codiert, wie dies im folgenden Screenshot gezeigt ist:

![Farbgebung für die Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-colorization.png)

Die Gliederungsansicht für Vorlagen erleichtert das Navigieren durch große Vorlagen.

### <a name="intellisense"></a>IntelliSense

Die Erweiterung für Resource Manager-Vorlagen kennt mögliche Vervollständigungen für Funktionsnamen, Parameter, Variablen und Verweise. Die IntelliSense-Vorschläge werden angezeigt, während Sie eingeben. Wenn Sie die Eingabe von Zeichen fortsetzen, wird die Liste der Member (Variablen, Methoden usw.) so gefiltert, dass nur Member einbezogen werden, die die von Ihnen eingegebenen Zeichen enthalten. Durch Drücken von **TAB** oder **EINGABE** wird das ausgewählte Element eingefügt.

- Namen der integrierten Funktionen

    ![IntelliSense-Funktionen der Visual Studio Code-Erweiterung „Azure Resource Manager-Tools“](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-functions.png)

- Parameterverweise

    ![IntelliSense-Parameter der Visual Studio Code-Erweiterung „Azure Resource Manager-Tools“](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-parameters.png)

- Variablenverweise

    ![IntelliSense-Variablen der Visual Studio Code-Erweiterung „Azure Resource Manager-Tools“](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-variables.png)

- resourceGroup()-Eigenschaften

    ![IntelliSense-Funktionen der Visual Studio Code-Erweiterung „Azure Resource Manager-Tools“](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-resourcegroup.png)

Außerdem funktioniert IntelliSense auch mit subscription()-Eigenschaften und den Eigenschaften von Verweisen auf Variablen, die Objekte sind.

### <a name="signature-help-for-function-parameters"></a>Signaturhilfe für Funktionsparameter

Wenn Sie auf Funktionsnamen zeigen, zeigt die Erweiterung die Signaturhilfe für Funktionsparameter an.

![Signaturfunktionen der Visual Studio Code-Erweiterung „Azure Resource Manager-Tools“](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-signature-function.png)

### <a name="go-to-definition-for-variable-and-parameter-references"></a>„Zu Definition wechseln“ für Variablen- und Parameterverweise

Sie können mit **STRG+Klicken** oder über das Kontextmenü (wie im Screenshot gezeigt) zur jeweiligen Definition springen: ![„Zu Definition wechseln“ für die Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-context-menu.png)

Sie können die Definition auf der Seite mit **STRG+ALT+Klicken** öffnen.

### <a name="peek-for-variable-and-parameter-definitions"></a>Anzeigen des Peek-Editors für Variablen- und Parameterdefinitionen

Um den Peek-Editor zu öffnen, verwenden Sie das Kontextmenü, wie im vorherigen Screenshot dargestellt.

Im folgenden Screenshot ist der Peek-Editor dargestellt:

![Peek-Editor für die Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-peek-editor.png)

### <a name="find-all-references-for-variables-and-parameters"></a>Suchen nach allen Verweisen für Variablen und Parameter

Um nach allen Verweisen zu suchen, verwenden Sie das Kontextmenü, wie im vorherigen Screenshot dargestellt.

Der folgende Screenshot zeigt, wie die Verweise hervorgehoben werden:

![Suchen nach allen Verweisen in der Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-find-all-references.png)

### <a name="rename-all-references-for-variables-and-parameters"></a>Umbenennen aller Verweise für Variablen und Parameter

Um alle Verweise für Variablen und Parameter umzubenennen, verwenden Sie das Kontextmenü, wie im vorherigen Screenshot dargestellt.

### <a name="hover-for-parameter-description"></a>Darauf zeigen für Parameterbeschreibung

![Auf Definition zeigen in der Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-hover-parameters.png)

### <a name="brace-matching"></a>Klammernabgleich (zugehörige Klammer)

Zusammengehörende Klammern werden hervorgehoben, sobald sich der Cursor in der Nähe eines der Klammernzeichen befindet. Wenn Sie auf eine Klammer klicken, wird auch die zugehörige Klammer hervorgehoben, wie im folgenden Screenshot gezeigt:

![Klammernabgleich in der Erweiterung „Azure Resource Manager-Tools“ in Visual Studio Code](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-brace-matching.png)

### <a name="show-errors-and-warnings"></a>Anzeigen von Fehlern und Warnungen

Zu den Fehlern, die von der Erweiterung erkannt werden, gehören:

- Nicht definierte Parameterverweise
- Nicht definierte Variablenverweise
- Nicht erkannte Funktionsnamen
- reference()-Funktionsverwendung in einer Variablendefinition
- Falsche Anzahl von Argumenten in Funktionen

Zu den Warnungen gehören:

- Nicht verwendete Parameter
- Nicht verwendete Variablen

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Resource Manager und Resource Manager-Vorlagen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](./resource-group-overview.md).
- Wenn Sie einen Schnellstart mit Visual Studio Code durchlaufen möchten, lesen Sie [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md).
