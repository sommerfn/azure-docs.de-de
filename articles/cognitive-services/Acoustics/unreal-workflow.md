---
title: Entwurfstutorial für Projekt Akustik in Unreal
titlesuffix: Azure Cognitive Services
description: Dieses Tutorial beschreibt den Entwurfsworkflow für Projekt Akustik in Unreal und Wwise.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1692032b093cd6189cac3ea3f63c563d9accd8ed
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477829"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Entwurfstutorial für Projekt Akustik in Unreal/Wwise
Dieses Tutorial beschreibt die Entwurfseinrichtung und den -workflow für Projekt Akustik in Unreal und Wwise.

Softwarevoraussetzungen:
* Ein Unreal-Projekt mit den Projekt Akustik-Plug-Ins für Wwise und Unreal

Ein Unreal-Projekt mit Projekt Akustik erhalten Sie auf folgende Weise:
* Befolgen Sie die Anweisungen zur [Unreal-Integration in Projekt Akustik](unreal-integration.md), um Ihrem Unreal-Projekt Projekt Akustik hinzuzufügen.
* Oder verwenden Sie das [Projekt Akustik-Beispielprojekt](unreal-quickstart.md).

## <a name="setup-project-wide-wwise-properties"></a>Einrichten von projektweiten Wwise-Eigenschaften
Wwise verfügt über globale Kurven für Behinderung und Verdeckung, die beeinflussen, wie das Projekt Akustik-Plug-In den Wwise-Audio-DSP steuert.

### <a name="design-wwise-occlusion-curves"></a>Entwerfen von Wwise-Verdeckungskurven
Wenn Projekt Akustik aktiv ist, reagiert es auf die in Wwise festgelegten Kurven für Verdeckungslautstärke, Tiefpassfilter (LPF) und Hochpassfilter (HPF). Es wird empfohlen, einen linearen Lautstärkekurventyp mit dem Wert -100 dB für den Verdeckungswert 100 festzulegen.

Mit dieser Einstellung wird bei einer berechneten Verdeckung von -18 dB durch die Projekt Akustik-Simulation die nachstehende Kurve bei X = 18 abgefragt, und der entsprechende Y-Wert ist die angewandte Dämpfung. Legen Sie den Endpunkt auf -50 dB statt -100 dB fest, um eine halbe Verdeckung anzuwenden, oder auf -200 dB, um die Verdeckung zu intensivieren. Sie können jede Kurve anpassen und optimieren, sodass sie für Ihr Spiel am besten geeignet ist.
 
