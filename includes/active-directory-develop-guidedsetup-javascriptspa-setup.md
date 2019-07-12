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
ms.openlocfilehash: 5ce0f18c1ec7a0fcb6465ab20e774976552687f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133739"
---
## <a name="set-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

> Möchten Sie stattdessen das Projekt dieses Beispiels herunterladen? Führen Sie einen der folgenden Schritte aus:
> 
> - Um das Projekt mit einem lokalen Webserver, wie z.B. Node.js, auszuführen, [laden Sie die Projektdateien herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - (Optional) Um das Projekt mit dem IIS-Server auszuführen, [laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> Um das Codebeispiel zu konfigurieren, bevor Sie es ausführen, wechseln Sie zum [Konfigurationsschritt](#register-your-application).

## <a name="prerequisites"></a>Voraussetzungen

* Zum Durcharbeiten dieses Tutorials ist ein lokaler Webserver erforderlich, z.B. [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) oder eine IIS Express-Integration mit [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Installieren Sie bei Verwenden von Node.js zum Ausführen des Projekts eine IDE (Integrated Development Environment, integrierte Entwicklungsumgebung), z.B. [Visual Studio Code](https://code.visualstudio.com/download), um die Projektdateien zu bearbeiten.

* Die Anweisungen in diesem Leitfaden basieren auf Node.js und Visual Studio 2017. Sie können aber auch eine andere Entwicklungsumgebung oder einen anderen Webserver verwenden.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Option 1: Node.js oder andere Webserver
> Vergewissern Sie sich, dass Sie [Node.js](https://nodejs.org/en/download/) installiert haben, und gehen Sie dann wie folgt vor:
> - Erstellen Sie einen Ordner zum Hosten Ihrer Anwendung.
>
> ### <a name="option-2-visual-studio"></a>Option 2: Visual Studio
> Wenn Sie Visual Studio verwenden und ein neues Projekt erstellen, gehen Sie so vor:
> 1. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
> 1. Wählen Sie unter **Visual C#\Web** die Option **ASP.NET-Webanwendung (.NET Framework)** aus.
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und klicken Sie auf **OK**.
> 1. Wählen Sie unter **Neue ASP.NET-Webanwendung** die Option **Leer** aus.

## <a name="create-the-spa-ui"></a>Erstellen der Benutzeroberfläche für die Single-Page-Webanwendung (SPA)
1. Erstellen Sie die Datei *index.html* für Ihre JavaScript-Einzelseitenanwendung. Gehen Sie bei Verwendung von Visual Studio wie folgt vor: Wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen** > **Neues Element** > **HTML-Seite**. Geben Sie der Datei den Namen *index.html*.

1. Fügen Sie der Datei *index.html* den folgenden Code hinzu:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Sie können die Version von „MSAL.js“ im obigen Skript durch die neueste veröffentlichte Version unter [MSAL.js-Releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases) ersetzen.
