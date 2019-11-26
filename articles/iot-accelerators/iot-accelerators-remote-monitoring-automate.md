---
title: 'Verwenden der Remoteüberwachungslösung zum Erkennen von Geräteproblemen: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird erläutert, wie Sie mithilfe von Regeln und Aktionen automatisch schwellenwertbasierte Geräteprobleme in der Remoteüberwachungslösung erkennen können.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 676f1133a516174478a456a97bc467e7770fe6e7
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888890"
---
# <a name="tutorial-detect-issues-with-devices-connected-to-your-monitoring-solution"></a>Tutorial: Erkennen von Problemen mit Geräten, die mit der Überwachungslösung verbunden sind

In diesem Tutorial konfigurieren Sie den Solution Accelerator für die Remoteüberwachung für die Erkennung von Problemen mit Ihren verbundenen IoT-Geräten. Zum Erkennen der Probleme für Ihre Geräte fügen Sie Regeln hinzu, mit denen im Lösungsdashboard Benachrichtigungen generiert werden.

Zur Einführung von Regeln und Benachrichtigungen wird im Tutorial ein simuliertes Gerät vom Typ „Kältemaschine“ verwendet. Die Kältemaschine wird von der Organisation Contoso verwaltet und ist mit dem Solution Accelerator für die Remoteüberwachung verbunden. Contoso verfügt bereits über eine Regel, die eine kritische Benachrichtigung generiert, wenn der Druck in einer Kältemaschine auf über 298 PSI ansteigt. Als Bediener bei Contoso möchten Sie Geräte vom Typ „Kältemaschine“ identifizieren, die unter Umständen problematische Sensoren aufweisen, indem Sie nach Anfangsdruckspitzen suchen. Zum Identifizieren dieser Geräte fügen Sie eine Regel hinzu, mit der eine Warnungsbenachrichtigung generiert wird, wenn der Druck in der Kältemaschine auf mehr als 150 PSI ansteigt.

Sie wurden außerdem gebeten, eine kritische Benachrichtigung für eine Kältemaschine zu erstellen, wenn innerhalb der letzten fünf Minuten die durchschnittliche Luftfeuchtigkeit im Gerät über 80% und die Temperatur des Geräts oberhalb von 75 Grad Fahrenheit gelegen hat.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Anzeigen der Regeln in der Lösung
> * Erstellen einer Regel
> * Erstellen einer Regel mit mehreren Bedingungen
> * Bearbeiten einer vorhandenen Regel
> * Ein- und Ausschalten von Regeln

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="review-the-existing-rules"></a>Überprüfen der vorhandenen Regeln

Im Solution Accelerator wird auf der Seite **Regeln** eine Liste mit allen aktuellen Regeln angezeigt:

