---
title: Einrichten einer Gerätevorlage in einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Informationen zum Einrichten einer Gerätevorlage mit Messungen, Einstellungen, Eigenschaften, Regeln und Dashboard.
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e3925f2d6c2a1034cc96475721de7936deeb4a9d
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894414"
---
# <a name="set-up-and-manage-a-device-template-preview-features"></a>Einrichten und Verwalten einer Gerätevorlage (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Eine Gerätevorlage ist eine Blaupause, die die Merkmale und das Verhalten eines Gerätetyps definiert, der eine Verbindung mit einer Azure IoT Central-Anwendung herstellt.

Beispielsweise kann ein Hersteller eine Gerätevorlage für einen verbundenen Lüfter mit folgenden Merkmalen erstellen:

- Sendet Temperaturtelemetriedaten
- Sendet Standorteigenschaft
- Sendet Lüftermotor-Fehlerereignisse
- Sendet Lüfterbetriebszustand
- Schreibbare Eigenschaft für Lüftergeschwindigkeit
- Befehl zum Neustarten des Geräts
- Dashboard mit einer allgemeinen Übersicht über das Gerät

Anhand dieser Gerätevorlage kann ein Bediener echte Lüftergeräte erstellen und verbinden. Alle diese Lüfter weisen Messungen, Eigenschaften und Befehle auf, die von Bedienern zum Überwachen und Verwalten verwendet werden. Bediener verwenden die Gerätedashboards und -formulare, um mit den Lüftergeräten zu interagieren.

> [!NOTE]
> Nur Ersteller und Administratoren können Gerätevorlagen erstellen, bearbeiten und löschen. Auf der Seite **Geräte** kann jeder Benutzer Geräte anhand vorhandener Gerätevorlagen erstellen.

[IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md) ermöglicht IoT Central die Integration von Geräten, ohne dass Sie eingebetteten Gerätecode schreiben müssen. Das Herzstück von IoT Plug & Play ist ein Gerätefunktionsmodell-Schema, das Gerätefunktionen beschreibt. In einer IoT Central Preview-Anwendung verwenden Gerätevorlagen diese IoT Plug & Play-Gerätefunktionsmodelle.

Als Ersteller haben Sie mehrere Möglichkeiten zum Erstellen von Gerätevorlagen:

- Entwerfen Sie die Gerätevorlage in IoT Central, und implementieren Sie dann das entsprechende Gerätefunktionsmodell in Ihrem Gerätecode.
- Importieren Sie ein Gerätefunktionsmodell aus dem [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat), und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie Ihren Gerätecode aus dem Modell. Importieren Sie das Gerätefunktionsmodell manuell in Ihre IoT Central-Anwendung, und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie den Gerätecode aus dem Modell, und verbinden Sie das echte Gerät mithilfe einer gerätepriorisierenden Verbindung mit Ihrer IoT Central-Anwendung. IoT Central ermittelt und importiert das Gerätefunktionsmodell automatisch aus dem öffentlichen Repository. Sie können dann alle Cloudeigenschaften, Anpassungen und Dashboards, die Ihre IoT Central-Anwendung benötigt, der Gerätevorlage hinzufügen.

## <a name="create-a-device-template-from-the-device-catalog"></a>Erstellen einer Gerätevorlage aus dem Gerätekatalog

Als Ersteller können Sie mit einem IoT Plug & Play-zertifizierten Gerät, das im [Azure IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/alldevices) aufgeführt ist, schnell mit der Erstellung Ihrer Lösung beginnen. IoT Central ist in den Gerätekatalog integriert, damit Sie ein Gerätefunktionsmodell von einem dieser IoT Plug & Play-zertifizierten Geräte importieren können. Zum Erstellen einer Gerätevorlage von einem dieser Geräte in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** aus, und wählen Sie dann eines der IoT Plug & Play-zertifizierten Geräte aus dem unten aufgeführten Katalog aus. IoT Central erstellt eine Gerätevorlage basierend auf diesem Gerätefunktionsmodell.
1. Fügen Sie der Gerätevorlage beliebige Cloudeigenschaften, Anpassungen oder Ansichten hinzu.
1. Wählen Sie **Veröffentlichen** aus, um diese Gerätevorlage zu veröffentlichen und so den Bedienern zum Anzeigen und Verbinden von Geräten zur Verfügung zu stellen.

## <a name="create-a-device-template-from-scratch"></a>Erstellen einer vollkommen neuen Gerätevorlage

Eine Gerätevorlage umfasst Folgendes:

