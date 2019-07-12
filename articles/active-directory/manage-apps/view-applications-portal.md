---
title: Anzeigen von Mandantenanwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Verwenden Sie das Azure-Portal, um die Anwendungen in Ihrem Azure Active Directory-Mandanten (Azure AD) anzuzeigen.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/09/2019
ms.author: mimart
ms.reviewer: arvinh
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cccd5ddc2f268eda48bd9803596a1db1c5ee9395
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481175"
---
# <a name="view-your-azure-active-directory-tenant-applications"></a>Anzeigen Ihrer Azure Active Directory-Mandantenanwendungen

In dieser Schnellstartanleitung wird das Azure-Portal verwendet, um die Anwendungen in Ihrem Azure Active Directory-Mandanten (Azure AD) anzuzeigen.

## <a name="before-you-begin"></a>Voraussetzungen

Um Ergebnisse anzeigen zu können, muss mindestens eine Anwendung in Ihrem Azure AD-Mandanten vorhanden sein. Informationen zum Hinzufügen einer Anwendung finden Sie in der Schnellstartanleitung zum [Hinzufügen einer Anwendung](add-application-portal.md).

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator für Ihren Azure AD-Mandanten, als Cloudanwendungsadministrator oder als Anwendungsadministrator an.

## <a name="find-the-list-of-tenant-applications"></a>Suchen der Liste der Mandantenanwendungen

Ihre Azure AD-Mandantenanwendungen können im Azure-Portal im Abschnitt **Unternehmens-Apps** angezeigt werden.

So suchen Sie nach Ihren Mandantenanwendungen:

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus. 

1. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus. 

1. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Anwenden** aus. Eine nach dem Zufallsprinzip ausgewählte Gruppe Ihrer Mandantenanwendungen wird angezeigt.
   
1. Wenn Sie weitere Anwendungen anzeigen möchten, wählen Sie unten in der Liste **Mehr anzeigen** aus. Abhängig von der Anzahl von Anwendungen in Ihrem Mandanten ist es unter Umständen einfacher, [nach einer bestimmten Anwendung zu suchen](#search-for-a-tenant-application) als durch die Liste zu scrollen.

## <a name="select-viewing-options"></a>Auswählen der Anzeigeoptionen

Wählen Sie entsprechende Optionen für die gesuchten Elemente aus.

1. Sie können die Anwendungen nach **Anwendungstyp**, **Anwendungsstatus** und **Anwendungssichtbarkeit** anzeigen. 

1. Wählen Sie unter **Anwendungstyp** eine der folgenden Optionen aus:

    - Mit der Option **Unternehmensanwendungen** werden nicht von Microsoft stammende Anwendungen angezeigt.
    - Mit der Option **Microsoft-Anwendungen** werden Microsoft-Anwendungen angezeigt.
    - Mit der Option **Alle Anwendungen** werden nicht von Microsoft stammende Anwendungen und Microsoft-Anwendungen angezeigt.

1. Wählen Sie unter **Anwendungsstatus** die Option **Any** (Alle), **Deaktiviert** oder **Aktiviert** aus. Mit der Option **Any** (Alle) werden sowohl deaktivierte als auch aktivierte Anwendungen angezeigt.

1. Wählen Sie unter **Anwendungssichtbarkeit** die Option **Any** (Alle) oder **Ausgeblendet**. Mit der Option **Ausgeblendet** werden Anwendungen angezeigt, die sich zwar im Mandanten befinden, aber nicht für Benutzer sichtbar sind.

1. Klicken Sie nach dem Auswählen der gewünschten Optionen auf **Anwenden**.

## <a name="search-for-a-tenant-application"></a>Suchen nach einer Mandantenanwendung

So suchen Sie nach einer bestimmten Anwendung:

1. Wählen Sie im Menü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Anwenden** aus.

1. Geben Sie den Namen der gewünschten Anwendung ein. Wenn die Anwendung zu Ihrem Azure AD-Mandanten hinzugefügt wurde, wird sie in den Suchergebnissen angezeigt. Dieses Beispiel zeigt, dass GitHub nicht zu den Mandantenanwendungen hinzugefügt wurde.

    ![Beispiel zeigt, dass eine App nicht zum Mandanten hinzugefügt wurde](media/view-applications-portal/search-for-tenant-application.png)

1. Geben Sie die ersten Buchstaben eines Anwendungsnamens ein. Dieses Beispiel zeigt alle Anwendungen, die mit **Sales** beginnen.

    ![Beispiel zeigt alle Apps, die mit Sales beginnen](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie die Anwendungen in Ihrem Azure AD-Mandanten angezeigt werden. Sie haben gelernt, wie Sie die Liste der Anwendungen nach Anwendungstyp, -status und -sichtbarkeit filtern. Darüber hinaus haben Sie gelernt, wie Sie nach einer bestimmten Anwendung suchen.

Sie haben die gewünschte Anwendung gefunden und können nun [weitere Anwendungen zu Ihrem Mandanten hinzufügen](add-application-portal.md). Alternativ können Sie die Anwendung auswählen, um Eigenschaften und Konfigurationsoptionen anzuzeigen oder zu bearbeiten. Beispielsweise können Sie einmaliges Anmelden konfigurieren.

> [!div class="nextstepaction"]
> [Konfigurieren von einmaligem Anmelden](configure-single-sign-on-portal.md)