[![Seite „Regeln“](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-expanded.png#lightbox)

Wenden Sie einen Filter an, um nur die Regeln anzuzeigen, die für Geräte vom Typ „Kältemaschine“ gelten. Sie können weitere Informationen zu einer Regel anzeigen und diese bearbeiten, wenn Sie sie in der Liste auswählen:

[![Anzeigen der Regeldetails](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-expanded.png#lightbox)

## <a name="create-a-rule"></a>Erstellen einer Regel

Klicken Sie auf **New rule** (Neue Regel), um eine Regel zu erstellen, die eine Warnung generiert, wenn der Druck in einer Kältemaschine auf mehr als 150 PSI ansteigt. Verwenden Sie zum Erstellen der Regel die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Regelname        | Chiller warning                       |
| BESCHREIBUNG      | Chiller pressure has exceeded 150 PSI |
| Gerätegruppe     | **Chillers** device group             |
| Berechnung      | Sofort                               |
| Feld für die erste Bedingung| pressure                              |
| Operator für die erste Bedingung | Größer als                      |
| Wert für die erste Bedingung    | 150                               |
| Schweregrad  | Warnung                               |

[![Erstellen einer Warnregel](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-expanded.png#lightbox)

Klicken Sie auf **Übernehmen**, um die neue Regel zu speichern.

Der Auslösezeitpunkt der Regel wird auf der Seite **Regeln** und auf der Seite **Dashboard** angezeigt:

[![Warnungsregel ausgelöst](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-expanded.png#lightbox)

## <a name="create-an-advanced-rule"></a>Erstellen einer erweiterten Regel

Klicken Sie auf **New rule** (Neue Regel), um eine Regel mit mehreren Bedingungen zu erstellen, die im folgenden Fall eine kritische Benachrichtigung generiert: Innerhalb der letzten fünf Minuten hat für ein Gerät vom Typ „Kältemaschine“ die durchschnittliche Luftfeuchtigkeit über 80% und die Durchschnittstemperatur oberhalb von 75 Grad Fahrenheit gelegen. Verwenden Sie zum Erstellen der Regel die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Regelname        | Chiller humidity and temp critical    |
| BESCHREIBUNG      | Humidity and temperature are critical |
| Gerätegruppe     | **Chillers** device group             |
| Berechnung      | Durchschnitt                               |
| Zeitraum      | 5                                     |
| Feld für die erste Bedingung| Luftfeuchtigkeit                              |
| Operator für die erste Bedingung | Größer als                      |
| Wert für die erste Bedingung    | 80                                |
| Schweregrad  | Kritisch                              |

[![Erstellen einer Regel mit mehreren Bedingungen – Teil 1](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-expanded.png#lightbox)

Klicken Sie zum Hinzufügen der zweiten Bedingung auf „+ Bedingung hinzufügen“. Verwenden Sie für die neue Bedingung die folgenden Werte:

| Einstellung          | Wert                                 |
| ---------------- | ------------------------------------- |
| Feld für die zweite Bedingung| Temperatur                           |
| Operator für die zweite Bedingung | Größer als                      |
| Wert für die zweite Bedingung    | 75                                |

[![Erstellen einer Regel mit mehreren Bedingungen – Teil 2](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-expanded.png#lightbox)

Klicken Sie auf **Übernehmen**, um die neue Regel zu speichern.

Der Auslösezeitpunkt der Regel wird auf der Seite **Regeln** und auf der Seite **Dashboard** angezeigt:

[![Auslösung der Regel mit mehreren Bedingungen](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-expanded.png#lightbox)

## <a name="edit-an-existing-rule"></a>Bearbeiten einer vorhandenen Regel

Um eine Änderung an einer vorhandenen Regel vorzunehmen, wählen Sie sie in der Regelliste aus und klicken auf **Bearbeiten**:

[![Bearbeiten einer Regel](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-expanded.png#lightbox)

## <a name="disable-a-rule"></a>Deaktivieren einer Regel

Um eine Regel vorübergehend auszuschalten, können Sie sie in der Regelliste deaktivieren. Wählen Sie die zu deaktivierende Regel aus, und klicken Sie anschließend auf **Deaktivieren**. Der **Status** der Regel in der Liste ändert sich, um anzugeben, dass die Regel jetzt deaktiviert ist. Sie können eine zuvor deaktivierte Regel mit dem gleichen Verfahren wieder aktivieren.

[![Deaktivieren einer Regel](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-expanded.png#lightbox)

Sie können mehrere Regeln gleichzeitig aktivieren und deaktivieren, indem Sie in der Liste mehrere Regeln auswählen.

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel dauerhaft löschen möchten, können Sie sie in der Regelliste löschen. Wählen Sie die zu löschende Regel aus, und klicken Sie anschließend auf **Löschen**.

[![Löschen der Regel](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdelete-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdelete-expanded.png#lightbox)

Nachdem Sie bestätigt haben, dass Sie die Regel löschen möchten, haben Sie die Option, alle Warnungen im Zusammenhang mit der Regel auf der Seite **Wartung** zu löschen.

[![Löschen der Regel](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdeletetidy-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdeletetidy-expanded.png#lightbox)

Die Regeln können nur einzeln gelöscht werden.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde veranschaulicht, wie Sie im Solution Accelerator für die Remoteüberwachung die Seite **Rules** (Regeln) verwenden, um Regeln zu erstellen und zu verwalten, mit denen Benachrichtigungen in der Lösung ausgelöst werden. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Solution Accelerator zum Verwalten und Konfigurieren Ihrer verbundenen Geräte verwenden.

> [!div class="nextstepaction"]
> [Konfigurieren und Verwalten von Geräten, die mit der Überwachungslösung verbunden sind](iot-accelerators-remote-monitoring-manage.md)