- Ein _Gerätefunktionsmodell_, das die Telemetrie, Eigenschaften und Befehle angibt, die das Gerät implementiert. Diese Funktionen sind in einer oder mehreren Schnittstellen organisiert.
- _Cloudeigenschaften_, die Informationen definieren, die Ihre IoT Central-Anwendung über Ihre Geräte speichert. Beispielsweise kann eine Cloudeigenschaft das Datum der letzten Wartung eines Geräts erfassen. Diese Informationen werden niemals für das Gerät freigegeben.
- Durch _Anpassungen_ kann der Ersteller einige der Definitionen im Gerätefunktionsmodell überschreiben. Beispielsweise kann der Ersteller den Namen einer Geräteeigenschaft überschreiben. Eigenschaftsnamen werden in IoT Central-Dashboards und -Formularen angezeigt.
- Mithilfe von _Dashboards und Formularen_ kann der Ersteller eine Benutzeroberfläche erstellen, mit der Bediener die mit der Anwendung verbundenen Geräte überwachen und verwalten können.

Zum Erstellen einer Gerätevorlage in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** und dann **Benutzerdefiniert** aus.
1. Geben Sie einen Namen für die Vorlage ein, z.B. **Umgebungssensor**.
1. Drücken Sie die **EINGABETASTE**. IoT Central erstellt eine leere Gerätevorlage.

## <a name="manage-a-device-template"></a>Verwalten einer Gerätevorlage

Sie können eine Vorlage auf der jeweiligen Startseite umbenennen oder löschen.

Nachdem Sie Ihrer Vorlage ein Gerätefunktionsmodell hinzugefügt haben, können Sie sie veröffentlichen. Erst nachdem Sie die Vorlage veröffentlicht haben, können Sie ein Gerät basierend auf dieser Vorlage verbinden, damit es den Bedienern auf der Seite **Geräte** angezeigt wird.

## <a name="create-a-capability-model"></a>Erstellen eines Funktionsmodells

Sie können ein Gerätefunktionsmodell auf folgende Arten erstellen:

- Verwenden Sie IoT Central, um ein benutzerdefiniertes Modell ohne Vorlage zu erstellen.
- Importieren Sie ein Modell aus einer JSON-Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um ein Gerätefunktionsmodell für Ihre Anwendung zu erstellen.
- Wählen Sie eines der Geräte aus dem Gerätekatalog aus. Mit dieser Option wird das Gerätefunktionsmodell importiert, das der Hersteller für dieses Gerät veröffentlicht hat. Ein auf diese Weise importiertes Gerätefunktionsmodell wird automatisch veröffentlicht.

## <a name="manage-a-capability-model"></a>Verwalten eines Funktionsmodells

Nach dem Erstellen eines Gerätefunktionsmodells haben Sie folgende Möglichkeiten:

- Fügen Sie dem Modell Schnittstellen hinzu. Ein Modell muss mindestens eine Schnittstelle aufweisen.
- Bearbeiten Sie Modellmetadaten, z.B. ID, Namespace und Name.
- Löschen Sie das Modell.

## <a name="create-an-interface"></a>Erstellen einer Schnittstelle

Eine Gerätefunktion muss mindestens eine Schnittstelle aufweisen. Eine Schnittstelle ist eine wiederverwendbare Sammlung von Funktionen.

Zum Erstellen einer Schnittstelle führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrem Gerätefunktionsmodell, und wählen Sie **+ Schnittstelle hinzufügen** aus.

1. Auf der Seite **Schnittstelle auswählen** können Sie folgende Aktionen ausführen:

    - Erstellen Sie eine benutzerdefinierte Schnittstelle ohne Vorlage.
    - Importieren Sie eine vorhandene Schnittstelle aus einer Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um eine Schnittstelle für Ihr Gerät zu erstellen.
    - Wählen Sie eine der Standardschnittstellen aus, z.B. die Schnittstelle **Geräteinformationen**. Standardschnittstellen geben die Funktionen an, die viele Geräte gemeinsam haben. Diese Standardschnittstellen werden von Microsoft Azure IoT veröffentlicht und können nicht mit einer Versionsangabe versehen oder bearbeitet werden.

1. Nachdem Sie eine Schnittstelle erstellt haben, wählen Sie **Identität bearbeiten** aus, um den Anzeigenamen der Schnittstelle zu ändern.

1. Wenn Sie eine benutzerdefinierte Schnittstelle ohne Vorlage erstellen möchten, können Sie die Funktionen Ihres Geräts hinzufügen. Gerätefunktionen sind Telemetrie, Eigenschaften und Befehle.

### <a name="telemetry"></a>Telemetrie

Telemetrie ist ein Wertedatenstrom, der vom Gerät gesendet wird, üblicherweise von einem Sensor. Beispielsweise kann ein Sensor die Umgebungstemperatur melden.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Telemetriefunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Telemetriewert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name des Felds in der Telemetrienachricht. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Telemetrie. |
| Semantischer Typ | Der semantische Typ der Telemetrie, z.B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Telemetriedatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. **Fehler**, **Information** oder **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Unit | Eine Einheit für den Telemetriewert, z.B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Dashboards und Formularen. |
| Comment | Beliebige Kommentare zur Telemetriefunktion. |
| BESCHREIBUNG | Eine Beschreibung der Telemetriefunktion. |

