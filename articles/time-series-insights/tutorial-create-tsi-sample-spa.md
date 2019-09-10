---
title: 'Tutorial: Erstellen einer einseitigen Azure Time Series Insights-Web-App | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie eine einseitige Webanwendung erstellen, die Daten aus einer Azure Time Series Insights-Umgebung abfragt und rendert.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 031e8074585426584d7ef63a103c9c2b4d90e6c3
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194232"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Tutorial: Erstellen einer einseitigen Azure Time Series Insights-Web-App

In diesem Tutorial werden die Schritte zum Erstellen Ihrer eigenen einseitigen Webanwendung (Single-Page Application, SPA) für den Zugriff auf Azure Time Series Insights-Daten erläutert.

In diesem Tutorial erhalten Sie Informationen zu Folgendem:

> [!div class="checklist"]
> * Anwendungsentwurf
> * Registrieren Ihrer Anwendung bei Azure Active Directory (Azure AD)
> * Erstellen, Veröffentlichen und Testen Ihrer Webanwendung

> [!NOTE]
> * Den Quellcode für dieses Tutorial finden Sie auf [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Die [Clientbeispiel-App](https://insights.timeseries.azure.com/clientsample) von Time Series Insights wird gehostet, um die in diesem Tutorial verwendete fertige App anzuzeigen.

Registrieren Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/), falls Sie noch keins besitzen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine kostenlose Kopie von Visual Studio. Laden Sie für den Einstieg die [2017 oder 2019 Communityversionen](https://www.visualstudio.com/downloads/) herunter.

* Die IIS Express-, Web Deploy- und Azure Cloud Services Core Tools-Komponenten für Visual Studio. Fügen Sie die Komponenten hinzu, indem Sie Ihre Visual Studio-Installation ändern.

## <a name="understand-application-design"></a>Grundlegendes zum Anwendungsentwurf

Die Time Series Insights-Beispiel-SPA bildet die Grundlage für den in diesem Tutorial verwendeten Entwurf und Code. Der Code verwendet die Time Series Insights-JavaScript-Clientbibliothek. Die Time Series Insights-Clientbibliothek stellt eine Abstraktion für zwei API-Hauptkategorien bereit:

- **Wrappermethoden für den Aufruf der Time Series Insights-Abfrage-APIs**: REST-APIs, mit denen Sie Time Series Insights-Daten unter Verwendung von JSON-basierten Ausdrücken abrufen können. Die Methoden sind unter dem „TsiClient.server“-Namespace der Bibliothek organisiert.

- **Methoden zum Erstellen und Auffüllen mehrerer Typen von Diagrammsteuerelementen:** Methoden, mit denen Sie Time Series Insights-Daten auf einer Webseite visualisieren können. Die Methoden sind unter dem „TsiClient.ux“-Namespace der Bibliothek organisiert.

In diesem Tutorial werden darüber hinaus Daten aus der Time Series Insights-Umgebung der Beispielanwendung verwendet. Ausführliche Informationen zur Struktur der Time Series Insights-Beispielanwendung und wie sie die Time Series Insights-Clientbibliothek verwendet, finden Sie im Tutorial [Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek](tutorial-explore-js-client-lib.md).

## <a name="register-with-azure-ad"></a>Registrieren bei Azure AD

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

## <a name="build-and-publish"></a>Erstellen und Veröffentlichen

1. Erstellen Sie ein Verzeichnis, um die Anwendungsprojektdateien zu speichern. Besuchen Sie dann jede der folgenden URLs. Klicken Sie in der oberen rechten Ecke der Seite mit der rechten Maustaste auf den Link **Raw** (Roh), und wählen Sie dann **Speichern unter** aus, um die Dateien in Ihrem Projektverzeichnis zu speichern.

   - [*index.html*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): HTML und JavaScript für die Seite
   - [*sampleStyles.css*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): das CSS-Stylesheet

   > [!NOTE]
   > Je nach Browser müssen Sie eventuell die Dateierweiterungen in HTML oder CSS ändern, bevor Sie die Datei speichern.

1. Überprüfen Sie, ob die erforderlichen Komponenten in Visual Studio installiert sind. Die IIS Express-, Web Deploy- und Azure Cloud Services Core Tools-Komponenten für Visual Studio müssen installiert sein.

    [![Visual Studio: Ändern der installierten Komponenten](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Ihre Visual Studio-Erfahrung kann sich von den dargestellten Beispielen leicht unterscheiden, abhängig von Ihrer Version und den Konfigurationseinstellungen.

1. Öffnen Sie Visual Studio, und melden Sie sich an. Um ein Projekt für die Webanwendung zu erstellen, wählen Sie im Menü **Datei** die Einträge **Öffnen** > **Website** aus.

    ![[Visual Studio: Erstellen einer neuen Projektmappe](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. Wählen Sie im Bereich **Website öffnen** das Arbeitsverzeichnis aus, in dem Sie die HTML- und CSS-Dateien gespeichert haben, und wählen Sie dann **Öffnen** aus.

   [![Visual Studio: Das Menü „Datei“ mit den Optionen „Öffnen“ und „Website“](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. Wählen Sie im Visual Studio-Menü **Ansicht** den Eintrag **Projektmappen-Explorer** aus. Ihre neue Projektmappe wird geöffnet. Sie enthält ein Websiteprojekt (Globussymbol), das die HTML- und CSS-Dateien enthält.

   [![Visual Studio: Die neue Projektmappe im Projektmappen-Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Bevor Sie Ihre App veröffentlichen, müssen Sie die Konfigurationseinstellungen in *index.html* ändern.

   1. Heben Sie die Auskommentierung der drei Zeilen unter dem Kommentar `"PROD RESOURCE LINKS"` auf, um die Abhängigkeiten von ENTWICKLUNG auf PRODUKTION umzustellen. Heben Sie die Auskommentierung der drei Zeilen unter dem Kommentar `"DEV RESOURCE LINKS"` auf.

      [!code-html[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Ihre Abhängigkeiten sollte wie im folgenden Beispiel kommentiert sein:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   1. Um die App für die Verwendung Ihrer Azure AD-App-Registrierungs-ID zu konfigurieren, ändern Sie den Wert `clientID` so, dass die **Anwendungs-ID** verwendet wird, die Sie in **Schritt 3** beim [Registrieren der Anwendung bei Azure AD](#register-with-azure-ad) kopiert haben. Wenn Sie in Azure AD eine **Abmelde-URL** erstellt haben, legen Sie diesen Wert als Wert von `postLogoutRedirectUri` fest.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Beispiel:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Wenn Sie mit den Änderungen fertig sind, speichern Sie *index.html*.

1. Veröffentlichen Sie die Webanwendung in Ihrem Azure-Abonnement als Azure App Service.  

   > [!NOTE]
   > Mehrere Optionen in den Screenshots, die in den folgenden Schritten angezeigt werden, werden automatisch mit Daten aus Ihrem Azure-Abonnement aufgefüllt. Es kann möglicherweise ein paar Sekunden dauern, bis jeder Bereich vollständig geladen ist.  

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Websiteprojektknoten, und wählen Sie dann **Web-App veröffentlichen** aus.  

      [![Visual Studio: Auswählen der Projektmappen-Explorer-Option „Web-App veröffentlichen“](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Wählen Sie **Starten** aus, um die Veröffentlichung Ihrer App zu beginnen.

      [![Visual Studio: Der Bereich „Veröffentlichungsprofil“](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Wählen Sie diese Option aus, um eine neue Azure App Service-Instanz zu veröffentlichen oder eine vorhandene Instanz zu verwenden.

      [![Auswählen einer Azure App Service-Instanz](media/tutorial-create-tsi-sample-spa/vs-publish-select-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-select-target.png#lightbox)

   1. Wählen Sie das Abonnement aus, das Sie zum Veröffentlichen der Anwendung verwenden möchten. Wählen Sie das Projekt **TsiSpaApp** aus. Wählen Sie anschließend **OK** aus.

      [![Visual Studio: Der App Service-Bereich „Veröffentlichungsprofil“](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Wählen Sie **Veröffentlichen** aus, um die Webanwendung bereitzustellen.

      [![Visual Studio: Die Option „Veröffentlichen“ und die Veröffentlichungsprotokollausgabe](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Im Visual Studio-Bereich **Ausgabe** wird ein Protokoll der erfolgreichen Veröffentlichung angezeigt. Nach Abschluss der Bereitstellung öffnet Visual Studio die Webanwendung auf einer Browserregisterkarte und fordert Sie auf, sich anzumelden. Nach der erfolgreichen Anmeldung sind alle Time Series Insights-Steuerelemente mit Daten aufgefüllt.

   1. Navigieren Sie zu Ihrer Web-App, und melden Sie sich an, um die gerenderten visuellen Time Series Insights-Daten anzuzeigen.

      [![Überprüfen der gehosteten Web-App](media/tutorial-create-tsi-sample-spa/vs-publish-hosted-app.png)](media/tutorial-create-tsi-sample-spa/vs-publish-hosted-app.png#lightbox)

## <a name="troubleshoot"></a>Problembehandlung  

Fehlercode/Bedingung | BESCHREIBUNG
---------------------| -----------
*AADSTS50011: No reply address is registered for the application.* (AADSTS50011: Für die Anwendung ist keine Antwortadresse registriert.) | In der Azure AD-Registrierung fehlt eine Eigenschaft vom Typ **Antwort-URI**. Navigieren Sie für Ihre Azure AD-Anwendungsregistrierung zu **Authentifizierung** > **Umleitungs-URIs**. Vergewissern Sie sich, dass der **Umleitungs-URI** vorhanden ist, den Sie in **Schritt 2** oder **Schritt 4** beim [Registrieren der Anwendung für die Verwendung von Azure AD](#register-with-azure-ad) angegeben haben.
*AADSTS50011: The reply url specified in the request does not match the reply urls configured for the application: '\<Application ID GUID>'* (AADSTS50011: Die in der Anforderung angegebene Antwort-URL entspricht nicht den für die Anwendung konfigurierten Antwort-URLs: <Anwendungs-ID>.) | Der in **Schritt 6.b** unter [Erstellen und Veröffentlichen der Webanwendung](#build-and-publish) angegebene Wert für `postLogoutRedirectUri` muss dem Wert entsprechen, den Sie in Ihrer Azure AD-Anwendungsregistrierung unter **Authentifizierung** > **Umleitungs-URIs** angegeben haben. |
Die Webanwendung wird zwar geladen, es wird jedoch eine nicht formatierte Nur-Text-Anmeldeseite mit weißem Hintergrund angezeigt. | Vergewissern Sie sich, dass die in **Schritt 6** unter [Erstellen und Veröffentlichen der Webanwendung](#build-and-publish) erläuterten Pfade richtig sind. Kann die Webanwendung die CSS-Dateien nicht finden, ist die Seite nicht ordnungsgemäß formatiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden mehrere ausgeführte Azure-Dienste erstellt. Falls Sie diese Tutorialreihe nicht fertig stellen möchten, empfehlen wir, dass Sie alle Ressourcen löschen, damit keine unnötigen Kosten entstehen.

Im linken Menü des Azure-Portals:

1. Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die Sie für die Time Series Insights-Umgebung erstellt haben. Wählen Sie am oberen Rand der Seite **Ressourcengruppe löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus.
1. Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann die Ressourcengruppe aus, die mit dem Solution Accelerator für die Gerätesimulation erstellt wurde. Wählen Sie am oberen Rand der Seite **Ressourcengruppe löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anwendungsentwurf
> * Registrieren Ihrer Anwendung bei Azure AD
> * Erstellen, Veröffentlichen und Testen Ihrer Webanwendung

Dieses Tutorial ist in Azure AD integriert, sodass die Identität des angemeldeten Benutzers verwendet wird, um ein Zugriffstoken abzurufen. Informationen zum Zugreifen auf die Time Series Insights-API mithilfe der Identität einer Dienst- oder Daemon-Anwendung finden Sie in diesem Artikel:

> [!div class="nextstepaction"]
> [Authentifizierung und Autorisierung für die Azure Time Series Insights-API](time-series-insights-authentication-and-authorization.md)
