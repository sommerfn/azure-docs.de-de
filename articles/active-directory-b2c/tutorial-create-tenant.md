---
title: 'Tutorial: Erstellen eines Azure Active Directory B2C-Mandanten'
description: Erfahren Sie, wie Sie sich auf die Registrierung Ihrer Anwendungen vorbereiten können, indem Sie einen Azure Active Directory B2C-Mandanten über das Azure-Portal erstellen.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: de29929c8fda476fe276f91d4a68ce4d632503fd
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345216"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Erstellen eines Azure Active Directory B2C-Mandanten

Bevor Ihre Anwendungen mit Azure Active Directory B2C (Azure AD B2C) interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement
> * Wechseln zu dem Verzeichnis, das Ihren Azure AD B2C-Mandanten enthält
> * Hinzufügen der Azure AD B2C-Ressource als *Favorit* im Azure-Portal

Im nächsten Tutorial erfahren Sie, wie Sie eine Anwendung registrieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihr Abonnement enthält.

    Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** und dann das Verzeichnis mit Ihrem Abonnement aus. Dabei handelt es sich um ein anderes Verzeichnis als das, in dem Ihr Azure AD B2C-Mandant enthalten sein wird.

    ![Filter „Verzeichnis und Abonnement“ mit ausgewähltem Abonnementmandanten](media/tutorial-create-tenant/portal-01-select-directory.png)

1. Wählen Sie im Azure-Portal oben links **Ressource erstellen** aus.
1. Suchen Sie nach **Active Directory B2C**, und wählen Sie es aus. Wählen Sie dann **Erstellen** aus.
1. Wählen Sie **Neuen Azure AD B2C-Mandanten erstellen**.

    ![„Neuen Azure AD B2C-Mandanten erstellen“ im Azure-Portal ausgewählt](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. Geben Sie die gewünschten Namen in **Organisationsname** und **Name der Anfangsdomäne** ein. Wählen Sie einen Eintrag in **Land oder Region** aus (diese Auswahl kann später nicht geändert werden), und wählen Sie dann **Erstellen** aus.

    Der Domänenname wird als Teil Ihres vollständigen Mandantendomänennamens verwendet. In diesem Beispiel lautet der Mandantenname *contosob2c.onmicrosoft.com*:

    ![Formular „Mandanten erstellen“ mit Beispielwerten im Azure-Portal](media/tutorial-create-tenant/portal-03-tenant-naming.png)

1. Wählen Sie nach Abschluss der Mandantenerstellung oben auf der Seite für die Mandantenerstellung den Link **Neuen B2C-Mandanten erstellen oder mit vorhandenem Mandanten verknüpfen** aus.

    ![Breadcrumb-Link zum Verknüpfen des Mandanten im Azure-Portal hervorgehoben](media/tutorial-create-tenant/portal-04-select-link-sub-link.png)

1. Wählen Sie **Vorhandenen Azure AD B2C Mandanten mit meinem Azure-Abonnement verknüpfen** aus.

   ![Auswahl von „Mit vorhandenem Abonnement verknüpfen“ im Azure-Portal](media/tutorial-create-tenant/portal-05-link-subscription.png)

1. Wählen Sie den erstellten **Azure AD B2C-Mandanten** und dann Ihr **Abonnement** aus.

    Wählen Sie für **Ressourcengruppe** die Option **Neu erstellen** aus. Geben Sie einen **Namen** für die Ressourcengruppe ein, die den Mandanten enthalten soll, wählen Sie den **Ressourcengruppenstandort** und dann **Erstellen** aus.

    ![Formular zum Verknüpfen von Abonnementeinstellungen im Azure-Portal](media/tutorial-create-tenant/portal-06-link-subscription-settings.png)

## <a name="select-your-b2c-tenant-directory"></a>Auswählen Ihres B2C-Mandantenverzeichnisses

Wenn Sie mit der Verwendung Ihres neuen Azure AD B2C-Mandanten beginnen möchten, müssen Sie zu dem Verzeichnis wechseln, das den Mandanten enthält.

Wählen Sie im oberen Menü des Azure-Portals den Filter **Verzeichnis und Abonnement** und dann das Verzeichnis mit Ihrem Azure AD B2C-Mandanten aus.

Wenn Ihr neuer Azure B2C-Mandant zunächst nicht in der Liste angezeigt wird, aktualisieren Sie Ihr Browserfenster, und wählen Sie dann erneut im oberen Menü den Filter **Verzeichnis und Abonnement** aus.

![B2C-Mandant mit Verzeichnis im Azure-Portal ausgewählt](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Hinzufügen von Azure AD B2C als Favorit (optional)

Dieser optionale Schritt erleichtert Ihnen die Auswahl Ihres Azure AD B2C-Mandanten in den folgenden Abschnitten und allen nachfolgenden Tutorials.

Statt jedes Mal, wenn Sie mit Ihrem Mandanten arbeiten möchten, in **Alle Dienste** nach „Azure AD B2C“ suchen zu müssen, können Sie die Ressource als Favorit markieren. Anschließend können Sie sie im Menü **Favoriten** auf der linken Seite auswählen, um schnell zu Ihrem Azure AD B2C-Mandanten zu navigieren.

Sie müssen diesen Vorgang nur einmal ausführen. Bevor Sie diese Schritte ausführen, müssen Sie in das Verzeichnis mit Ihrem Azure AD B2C-Mandanten gewechselt haben, wie im vorherigen Abschnitt, [Auswählen Ihres B2C-Mandantenverzeichnisses](#select-your-b2c-tenant-directory), beschrieben wurde.

1. Wählen Sie im linken Menü des [Azure-Portals](https://portal.azure.com) **Alle Dienste** aus.
1. Geben Sie im Suchtextfeld *Azure AD B2C* ein.
1. Wählen Sie den **Stern** aus, um „Azure AD B2C“ Ihren Favoriten hinzuzufügen.
1. Im linken Menü *Favoriten* wird jetzt **Azure AD B2C** angezeigt. Bei Bedarf können Sie diesen Eintrag auswählen und in der Liste nach oben ziehen, wie die folgende Abbildung zeigt:

![Schritte zum Hinzufügen von Azure AD B2C als Favorit im Azure-Portal](media/tutorial-create-tenant/portal-08-favorite-b2c.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement
> * Wechseln zu dem Verzeichnis, das Ihren Azure AD B2C-Mandanten enthält
> * Hinzufügen der Azure AD B2C-Ressource als *Favorit* im Azure-Portal

Als Nächstes erfahren Sie, wie Sie eine Webanwendung in Ihrem neuen Mandanten registrieren.

> [!div class="nextstepaction"]
> [Registrieren Ihrer Anwendungen](tutorial-register-applications.md)