### <a name="properties"></a>Properties

Eigenschaften stellen Zeitpunktwerte dar. Ein Gerät kann beispielsweise eine Eigenschaft verwenden, um die Zieltemperatur zu melden, die es zu erreichen versucht. Sie können schreibbare Eigenschaften über IoT Central festlegen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Eigenschaftsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Eigenschaftswert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name der Eigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Eigenschaft. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z.B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Eigenschaftsdatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| Schreibbar | Wenn die Eigenschaft nicht schreibbar ist, kann das Gerät Eigenschaftswerte an IoT Central melden. Ist die Eigenschaft schreibbar, kann das Gerät Eigenschaftswerte an IoT Central melden, und IoT Central kann Aktualisierungen der Eigenschaft an das Gerät senden.
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. **Fehler**, **Information** oder **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Unit | Eine Einheit für den Eigenschaftswert, z.B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Dashboards und Formularen. |
| Comment | Beliebige Kommentare zur Eigenschaftsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Eigenschaftsfunktion. |

### <a name="commands"></a>Befehle

Sie können Gerätebefehle über IoT Central aufrufen. Befehle übergeben optional Parameter an das Gerät und empfangen eine Antwort vom Gerät. Sie können beispielsweise einen Befehl zum Neustarten eines Geräts in 10 Sekunden aufrufen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Befehlsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Befehl, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name des Befehls. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Get-Help |
| Get-Help | SynchronousExecutionType. |
| Comment | Beliebige Kommentare zur Befehlsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Befehlsfunktion. |
| Anforderung | Wenn aktiviert, eine Definition des Anforderungsparameters, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |
| response | Wenn aktiviert, eine Definition der Befehlsantwort, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |

## <a name="manage-an-interface"></a>Verwalten einer Schnittstelle

Wenn Sie die Schnittstelle noch nicht veröffentlicht haben, können Sie die durch die Schnittstelle definierten Funktionen bearbeiten. Nachdem die Schnittstelle veröffentlicht wurde, müssen Sie eine neue Version der Gerätevorlage erstellen und die Schnittstelle mit einer Versionsangabe versehen, um Änderungen vorzunehmen. Änderungen, für die keine Versionsangabe erforderlich ist (z.B. Anzeigenamen oder Einheiten), können im Abschnitt **Anpassen** vorgenommen werden.

Sie können die Schnittstelle auch als JSON-Datei exportieren, wenn Sie sie in einem anderen Funktionsmodell wiederverwenden möchten.

## <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Verwenden Sie Cloudeigenschaften, um Informationen zu Geräten in IoT Central zu speichern. Cloudeigenschaften werden niemals an ein Gerät gesendet. Sie können Cloudeigenschaften z.B. verwenden, um den Namen des Kunden, der das Gerät installiert hat, oder das Datum der letzten Wartung des Geräts zu speichern.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Cloudeigenschaft angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Cloudeigenschaftswert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name der Cloudeigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z.B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Datentyp der Cloudeigenschaft, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. |

## <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, wenn Sie eine importierte Schnittstelle ändern oder IoT Central-spezifische Features zu einer Funktion hinzufügen müssen. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

Der Funktionsname oder Funktionstyp kann nicht angepasst werden. Wenn Änderungen vorgenommen werden müssen, die im Abschnitt **Anpassen** nicht möglich sind, müssen Sie die Gerätevorlage und Schnittstelle mit einer Versionsangabe versehen, um die Funktion zu ändern.

### <a name="generate-default-views"></a>Generieren von Standardansichten

Das Generieren von Standardansichten ist eine schnelle Möglichkeit, um mit der Visualisierung Ihrer wichtigen Geräteinformationen zu beginnen. Für Ihre Gerätevorlage werden bis zu drei Standardansichten generiert:

1. **Befehle** bietet eine Ansicht der Gerätebefehle und ermöglicht es dem Bediener, diese an Ihr Gerät auszugeben.
1. **Übersicht** bietet eine Ansicht der Gerätetelemetrie und zeigt Diagramme und Metriken.
1. **Info** bietet eine Ansicht der Geräteinformationen und zeigt Geräteeigenschaften.

Nachdem Sie **Standardansichten generieren** ausgewählt haben, sehen Sie, dass diese automatisch im Abschnitt **Ansichten** Ihrer Gerätevorlage hinzugefügt wurden.

## <a name="add-dashboards"></a>Hinzufügen von Dashboards

Fügen Sie Dashboards zu einer Gerätevorlage hinzu, um Bedienern die Visualisierung eines Geräts mithilfe von Diagrammen und Metriken zu ermöglichen. Sie können über mehrere Dashboards für eine Gerätevorlage verfügen.

