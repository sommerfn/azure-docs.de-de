---
title: Azure Security Center-Suche | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Azure Security Center Ihre Sicherheitsdaten mithilfe der Azure Monitor-Protokollsuche abruft und analysiert.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: 90dbb33fa516d3a831d4e60969ac6b6c8312d539
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64574141"
---
# <a name="azure-security-center-search"></a>Azure Security Center-Suche

> [!NOTE]
> Das Dashboard „Suchen“ des Azure Security Center wird am 31. Juli 2019 eingestellt. Weitere Informationen und alternative Dienste finden Sie unter [Auslaufen von Security Center-Funktionen (Juli 2019)](security-center-features-retirement-july2019.md#menu_search).

Azure Security Center verwendet die [Azure Monitor-Protokollsuche](../log-analytics/log-analytics-log-searches.md), um Ihre Sicherheitsdaten abzurufen und zu analysieren. Azure Monitor-Protokolle bietet eine Abfragesprache zum schnellen Abrufen und Konsolidieren von Daten. In Security Center können Sie mithilfe der Azure Monitor-Protokollsuche Abfragen erstellen und gesammelte Daten analysieren.

Die Suche steht sowohl im Free-Tarif als auch im Standard-Tarif von Security Center zur Verfügung.  Welche Daten in Ihren Protokollsuchen verfügbar sind, hängt davon ab, welcher Tarif auf Ihren Arbeitsbereich angewendet wurde.  Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](../security-center/security-center-pricing.md).


> [!NOTE]
> Security Center speichert keine Sicherheitsdaten für einen Arbeitsbereich mit dem Free-Tarif. Sie können verschiedene Protokolle an einen Arbeitsbereich mit dem Free-Tarif senden und Suchvorgänge für diese Daten durchführen, die Suchergebnisse enthalten jedoch keine Daten von Security Center. Security Center speichert Daten nur in einem Arbeitsbereich mit dem Standard-Tarif.
>
>

## <a name="access-search"></a>Zugreifen auf die Suche
1. Klicken Sie im Hauptmenü von Security Center auf **Suche**.

   ![Auswählen der Protokollsuche][1]

2. Security Center führt alle Arbeitsbereiche unter Ihren Azure-Abonnements auf. Wählen Sie einen Arbeitsbereich aus. (Falls nur ein Arbeitsbereich vorhanden ist, wird diese Arbeitsbereichsauswahl nicht angezeigt.)

   ![Auswählen eines Arbeitsbereichs][2]

3. Die **Protokollsuche** wird geöffnet. Wenn Sie weitere Daten aus dem ausgewählten Arbeitsbereich abfragen möchten, geben Sie die folgende Beispielabfrage ein:

   SecurityEvent | where EventID == 4625 | summarize count() by TargetAccount

   Das Ergebnis enthält alle Konten mit nicht erfolgreicher Anmeldung (Ereignis 4625).

   ![Suchergebnisse][3]

Weitere Informationen zum Abfragen von Daten im ausgewählten Arbeitsbereich finden Sie in der [Referenz zur Kusto-Abfragesprache](../log-analytics/log-analytics-search-reference.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie auf die Suche in Security Center zugreifen. Security Center verwendet die Azure Monitor-Protokollsuche. Weitere Informationen zur Azure Monitor-Protokollsuche finden Sie unter folgenden Themen:

- [Was ist Azure Monitor-Protokolle?](../log-analytics/log-analytics-overview.md) – Übersicht über Azure Monitor-Protokolle
- [Grundlegendes zu Protokollsuchvorgängen in Azure Monitor-Protokolle](../log-analytics/log-analytics-log-search-new.md) – In diesem Artikel wird beschrieben, wie Protokollsuchen in Azure Monitor-Protokolle verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen einer Protokollsuche bekannt sein sollten.
- [Suchen von Daten mit Protokollsuchen in Azure Monitor-Protokolle](../log-analytics/log-analytics-log-searches.md) – Tutorial zur Verwendung der Protokollsuche
- [Referenz zur Kusto-Suche](../log-analytics/log-analytics-search-reference.md) – Beschreibung der Abfragesprache in Azure Monitor-Protokolle

Weitere Informationen zu Security Center finden Sie unter folgendem Link:

- [Einführung in Azure Security Center](security-center-intro.md) – Hier werden wichtige Funktionen von Security Center beschrieben.

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
