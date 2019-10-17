---
title: Datenmodellierung in Azure Time Series Insights (Vorschauversion) | Microsoft-Dokumentation
description: Grundlegendes zur Datenmodellierung in Azure Time Series Insights Preview.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: seodec18
ms.openlocfilehash: 64f892f344780d4af58c70935b9b7b68bad9550d
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273747"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Datenmodellierung in Azure Time Series Insights Preview

In diesem Dokument wird beschrieben, wie Sie mit Zeitreihenmodellen gemäß der Azure Time Series Insights Preview arbeiten. Es werden mehrere allgemeine Datenszenarien dargestellt.

Weitere Informationen zur Verwendung des Updates finden Sie unter [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

## <a name="types"></a>Typen

### <a name="create-a-single-type"></a>Erstellen eines einzelnen Typs

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Typen** aus. Reduzieren Sie den Bereich, um den Fokus auf die Typen von Zeitreihenmodellen zu legen.

    [![Erstellen eines einzelnen Typs](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. Wählen Sie **+ Hinzufügen**.
1. Geben Sie alle Details ein, die sich auf Typen beziehen, und wählen Sie **Erstellen** aus. Diese Aktion erstellt Typen in der Umgebung.

    [![Hinzufügen eines Typs](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Massenupload eines oder mehrerer Typen

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Typnutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    [![Hochladen von JSON](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Bearbeiten eines einzelnen Typs

1. Wählen Sie den Typ und dann **Bearbeiten** aus. 
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Bearbeiten eines Typs](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Löschen eines Typs

1. Wählen Sie den Typ aus, und wählen Sie **Bearbeiten** aus.
1. Wenn den Typen keine Instanzen zugeordnet sind, wird er gelöscht.

    [![Löschen eines Typs](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Hierarchien

### <a name="create-a-single-hierarchy"></a>Erstellen einer einzelnen Hierarchie

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Hierarchien** aus. Reduzieren Sie den Bereich, um den Fokus auf die Hierarchien von Zeitreihenmodellen zu legen.

    [![Auswählen von Hierarchien](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. Wählen Sie **+ Hinzufügen**.

    [![Hinzufügen einer Hierarchie](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Wählen Sie im rechten Bereich **+ Ebene hinzufügen** aus.

    [![Hinzufügen einer Ebene](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Geben Sie die Hierarchiedetails ein, und wählen Sie **Erstellen** aus.

    [![Erstellen einer Ebene](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Massenupload einer oder mehrerer Hierarchien

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Hierarchienutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    [![Massenupload von Hierarchien](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Bearbeiten einer einzelnen Hierarchie

1. Wählen Sie die Hierarchie und dann **Bearbeiten** aus.
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Bearbeiten einer einzelnen Hierarchie](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Löschen einer Hierarchie

1. Wählen Sie die Hierarchie und dann **Löschen** aus. 
1. Wenn der Hierarchie keine Instanzen zugeordnet sind, wird sie gelöscht.

    [![Löschen einer Hierarchie](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>Instanzen

### <a name="create-a-single-instance"></a>Erstellen einer einzelnen Instanz

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Instanzen** aus. Reduzieren Sie den Bereich, um den Fokus auf die Instanzen von Zeitreihenmodellen zu legen.

    [![Erstellen einer einzelnen Instanz](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. Wählen Sie **Hinzufügen**.

    [![Hinzufügen einer Instanz](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Geben Sie die Instanzdetails ein, wählen Sie die Zuordnung von Typ und Hierarchie aus, und wählen Sie **Erstellen** aus.

### <a name="bulk-upload-one-or-more-instances"></a>Massenupload einer oder mehrerer Instanzen

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Instanzennutzlast enthält.

    [![Massenupload einer oder mehrerer Instanzen](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. Wählen Sie die Option **Hochladen**.

### <a name="edit-a-single-instance"></a>Bearbeiten einer einzelnen Instanz

1. Wählen Sie die Instanz und dann **Bearbeiten** aus. 
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Bearbeiten einer einzelnen Instanz](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Datenmodellierung](./time-series-insights-update-tsm.md).

- Weitere Informationen zur Preview finden Sie unter [Visualisieren von Daten im Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

- Informationen zu unterstützten JSON-Formen finden Sie unter [Unterstützte JSON-Formen](./time-series-insights-send-events.md#supported-json-shapes).
