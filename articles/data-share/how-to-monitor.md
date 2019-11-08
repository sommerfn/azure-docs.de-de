---
title: Überwachen von Azure Data Share
description: In diesem Artikel erfahren Sie, wie Sie den Status der Einladung, Freigabeabonnements und den Momentaufnahmeverlauf in Azure Data Share überwachen.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 15089dd99de0471c244a6c0d93931438442599a7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490503"
---
# <a name="monitor-azure-data-share"></a>Überwachen von Azure Data Share  

In diesem Artikel wird erläutert, wie Sie Ihre Datenfreigaben mit Azure Data Share überwachen können. Als Datenanbieter können Sie verschiedene Aspekte Ihrer Datenfreigabebeziehungen überwachen. Details wie Antworten auf die Fragen, ob Ihre Datenconsumer Ihre Einladung für die Datenfreigabe angenommen haben, ob sie ein Freigabeabonnement erstellt und mit der Nutzung Ihrer Daten begonnen haben, stehen für die Überwachung zur Verfügung. 

Als Datenconsumer können Sie die Momentaufnahmen überwachen, die in Ihrem Azure-Abonnement ausgelöst wurden. 

## <a name="monitor-invitation-status"></a>Überwachen des Status der Einladung

Zeigen Sie den Status Ihrer Einladungen zur Datenfreigabe an, indem Sie zu „Gesendete Freigaben > Einladungen“ navigieren. 

![Einladungsstatus](./media/invitation-status.png "Einladungsstatus") 

Es gibt drei Zustände, in denen sich Ihre Einladung befinden kann:

* Ausstehend: Der Empfänger der Datenfreigabe hat die Einladung noch nicht angenommen.
* Angenommen: Der Empfänger der Datenfreigabe hat die Einladung angenommen.
* Abgelehnt: Der Empfänger der Datenfreigabe hat die Einladung abgelehnt.

> [!IMPORTANT]
> Wenn Sie eine Einladung löschen, nachdem sie bereits angenommen wurde, entspricht dies nicht dem Widerrufen des Zugriffs. Wenn Sie verhindern möchten, dass zukünftige Momentaufnahmen in das Speicherkonto des Datenconsumers kopiert werden, müssen Sie den Zugriff über die Registerkarte *Freigabeabonnements* widerrufen. 

## <a name="monitor-share-subscriptions"></a>Überwachen von Freigabeabonnements

Zeigen Sie den Status Ihrer Freigabeabonnements an, indem Sie zu „Gesendete Freigaben > Freigabeabonnements navigieren. Auf diese Weise erhalten Sie Informationen zu aktiven Abonnements, die von Ihren Datenconsumern nach der Annahme Ihrer Einladung erstellt wurden. Sie können zukünftige Aktualisierungen für den Datenconsumer beenden, indem Sie das Freigabeabonnement und dann *Widerrufen* auswählen. 

## <a name="snapshot-history"></a>Momentaufnahmeverlauf 

Auf der Registerkarte „Verlauf“ können Sie die Momentaufnahmen anzeigen, die in den Mandanten Ihres Datenconsumers kopiert wurden. Sie können die Häufigkeit und Dauer der einzelnen Momentaufnahmeintervalle überwachen. 

![Momentaufnahmeverlauf](./media/sent-shares.png "Momentaufnahmeverlauf") 

Sie können weitere Details zu jeder Momentaufnahmeausführung anzeigen, indem Sie auf das Startdatum der Ausführung klicken. 

Der Momentaufnahmeverlauf wird standardmäßig bis zu 30 Tage angezeigt. Wenn Sie den Verlauf über 30 Tage hinaus sehen möchten, navigieren Sie zu „Überwachung -> Diagnoseeinstellungen“ und wählen **Diagnoseeinstellung hinzufügen** aus. Sie müssen ein Speicherkonto auswählen, in dem die Protokolle gespeichert werden sollen. 

![Momentaufnahmeverlauf](./media/diagnostic-settings.png "Diagnoseeinstellungen") 

## <a name="next-steps"></a>Nächste Schritte 

Weitere Informationen zur [Azure Data Share-Terminologie](terminology.md)