![Screenshot des Editors für Wwise-Verdeckungskurven](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Deaktivieren der Wwise-Hinderniskurven
Die Wwise-Hinderniskurven betreffen isoliert den Trockenheitsgrad. Projekt Akustik nutzt jedoch Entwurfssteuerelemente und Simulationen, um Verwässerungs-Trockenheits-Verhältnisse zu erzwingen. Es wird empfohlen, die Hindernis-Lautstärkekurve zu deaktivieren. Um die Verwässerung zu entwerfen, verwenden Sie das weiter unten beschriebene Verwässerungs-Anpassungssteuerelement.
 
Wenn Sie Hindernis-LPF-/HPF-Kurven für andere Zwecke verwenden, stellen Sie sicher, diese bei X = 0 auf Y = 0 festzulegen (d. h., wenn LPF und HPF nicht aktiv sind, ist auch kein Hindernis vorhanden).

![Screenshot des Editors für Wwise-Hinderniskurven](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Entwerfen von Projekt Akustik-Mixerparametern
Sie können globale Halleigenschaften steuern, indem Sie zur Registerkarte für das Mixer-Plug-In des Projekt Akustik-Busses navigieren. Doppelklicken Sie auf „Project Acoustics Mixer (Custom)“ (Projekt Akustik-Mixer (benutzerdefiniert)), um das Einstellungsbedienfeld des Mixer-Plug-Ins zu öffnen.

Sie sehen auch, dass das Mixer-Plug-In die Option „Perform Spatialization“ (Räumliche Aufteilung ausführen) aufweist. Wenn Sie bevorzugen, die integrierte räumliche Aufteilung von Projekt Akustik zu verwenden, aktivieren Sie das Kontrollkästchen „Perform Spatialization“ (Räumliche Aufteilung ausführen), und wählen Sie zwischen HRTF und Schwenken aus. Deaktivieren Sie ggf. eingerichtete trockene Aux-Busse, andernfalls wird der direkte Pfad zweimal wiedergegeben. Verwenden Sie „Wetness Adjust“ (Verwässerungsanpassung) und „Reverb Time Scale Factor“ (Skalierungsfaktur für Hallzeit), um die Hallabmischung global zu steuern. Beachten Sie, dass Sie Unreal neu starten und dann die Soundbänke neu generieren müssen, bevor Sie die Umgebung wiedergeben, damit das Mixer-Plug-In Konfigurationsänderungen wie das Kontrollkästchen „Perform Spatialization“ (Räumliche Aufteilung ausführen) übernimmt.

![Screenshot der Wwise-Optionen des Mixer-Plug-Ins für Projekt Akustik](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Festlegen der Projekt Akustik-Entwurfssteuerelemente in der Wwise-Akteurmixerhierarchie
Um die Steuerelementparameter eines einzelnen Akteurmixers zu steuern, doppelklicken Sie auf den Akteurmixer, und klicken Sie auf die Registerkarte seines Mixer-Plug-Ins. Hier können Sie alle Parameter auf der Ebene der einzelnen Sounds ändern. Diese Werte werden mit den Werten von der Unreal-Seite (weiter unten beschrieben) kombiniert. Wenn das Unreal-Plug-In von Projekt Akustik beispielsweise die Außenbereichsanpassung für ein Objekt auf 0,5 und Wwise sie auf -0,25 festlegt, wird auf diesen Sound die resultierende Außenbereichsanpassung von 0,25 angewandt.

![Screenshot der Mixereinstellungen pro Sound in der Wwise-Akteurmixerhierarchie](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Sicherstellen, dass der Aux-Bus eine trockene Sendung aufweist und der Ausgabebus eine verwässerte
Denken Sie daran, dass in der erforderlichen Akteurmixereinrichtung das übliche Trockenheits- und Verwässerungsrouting in Wwise miteinander gemischt werden. Es wird ein Hallsignal auf dem Ausgabebus des Akteurmixers (auf den Projekt Akustik-Bus festgelegt) und ein trockenes Signal am benutzerdefinierten Aux-Bus erzeugt. Dieses Routing ist erforderlich, da bestimmte Funktionen der Wwise-Mixer-Plug-In-API vom Projekt Akustik-Wwise Plug-In genutzt werden.

![Screenshot des Wwise-Editors mit Richtlinien zum Stimmendesign für Projekt Akustik](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Einrichten von Distanzdämpfungskurven
Stellen Sie sicher, dass für alle von Akteurmixern mit Projekt Akustik verwendeten Dämpfungskurven die benutzerdefinierte Aux-Ausgabe auf „output bus volume“ (Ausgabebuslautstärke) festgelegt ist. Dies ist in Wwise für neu erstellte Dämpfungskurven standardmäßig der Fall. Wenn Sie ein vorhandenes Projekt migrieren, überprüfen Sie die Kurveneinstellungen.

Standardmäßig weist die Projekt Akustik-Simulation einen Radius von 45 Metern um die Spielerposition herum auf. Im Allgemeinen sollten Sie die Dämpfungskurve bei etwa dieser Entfernung auf 200 dB festlegen. Diese Entfernung ist keine feste Einschränkung. Bei einigen Sounds wie Waffen empfiehlt sich ein größerer Radius. Der Nachteil ist in diesen Fällen, dass nur die Geometrie innerhalb von 45 m um die Position des Spielers herum einbezogen wird. Wenn sich der Spieler in einem Raum befindet, und eine Tonquelle befindet sich außerhalb des Raums und ist 100 m weit entfernt, wird sie ordnungsgemäß verdeckt. Wenn sich die Quelle in einem Raum befindet, und der Spieler befindet sich außerhalb und ist 100 m weit entfernt, wird sie nicht ordnungsgemäß verdeckt.

![Screenshot der Wwise-Dämpfungskurven](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>Post-Mixer-Equalizer ###
 Falls gewünscht, können Sie auch einen Post-Mixer-Equalizer hinzufügen. Sie können den Projekt Akustik-Bus als typischen Hallbus (im Standardhallmodus) behandeln und zu Ausgleichszwecken einen Filter anwenden. Ein Beispiel dafür finden Sie im Wwise-Beispielprojekt von Projekt Akustik.

![Screenshot: Post-Mixer-Equalizer von Wwise](media/wwise-post-mixer-eq.png)

So kann beispielsweise der Bass von Nahbereichsaufnahmen mit dröhnendem, unrealistischem Hall mit einem Hochpassfilter behandelt werden. Sie können den Equalizer auch über RTPCs anpassen, um ein höheres Maß an Post-Bake-Kontrolle zu erreichen und die Farbe des Halls zur Spielzeit ändern zu können.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Einrichten der szenenweiten Projekt Akustik-Eigenschaften

Der Akteur für den Akustikraum stellt viele Steuerelemente bereit, die das Verhalten des Systems ändern und nützlich beim Debuggen sind.

![Screenshot der Steuerelemente für den Akustikraum in Unreal](media/acoustics-space-controls.png)

* **Acoustics Data** (Akustikdaten): Diesem Feld muss eine Akustikressource mit ausgeführtem Baking aus dem Verzeichnis „Content/Acoustics“ zugewiesen werden. Das Projekt Akustik-Plug-In fügt das Verzeichnis „Content/Acoustics“ automatisch den Verzeichnissen in Ihrem Projektpaket hinzu.
* **Tile size** (Kachelgröße): Das Ausmaß des Bereichs um den Zuhörer, für den Akustikdaten in den Arbeitsspeicher geladen werden sollen. Sofern die Zuhörerprüfpunkte im direkten Umkreis des Spielers geladen werden, ist das Ergebnis dasselbe wie beim Laden der Akustikdaten für alle Prüfpunkte. Größere Kacheln benötigen mehr Arbeitsspeicher, jedoch weniger Datenträgervorgänge.
* **Auto Stream** (Automatischer Stream): Wenn diese Option aktiviert ist, werden neue Kacheln automatisch geladen, sobald der Zuhörer den Rand einer geladenen Region erreicht. Bei deaktivierter Option müssen Sie neue Kacheln manuell über Code oder Blaupausen laden.
* **Cache Scale** (Cacheskalierung): steuert die Größe des Caches für akustische Abfragen. Ein kleinerer Cache benötigt weniger Arbeitsspeicher, jedoch eventuell für jede Abfrage eine höhere CPU-Nutzung.
* **Acoustics Enabled** (Akustik aktiviert): Ein Debugsteuerelement zum schnellen A/B-Umschalten der Akustiksimulation. Dieses Steuerelement wird in zur Auslieferung vorgesehenen Konfigurationen ignoriert. Das Steuerelement ist nützlich, um herauszufinden, ob ein bestimmter Audiofehler durch die Akustikberechnungen oder ein anderes Problem im Wwise-Projekt verursacht wird.
* **Update Distances** (Entfernungen aktualisieren): Verwenden Sie diese Option, wenn Sie die Akustikinformationen mit vorab ausgeführtem Baking für Entfernungsabfragen verwenden möchten. Diese Abfragen ähneln dem Aussenden von Strahlen, wurden jedoch vorab berechnet und benötigen daher deutlich weniger CPU-Leistung. Eine Beispiel für die Verwendung sind diskrete Reflexionen an der dem Zuhörer am nächsten gelegenen Fläche. Um dies vollständig zu nutzen, müssen Sie Entfernungen in Code oder Blaupausen abfragen.
* **Draw Stats** (Statistiken anzeigen): Die Funktion `stat Acoustics` der Unreal Engine kann zwar CPU-Informationen bereitstellen, diese Statusanzeige zeigt jedoch die derzeit geladene ACE-Datei, die Arbeitsspeichernutzung und andere Statusinformationen links oben auf dem Bildschirm an.
* **Draw Voxels** (Voxel anzeigen): Überlagert Voxel nahe am Zuhörer, sodass das während der Laufzeitinterpolation verwendete Voxelraster erkennbar ist. Wenn eine Tonquelle sich in einem Laufzeitvoxel befindet, tritt bei der akustischen Abfrage ein Fehler auf.
* **Draw Probes** (Prüfpunkte anzeigen): Zeigt alle Prüfpunkte für diese Szene an. Sie werden je nach ihrem Ladezustand in unterschiedlichen Farben dargestellt.
* **Draw Distances** (Entfernungen anzeigen): Wenn das Aktualisieren von Entfernungen aktiviert ist, wird hiermit ein Feld auf der dem Zuhörer am nächsten gelegenen Fläche in quantisierten Richtungen um den Zuhörer angezeigt.

## <a name="actor-specific-acoustics-design-controls"></a>Akteurspezifische Akustik-Entwurfssteuerelemente
Diese Entwurfssteuerelemente beziehen sich auf eine bestimmte Audiokomponente in Unreal.

![Screenshot der Steuerelemente für Audiokomponenten in Unreal](media/audio-component-controls.png)

* **Occlusion Multiplier** (Verdeckungsmultiplikator): Steuert den Verdeckungseffekt. Werte über 1 verstärken die Verdeckung. Werte unter 1 minimieren sie.
* **Wetness Adjustment** (Verwässerungsanpassung): Zusätzlicher Hall in dB
* **Decay Time Multiplier** (Verfallszeitmultiplikator): Steuert die RT60 als Faktor basierend auf der Ausgabe der Akustiksimulation
* **Outdoorness Adjustment** (Außenbereichsanpassung): Steuert den Außenbereichsanteil des Halls. Näher an 0 liegende Werte entsprechen stärker geschlossenen Räumen, näher an 1 liegende stärker Außenbereichen. Diese Anpassung erfolgt additiv, sodass ein Festlegen auf -1 geschlossenen Räumen entspricht und ein Festlegen auf +1 einem Außenbereich.
* **Transmission Db** (Übertragung in dB): Rendert einen weiteren „durch die Wand“ übertragenen Sound mit dieser Lautstärke in Verbindung mit auf der Sichtlinie basierender Distanzdämpfung.
* **Wet Ratio Distance Warp** (Verwässerungsverhältnis der Entfernungsverzerrung): Passt die Halleigenschaften der Quelle an, als wäre sie weniger weit/weiter entfernt, ohne den direkten Pfad zu beeinflussen.
* **Play on Start** (Beim Start wiedergeben): Hiermit können Sie steuern, ob der Sound beim Start der Szene automatisch wiedergegeben werden soll. Standardmäßig aktiviert.
* **Show Acoustic Parameters** (Akustikparameter anzeigen): Zeigt Debuginformationen direkt über der Komponente im Spiel an (nur für nicht für die Auslieferung bestimmte Konfigurationen).

## <a name="blueprint-functionality"></a>Blaupausenfunktionen
Der Akustikraumakteur ist über Blaupausen zugänglich und bietet Funktionen wie das Laden einer Karte oder das Ändern von Einstellungen über Levelskripts. Wir führen hier zwei Beispiele auf.

### <a name="add-finer-grained-control-over-streaming-load"></a>Hinzufügen von feiner abgestufter Steuerung der Streaminglast
Wenn Sie das Streamen akustischer Daten selbst verwalten möchten, anstatt automatisch basierend auf der Spielerposition zu streamen, können Sie die Blaupausenfunktion „Force Load Tile“ (Laden der Kachel erzwingen) verwenden:

![Screenshot der Optionen für Blaupausenstreaming in Unreal](media/blueprint-streaming.png)

* **Target** (Ziel): Der Akteur AcousticsSpace
* **Center Position** (Mittenposition): Die Mitte des Bereichs, in dem Daten geladen werden sollen
* **Unload Probes Outside Tile** (Prüfpunkte außerhalb der Kachel entladen): Wenn diese Option aktiviert ist, werden alle Prüfpunkte, die sich nicht in der neuen Region befinden, aus dem Arbeitsspeicher entladen. Bei deaktivierter Option wird die neue Region in den Arbeitsspeicher geladen, während die vorhandenen Prüfpunkte ebenfalls im Arbeitsspeicher verbleiben.
* **Block on Completion** (Nach Fertigstellung sperren): Führt das Laden der Kachel als synchronen Vorgang aus

Die Kachelgröße muss bereits vor dem Aufrufen von „Force Load Tile“ (Laden der Kachel erzwingen) festgelegt werden. Beispielsweise können Sie auf diese Weise eine ACE-Datei laden, die Kachelgröße festlegen und den Stream in einer Region beginnen:

![Screenshot der Optionen für Streamingeinrichtung in Unreal](media/streaming-setup.png)

Die in diesem Beispiel verwendete Azure Blueprint-Funktion „Load Acoustics Data“ hat die folgenden Parameter:

* **Target** (Ziel): Den Akteur AcousticsSpace.
* **Neues Baking:** Das zu ladende Objekt mit den Akustikdaten. Wenn Sie dieses Feld leer lassen bzw. auf NULL festlegen, wird der aktuelle Bakingvorgang entladen, ohne einen neuen zu laden.

### <a name="optionally-query-for-surface-proximity"></a>Optionales Abfragen der Nähe zu einer Fläche
Wenn Sie sehen möchten, wie nah sich Flächen in einer bestimmten Richtung um den Zuhörer befinden, können Sie die Funktion „Query Distance“ (Entfernung abfragen) verwenden. Diese Funktion kann zum Steuern direktional verzögerter Reflexionen oder für andere Spiellogik, die von der Nähe zu Flächen abhängt, nützlich sein. Die Abfrage ist weniger aufwendig als das Aussenden von Strahlen, da die Ergebnisse aus der Akustiksuchtabelle abgerufen werden.

![Screenshot einer Beispielentfernungsabfrage für die Blaupause](media/distance-query.png)

* **Target** (Ziel): Der Akteur AcousticsSpace
* **Look Direction** (Blickrichtung): Die Richtung für die Abfrage mit dem Zuhörer in der Mitte
* **Distance** (Entfernung): Wenn die Abfrage erfolgreich ist, die Entfernung zur nächsten Fläche
* **Return Value** (Rückgabewert): Boolescher Wert: TRUE, wenn die Abfrage erfolgreich war, andernfalls FALSE

## <a name="next-steps"></a>Nächste Schritte
* Erkunden der Konzepte hinter dem [Entwurfsvorgang](design-process.md)
* [Erstellen eines Azure-Kontos](create-azure-account.md) für eigene Bakingvorgänge einer Szene


