---
title: Konfigurieren des Features „1-Klick-SSO“ für Ihre Anwendung über den Azure AD-App-Katalog | Microsoft-Dokumentation
description: Schritte zum Konfigurieren von 1-Klick-SSO für Ihre Anwendung über den Azure AD-App-Katalog.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 358240823da469551e254356fc0613bea20d78c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064814"
---
# <a name="one-click-sso-feature-for-azure-ad-gallery-applications"></a>Das Feature „1-Klick-SSO“ für Azure AD-Kataloganwendungen

 In diesem Tutorial erfahren Sie, wie Sie 1-Klick-SSO für alle SAML-Anwendungen einrichten, die eine Benutzeroberfläche für die Konfiguration des einmaligen Anmeldens (SSO) bieten.

## <a name="introduction-to-one-click-sso"></a>Einführung in 1-Klick-SSO

Das neue Feature "1-Klick-SSO" dient zur Konfiguration des einmaligen Anmeldens für Azure AD-Kataloganwendungen, die das SAML-Protokoll unterstützen. Wir diese Option haben auf der Azure AD-Seite zur SSO-Konfiguration bereitgestellt, damit unsere Kunden die Azure AD-Metadaten auf Anwendungsseite automatisch konfigurieren können. Ziel ist es, Kunden bei der schnellen Einrichtung des einmaligen Anmeldens mit minimalem manuellen Aufwand zu unterstützen. 

## <a name="advantages-of-the-one-click-sso"></a>Vorteile von 1-Klick-SSO

- Schnelle SSO-Konfiguration der Kataloganwendungen, bei denen Kunden die manuelle Einrichtung auf Anwendungsseite vornehmen müssen
- Effizientere und präzisere Art der Konfiguration
- Wegfall der Kommunikation mit dem Partner oder Unterstützung bei der Einrichtung, da die Anwendung die Benutzeroberfläche für die SAML-Konfiguration bereitstellt

## <a name="prerequisites"></a>Voraussetzungen

- Aktives Abonnement der Anwendung mit Administratoranmeldeinformationen, die Sie mit 1-Klick-SSO konfigurieren möchten.
- **Erweiterung zur sicheren Anmeldung bei „Meine Apps“** von Microsoft, die im Browser installiert ist. Wenn Sie mehr zu dieser Erweiterung wissen möchten, folgen Sie diesem [Link](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-feature-step-by-step-details"></a>Schritt-für-Schritt-Anweisungen für das Feature „1-Klick-SSO“

1. Fügen Sie die Anwendung aus dem Azure AD-App-Katalog hinzu.

2. Klicken Sie auf „Einmaliges Anmelden“.

3. Klicken Sie auf „Einmaliges Anmelden aktivieren“.

4. Geben Sie im Abschnitt „Grundlegende SAML-Konfiguration“ die erforderlichen Konfigurationswerte ein.

    > [!NOTE] 
    > Wenn die Anwendung eine Konfiguration benutzerdefinierter Ansprüche benötigt, konfigurieren Sie diese, bevor Sie 1-Klick-SSO ausführen.

5. Wenn 1-Klick-SSO für eine beliebige Kataloganwendung implementiert ist, sehen Sie den folgenden Bildschirm. Wenn die **Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“** noch nicht installiert ist, müssen Sie auf die Option **Die Erweiterung installieren** klicken.

    ![Installieren der Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Nachdem Sie dem Browser die Erweiterung hinzugefügt haben, klicken Sie auf **Anwendungsnamen einrichten**, wodurch Sie zum Verwaltungsportal der Anwendung umgeleitet werden. Sie müssen sich als Administrator anmelden, um in die Anwendung zu gelangen.

    ![Einrichten des Anwendungsnamens](./media/one-click-sso-tutorial/setup-sso.png)

7. Die Browsererweiterung konfiguriert nun die Anwendung automatisch für Sie. Sie bittet zuerst um Ihre Bestätigung, dass Sie fortfahren möchten. Klicken Sie auf **Ja**.

    ![Speichern der automatisch aufgefüllten Daten](./media/one-click-sso-tutorial/save-autopopulate.png)

    > [!NOTE]
    > Wenn eine Anwendung zusätzliche Navigation oder Schritte benötigt, sollten Sie entsprechende Meldungen sehen, in denen Sie zur Ausführung dieser Schritte aufgefordert werden. 

8. Klicken Sie nach Abschluss der Konfiguration auf **OK**, um die Änderungen zu speichern.

    ![Speichern der automatisch aufgefüllten Daten](./media/one-click-sso-tutorial/save-data.png)

9. Eine erfolgreiche Bestätigungsmeldung wird angezeigt, sobald Ihre SSO-Einstellungen erfolgreich konfiguriert sind. Sie können dann die Anwendung testen.

    ![SSO konfiguriert](./media/one-click-sso-tutorial/sso-configured.png)

10. Sobald die Konfiguration erfolgreich abgeschlossen ist, wird die Anwendung abgemeldet, woraufhin Sie wieder zum Azure-Portal zurückkehren.

11. Sie können auf die Schaltfläche „Testen“ klicken, um einmaliges Anmelden zu testen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [Was ist die Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“?](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 