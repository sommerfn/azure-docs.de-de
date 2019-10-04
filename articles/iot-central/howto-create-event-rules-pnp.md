---
title: Erstellen und Verwalten von Ereignisregeln in Ihre Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Azure IoT Central-Ereignisregeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
author: dominicbetts
ms.author: dobett
ms.date: 07/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1a7fc2b38e8354fb29255bb7f9d6b2c03ed53725
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879046"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Erstellen einer Ereignisregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Mithilfe von Azure IoT Central können Sie Ihre verbundenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen (wie das Senden einer E-Mail) automatisch aufzurufen. Mit nur wenigen Mausklicks können Sie die Bedingung für die Überwachung Ihrer Gerätedaten definieren und die entsprechende Aktion konfigurieren. In diesem Artikel wird erläutert, wie Regeln zum Überwachen von Ereignissen erstellt werden, die vom Gerät gesendet werden.

Geräte können Ereignismessungen zum Senden von wichtigen Geräteereignissen oder Informationen verwenden. Eine Ereignisregel wird ausgelöst, wenn das ausgewählte Geräteereignis vom Gerät gemeldet wird.

## <a name="create-an-event-rule"></a>Erstellen einer Ereignisregel

Um eine Ereignisregel zu erstellen, muss für die Gerätevorlage mindestens eine Ereignismessung definiert sein. In diesem Beispiel wird ein Warenautomat mit Kühlung verwendet, der einen Fehler des Lüftermotors meldet. Die Regel überwacht das vom Gerät gemeldete Ereignis und sendet eine E-Mail, sobald das Ereignis gemeldet wird.

1. Navigieren Sie zur Seite **Regeln**.

1. Wenn Sie noch keine Regeln erstellt haben, gelangen Sie auf folgenden Bildschirm:

   ![Noch keine Regeln](media/howto-create-event-rules-pnp/rules-landing-page1.png)

1. Wählen Sie **+ Neue Regel** aus, um die Typen von Regeln anzuzeigen, die Sie erstellen können.

1. Wählen Sie **Ereignis** aus, um eine Ereignisüberwachungsregel zu erstellen.

   ![Regeltypen](media/howto-create-event-rules-pnp/rule-types1.png)

1. Geben Sie einen aussagekräftigen Namen ein, um die Regel identifizieren zu können, und drücken Sie die EINGABETASTE.

1. Wählen Sie im Abschnitt **Bereiche** die Gerätedefinition aus, auf die diese Regel festgelegt werden soll. Auf diesem Bildschirm können Sie auch die Geräte filtern, auf die diese Regel angewendet wird, indem Sie die Option **+ Filter** verwenden. Die Regel gilt automatisch für alle Geräte unter der Gerätevorlage. Wenn Sie die Regel deaktivieren möchten, wählen Sie in der Kopfzeile die Schaltfläche **Deaktivieren** aus.

### <a name="configure-the-rule-conditions"></a>Konfigurieren der Regelbedingungen

Eine Bedingung definiert die Kriterien, die von der Regel überwacht werden.

1. Wählen Sie aus, ob die **Aggregation** aktiviert oder deaktiviert werden soll.

   - Ohne Aggregation wird diese Regel für jeden Ereignisdatenpunkt ausgelöst, der die Bedingung erfüllt. Wenn Sie z.B. die Bedingung der Regel so konfigurieren, dass sie ausgelöst wird, wenn ein Ereignis **Fan Motor Error** auftritt, wird die Regel fast sofort ausgelöst, wenn das Gerät dieses Ereignis meldet.
   - Wenn **Anzahl** als Aggregatfunktion verwendet wird, müssen Sie einen **Wert** und ein **Zeitfenster** angeben, in dem die Bedingung ausgewertet werden soll. In diesem Fall wird die Anzahl der Ereignisse aggregiert, und die Regel wird nur dann ausgelöst, wenn die aggregierte Ereignisanzahl mit dem Wert übereinstimmt.

1. Wählen Sie in der Dropdownliste **Messung** das Ereignis aus, das Sie überwachen möchten. In diesem Beispiel wurde das Ereignis **Fan Motor Error** (Lüftermotorfehler) ausgewählt.

