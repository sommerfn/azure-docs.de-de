---
title: 'Registrieren einer öffentlichen Clientanwendung in Azure Active Directory: Azure API for FHIR'
description: In diesem Artikel wird erläutert, wie Sie eine öffentliche Clientanwendung in Azure Active Directory registrieren.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 69504bc9ba0420b47a26519a0b112ff102171254
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60709599"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Registrieren einer öffentlichen Clientanwendung in Azure Active Directory

In diesem Artikel erfahren Sie, wie Sie eine öffentliche Anwendung in Azure Active Directory registrieren. Clientanwendungsregistrierungen stellen Anwendungen in Azure Active Directory dar, die im Auftrag des Benutzers authentifiziert werden und API-Berechtigungen anfordern können. Öffentliche Clients sind Anwendungen wie mobile Anwendungen und Javascript-Einzelseitenanwendungen, die Geheimnisse nicht vertraulich behandeln können. Das Verfahren ähnelt dem [Registrieren eines vertraulichen Clients](register-confidential-azure-ad-client-app.md). Öffentliche Clients erweisen sich jedoch beim Speichern eines Anwendungsgeheimnisses als nicht vertrauenswürdig, daher muss kein Geheimnis hinzugefügt werden.

## <a name="app-registrations-in-azure-portal"></a>App-Registrierungen im Azure-Portal

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **App-Registrierungen (Vorschauversion)** :

    ![Azure-Portal. Neue App-Registrierung.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klicken Sie auf **Neue Registrierung**.

## <a name="application-registration-overview"></a>Übersicht über die Anwendungsregistrierung

1. Weisen Sie der Anwendung einen Anzeigenamen zu.

2. Stellen Sie eine Antwort-URL bereit. Unter der Antwort-URL werden Authentifizierungscodes an die Clientanwendung zurückgegeben. Sie können mehrere Antwort-URLs hinzufügen und vorhandene URLs später bearbeiten.

    ![Azure-Portal. Neue öffentliche App-Registrierung](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>API-Berechtigungen

Sie müssen ähnlich wie bei [vertraulichen Clientanwendungen](register-confidential-azure-ad-client-app.md) festlegen, welche API-Berechtigungen im Auftrag der Benutzer von dieser Anwendung angefordert werden können:

1. Öffnen Sie **API-Berechtigungen**, und wählen Sie Ihre [FHIR-API-Ressourcenanwendungsregistrierung](register-resource-azure-ad-client-app.md) aus:

    ![Azure-Portal. Neue öffentliche API-Berechtigungen](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-API-PERMISSIONS.png)

2. Wählen Sie die Bereiche aus, die die Anwendung anfordern kann.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine öffentliche Clientanwendung in Azure Active Directory registrieren. Als Nächstes wird eine FHIR-API in Azure bereitgestellt.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Open Source-FHIR-Servern](fhir-oss-powershell-quickstart.md)
