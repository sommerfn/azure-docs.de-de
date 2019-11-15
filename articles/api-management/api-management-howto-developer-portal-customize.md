---
title: Zugreifen auf das verwaltete Entwicklerportal und Anpassen des Portals – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die verwaltete Version des Entwicklerportals in API Management verwenden.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 7547db502056304307094541dcfd2474eda999d8
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73620435"
---
# <a name="access-and-customize-developer-portal"></a>Zugreifen auf das Entwicklerportal und Anpassen dieses Portals

Das Entwicklerportal ist eine automatisch generierte, vollständig anpassbare Website mit der Dokumentation Ihrer APIs. Dort können API-Consumer Ihre APIs entdecken, erfahren, wie diese verwendet werden, und Zugriff anfordern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Zugreifen auf die verwaltete Version des Entwicklerportals
> * Navigieren durch die Administratoroberfläche
> * Anpassen des Inhalts
> * Veröffentlichen der Änderungen
> * Anzeigen des veröffentlichten Portals

Weitere Informationen zum Entwicklerportal finden Sie in der [Übersicht über das Azure API Management-Entwicklerportal](api-management-howto-developer-portal.md).

![API Management-Entwicklerportal: Administratormodus](media/api-management-howto-developer-portal-customize/cover.png)

> [!WARNING]
> Das Entwicklerportal wird zurzeit in den API Management-Diensten bereitgestellt.
> Wenn Ihr Dienst neu erstellt wurde oder es sich um einen Dienst der Entwicklerebene handelt, sollten Sie bereits über die neueste Version verfügen. Andernfalls können Probleme auftreten (z. B. mit der Veröffentlichungsfunktion). Das Featurerollout wird am Montag, den 11. November 2019, abgeschlossen sein. 

## <a name="prerequisites"></a>Voraussetzungen

- Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Importieren und veröffentlichen Sie eine Azure API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-the-portal-as-an-administrator"></a>Zugreifen auf das Portal als Administrator

Führen Sie die folgenden Schritte aus, um auf die verwaltete Version des Portals zuzugreifen.

1. Wechseln Sie im Azure-Portal zu Ihrer API Management-Dienstinstanz.
1. Klicken Sie in der oberen Navigationsleiste auf die Schaltfläche **Entwicklerportal**. Eine neue Browserregisterkarte mit einer administrativen Version des Portals wird geöffnet.

## <a name="understand-the-portals-administrative-interface"></a>Grundlegendes zur Administratoroberfläche des Portals

### <a name="default-content"></a>Standardinhalt 

Wenn Sie zum ersten Mal auf das Portal zugreifen, wird der Standardinhalt automatisch im Hintergrund bereitgestellt. Der Standardinhalt ist dafür konzipiert, die Funktionen des Portals zu zeigen und den Umfang der Anpassungen zu minimieren, die zur Personalisierung des Portals erforderlich sind. Weitere Informationen dazu, was im Portal enthalten ist, finden Sie in der [Übersicht über das Azure API Management-Entwicklerportal](api-management-howto-developer-portal.md).

### <a name="visual-editor"></a>Visueller Editor

Sie können den Inhalt des Portals mithilfe des visuellen Editors anpassen. Über die Menüabschnitte auf der linken Seite können Sie Seiten, Medien, Layouts, Menüs, Formatvorlagen oder Websiteeinstellungen erstellen und ändern. Mit den Menüelementen im unteren Bereich können Sie zwischen Viewports wechseln (z. B. mobil und Desktop), die Elemente des Portals anzeigen, die für authentifizierte oder anonyme Benutzer sichtbar sind, und Aktionen speichern oder rückgängig machen.

Sie können einer Seite Zeilen hinzufügen, indem Sie auf ein blaues Symbol mit einem Pluszeichen klicken. Widgets (z. B. Text, Bilder oder API-Listen) können durch Klicken auf ein graues Symbol mit Pluszeichen hinzugefügt werden. Sie können die Elemente auf einer Seite per Drag & Drop neu anordnen. 

### <a name="layouts-and-pages"></a>Layouts und Seiten

![Seiten und Layouts](media/api-management-howto-developer-portal-customize/pages-layouts.png)

Layouts definieren, wie Seiten angezeigt werden. Im Standardinhalt gibt es beispielsweise zwei Layouts: eines für die Startseite, das andere für alle anderen Seiten.

Ein Layout wird auf eine Seite angewendet, indem die zugehörige URL-Vorlage mit der URL der Seite abgeglichen wird. Beispielsweise wird das Layout mit einer URL-Vorlage vom Typ `/wiki/*` auf jede Seite mit dem Segment `/wiki/` angewendet: `/wiki/getting-started`, `/wiki/styles` usw.

