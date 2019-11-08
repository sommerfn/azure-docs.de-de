---
title: Anzeigen des Status von Marketplace-Angeboten | Azure Marketplace
description: Anzeigen des Angebotsstatus in Azure Marketplace und AppSource über das Cloud-Partnerportal
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: pabutler
ms.openlocfilehash: e58670d37e01bb6e453b73e42a87e42e890d10d8
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73826699"
---
# <a name="view-the-publishing-status-of-azure-marketplace-and-appsource-offers"></a>Anzeigen des Veröffentlichungsstatus von Azure Marketplace- und AppSource-Angeboten

Nachdem Sie ein Angebot erstellt haben, und insbesondere während des Veröffentlichungsvorgangs können Sie den Status Ihres Angebots im Cloud-Partnerportal einsehen.  Die Übersicht aller Veröffentlichungsstatusangaben ist auf den Seiten [**Alle Angebote**](../portal-tour/cpp-all-offers-page.md) und [**Genehmigungen**](../portal-tour/cpp-approvals-page.md) des Portals verfügbar.  Für jedes Angebot sollte eine der folgenden Statusangaben angezeigt werden.  

|            Status              |   BESCHREIBUNG                                                           |
|            ------              |   -----------                                                           |
| **-**                          | Das Angebot wurde erstellt, der Veröffentlichungsvorgang aber noch nicht gestartet.            |
| **Veröffentlichung wird durchgeführt**        | Das Angebot durchläuft die Schritte des Veröffentlichungsvorgangs.   |
| **Fehler beim Veröffentlichen**             | Bei der Validierung oder Prüfung durch Microsoft wurde ein schwerwiegendes Problem entdeckt. |
| **Veröffentlichen abgebrochen**           | Der Herausgeber hat den Veröffentlichungsvorgang für das Angebot abgebrochen.  Dieser Status hebt die Listung eines vorhandenen Angebots im Marketplace nicht auf. | 
| **Bestätigung durch den Herausgeber steht aus** | Das Angebot wurde von Microsoft geprüft und muss nun noch abschließend durch den Herausgeber verifiziert werden. |
| **Listung entfernt**                   | Ein zuvor im Marketplace veröffentlichtes Angebot wurde entfernt.      | 
|  |  |


## <a name="publishing-status-details"></a>Details zum Veröffentlichungsstatus 

Weitere Informationen zum Status eines Angebots während des Veröffentlichungsvorgangs finden Sie auf der Registerkarte **Status** der Seite **Neues Angebot**.  Auf dieser Seite sind die Veröffentlichungsschritte für diesen Angebotstyp aufgelistet.  *Beachten Sie, dass sich die Anzahl und die einzelnen Schritte je nach Angebotstyp oft unterscheiden.*  Auf dieser Seite werden auch alle offenen Fragen aufgeführt, die sich aus den Validierungs- und Prüfungsschritten von Microsoft ergeben, die oft ein Eingreifen des Herausgebers erfordern, bevor der Veröffentlichungsvorgang fortgesetzt werden kann.  Das folgende Bild zeigt beispielsweise die Registerkarte **Status** für ein neues VM-Angebot. 

![Registerkarte „Status“ für VM-Angebot](./media/vm-offer-pub-steps1.png)

Im nächsten Beispiel sehen Sie die Registerkarte **Status** für einen Beratungsdienst, die einen gemeldeten Fehler in den Einstellungen für die Leadverwaltung zeigt.  Da für Beratungsdienste eine Leadverwaltung erforderlich ist, muss dieser Fehler korrigiert werden, bevor die Veröffentlichung fortgesetzt werden kann.

![Registerkarte „Status“ für Beratungsdienst, die einen Fehler anzeigt](./media/consulting-service-error.png)

Das letzte Statusbeispiel einer Azure-Anwendung zeigt ein kritisches Microsoft-Prüfungsproblem.  Es enthält einen Hotlink zu dem Azure DevOps-Element, das detaillierte Informationen zu diesem Prüfungsproblem enthält.  Informationen hierzu finden Sie unter [Veröffentlichen eines Azure-Anwendungsangebots](cpp-publish-offer.md).

![Registerkarte „Status“ für Azure-App mit Prüfungsproblem](../azure-applications/media/status-tab-ms-review.png)


## <a name="next-steps"></a>Nächste Schritte

Um ausstehende Probleme zu beheben oder die Angebotseinstellungen zu aktualisieren, müssen Sie [ein Angebot aktualisieren](./cpp-update-offer.md). 
