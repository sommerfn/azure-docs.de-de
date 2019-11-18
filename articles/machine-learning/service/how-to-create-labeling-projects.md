---
title: Abrufen von Beschriftungen für Daten
titleSuffix: Azure Machine Learning
description: Dieser Artikel zeigt, wie Sie Beschriftungsprojekte zum Kennzeichnen von Daten für das maschinelle Lernen erstellen und ausführen.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 76f995901814c90ff9fd78585c98d56b3478e8b4
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612768"
---
# <a name="get-labels-for-data"></a>Abrufen von Beschriftungen für Daten

Das Beschriften großer Datenmengen bereitet in Projekten für maschinelles Lernen (ML) häufig Kopfzerbrechen. ML-Projekte mit einer Komponente für maschinelles Sehen – z. B. zur Bildklassifizierung oder Objekterkennung – erfordern im Allgemeinen Tausende von Bildern und entsprechende Beschriftungen. 
 
Das Azure Machine Learning-Studio bietet Ihnen eine Zentrale zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten. Beschriftungsprojekte helfen dabei, Daten, Beschriftungen und Teammitglieder zu koordinieren, sodass Sie die Beschriftungsaufgaben effizienter verwalten können. Zu den zurzeit unterstützten Aufgaben gehören die Bildklassifizierung – mit mehreren Beschriftungen oder mehreren Klassen – und die Objektidentifikation mithilfe von Begrenzungsrahmen.

Azure verfolgt den Fortschritt nach und verwaltet die Warteschlange mit unvollständigen Beschriftungsaufgaben. Beschriftungsersteller benötigen kein Azure-Konto, um an einem solchen Projekt teilzunehmen. Sobald sie mit ihrem Microsoft-Konto (MSA) oder über [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) authentifiziert sind, können sie sich Beschriftungsaufgaben widmen, so wie es ihre Zeit erlaubt. Diese Personen können Beschriftungen mithilfe von Tastenkombination zuweisen und ändern. 

Sie als Administrator können das Projekt starten und beenden, Personen und Teams hinzufügen und entfernen und den Fortschritt überwachen. Sie können beschriftete Daten entweder im COCO-Format oder als Azure ML-Dataset exportieren. 

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Projekts
> * Angeben der Daten und Struktur des Projekts
> * Verwalten der Teams und Personen, die am Projekt arbeiten
> * Ausführen und Überwachen des Projekts
> * Exportieren der Beschriftungen 

## <a name="prerequisites"></a>Voraussetzungen

* Die Daten, die beschriftet werden sollen und entweder in lokalen Dateien oder bereits im Azure-Speicher vorhanden sind.
* Die Beschriftungen, die zugewiesen werden sollen.
* Anweisungen für die Beschriftung.
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.
* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Erstellen eines Beschriftungsprojekts

