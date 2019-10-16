---
title: 'Schnellstart: Microsoft Identity Platform – Java-Web-App | Azure'
description: Hier erfahren Sie, wie „Bei Microsoft anmelden“ mithilfe von OpenID Connect für eine Java-Web-App implementiert wird.
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/09/2019
ms.author: sagonzal
ms.custom: aaddev
ms.openlocfilehash: 8bb9073ccb4aef81b46b3b2b87730ddede5c0ff7
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72240206"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Java-Web-App

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

In dieser Schnellstartanleitung wird die Integration einer Java-Webanwendung in Microsoft Identity Platform beschrieben. Ihre App meldet einen Benutzer an, ruft ein Zugriffstoken zum Aufrufen der Microsoft Graph-API ab und sendet eine Anforderung an die Microsoft Graph-API. 

Am Ende dieses Leitfadens akzeptiert Ihre Anwendung Anmeldungen von persönlichen Microsoft-Konten (z.B. outlook.com, live.com u.a.) sowie von Geschäfts,- Schul- oder Unikonten von allen Unternehmen oder Organisationen, die Azure Active Directory nutzen.

![Zeigt, wie die in diesem Schnellstart generierte Beispiel-App funktioniert](media/quickstart-v2-java-webapp/java-quickstart.svg)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel benötigen Sie Folgendes:
- [Java Development Kit (JDK)](https://openjdk.java.net/) 8 oder höher und [Maven](https://maven.apache.org/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registrieren und Herunterladen Ihrer Schnellstart-App
> Die Schnellstartanwendung kann auf zwei Arten gestartet werden: Express (Option 1) oder manuell (Option 2).
> 
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Option 1: Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels
>
> 1. Navigieren Sie zu [Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **Registrieren** aus.
> 1. Befolgen Sie die Anweisungen zum Herunterladen und automatischen Konfigurieren der neuen Anwendung.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Option 2: Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels
> 
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
> Führen Sie die folgenden Schritte aus, um Ihre Anwendung zu registrieren und Ihrer Projektmappe manuell die Registrierungsinformationen Ihrer App hinzuzufügen:
>
> 1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
> 1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
> 1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
> 1. Wählen Sie **Neue Registrierung** aus.
> 1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen für Ihre Anwendung ein:
>    - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `java-webapp`).
>    - Lassen Sie **Umleitungs-URI** vorerst noch leer, und wählen Sie **Registrieren** aus.
> 1. Suchen Sie nach dem Wert **Anwendungs-ID (Client)** der Anwendung. Kopieren Sie diesen Wert. Sie benötigen ihn später.
> 1. Suchen Sie nach dem Wert **Verzeichnis-ID (Mandant)** der Anwendung. Kopieren Sie diesen Wert. Sie benötigen ihn später.
> 1. Wählen Sie das Menü **Authentifizierung** aus, und fügen Sie dann die folgenden Informationen hinzu:
>    - Fügen Sie unter **Umleitungs-URI** Folgendes hinzu: `http://localhost:8080/msal4jsamples/secure/aad` und `https://localhost:8080/msal4jsamples/graph/users`.
>    - Wählen Sie **Speichern** aus.
> 1. Wählen Sie im linken Menü die Option **Certificates & secrets** (Zertifikate und Geheimnisse) aus, und klicken Sie im Abschnitt **Geheime Clientschlüssel** auf **Neuer geheimer Clientschlüssel**:
>     
>    - Geben Sie eine Schlüsselbeschreibung (des Instanz-App-Geheimnisses) ein.
>    - Wählen Sie als Schlüsseldauer die Option **In 1 Jahr** aus.
>    - Wenn Sie auf **Hinzufügen** klicken, wird der Schlüsselwert angezeigt. 
>    - Kopieren Sie den Wert des Schlüssels. Sie benötigen ihn später.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Schritt 1: Konfigurieren Ihrer Anwendung im Azure-Portal
> Damit das Codebeispiel in dieser Schnellstartanleitung funktioniert, sind folgende Schritte erforderlich:
> 1. Fügen Sie Antwort-URLs als `http://localhost:8080/msal4jsamples/secure/aad` und `https://localhost:8080/msal4jsamples/graph/users` hinzu.
> 1. Erstellen Sie einen geheimen Clientschlüssel.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-aspnet-webapp/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-code-sample"></a>Schritt 2: Herunterladen des Codebeispiels
 
 [Codebeispiel herunterladen](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)
 
 #### <a name="step-3-configure-the-code-sample"></a>Schritt 3: Konfigurieren des Codebeispiels 
 
 1. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner.
 1. Falls Sie eine integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) verwenden, können Sie das Beispiel in Ihrer bevorzugten IDE öffnen. (Dieser Schritt ist optional.)
 1. Öffnen Sie die Datei **application.properties** (unter *src/main/resources/* ).
 1. Ersetzen Sie Anwendungseigenschaften.
   1. Suchen Sie nach `aad.clientId`, und aktualisieren Sie den Wert von `Enter_the_Application_Id_here` mit dem Wert der **Anwendungs-ID (Client)** der soeben registrierten Anwendung. 
   1. Suchen Sie nach `aad.authority`, und aktualisieren Sie den Wert von `Enter_the_Tenant_Name_Here` mit dem Wert von **Verzeichnis-ID (Mandant)** der registrierten Anwendung.
   1. Suchen Sie nach `aad.secretKey`, und aktualisieren Sie den Wert von `Enter_the_Client_Secret_Here` mit dem geheimen **Clientschlüssel**, den Sie in **Certificates & Secrets** (Zertifikate und Geheimnisse) für die registrierte Anwendung erstellt haben.

> [!div renderon="docs"]
> Hierbei gilt:
>
> - `Enter_the_Application_Id_here` ist die Anwendungs-ID für die von Ihnen registrierte Anwendung.
> - `Enter_the_Client_Secret_Here` ist der **geheime Clientschlüssel**, den Sie unter **Certificates & Secrets** (Zertifikate und Geheimnisse) für die registrierte Anwendung erstellt haben.

#### <a name="step-4-run-the-code-sample"></a>Schritt 4: Ausführen des Codebeispiels
1. Führen Sie das Codebeispiel aus, öffnen Sie einen Browser, und navigieren Sie zu *http://localhost:8080* .
1. Auf der ersten Seite befindet sich eine **Anmeldeschaltfläche**. Klicken Sie auf die **Anmeldeschaltfläche**. Daraufhin werden Sie zu Azure Active Directory umgeleitet. Der Benutzer wird zur Eingabe seiner Anmeldeinformationen aufgefordert.  
1. Nach erfolgreicher Authentifizierung bei Azure Active Directory wird der Benutzer zu *http://localhost:8080/msal4jsamples/secure/aad* umgeleitet. Er ist damit offiziell bei der Anwendung angemeldet, und auf der Seite werden Informationen für das angemeldete Konto angezeigt. Auf der Seite befinden sich nun außerdem Schaltflächen für Folgendes: 
    - *Sign Out* (Abmelden): Meldet den aktuellen Benutzer von der Anwendung ab und leitet ihn auf die Startseite um.
    - *Show Users* (Benutzer anzeigen): Ruft ein Token für Microsoft Graph ab und ruft anschließend Microsoft Graph auf. Dabei wird das Token an die Anforderung angefügt, um alle Benutzer des Mandanten abzurufen.

## <a name="more-information"></a>Weitere Informationen

### <a name="getting-msal"></a>Abrufen von MSAL
MSAL4J ist die Bibliothek zum Anmelden von Benutzern und Anfordern von Token, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet wird. Sie können Ihrer Anwendung MSAL4J hinzufügen, indem Sie Maven oder Gradle für die Verwaltung Ihrer Abhängigkeiten verwenden. Hierzu sind in Ihrer Anwendung folgende Änderungen an der Datei „pom.xml“ oder „build.gradle“ erforderlich: 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>0.5.0-preview</version>
</dependency>
```

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '0.5.0-preview'
```


### <a name="msal-initialization"></a>MSAL-Initialisierung
Sie können den Verweis auf MSAL4J hinzufügen, indem Sie den folgenden Code am Anfang der Datei hinzufügen, in der Sie MSAL4J verwenden möchten: 

```Java
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über Berechtigungen und die Einwilligung:

> [!div class="nextstepaction"]
> [Berechtigungen und Einwilligung](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent)

Weitere Informationen zum Authentifizierungsfluss für dieses Szenario finden Sie unter dem OAuth 2.0-Autorisierungscodeflow:

> [!div class="nextstepaction"]
> [Microsoft Identity Platform und der OAuth 2.0-Autorisierungscodeflow](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
