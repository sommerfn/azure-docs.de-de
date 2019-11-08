---
title: 'Zusammenarbeit mit anderen Benutzern: LUIS'
titleSuffix: Azure Cognitive Services
description: Ein App-Besitzer kann Mitwirkende zur Erstellungsressource hinzufügen. Diese Mitwirkenden können das Modell ändern und die App trainieren und veröffentlichen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 15c7c6664b0c17d02f1f4e9120c9509fda21e0f4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467531"
---
# <a name="add-contributors-to-your-app"></a>Hinzufügen von Mitwirkenden zu Ihrer App

Ein App-Besitzer kann Mitwirkende zu Apps hinzufügen. Diese Projektmitarbeiter können das Modell ändern und die App trainieren und veröffentlichen. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

[!INCLUDE [Manage collaborators and contributors based on migrated or not-migrated apps](./includes/manage-contributor-collaborator-migration.md)]

## <a name="add-contributor-to-azure-authoring-resource"></a>Hinzufügen von Mitwirkenden zu einer Azure-Erstellungsressource

Das folgende Verfahren gilt für alle Benutzer, die **migriert** haben, um die Azure-Erstellungsressource zu verwenden.

Sie haben migriert, wenn Ihre LUIS-Erstellungsumgebung im LUIS-Portal auf der Seite **Verwalten -> Azure-Ressourcen** an eine Erstellungsressource gebunden ist.

1. Suchen Sie im Azure-Portal nach der Language Understanding-Erstellungsressource (LUIS-Ressource). Sie hat den Typ `LUIS.Authoring`.
1. Wählen Sie auf der **Zugriffssteuerung (IAM)** -Seite der Ressource die Option **+Hinzufügen** aus, und wählen Sie dann **Rollenzuweisung hinzufügen** aus.

    ![Im Azure-Portal eine Rollenzuweisung für die Erstellungsressource hinzufügen.](./media/luis-how-to-collaborate/authoring-resource-access-control-add-role.png)

