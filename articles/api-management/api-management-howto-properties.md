---
title: Verwenden von benannten Werten in Azure API Management-Richtlinien
description: Erfahren Sie, wie benannte Werte in Azure API Management-Richtlinien verwendet werden.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/05/2019
ms.author: apimpm
ms.openlocfilehash: d11239aa49a53a90a38f2b5336d36cea6c97e9df
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824178"
---
# <a name="how-to-use-named-values-in-azure-api-management-policies"></a>Verwenden von benannten Werten in Azure API Management-Richtlinien

API Management-Richtlinien sind eine leistungsfähige Funktion des Systems, mit der das Azure-Portal das Verhalten der API über eine Konfiguration ändern kann. Richtlinien sind eine Sammlung von Anweisungen, die sequenziell bei Anfragen oder Antworten einer API ausgeführt werden. Richtlinienanweisungen können mithilfe von literalen Textwerten, Richtlinienausdrücken und benannten Werten erstellt werden.

Jede API Management-Dienstinstanz weist eine Eigenschaftensammlung von Schlüssel-Wert-Paaren auf, die als benannte Werte bezeichnet werden und die für die gesamte Dienstinstanz gelten. Es gibt keine Beschränkung für die Anzahl der Elemente in der Sammlung. Benannte Werte können zum Verwalten konstanter Zeichenfolgenwerte für alle API-Konfigurationen und -Richtlinien verwendet werden. Jeder benannte Wert kann die folgenden Attribute aufweisen:

| Attribut      | type            | BESCHREIBUNG                                                                                                                         |
| -------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `Display name` | Zeichenfolge          | Wird zum Verweisen auf den benannten Wert in Richtlinien verwendet. Eine Zeichenfolge von einem bis 256 Zeichen. Nur Buchstaben, Ziffern, Punkt und Leerzeichen sind zulässig. |
| `Value`        | Zeichenfolge          | Tatsächlicher Wert. Darf nicht leer sein oder nur aus Leerzeichen bestehen. Maximal 4.096 Zeichen lang.                                     |
| `Secret`       | boolean         | Bestimmt, ob der Wert ein geheimer Schlüssel ist und ob er verschlüsselt werden sollte.                                                            |
| `Tags`         | Array von Zeichenfolgen | Wird zum Filtern der Liste der benannten Werte verwendet. Bis zu 32 Tags.                                                                                    |

![Benannte Werte](./media/api-management-howto-properties/named-values.png)

Benannte Werte können Literalzeichenfolgen und [Richtlinienausdrücke](/azure/api-management/api-management-policy-expressions) enthalten. Der Wert von `Expression` ist beispielsweise ein Richtlinienausdruck, der eine Zeichenfolge zurückgibt, die das aktuelle Datum und die Uhrzeit enthält. Der benannte Wert `Credential` ist als Geheimnis markiert, sodass er standardmäßig nicht angezeigt wird.

| NAME       | Wert                      | `Secret` | `Tags`          |
| ---------- | -------------------------- | ------ | ------------- |
| Wert      | 42                         | False  | vital-numbers |
| Anmeldeinformation | ••••••••••••••••••••••     | True   | security      |
| Ausdruck | @(DateTime.Now.ToString()) | False  |               |

## <a name="to-add-and-edit-a-named-value"></a>So fügen Sie einen benannten Wert hinzu und bearbeiten ihn

![Hinzufügen eines benannten Werts](./media/api-management-howto-properties/add-property.png)

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Klicken Sie auf **Benannte Werte**.
3. Klicken Sie auf **+ Hinzufügen**.

    „Name“ und „Wert“ sind erforderliche Werte. Wenn der Wert ein Geheimnis ist, aktivieren Sie das Kontrollkästchen *Dies ist ein Geheimnis*. Geben Sie einen oder mehrere optionale Tags ein, um Ihre benannten Werte besser organisieren zu können, und klicken Sie auf „Speichern“.

4. Klicken Sie auf **Create**.

Sobald der benannte Wert erstellt wurde, können Sie ihn bearbeiten, indem Sie darauf klicken. Wenn Sie den Namen des benannten Werts ändern, werden alle Richtlinien, die auf diesen benannten Wert verweisen, automatisch aktualisiert, sodass sie den neuen Namen verwenden.

Informationen zum Bearbeiten eines benannten Werts mithilfe der REST-API finden Sie unter [Bearbeiten eines benannten Werts mit der REST-API](/rest/api/apimanagement/2019-01-01/property?patch).

## <a name="to-delete-a-named-value"></a>So löschen Sie einen benannten Wert