Beschriftungsprojekte werden im [Azure Machine Learning-Studio](https://ml.azure.com/) verwaltet. Auf der Seite **Beschriftungsprojekte** können Sie Projekte, Teams und Personen verwalten. Einem Projekt können ein oder mehrere Teams zugewiesen sein, und ein Team kann aus einem oder mehreren Personen bestehen. 

Wenn sich Ihre Daten bereits in einem Azure-Blobspeicher befinden, sollten Sie diese als Datenspeicher zur Verfügung stellen, bevor Sie das Beschriftungsprojekt erstellen. Informationen dazu finden Sie unter [Erstellen und Registrieren von Datenspeichern](https://docs.microsoft.com/azure/machine-learning/service/how-to-access-data#create-and-register-datastores). 

Um ein Projekt zu erstellen, wählen Sie **Projekt hinzufügen** aus. Geben Sie einen geeigneten Namen an, und wählen Sie den **Beschriftungsaufgabentyp** aus. 

![Assistent zum Erstellen von Beschriftungsprojekten](media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Wählen Sie **Bildklassifizierung mit mehreren Beschriftungen** für Projekte aus, in denen **eine _oder mehrere_** Beschriftungen aus einer Reihe von Klassen auf ein Bild angewendet werden können. Ein Foto eines Hunds z. B. kann sowohl mit *Hund* als auch mit *Tag* beschriftet werden.
* Wählen Sie **Bildklassifizierung mit mehreren Klassen** für Projekte aus, in denen nur eine **einzige Klasse** aus einer Reihe von Klassen auf ein Bild angewendet werden kann.
* Wählen Sie **Objektidentifikation (umgebendes Rechteck)** für Projekte aus, in denen die Aufgabe darin besteht, sowohl einem Objekt in einem Bild eine Klasse zuzuweisen als auch einen Begrenzungsrahmen um dieses Objekt anzugeben.

Klicken Sie auf **Weiter**, wenn Sie bereit sind fortzufahren.

## <a name="specify-data-to-be-labeled"></a>Angeben der zu beschriftenden Daten

Wenn Sie bereits ein Dataset mit Ihren Daten erstellt haben, wählen Sie dieses aus der Dropdownliste **Vorhandenes Dataset auswählen** aus. Oder wählen Sie **Dataset erstellen** aus, um entweder einen vorhandenen Azure-Datastore auszuwählen oder lokale Dateien hochzuladen. 

### <a name="create-a-dataset-from-an-azure-datastore"></a>Erstellen eines Datasets aus einem Azure-Datenspeicher

In vielen Fällen ist es in Ordnung, lokale Dateien direkt hochzuladen, allerdings ist [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) sowohl stabiler als auch schneller, wenn es um das Übertragen von großen Datenmengen geht. Es wird empfohlen, Azure Storage-Explorer als Standardmethode zum Verschieben von Dateien zu verwenden.

So erstellen Sie ein Dataset aus Dateien, die Sie bereits in einem Azure-Blobspeicher gespeichert haben:

1. Wählen Sie **Dataset erstellen** und **Aus Datenspeicher** aus.
1. Geben Sie einen **Namen** für Ihr Dataset an.
1. Als **Datasettyp** müssen Sie „Datei“ auswählen.  
1. Wählen Sie den Datenspeicher aus. 
1. Wenn Ihre Daten sich in einem Unterordner des Blobspeichers befinden, klicken Sie auf **Durchsuchen**, um den Ordnerpfad auszuwählen. 
    * Sie können `/**` an den Pfad anfügen, um alle Dateien in den Unterordnern des ausgewählten Pfads einzubeziehen.
    * Verwenden Sie `**/*.*`, um alle Daten im aktuellen Container und den zugehörigen Unterordner einzubeziehen.
1. Geben Sie eine Beschreibung des Datasets ein.
1. Klicken Sie auf **Weiter**. 
1. Bestätigen Sie die Informationen. Sie können auf **Zurück** klicken, um Einstellungen zu ändern, oder **Erstellen** auswählen, um das Dataset zu erstellen.

### <a name="create-a-dataset-by-uploading-data"></a>Erstellen eines Datasets durch Hochladen von Daten

Wenn Sie Ihre Daten direkt hochladen möchten, gehen Sie folgendermaßen vor:

1. Wählen Sie **Dataset erstellen** und **Aus lokalen Dateien** aus.
1. Geben Sie einen **Namen** für Ihr Dataset an.
1. Als **Datasettyp** müssen Sie „Datei“ auswählen.
1. Wenn Sie **Erweiterte Einstellungen** auswählen, können Sie den Datenspeicher, den Container und den Pfad zu Ihren Daten anpassen.
1. Klicken Sie auf **Durchsuchen**, um lokale Dateien zum Hochladen auszuwählen.
1. Geben Sie eine Beschreibung des Datasets ein.
1. Klicken Sie auf **Weiter**. 
1. Bestätigen Sie die Informationen. Sie können auf **Zurück** klicken, um Einstellungen zu ändern, oder **Erstellen** auswählen, um das Dataset zu erstellen.

Die Daten werden in den Standardblobspeicher (`workspaceblobstore`) Ihres Azure ML-Arbeitsbereichs hochgeladen.

## <a name="specify-label-classes"></a>Angeben von Beschriftungsklassen

Auf der Seite **Beschriftungsklassen** geben Sie den Satz von Klassen an, die zum Kategorisieren Ihrer Daten verwendet werden. Wählen Sie diese Klassen mit Bedacht aus, da die Genauigkeit und Geschwindigkeit der Beschriftungsersteller dadurch beeinflusst wird, wie einfach aus diesen Klassen eine Auswahl getroffen werden kann. Ein Beispiel: Anstatt Gattung und Art von Pflanzen oder Tieren vollständig anzugeben, ist es möglicherweise besser, Feldcodes zu verwenden oder die Gattung abzukürzen. 

Geben Sie eine Beschriftung pro Zeile ein, und verwenden Sie die Schaltfläche **+** , um eine neue Zeile hinzuzufügen. Wenn Sie über mehr als drei oder vier, aber weniger als zehn Beschriftungen verfügen, können Sie diesen eine Ziffer voranstellen, sodass die Ersteller die Beschriftungen über die Zifferntasten eingeben und so schneller arbeiten können. 

## <a name="describe-the-labeling-task"></a>Beschreiben der Beschriftungsaufgabe

Es ist wichtig, die Beschriftungsaufgabe genau zu erläutern. Auf der Seite **Beschriftungsanweisungen** können Sie einen Link zu einer externen Website einfügen, auf der Beschriftungsersteller genaue Anweisungen erhalten. Stellen Sie Anweisungen bereit, die aufgabenorientiert und für die Zielgruppe geeignet sind. 

* Welche Beschriftungen sehen die Ersteller, und wie treffen sie ihre Auswahl? Gibt es Referenztext, auf den sie sich beziehen können?
* Was sollen sie tun, wenn keine Beschriftung geeignet erscheint? 
* Was sollen sie tun, wenn mehrere Beschriftungen geeignet erscheinen?
* Welchen Konfidenzschwellenwert sollen sie einer Beschriftung zuweisen? Sollen sie „nach bestem Wissen und Gewissen“ beschriften, wenn sie nicht sicher sind?
* Was sollen sie tun, wenn interessante Objekte teilweise verdeckt sind oder sich überlappen?
* Was sollen sie tun, wenn ein interessantes Objekt am Bildrand abgeschnitten ist?
* Was sollen sie tun, wenn sie glauben, einen Fehler gemacht zu haben, aber das Bild schon übermittelt wurde? 

Bei Begrenzungsrahmen stellen sich weitere wichtige Fragen:

* Wie wird der Begrenzungsrahmen für diese Aufgabe definiert? Soll er sich vollständig innerhalb des Objekts befinden, so genau wie möglich auf ein Objekt zugeschnitten sein, oder ist ein gewisser Spielraum akzeptabel? 
* Welches Maß an Sorgfalt und Konsistenz erwarten Sie von den Beschriftungserstellern beim Definieren von Begrenzungsrahmen?

>[!Note]
> Weisen Sie unbedingt darauf hin, dass die Beschriftungsersteller mit den Zifferntasten 1–9 aus den ersten neun Beschriftungen auswählen können. 

## <a name="initialize-the-labeling-project"></a>Initialisieren des Beschriftungsprojekts

Nachdem ein Beschriftungsprojekt initialisiert wurde, sind einige Aspekte unveränderlich: Sie können weder Aufgabentyp noch Dataset ändern. Beschriftungen sowie die URL für die Aufgabenbeschreibung dagegen können geändert werden. Überprüfen Sie die Einstellungen sorgfältig, bevor Sie das Projekt erstellen. Nachdem Sie das Projekt übermittelt haben, werden Sie wieder auf die Startseite für **Beschriftungen** geleitet, wo das Projekt als **Wird initialisiert** angezeigt wird. Diese Seite wird nicht automatisch aktualisiert. Aktualisieren Sie sie daher nach einiger Zeit manuell, und das Projekt wird als **Erstellt** angezeigt. 

## <a name="manage-teams-and-people"></a>Verwalten von Teams und Personen

Ein Beschriftungsprojekt erhält ein Standardteam und fügt Sie als Standardmitglied hinzu. Jedes Beschriftungsprojekt erhält ein neues Standardteam, Projekte können sich aber Teams teilen. Projekte können über mehrere Teams verfügen. Um ein Team zu erstellen, klicken Sie auf der Seite **Teams** auf **Team hinzufügen**. 

Personen werden auf der Seite **Personen** verwaltet. Sie können Personen anhand ihrer E-Mail-Adresse hinzufügen und entfernen. Jeder Beschriftungsersteller muss sich entweder mit seinem Microsoft-Konto oder (falls Sie dieses verwenden) über Azure Active Directory authentifizieren.  

Nachdem Sie eine Person hinzugefügt haben, können Sie diese einem oder mehreren Teams zuweisen. Navigieren Sie zur Seite **Teams**, wählen Sie das gewünschte Team aus, und verwenden Sie dann **Personen zuweisen** oder **Personen entfernen** aus.

Wenn Sie eine E-Mail an alle Teammitglieder senden möchten, können Sie das Team auswählen, um die Seite mit **Teamdetails** zu öffnen. Klicken Sie auf dieser Seite auf die Schaltfläche **E-Mail an Team**. Dadurch wird Ihr E-Mail-Editor geöffnet, der die Adressen aller Mitglieder des Teams enthält.

## <a name="run-and-monitor-the-project"></a>Ausführen und Überwachen des Projekts

Sobald das Projekt initialisiert wurde, startet Azure die Ausführung. Wenn Sie auf der Hauptseite **Beschriftung** auf das Projekt klicken, wird die Seite **Projektdetails** geöffnet. Auf der Registerkarte **Dashboard** wird der Fortschritt der Beschriftungsaufgabe angezeigt. 

Auf der Registerkarte**Daten** können Sie Ihre Dataset anzeigen und beschriftete Daten überprüfen. Wenn Sie falsch beschriftete Daten sehen, können Sie diese **auswählen** und auf **Ablehnen** klicken. Dadurch werden die Beschriftungen entfernt und die Daten wieder in die Warteschlange der unbeschrifteten Daten eingereiht. 

Auf der Registerkarte **Team** können Sie diesem Projekt Teams zuweisen oder eine Zuweisung aufheben. 

Wenn Sie das Projekt offline oder online schalten möchten, klicken Sie auf die Schaltfläche **Pause**/**Start**, die den Ausführungsstatus des Projekts umschaltet.

Sie können Daten direkt über die Seite **Projektdetails** beschriften, indem Sie **Daten beschriften** auswählen. Sie können Daten nur dann beschriften, wenn das Projekt ausgeführt wird. 

## <a name="export-the-labels"></a>Exportieren der Beschriftungen

Sie können die Beschriftungsdaten jederzeit für Experimente des maschinellen Lernens exportieren. Bildbeschriftungen können im [COCO-Format](http://cocodataset.org/#format-data) oder als Azure ML-Dataset exportiert werden. Sie finden die Schaltfläche **Exportieren** auf der Seite **Projektdetails** Ihres Beschriftungsprojekts.

Die COCO-Datei wird im Standardblobspeicher des Azure ML-Arbeitsbereichs in einem Ordner unter **export/coco** erstellt. Sie können im Abschnitt **Datasets** des Studios auf das exportierte Azure ML-Dataset zugreifen. Die Seite mit Datasetdetails bietet auch Beispielcode für den Zugriff auf Ihre Beschriftungen aus Python.

![Exportiertes Dataset](media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Nächste Schritte

* Beschriften von Bildern für die [Bildklassifizierung oder Objekterkennung](how-to-label-images.md)
* Weitere Informationen zu [Azure Machine Learning und Studio](../compare-azure-ml-to-studio-classic.md)