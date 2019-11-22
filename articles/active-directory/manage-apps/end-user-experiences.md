---
title: Endbenutzerumgebungen für Anwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Azure Active Directory (Azure AD) bietet mehrere anpassbare Möglichkeiten, um Anwendungen für Endbenutzer in Ihrer Organisation bereitzustellen.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4a091abff02e544f97f5621b56836cf6ba6e237
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175959"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Endbenutzerumgebungen für Anwendungen in Azure Active Directory

Azure Active Directory (Azure AD) bietet mehrere anpassbare Möglichkeiten, um Anwendungen für Endbenutzer in Ihrer Organisation bereitzustellen:

* Azure AD-Zugriffsbereich
* Office 365-Anwendungsstartprogramm
* Direkte Anmeldung bei Verbund-Apps
* Deep-Links zu verbundenen, kennwortbasierten oder vorhandene Apps

Welche Methode(n) Sie zur Bereitstellung in Ihrer Organisation wählen, liegt in Ihrem eigenen Ermessen.

## <a name="azure-ad-access-panel"></a>Azure AD-Zugriffsbereich

Der Zugriffsbereich unter https://myapps.microsoft.com ist ein webbasiertes Portal, das es einem Endbenutzer mit einem Organisationskonto in Azure Active Directory ermöglicht, cloudbasierte Anwendungen anzuzeigen und zu starten, für die der Azure AD-Administrator Zugriff gewährt hat. Als Endbenutzer mit [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/) können Sie auch die Funktionen zur Self-Service-Gruppenverwaltung über den Zugriffsbereich verwenden.

![Screenshot zeigt das Azure AD-Zugriffsbereichsportal](media/what-is-single-sign-on/azure-ad-access-panel.png)

Standardmäßig werden alle Anwendungen auf einer einzelnen Seite aufgelistet. Sie können jedoch mithilfe von Arbeitsbereichen verwandte Anwendungen gruppieren und sie auf einer separaten Registerkarte präsentieren, wodurch sie leichter auffindbar sind. So können Sie mit Arbeitsbereichen beispielsweise logische Gruppierungen von Anwendungen für bestimmte Positionen, Aufgaben, Projekte usw. erstellen. Weitere Informationen finden Sie unter [Verwenden von „Meine Apps“-Arbeitsbereichen zum Anpassen von Zugriffsbereichen für Benutzer (Vorschau)](access-panel-workspaces.md). 

Der Zugriffsbereich ist unabhängig vom Azure-Portal und setzt nicht voraus, dass Benutzer über ein Azure- oder Office 365-Abonnement verfügen.

