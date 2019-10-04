---
title: 'Tutorial: Erstellen eines Azure Active Directory B2C-Mandanten'
description: Erfahren Sie, wie Sie sich auf die Registrierung Ihrer Anwendungen vorbereiten können, indem Sie einen Azure Active Directory B2C-Mandanten über das Azure-Portal erstellen.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 57aed26fe2f2f03dca347836f21d1cad14a07642
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063345"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Erstellen eines Azure Active Directory B2C-Mandanten

Bevor Ihre Anwendungen mit Azure Active Directory B2C (Azure AD B2C) interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Im nächsten Tutorial erfahren Sie, wie Sie eine Anwendung registrieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihr Abonnement enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihr Abonnement enthält. Dabei handelt es sich um ein anderes Verzeichnis als das, in dem Ihr Azure AD B2C-Mandant enthalten sein wird.

    ![Verzeichnis- und Abonnementfilter mit ausgewähltem Abonnement-Mandanten](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Wählen Sie im Azure-Portal oben links die Option **Ressource erstellen** aus.
4. Suchen Sie nach **Active Directory B2C**, wählen Sie dies aus, und klicken Sie dann auf **Erstellen**.
5. Wählen Sie **Neuen Azure AD B2C-Mandanten erstellen** aus, und geben Sie einen Organisationsnamen und den anfänglichen Domänenname ein. Wählen Sie das Land oder die Region aus (kann später nicht geändert werden), und klicken Sie dann auf **Erstellen**.

    Der anfängliche Domänenname dient als Teil Ihres Mandantennamens. In diesem Beispiel lautet der Mandantenname *contoso0926Tenant.onmicrosoft.com*.

    ![Seite zum Erstellen des B2C-Mandanten im Azure-Portal](./media/tutorial-create-tenant/create-tenant.PNG)

6. Wählen Sie auf der Seite **Neuen B2C-Mandanten erstellen oder mit vorhandenem Mandanten verknüpfen** die Option **Vorhandenen Azure AD B2C-Mandanten mit meinem Azure-Abonnement verknüpfen** aus.

    Wählen Sie den Mandanten aus, den Sie erstellt haben, und wählen Sie dann Ihr Abonnement aus.

    Wählen Sie für die Ressourcengruppe **Neu erstellen** aus. Geben Sie einen Namen für die Ressourcengruppe ein, die den Mandanten enthalten soll, wählen Sie den Speicherort aus, und klicken Sie dann auf **Erstellen**.
1. Damit Sie Ihren neuen Mandanten verwenden können, müssen Sie sicherstellen, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie dazu im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Mandanten enthält.

    Wenn Ihr neuer Azure B2C-Mandant zunächst nicht in der Liste angezeigt wird, aktualisieren Sie Ihr Browserfenster, und wählen Sie dann erneut im oberen Menü den Filter **Verzeichnis und Abonnement** aus.

    ![Verzeichnis- und Abonnementfilter mit ausgewähltem B2C-Mandanten](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Als Nächstes erfahren Sie, wie Sie eine Webanwendung in Ihrem neuen Mandanten registrieren.

> [!div class="nextstepaction"]
> [Registrieren Ihrer Anwendungen](tutorial-register-applications.md)
