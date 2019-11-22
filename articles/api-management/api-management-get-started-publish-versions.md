---
title: Veröffentlichen von Versionen Ihrer API mithilfe von Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie in API Management mehrere Versionen veröffentlichen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: d0c837ac376f9d496baaf1ed67d0ec80c996804a
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176823"
---
# <a name="publish-multiple-versions-of-your-api"></a>Veröffentlichen mehrerer Versionen Ihrer API 

Manchmal ist es unpraktisch, wenn alle Aufrufer genau die gleiche Version Ihrer API verwenden müssen. Wenn Aufrufer auf eine höhere Version upgraden möchten, möchten Sie dafür einen leicht verständlichen Ansatz verwenden. Dies ist mit **Versionen** in Azure API Management möglich. Weitere Informationen finden Sie unter [Versions & revisions](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) (Versionen und Revisionen).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Version zu einer vorhandenen API
> * Auswählen eines Versionsschemas
> * Hinzufügen der Version zu einem Produkt
> * Anzeigen der Version im Entwicklerportal

![Angezeigte Version im Entwicklerportal](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ Absolvieren Sie außerdem das folgende Tutorial: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

## <a name="add-a-new-version"></a>Hinzufügen einer neuen Version

![API-Kontextmenü – Version hinzufügen](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Wählen Sie in der API-Liste **Demo Conference API** aus.
2. Klicken Sie auf das Kontextmenü ( **...** ) daneben.
3. Klicken Sie auf **+ Version hinzufügen**.

> [!TIP]
> Versionen können auch beim Erstellen einer neuen API aktiviert werden. Wählen Sie hierzu auf dem Bildschirm **API hinzufügen** die Option **Versionsverwaltung für diese API?** aus.

## <a name="choose-a-versioning-scheme"></a>Auswählen eines Schemas für die Versionsverwaltung

Mit Azure API Management können Sie auswählen, wie Aufrufer angeben können, welche Version Ihrer API sie verwenden möchten. Zur Angabe der zu verwendenden API-Version wählen Sie ein **Schema für die Versionsverwaltung** aus. Mögliche Optionen für dieses Schema sind **Pfad, Header oder Abfragezeichenfolge**. Im folgenden Beispiel wird zur Wahl des Schemas für die Versionsverwaltung die Pfadoption verwendet.

![Bildschirm zum Hinzufügen von Versionen](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Lassen Sie **Pfad** als **Schema für Versionsverwaltung** ausgewählt.
2. Geben Sie **demo-conference-api-v1** in das Feld **Name** ein.

    > [!NOTE]
    > Die Version ist tatsächlich eine neue API, die auf der API-Revision basiert. **Name** ist der Name der neuen API. Er muss in der gesamten API Management-Instanz eindeutig sein.

3. Geben Sie **v1** in das Feld **Versionsbezeichner** ein.

    > [!TIP]
    > Wenn Sie **Header** oder **Abfragezeichenfolge** als Schema für die Versionsverwaltung auswählen, müssen Sie einem zusätzlichen Wert (den Namen des Headers oder Abfragezeichenfolgen-Parameters) angeben.

4. Klicken Sie auf **Erstellen**, um Ihre neue Version einzurichten.
5. In der API-Liste werden unter **Demo Conference API** nun zwei unterschiedliche APIs angezeigt: **Original** und **v1**.

    ![Unter einer API aufgeführte Versionen im Azure-Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Wenn Sie einer API ohne Versionsverwaltung eine Version hinzufügen, wird automatisch ein **Original** erstellt, das auf die Standard-URL reagiert. Dadurch wird sichergestellt, dass bereits vorhandene Aufrufer durch das Hinzufügen einer Version nicht beeinträchtigt werden. Wenn Sie Versionen gleich bei der Erstellung einer neuen API aktivieren, wird kein Original erstellt.

6. Nun können Sie **v1** als eine API bearbeiten und konfigurieren, die vom **Original** getrennt ist. Änderungen an einer Version haben keine Auswirkungen auf die andere.

## <a name="add-the-version-to-a-product"></a>Hinzufügen der Version zu einem Produkt

Damit Aufrufern die neue Version angezeigt wird, muss sie einem **Produkt** hinzugefügt werden.

![API Management-Produkte](media/api-management-getstarted-publish-versions/08-AddMultipleVersions-03-AddVersionToProduct.png)

1. Klicken Sie auf der Seite des klassischen Bereitstellungsmodells auf **Produkte**.
2. Klicken Sie auf **Unbegrenzt**.
3. Klicken Sie auf **APIs**.
4. Wählen Sie **Hinzufügen**.
5. Wählen Sie **Demo Conference API, Version v1** aus.
6. Klicken Sie auf **Auswählen**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Anzeigen der Version im Entwicklerportal

1. Klicken Sie im oberen Menü auf **Entwicklerportal**.
2. Wählen Sie **APIs** aus, und klicken Sie auf die **Demo Conference API**.
3. Ein Dropdownmenü mit mehreren Versionen neben dem API-Namen sollte angezeigt werden.
4. Klicken Sie auf **v1**.
5. Beachten Sie die **Anforderungs-URL** des ersten Vorgangs in der Liste. Dort sehen Sie, dass der API-URL-Pfad **v1** enthält.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Version zu einer vorhandenen API
> * Auswählen eines Versionsschemas 
> * Hinzufügen der Version zu einem Produkt
> * Anzeigen der Version im Entwicklerportal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Anpassen des Stils der Seiten im Entwicklerportal](api-management-customize-styles.md)