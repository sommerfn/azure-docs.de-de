---
title: Registrieren einer Dienstanwendung in Azure Active Directory – Azure API for FHIR
description: In diesem Artikel wird erläutert, wie Sie eine Dienstanwendung in Azure Active Directory registrieren.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 19f730c1d8a0fc0f809e8e16016795e725d80b61
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60709889"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Registrieren einer Dienstclientanwendun in Azure Active Directory

In diesem Artikel erfahren Sie, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Clientanwendungsregistrierungen sind Azure Active Directory-Darstellungen von Anwendungen, mit denen Token authentifiziert und abgerufen werden können. Ein Dienstclient ist zur Verwendung durch eine Anwendung vorgesehen, um ein Zugriffstoken ohne interaktive Authentifizierung eines Benutzers abzurufen. Der Client hat bestimmte Anwendungsberechtigungen und verwendet ein Anwendungsgeheimnis (Kennwort), wenn er Zugriffstoken abruft.

Führen Sie die folgenden Schritte aus, um einen neuen Dienstclient zu erstellen.

## <a name="app-registrations-in-azure-portal"></a>App-Registrierungen im Azure-Portal

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **App-Registrierungen (Vorschauversion)** :

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klicken Sie auf **Neue Registrierung**.

## <a name="service-client-application-details"></a>Details einer Dienstclientanwendung

* Der Dienstclient benötigt einen Anzeigenamen, und Sie können auch eine Antwort-URL angeben, aber diese wird in der Regel nicht verwendet.

    ![Azure-Portal. Registrierung einer neuen Dienstclientanwendung.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>API-Berechtigungen

Sie müssen der Dienstclientanwendungs Rollen gewähren. 

1. Öffnen Sie **API-Berechtigungen**, und wählen Sie Ihre [FHIR-API-Ressourcenanwendungsregistrierung](register-resource-azure-ad-client-app.md) aus:

    ![Azure-Portal. API-Berechtigungen eines Dienstclients.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Wählen Sie die Anwendungsrollen aus denjenigen aus, die für die Ressourcenanwendung definiert sind:

    ![Azure-Portal. Berechtigungen einer Dienstclientanwendung.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Gewähren Sie der Anwendung die Einwilligung. Sollten Sie nicht die erforderlichen Berechtigungen haben, wenden Sie sich an Ihren Azure Active Directory-Administrator:

    ![Azure-Portal. Administratoreinwilligung für Dienstclient](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Anwendungsgeheimnis

Der Dienstclient benötigt ein Geheimnis (Kennwort), das Sie beim Abrufen von Token verwenden.

1. Klicken Sie auf **Zertifikate &amp; Geheimnisse**.

2. Klicken Sie auf **Neuer geheimer Clientschlüssel**.

    ![Azure-Portal. Geheimer Schlüssel für Dienstclient](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Geben Sie eine Dauer für das Geheimnis an.

4. Sobald es generiert wurde, wird es nur einmal im Portal angezeigt. Notieren Sie sich das Geheimnis, und bewahren Sie es sicher auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Dienstclientanwendung in Azure Active Directory registrieren. Als Nächstes stellen Sie eine FHIR-API in Azure bereit.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Open Source-FHIR-Servern](fhir-oss-powershell-quickstart.md)