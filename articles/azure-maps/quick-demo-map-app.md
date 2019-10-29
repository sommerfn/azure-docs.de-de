---
title: Interaktive Kartensuche mit Azure Maps | Microsoft-Dokumentation
description: 'Azure-Schnellstart: Erstellen einer Demo-App für die interaktive Kartensuche mit Azure Maps'
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 092da53c3179d939ebf0472219338b8d8392fda6
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554290"
---
# <a name="create-an-interactive-search-map-by-using-azure-maps"></a>Erstellen einer interaktiven Kartensuche mit Azure Maps

In diesem Artikel werden die Funktionen von Azure Maps beschrieben, um eine Karte mit interaktiven Suchfunktionen zu erstellen. Die folgenden grundlegenden Schritte werden erläutert:
* Erstellen eines eigenen Azure Maps-Kontos
* Abrufen Ihres Kontoschlüssels zur Verwendung in der Demowebanwendung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-an-account-and-get-your-key"></a>Erstellen eines Kontos und Abrufen Ihres Schlüssels

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) oben links auf **Ressource erstellen**.
2. Geben Sie **Maps** in das Feld **Marketplace durchsuchen** ein.
3. Wählen Sie in den **Ergebnissen** die Option **Maps** aus. Klicken Sie auf die unterhalb der Karte angezeigte Schaltfläche **Erstellen**.
4. Geben Sie auf der Seite **Azure Maps-Konto erstellen** die folgenden Werte ein:
   - **Name** des neuen Kontos
   - **Abonnement**, das Sie für dieses Konto verwenden möchten
   - Die **Ressourcengruppe** für dieses Konto. Sie können für die Ressourcengruppe die Option **Neue erstellen** oder **Vorhandene verwenden** auswählen.
   - Wählen Sie den gewünschten **Tarif** aus.
   - Lesen Sie die **Lizenz** und die  **Datenschutzerklärung**. Aktivieren Sie das Kontrollkästchen, um die Bedingungen zu akzeptieren.
   - Klicken Sie abschließend auf die Schaltfläche **Erstellen**.

     ![Erstellen eines Azure Maps-Kontos im Portal](./media/quick-demo-map-app/create-account.png)

5. Nachdem Ihr Konto erfolgreich erstellt wurde, öffnen Sie es, und navigieren Sie zum Abschnitt „Einstellungen“ des Kontomenüs. Wählen Sie **Authentifizierung** aus, um den primären und den sekundären Schlüssel für Ihr Azure Maps-Konto anzuzeigen. Kopieren Sie den Wert für den **Primärschlüssel** zur Verwendung im folgenden Abschnitt in die lokale Zwischenablage.

## <a name="download-the-application"></a>Herunterladen der Anwendung

1. Laden Sie den Inhalt der Datei [interactiveSearch.html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html) herunter, oder kopieren Sie ihn.
2. Speichern Sie den Inhalt dieser Datei lokal unter **AzureMapDemo.html**. Öffnen Sie die Datei in einem Text-Editor.
3. Suchen Sie nach der Zeichenfolge `<Your Azure Maps Key>`. Ersetzen Sie sie durch den **Primärschlüssel**, den Sie im vorherigen Abschnitt kopiert haben.

## <a name="open-the-application"></a>Öffnen der Anwendung

1. Öffnen Sie die Datei **AzureMapDemo.html** in einem Browser Ihrer Wahl.
2. Die Karte der Stadt Los Angeles wird angezeigt. Vergrößern und verkleinern Sie sie, um zu sehen, wie die Karte abhängig vom Zoomfaktor automatisch mit mehr oder weniger Informationen gerendert wird. 
3. Ändern Sie den Standardmittelpunkt der Karte. Suchen Sie in der Datei **AzureMapDemo.html** nach der Variable **center**. Ersetzen Sie den Wert des Längengrad/Breitengrad-Paars für diese Variable durch die neuen Werte **[-74,0060, 40,7128]** . Speichern Sie die Datei, und aktualisieren Sie Ihren Browser.
4. Probieren Sie die interaktiven Suchfunktionen aus. Geben Sie **Restaurants** in das Suchfeld in der oberen linken Ecke der Demowebanwendung ein.
5. Bewegen Sie den Mauszeiger über die Liste der Adressen und Standorte, die unterhalb des Suchfelds angezeigt wird. Beachten Sie dabei, wie über dem entsprechenden Pin auf der Karte Informationen zum jeweiligen Standort angezeigt werden. Zum Schutz von Privatunternehmen werden fiktive Namen und Adressen angezeigt.

    ![Webanwendung für die interaktive Suche](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den Tutorials wird ausführlich erläutert, wie Sie Azure Maps mit Ihrem Konto verwenden und konfigurieren. Wenn Sie mit den Tutorials fortfahren möchten, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um die Ressourcen zu bereinigen:

1. Schließen Sie den Browser, in dem die Webanwendung **AzureMapDemo.html** ausgeführt wird.
2. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**. Wählen Sie dann Ihr Azure Maps-Konto aus. Wählen Sie oben auf dem Blatt **Alle Ressourcen** die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Ihr Azure Maps-Konto und eine Demo-App erstellt. Sehen Sie sich die folgenden Tutorials an, um mehr über Azure Maps zu erfahren:

> [!div class="nextstepaction"]
> [Suchen nach Points of Interest in der Nähe mit Azure Maps](tutorial-search-location.md)

Weitere Codebeispiele und eine interaktive Codierumgebung finden Sie in den folgenden Anleitungen:

> [!div class="nextstepaction"]
> [Suchen nach einer Adresse mit dem Suchdienst von Azure Maps](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Verwenden des Azure Maps-Kartensteuerelements](how-to-use-map-control.md)
