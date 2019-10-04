---
title: Erstellen und Verwalten von Telemetrieregeln in Ihre Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Azure IoT Central-Telemetrieregeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
author: ankitgupta
ms.author: ankitgup
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d6deecf558105701be591c1f08923eca2c1e3bbd
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879042"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Erstellen einer Telemetrieregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Mithilfe von Azure IoT Central können Sie Ihre verbundenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und automatisch Aktionen aufzurufen, z. B. das Senden einer E-Mail oder das Auslösen von Microsoft Flow. Mit nur wenigen Mausklicks können Sie die Bedingung für die Überwachung Ihrer Gerätedaten definieren und die entsprechende Aktion konfigurieren. In diesem Artikel wird erläutert, wie Regeln zum Überwachen von Telemetriedaten erstellt werden, die vom Gerät gesendet werden.

Geräte können mithilfe von Telemetriemessungen numerische Daten vom Gerät senden. Eine Telemetrieregel wird ausgelöst, wenn die ausgewählten Gerätetelemetriedaten einen angegebenen Schwellenwert überschreiten.

## <a name="create-a-telemetry-rule"></a>Erstellen einer Telemetrieregel

Um eine Telemetrieregel erstellen zu können, muss für die Gerätedefinition mindestens eine Telemetriemessung definiert sein. In diesem Beispiel wird ein gekühlter Verkaufsautomat verwendet, der Temperatur- und Feuchtetelemetriedaten sendet. Die Regel überwacht die vom Gerät gemeldete Temperatur und sendet eine E-Mail, sobald diese 80 Grad übersteigt.

1. Navigieren Sie zur Seite **Regeln**.

1. Wenn Sie noch keine Regeln erstellt haben, wird der folgende Bildschirm angezeigt:

    ![Noch keine Regeln](media/howto-create-telemetry-rules-pnp/rules-landing-page1.png)

1. Wählen Sie **+ Neue Regel** aus, um die Typen von Regeln anzuzeigen, die Sie erstellen können.

1. Klicken Sie auf **Telemetrie**, um eine Regel zum Überwachen von Gerätetelemetriedaten zu erstellen.

    ![Regeltypen](media/howto-create-telemetry-rules-pnp/rule-types1.png)


1. Geben Sie einen aussagekräftigen Namen ein, um die Regel identifizieren zu können, und drücken Sie die EINGABETASTE.

1. Wählen Sie im Abschnitt „Bereiche“ die Gerätedefinition aus, auf die Sie diese Regel festlegen möchten. Auf diesem Bildschirm können Sie auch die Geräte filtern, auf die diese Regel angewendet wird, indem Sie die Option **+ Filter** verwenden. Die Regel gilt automatisch für alle Geräte unter der Gerätevorlage. Wenn Sie die Regel deaktivieren möchten, wählen Sie in der Kopfzeile die Schaltfläche **Deaktivieren** aus.

### <a name="configure-the-rule-conditions"></a>Konfigurieren der Regelbedingungen

Eine Bedingung definiert die Kriterien, die von der Regel überwacht werden.

1. Wählen Sie aus, ob die **Aggregation** aktiviert oder deaktiviert werden soll.

      - Die Aggregation ist optional. Ohne Aggregation wird diese Regel für jeden Telemetriedatenpunkt ausgelöst, der die Bedingung erfüllt. Wenn beispielswiese die Regel so konfiguriert ist, dass sie bei einem Temperaturwert über 80 ausgelöst wird, erfolgt die Auslösung nahezu sofort, wenn das Gerät eine Temperatur von mehr als 80 meldet.
      - Wenn eine Aggregatfunktion wie „Durchschnitt“, „Min“, „Max“ oder „Anzahl“ ausgewählt wird, muss der Benutzer ein **Zeitfenster** angeben, in dem die Bedingung ausgewertet werden soll. Wenn Sie beispielsweise den Zeitraum auf „5 Minuten“ festlegen und die Regel nach einer Durchschnittstemperatur von über 80 sucht, löst die Regel aus, wenn die Durchschnittstemperatur für mindestens 5 Minuten 80 Grad übersteigt. Die Regelauswertungshäufigkeit ist identisch mit dem **Zeitfenster**. Dies bedeutet in diesem Beispiel, dass die Regel alle 5 Minuten ausgewertet wird.

