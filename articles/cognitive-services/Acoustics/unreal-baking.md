---
title: Unreal-Bakingtutorial für Projekt Akustik
titlesuffix: Azure Cognitive Services
description: In diesem Dokument wird das Übermitteln eines Akustikbakingvorgangs mithilfe der Unreal-Editor-Erweiterung beschrieben.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 7a868a5f9b06499e23710399733b0659d97f900d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854902"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Unreal-Bakingtutorial für Projekt Akustik
In diesem Dokument wird das Übermitteln eines Akustikbakingvorgangs mithilfe der Unreal-Editor-Erweiterung beschrieben.

Für einen Bake-Vorgang sind fünf Schritte erforderlich:

1. Erstellen Sie Ihr Navigationsgittermodell für den Spieler, oder versehen Sie es mit Tags.
2. Versehen Sie die Akustikgeometrie mit Tags.
3. Weisen Sie der Geometrie Akustikmaterialeigenschaften zu.
4. Zeigen Sie eine Vorschau der Testplatzierung an.
5. Führen Sie den Bake-Vorgang durch.

## <a name="open-the-project-acoustics-editor-mode"></a>Öffnen des Projekt Akustik-Editor-Modus

Importieren Sie das Projekt Akustik-Plug-In-Paket in Ihr Projekt. Hilfe hierzu finden Sie im Thema [Unreal-Integration](unreal-integration.md). Nachdem das Plug-In integriert ist, öffnen Sie die Akustikbenutzeroberfläche, indem Sie auf das neue Symbol „Acoustics Mode“ (Akustikmodus) klicken.

