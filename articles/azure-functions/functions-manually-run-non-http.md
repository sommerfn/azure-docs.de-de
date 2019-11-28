---
title: Manuelles Ausführen einer Azure-Funktion ohne HTTP-Trigger
description: Ausführen einer Azure-Funktion ohne HTTP-Trigger mithilfe einer HTTP-Anforderung
author: craigshoemaker
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: 8198ff6579aff839ff9aacb729e2f3f8d3472fae
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230478"
---
# <a name="manually-run-a-non-http-triggered-function"></a>Manuelles Ausführen einer Funktion ohne HTTP-Trigger

In diesem Artikel wird gezeigt, wie Sie eine Funktion ohne HTTP-Trigger manuell über eine speziell formatierte HTTP-Anforderung ausführen.

In bestimmten Kontexten müssen Sie unter Umständen bei Bedarf eine indirekt ausgelöste Azure-Funktion ausführen.  Zu Beispielen indirekter Trigger zählen u.a. [Funktionen eines Zeitplans](./functions-create-scheduled-function.md) und Funktionen, die als Folge einer [Aktion einer anderen Ressource](./functions-create-storage-blob-triggered-function.md) ausgeführt werden. 

Im folgenden Beispiel wird [Postman](https://www.getpostman.com/) verwendet. Sie können zum Senden von HTTP-Anforderungen aber auch [cURL](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) oder ein anderes Tool nutzen.

## <a name="define-the-request-location"></a>Festlegen des Anforderungsorts

Wenn Sie eine Funktion ohne HTTP-Trigger ausführen möchten, benötigen Sie eine Methode, mit der Sie die Ausführung der Funktion bei Azure anfordern können Die zum Senden dieser Anforderung verwendete URL hat ein bestimmtes Format.

![Festlegen des Anforderungsorts: Hostname + Ordnerpfad + Funktionsname](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Hostname:** Der öffentliche Ort der Funktions-App, der sich aus dem Namen der Funktions-App und *azurewebsites.net* bzw. Ihrer benutzerdefinierten Domäne zusammensetzt.
- **Ordnerpfad:** Wenn Sie über eine HTTP-Anforderung auf Funktionen ohne HTTP-Trigger zugreifen möchten, müssen Sie die Anforderung über die Ordner *admin/functions* senden.
- **Funktionsname:** Der Name der Funktion, die Sie ausführen möchten.

Dieser Anforderungsort wird in Postman zusammen mit dem Hauptschlüssel der Funktion in der Azure-Anforderung zum Ausführen der Funktion verwendet.

> [!NOTE]
> Bei lokaler Ausführung wird der Hauptschlüssel der Funktion nicht benötigt. Sie können direkt [die Funktion aufrufen](#call-the-function) und den Header `x-functions-key` weglassen.

## <a name="get-the-functions-master-key"></a>Abrufen des Hauptschlüssels der Funktion

Navigieren Sie im Azure-Portal zu Ihrer Funktion, klicken Sie auf **Verwalten**, und navigieren Sie zum Abschnitt **Hostschlüssel**. Klicken Sie in der Zeile *_master* auf die Schaltfläche **Kopieren**, um den Hauptschlüssel in die Zwischenablage zu kopieren.

![Kopieren des Hauptschlüssels auf dem Bildschirm zur Funktionsverwaltung](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Klicken Sie nach dem Kopieren des Hauptschlüssels auf den Funktionsnamen, um zum Fenster mit der Codedatei zurückzukehren. Klicken Sie anschließend auf die Registerkarte **Protokolle**. Hier werden Meldungen zur Funktion protokolliert, wenn Sie die Funktion manuell über Postman ausführen.

> [!CAUTION]  
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel in Ihrer Funktions-App gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in einer Anwendung verteilen.

## <a name="call-the-function"></a>Aufrufen der Funktion

Öffnen Sie Postman, und führen Sie die folgenden Schritte aus:

1. Geben Sie den **Anforderungsort in das Textfeld für die URL** ein.
2. Vergewissern Sie sich, dass die HTTP-Methode auf **POST** festgelegt ist.
3. **Klicken** Sie auf die Registerkarte **Header**.
4. Geben Sie **x-functions-key** als ersten **Schlüssel** ein, und fügen Sie den Hauptschlüssel (aus der Zwischenablage) ins Feld **Wert** ein.
5. Geben Sie **Content-Type** als zweiten **Schlüssel** und **application/json** als **Wert** ein.

    ![Header-Einstellungen in Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

6. **Klicken** Sie auf die Registerkarte **Text**.
7. Geben Sie **{ "input": "test" }** als Anforderungstext ein.

    ![Postman-Einstellungen für den Anforderungstext](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

8. Klicken Sie auf **Send**.

    ![Senden einer Anforderung mit Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postman meldet anschließend den Status **202 – Akzeptiert**.

Kehren Sie anschließend zu Ihrer Funktion im Azure-Portal zurück. Navigieren Sie zum Fenster *Protokolle*. Dort werden Meldungen aus dem manuellen Aufruf der Funktion angezeigt.

![Funktionsprotokollergebnisse des manuellen Aufrufs](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Nächste Schritte

- [Strategien zum Testen Ihres Codes in Azure Functions](./functions-test-a-function.md)
- [Lokales Debuggen von Azure-Funktionen mit Event Grid-Trigger](./functions-debug-event-grid-trigger-local.md)
