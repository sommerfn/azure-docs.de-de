---
title: Bereitstellen von Datenfeedback in Azure Maps | Microsoft-Dokumentation
description: Bereitstellen von Datenfeedback mithilfe des Azure Maps-Feedbacktools.
author: walsehgal
ms.author: v-musehg
ms.date: 08/19/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 076f98cb240014bcc88a395902203413e31fe0f1
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641870"
---
# <a name="provide-data-feedback-to-azure-maps"></a>Bereitstellen von Datenfeedback in Azure Maps

Azure Maps ist seit Mai 2018 allgemein verfügbar und bietet aktuelle Kartendaten, einfach zu verwendende REST-APIs und leistungsstarke SDKs zur Unterstützung unserer Unternehmenskunden in einer Vielzahl von geschäftlichen Anwendungsfällen. Die reale Welt ändert sich jede Sekunde, und es ist entscheidend für uns, dass wir für unsere Kunden eine tatsachenbasierte digitale Repräsentation bereitstellen. Unsere Kunden, die das Öffnen oder Schließen von Einrichtungen planen, müssen sicherstellen, dass unsere Karten umgehend aktualisiert werden, damit Sie Lieferung, Wartung oder Kundendiensteinsätze in den richtigen Einrichtungen effizient planen können. Wir haben das Azure Maps-Datenfeedbacktool erstellt, um unseren Kunden die Bereitstellung von direktem Datenfeedback zu ermöglichen. Das Datenfeedback der Kunden geht direkt an unsere Datenanbieter und deren Karten-Editoren, die Feedback schnell beurteilen und in unsere Kartenprodukte integrieren können.  

Das [Azure Maps-Datenfeedback](https://feedback.azuremaps.com)tool bietet unseren Kunden eine einfache Möglichkeit zum Bereitstellen von Feedback zu Kartendaten, insbesondere für geschäftliche POIs (Point of Interest) und Wohnadressen. In diesem Artikel werden Sie angeleitet, wie Sie mit dem Azure Maps-Feedbacktool verschiedene Arten von Feedback bereitstellen.

## <a name="add-a-business-place-or-a-residential-address"></a>Hinzufügen eines Geschäftsorts oder einer Wohnadresse 

Möglicherweise möchten Sie Feedback zu einem fehlenden Point of Interest oder einer fehlenden Wohnadresse auf der Karte geben. Hierfür gibt es zwei Möglichkeiten: Öffnen Sie das Azure Map-Datenfeedbacktool, und suchen Sie nach den Koordinaten des fehlenden Standorts, und klicken Sie dann auf „Ort hinzufügen“.

  ![Suchen eines fehlenden Orts](./media/how-to-use-feedback-tool/search-poi.png)

Oder Sie können mit der Karte interagieren und auf den Ort klicken, um eine Stecknadel an den Koordinaten abzulegen, und dann auf „Ort hinzufügen“ klicken. 

  ![Hinzufügen einer Stecknadel](./media/how-to-use-feedback-tool/add-poi.png)

Nachdem Sie geklickt haben, werden Sie zu einem Formular weitergeleitet, um die entsprechenden Details für den Ort bereitzustellen.

  ![Hinzufügen eines Orts](./media/how-to-use-feedback-tool/add-a-place.png)

## <a name="fix-a-business-place-or-a-residential-address"></a>Korrigieren eines Geschäftsorts oder einer Wohnadresse 

Das Feedbacktool ermöglicht Ihnen außerdem das Suchen und Auffinden eines Geschäftsorts oder einer Adresse und das Bereitstellen von Feedback zur Korrektur der Adresse oder der Stecknadelposition, wenn diese nicht korrekt sind. Um Feedback zur Korrektur der Adresse bereitzustellen, verwenden Sie die Suchleiste, um nach einem Geschäftsort oder einer Wohnadresse zu suchen. Klicken Sie in der Ergebnisliste auf den gewünschten Ort, und klicken Sie auf „Diesen Ort korrigieren“.

  ![Suchen des zu korrigierenden Orts](./media/how-to-use-feedback-tool/fix-place.png)

Wenn Sie Feedback zum Korrigieren der Adresse bereitstellen möchten, füllen Sie das Formular „Ort korrigieren“ aus, und klicken Sie dann auf die Schaltfläche „Absenden“.

  ![Korrekturformular](./media/how-to-use-feedback-tool/fix-form.png)

Wenn die Position der Stecknadel für den Ort falsch ist, aktivieren Sie in dem Formular „Ort korrigieren“ das Kontrollkästchen „Stecknadelposition ist falsch“, und verschieben Sie die Stecknadel an die richtigen Position, und klicken Sie dann auf die Schaltfläche „Absenden“.

  ![Verschieben der Stecknadelposition](./media/how-to-use-feedback-tool/move-pin.png)

## <a name="add-a-comment"></a>Hinzufügen eines Kommentars 

Zusätzlich zur Suchmöglichkeit gestattet Ihnen das Feedbacktool außerdem, einen frei formulierten Kommentartext hinzuzufügen, um auf den Ort bezogene Details anzugeben. Um einen Kommentar hinzuzufügen, suchen Sie den Ort, oder klicken Sie auf den Ort, und klicken Sie dann auf „Kommentar hinzufügen“, schreiben Sie einen Kommentar, und klicken Sie dann auf „Absenden“. 

  ![Hinzufügen eines Kommentars](./media/how-to-use-feedback-tool/add-comment.png)

## <a name="track-status"></a>Nachverfolgen des Status 

Sie können auch den Status Ihrer Anforderung überprüfen, indem Sie das Kontrollkästchen „Ich möchte den Status nachverfolgen“ aktivieren und Ihre E-Mail-Adresse während des Stellens der Anforderung angeben. Sie erhalten einen Nachverfolgungslink in der E-Mail, der einen aktuellen Status für Ihre Anforderung bereitstellt. 

  ![Feedbackstatus](./media/how-to-use-feedback-tool/feedback-status.png)


## <a name="next-steps"></a>Nächste Schritte

Um technische Fragen im Zusammenhang mit Azure Maps bereitzustellen, besuchen Sie:

* [Azure Maps Stack Overflow](https://stackoverflow.com/questions/tagged/azure-maps)
* [Azure Maps-Feedbackforum](https://feedback.azure.com/forums/909172-azure-maps)