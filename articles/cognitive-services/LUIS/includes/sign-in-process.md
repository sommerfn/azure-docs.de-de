---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 09/02/2018
ms.author: diberry
ms.openlocfilehash: 00eb6f40e77cbdb64249600e35ca8531f1a744a8
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258110"
---
## <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

Als neuer LUIS-Benutzer müssen Sie wie folgt vorgehen:

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, wählen Sie Ihr Land aus, und stimmen Sie den Nutzungsbedingungen zu.
1. Wählen Sie **Azure-Ressource erstellen** und anschließend **Create an authoring resource to migrate your apps to** (Erstellen einer Erstellungsressource für die Migration Ihrer Apps) aus.

    ![Auswählen des Typs für eine LUIS-Erstellungsressource](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Füllen Sie die Details für Ihre Ressource aus.

    ![Erstellen einer Erstellungsressource](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    Geben Sie beim **Erstellen einer neuen Erstellungsressource** die folgenden Informationen an: 

    * **Ressourcenname**: ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird
    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist 
    * **Abonnementname**: das Abonnement, unter dem die Ressource abgerechnet wird
    * **Ressourcengruppe**: ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren. 
    * **Standort**: Die Auswahl des Standorts hängt von der Auswahl der **Ressourcengruppe** ab.
    * **Tarif**: Der Tarif bestimmt die maximale Transaktion pro Sekunde und Monat.

1. Eine Zusammenfassung der zu erstellenden Ressource wird angezeigt. Klicken Sie auf **Weiter**.

    ![Erstellen einer Erstellungsressource](./media/sign-in-confirm-key-selection.png)

1. Schritt 3 ist eine Bestätigung. Bestätigen Sie die Auswahl, indem Sie **Weiter** auswählen. 

    ![Erstellen einer Erstellungsressource](./media/sign-in-confirm-continue.png)