Zum Hinzufügen eines Dashboards zu einer Gerätevorlage führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** aus.
1. Wählen Sie dann **Gerät visualisieren** aus.
1. Geben Sie im Feld **Dashboardname** einen Namen für das Dashboard ein.
1. Fügen Sie dem Dashboard Kacheln aus der Liste mit statischen Kacheln und Kacheln für Eigenschaften, Cloudeigenschaften, Telemetrie und Befehle hinzu. Ziehen Sie die Kacheln, die Sie dem Dashboard hinzufügen möchten, per Drag & Drop.
1. Wenn Sie mehrere Telemetriewerte auf einer einzelnen Diagrammkachel darstellen möchten, wählen Sie die Telemetriewerte und dann **Kombinieren** aus.
1. Konfigurieren Sie jede hinzugefügte Kachel, um die Anzeige von Daten anzupassen. Wählen Sie dazu das Zahnradsymbol oder die Schaltfläche **Konfiguration ändern** auf der Diagrammkachel aus.
1. Ordnen Sie die Kacheln auf Ihrem Dashboard an, und ändern Sie deren Größe.
1. Speichern Sie die Änderungen.

### <a name="configure-preview-device-to-view-dashboard"></a>Konfigurieren eines Vorschaugeräts zum Anzeigen des Dashboards

Zum Anzeigen und Testen Ihres Dashboards können Sie **Vorschaugerät konfigurieren** auswählen. Dadurch können Sie das Dashboard so sehen, wie es den Bedienern nach der Veröffentlichung angezeigt wird. Mit dieser Option können Sie überprüfen, ob in Ihren Ansichten die richtigen Daten angezeigt werden. Sie können mithilfe der Geräte-ID zwischen keinem Vorschaugerät, dem echten Testgerät, das Sie für Ihre Gerätevorlage konfiguriert haben, oder einem vorhandenen Gerät in Ihrer Anwendung auswählen.

## <a name="add-forms"></a>Hinzufügen von Formularen

Fügen Sie Formulare zu einer Gerätevorlage hinzu, damit Bediener ein Gerät durch Anzeigen und Festlegen von Eigenschaften verwalten können. Bediener können nur Cloudeigenschaften und schreibbare Geräteeigenschaften bearbeiten. Sie können über mehrere Formulare für eine Gerätevorlage verfügen.

Zum Hinzufügen eines Formulars zu einer Gerätevorlage führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** aus.
1. Wählen Sie dann **Geräte- und Clouddaten bearbeiten** aus.
1. Geben Sie im Feld **Formularname** einen Namen für das Formular ein.
1. Wählen Sie die Anzahl der Spalten aus, die für das Layout des Formulars verwendet werden sollen.
1. Fügen Sie einem vorhandenen Abschnitt im Formular Eigenschaften hinzu, oder wählen Sie Eigenschaften und dann **Abschnitt hinzufügen** aus. Verwenden Sie Abschnitte, um Eigenschaften auf dem Formular zu gruppieren. Sie können einem Abschnitt einen Titel hinzufügen.
1. Konfigurieren Sie die einzelnen Eigenschaften auf dem Formular, um deren Verhalten anzupassen.
1. Ordnen Sie die Eigenschaften auf dem Formular an.
1. Speichern Sie die Änderungen.

## <a name="publish-a-device-template"></a>Veröffentlichen einer Gerätevorlage

Bevor Sie ein Gerät verbinden können, das Ihr Gerätefunktionsmodell implementiert, müssen Sie die Gerätevorlage veröffentlichen.

Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie nur eingeschränkte Änderungen am Gerätefunktionsmodell vornehmen. Zum Ändern einer Schnittstelle müssen Sie [eine neue Version erstellen und veröffentlichen](./howto-version-device-template.md).

Wenn Sie eine Gerätevorlage veröffentlichen möchten, navigieren Sie zu der Gerätevorlage, und wählen Sie **Veröffentlichen** aus.

Nachdem Sie eine Gerätevorlage veröffentlicht haben, kann ein Bediener zur Seite **Geräte** wechseln und entweder echte oder simulierte Geräte hinzufügen, die Ihre Gerätevorlage verwenden. Sie können Ihre Gerätevorlage weiterhin ändern und während der Änderungen speichern. Wenn Sie diese Änderungen jedoch auf der Seite **Geräte** für den Bediener bereitstellen möchten, müssen Sie jedes Mal die Option **Veröffentlichen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung eingerichtet wird, wird als Nächstes Folgendes empfohlen:

> [!div class="nextstepaction"]
> [Erstellen einer neuen Gerätevorlagenversion](howto-version-device-template.md)
> [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung](howto-connect-devkit.md)
