---
title: Erste Schritte mit dem Web-Kartensteuerelement in Azure Maps | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die clientseitige JavaScript-Bibliothek des Azure Maps-Kartensteuerelements verwenden.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: c764460fb65287dab0031b27291d2f332cec3b3d
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756552"
---
# <a name="use-the-azure-maps-map-control"></a>Verwenden des Azure Maps-Kartensteuerelements

Mit der clientseitigen JavaScript-Bibliothek des Kartensteuerelements können Sie Karten und eingebettete Azure Maps-Funktionen in Ihrer Web- oder Mobilanwendung rendern.

## <a name="create-a-new-map-in-a-web-page"></a>Erstellen einer neuen Karte auf einer Webseite

Sie können eine Karte in eine Webseite einbetten, indem Sie die clientseitige JavaScript-Bibliothek des Kartensteuerelements verwenden.

1. Erstellen Sie eine neue HTML-Datei.

2. Laden Sie das Azure Maps Web SDK. Dies kann auf einem von zwei Wegen erfolgen:

    a. Verwenden Sie die global gehostete CDN-Version des Azure Maps Web SDK, indem Sie dem Stylesheet die URL-Endpunkte und Skriptverweise im `<head>`-Element der Datei hinzufügen:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

    b. Laden Sie alternativ den Quellcode des Azure Maps Web SDK lokal mithilfe des NPM-Pakets [azure-maps-control](https://www.npmjs.com/package/azure-maps-control), und hosten Sie es zusammen mit Ihrer App. Dieses Paket enthält außerdem TypeScript-Definitionen.

    > npm install azure-maps-control

    Fügen Sie dann dem `<head>`-Element der Datei Verweise auf das Azure Maps-Stylesheet und die Skriptquelle hinzu:

    ```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
    <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
    ```

    >[!Note]
    > Sie können TypeScript-Definitionen in Ihre Anwendung importieren, indem Sie Folgendes hinzufügen:
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

3. Um die Karte so zu rendern, dass sie den gesamten Bereich der Seite ausfüllt, fügen Sie dem `<head>`-Element das folgende `<style>`-Element hinzu.

    ```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
    ```

4. Fügen Sie im Seitenbereich ein `<div>`-Element hinzu, und geben Sie ihm die `id` **myMap**.

    ```HTML
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Definieren Sie zum Initialisieren des Kartensteuerelements im HTML-Text einen neuen Abschnitt, und erstellen Sie ein Skript. Übergeben Sie beim Erstellen einer Instanz der `Map`-Klasse die `id` der Karte `<div>` oder ein `HTMLElement` (z.B. `document.getElementById('myMap')`) als ersten Parameter. Verwenden Sie Ihren eigenen Azure Maps-Kontoschlüssel oder Ihre Azure Active Directory-Anmeldeinformationen (AAD), um die Karte mithilfe der [Authentifizierungsoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions) zu authentifizieren. Wenn Sie ein Konto erstellen oder Ihren Schlüssel ermitteln müssen, lesen Sie die Informationen unter [Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md). Die Option**language** gibt die Sprache an, die für Kartenbeschriftungen und Steuerelemente verwendet werden soll. Weitere Informationen zu unterstützten Sprachen finden Sie unter [Unterstützte Sprachen](supported-languages.md). Falls ein Abonnementschlüssel für die Authentifizierung verwendet wird:

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

    Falls Azure Active Directory (AAD) für die Authentifizierung verwendet wird:

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
    ```

    Eine Liste mit Beispielen zur Integration von Azure Active Directory (AAD) in Azure Maps finden Sie [hier](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples). 
    
    Weitere Informationen finden Sie im Dokument [Authentifizierung mit Azure Maps](azure-maps-authentication.md).

6. Es kann sinnvoll sein, dem Kopf der Seite die folgenden optionalen Metatagelemente hinzuzufügen:

    ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

7. Im Ergebnis sollte Ihre HTML-Datei ungefähr wie der folgende Code aussehen:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. Öffnen Sie die Datei in Ihrem Webbrowser, und zeigen Sie die gerenderte Karte an. Es sollte in etwa wie der folgende Code aussehen:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Verwenden des Kartensteuerelements" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>Verwenden des Kartensteuerelements</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="localizing-the-map"></a>Lokalisieren der Karte

Azure Maps bietet zwei verschiedene Möglichkeiten zum Festlegen der Sprache und der regionalen Ansicht für die Karte. Die erste Option besteht darin, diese Informationen dem globalen Namespace `atlas` hinzuzufügen. Dies führt dazu, dass alle Kartensteuerelementinstanzen in Ihrer App diese Einstellungen als Standard verwenden. Im Folgenden wird die Sprache auf Französisch („fr-FR“) und die regionale Ansicht auf „Auto“ festgelegt:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

Die zweite Option besteht darin, diese Informationen beim Laden der Karte folgendermaßen an die Kartenoptionen zu übergeben:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> Mit dem Web SDK ist es möglich, mehrere Karteninstanzen auf derselben Seite mit unterschiedlichen Sprach- und Regionseinstellungen zu laden. Darüber hinaus können diese Einstellungen aktualisiert werden, nachdem die Karte mithilfe von deren Funktion `setStyle` geladen wurde. 

Hier ist ein Beispiel für Azure Maps, bei dem die Sprache auf „fr-FR“ und die regionale Ansicht auf „Auto“ festgelegt wurde.

![Kartenbild mit Beschriftungen in Französisch](./media/how-to-use-map-control/websdk-localization.png)

Eine vollständige Liste der unterstützten Sprachen und regionalen Ansichten ist [hier](supported-languages.md) dokumentiert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Karten erstellen und mit ihnen interagieren:

> [!div class="nextstepaction"]
> [Erstellen einer Karte](map-create.md)

Erfahren Sie, wie Sie den Stil einer Karte auswählen:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](choose-map-style.md)

So fügen Sie Ihrer Karte weitere Daten hinzu:

> [!div class="nextstepaction"]
> [Erstellen einer Karte](map-create.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)

Eine Liste mit Beispielen zur Integration von Azure Active Directory (AAD) in Azure Maps finden Sie unter:

> [!div class="nextstepaction"]
> [Beispiele für AAD-Authentifizierung](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)