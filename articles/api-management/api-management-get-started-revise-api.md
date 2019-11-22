---
title: Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen in Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie mithilfe von Revisionen geringfügige Änderungen in API Management vornehmen.
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
ms.openlocfilehash: 811591a3340f07b08b3a954ccd475e926b73a9cf
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176773"
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen
Wenn Ihre API einsatzbereit ist und von Entwicklern verwendet wird, müssen Sie letztendlich Änderungen an der API vornehmen und dabei darauf achten, keine Aufrufe Ihrer API zu beeinträchtigen. Außerdem empfiehlt es sich, Entwickler über die vorgenommenen Änderungen zu informieren. Hierzu können Sie **Revisionen** in Azure API Management verwenden. Weitere Informationen finden Sie unter [Versions & revisions](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) (Versionen und Revisionen) und [API Versioning with Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/) (API-Versionsverwaltung mit Azure API Management).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Revision
> * Vornehmen geringfügiger Änderungen an Ihrer Revision
> * Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
> * Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal

![Änderungsprotokoll im Entwicklerportal](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ Absolvieren Sie außerdem das folgende Tutorial: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

## <a name="add-a-new-revision"></a>Hinzufügen einer neuen Revision

![Hinzufügen der API-Revision](media/api-management-getstarted-revise-api/07-AddRevisions-01-AddNewRevision.png)

1. Klicken Sie auf die Seite **APIs**.
2. Wählen Sie in der API-Liste den Eintrag **Demo Conference API** aus (oder eine andere API, der Sie Revisionen hinzufügen möchten).
3. Klicken Sie im Menü im oberen Bereich der Seite auf die Registerkarte **Revisionen**.
4. Klicken Sie auf **+ Revision hinzufügen**.

    > [!TIP]
    > Alternativ können Sie im Kontextmenü ( **...** ) für die API auf **Revision hinzufügen** klicken.

5. Geben Sie eine Beschreibung für Ihre neue Revision an, damit Sie wissen, wofür sie verwendet wird.
6. Klicken Sie auf **Erstellen**
7. Die neue Revision wird erstellt.

    > [!NOTE]
    > Ihre ursprüngliche API bleibt in **Revision 1**. Dies ist die Revision, die weiterhin von den Benutzern aufgerufen wird, bis Sie beschließen, eine andere Revision zur aktuellen Revision zu machen.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vornehmen geringfügiger Änderungen an Ihrer Revision

![Ändern der Revision](media/api-management-getstarted-revise-api/07-AddRevisions-02-MakeChanges.png)

1. Wählen Sie in der API-Liste **Demo Conference API** aus.
2. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
3. Beachten Sie, dass die **Revisionsauswahl** (direkt über der Registerkarte „Entwurf“) **Revision 2** als derzeit ausgewählt anzeigt.

    > [!TIP]
    > Mit der Revisionsauswahl können Sie zwischen Revisionen wechseln, die Sie bearbeiten möchten.

4. Klicken Sie auf **+ Vorgang hinzufügen**.
5. Legen Sie den neuen Vorgang auf **POST** und Name, Anzeigename und URL des Vorgangs auf **test** fest.
6. **Speichern** Sie Ihren neuen Vorgang.
7. Sie haben nun eine Änderung an **Revision 2** vorgenommen. Wechseln Sie mithilfe der **Revisionsauswahl** im oberen Seitenbereich zu **Revision 1** zurück.
8. Wie Sie sehen, ist der neue Vorgang in **Revision 1** nicht vorhanden. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll

1. Klicken Sie im oberen Bereich der Seite auf die Registerkarte **Revisionen**.

    ![Das Revisionsmenü auf dem Revisionsbildschirm](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)

2. Öffnen Sie das Kontextmenü ( **...** ) für **Revision 2**.
3. Klicken Sie auf **Make Current** (Als aktuell festlegen).
4. Aktivieren Sie **Post to Public Change log for this API** (In öffentlichem Änderungsprotokoll für diese API veröffentlichen), wenn Sie Hinweise zu dieser Änderung veröffentlichen möchten. Geben Sie eine Änderungsbeschreibung für Entwickler an, beispielsweise **Testrevisionen. Neuer Vorgang „Test“ hinzugefügt**).
5. **Revision 2** ist jetzt aktuell.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal

1. Klicken Sie im Azure-Portal auf **APIs**.
2. Klicken Sie im oberen Menü auf **Entwicklerportal**.
3. Klicken Sie auf **APIs** und anschließend auf **Demo Conference API**.
4. Wie Sie sehen, ist Ihr neuer **test**-Vorgang nun verfügbar.
5. Klicken Sie beim API-Namen auf **Änderungsprotokoll**.
6. Der Änderungsprotokolleintrag ist in der Liste enthalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen einer neuen Revision
> * Vornehmen geringfügiger Änderungen an Ihrer Revision
> * Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
> * Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Planbares Veröffentlichen mehrerer Versionen Ihrer API](api-management-get-started-publish-versions.md)
