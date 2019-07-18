---
title: Ausfüllen bestimmter Felder für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation
description: Informationen zum Ausfüllen bestimmter Felder beim Registrieren einer benutzerdefiniert entwickelten Anwendung bei Azure AD
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bccaa28d34ebff47c7de73a4d9b3d8296ae9fef
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476123"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Ausfüllen bestimmter Felder für eine benutzerdefiniert entwickelte Anwendung

Dieser Artikel enthält eine kurze Beschreibung aller verfügbaren Felder im Anwendungsregistrierungsformular im [Azure-Portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Registrieren einer neuen Anwendung

-   Um eine neue Anwendung zu registrieren, navigieren Sie zum [Azure-Portal](https://portal.azure.com).

-   Klicken Sie im linken Navigationsbereich auf **Azure Active Directory**.

-   Wählen Sie **App-Registrierungen** aus, und klicken Sie auf **Hinzufügen**.

-   Dadurch wird das Formular zur Registrierung der Anwendung geöffnet.

## <a name="fields-in-the-application-registration-form"></a>Felder im Registrierungsformular für Anwendungen


| Feld            | BESCHREIBUNG                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| NAME             | Der Namen der Anwendung. Dieser sollte mindestens vier Zeichen umfassen.                |
| Unterstützte Kontotypen| Wählen Sie die Konten aus, die Ihre Anwendung unterstützen soll: „Nur Konten in diesem Organisationsverzeichnis“, „Konten in einem beliebigen Organisationsverzeichnis“ oder „Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten“.  |
| Umleitungs-URI (optional) | Wählen Sie die Art der App aus, die Sie erstellen (**Web** oder **Öffentlicher Client (Mobilgerät und Desktop)** ), und geben Sie dann den Umleitungs-URI (oder die Antwort-URL) für Ihre Anwendung ein. Geben Sie für Webanwendungen die Basis-URL Ihrer App an. http://localhost:31544 kann beispielsweise die URL für eine Web-App sein, die auf einem lokalen Computer ausgeführt wird. Benutzer können diese URL nutzen, um sich an einer Webclientanwendung anzumelden. Geben Sie für öffentliche Clientanwendungen den URI an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein (z. B. „myapp://auth“). Spezifische Beispiele für Webanwendungen oder native Anwendungen finden Sie in unseren [Schnellstartanleitungen](https://docs.microsoft.com/azure/active-directory/develop/#quickstarts).|

Wenn Sie die oben aufgeführten Felder ausgefüllt haben, wird Ihre Anwendung im Azure-Portal registriert, und Sie werden auf die Übersichtsseite der Anwendung weitergeleitet. Die Einstellungsseiten im linken Bereich unter **Verwalten** enthalten weitere Felder, über die Sie Ihre Anwendung weiter anpassen können. In den folgenden Tabellen werden alle Felder beschrieben. Ihnen wird möglicherweise nur ein Teil dieser Felder angezeigt, je nachdem, ob Sie eine Webanwendung oder eine öffentliche Clientanwendung erstellt haben.

### <a name="overview"></a>Übersicht
| Feld           | BESCHREIBUNG        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anwendungs-ID  | Wenn Sie eine Anwendung registrieren, weist Azure AD Ihrer Anwendung eine Anwendungs-ID zu. Die Anwendungs-ID kann verwendet werden, um Ihre Anwendung in Authentifizierungsanforderungen bei Azure AD eindeutig zu identifizieren und um auf Ressourcen wie die Graph-API zuzugreifen.                                                          |
| App-ID-URI      | Hierbei sollte es sich um einen eindeutigen URI handeln, üblicherweise in der Form **https://&lt;Mandantenname\_&gt;/&lt;Anwendungsname\_&gt;.** Dieser URI wird während der Autorisierungsgewährung als eindeutiger Bezeichner verwendet, um die Ressource anzugeben, für die das Token ausgestellt werden soll. Er wird auch zum „aud“-Anspruch im ausgestellten Zugriffstoken. |

### <a name="branding"></a>Branding

| Feld           | BESCHREIBUNG        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Neues Logo hochladen | Mit diesem Verfahren können Sie ein Logo für Ihre Anwendung hochladen. Das Logo muss im Format BMP, JPG oder PNG vorliegen, und die Datei sollte kleiner als 100 KB sein. Die Bildmaße sollten 215 × 215 Pixel betragen, die Bildmitte sollte 94 × 94 Pixel groß sein.|
| URL der Startseite   | Dies ist die Anmelde-URL, die während der Registrierung der Anwendung angegeben wurde.|

### <a name="authentication"></a>Authentication

| Feld           | BESCHREIBUNG        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abmelde-URL      | Dies ist die Abmelde-URL für das einmalige Abmelden. Azure AD sendet eine Abmeldeanforderung an diese URL, wenn ein Benutzer seine Azure AD-Sitzung über eine beliebige andere registrierte Anwendung löscht.|
| Unterstützte Kontotypen  | Diese Option gibt an, ob die Anwendung von mehreren Mandanten verwendet werden kann. Dies bedeutet in der Regel, dass externe Organisationen Ihre Anwendung nutzen können, indem sie die Anwendung in ihrem Mandanten registrieren und ihr Zugriff auf die Organisationsdaten gewähren.|
| Umleitungs-URLs      | Die Umleitungs- oder Antwort-URLs sind die Endpunkte, an die Azure AD die von Ihrer Anwendung angeforderten Token zurückgibt. Bei nativen Anwendungen werden Benutzer nach erfolgreicher Autorisierung an diese URIs weitergeleitet. Azure AD prüft, ob der Umleitungs-URI, den Ihre Anwendung in der OAuth 2.0-Anforderung bereitstellt, mit einem der im Portal registrierten Werte übereinstimmt.|

### <a name="certificates-and-secrets"></a>Zertifikate und Geheimnisse

| Feld           | BESCHREIBUNG        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Geheime Clientschlüssel            | Sie können geheime Clientschlüssel oder Schlüssel erstellen, um programmgesteuert und ohne Benutzerinteraktion auf Web-APIs zuzugreifen, die von Azure AD geschützt werden. Geben Sie auf der Seite **Neuer geheimer Clientschlüssel** eine Beschreibung des Schlüssels sowie das Ablaufdatum ein, und speichern Sie die Informationen, um den Schlüssel zu generieren. Stellen Sie sicher, dass Sie den Schlüssel an sicherer Stelle speichern, da Sie später nicht mehr darauf zugreifen können.             |

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Anwendungen mit Azure Active Directory](../manage-apps/what-is-application-management.md)
