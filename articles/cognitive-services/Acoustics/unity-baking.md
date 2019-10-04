---
title: Unity-Bakingtutorial für Projekt Akustik
titlesuffix: Azure Cognitive Services
description: Dieses Tutorial beschreibt Akustikbaking mit Projekt Akustik in Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: b7249c3048ba3af3adbaac01f43770482a0d38ad
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933273"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Unity-Bakingtutorial für Projekt Akustik
Dieses Tutorial beschreibt Akustikbaking mit Projekt Akustik in Unity.

Softwareanforderungen:
* [Unity 2018.2 oder höher](https://unity3d.com) für Windows oder macOS
* [In das Unity-Projekt integriertes Projekt Akustik-Plug-In](unity-integration.md) oder [Projekt Akustik-Beispielinhalte für Unity](unity-quickstart.md)
* Optional: Ein [Azure Batch-Konto](create-azure-account.md) zum Beschleunigen des Bakings mithilfe von Cloudcomputing

## <a name="open-the-project-acoustics-bake-window"></a>Öffnen des Bakingfensters in Projekt Akustik
Wählen Sie im Unity-Menü **Window > Acoustics** (Fenster > Akustik) aus:

![Screenshot des Unity-Editors, in dem im Menü „Window“ (Fenster) die Option „Acoustics“ (Akustik) hervorgehoben ist](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Erstellen eines Navigationsgittermodells
Projekt Akustik verwendet ein Navigationsgittermodell, um Zuhörerprüfpunkte für die Simulation zu platzieren. Sie können den [Navigationsgittermodell-Workflow](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) von Unity verwenden oder mit einem anderen 3D-Modellierungspaket Ihr eigenes Gittermodell entwerfen. 

## <a name="mark-acoustic-scene-objects"></a>Markieren von akustischen Szenenobjekten
Projekt Akustik ist für die Simulation auf zwei Arten von Szenenobjekten angewiesen: die Objekte, die Sounds in der Simulation reflektieren und verdecken, und das Spielernavigations-Gittermodell, das die Zuhörerprüfpunkte in der Simulation einschränkt. Beide Objekttypen werden über die Registerkarte **Objects** (Objekte) markiert. 

Da beim Markieren von Objekten diesen einfach die Komponenten **AcousticsGeometry** oder **AcousticsNavigation** hinzugefügt werden, können Sie auch den [Unity-Standard-Komponentenworkflow](https://docs.unity3d.com/Manual/UsingComponents.html) zum Markieren oder Aufheben der Markierung von Objekten verwenden. Nur Gittermodellrenderer und Geländeobjekte können markiert werden. Alle anderen Objekttypen werden ignoriert. Über die Kontrollkästchen werden alle entsprechenden Objekte markiert (oder die Markierung wird aufgehoben).

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Markieren von Geometrie für akustische Verdeckung und Reflexion
Öffnen Sie die Registerkarte **Objects** des Fensters **Acoustics**. Markieren Sie alle Objekte als **Acoustics Geometry** (Akustikgeometrie), die Sounds verdecken, reflektieren oder absorbieren sollen. Objekte für die Akustikgeometrie können z. B. Boden, Wände, Dächer, Fenster, Fensterglas, Teppiche oder große Möbelstücke umfassen. Sie können für diese Objekte jede beliebige Komplexität verwenden. Da die Szene vor der Simulation in Voxel umgewandelt wird, ist das Baking für hochgradig detaillierte Gittermodelle wie Bäume mit vielen kleinen Blättern nicht aufwendiger als vereinfachte Objekte.

Fügen Sie keine Objekte hinzu, die sich nicht auf die Akustik auswirken sollen, z. B. unsichtbare Gittermodelle für Kollisionen.

Die Transformation eines Objekts zum Zeitpunkt der Testberechnung (über die Registerkarte **Probes** (Tests)) wird in den Ergebnissen des Bake-Vorgangs festgelegt. Wenn Sie eines der markierten Objekte in der Szene bewegen, müssen Sie die Testberechnung und den Bake-Vorgang für die Szene wiederholen.

### <a name="mark-the-navigation-mesh"></a>Markieren des Navigationsgittermodells
Navigationsgittermodelle, die mit dem Unity-Workflow erstellt werden, werden vom Akustiksystem übernommen. Wenn Sie eigene Gittermodelle verwenden möchten, markieren Sie diese auf der Registerkarte **Objects** (Objekte).

### <a name="for-reference-the-objects-tab-parts"></a>Zur Referenz: Die Elemente der Registerkarte „Objects“ (Objekte)
Die Registerkartenseite weist folgende Bestandteile auf:

1. Schaltflächen für die Registerkartenauswahl (Registerkarte **Objects** ist ausgewählt). Verwenden Sie diese Schaltflächen, um die Schritte eines Bake-Vorgangs für Akustik von links nach rechts zu durchlaufen.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verfügbare Filter für das Hierarchiefenster. Verwenden Sie diese, um das Hierarchiefenster nach Objekten des angegebenen Typs zu filtern, um diese einfacher markieren zu können. Wenn Sie bisher nichts für Akustik ausgewählt haben, wird nichts angezeigt, wenn Sie die letzten beiden Optionen auswählen. Diese können jedoch nützlich sein, um bei Bedarf markierte Objekte zu suchen.
4. Wenn keine Objekte ausgewählt sind, zeigt dieser Abschnitt den Status aller Objekte in der Szene an:
    * Total (Gesamt): Die Gesamtanzahl der aktiven, nicht ausgeblendeten Objekte in der Szene.
    * Ignored (Ignoriert): Die Anzahl der Objekte, die keine Gittermodellrenderer oder Geländeobjekte sind.
    * Mesh (Gittermodell): Die Anzahl der Gittermodellrendererobjekte in der Szene
    * Terrain (Gelände): Die Anzahl der Geländeobjekte in der Szene
    * Geometry (Geometrie): Die Anzahl der Gittermodell- oder Geländeobjekte in der Szene, die als „Acoustics Geometry“ (Akustikgeometrie) markiert sind.
    * Navigation: Die Anzahl der Gittermodell- oder Geländeobjekte in der Szene, die als „Acoustics Navigation“ (Akustiknavigation) markiert sind. In dieser Anzahl ist das Navigationsgittermodell von Unity (NavMesh) nicht eingeschlossen.
5. Die Gesamtanzahl der „markierbaren“ Objekte in der Szene (nur Gittermodellrenderer und Geländeobjekte). Die Kontrollkästchen, die Sie zum Markieren (d.h. zum Hinzufügen der entsprechenden Komponente) dieser Objekte als „Geometrie“ oder „Navigation“ für Akustik verwenden können.
6. Wenn keine Objekte ausgewählt sind, erinnert dieser Hinweis Sie daran, bei Bedarf Objekte zu markieren. Sie können ebenfalls ein Kontrollkästchen (oder beide) aktivieren, um alle Objekte in der Szene zu markieren, ohne eine Auswahl vorzunehmen.
7. Wenn Objekte ausgewählt sind, zeigt dieser Abschnitt nur den Status der ausgewählten Objekte an.
8. Die Gesamtanzahl der „markierbaren“ ausgewählten Objekte. Durch das Aktivieren oder Deaktivieren der Kontrollkästchen werden nur die ausgewählten Objekte markiert (bzw. die Markierung wird aufgehoben).

Wenn in Ihrer Szene nichts ausgewählt ist, sieht die Registerkarte „Objects“ (Objekte) wie folgt aus:

![Screenshot der Option „Objects“ (Objekte) auf der Registerkarte „Acoustics“ (Akustik) ohne ausgewählte Elemente](media/objects-tab-no-selection-detail.png)

Wenn in Ihrem Szenen- oder Hierarchiefenster Elemente ausgewählt sind, sehen diese folgendermaßen aus:

![Screenshot der Option „Objects“ (Objekte) auf der Registerkarte „Acoustics“ (Akustik) mit ausgewählten Elementen](media/objects-tab-selection-detail.png)

Wenn einige Objekte markiert sind und einige nicht, zeigt das entsprechende Kontrollkästchen einen „gemischten“ Wert an:

![Screenshot der Option „Objects“ (Objekte) auf der Registerkarte „Acoustics“ (Akustik), in dem das Symbol für die gemischte Auswahl hervorgehoben ist](media/mixed-object-selection-detail.png)

Wenn Sie das Kontrollkästchen aktivieren, werden alle Objekte markiert. Wenn Sie es deaktivieren, wird die Markierung für alle Objekte aufgehoben.

Objekte können für Geometrie und Navigation markiert werden.

## <a name="select-acoustic-materials"></a>Auswählen akustischer Materialien
Wenn Ihre Objekte markiert sind, klicken Sie auf die Schaltfläche **Materials** (Materialien), und weisen Sie akustische Materialien zu. Das Materialiensystem von Projekt Akustik ist an das visuelle Materialiensystem von Unity gebunden: Damit zwei Objekte separate akustische Materialien erhalten können, müssen sie separate visuelle Materialien aufweisen.

Die akustischen Materialien bestimmten die Menge an Schallenergie, die von der Oberfläche reflektiert wird. Das akustische Standardmaterial weist eine Absorption ähnlich der von Zement auf. Projekt Akustik schlägt die Materialien basierend auf dem Namen des visuellen Materials vor. Sie können einem Material das akustische Material „Custom“ (Benutzerdefiniert) zuweisen, um einen Schieberegler für den Absorptionskoeffizienten zu aktivieren.

Die Nachhallzeit eines bestimmten Materials in einem Raum ist umgekehrt proportional zum Absorptionskoeffizienten, wobei die meisten Materialien Absorptionswerte im Bereich zwischen 0,01 und 0,20 aufweisen. Materialien mit Absorptionskoeffizienten außerhalb dieses Bereichs sind extrem absorbierend.

![Graph, der die negative Korrelation der Nachhallzeit mit dem Absorptionskoeffizienten zeigt](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Materials“ (Materialien)
![Screenshot der Akustikregisterkarte „Materials“ (Materialien) in Unity](media/materials-tab-detail.png)

1. Die Schaltfläche **Materials** (Materialien), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Wenn dieses Kontrollkästchen aktiviert ist, werden nur Materialien aufgeführt, die von Objekten verwendet werden, die als **Acoustics Geometry** (Akustikgeometrie) markiert sind. Andernfalls werden alle Materialien aufgeführt, die in dieser Szene verwendet werden.
4. Verwenden Sie diese Optionen, um die Reihenfolge des Dropdownmenüs zu ändern, das angezeigt wird, wenn Sie auf einen Dropdownpfeil in der unten dargestellten Acoustics-Spalte (6) klicken. **Name** sortiert die Akustikmaterialien nach Name. „Absorptivity“ (Absorptionsgrad) sortiert diese aufsteigend nach Absorptionsgrad.
5. Die Liste der in dieser Szene verwendeten Materialien (alphabetisch sortiert). Wenn das Kontrollkästchen **Show Marked Only** (Nur markierte anzeigen) (3) aktiviert ist, werden nur Materialien angezeigt, die von Objekten verwendet werden, die als **Acoustics Geometry** (Akustikgeometrie) markiert sind. Wenn Sie hier auf eines der Materialien klicken, werden alle Objekte in der Szene ausgewählt, die dieses Material verwenden.
6. Anzeige der Akustikmaterialien, denen die Szenenmaterialien zugewiesen wurden. Klicken Sie auf eine Dropdownliste, um ein Szenenmaterial einem anderen Akustikmaterial zuzuweisen. Sie können die Sortierreihenfolge des anzeigten Menüs ändern, wenn Sie mithilfe der Optionen **Sort Acoustics By:** (Akustik sortieren nach:) (4) auf ein Element klicken.
7. Zeigt den Absorptionskoeffizienten des ausgewählten Akustikmaterials an, das in der vorherigen Spalte ausgewählt wurde. Ein Wert von „0“ bedeutet „perfekte Reflektion“ (keine Absorption), während ein Wert von „1“ „perfekte Absorption“ (keine Reflektion) bedeutet. Der Absorptionskoeffizient kann nicht geändert werden, sofern das ausgewählte Material nicht vom Typ „Custom“ (Benutzerdefiniert) ist.
8. Für ein auf „Custom“ (Benutzerdefiniert) festgelegtes Material ist der Schieberegler nicht mehr deaktiviert, und Sie können den Absorptionskoeffizienten mithilfe des Schiebereglers oder durch Eingeben eines Werts auswählen.

## <a name="calculate-and-review-listener-probe-locations"></a>Berechnen und Überprüfen der Zuhörer-Prüfpunktpositionen
Wechseln Sie nach dem Zuweisen der Materialien zur Registerkarte **Probes** (Prüfpunkte), und klicken Sie auf **Calculate** (Berechnen), um Zuhörerprüfpunkte für die Simulation zu platzieren.

### <a name="what-the-calculate-button-calculates"></a>Berechnungen der Schaltfläche „Calculate...“ (Berechnen...)
Die Schaltfläche **Calculate** (Berechnen) bereitet Ihre Szene mithilfe Ihrer ausgewählten akustischen Szenengeometrie für die Simulation vor:

1. Zunächst wird die Geometrie aus dem Szenengittermodell verwendet und ein Voxelvolumen berechnet. Das Voxelvolumen ist ein dreidimensionales Volumen, das die gesamte Szene umfasst und aus kleinen, kubischen „Voxeln“ besteht. Die Größe der Voxels richtet sich nach der Häufigkeit der Simulation, die durch die Einstellung **Simulation Resolution** (Simulationsauflösung) festgelegt wird. Für jedes Voxel wird markiert, ob es „frei“ ist oder Szenengeometrie enthält. Wenn ein Voxel Geometrie enthält, wird es mit dem Absorptionskoeffizienten des Materials markiert, das dieser Geometrie zugewiesen ist.
2. Die Zuhörerprüfpunkte werden mithilfe der Navigationsgittermodelle platziert. Der Algorithmus gleicht die konkurrierenden Aspekte der räumlichen Abdeckung und der Simulationszeit sowie Dateigröße ab, wobei sichergestellt wird, dass schmale Korridore und kleine Räume gleichermaßen abgedeckt werden. Typische Prüfpunktanzahlen liegen zwischen 100 für kleine Szenen und einigen Tausend für große Szenen.

Je nach Größe Ihrer Szene und der Geschwindigkeit Ihres Computers können die Berechnungen mehrere Minuten dauern.

### <a name="review-voxel-and-probe-placement"></a>Überprüfen der Voxel und der Prüfpunktplatzierung
Zeigen Sie eine Vorschau der Datenvoxel und der Prüfpunktpositionen an, um sicherzustellen, dass Sie für das Baking der Szene bereit sind. Ein unvollständiges Navigationgittermodell oder fehlende bzw. zusätzliche akustische Geometrie sind in der Regel in der Vorschau schnell ersichtlich. Voxeldaten und Prüfpunktpositionen können über das Menü „Gizmos“ aktiviert oder deaktiviert werden:

![Screenshot des Gizmos-Menüs in Unity](media/gizmos-menu.png)

Voxel mit akustischer Geometrie werden als grüne Würfel angezeigt. Erkunden Sie die Szene, und stellen Sie sicher, dass alle Geometrieobjekte Voxel enthalten. Die Szenenkamera muss sich in einem Radius von etwa 5 Metern um das Objekt befinden, damit die Voxels angezeigt werden.

Wenn Sie die erstellten Voxel mit niedriger und hoher Auflösung vergleichen, sehen Sie, dass die Voxel mit niedriger Auflösung doppelt so groß sind.

![Screenshot mit Vorschau der Voxel mit niedriger Auflösung im Unity-Editor](media/voxel-cubes-preview.png)

Die Simulationsergebnisse werden zur Laufzeit zwischen den Positionen der Zuhörerprüfpunkte interpoliert. Überprüfen Sie die Prüfpunkte in der Nähe aller Orte, an denen sich der Spieler voraussichtlich durch die Szene bewegen wird.

![Screenshot mit Vorschau der Tests im Unity-Editor](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Vorsicht bei der Umbenennung von Szenen
Der Szenenname wird verwendet, um die Szene mit den Dateien zu verbinden, in denen die Position und die Voxels des Testpunkts gespeichert sind. Wenn die Szene nach der Berechnung der Testpunkte umbenannt wird, gehen die Zuweisungs- und Platzierungsdaten des Materials verloren, und die Berechnung sollte erneut ausgeführt werden.

### <a name="for-reference-parts-of-the-probes-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Probes“ (Tests)
![Screenshot der Akustikregisterkarte „Probes“ (Tests) in Unity](media/probes-tab-detail.png)

1. Die Schaltfläche **Probes** (Tests), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verwenden Sie diese Kontrollkästchen, um „Coarse“ (Niedrig) oder „Fine“ (Hoch) als Auflösung für die Simulation auszuwählen. Eine niedrige Auflösung ist schneller, hat jedoch einige Nachteile. Ausführliche Informationen finden Sie unter [Bake Resolution](bake-resolution.md) (Bakingauflösung).
4. Wählen Sie mithilfe dieses Felds den Speicherort für die Akustikdatendateien aus. Klicken Sie auf die Schaltfläche mit den Auslassungspunkten (...), um die Ordnerauswahl zu verwenden. Der Standardordner ist **Assets/AcousticsData**. An diesem Speicherort wird ebenfalls ein **Editor**-Unterordner erstellt. Weitere Informationen zu Datendateien finden Sie weiter unten unter [Datendateien](#Data-Files).
5. Die Datendateien für diese Szene werden mithilfe des hier bereitgestellten Präfixes benannt. Der Standardname ist „Acoustics_[Scene Name]“.
6. Nachdem die Tests berechnet wurden, werden die oben genannten Steuerelemente deaktiviert. Klicken Sie auf die Schaltfläche **Clear** (Löschen), um die Berechnungen zu löschen und die Steuerelemente zu aktivieren, damit Sie unter Verwendung der neuen Einstellungen eine Neuberechnung vornehmen können.
7. Klicken Sie auf die Schaltfläche **Calculate...** (Berechnen...), um die Szene in Voxels zu konvertieren und die Positionen der Testpunkte zu berechnen. Dies wird lokal auf Ihrem Computer durchgeführt und muss daher vor einem Bake-Vorgang erfolgen.

In dieser Version von Project Acoustics können Tests nicht manuell platziert werden, sondern müssen über einen automatisierten Prozess platziert werden, der auf der Registerkarte **Probes** (Tests) bereitgestellt wird.

Ausführliche Informationen zur niedrigen und hohen Auflösung finden Sie unter [Bake Resolution](bake-resolution.md) (Bakingauflösung).

## <a name="bake-your-scene-using-azure-batch"></a>Ausführen des Bakings Ihrer Szene mit Azure Batch
Sie können das Baking Ihrer Szene mit dem Azure Batch-Dienst in einem Computecluster in der Cloud ausführen. Das Unity-Plug-In für Projekt Akustik stellt eine direkte Verbindung mit Azure Batch her, um einen Azure Batch-Cluster für jeden Bakingvorgang zu instanziieren, zu verwalten und zu beenden. Geben Sie auf der Registerkarte **Bake** Ihre Azure-Anmeldeinformationen ein, wählen Sie einen Clustercomputertyp und eine Größe aus, und klicken Sie auf **Bake**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Bake“
![Screenshot der Registerkarte „Acoustics Bake“ (Akustikbaking) in Unity](media/bake-tab-details.png)

1. Die Schaltfläche Bake, die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie auf dieser Seite durchführen müssen.
3. Felder zum Eingeben Ihrer Azure-Anmeldeinformationen, sobald Ihr Azure-Konto erstellt wurde. Weitere Informationen finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
4. Docker-Imagetag für das Akustiktoolset.
5. Starten Sie das Azure-Portal, um Ihre Abonnements zu verwalten, die Nutzung zu überwachen und Abrechnungsinformationen anzuzeigen usw. 
6. Der Azure Batch-Computeknotentyp, der für die Berechnung verwendet werden soll. Der Knotentyp muss vom Standort Ihres Azure-Rechenzentrums unterstützt werden. Wenn Sie sich nicht sicher sind, behalten Sie den Wert **Standard_F8s_v2** bei.
7. Die Anzahl der Knoten, die für diese Berechnung verwendet werden soll. Die Anzahl, die Sie hier eingeben, wirkt sich auf die Zeit aus, die für den Bake-Vorgang erforderlich ist, und wird von der Zuteilung Ihres Azure Batch-Kerns eingeschränkt. Die Standardzuteilung ermöglicht nur zwei Knoten mit acht Kernen oder einen Knoten mit 16 Kernen, kann jedoch erweitert werden. Weitere Informationen zu Einschränkungen bei der Kernzuteilung finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
8. Aktivieren Sie dieses Kontrollkästchen, um Ihren Computepool so zu konfigurieren, dass er [Knoten mit niedriger Priorität](https://docs.microsoft.com/azure/batch/batch-low-pri-vms) verwendet. Computeknoten mit niedriger Priorität sind deutlich kostengünstiger, aber sie sind möglicherweise nicht immer verfügbar oder können jederzeit vorzeitig entfernt werden.
9. Die Anzahl der Tests für Ihre Szene, die auf der Registerkarte **Probe** (Tests) berechnet wurde. Die Anzahl der Tests bestimmt die Anzahl der Simulationen, die in der Cloud ausgeführt werden müssen. Sie können nicht mehr Knoten als Tests angeben.
10. Die Zeit, die voraussichtlich verstreicht, während Ihr Auftrag in der Cloud ausgeführt wird. Diese schließt die Startzeit der Knoten nicht ein. Sobald der Auftrag ausgeführt wird, bestimmt diese Zeit, wie lange die Ausführung dauern sollte, bis Sie die Ergebnisse erhalten. Beachten Sie, dass es sich dabei nur um eine Schätzung handelt.
11. Die gesamte Computezeit, die für die Ausführung der Simulationen erforderlich ist. Diese entspricht der Computezeit für die Knoten, die in Azure verwendet werden. Weitere Informationen zur Verwendung dieses Werts finden Sie im Verlauf dieses Artikels unter [Schätzen der Bake-Kosten](#Estimating-bake-cost).
12. Diese Meldung teilt Ihnen mit, wo die Ergebnisse des Bake-Vorgangs nach Abschluss des Auftrags gespeichert werden.
13. (Nur für fortgeschrittene Benutzer) Wenn es erforderlich ist, dass Unity einen übermittelten Bake-Vorgang „vergisst“ (z.B., wenn Sie die Ergebnisse mit einem anderen Computer heruntergeladen haben), klicken Sie auf die Schaltfläche **Clear State** (Status löschen), um den übermittelten Auftrag zu vergessen. Das bedeutet, dass die Ergebnisdatei **nicht** heruntergeladen wird, wenn diese bereit ist, und dass dies **nicht dem Abbrechen des Auftrags entspricht**. Wenn der Auftrag bereits ausgeführt wird, wird er weiterhin in der Cloud ausgeführt.
14. Klicken Sie auf die Schaltfläche **Bake**, um den Bakingvorgang an die Cloud zu übermitteln. Während ein Auftrag ausgeführt wird, zeigt diese stattdessen **Cancel Job** (Auftrag abbrechen) an.
15. Die Verarbeitung der [Akustiksimulation auf dem PC](#Local-bake) wird vorbereitet.
16. Dieser Bereich zeigt den Status des Bake-Vorgangs an. Wenn dieser abgeschlossen ist, sollte dieser **Downloaded** (Heruntergeladen) anzeigen.

Vollständige Informationen zu aktiven Aufträgen, Computepools und Speicherplatz können Sie jederzeit im [Azure-Portal](https://portal.azure.com) abrufen.

Während ein Auftrag ausgeführt wird, ändert sich der Text der Schaltfläche **Bake** in **Cancel Job** (Auftrag abbrechen). Verwenden Sie diese Schaltfläche, um einen ausgeführten Auftrag abzubrechen. Sie werden zur Bestätigung aufgefordert, bevor der Auftrag abgebrochen wird. Das Abbrechen eines Auftrags kann nicht rückgängig gemacht werden, es sind keine Ergebnisse verfügbar, und die verwendete Azure-Computezeit wird dennoch in Rechnung gestellt.

Sobald Sie einen Bake-Vorgang gestartet haben, können Sie Unity schließen. Je nach Projekt, Knotentyp und Anzahl der Knoten kann ein Bake-Vorgang in die Cloud mehrere Stunden dauern. Der Status des Bake-Auftrags wird aktualisiert, wenn Sie das Projekt neu laden und das Acoustics-Fenster öffnen. Wenn der Auftrag abgeschlossen ist, wird die Ausgabedatei heruntergeladen.

Die Azure-Anmeldeinformationen werden sicher auf Ihrem lokalen Computer gespeichert und Ihrem Unity-Editor zugeordnet. Sie werden nur verwendet, um eine sichere Verbindung mit Azure herzustellen.

## <a name="to-find-the-status-of-a-running-job-on-the-azure-portal"></a>So finden Sie den Status eines ausgeführten Auftrags im Azure-Portal

1. Suchen Sie die ID des Bakingauftrags auf der Registerkarte „Baking“:

![Screenshot der ID des Unity-Bakingauftrags](media/unity-job-id.png)  

2. Öffnen Sie das [Azure-Portal](https://portal.azure.com), navigieren Sie zu dem Batch-Konto, das für das Baking verwendet wird, und klicken Sie auf **Aufträge**.

![Screenshot des Links „Aufträge“](media/azure-batch-jobs.png)  

3. Suchen Sie in der Auftragsliste nach der Auftrags-ID.

![Screenshot des Status des Bakingauftrags](media/azure-bake-job-status.png)  

4. Klicken Sie auf die Auftrags-ID, um den Status der zugehörigen Aufgaben und den Status des Gesamtauftrags anzuzeigen.

![Screenshot des Status der Bakingaufgabe](media/azure-batch-task-state.png)  


### <a name="Estimating-bake-cost"></a> Schätzen der Bake-Kosten für Azure

Wenn Sie einschätzen möchten, wie viel ein bestimmter Bake-Vorgang kosten wird, multiplizieren Sie den angezeigten Wert für **Estimated Compute Cost** (Geschätzte Computekosten) mit den stündlichen Kosten des ausgewählten **VM-Knotentyps** (in Ihrer lokalen Währung). Das Ergebnis umfasst nicht die Zeit, die erforderlich ist, um die Knoten bereit zu machen und auszuführen. Wenn Sie z. B. **Standard_F8s_v2** als Knotentyp ausgewählt haben (Kosten: 0,40 $/Stunde) und die geschätzte Computezeit 3 Stunden und 57 Minuten beträgt, betragen die geschätzten Kosten für die Ausführung des Auftrags „0,40 $ × ~4 h = ~1,60 $“. Die tatsächlichen Kosten sind aufgrund der zusätzlichen Zeit für das Starten der Knoten wahrscheinlich etwas höher. Die stündlichen Kosten für Knoten finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (klicken Sie auf die Kategorie „Computeoptimiert“ oder „High Performance Computing“).

## <a name="Local-bake"></a> Baking Ihrer Szene auf Ihrem PC
Sie können das Baking Ihrer Szene auf Ihrem PC ausführen. Dies kann nützlich sein, um in kleinen Szenen mit der Akustik zu experimentieren, bevor Sie ein Azure Batch-Konto erstellen. Beachten Sie, dass die Akustiksimulation je nach der Größe der Szene sehr lange dauern kann.

### <a name="minimum-hardware-requirements"></a>Minimale Hardwareanforderungen
* Ein x86-64-Prozessor mit mindestens 8 Prozessorkernen und 32 GB RAM
* [Hyper-V-aktiviert](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) zur Ausführung von Docker

Beispiel unserer Tests: Computer mit Intel Xeon-Prozessor E5-1660 mit 8 Prozessorkernen bei 3 GHz und 32 GB RAM.
* Hier nimmt eine kleine Szene mit 100 Prüfpunkten etwa 2 Stunden für einen einfachen Bakingvorgang und etwa 32 Stunden für einen Bakingvorgang mit hoher Auflösung in Anspruch.
* Eine mittelgroße Szene mit 1.000 Prüfpunkten nimmt etwa 20 Stunden für einen einfachen Bakingvorgang und 21 Tage für einen Bakingvorgang mit hoher Auflösung in Anspruch.

### <a name="setup-docker"></a>Einrichten von Docker
Installieren und Konfigurieren von Docker auf dem PC, auf dem die Simulation verarbeitet werden soll –
1. Installieren Sie den [Docker-Desktop](https://www.docker.com/products/docker-desktop).
2. Starten Sie die Docker-Einstellungen, navigieren Sie zu den Optionen unter „Erweitert“, und konfigurieren Sie die Ressourcen so, dass mindestens 8 GB RAM vorhanden sind. Je mehr CPUs Sie Docker zuweisen können, desto schneller wird der Bake-Vorgang abgeschlossen.  
![Screenshot mit Beispiel zu Docker-Einstellungen](media/docker-settings.png)
1. Navigieren Sie zu „Freigegebene Laufwerke“ und aktivieren Sie die Freigabe für das Laufwerk, das für die Verarbeitung verwendet wird.  
![Screenshot mit Docker-Optionen für freigegebene Laufwerke](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Ausführen eines lokalen Bake-Vorgangs
1. Klicken Sie auf der Registerkarte **Bake** auf die Schaltfläche „Prepare Local Bake“ (Lokales Baking vorbereiten), und wählen Sie einen Ordner aus, in dem die Eingabedateien und Ausführungsskripts gespeichert werden. Sie können den Bake-Vorgang dann auf jedem beliebigen Computer ausführen, solange er die minimalen Hardwareanforderungen erfüllt und Docker installiert ist, indem Sie den Ordner auf diesen Computer kopieren.
2. Starten Sie die Simulation unter Windows mit dem Skript „runlocalbake.bat“ oder unter macOS mit dem Skript „runlocalbake.sh“. Dieses Skript ruft das Project Acoustics-Docker-Image mit dem für die Simulationsbearbeitung notwendigen Toolset ab und startet die Simulation. 
3. Nach Abschluss der Simulation kopieren Sie die resultierende ACE-Datei zurück in Ihr Unity-Projekt. Um sicherzustellen, dass Unity dies als Binärdatei erkennt, fügen Sie „.bytes“ an die Dateiendung an (z. B. „Scene1.ace.bytes“). Die ausführlichen Protokolle für die Simulation werden in der Datei „AcousticsLog.txt“ gespeichert. Wenn Probleme auftreten, teilen Sie diese Datei, um die Diagnose zu erleichtern.

## <a name="Data-Files"></a> Durch den Bakingvorgang hinzugefügte Datendateien

Während des Bakingvorgangs werden vier Datendateien erstellt. Eine enthält die Simulationsergebnisse und wird mit Ihrem Titel erstellt. Die anderen enthalten die zugehörigen Daten des Unity-Editors.

Simulationsergebnis:
* **Assets/AcousticsData/Acoustics\_[Szenenname].ace.bytes:** Dies ist die Nachschlagetabelle für die Laufzeit, die die Simulationsergebnisse und die als Voxel aufbereiteten Akustikszenenelemente enthält. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.

Achten Sie darauf, die Simulationsergebnisdatei nicht zu löschen. Sie kann nicht wiederhergestellt werden, sondern nur durch eine Wiederholung des Bakingvorgangs erneut generiert werden.

Editor-Datendateien:
* **Assets/Editor/[Szenenname]\_AcousticsParameters.asset:** Diese Datei speichert die Daten, die Sie in die Felder auf der Acoustics-Benutzeroberfläche eingeben. Der Speicherort und der Name dieser Datei können nicht geändert werden.
* **Assets/AcousticsData/Editor/Acoustics_[Szenenname].vox:** In dieser Datei werden die als Voxel aufbereitete Akustikgeometrie und die Materialeigenschaften gespeichert, die mithilfe der Schaltfläche **Calculate** (Berechnen) auf der Registerkarte „Probes“ (Prüfpunkte) berechnet werden. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.
* **Assets/AcousticsData/Editor/Acoustics\_[Szenenname]\_config.xml:** In dieser Datei werden die Simulationsparameter gespeichert, die mithilfe der Schaltfläche **Calculate** (Berechnen) auf der Registerkarte **Probes** (Prüfpunkte) berechnet wurden. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.

## <a name="set-up-the-acoustics-lookup-table"></a>Einrichten der Akustik-Nachschlagetabelle
Ziehen Sie das Prefab **Project Acoustics** (Projekt Akustik) per Drag & Drop aus dem Projektbereich in Ihre Szene:

![Screenshot mit Akustikprefab in Unity](media/acoustics-prefab.png)

Klicken Sie auf das Spielobjekt **ProjectAcoustics**, und wechseln Sie zum zugehörigen Inspectorbereich. Geben Sie den Speicherort Ihres Bakeergebnisses (die ACE-Datei unter **Assets/AcousticsData**) an, indem Sie die Datei per Drag & Drop in das Skript „Acoustics Manager“ (Akustikverwalter) ziehen oder auf den Kreis neben dem Textfeld klicken.

![Screenshot mit Akustikverwalter-Prefab in Unity](media/acoustics-manager.png)  

## <a name="next-steps"></a>Nächste Schritte
* Erkunden der [Entwurfssteuerelemente für Unity](unity-workflow.md)
* Erkunden der [Entwurfskonzepte in Projekt Akustik](design-process.md)