![Screenshot der Option für den Akustikmodus im Unreal-Editor](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Versehen von Akteuren mit Tags für Akustik

Die Registerkarte „Objects“ (Objekte) ist die erste Registerkarte, die nach dem Öffnen des Akustikmodus angezeigt wird. Auf dieser Registerkarte können Sie Akteure in Ihrem Level mit Tags versehen, sodass die Tags **AcousticsGeometry** oder **AcousticsNavigation** den Akteuren hinzugefügt werden.

Wählen Sie Objekte im World Outliner aus, oder verwenden Sie den Abschnitt **Bulk Selection** (Massenauswahl), um alle Objekte einer bestimmten Kategorie auszuwählen. Nachdem Objekte ausgewählt wurden, verwenden Sie den Abschnitt **Tagging**, um das gewünschte Tag auf die ausgewählten Objekte anzuwenden.

Objekte, die weder das Tag **AcousticsGeometry** noch das Tag **AcousticsNavigation** aufweisen, werden bei der Simulation ignoriert. Es werden nur statische Gittermodelle, Navigationsgittermodelle und Querformate unterstützt. Wenn Sie ein anderes Objekt taggen, wird es ignoriert.

### <a name="for-reference-the-objects-tab-parts"></a>Zur Referenz: Die Elemente der Registerkarte „Objects“ (Objekte)

![Screenshot der Option „Objects“ (Objekte) auf der Registerkarte „Acoustics“ (Akustik) in Unreal](media/unreal-objects-tab-details.png)

1. Schaltflächen für die Registerkartenauswahl (Registerkarte **Objects** ist ausgewählt). Verwenden Sie diese Schaltflächen, um die Schritte eines Akustikbakingvorgangs von oben nach unten zu durchlaufen.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verfügbare Selektoren für Akteure im Level.
4. Durch Klicken auf **Select** (Auswählen) werden alle Objekte im Level ausgewählt, die mindestens einem der aktivierten Akteurtypen entsprechen.
5. Durch Klicken auf **Deselect all** (Auswahl aufheben) wird die aktuelle Auswahl gelöscht. Dies entspricht dem Drücken der ESC-Taste.
6. Verwenden Sie diese Optionsfelder, um auszuwählen, ob das Geometrietag oder das Navigationstags auf die ausgewählten Akteure angewandt werden soll.
7. Wenn Sie auf **Tag** klicken, wird allen derzeit ausgewählten Akteuren das ausgewählte Tag hinzugefügt.
8. Wenn Sie auf **Untag** (Tag entfernen) klicken, wird das ausgewählte Tag von allen derzeit ausgewählten Akteuren entfernt.
9. Durch Klicken auf **Select Tagged** (Mit Tags markierte auswählen) wird die aktuelle Auswahl gelöscht, und es werden alle Akteure mit dem aktuell ausgewählten Tag ausgewählt.
10. Diese Statistiken zeigen, wie viele Akteure mit dem Tagtyp versehen sind.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Versehen von Geometrie für akustische Verdeckung und Reflexion mit Tags

Öffnen Sie die Registerkarte „Objects“ (Objekte) des Fensters „Acoustics“ (Akustik). Markieren Sie alle Objekte als „Acoustics Geometry“ (Akustikgeometrie), die Sounds verdecken, reflektieren oder absorbieren sollen. Objekte für die Akustikgeometrie können z. B. Boden, Wände, Dächer, Fenster, Fensterglas, Teppiche oder große Möbelstücke umfassen. Sie können für diese Objekte jede beliebige Komplexität verwenden. Da die Szene vor der Simulation in Voxel umgewandelt wird, ist das Baking für hochgradig detaillierte Gittermodelle wie Bäume mit vielen kleinen Blättern nicht aufwendiger als vereinfachte Objekte.

Fügen Sie keine Objekte hinzu, die sich nicht auf die Akustik auswirken sollen, z. B. unsichtbare Gittermodelle für Kollisionen.

Die Transformation eines Objekts zum Zeitpunkt der Prüfungsberechnung (über die unten dargestellte Registerkarte „Probes“ (Prüfpunkte)) wird in den Ergebnissen des Bakingvorgangs festgehalten. Wenn Sie eines der markierten Objekte in der Szene bewegen, müssen Sie die Testberechnung und den Bake-Vorgang für die Szene wiederholen.

### <a name="create-or-tag-a-navigation-mesh"></a>Erstellen eines Navigationsgittermodells oder Versehen des Gittermodells mit Tags

Ein Navigationsgittermodell wird verwendet, um Testpunkte für die Simulation zu platzieren. Sie können die Funktion [Nav Mesh Bounds Volume](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html) (Navigationsgittermodell-Grenzvolumen) von Unreal verwenden oder ein eigenes Navigationsgittermodell angeben. Sie müssen mindestens ein Objekt mit dem Tag **AcousticsNavigation** versehen. Wenn Sie das Navigationsgittermodell von Unreal verwenden, müssen Sie zunächst sicherstellen, dass Sie das Gittermodell erstellt haben.

### <a name="acoustics-volumes"></a>Akustikvolumen (Acoustics Volume) ###

Mit **Akustikvolumen** können Sie eine erweiterte Anpassung an Ihren Navigationsbereichen vornehmen. **Akustikvolumen** sind Akteure, die Sie Ihrer Szene hinzufügen können und Ihnen die Auswahl von Bereichen ermöglichen, die in Ihr Navigationsgittermodell eingeschlossen oder ignoriert werden sollen. Der Akteur macht eine Eigenschaft verfügbar, die auf „Include“ (Einschließen) oder „Exclude“ (Ausschließen) festgelegt werden kann. Mit Volumen vom Typ „Include“ (Einschließen) können Sie sicherstellen, dass nur Bereiche des Navigationsgittermodells, die in diesen Volumen enthalten sind, berücksichtigt werden. Volumen vom Typ „Exclude“ (Ausschließen) kennzeichnen die Bereiche, die ignoriert werden sollen. Volumen vom Typ „Exclude“ (Ausschließen) werden immer nach denen vom Typ „Include“ (Einschließen) angewendet. Denken Sie daran, **Akustikvolumen** wie oben beschrieben auf der Registerkarte „Objects“ (Objekte) mit dem Tag **AcousticsNavigation** zu versehen. Diese Akteure werden ***nicht*** automatisch mit Tags versehen.

![Screenshot der Eigenschaften von Akustikvolumen in Unreal](media/unreal-acoustics-volume-properties.png)

Volumen vom Typ „Exclude“ (Ausschließen) dienen in erster Linie zur präzisen Steuerung der Stellen, an denen keine Tests platziert werden sollen, um die Ressourcennutzung zu reduzieren.

![Screenshot von Akustikvolumen vom Typ „Exclude“ (Ausschließen) in Unreal](media/unreal-acoustics-volume-exclude.png)

Mit Volumen vom Typ „Include“ (Einschließen) können Sie manuell Szenenabschnitte erstellen, z. B. wenn Sie Ihre Szene in mehreren Akustikzonen aufteilen möchten. Angenommen, Sie haben eine viele Quadratkilometer große Szene mit zwei relevanten Bereichen, für die Sie das Akustikbaking ausführen möchten. Sie können zwei große Volumen vom Typ „Include“ (Einschließen) in der Szene zeichnen und nacheinander ACE-Dateien für diese Volumen erstellen. Im Spiel können Sie dann Triggervolumen in Kombination mit Blaupausenaufrufen verwenden, um die entsprechende ACE-Datei zu laden, wenn sich der Spieler den einzelnen Kacheln nähert.

**Akustikvolumen** schränken nur die Navigation ein, aber ***nicht*** die Geometrie. Beim Durchführen von Wellensimulationen pullt jeder Test in einem **Akustikvolumen** vom Typ „Include“ (Einschließen) nach wie vor die gesamte erforderliche Geometrie außerhalb des Volumens. Daher sollte das Wechseln des Spielers zwischen den Abschnitten nicht zu verdeckten Diskontinuitäten oder anderen Akustikelementen führen.

## <a name="select-acoustic-materials"></a>Auswählen akustischer Materialien

Wenn Ihre Objekte mit Tags versehen sind, klicken Sie auf die Schaltfläche **Materials** (Materialien), um zur Registerkarte „Materials“ (Materialien) zu gelangen. Auf dieser Registerkarte werden die Materialeigenschaften für jedes Material im Level angegeben. Bevor ein Akteur mit Tags versehen wird, ist er leer.

Die akustischen Materialien bestimmten die Menge an Schallenergie, die von der Oberfläche reflektiert wird. Das akustische Standardmaterial weist eine Absorption ähnlich der von Zement auf. Projekt Akustik schlägt basierend auf dem Namen des Szenenmaterials Materialien vor.

Die Nachhallzeit eines bestimmten Materials in einem Raum ist umgekehrt proportional zum Absorptionskoeffizienten, wobei die meisten Materialien Absorptionswerte im Bereich zwischen 0,01 und 0,20 aufweisen. Materialien mit Absorptionskoeffizienten über diesem Bereich sind stark absorbierend. Wenn der Hall in einem Raum beispielsweise zu stark ist, können Sie das Akustikmaterial der Wände, Böden und Decken in ein Material mit höherer Absorption ändern. Die Zuweisung des Akustikmaterials gilt für alle Akteure, die dieses Szenenmaterial verwenden.

![Graph, der die negative Korrelation der Nachhallzeit mit dem Absorptionskoeffizienten zeigt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Materials“ (Materialien)

![Screenshot der Option „Objects“ (Objekte) auf der Registerkarte „Acoustics“ (Akustik) in Unreal](media/unreal-materials-tab-details.png)

1. Die Schaltfläche **Materials** (Materialien), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Die Liste der Materialien im Level; den mit **AcousticsGeometry** markierten Akteuren entnommen. Wenn Sie hier auf eines der Materialien klicken, werden alle Objekte in der Szene ausgewählt, die dieses Material verwenden.
4. Anzeige der Akustikmaterialien, denen die Szenenmaterialien zugewiesen wurden. Klicken Sie auf eine Dropdownliste, um ein Szenenmaterial einem anderen Akustikmaterial zuzuweisen.
5. Zeigt den Absorptionskoeffizienten des ausgewählten Akustikmaterials an, das in der vorherigen Spalte ausgewählt wurde. Ein Wert von „0“ bedeutet „perfekte Reflektion“ (keine Absorption), während ein Wert von „1“ „perfekte Absorption“ (keine Reflektion) bedeutet. Durch Ändern dieses Werts wird das Akustikmaterial (Schritt 4) in **Custom** (Benutzerdefiniert) aktualisiert.

Wenn Sie Änderungen an den Materialien in Ihrer Szene vornehmen, müssen Sie im Plug-In „Projekt Akustik“ die Registerkarte wechseln, um diese Änderungen auf der Registerkarte **Materials** (Materialien) zu sehen.

## <a name="calculate-and-review-listener-probe-locations"></a>Berechnen und Überprüfen der Zuhörer-Prüfpunktpositionen

Wenn Sie die Materialien zugewiesen haben, wechseln Sie zur Registerkarte **Probes** (Tests).

### <a name="for-reference-parts-of-the-probes-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Probes“ (Tests)

![Screenshot der Option „Probes“ (Tests) auf der Registerkarte „Acoustics“ (Akustik) in Unreal](media/unreal-probes-tab-details.png)

1. Die Schaltfläche **Probes** (Tests), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verwenden Sie dies, um „Coarse“ (Niedrig) oder „Fine“ (Hoch) als Auflösung für die Simulation auszuwählen. Eine niedrige Auflösung ist schneller, hat jedoch einige Nachteile. Ausführliche Informationen finden Sie unter [Bake Resolution](bake-resolution.md) (Bakingauflösung).
4. Wählen Sie mithilfe dieses Felds den Speicherort für die Akustikdatendateien aus. Klicken Sie auf die Schaltfläche mit den Auslassungspunkten (...), um die Ordnerauswahl zu verwenden. Weitere Informationen zu Datendateien finden Sie weiter unten unter [Datendateien](#Data-Files).
5. Die Datendateien für diese Szene werden mithilfe des hier bereitgestellten Präfixes benannt. Der Standardwert ist „[Levelname]_AcousticsData“.
6. Klicken Sie auf die Schaltfläche **Calculate** (Berechnen), um die Szene in Voxel umzuwandeln und Prüfpunktpositionen zu berechnen. Dies wird lokal auf Ihrem Computer durchgeführt und muss daher vor einem Bake-Vorgang erfolgen. Nachdem die Prüfpunkte berechnet wurden, werden die oben genannten Steuerelemente deaktiviert, und diese Schaltfläche wird in **Clear** (Löschen) umbenannt. Klicken Sie auf die Schaltfläche **Clear** (Löschen), um die Berechnungen zu löschen und die Steuerelemente zu aktivieren, damit Sie unter Verwendung der neuen Einstellungen eine Neuberechnung vornehmen können.

Prüfpunkte müssen über den automatisierten Vorgang auf der Registerkarte **Probes** (Prüfpunkte) platziert werden.


### <a name="what-the-calculate-button-calculates"></a>Berechnungen der Schaltfläche „Calculate“ (Berechnen)

Die Schaltfläche **Calculate** (Berechnen) verwendet alle bisher bereitgestellten Daten (Geometrie, Navigation, Materialien und die Einstellung für niedrige/hohe Auflösung) und durchläuft mehrere Schritte:

1. Zunächst wird die Geometrie aus dem Szenengittermodell verwendet und ein Voxelvolumen berechnet. Das Voxelvolumen ist ein dreidimensionales Volumen, das die gesamte Szene umfasst und aus kleinen, kubischen „Voxeln“ besteht. Die Größe der Voxels richtet sich nach der Häufigkeit der Simulation, die durch die Einstellung **Simulation Resolution** (Simulationsauflösung) festgelegt wird. Für jedes Voxel wird markiert, ob es „frei“ ist oder Szenengeometrie enthält. Wenn ein Voxel Geometrie enthält, wird es mit dem Absorptionskoeffizienten des Materials markiert, das dieser Geometrie zugewiesen ist.
2. Anschließend werden die Navigationsdaten verwendet, um die für die Akustik relevanten Positionen zu berechnen, an die der Spieler sich möglicherweise bewegt. Es wird versucht, kleine Gruppen dieser Positionen zu suchen, die kleinere Bereiche wie Eingänge oder Flure bis hin zu Räumen und offenen Flächen enthalten. Für kleine Szenen sind dies meist weniger als 100 Positionen, während große Szenen bis zu 1.000 enthalten können.
3. Für jede der endgültigen Listenerpositionen, für die eine Berechnung durchgeführt wird, werden einige Parameter bestimmt (z.B., wie „offen“ die Fläche ist oder die Größe des Raums, in dem sich die Fläche befindet).
4. Die Ergebnisse dieser Berechnungen werden an dem festgelegten Speicherort in Dateien gespeichert (siehe [Datendateien](#Data-Files) weiter unten).

Je nach Größe Ihrer Szene und der Geschwindigkeit Ihres Computers können die Berechnungen mehrere Minuten dauern.

Sobald die Berechnungen abgeschlossen wurden, können Sie eine Vorschau der Voxeldaten und der Position der Testpunkte anzeigen, um sicherzugehen, dass die Ergebnisse des Bake-Vorgangs gut sind. Ein ungültiges Navigationsgittermodell oder fehlende bzw. zusätzliche Geometrie sind in der Vorschau üblicherweise schnell zu erkennen, sodass Sie diese Fehler korrigieren können.


## <a name="debug-display"></a>Debugginganzeige

Nach Abschluss der Prüfpunktberechnung wird ein neuer Akteur namens **AcousticsDebugRenderer** im World Outliner dargestellt. Durch Aktivieren der Kontrollkästchen **Render Probes** (Prüfpunkte rendern) und **Render Voxels** (Voxel rendern) wird die Debugginganzeige im Editor-Viewport aktiviert.

![Screenshot mit dem Akteur für den Akustikdebugging-Renderer im Unreal-Editor](media/acoustics-debug-renderer.png)

Wenn in Ihrem Level keine Voxel oder Prüfpunkte überlagert werden, vergewissern Sie sich, dass das Echtzeitrendering im Viewport aktiviert ist.

![Screenshot der Option für das Echtzeitrendering in Unreal](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Voxels

Voxels werden im Szenenfenster als grüne Würfel um die beteiligte Geometrie angezeigt. Voxels, die nur „Luft“ enthalten, werden nicht angezeigt. Ihre gesamte Szene ist von einem großen grünen Kasten umschlossen, der das vollständige Voxelvolumen angibt, das während der Simulation verwendet wird.
Bewegen Sie die Szene, und vergewissern Sie sich, dass sämtliche Geometrie, die als akustische Verdeckung dient, Voxel aufweist. Vergewissern Sie sich außerdem, dass keine nicht akustischen Objekte wie Kollisionsgittermodelle als Voxel aufbereitet wurden. Die Szenenkamera muss sich in einem Radius von etwa 5 Metern um das Objekt befinden, damit die Voxels angezeigt werden.

Wenn Sie die erstellten Voxel mit niedriger und hoher Auflösung vergleichen, sehen Sie, dass die Voxel mit niedriger Auflösung doppelt so groß sind.

![Screenshot mit Vorschau der Akustik-Voxel im Unreal-Editor](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Testpunkte

Die Testpunkte entsprechen den möglichen Positionen des Spielers (Listeners). Beim Baking berechnet die Simulation die Akustik durch Verbinden aller möglichen Quellpositionen mit jedem Prüfpunkt. Zur Laufzeit wird die Zuhörerposition zwischen nahe gelegenen Prüfpunkten interpoliert.

Es ist wichtig, sicherzustellen, dass an jeder Position, die der Spieler in der Szene voraussichtlich durchlaufen wird, Prüfpunkte vorhanden sind. Prüfpunkte werden von der Projekt Akustik-Engine im Navigationsgittermodell platziert und können nicht verschoben oder bearbeitet werden. Stellen Sie daher sicher, dass das Navigationsgittermodell alle möglichen Spielerpositionen abdeckt, indem Sie die Prüfpunkte durchgehen.

![Screenshot einer Vorschau der Akustiktests in Unreal](media/unreal-probes-preview.png)

Ausführliche Informationen zur niedrigen und hohen Auflösung finden Sie unter [Bake Resolution](bake-resolution.md) (Bakingauflösung).

## <a name="bake-your-level-using-azure-batch"></a>Ausführen des Bakings Ihres Levels mit Azure Batch

Sie können das Baking Ihrer Szene mit dem Azure Batch-Dienst in einem Computecluster in der Cloud ausführen. Das Unreal-Plug-In für Projekt Akustik stellt eine direkte Verbindung mit Azure Batch her, um einen Azure Batch-Cluster für jeden Bakingvorgang zu instanziieren, zu verwalten und zu beenden. Geben Sie auf der Registerkarte „Bake“ Ihre Azure-Anmeldeinformationen ein, wählen Sie einen Clustercomputertyp und eine Größe aus, und klicken Sie auf „Bake“.

### <a name="for-reference-parts-of-the-bake-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Bake“

![Screenshot der Registerkarte „Acoustics Bake“ (Akustikbaking) in Unreal](media/unreal-bake-tab-details.png)

1. Die Schaltfläche Bake, die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie auf dieser Seite durchführen müssen.
3. Felder zum Eingeben Ihrer Azure-Anmeldeinformationen, sobald Ihr Azure-Konto erstellt wurde. Weitere Informationen finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
4. Starten Sie das Azure-Portal, um Ihre Abonnements zu verwalten, die Nutzung zu überwachen und Abrechnungsinformationen anzuzeigen usw. 
5. Der Azure Batch-Computeknotentyp, der für die Berechnung verwendet werden soll. Der Knotentyp muss vom Standort Ihres Azure-Rechenzentrums unterstützt werden. Wenn Sie sich nicht sicher sind, behalten Sie den Wert **Standard_F8s_v2** bei.
6. Die Anzahl der Knoten, die für diese Berechnung verwendet werden soll. Die Anzahl, die Sie hier eingeben, wirkt sich auf die Zeit aus, die für den Bake-Vorgang erforderlich ist, und wird von der Zuteilung Ihres Azure Batch-Kerns eingeschränkt. Die Standardzuteilung ermöglicht nur zwei Knoten mit acht Kernen oder einen Knoten mit 16 Kernen, kann jedoch erweitert werden. Weitere Informationen zu Einschränkungen bei der Kernzuteilung finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
7. Aktivieren Sie dieses Kontrollkästchen, um Ihren Computepool so zu konfigurieren, dass er [Knoten mit niedriger Priorität](https://docs.microsoft.com/azure/batch/batch-low-pri-vms) verwendet. Computeknoten mit niedriger Priorität sind deutlich kostengünstiger, aber sie sind möglicherweise nicht immer verfügbar oder können jederzeit vorzeitig entfernt werden.
8. Die Zeit, die voraussichtlich verstreicht, während Ihr Auftrag in der Cloud ausgeführt wird. Diese schließt die Startzeit der Knoten nicht ein. Sobald der Auftrag ausgeführt wird, bestimmt diese Zeit, wie lange die Ausführung dauern sollte, bis Sie die Ergebnisse erhalten. Beachten Sie, dass es sich dabei nur um eine Schätzung handelt.
9. Die gesamte Computezeit, die für die Ausführung der Simulationen erforderlich ist. Diese entspricht der Computezeit für die Knoten, die in Azure verwendet werden. Weitere Informationen zur Verwendung dieses Werts finden Sie im Verlauf dieses Artikels unter [Schätzen der Bake-Kosten](#Estimating-bake-cost).
10. Klicken Sie auf die Schaltfläche „Bake“, um den Bake-Vorgang an die Cloud zu übermitteln. Während ein Auftrag ausgeführt wird, zeigt diese stattdessen **Cancel Job** (Auftrag abbrechen) an. Wenn auf dieser Registerkarte Fehler angezeigt werden oder der Workflow auf der Registerkarte **Probes** (Prüfpunkte) nicht abgeschlossen wurde, ist diese Schaltfläche deaktiviert.
11. Die Anzahl der Tests für Ihre Szene, die auf der Registerkarte **Probe** (Tests) berechnet wurde. Die Anzahl der Tests bestimmt die Anzahl der Simulationen, die in der Cloud ausgeführt werden müssen. Sie können nicht mehr Knoten als Tests angeben.
12. Diese Meldung weist Sie auf den aktuellen Status des Auftrags hin, und wenn auf dieser Registerkarte Fehler vorhanden sind, zeigt sie diese Fehler an.

Vollständige Informationen zu aktiven Aufträgen, Computepools und Speicherplatz können Sie jederzeit im [Azure-Portal](https://portal.azure.com) abrufen.

Während ein Auftrag ausgeführt wird, ändert sich der Text der Schaltfläche **Bake** in **Cancel Job** (Auftrag abbrechen). Verwenden Sie diese Schaltfläche, um einen ausgeführten Auftrag abzubrechen. Das Abbrechen eines Auftrags kann nicht rückgängig gemacht werden, es sind keine Ergebnisse verfügbar, und die aufgewendete Azure-Computezeit vor dem Abbrechen wird Ihnen weiterhin in Rechnung gestellt.

Sobald Sie einen Bakingvorgang gestartet haben, können Sie Unreal schließen. Je nach Projekt, Knotentyp und Anzahl der Knoten kann ein Bake-Vorgang in die Cloud mehrere Stunden dauern. Der Status des Bake-Auftrags wird aktualisiert, wenn Sie das Projekt neu laden und das Acoustics-Fenster öffnen. Wenn der Auftrag abgeschlossen ist, wird die Ausgabedatei heruntergeladen.

Die Azure-Anmeldeinformationen werden sicher auf Ihrem lokalen Computer gespeichert und Ihrem Unreal-Projekt zugeordnet. Sie werden nur verwendet, um eine sichere Verbindung mit Azure herzustellen.

### <a name="Estimating-bake-cost"></a> Schätzen der Bake-Kosten für Azure

Wenn Sie einschätzen möchten, wie viel ein bestimmter Bake-Vorgang kosten wird, multiplizieren Sie den angezeigten Wert für **Estimated Compute Cost** (Geschätzte Computekosten) mit den stündlichen Kosten des ausgewählten **VM-Knotentyps** (in Ihrer lokalen Währung). Das Ergebnis umfasst nicht die Zeit, die erforderlich ist, um die Knoten bereit zu machen und auszuführen. Wenn Sie z. B. **Standard_F8s_v2** als Knotentyp ausgewählt haben (Kosten: 0,40 $/Stunde) und die geschätzte Computezeit 3 Stunden und 57 Minuten beträgt, betragen die geschätzten Kosten für die Ausführung des Auftrags „0,40 $ × ~4 h = ~1,60 $“. Die tatsächlichen Kosten sind aufgrund der zusätzlichen Zeit für das Starten der Knoten wahrscheinlich etwas höher. Die stündlichen Kosten für Knoten finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (klicken Sie auf die Kategorie „Computeoptimiert“ oder „High Performance Computing“).

### <a name="reviewing-the-bake-results"></a>Überprüfen der Bake-Ergebnisse

Überprüfen Sie nach dem Bake-Vorgang, ob die Voxels und Testpunkte sich an den erwarteten Positionen befinden, indem Sie das Runtime-Plug-In ausführen.

## <a name="Data-Files"></a>Datendateien

Es gibt vier Datendateien, die von diesem Plug-In an verschiedenen Speicherorten erstellt wurden. Nur eine davon ist zur Laufzeit erforderlich. Diese befindet sich im Ordner „Content/Acoustics“ Ihres Projekts, der dem Paketpfad des Projekts automatisch hinzugefügt wird. Die anderen drei befinden sich im Ordner „Acoustics Data“ und werden nicht in das Paket aufgenommen.

* **[Projekt]/Config/ProjectAcoustics.cfg:** In dieser Datei werden die Daten gespeichert, die Sie in die Felder auf der Akustikmodus-Benutzeroberfläche eingeben. Der Speicherort und der Name dieser Datei können nicht geändert werden. In dieser Datei werden weitere Werte gespeichert, die sich auf den Bake-Vorgang auswirken, diese sind jedoch für fortgeschrittene Benutzer gedacht und sollten nicht geändert werden.
* **[Projekt]/Content/Acoustics/[Levelname]\_AcousticsData.ace:** Diese Datei wurde während der Bake-Simulation erstellt und enthält die Suchdaten, die von der Runtime verwendet wurden, um die Akustik der Szene zu rendern. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden. Wenn Sie diese Datei nach der Erstellung umbenennen möchten, löschen Sie das UAsset aus Ihrem Unreal-Projekt, benennen Sie die Datei außerhalb von Unreal im Datei-Explorer um, und importieren Sie diese Datei dann erneut in Unreal, um ein neues UAsset zu erstellen. Die Umbenennung des UAssets selbst funktioniert nicht.
* **[Projekt]/Plugins/ProjectAcoustics/AcousticsData/[Levelname]\_AcousticsData.vox:** Diese Datei speichert die in Voxel konvertierte Akustikgeometrie und die Materialeigenschaften. Diese wurden mithilfe der Schaltfläche **Calculate** (Berechnen) auf der Registerkarte **Probes** (Prüfpunkte) berechnet. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.
* **[Projekt]/Plugins/ProjectAcoustics/AcousticsData/[Levelname]\_AcousticsData\_config.xml:** In dieser Datei werden die Parameter gespeichert, die mithilfe der Schaltfläche **Calculate** (Berechnen) auf der Registerkarte **Probes** (Prüfpunkte) berechnet wurden. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.

Achten Sie darauf, die von Azure heruntergeladene Datei „*.ace“ nicht zu löschen. Diese Datei kann nicht wiederhergestellt werden, sondern kann nur durch eine Wiederholung des Bake-Vorgangs erneut generiert werden.

## <a name="next-steps"></a>Nächste Schritte
* Erkunden der [Entwurfssteuerelemente für Unreal](unreal-workflow.md)
* Erkunden der [Entwurfskonzepte in Projekt Akustik](design-process.md)

