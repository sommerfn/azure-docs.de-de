---
title: 'Schnellstart: Clientbibliothek zur Gesichtserkennung für Python | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek zur Gesichtserkennung für Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: pafarley
ms.openlocfilehash: b265ec68039f03c09d45c9605965de8ce0baba99
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935870"
---
# <a name="quickstart-face-client-library-for-python"></a>Schnellstart: Clientbibliothek zur Gesichtserkennung für Python

Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek der Gesichtserkennung für Python. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Über den Dienst für die Gesichtserkennungs-API haben Sie Zugriff auf erweiterte Algorithmen für die Erkennung von menschlichen Gesichtern in Bildern.

Verwenden Sie die Clientbibliothek zur Gesichtserkennung für Python für Folgendes:

* Gesichtserkennung in einem Bild
* Suchen ähnlicher Gesichter
* Erstellen und Trainieren einer Personengruppe
* Identifizieren eines Gesichts
* Überprüfen von Gesichtern
* Erstellen einer Momentaufnahme für die Datenmigration

[Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-face) | [Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-face/) | [Beispiele](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-face-azure-resource"></a>Erstellen einer Azure-Ressource für die Gesichtserkennung

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für die Gesichtserkennung. Weitere Funktionen:

* Rufen Sie einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, der sieben Tage lang gültig ist. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel (`FACE_SUBSCRIPTION_KEY`).
 
### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie ein neues Python-Skript, etwa *quickstart-file.py*. Öffnen Sie es anschließend in Ihrem bevorzugten Editor bzw. Ihrer bevorzugten IDE, und importieren Sie die folgenden Bibliotheken:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_imports)]

Erstellen Sie dann Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Möglicherweise müssen Sie den ersten Teil des Endpunkts (`westus`) so ändern, dass er Ihrem Abonnement entspricht.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_subvars)]

> [!NOTE]
> Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Sie können die Clientbibliothek wie folgt installieren:

