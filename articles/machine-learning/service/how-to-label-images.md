---
title: Verwenden des Datenbeschriftungstools von Azure Machine Learning
title.suffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie die Datentaggingtools in einem Azure Machine Learning-Bezeichnungsprojekt verwenden.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: af12361ed11d0a16e5a5d0cfe5989bb3918ce154
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585586"
---
# <a name="how-to-tag-images-in-a-labeling-project"></a>Taggen von Bildern in einem Bezeichnungsprojekt

Nachdem Ihr Projektadministrator ein Bezeichnungsprojekt in Azure Machine Learning Studio erstellt hat, können Sie mit dem Bezeichnungstool schnell Daten für ein Projekt mit maschinellem Lernen vorbereiten. 

> [!div class="checklist"]
> * Zugreifen auf Ihre Bezeichnungsprojekte
> * Bezeichnungstools
> * Verwenden der Tools für bestimmte Bezeichnungsaufgaben

## <a name="prerequisites"></a>Voraussetzungen

* Die Bezeichnungsportal-URL für ein laufendes Datenbeschriftungsprojekt
* Ein [Microsoft-Konto](https://account.microsoft.com/account) oder
* Ein Azure Active Directory Konto für die Organisation und das Projekt

> [!Note]
> Der Projektadministrator kann die URL des Bezeichnungsportals auf der Registerkarte **Details** auf der Seite **Projektdetails** finden. 

## <a name="logging-in-to-the-projects-labeling-portal"></a>Anmelden beim Bezeichnungsportal des Projekts

Navigieren Sie zu der vom Projektadministrator bereitgestellten Bezeichnungsportal-URL. 

Melden Sie sich mit dem E-Mail-Konto an, über die der Administrator Sie dem Team hinzugefügt hat. Für die meisten Benutzer erfolgt die Anmeldung mit ihrem Microsoft-Konto. Wenn im Bezeichnungsprojekt Azure Active Directory verwendet wird, melden Sie sich darüber an. 

## <a name="understanding-the-labeling-task"></a>Grundlegendes zur Bezeichnungsaufgabe

Nach dem Anmelden gelangen Sie zur Übersichtsseite des Projekts. 

Der erste Schritt besteht darin, **ausführliche Anweisungen anzuzeigen**. Diese Anweisungen sind für Ihr Projekt spezifisch und erläutern die Art der Daten, mit denen Sie es zu tun haben, Hinweise zum Treffen von Entscheidungen und weitere relevante Informationen. Kehren Sie anschließend zur Projektseite zurück, und wählen Sie **Start labeling** (Bezeichnen beginnen) aus.

## <a name="common-features-of-the-labeling-task"></a>Allgemeine Features der Bezeichnungsaufgabe

Alle Bildbezeichnungsaufgaben schließen die Auswahl eines oder mehrerer geeigneter Tags in einer vom Projektadministrator angegebenen Gruppe ein. Sie können über die Zifferntasten auf der Tastatur zwischen den ersten neun Tags auswählen.  

In Bildklassifizierungsaufgaben können Sie mehrere Bilder gleichzeitig darstellen. Die verschiedenen Layouts können mithilfe der Symbole oberhalb des Bildbereichs ausgewählt werden. Sie können alle angezeigten Bilder gleichzeitig auswählen, indem Sie auf die Schaltfläche **Alle auswählen** klicken. Wählen Sie einzelne Fotos mithilfe der runden Auswahlschaltfläche in der rechten oberen Ecke des Bilds aus. Sie müssen mindestens ein Bild auswählen, um ein Tag anzuwenden. Wenn Sie mehrere Bilder ausgewählt haben, wird das Tag auf alle ausgewählten Fotos angewandt.

Hier haben wir ein 2 × 2-Layout ausgewählt und sind im Begriff, das Tag „Mammal“ (Säugetier) auf die Bilder mit dem Bär und dem Orca anzuwenden. Das Bild mit dem Hai wurde bereits als „Cartilaginous fish“ (Knorpelfisch) markiert, und der Leguan wurde noch nicht markiert.

![Layouts mit mehreren Bildern und Auswahl](media/how-to-label-images/layouts.png)

> [!Important] 
> Wechseln Sie Layouts nur für eine neue Seite mit Daten ohne Bezeichnung. Durch das Wechseln von Layouts werden die bereits ausgeführten Bezeichnungen der Seite gelöscht. 

Azure aktiviert die Schaltfläche **Senden**, wenn Sie alle Bilder auf der Seite markiert haben. Klicken Sie auf **Senden**, um Ihre Arbeit zu speichern. 

Nachdem Sie Tags für die gegebenen Daten übermittelt haben, aktualisiert Azure die Seite mit einer neuen Gruppe von Bildern aus der Arbeitswarteschlange.  

## <a name="tagging-images-for-multi-class-classification"></a>Tagging von Bildern für die Klassifizierung mit mehreren Klassen

Wenn Ihr Projekt vom Typ „Bildklassifizierung mit mehreren Klassen“ ist, weisen Sie dem gesamten Bild ein einzelnes Tag zu. Wählen Sie jederzeit die Seite **Anweisungen** aus, und navigieren Sie zu **Detaillierte Anweisungen anzeigen**, um eine projektspezifische Anleitung anzuzeigen. 

Wie bereits erwähnt, können Sie zwischen verschiedenen Layouts zum Darstellen von Bildern auswählen. Wenn Ihnen nach dem Auswählen eines Bilds und dem Zuweisen eines Tags auffällt, dass Sie einen Fehler gemacht haben, können Sie diesen beheben. Wenn Sie in der Bezeichnung unterhalb des Bilds auf das Ziel `X` klicken, löschen Sie das Tag. Wenn Sie stattdessen das Bild auswählen und eine andere Klasse auswählen, ändert sich das Tag in den neuen ausgewählten Wert.

## <a name="tagging-images-for-multi-label-classification"></a>Tagging von Bildern für die Klassifizierung mit mehreren Beschriftungen

Wenn Sie an einem Projekt vom Typ „Bildklassifizierung mit mehreren Beschriftungen“ arbeiten, wenden Sie ein _oder mehrere_ Tags auf ein Bild an. Wählen Sie jederzeit die Seite **Anweisungen** aus, und navigieren Sie zu **Detaillierte Anweisungen anzeigen**, um eine projektspezifische Anleitung anzuzeigen. 

Wählen Sie das Bild aus, das Sie bezeichnen möchten, und klicken Sie dann auf das Tag. Durch Auswählen des Tags wird es auf alle ausgewählten Bilder angewandt, und die Auswahl wird aufgehoben. Wenn Sie weitere Tags anwenden möchten, müssen Sie die Bilder erneut auswählen. In dieser Animation wird eine Taggingseite mit mehreren Bezeichnungen dargestellt:

* **Alle auswählen** wird zum Anwenden des Tags „Ocean“ (Meer) verwendet.
* Ein einzelnes Bild ist ausgewählt und als „Closeup“ (Nahaufnahme) markiert.
* Drei Bilder sind ausgewählt und als „Wide Angle“ (Weitwinkel) markiert.

![Animation mit dem Flow mit mehreren Bezeichnung](media/how-to-label-images/multilabel.gif)

Wenn Sie einen Fehler korrigieren müssen, können Sie entweder auf das `X` klicken, um einzelne Tags zu löschen, oder die Bilder auswählen, und das Tag auswählen. Dadurch wird das Tag aus allen ausgewählten Bildern gelöscht. Dieses Szenario wird hier gezeigt: Durch Klicken auf „Land“ wird dieses Tag aus den beiden ausgewählten Bildern gelöscht.

![Screenshot des Aufhebens einer Mehrfachauswahl](media/how-to-label-images/multiple-deselection.png)

Azure aktiviert die Schaltfläche **Senden** erst, nachdem Sie mindestens ein Tag auf jedes Bild angewandt haben. Klicken Sie auf **Senden**, damit Ihre Arbeit gespeichert wird.

## <a name="tagging-images-and-bounding-boxes-for-object-detection"></a>Tagging von Bildern und Begrenzungsrahmen für Objekterkennung

Wenn Ihr Projekt vom Typ „Object Identification (Bounding Boxes)“ (Objektidentifizierung (Begrenzungsrahmen)) ist, geben Sie Begrenzungsrahmen im Bild an und wenden Tags auf diese Felder an. Jedes Bild kann mehrere Begrenzungsrahmen aufweisen, die jeweils über ein einzelnes Tag verfügen. Verwenden Sie **Detaillierte Anweisungen anzeigen**, um zu ermitteln, ob das Hinzufügen mehrerer Begrenzungsrahmen für Ihr Projekt geeignet ist.

1. Wählen Sie für den Begrenzungsrahmen, den Sie erstellen möchten, ein Tag aus.
1. Wählen Sie das Tool für **rechteckige Felder** (![Tool für rechteckige Felder](media/how-to-label-images/rectangular-box-tool.png)) aus, oder drücken Sie „R“. 
1. Klicken Sie in das Ziel, und ziehen Sie, um einen ungefähren Begrenzungsrahmen zu erstellen.
    * Passen Sie den Begrenzungsrahmen durch Klicken und Ziehen der Kanten oder Ecken des Rahmens an.

![Screenshot der Erstellung eines einfachen Begrenzungsrahmens](media/how-to-label-images/bounding-box-sequence.png)

Wenn Sie den Begrenzungsrahmen löschen möchten, klicken Sie auf das X-förmige Ziel, das nach der Erstellung neben dem Begrenzungsrahmen angezeigt wird.

Sie können das Tag eines vorhandenen Begrenzungsrahmens nicht neu zuweisen. Wenn Sie einen Fehler bei der Tagzuweisung machen, müssen Sie den Begrenzungsrahmen löschen und einen neuen mit dem richtigen Tag erstellen.

Standardmäßig können vorhandene Begrenzungsrahmen bearbeitet werden. Das Tool zum **Sperren/Entsperren von Bereichen** (![Tool zum Sperren/Entsperren von Bereichen](media/how-to-label-images/lock-bounding-boxes-tool.png)) oder „L“ schaltet dieses Verhalten um. Wenn Bereiche gesperrt sind, können Sie nur die Form oder Position eines neuen Begrenzungsrahmens ändern.

Verwenden Sie das Tool zum **Bearbeiten von Bereichen** (![Tool zum Bearbeiten von Bereichen](media/how-to-label-images/regions-tool.png)) oder „M“, um ein vorhandenes Begrenzungsfeld anzupassen. Sie können auf die Ränder oder Ecken klicken und diese ziehen, um die Form anzupassen. Wenn Sie in den Innenbereich klicken, können Sie den gesamten Begrenzungsrahmen ziehen. Wenn Sie einen Bereich nicht bearbeiten können, haben Sie diese Funktionalität wahrscheinlich mit dem Tool zum **Sperren/Entsperren von Bereichen** umgeschaltet. 

Verwenden Sie das Tool für **vorlagenbasierte Felder** (![Tool für vorlagenbasierte Felder](media/how-to-label-images/template-box-tool.png)) oder „T“, um mehrere Begrenzungsrahmen derselben Größe zu erstellen. Wenn das Bild keine Begrenzungsrahmen aufweist und Sie vorlagenbasierte Felder aktivieren, erzeugt das Tool Felder mit einer Größe von 50 × 50 Pixel. Wenn Sie einen Begrenzungsrahmen erstellt haben und dann vorlagenbasierte Felder aktivieren, weisen neue Begrenzungsrahmen die Größe des letzten erstellten Rahmens auf. Die Größe von vorlagenbasierten Feldern kann nach der Platzierung angepasst werden. Beim Ändern der Größe eines vorlagenbasierten Felds wird nur die Größe dieses spezifischen Felds geändert. 

Wenn Sie _alle_ Begrenzungsrahmen im aktuellen Bild löschen möchten, können Sie das Tool zum **Löschen aller Regionen** (![Tool zum Löschen aller Regionen](media/how-to-label-images/delete-regions-tool.png)) auswählen. 

Nachdem Sie die Begrenzungsrahmen für das Bild erstellt haben, klicken Sie auf **Senden**, um Ihre Arbeit zu speichern. Die derzeit ausgeführte Arbeit wird erst gespeichert, wenn Sie auf die Schaltfläche **Senden** klicken. 

## <a name="finishing-up"></a>Letzte Schritte 

Wenn Sie eine Seite mit markierten Daten übermitteln, weist Azure Ihnen neue Daten ohne Bezeichnung aus einer Arbeitswarteschlange zu. Wenn keine unbezeichneten Daten mehr vorhanden sind, wird eine entsprechende Meldung mit einem Link zurück zur Startseite des Portals angezeigt. 

Wenn Sie sicher sind, dass Sie keine weitere Bezeichnung mehr durchführen, wählen Sie Ihren Namen in der rechten oberen Ecke des Bezeichnungsportals und dann **Abmelden** aus. Wenn Sie sich nicht abmelden, wird in Azure zu einem späteren Zeitpunkt eine Zeitüberschreitung festgestellt, und die Daten werden einer anderen Person für die Bezeichnung zugewiesen. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Bildklassifizierungsmodelle in Azure trainieren](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml).
* Erfahren Sie etwas über die [Erkennung von Objekten mit Azure und der schnelleren R-CNN-Technik](https://www.microsoft.com/developerblog/2017/10/24/bird-detection-with-azure-ml-workbench/).