Wenn Sie einen benannten Wert löschen möchten, klicken Sie neben dem zu löschenden benannten Wert auf **Löschen**.

> [!IMPORTANT]
> Wenn Richtlinien auf den benannten Wert verweisen, können Sie ihn erst erfolgreich löschen, nachdem Sie den benannten Wert aus allen Richtlinien entfernt haben, die ihn verwenden.

Informationen zum Löschen eines benannten Werts mithilfe der REST-API finden Sie unter [Löschen eines benannten Werts mit der REST-API](/rest/api/apimanagement/2019-01-01/property/delete).

## <a name="to-search-and-filter-named-values"></a>So suchen und filtern Sie benannte Werte

Die Registerkarte **Benannte Werte** umfasst Such- und Filterfunktionen, die Ihnen beim Verwalten Ihrer benannten Werte helfen. Geben Sie zum Filtern der Liste der benannten Werte nach einem Namen einen Suchbegriff in das Textfeld **Eigenschaft suchen** ein. Wenn Sie alle benannten Werte anzeigen möchten, löschen Sie den Inhalt des Textfelds **Eigenschaft suchen**, und drücken Sie die Eingabetaste.

Geben Sie zum Filtern der Liste nach Tags mindestens ein Tag in das Textfeld **Nach Tags filtern** ein. Wenn Sie alle benannten Werte anzeigen möchten, löschen Sie den Inhalt des Textfelds **Nach Tags filtern**, und drücken Sie die Eingabetaste.

## <a name="to-use-a-named-value"></a>So verwenden Sie einen benannten Wert

Um einen benannten Wert in einer Richtlinie zu verwenden, setzen Sie seinen Namen in ein doppeltes Paar geschweifter Klammern (z. B. `{{ContosoHeader}}`), wie im folgenden Beispiel gezeigt:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

In diesem Beispiel dient `ContosoHeader` als Name eines Headers in einer `set-header`-Richtlinie und `ContosoHeaderValue` als Wert dieses Headers. Wenn diese Richtlinie in einer Anforderung oder Antwort an das API Management-Gateway ausgewertet wird, werden `{{ContosoHeader}}` und `{{ContosoHeaderValue}}` durch ihre jeweiligen Werte ersetzt.

Benannte Werte können als vollständige Attribut- oder Elementwerte verwendet werden, wie im vorherigen Beispiel gezeigt, sie können aber auch in einen literalen Textausdruck eingefügt oder mit einem Teil davon kombiniert werden, wie im folgenden Beispiel gezeigt wird: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Benannte Werte können auch Richtlinienausdrücke enthalten. Im folgenden Beispiel wird `ExpressionProperty` verwendet.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Wenn diese Richtlinie ausgewertet wird, wird `{{ExpressionProperty}}` durch ihren Wert ersetzt: `@(DateTime.Now.ToString())`. Da der Wert ein Richtlinienausdruck ist, wird der Ausdruck ausgewertet, und die Ausführung der Richtlinie wird fortgesetzt.

Sie können dies im Entwicklerportal durch Aufrufen eines Vorgangs testen, der eine Richtlinie mit entsprechenden benannten Werten aufweist. Im folgenden Beispiel wird ein Vorgang mit den beiden vorherigen `set-header`-Beispielrichtlinien mit benannten Werten aufgerufen. Beachten Sie, dass die Antwort zwei benutzerdefinierte Header enthält, die mithilfe von Richtlinien mit benannten Werten konfiguriert wurden.

![Entwicklerportal][api-management-send-results]

Beim Betrachten der [Verfolgung mit dem API-Inspektor](api-management-howto-api-inspector.md) für einen Aufruf, der die beiden vorherigen Beispielrichtlinien mit benannten Werten enthält, sehen Sie beide `set-header`-Richtlinien mit den eingefügten benannten Werten und die Auswertung des Richtlinienausdrucks für den benannten Wert, der den Richtlinienausdruck enthält.

![Verfolgung mit dem API-Inspektor][api-management-api-inspector-trace]

Benannte Werte können zwar Richtlinienausdrücke, aber keine anderen benannten Werte enthalten. Wenn für einen Wert, z. B. `Text: {{MyProperty}}`, Text verwendet wird, der einen Verweis auf einen benannten Wert enthält, wird dieser Verweis nicht aufgelöst und ersetzt.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zum Arbeiten mit Richtlinien
    -   [Richtlinien in Azure API Management](api-management-howto-policies.md)
    -   [Richtlinienreferenz](/azure/api-management/api-management-policies)
    -   [Richtlinienausdrücke](/azure/api-management/api-management-policy-expressions)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png