In der obigen Abbildung ist zum Layout gehöriger Inhalt blau markiert, die Seite selbst ist rot umrandet. Die Menüabschnitte sind entsprechend gekennzeichnet.

### <a name="styling-guide"></a>Gestaltungsvorlagen

![Gestaltungsvorlagen](media/api-management-howto-developer-portal-customize/styling-guide.png)

Gestaltungsvorlagen sind ein Panel, das speziell für Designer erstellt wurde. Hier können alle visuellen Elemente in Ihrem Portal angezeigt und gestaltet werden. Die Gestaltung erfolgt hierarchisch: Viele Elemente erben Eigenschaften von anderen Elementen. Ein Beispiel: Schaltflächenelemente verwenden Farben für Text und Hintergrund. Um die Farbe einer Schaltfläche zu ändern, müssen Sie die ursprüngliche Farbvariante ändern.

Um eine Variante zu bearbeiten, klicken Sie darauf, und wählen Sie das angezeigte Stiftsymbol aus. Wenn Sie die gewünschten Änderungen im Popupfenster vorgenommen haben, schließen Sie das Fenster.

### <a name="save-button"></a>Schaltfläche „Speichern“

![Schaltfläche „Speichern“](media/api-management-howto-developer-portal-customize/save-button.png)

Wenn Sie eine Änderung im Portal vorgenommen haben, müssen Sie diese manuell speichern, indem Sie im Menü unten auf die Schaltfläche **Speichern** klicken. Nachdem Sie die Änderungen gespeichert haben, wird der geänderte Inhalt automatisch in Ihren API Management-Dienst hochgeladen.

## <a name="customize-the-portals-content"></a>Anpassen des Portalinhalts

Bevor Sie Ihr Portal Besuchern zur Verfügung stellen, sollten Sie den automatisch generierten Inhalt personalisieren. Empfohlen werden Änderungen an Layouts, Formatvorlagen und dem Inhalt der Startseite.

> [!NOTE]
> Aufgrund von Überlegungen in Bezug auf die Integration können die folgenden Seiten nicht entfernt oder zu einer anderen URL verschoben werden: `/404`, `/500`, `/captcha`, `/change-password`, `/config.json`, `/confirm/invitation`, `/confirm-v2/identities/basic/signup`, `/confirm-v2/password`, `/internal-status-0123456789abcdef`, `/publish`, `/signin`, `/signin-sso`, `/signup`.

### <a name="home-page"></a>Startseite

Die standardmäßige **Startseite** ist mit Dummyinhalt gefüllt. Sie können entweder ganze Abschnitte mitsamt Inhalt entfernen oder die Struktur beibehalten und die Elemente einzeln anpassen. Ersetzen Sie die generierten Texte und Bilder durch eigene, und stellen Sie sicher, dass die Links auf die gewünschte Speicherorte zeigen.

### <a name="layouts"></a>Layouts

Ersetzen Sie das automatisch generierte Logo in der Navigationsleiste durch ein eigenes Bild.

### <a name="styling"></a>Gestaltung

Sie müssen zwar keine Formatvorlagen ändern, aber möglicherweise möchten Sie bestimmte Elemente anpassen. Ändern Sie beispielsweise die primäre Farbe in die Farbe Ihrer Marke.

### <a name="customization-example"></a>Anpassungsbeispiel

In dem folgenden Video demonstrieren wir, wie Sie den Inhalt des Portals bearbeiten, das Aussehen der Website anpassen und die Änderungen veröffentlichen.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="publish-the-portal"></a>Veröffentlichen des Portals

Um Ihr Portal mit den neuesten Änderungen Besuchern zur Verfügung zu stellen, müssen Sie es veröffentlichen.

1. Stellen Sie sicher, dass Ihre Änderungen gespeichert wurden, indem Sie auf das Symbol **Speichern** klicken.
1. Klicken Sie im Menüabschnitt **Vorgänge** auf **Website veröffentlichen**. Dieser Vorgang kann einige Minuten dauern.  
    ![Portal veröffentlichen](media/api-management-howto-developer-portal-customize/publish-portal.png)

## <a name="visit-the-published-portal"></a>Besuchen des veröffentlichten Portals

Nachdem Sie das Portal veröffentlicht haben, können Sie über die gleiche URL wie für das Verwaltungspanel darauf zugreifen, z. B. über `https://contoso-api.developer.azure-api.net`. Sie können es in einer separaten Browsersitzung (inkognito/privater Modus) als externer Besucher anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwicklerportal:

- [Übersicht über das Azure API Management-Entwicklerportal](api-management-howto-developer-portal.md)