```console
pip install --upgrade azure-cognitiveservices-vision-face
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen des Face Python SDK.

|NAME|BESCHREIBUNG|
|---|---|
|[FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) | Diese Klasse stellt Ihre Autorisierung für die Verwendung des Gesichtserkennungsdiensts dar. Sie benötigen sie für alle Gesichtserkennungsfunktionen. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen. |
|[FaceOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python)|Diese Klasse behandelt die grundlegenden Erkennungs- und Wiedererkennungsaufgaben, die Sie für menschliche Gesichter ausführen können. |
|[DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)|Diese Klasse stellt alle Daten dar, die in einem einzelnen Gesicht in einem Bild erkannt wurden. Sie können damit ausführliche Informationen zum Gesicht abrufen.|
|[FaceListOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.facelistoperations?view=azure-python)|Diese Klasse verwaltet die in der Cloud gespeicherten **FaceList**-Konstrukte, die verschiedene Gesichter speichern. |
|[PersonGroupPersonOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongrouppersonoperations?view=azure-python)| Diese Klasse verwaltet die in der Cloud gespeicherten **Person**-Konstrukte, die eine Gruppe von Gesichtern speichern, die zu einer einzelnen Person gehören.|
|[PersonGroupOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongroupoperations?view=azure-python)| Diese Klasse verwaltet die in der Cloud gespeicherten **PersonGroup**-Konstrukte, die verschiedene **Person**-Objekte speichern. |
|[ShapshotOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.snapshotoperations?view=azure-python)|Mit dieser Klasse wird die Funktionalität für Momentaufnahmen verwaltet. Sie können mit ihr vorübergehend alle cloudbasierten Gesichtserkennungsdaten speichern und zu einem neuen Azure-Abonnement migrieren. |

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Clientbibliothek zur Gesichtserkennung für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Überprüfen von Gesichtern](#verify-faces)
* [Erstellen einer Momentaufnahme für die Datenmigration](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE]
> In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie für den Schlüssel für die Gesichtserkennung mit dem Namen `FACE_SUBSCRIPTION_KEY` [eine Umgebungsvariable erstellt haben](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication).

Instanziieren Sie einen Client mit Ihrem Endpunkt und Schlüssel. Erstellen Sie ein [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python)-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python)-Objekt zu erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_auth)]

## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Der folgende Code erkennt ein Gesicht in einem Remotebild. Die ID des erkannten Gesichts wird in der Konsole ausgegeben und darüber hinaus im Programmspeicher gespeichert. Anschließend werden die Gesichter in einem Bild mit mehreren Personen erkannt und ihre IDs ebenfalls in der Konsole ausgegeben. Indem Sie die Parameter in der Methode [detect_with_url](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#detect-with-url-url--return-face-id-true--return-face-landmarks-false--return-face-attributes-none--recognition-model--recognition-01---return-recognition-model-false--detection-model--detection-01---custom-headers-none--raw-false----operation-config-) ändern, können Sie mit jedem [DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)-Objekt andere Informationen zurückgeben.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detect)]

Weitere Szenarien zur Gesichtserkennung finden Sie im Beispielcode auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py).

### <a name="display-and-frame-faces"></a>Anzeigen und Umranden von Gesichtern

Der folgende Code gibt ein bestimmtes Bild an die Anzeige aus und zeichnet unter Verwendung der Eigenschaft „DetectedFace.faceRectangle“ Rechtecke um die Gesichter.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_frame)]

![Eine junge Frau mit einem roten Rechteck um das Gesicht](../images/face-rectangle-result.png)

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Der folgende Code verwendet ein einzelnes erkanntes Gesicht und durchsucht eine Gruppe anderer Gesichter auf Übereinstimmungen. Bei einer Übereinstimmung werden die Rechteckkoordinaten des entsprechenden Gesichts in der Konsole ausgegeben. 

### <a name="find-matches"></a>Suchen von Übereinstimmungen

Führen Sie zuerst den Code im obigen Abschnitt ([Gesichtserkennung in einem Bild](#detect-faces-in-an-image)) aus, um einen Verweis auf ein einzelnes Gesicht zu speichern. Führen Sie dann den folgenden Code aus, um Verweise auf mehrere Gesichter in einem Gruppenbild zu erhalten:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detectgroup)]

Fügen Sie anschließend den folgenden Codeblock hinzu, um nach Instanzen des ersten Gesichts in der Gruppe zu suchen. Informationen zum Ändern dieses Verhaltens finden Sie im Artikel zur Methode [find_similar](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#find-similar-face-id--face-list-id-none--large-face-list-id-none--face-ids-none--max-num-of-candidates-returned-20--mode--matchperson---custom-headers-none--raw-false----operation-config-).

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar)]

### <a name="print-matches"></a>Ausgeben von Übereinstimmungen

Verwenden Sie den folgenden Code, um die Übereinstimmungsdetails in der Konsole auszugeben:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar_print)]

## <a name="create-and-train-a-person-group"></a>Erstellen und Trainieren einer Personengruppe

Mit dem folgenden Code wird eine Personengruppe (**PersonGroup**) mit drei verschiedenen Personen (**Person**) erstellt. Er ordnet jeder **Person** verschiedene Beispielbilder zu. Danach folgt ein Trainingsschritt, um die einzelnen Personen zu erkennen. 

### <a name="create-persongroup"></a>Erstellen einer Personengruppe

Für dieses Szenario müssen Sie die folgenden Bilder im Stammverzeichnis Ihres Projekts speichern: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

Diese Gruppe von Bildern enthält drei verschiedene Bilder des Gesichts, die den drei Personen entsprechen. Der Code definiert drei **Person**-Objekte und ordnet sie Bilddateien zu, die mit `woman`, `man` und `child` beginnen.

Definieren Sie nach der Einrichtung Ihrer Bilder am Anfang Ihres Skripts eine Bezeichnung für das **PersonGroup**-Objekt, das Sie erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroupvars)]

Fügen Sie dann am Ende des Skripts den folgenden Code hinzu. Mit diesem Code werden ein **PersongGroup**-Objekt und drei **Person**-Objekte erstellt.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_create)]

### <a name="assign-faces-to-persons"></a>Zuweisen von Gesichtern zu Personen

Der folgende Code sortiert Ihre Bilder anhand ihres Präfixes, erkennt Gesichter und weist die Gesichter den einzelnen **Person**-Objekten zu.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_assign)]

### <a name="train-persongroup"></a>Trainieren von PersonGroup

Nach dem Zuweisen von Gesichtern müssen Sie das **PersonGroup**-Objekt trainieren, sodass es die den einzelnen **Person**-Objekten zugewiesenen visuellen Merkmale erkennen kann. Der folgende Code ruft die asynchrone **train**-Methode auf, ruft das Ergebnis ab und gibt den Status in der Konsole aus.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_train)]

## <a name="identify-a-face"></a>Identifizieren eines Gesichts

Der folgende Code sucht auf einem Bild mit mehreren Gesichtern nach der Identität der einzelnen Personen im Bild. Er vergleicht jedes erkannte Gesicht mit einem **PersonGroup**-Objekt, einer Datenbank mit verschiedenen **Person**-Objekten,deren Gesichtsmerkmale bekannt sind.

> [!IMPORTANT]
> Damit Sie dieses Beispiel ausführen können, müssen Sie zunächst den Code unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) ausführen.

### <a name="get-a-test-image"></a>Abrufen eines Testbilds

Der folgende Code sucht im Stammverzeichnis des Projekts nach dem Bild _test-image-person-group.jpg_ und erkennt die Gesichter im Bild. Dieses Bild befindet sich am gleichen Speicherort wie die für die Verwaltung von **PersonGroup** verwendeten Bilder: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify_testimage)]

### <a name="identify-faces"></a>Identifizieren von Gesichtern

Die Methode **identify** verwendet ein Array erkannter Gesichter und vergleicht diese mit einem **PersonGroup**-Objekt. Stimmt ein erkanntes Gesicht mit einem **Person**-Objekt überein, wird das Ergebnis gespeichert. Dieser Code gibt detaillierte Übereinstimmungsergebnisse in der Konsole aus.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify)]

## <a name="verify-faces"></a>Überprüfen von Gesichtern

Der Überprüfungsvorgang (Verify) akzeptiert eine Gesichtserkennungs-ID und entweder eine andere Gesichtserkennungs-ID oder ein **Person**-Objekt und ermittelt, ob sie zu derselben Person gehören.

Mit dem folgenden Code werden Gesichter in zwei Quellbildern erkannt und dann anhand eines Gesichts überprüft, das in einem Zielbild erkannt wurde.

### <a name="get-test-images"></a>Abrufen von Testbildern

Die folgenden Codeblöcke deklarieren Variablen, die für den Überprüfungsvorgang auf die Quell- und Zielbilder verweisen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_baseurl)]

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_photos)]

### <a name="detect-faces-for-verification"></a>Erkennen von Gesichtern für die Überprüfung

Mit dem folgenden Code werden Gesichter in den Quell- und Zielbildern erkannt und in Variablen gespeichert.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_detect)]

### <a name="get-verification-results"></a>Abrufen der Überprüfungsergebnisse

Der folgende Code vergleicht jedes Quellbild mit dem Zielbild und gibt eine Meldung mit dem Hinweis aus, ob sie zur gleichen Person gehören.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify)]

## <a name="take-a-snapshot-for-data-migration"></a>Erstellen einer Momentaufnahme für die Datenmigration

Mit dem Momentaufnahmefeature können Sie Ihre gespeicherten Gesichtserkennungsdaten (etwa ein trainiertes **PersonGroup**-Objekt) in ein anderes Abonnement für die Gesichtserkennung von Azure Cognitive Services verschieben. Sie können dieses Feature beispielsweise verwenden, wenn Sie ein **PersonGroup**-Objekt mit einem kostenlosen Testabonnement erstellt haben und dieses nun zu Ihrem kostenpflichtigen Abonnement migrieren möchten. Eine umfassende Übersicht über das Feature für Momentaufnahmen finden Sie unter [Migrieren Ihrer Gesichtserkennungsdaten in ein anderes Abonnement für die Gesichtserkennung](../Face-API-How-to-Topics/how-to-migrate-face-data.md).

In diesem Beispiel migrieren Sie das **PersonGroup**-Objekt, das Sie unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) erstellt haben. Sie können entweder zuerst diesen Abschnitt abschließen oder Ihre eigenen Konstrukte mit Gesichtserkennungsdaten verwenden.

### <a name="set-up-target-subscription"></a>Einrichten des Zielabonnements

Sie benötigen zunächst ein zweites Azure-Abonnement mit einer Gesichtserkennungsressource. Führen Sie dazu die Schritte im Abschnitt [Einrichten](#setting-up) aus. 

Erstellen Sie dann die folgenden Variablen am Anfang des Skripts. Darüber hinaus müssen Sie neue Umgebungsvariablen für die Abonnement-ID Ihres Azure-Kontos sowie den Schlüssel, den Endpunkt und die Abonnement-ID Ihres neuen Kontos (Zielkonto) erstellen. 

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshotvars)]

### <a name="authenticate-target-client"></a>Authentifizieren des Zielclients

Speichern Sie weiter unten in Ihrem Skript das aktuelle Clientobjekt als Quellclient, und authentifizieren Sie anschließend ein neues Clientobjekt für Ihr Zielabonnement. 

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_auth)]

### <a name="use-a-snapshot"></a>Verwenden einer Momentaufnahme

Die übrigen Momentaufnahmevorgänge werden in einer asynchronen Funktion ausgeführt. 

1. Der erste Schritt besteht darin, die Momentaufnahme zu **erstellen**. Dadurch werden die Gesichtserkennungsdaten Ihres ursprünglichen Abonnements an einem temporären Cloudspeicherort gespeichert. Diese Methode gibt eine ID zurück, mit der Sie den Status des Vorgangs abfragen.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_take)]

1. Fragen Sie als Nächstes die ID ab, bis der Vorgang abgeschlossen ist.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_wait)]

    In diesem Code wird die Funktion `wait_for_operation` verwendet, die Sie separat definieren sollten:

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_waitforop)]

1. Gehen Sie zurück zu ihrer asynchronen Funktion. Schreiben Sie mithilfe des Vorgangs **apply** die Gesichtserkennungsdaten in Ihr Zielabonnement. Mit dieser Methode wird ebenfalls eine ID zurückgegeben.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_apply)]

1. Fragen Sie als nächstes mit der Funktion `wait_for_operation` die ID ab, bis der Vorgang abgeschlossen ist.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_wait2)]

Wenn Sie diese Schritte ausgeführt haben, können Sie über Ihr neues (Ziel-)Abonnement auf Ihre Konstrukte mit Gesichtserkennungsdaten zugreifen.

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python` für die Schnellstartdatei aus.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)

Falls Sie in dieser Schnellstartanleitung ein **PersonGroup**-Objekt erstellt haben und dieses löschen möchten, führen Sie in Ihrem Skript den folgenden Code aus:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_deletegroup)]

Wenn Sie in dieser Schnellstartanleitung Daten mithilfe des Momentaufnahmefeatures migriert haben, müssen Sie auch das im Zielabonnement gespeicherte **PersonGroup**-Objekt löschen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_deletetargetgroup)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Gesichtserkennungsbibliothek für Python für allgemeine Aufgaben verwenden. In der Referenzdokumentation finden Sie weitere Informationen zur Bibliothek.

> [!div class="nextstepaction"]
> [Referenz zur Gesichtserkennungs-API (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python)

* [Was ist die Gesichtserkennungs-API?](../overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py).