1. Wählen Sie in der Dropdownliste **Messung** die Telemetriedaten aus, die Sie überwachen möchten.

1. Wählen Sie als Nächstes einen **Operator** aus, und geben Sie einen **Wert** an.

     ![Bedingung](media/howto-create-telemetry-rules-pnp/aggregate-condition-filled-out1.png)


>[!NOTE]
>Es können mehrere Telemetriemessungen hinzugefügt werden. Wählen Sie dazu **+ Bedingung** aus. Wenn mehrere Bedingungen angegeben werden, müssen alle Bedingungen erfüllt sein, damit die Regel ausgelöst wird. Jede Bedingung wird implizit durch eine „UND“-Klausel verknüpft. Wenn Sie die Aggregatfunktion verwenden, müssen alle Messungen aggregiert werden.

### <a name="configure-actions"></a>Konfigurieren von Aktionen

In diesem Abschnitt erfahren Sie, wie Aktionen eingerichtet werden, die ausgeführt werden sollen, wenn die Regel ausgelöst wird. Aktionen werden aufgerufen, wenn alle in der Regel angegebenen Bedingungen als TRUE ausgewertet werden.

1. Klicken Sie in den Abschnitten **Aktion** auf **+ Aktion**. Hier sehen Sie die Liste der verfügbaren Aktionen.  

    ![Hinzufügen einer Aktion](media/howto-create-telemetry-rules-pnp/add-action1.png)


1. Wählen Sie die Aktion **E-Mail** aus, geben Sie einen Anzeigenamen für die Aktion und im Feld **An** eine gültige E-Mail-Adresse ein, und geben Sie dann einen Hinweis an, der im Textkörper der E-Mail angezeigt werden soll, wenn die Regel ausgelöst wird.

    > [!NOTE]
    > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) in Azure IoT Central.

   ![Konfigurieren einer Aktion](media/howto-create-telemetry-rules-pnp/configure-action1.png)


1. Wählen Sie zum Speichern der Aktion die Option **Fertig** aus.

1. Um die neue Regel zu speichern, wählen Sie **Speichern** aus. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Telemetriedaten, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

## <a name="parameterize-the-rule"></a>Parametrisieren der Regel

Regeln können bestimmte Werte aus den **Geräteeigenschaften** als Parameter ableiten. Die Verwendung von Parametern ist in Szenarien hilfreich, in denen die Telemetrieschwellenwerte für verschiedene Geräte variieren. Wählen Sie beim Erstellen der Regel eine Geräteeigenschaft aus, die den Schwellenwert angibt, z. B. **Maximaler Idealschwellenwert**, statt einen absoluten Wert wie 80 Grad anzugeben. Wenn die Regel ausgeführt wird, gleicht sie die Gerätetelemetrie mit dem in der Geräteeigenschaft festgelegten Wert ab.

Die Verwendung von Parametern ist eine effektive Möglichkeit, die Anzahl der zu verwaltenden Regeln zu reduzieren.

Aktionen können auch über **Geräteeigenschaft** als Parameter konfiguriert werden. Wenn eine E-Mail-Adresse als Eigenschaft gespeichert ist, kann sie bei der Definition der Empfängeradresse (**An**) verwendet werden.

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen. Das Löschen der Regel entfernt sie aus der Gerätevorlage und allen zugehörigen Geräten.

## <a name="enable-or-disable-a-rule"></a>Aktivieren oder Deaktivieren einer Regel

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Verwenden Sie in der Regel die Schaltfläche **Aktivieren** oder **Deaktivieren**, um die Regel für alle in der Regel definierten Geräte zu aktivieren oder zu deaktivieren.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Aktivieren oder Deaktivieren einer Regel für ein Gerät

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Fügen Sie im Abschnitt **Bereiche** einen Filter hinzu, um ein bestimmtes Gerät in die Gerätevorlage aufzunehmen oder daraus auszuschließen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Regeln in Ihrer Azure IoT Central-Anwendung erstellen, wird als Nächstes der folgende Schritt empfohlen: [Verwalten von Geräten](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