Weitere Informationen zum Azure AD-Zugriffsbereich finden Sie in der [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Office 365-Anwendungsstartprogramm

Für Organisationen, die Office 365 bereitgestellt haben, werden Anwendungen, die Benutzern über Azure AD zugewiesen werden, auch im Office 365-Portal unter [https://portal.office.com/myapps](https://portal.office.com/myapps) angezeigt. So können Benutzer in einer Organisation auf einfache und komfortable Weise ihre Apps starten, ohne ein zweites Portal verwenden zu müssen. Diese Lösung für den App-Start wird für Unternehmen empfohlen, die Office 365 einsetzen.

![Screenshot zeigt das Office 365-Portal](./media/end-user-experiences/microsoft-365-portal-office-com.png)

Weitere Informationen zum Office 365-Anwendungsstartprogramm finden Sie unter [Anzeige Ihrer App im Office 365-Anwendungsstartprogramm](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Direkte Anmeldung bei Verbund-Apps

Die meisten Verbundanwendungen, die SAML 2.0, WS-Verbund oder OpenID Connect unterstützen, bieten die Möglichkeit, dass Benutzer die Anwendung aufrufen, und dann entweder durch automatische Umleitung oder Klicken auf einen Link zur Anmeldung über Azure AD angemeldet werden. Dies bezeichnet man als vom Dienstanbieter initiierte Anmeldung, und die meisten Verbundanwendungen im Azure AD-Anwendungskatalog unterstützen diese Form der Anmeldung (weitere Informationen finden Sie in der Dokumentation, die Sie im Azure-Portal über einen Link im Assistenten zum Konfigurieren der einmaligen Anmeldung für eine App aufrufen können).

![Beispiel für eine Anmeldeseite in einer mobilen App](./media/end-user-experiences/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Links für die direkte Anmeldung

Azure AD unterstützt auch Direktlinks für die einmalige Anmeldung bei einzelnen Anwendungen, die eine kennwortbasierte einmalige Anmeldung, einmaliges Anmelden über Link und eine beliebige Art der einmaligen Verbundanmeldung unterstützen.

Diese Links sind speziell gestaltete URLs, die einen Benutzer durch den Azure AD-Anmeldevorgang für eine bestimmte Anwendung leiten, ohne dass der Benutzer die Anwendung aus dem Azure AD-Zugriffsbereich oder Office 365 starten muss. Diese **Benutzer-Zugriffs-URLs** befinden sich unter den Eigenschaften der verfügbaren Unternehmensanwendungen. Wählen Sie im Azure-Portal **Azure Active Directory** > **Unternehmens-Apps** aus. Wählen Sie die Anwendung und dann **Eigenschaften** aus.

![Beispiel für die Benutzerzugriffs-URL in den Twitter-Eigenschaften](media/end-user-experiences/direct-sign-on-link.png)

Diese Links können Sie kopieren und überall dort einfügen, wo Sie einen Anmeldelink zur ausgewählten Anwendung bereitstellen möchten. Dies kann eine E-Mail oder ein beliebiges benutzerdefiniertes webbasiertes Portal sein, das Sie für den Benutzerzugriff auf eine Anwendung eingerichtet haben. Hier sehen Sie ein Beispiel für eine Azure AD-Direkt-URL zur einmaligen Anmeldung für Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Ähnlich wie organisationsspezifische URLs für den Zugriffsbereich können Sie diese URL durch Hinzufügen der aktiven oder überprüften Domänen für Ihr Verzeichnis nach der Domäne "myapps.microsoft.com" weiter anpassen. So wird sichergestellt, dass jegliches Organisationsbranding sofort auf der Anmeldeseite geladen wird, ohne dass Benutzer vorher ihre Benutzer-ID eingeben müssen:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Wenn autorisierte Benutzer auf einen dieser anwendungsspezifischen Links klicken, sehen sie zuerst die Anmeldeseite der Organisation (vorausgesetzt, dass sie noch nicht angemeldet sind), und nach dem Anmelden werden sie ohne Zwischenschritt über den Zugriffsbereich zu ihrer App umgeleitet. Wenn Voraussetzungen für den Zugriff auf die Anwendung nicht erfüllt sind, wenn z. B. die kennwortbasierte Browsererweiterung zur einmaligen Anmeldung fehlt, wird der Benutzer über den Link zur Installation der fehlenden Erweiterung aufgefordert. Die Link-URL bleibt auch dann unverändert, wenn die Konfiguration für die einmalige Anmeldung geändert wird.

Diese Links verwenden die gleichen Mechanismen der Zugriffssteuerung wie der Zugriffsbereich und Office 365, und nur die Benutzer oder Gruppen, die der Anwendung im Azure-Portal zugewiesen wurden, können sich authentifizieren. Alle nicht autorisierten Benutzer werden in einer Meldung darüber informiert, dass ihnen kein Zugriff gewährt wurde. Anschließend wird ein Link zum Laden des Zugriffsbereichs angezeigt, um verfügbare Anwendungen anzuzeigen, auf die zugegriffen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Bereitstellungsplänen finden Sie unter [Azure Active Directory-Bereitstellungspläne](../fundamentals/active-directory-deployment-plans.md).