1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** für **Rolle** die Option „Mitwirkender“ aus. Wählen Sie für die Option **Zugriff zuweisen zu** den Eintrag **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus. Geben Sie für die Option **Auswählen** die E-Mail-Adresse des Benutzers ein. Ist bekannt, dass der Benutzer mehrere E-Mail-Adressen für dieselbe Domäne hat, müssen Sie das _primäre_ E-Mail-Konto eingeben.

    ![E-Mail-Adresse des Benutzers zur Rolle „Mitwirkender“ für Azure AD hinzufügen](./media/luis-how-to-collaborate/add-role-assignment-for-contributor.png)

    Wenn Sie die E-Mail-Adresse des Benutzers gefunden haben, wählen Sie das Konto aus, und wählen Sie **Speichern** aus. 

    Wenn Sie Probleme mit dieser Rollenzuweisung haben, lesen Sie [Azure-Rollenzuweisungen ](../../role-based-access-control/role-assignments-portal.md) und [Probleme mit RBAC-Rollenzuweisungen](../../role-based-access-control/troubleshooting.md#problems-with-rbac-role-assignments).

## <a name="add-collaborator-to-luis-app"></a>Hinzufügen eines Projektmitarbeiters zu einer LUIS-App

Das folgende Verfahren gilt für alle Benutzer, die **nicht migriert** haben, um die Azure-Erstellungsressource zu verwenden.

Sie haben nicht migriert, wenn Ihre LUIS-Erstellungsumgebung im LUIS-Portal auf der Seite **Verwalten -> Azure-Ressourcen** nicht an eine Erstellungsressource gebunden ist.

Eine App hat einen einzelnen Autor, den Besitzer, sie kann aber viele Mitarbeiter aufweisen. Um Projektmitarbeitern das Bearbeiten Ihrer LUIS-App zu ermöglichen, müssen Sie ihre E-Mail-Adresse, die sie für den Zugriff auf das LUIS-Portal verwenden, zur Liste der Mitwirkenden hinzufügen. Nach dem Hinzufügen wird die App in ihrem LUIS-Portal angezeigt.

1. Wählen Sie im Menü rechts oben **Verwalten** und dann im linken Menü **Projektmitarbeiter** aus.

1. Wählen Sie auf der Symbolleiste **Projektmitarbeiter hinzufügen** aus.

    [![Projektmitarbeiter hinzufügen](./media/luis-how-to-collaborate/add-collaborator.png "Mitarbeiter hinzufügen")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

1. Geben Sie die E-Mail-Adresse ein, die der Projektmitarbeiter für die Anmeldung beim LUIS-Portal verwendet.

    ![Hinzufügen der E-Mail-Adresse des Projektmitarbeiters](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)


### <a name="users-with-multiple-emails"></a>Benutzer mit mehreren E-Mail-Adressen 

Wenn Sie Mitwirkende/Projektmitarbeiter zu einer LUIS-App hinzufügen, geben Sie die genauen E-Mail-Adressen an. Während ein Einzelbenutzer in Azure Active Directory (Azure AD) mehrere E-Mail-Konten haben kann, die austauschbar verwendet werden, erfordert LUIS, dass sich der Benutzer mit der E-Mail-Adresse anmeldet, die beim Hinzufügen des Mitwirkenden/Projektmitarbeiters angegeben wurde.

<a name="owner-and-collaborators"></a>

### <a name="azure-active-directory-resources"></a>Azure Active Directory-Ressourcen

Wenn Sie in Ihrer Organisation [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (Azure AD) verwenden, benötigt LUIS (Language Understanding) die Berechtigung für Informationen über Ihre Benutzer, wenn diese LUIS verwenden möchten. Die von LUIS benötigten Ressourcen sind minimal. 

Die detaillierte Beschreibung wird angezeigt, wenn Sie sich mit einem Konto zu registrieren versuchen, für das eine Administratoreinwilligung vorliegt oder keine Administratoreinwilligung erforderlich ist:

* Ermöglicht Ihnen die Anmeldung bei der App mit Ihrem Organisationskonto und lässt die App Ihr Profil lesen. Es erlaubt der App darüber hinaus, grundlegende Firmeninformationen zu lesen. Dies gibt LUIS die Berechtigung, grundlegende Profildaten wie die Benutzer-ID, die E-Mail-Adresse und den Namen zu lesen.
* Es ermöglicht der App, Ihre Daten zu sehen und zu aktualisieren, auch wenn Sie die App aktuell nicht verwenden. Die Berechtigung ist erforderlich, um das Zugriffstoken des Benutzers zu aktualisieren.


### <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-Mandantenbenutzer

LUIS verwendet den Standard-Zustimmungsfluss von Azure Active Directory (Azure AD). 

Der Administrator des Mandanten sollte direkt mit dem Benutzer zusammenarbeiten, der den Zugriff auf LUIS in der Azure AD benötigt. 

* Im ersten Schritt meldet sich der Benutzer bei LUIS an und sieht das Popupdialogfeld, aus dem er erfährt, dass er eine Administratorgenehmigung benötigt. Der Benutzer setzt sich mit dem Administrator des Mandanten in Verbindung, bevor er fortfährt. 
* Im zweiten Schritt meldet sich der Mandantenadministrator bei LUIS und sieht ein Popupdialogfeld zum Zustimmungsfluss. Dies ist das Dialogfeld, das der Administrator benötigt, um dem Benutzer die Genehmigung zu erteilen. Nachdem der Administrator die Berechtigung akzeptiert hat, kann der Benutzer mit LUIS fortfahren. Wenn der Mandantenadministrator sich nicht bei LUIS anmeldet, kann er auf die [Zustimmung](https://account.activedirectory.windowsazure.com/r#/applications) für LUIS zugreifen, wie im folgenden Screenshot gezeigt. Beachten Sie, dass die Liste nach Elementen gefiltert ist, die den Namen `LUIS` enthalten.

![Azure Active Directory-Website mit Berechtigungen nach Anwendung](./media/luis-how-to-collaborate/tenant-permissions.png)

Wenn der Mandantenadministrator nur bestimmten Benutzern die Verwendung von LUIS gestatten möchte, gibt es mehrere mögliche Lösungen:
* Erteilen der „Administratoreinwilligung“ (Einwilligung für alle Benutzer von Azure AD) und anschließendes Festlegen von „Benutzerzuweisung erforderlich“ unter den Eigenschaften der Unternehmensanwendung auf „Ja“ und Zuweisen/Hinzufügen nur der gewünschten Benutzer zur Anwendung. Mit dieser Methode erteilt der Administrator immer noch „Administratoreinwilligung“ für die App, es ist jedoch möglich, die Benutzer zu steuern, die darauf zugreifen können.
* Eine zweite Lösung ist die Verwendung der [Azure AD Graph-API](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) zum Bereitstellen der Einwilligung für jeden einzelnen Benutzer. 

Weitere Informationen zu Azure Active Directory-Benutzern und Einwilligung: 
* [Einschränken Ihrer App](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) auf eine Gruppe von Benutzern

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Sie Versionen verwenden](luis-how-to-manage-versions.md), um den Lebenszyklus ihrer App zu steuern.
* Informieren Sie sich über die Konzepte, einschließlich der [Erstellungsressource](luis-concept-keys.md#authoring-key) und der [Mitwirkenden](luis-concept-keys.md#contributions-from-other-authors) für diese Ressource.
* Erfahren Sie, wie Sie Erstellungs- und Laufzeitressourcen [erstellen](luis-how-to-azure-subscription.md).
* Migrieren Sie zu der neuen [Erstellungsressource](luis-migration-authoring.md). 
