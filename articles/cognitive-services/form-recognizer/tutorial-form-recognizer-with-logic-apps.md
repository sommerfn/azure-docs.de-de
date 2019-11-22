---
title: 'Tutorial: Verwenden der Formularerkennung mit Azure Logic Apps zum Analysieren von Rechnungen: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial verwenden Sie die Formularerkennung mit Azure Logic Apps zum Erstellen eines Workflows, mit dem der Prozess zum Trainieren und anschließenden Testen eines Modells mit Beispieldaten automatisiert wird.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 10/27/2019
ms.author: nitinme
ms.openlocfilehash: bf6efbdbe19be28c005e2081c99827734ef10174
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73177000"
---
# <a name="tutorial-use-form-recognizer-with-azure-logic-apps-to-analyze-invoices"></a>Tutorial: Verwenden der Formularerkennung mit Azure Logic Apps zum Analysieren von Rechnungen

In diesem Tutorial erstellen Sie einen Workflow in Azure Logic Apps, für den die Formularerkennung genutzt wird. Hierbei handelt es sich um einen Dienst, der Teil der Azure Cognitive Services-Suite ist und zum Extrahieren von Daten aus Rechnungen verwendet wird. Sie verwenden die Formularerkennung, um zunächst ein Modell mit einem Beispieldataset zu trainieren und es anschließend mit einem anderen Dataset zu testen. Die in diesem Tutorial verwendeten Beispieldaten werden in Azure Storage-Blobcontainern gespeichert.

Hier ist angegeben, was in diesem Tutorial vermittelt wird:

> [!div class="checklist"]
> * Anfordern des Zugriffs für die Formularerkennung
> * Erstellen eines Azure Storage-Blobcontainers
> * Hochladen von Beispieldaten in den Azure-Blobcontainer
> * Erstellen einer Azure-Logik-App
> * Konfigurieren der Logik-App für die Verwendung einer Formularerkennungsressource
> * Testen des Workflows per Ausführung der Logik-App

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)

## <a name="request-access-for-form-recognizer"></a>Anfordern des Zugriffs für die Formularerkennung

Die Formularerkennung ist als Vorschauversion mit eingeschränktem Zugriff verfügbar. Um Zugriff auf die Vorschauversion zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und übermitteln Sie es. Nachdem Ihre Anforderung vom Azure Cognitive Services-Team genehmigt wurde, erhalten Sie eine E-Mail mit Anweisungen für den Zugriff auf den Dienst.

## <a name="understand-the-invoice-to-be-analyzed"></a>Verstehen der zu analysierenden Rechnung

Das Beispieldataset, das wir zum Trainieren und Testen des Modells verwenden, ist auf [GitHub](https://go.microsoft.com/fwlink/?linkid=2090451) als ZIP-Datei verfügbar. Laden Sie die ZIP-Datei herunter, und extrahieren Sie sie. Öffnen Sie im Ordner **/Train** eine Rechnungsdatei im PDF-Format. Beachten Sie, dass sie eine Tabelle mit Rechnungsnummer, Rechnungsdatum usw. enthält. 

> [!div class="mx-imgBorder"]
> ![Beispielrechnung](media/tutorial-form-recognizer-with-logic-apps/sample-receipt.png)

In diesem Tutorial wird beschrieben, wie Sie die Informationen aus diesen Tabellen im JSON-Format extrahieren, indem Sie einen per Azure Logic Apps und Formularerkennung erstellten Workflow verwenden.

## <a name="create-an-azure-storage-blob-container"></a>Erstellen eines Azure Storage-Blobcontainers

Sie verwenden diesen Container, um Beispieldaten hochzuladen, die zum Trainieren des Modells erforderlich sind.

1. Befolgen Sie die Anleitung zum Erstellen eines Speicherkontos unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-quickstart-create-account.md). Verwenden Sie **formrecostorage** als Speicherkontonamen.
1. Befolgen Sie die Anleitung unter [Erstellen eines Azure-Blobcontainers](../../storage/blobs/storage-quickstart-blobs-portal.md), um einen Container im Azure Storage-Konto zu erstellen. Verwenden Sie **formrecocontainer** als Containernamen. Legen Sie die öffentliche Zugriffsebene auf **Container (Anonymer Lesezugriff für Container und Blobs)** fest.

    > [!div class="mx-imgBorder"]
    > ![Erstellen eines Blobcontainers](media/tutorial-form-recognizer-with-logic-apps/create-blob-container.png)

