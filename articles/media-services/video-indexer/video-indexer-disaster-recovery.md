---
title: 'Video Indexer: Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) – Azure | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie im Falle eines Ausfalls oder Fehlers des regionalen Rechenzentrums ein Failover auf ein sekundäres Video Indexer-Konto ausführen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: ce788b4640f0a6c6f25b3280ce4f52fd018d1699
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668314"
---
# <a name="handle-video-indexer-business-continuity-and-disaster-recovery"></a>Vorkehrungen für die Geschäftskontinuität und Notfallwiederherstellung von Video Indexer

Azure Media Services Video Indexer bietet kein sofortiges Failover des Diensts, wenn es zu einem Ausfall oder Fehler des regionalen Rechenzentrums gekommen ist. In diesem Artikel wird erläutert, wie Sie Ihre Umgebung für ein Failover konfigurieren, um eine optimale Verfügbarkeit für Anwendungen und eine minimale Wiederherstellungszeit bei einem Notfall sicherzustellen.

Wir empfehlen das Konfigurieren von Business Continuity Disaster Recovery (BCDR) zwischen Regionalpaaren, um von Richtlinien für Isolierung und Verfügbarkeit von Azure zu profitieren. Weitere Informationen finden Sie unter [Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="prerequisites"></a>Voraussetzungen 

Ein Azure-Abonnement. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren.

## <a name="failover-to-a-secondary-account"></a>Failover auf ein sekundäres Konto

Um BCDR implementieren zu können, müssen Sie über zwei Video Indexer-Konten verfügen, um Redundanz sicherzustellen.

1. Erstellen Sie zwei Video Indexer-Konten, die mit Azure verbunden sind (siehe [Erstellen von Konten](connect-to-azure.md)). Ein Konto für Ihre primäre Region und das andere für die gekoppelte Azure-Region. 
1. Wenn in Ihrer primären Region ein Fehler auftritt, wechseln Sie mit dem sekundären Konto zur Indizierung.

> [!TIP]
> Sie können BCDR automatisieren, indem Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen gemäß den Ausführungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../../service-health/alerts-activity-log-service-notifications.md) einrichten.

Informationen zum Verwenden mehrerer Mandanten finden Sie unter [Verwalten mehrerer Mandanten](manage-multiple-tenants.md). Wählen Sie zum Implementieren von BCDR eine der folgenden beiden Optionen aus: [Video Indexer-Konto pro Mandant](manage-multiple-tenants.md#video-indexer-account-per-tenant) oder [Azure-Abonnement pro Mandant](manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Nächste Schritte

[Verwalten eines Video Indexer-Kontos mit Azure-Verbindung](manage-account-connected-to-azure.md)
