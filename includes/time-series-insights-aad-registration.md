---
title: include file
description: include file
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 07/02/2019
ms.openlocfilehash: a463e3cf475909c34054717460dc10dbba4ad8f0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543952"
---
> [!IMPORTANT]
> * Das neue Blatt **Azure Active Directory** > **App-Registrierungen** ersetzt Mai 2019 das veraltete Blatt **Azure Active Directory** > **App-Registrierungen (Legacy)** .
> * App-Registrierungen, die auf dem Legacy-Blatt erstellt oder angezeigt werden, werden automatisch auf dem neuen Blatt angezeigt.
> * Ausführliche Informationen zur Migration zur neuen Azure-App-Registrierungsumgebung finden Sie im [Schulungshandbuch zu Azure-App-Registrierungen](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) und [Schnellstart zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. Wählen Sie im [Azure-Portal](https://ms.portal.azure.com/) nacheinander **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.

   [![Registrierung einer neuen Anwendung in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > Der neue App-Registrierungsbereich für Azure Active Directory ermöglicht Ihnen, die angezeigten Apps zu filtern, indem Sie **Anwendungen mit Besitzer** auswählen.

    Ihre App wird hier aufgelistet, nachdem Sie sie registriert haben.

1. Geben Sie der Anwendung einen Namen, und wählen Sie **Nur Konten in diesem Organisationsverzeichnis** aus, um **Unterstützten Kontotypen** anzugeben, die auf die API zugreifen dürfen. Wählen Sie einen gültigen URI, an den die Benutzer nach der Authentifizierung weitergeleitet werden sollen, und sich dann **registrieren**.

   [![Erstellen der Anwendung in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Wichtige Informationen zu Azure Active Directory-Apps werden auf dem Blatt **Übersicht** Ihrer aufgeführten App angezeigt. Wählen Sie Ihre App unter **Anwendungen mit Besitzer** aus, und klicken Sie dann auf **Übersicht**.

   [![Kopieren der Anwendungs-ID](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Kopieren Sie Ihre **Anwendungs-ID (Client)** , um sie in Ihrer Clientanwendung zu verwenden.

1. Auf dem Blatt **Authentifizierung** sind wichtige Konfigurationseinstellungen für die Authentifizierung angegeben. 

    1. **Umleitungs-URIs** müssen mit der in der Authentifizierungsanforderung angegebenen Adresse übereinstimmen:

        * Wählen Sie für Anwendungen, die in einer lokalen Entwicklungsumgebung gehostet werden, **Öffentlicher Client (Mobilgerät und Desktop)** aus. Stellen Sie sicher, dass **Standardclienttyp** auf „Ja“ festgelegt ist.
        * Wählen Sie für in Azure App Service gehostete Einzelseiten-Apps **Web** aus.

    1. Aktivieren Sie den Flow der impliziten Genehmigung, indem Sie **ID-Token** aktivieren.

   [![Erstellen eines neuen geheimen Clientschlüssels](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   Klicken Sie auf **Speichern**.

1. Ordnen Sie Ihre Azure Active Directory-App Azure Time Series Insights zu. Wählen Sie **API-Berechtigungen** > **Berechtigung hinzufügen** > **Von meiner Organisation verwendete APIs** aus. 

    [![Zuordnen einer API zu Ihrer Azure Active Directory-App](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Geben in die Suchleiste `Azure Time Series Insights` ein, und wählen Sie dann `Azure Time Series Insights` aus.

1. Geben Sie als Nächstes die Art von API-Berechtigung an, die Ihre App benötigt. Standardmäßig ist **Delegierte Berechtigungen** hervorgehoben. Wählen Sie einen Berechtigungstyp und dann **Berechtigungen hinzufügen**.

    [![Angeben der Art der von Ihrer App benötigten API-Berechtigung](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)