## <a name="upload-sample-data-to-the-azure-blob-container"></a>Hochladen von Beispieldaten in den Azure-Blobcontainer

Laden Sie die Beispieldaten herunter, die auf [GitHub](https://go.microsoft.com/fwlink/?linkid=2090451) verfügbar sind. Extrahieren Sie die Daten in einen lokalen Ordner, und laden Sie den Inhalt des Ordners **/Train** in den oben erstellten Container **formrecocontainer** hoch. Befolgen Sie die Anleitung zum [Hochladen eines Blockblobs](../../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob), um Daten in einen Container hochzuladen.

Kopieren Sie die URL des Containers. Sie benötigen sie später im Tutorial. Wenn Sie das Speicherkonto und den Container mit den gleichen Namen wie in diesem Tutorial erstellt haben, lautet die URL „*https:\//formrecostorage.blob.core.windows.net/formrecocontainer/* “.

## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](./includes/create-resource.md)]

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

Sie können Azure Logic Apps verwenden, um Aufgaben und Workflows zu automatisieren. In diesem Tutorial erstellen Sie eine Logik-App, die durch den Empfang einer zu analysierenden Rechnung als E-Mail-Anhang ausgelöst wird. In diesem Workflow führen Sie die folgenden Aufgaben durch:
* Konfigurieren der Logik-App für die automatische Auslösung bei Erhalt einer E-Mail mit angefügter Rechnung
* Konfigurieren der Logik-App für die Verwendung des Formularerkennungsvorgangs zum Trainieren des Modells (**Train Model**) mit den Beispieldaten, die Sie in den Azure-Blobspeicher hochgeladen haben
* Konfigurieren der Logik-App für die Verwendung des Formularerkennungsvorgangs zum Analysieren eines Formulars (**Analyze Form**) mit dem bereits trainierten Modell. Mit dieser Komponente wird die von Ihnen bereitgestellte Rechnung für diese Logik-App basierend auf dem zuvor trainierten Modell analysiert.

Fangen wir an! Führen Sie die unten angegebenen Schritte aus, um Ihren Workflow einzurichten.

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

1. Geben Sie unter **Logik-App erstellen** Details zur Logik-App wie hier gezeigt ein. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | <*logic-app-name*> | Der Name Ihrer Logik-App, der nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten darf. In diesem Beispiel wird „My-First-Logic-App“ verwendet. |
   | **Abonnement** | <*Name des Azure-Abonnements*> | Der Name Ihres Azure-Abonnements |
   | **Ressourcengruppe** | <*Name der Azure-Ressourcengruppe*> | Der Name der [Azure-Ressourcengruppe](./../../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. In diesem Beispiel wird „My-First-LA-RG“ verwendet. |
   | **Location** | <*Azure-Region*> | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. In diesem Beispiel wird „USA, Westen“ verwendet. |
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. |
   ||||

1. Wählen Sie nach der Bereitstellung Ihrer App durch Azure für Ihre bereitgestellte Logik-App auf der Azure-Symbolleiste **Benachrichtigungen** > **Zu Ressource wechseln** aus. Sie können zum Suchen und Auswählen Ihrer Logik-App auch den Namen in das Suchfeld eingeben.

   Der Logik-App-Designer wird geöffnet, und es wird eine Seite mit einem Einführungsvideo und häufig verwendeten Triggern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   > [!div class="mx-imgBorder"]
   > ![Auswählen der leeren Vorlage für die Logik-App](./../../logic-apps/media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

### <a name="configure-the-logic-app-to-trigger-the-workflow-when-an-email-arrives"></a>Konfigurieren der Logik-App für die Auslösung des Workflows bei Empfang einer E-Mail

In diesem Tutorial wird der Workflow ausgelöst, wenn eine E-Mail mit einer angefügten Rechnung empfangen wird. Für dieses Tutorial wählen wir Office 365 als E-Mail-Dienst, aber Sie können auch einen anderen E-Mail-Anbieter nutzen.

1. Wählen Sie aus den Registerkarten die Option „Alle“, **Office 365 Outlook** und dann unter **Trigger** die Option **Wenn eine neue E-Mail empfangen wird** aus.

    ![Auslösen der Logik-App über eine eingehende E-Mail](media/tutorial-form-recognizer-with-logic-apps/logic-app-email-trigger.png)

1. Klicken Sie im Feld **Office 365 Outlook** auf **Anmelden**, und geben Sie die Details für die Anmeldung an einem Office 365-Konto ein.

1. Führen Sie im nächsten Dialogfeld die folgenden Schritte aus:
    1. Wählen Sie den Ordner aus, der auf neue E-Mails überwacht werden soll.
    1. Wählen Sie unter **Mit Anlagen** die Option **Ja** aus. Hierdurch wird sichergestellt, dass der Workflow nur von E-Mails mit Anlagen ausgelöst wird.
    1. Wählen Sie unter **Anlagen einschließen** die Option **Ja** aus. Hierdurch wird sichergestellt, dass der Inhalt der Anlage bei der weiteren Verarbeitung verwendet wird.

        > [!div class="mx-imgBorder"]
        > ![Konfigurieren des E-Mail-Triggers für die Logik-App](media/tutorial-form-recognizer-with-logic-apps/logic-app-specify-email-folder.png)

1. Klicken Sie oben in der Symbolleiste auf **Speichern**.

### <a name="configure-the-logic-app-to-use-form-recognizer-train-model-operation"></a>Konfigurieren der Logik-App für die Verwendung des Formularerkennungsvorgangs „Modell trainieren“

Bevor Sie den Dienst „Formularerkennung“ zum Analysieren von Rechnungen verwenden können, müssen Sie ein Modell trainieren, indem Sie dafür einige Beispielrechnungsdaten bereitstellen, die vom Modell analysiert und zum Lernen verwendet werden können.

1. Wählen Sie **Neuer Schritt** aus, und suchen Sie unter **Aktion auswählen** nach **Formularerkennung**. Wählen Sie in den angezeigten Ergebnissen den Eintrag **Formularerkennung** und dann unter den Aktionen, die für die Formularerkennung verfügbar sind, die Option **Modell trainieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Trainieren eines Formularerkennungsmodells](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-train-model.png)

1. Geben Sie im Dialogfeld „Formularerkennung“ einen Namen für die Verbindung an, und geben Sie dann die Endpunkt-URL und den Schlüssel an, die bzw. den Sie für die Formularerkennungsressource abgerufen haben.

    > [!div class="mx-imgBorder"]
    > ![Verbindungsname für Formularerkennung](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-create-connection.png)

    Klicken Sie auf **Create**.

1. Geben Sie im Dialogfeld **Modell trainieren** unter **Quelle** die URL für den Container ein, in den Sie die Beispieldaten hochgeladen haben.

    > [!div class="mx-imgBorder"]
    > ![Speichercontainer für Beispielrechnungen](media/tutorial-form-recognizer-with-logic-apps/source-for-train-model.png)

1. Klicken Sie oben in der Symbolleiste auf **Speichern**.

### <a name="configure-the-logic-app-to-use-the-form-recognizer-analyze-form-operation"></a>Konfigurieren der Logik-App des Formularerkennungsvorgangs „Formular analysieren“

In diesem Abschnitt fügen Sie dem Workflow den Vorgang zum Analysieren des Formulars (**Analyze Form**) hinzu. Bei diesem Vorgang wird das bereits trainierte Modell verwendet, um eine neue Rechnung zu analysieren, die für die Logik-App bereitgestellt wird.

1. Wählen Sie **Neuer Schritt** aus, und suchen Sie unter **Aktion auswählen** nach **Formularerkennung**. Wählen Sie in den angezeigten Ergebnissen den Eintrag **Formularerkennung** und dann unter den Aktionen, die für die Formularerkennung verfügbar sind, die Option **Analyze Form** (Formular analysieren) aus.

    > [!div class="mx-imgBorder"]
    > ![Analysieren eines Formularerkennungsmodells](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-analyze-model.png)

1. Gehen Sie im Dialogfeld **Analyze Form** (Formular analysieren) wie folgt vor:

    1. Klicken Sie in das Textfeld **Modell-D**, und wählen Sie im angezeigten Dialogfeld auf der Registerkarte **Dynamischer Inhalt** die Option **modelId** aus. Hierdurch geben Sie für die Datenflussanwendung die Modell-ID des Modells an, das Sie im letzten Abschnitt trainiert haben.

        > [!div class="mx-imgBorder"]
        > ![Verwenden der Modell-ID für die Formularerkennung](media/tutorial-form-recognizer-with-logic-apps/analyze-form-model-id.png)

    2. Klicken Sie in das Textfeld **Dokument**, und wählen Sie im angezeigten Dialogfeld auf der Registerkarte **Dynamischer Inhalt** die Option **Anlageninhalt** aus. Hierdurch konfigurieren Sie den Ablauf so, dass der Workflow von der Beispielrechnungsdatei ausgelöst wird, die an die E-Mail angefügt ist.

        > [!div class="mx-imgBorder"]
        > ![Verwenden der E-Mail-Anlage zum Analysieren von Rechnungen](media/tutorial-form-recognizer-with-logic-apps/analyze-form-input-data.png)

1. Klicken Sie oben in der Symbolleiste auf **Speichern**.

### <a name="extract-the-table-information-from-the-invoice"></a>Extrahieren der Tabelleninformationen aus der Rechnung

In diesem Abschnitt konfigurieren wir die Logik-App so, dass die Informationen aus der Tabelle in den Rechnungen extrahiert werden.

1. Wählen Sie die Option **Aktion hinzufügen** aus, und suchen Sie unter **Aktion auswählen** nach **Verfassen**. Wählen Sie aus den verfügbaren Aktionen dann erneut die Option **Verfassen** aus.
    ![Extrahieren von Tabelleninformationen aus der Rechnung](media/tutorial-form-recognizer-with-logic-apps/extract-table.png)

1. Klicken Sie im Dialogfeld **Verfassen** in das Textfeld **Eingaben**, und wählen Sie im angezeigten Dialogfeld die Option **Tabellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Extrahieren von Tabelleninformationen aus der Rechnung](media/tutorial-form-recognizer-with-logic-apps/select-tables.png)

1. Klicken Sie auf **Speichern**.

## <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Verwenden Sie zum Testen der Logik-App die Beispielrechnungen im Ordner **/Test** des Beispieldatasets, das Sie von [GitHub](https://go.microsoft.com/fwlink/?linkid=2090451) heruntergeladen haben. Führen Sie die folgenden Schritte aus:

1. Wählen Sie im Designer für Azure-Logik-Apps für Ihre App oben in der Symbolleiste die Option **Ausführen** aus. Der Workflow ist jetzt aktiv und wartet auf den Eingang einer E-Mail mit der angefügten Rechnung.
1. Senden Sie eine E-Mail mit angefügter Beispielrechnung an die E-Mail-Adresse, die Sie beim Erstellen der Logik-App angegeben haben. Stellen Sie sicher, dass die E-Mail in den Ordner eingefügt wird, den Sie beim Konfigurieren der Logik-App angegeben haben.
1. Nachdem die E-Mail im Ordner eingegangen ist, wird im Designer für Logik-Apps ein Fenster mit dem Status der einzelnen Phasen angezeigt. Im folgenden Screenshot ist dargestellt, dass eine E-Mail mit Anlage eingegangen ist und der Workflow ausgeführt wird.

    > [!div class="mx-imgBorder"]
    > ![Starten des Workflows durch das Senden einer E-Mail](media/tutorial-form-recognizer-with-logic-apps/logic-apps-email-arrived-progress.png)

1. Nachdem die Ausführung aller Phasen des Workflows abgeschlossen wurde, wird im Designer für Logik-Apps für jede Phase ein grünes Häkchen angezeigt. Wählen Sie im Designer-Fenster die Option **For each 2** (Für 2) und dann **Verfassen** aus.

    > [!div class="mx-imgBorder"]
    > ![Workflow abgeschlossen](media/tutorial-form-recognizer-with-logic-apps/logic-apps-verify-output.png)

    Kopieren Sie im Feld **AUSGABEN** die Ausgabe, und fügen Sie sie in einen beliebigen Text-Editor ein.

1. Vergleichen Sie die JSON-Ausgabe mit der Beispielrechnung, die Sie als Anlage der E-Mail gesendet haben. Vergewissern Sie sich, dass die JSON-Daten mit den Daten in der Tabelle der Rechnung übereinstimmen.

    ```json
    [
      {
        "id": "table_0",
        "columns": [
          {
            "header": [
              {
                "text": "Invoice Number",
                "boundingBox": [
                  38.5,
                  585.2,
                  113.4,
                  585.2,
                  113.4,
                  575.8,
                  38.5,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "7689302",
                  "boundingBox": [
                    38.5,
                    549.8,
                    77.3,
                    549.8,
                    77.3,
                    536.2,
                    38.5,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Invoice Date",
                "boundingBox": [
                  139.7,
                  585.2,
                  198.5,
                  585.2,
                  198.5,
                  575.8,
                  139.7,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "3/09/2015",
                  "boundingBox": [
                    139.7,
                    548.1,
                    184,
                    548.1,
                    184,
                    536.2,
                    139.7,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Invoice Due Date",
                "boundingBox": [
                  240.5,
                  585.2,
                  321,
                  585.2,
                  321,
                  575.8,
                  240.5,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "6/29/2016",
                  "boundingBox": [
                    240.5,
                    549,
                    284.8,
                    549,
                    284.8,
                    536.2,
                    240.5,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Charges",
                "boundingBox": [
                  341.3,
                  585.2,
                  381.2,
                  585.2,
                  381.2,
                  575.8,
                  341.3,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "$22,123.24",
                  "boundingBox": [
                    380.6,
                    548.5,
                    430.5,
                    548.5,
                    430.5,
                    536.2,
                    380.6,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "VAT ID",
                "boundingBox": [
                  442.1,
                  590,
                  474.8,
                  590,
                  474.8,
                  575.8,
                  442.1,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "QR",
                  "boundingBox": [
                    447.7,
                    549.8,
                    462.6,
                    549.8,
                    462.6,
                    536.2,
                    447.7,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          }
        ]
      }
    ]
    ```
    Sie haben dieses Tutorial nun erfolgreich abgeschlossen!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Logic Apps-Workflow eingerichtet, bei dem die Formularerkennung zum Trainieren eines Modells und Extrahieren des Inhalts einer Rechnung verwendet wird. Als Nächstes lernen Sie, wie Sie ein Trainingsdataset erstellen, damit Sie ein ähnliches Szenario für Ihre eigenen Formulare erstellen können.

> [!div class="nextstepaction"]
> [Erstellen eines Trainingsdatasets](build-training-data-set.md)