1. Optional können Sie auch **Anzahl** als **Aggregation** festlegen und den Wert angeben, bei dem die Regel ausgelöst wird.

     Wenn Sie z. B. bei mehr als drei Geräteereignissen innerhalb von 5 Minuten eine Warnung erhalten möchten, wählen Sie das Ereignis aus, und legen Sie die Aggregatfunktion als **Anzahl**, den Operator als **größer als** und den **Wert** auf „3“ fest. Legen Sie das **Zeitfenster** auf **5 Minuten** fest. Die Regel wird ausgelöst, wenn mehr als drei Ereignisse innerhalb von 5 Minuten vom Gerät gesendet werden. Die Regelauswertungshäufigkeit ist identisch mit dem **Zeitfenster**. Dies bedeutet in diesem Beispiel, dass die Regel alle 5 Minuten ausgewertet wird.

 ![Bedingung](media/howto-create-event-rules-pnp/aggregate-condition-filled-out1.png)

> [!NOTE]
> Weitere Ereignismessungen können unter **Bedingung** hinzugefügt werden. Wenn mehrere Bedingungen angegeben werden, müssen alle Bedingungen erfüllt sein, damit die Regel ausgelöst wird. Jede Bedingung wird implizit durch eine „UND“-Klausel verknüpft. Wenn Sie die Aggregatfunktion verwenden, müssen alle Messungen aggregiert werden.

### <a name="configure-actions"></a>Konfigurieren von Aktionen

In diesem Abschnitt erfahren Sie, wie Aktionen eingerichtet werden, die ausgeführt werden sollen, wenn die Regel ausgelöst wird. Aktionen werden aufgerufen, wenn alle in der Regel angegebenen Bedingungen als TRUE ausgewertet werden.

1. Klicken Sie in den Abschnitten **Aktion** auf **+ Aktion**. Hier sehen Sie die Liste der verfügbaren Aktionen.  

   ![Hinzufügen einer Aktion](media/howto-create-event-rules-pnp/add-action1.png)

1. Wählen Sie die Aktion **E-Mail** aus, geben Sie einen Anzeigenamen für die Aktion und im Feld **An** eine gültige E-Mail-Adresse ein, und geben Sie dann einen Hinweis an, der im Textkörper der E-Mail angezeigt werden soll, wenn die Regel ausgelöst wird.

   > [!NOTE]
   > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) in Azure IoT Central.

   ![Konfigurieren einer Aktion](media/howto-create-event-rules-pnp/configure-action1.png)

1. Wählen Sie zum Speichern der Aktion die Option **Fertig** aus.

1. Um die neue Regel zu speichern, wählen Sie **Speichern** aus. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Ereignisse, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

## <a name="parameterize-the-rule"></a>Parametrisieren der Regel

Regeln können bestimmte Werte aus den **Geräteeigenschaften** als Parameter ableiten. Die Verwendung von Parametern ist in Szenarien hilfreich, in denen die Ereignisschwellenwerte für verschiedene Geräte variieren. Wählen Sie beim Erstellen der Regel eine Geräteeigenschaft aus, die den Schwellenwert angibt, z. B. **Maximaler Idealschwellenwert**, statt einen absoluten Wert wie 3 Ereignisse anzugeben. Wenn die Regel ausgeführt wird, gleicht sie die Geräteereignisse mit dem in der Geräteeigenschaft festgelegten Wert ab.

Die Verwendung von Parametern ist eine effektive Möglichkeit, die Anzahl der zu verwaltenden Regeln zu reduzieren.

Aktionen können auch über **Geräteeigenschaft** als Parameter konfiguriert werden. Wenn eine E-Mail-Adresse als Geräteeigenschaft gespeichert ist, kann sie bei der Definition der Empfängeradresse **verwendet** werden.

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen. Das Löschen der Regel entfernt sie aus der Gerätevorlage und allen zugehörigen Geräten.

## <a name="enable-or-disable-a-rule"></a>Aktivieren oder Deaktivieren einer Regel

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Verwenden Sie in der Regel die Schaltfläche **Aktivieren** oder **Deaktivieren**, um die Regel für alle in der Regel definierten Geräte zu aktivieren oder zu deaktivieren.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Aktivieren oder Deaktivieren einer Regel für ein Gerät

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Fügen Sie im Abschnitt **Bereiche** einen Filter hinzu, um ein bestimmtes Gerät in die Gerätevorlage aufzunehmen oder daraus auszuschließen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Regeln in Ihrer Azure IoT Central-Anwendung erstellen, wird als Nächstes der folgende Schritt empfohlen: [Verwalten von Geräten](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
