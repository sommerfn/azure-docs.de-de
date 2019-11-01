---
title: Konfigurieren der Vermerkdauer in der Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Vermerkdauer in Ihrer Azure Time Series Insights-Umgebung konfigurieren.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: ff4d326af691ae27894dc94d7581ba68951f090e
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990056"
---
# <a name="configuring-retention-in-time-series-insights"></a>Konfigurieren der Vermerkdauer in Time Series Insights

In diesem Artikel wird beschrieben, wie Sie die **Datenaufbewahrungsdauer** und das **Verhalten bei Überschreitung des Speicherlimits** in Azure Time Series Insights konfigurieren.

## <a name="summary"></a>Zusammenfassung

Jede Azure Time Series Insights-Umgebung verfügt über eine Einstellung zum Konfigurieren der **Datenaufbewahrungsdauer**. Der Wert kann zwischen 1 und 400 Tagen liegen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer (1 - 400) gelöscht, je nachdem, welcher Fall zuerst eintritt.

Jede Time Series Insights-Umgebung verfügt über die zusätzliche Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Diese Einstellung steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Sie können zwischen zwei Verhaltensweisen wählen:

- **Alte Daten bereinigen** (Standard)
- **Eingang anhalten**

Ausführliche Informationen zu einem besseren Verständnis dieser Einstellungen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie die vorhandene Time Series Insights-Umgebung. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Alle Ressourcen** aus. Wählen Sie Ihre Time Series Insights-Umgebung aus.

1. Klicken Sie unter der Überschrift **Einstellungen** auf **Konfigurieren**.

    [![Wählen Sie „Konfigurieren“ und dann „Einstellungen“ aus](media/data-retention/1-configure-data-retention.png)](media/data-retention/1-configure-data-retention.png#lightbox).

1. Wählen Sie die **Datenaufbewahrungsdauer** (in Tagen) aus, um die Vermerkdauer über den Schieberegler oder durch Eingabe einer Zahl in das Textfeld zu konfigurieren.

1. Notieren Sie die Einstellung **Kapazität**, da diese Konfiguration Auswirkungen auf die maximale Anzahl von Datenereignissen und die Gesamtspeicherkapazität für das Speichern von Daten hat.

1. Schalten Sie die Einstellung **Verhalten bei Überschreitung des Speicherlimits** um. Wählen Sie als Verhalten **Alte Daten bereinigen** oder **Eingang anhalten** aus.

    [![Datenaufbewahrung – Akzeptieren und Speichern](media/data-retention/2-accept-and-save.png)](media/data-retention/2-accept-and-save.png#lightbox).

1. Aktivieren Sie das Kontrollkästchen, das angibt, dass Sie die Dokumentation gelesen und das potenzielle Risiko von Datenverlusten verstanden haben. Wählen Sie zum Konfigurieren der Änderungen **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).

- Informationen zur [Vorgehensweise zur Skalierung Ihrer Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md).

- Informationen zum [Planen Ihrer Umgebung](time-series-insights-environment-planning.md).