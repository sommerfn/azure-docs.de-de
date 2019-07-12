---
title: include file
description: include file
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 494f5fb6f2bfdec86cad01a37e57c3ec219a77f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133234"
---
## <a name="test-your-code"></a>Testen Ihres Codes

Testen Sie Ihren Code in einer der folgenden Umgebungen.

### <a name="test-with-nodejs"></a>Testen mit Node.js

Wenn Sie nicht Visual Studio verwenden, stellen Sie sicher, dass Ihr Webserver gestartet wurde.

1. Konfigurieren Sie den Server zum Lauschen am TCP-Port, der auf dem Speicherort Ihrer Datei *index.html* basiert. Starten Sie für Node.js den Webserver, um am Port zu lauschen, indem Sie an einer Eingabeaufforderung im Anwendungsordner die folgenden Befehle ausführen:

    ```bash
    npm install
    node server.js
    ```
1. Geben Sie in Ihrem Browser **http://\<span>\</span>localhost:30662** oder **http://\<span>\</span>localhost:{Port}** ,ein. Hierbei steht *Port* für den Port, an dem Ihr Webserver lauscht. Der Inhalt der Datei *index.html* und die Schaltfläche **Anmelden** sollten angezeigt werden.

### <a name="test-with-visual-studio"></a>Testen mit Visual Studio

Wählen Sie bei Verwendung von Visual Studio die Projektmappe aus, und drücken Sie dann F5, um das Projekt auszuführen. Der Browser wird geöffnet, und Sie werden zu http://<span></span>localhost:{Port} weitergeleitet, wo die Schaltfläche **Anmelden** angezeigt werden sollte.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Nachdem der Browser die Datei *index.html* geladen hat, klicken Sie auf **Anmelden**. Sie werden aufgefordert, sich mit dem Microsoft Identity Platform-Endpunkt anzumelden:

![Das Fenster zum Anmelden bei Ihrem JavaScript SPA-Konto](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Wenn Sie sich zum ersten Mal bei Ihrer Anwendung anmelden, werden Sie aufgefordert, ihr Zugriff auf Ihr Profil zu gewähren und sich anzumelden:

![Das Fenster „Angeforderte Berechtigungen“](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen

Nach der Anmeldung werden Ihre Benutzerprofilinformationen in der Antwort der Microsoft Graph-API zurückgegeben, die auf der Seite angezeigt wird.

![Ergebnisse des Aufrufs der Microsoft Graph-API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Registrierungsportal registriert ist. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt beispielsweise den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu.

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich benötigt (nicht empfohlen), können Sie die *clientId* bei den Aufrufen zum Beziehen von Token als Bereich verwenden.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
