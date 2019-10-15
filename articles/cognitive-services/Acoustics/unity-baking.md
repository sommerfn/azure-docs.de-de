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
ms.openlocfilehash: 310decf8053ea16ba46250ba3aabe81c9c254e5e
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243121"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Unity-Bakingtutorial für Projekt Akustik
In diesem Tutorial wird das Akustikbaking anhand von Projekt Akustik in Unity beschrieben.

Softwareanforderungen:
* [Unity 2018.2 oder höher](https://unity3d.com) für Windows oder macOS
* [In das Unity-Projekt integriertes Projekt Akustik-Plug-In](unity-integration.md) oder [Projekt Akustik-Beispielinhalte für Unity](unity-quickstart.md)
* *Optional*: Ein [Azure Batch-Konto](create-azure-account.md) zum Beschleunigen des Bakings per Cloud Computing

## <a name="open-the-project-acoustics-bake-window"></a>Öffnen des Bakingfensters in Projekt Akustik
Wählen Sie in Unity im Menü **Window** (Fenster) die Option **Acoustics** (Akustik) aus.

![Unity-Editor mit hervorgehobener Option „Acoustics“ (Akustik) im Menü „Window“ (Fenster)](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Erstellen eines Navigationsgittermodells
Projekt Akustik verwendet ein Navigationsgittermodell, um Zuhörerprüfpunkte für die Simulation zu platzieren. Sie können den [Navigationsgittermodell-Workflow](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) von Unity verwenden. Alternativ können Sie ein anderes Paket für die 3D-Modellierung verwenden, um Ihr eigenes Gitter zu entwerfen.

## <a name="mark-acoustic-scene-objects"></a>Markieren von akustischen Szenenobjekten
Für Projekt Akustik werden zwei Arten von Szenenobjekten für die Simulation verwendet:
- Die Objekte, die Sound in der Simulation reflektieren und verdecken (Okklusion)
- Das Player-Navigationsgitter, durch das in der Simulation Zuhörerprüfpunkte eingeschränkt werden

Beide Objekttypen werden über die Registerkarte **Objects** (Objekte) markiert.

Da beim Markieren von Objekten diesen einfach die Komponenten *AcousticsGeometry* oder *AcousticsNavigation* hinzugefügt werden, können Sie auch den [Unity-Standard-Komponentenworkflow](https://docs.unity3d.com/Manual/UsingComponents.html) zum Markieren oder Aufheben der Markierung von Objekten verwenden. Sie können nur Gittermodellrenderer und Geländeobjekte markieren. Alle anderen Objekttypen werden ignoriert. Mit den Kontrollkästchen können alle betroffenen Objekte markiert bzw. nicht markiert werden.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Markieren von Geometrie für akustische Verdeckung und Reflexion
Öffnen Sie die Registerkarte **Objects** des Fensters **Acoustics**. Markieren Sie alle Objekte als *Acoustics Geometry* (Akustikgeometrie), die Sounds verdecken, reflektieren oder absorbieren sollen. Objekte für die Akustikgeometrie können z. B. Boden, Wände, Dächer, Fenster/Fensterglas, Teppiche oder große Möbelstücke umfassen. Sie können für diese Objekte jede beliebige Komplexität verwenden. Da die Szene vor der Simulation in Voxel umgewandelt wird, ist das Baking für hochgradig detaillierte Gittermodelle wie Bäume mit vielen Blättern nicht aufwendiger als für vereinfachte Objekte.

Fügen Sie keine Objekte hinzu, die sich nicht auf die Akustik auswirken sollen, z. B. unsichtbare Gittermodelle für Kollisionen.

Die Transformation eines Objekts während der Prüfungsberechnung (über die Registerkarte **Probes** (Prüfpunkte)) wird in den Ergebnissen des Bakingvorgangs festgehalten. Falls in der Szene markierte Objekte später verschoben werden, müssen die Prüfungsberechnung und der Bakingvorgang wiederholt werden.

### <a name="mark-the-navigation-mesh"></a>Markieren des Navigationsgittermodells
Navigationsgittermodelle, die mit dem Unity-Workflow erstellt wurden, werden vom Akustiksystem verwendet. Wenn Sie eigene Gittermodelle verwenden möchten, markieren Sie diese auf der Registerkarte **Objects** (Objekte).

### <a name="for-reference-the-objects-tab-parts"></a>Zur Referenz: Die Elemente der Registerkarte „Objects“ (Objekte)
Die Registerkarte (unten nach den Beschreibungen abgebildet) verfügt über die folgenden Teile:

1. Schaltflächen für die Registerkartenauswahl (Registerkarte **Objects** (Objekte) ist ausgewählt). Verwenden Sie diese Schaltflächen, um die Schritte eines Bakingvorgangs von links nach rechts zu durchlaufen.
1. Eine kurze Beschreibung der Optionen, die Ihnen mit dieser Registerkarte zur Verfügung stehen.
1. Verfügbare Filter für das Hierarchiefenster. Verwenden Sie diese Optionen, um das Hierarchiefenster nach Objekten des angegebenen Typs zu filtern, damit Sie diese leicht markieren können. Falls Sie bisher keine Elemente für Akustik markiert haben, wird nichts angezeigt, wenn Sie die letzten beiden Optionen auswählen. Mit diesen Optionen können Sie aber Objekte ermitteln, nachdem sie markiert wurden.
1. Wenn keine Objekte ausgewählt sind, zeigt dieser Abschnitt den Status aller Objekte in der Szene an.
    * Gesamt: Die Gesamtzahl von aktiven, nicht ausgeblendeten Objekten.
    * Ignored (Ignoriert): Die Anzahl von Objekten, die keine Gittermodellrenderer oder Geländeobjekte sind.
    * Mesh (Gittermodell): Die Anzahl von Gittermodellrenderer-Objekten.
    * Terrain (Gelände): Die Anzahl von Geländeobjekten.
    * Geometry (Geometrie): Die Anzahl von Gittermodell- oder Geländeobjekten, die als **Acoustics Geometry** (Akustikgeometrie) markiert sind.
    * Navigation: Die Anzahl von Gittermodell- oder Geländeobjekten, die als **Acoustics Navigation** (Akustiknavigation) markiert sind. In dieser Anzahl ist das Navigationsgittermodell von Unity (NavMesh) nicht eingeschlossen.
1. Die Gesamtzahl der „markierbaren“ Objekte in der Szene, bei denen es sich nur um Gittermodellrenderer und Geländeobjekte handelt. Verwenden Sie die Kontrollkästchen, um diese Objekte als „Geometrie“ oder „Navigation“ für Akustik zu markieren (Hinzufügen der entsprechenden Komponente).
1. Wenn keine Objekte ausgewählt sind, erinnert dieser Hinweis Sie daran, bei Bedarf Objekte zu markieren. Sie können auch ein oder beide Kontrollkästchen aktivieren, um alle Objekte der Szene zu markieren.
1. Wenn Objekte ausgewählt sind, zeigt dieser Abschnitt nur den Status der ausgewählten Objekte an.
1. Die Gesamtzahl der „markierbaren“ ausgewählten Objekte. Wenn Sie die Kontrollkästchen aktivieren bzw. deaktivieren, gilt dies nur für die ausgewählten Objekte.

Wenn in Ihrer Szene nichts ausgewählt ist, sieht die Registerkarte **Objects** (Objekte) wie in der folgenden Abbildung aus.

![Registerkarte „Acoustics Objects“ (Akustikobjekte) ohne ausgewählte Elemente](media/objects-tab-no-selection-detail.png)

Wenn in Ihrem Szenen- oder Hierarchiefenster Elemente ausgewählt sind, sieht die Registerkarte wie in der folgenden Abbildung aus.

![Registerkarte „Acoustics Objects“ (Akustikobjekte) mit ausgewählten Elementen](media/objects-tab-selection-detail.png)

Falls nur einige Objekte markiert sind, wird in den entsprechenden Kontrollkästchen ein Wert vom Typ „Gemischt“ angezeigt. Dies ist in der folgenden Abbildung dargestellt.

![Registerkarte „Acoustics Objects“ (Akustikobjekte) mit gemischter Markierung von Symbolen](media/mixed-object-selection-detail.png)

Aktivieren Sie das Kontrollkästchen, um alle Elemente zu markieren. Deaktivieren Sie es, um die Markierung für alle Objekte aufzuheben.

Objekte können für Geometrie und Navigation markiert werden.

## <a name="select-acoustic-materials"></a>Auswählen akustischer Materialien
Wählen Sie nach dem Markieren Ihrer Objekte die Schaltfläche **Materials** (Materialien) aus. Weisen Sie anschließend akustische Materialien zu. Das Materialsystem für Projekt Akustik ist mit dem Unity-System für visuelle Materialien verknüpft. Damit zwei Objekte über separates Akustikmaterial verfügen können, müssen auch die visuellen Materialien getrennt sein.

Anhand der Auswahl der akustischen Materialien wird die Menge an Schallenergie bestimmt, die von einer Fläche jeweils reflektiert wird. Das akustische Standardmaterial weist eine Absorption auf, die der von Stahl ähnelt. Projekt Akustik schlägt die Materialien basierend auf dem Namen des visuellen Materials vor. Sie können einem Material auch das akustische Material **Custom** (Benutzerdefiniert) zuweisen, um einen anpassbaren Schieberegler für den Absorptionskoeffizienten zu aktivieren.

Die Nachhallzeit eines bestimmten Materials in einem Raum ist umgekehrt proportional zum Absorptionskoeffizienten. Die meisten Materialien weisen Absorptionswerte im Bereich zwischen 0,01 und 0,20 auf. Materialien mit Absorptionskoeffizienten außerhalb dieses Bereichs sind extrem absorbierend.

![In einem Graphen ist die negative Korrelation der Nachhallzeit mit dem Absorptionskoeffizienten dargestellt.](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Materials“ (Materialien)
![Registerkarte „Acoustics Materials“ (Akustikmaterial) in Unity](media/materials-tab-detail.png)
1. Diese Registerkarte wird mit der Schaltfläche **Materials** (Materialien) angezeigt.
2. Eine kurze Beschreibung der Optionen, die Ihnen mit dieser Registerkarte zur Verfügung stehen.
3. Wenn dieses Kontrollkästchen aktiviert ist, werden nur Materialien aufgelistet, die von Objekten mit der Markierung **Acoustics Geometry** (Akustikgeometrie) verwendet werden. Andernfalls werden alle Materialien aufgelistet, die in der Szene verwendet werden.
4. Verwenden Sie diese Optionen, um die Reihenfolge der Optionen in einem Menü in der Spalte **Acoustics** (Akustik) (6) zu ändern. Sortieren Sie Akustikmaterialien nach **Name** oder aufsteigend nach **Absorptivity** (Absorptionsgrad).
5. Eine alphabetisch sortierte Liste mit den Materialien, die in der Szene verwendet werden. Wenn das Kontrollkästchen **Show Marked Only** (Nur markierte anzeigen) (3) aktiviert ist, werden nur Materialien angezeigt, die von Objekten mit der Markierung **Acoustics Geometry** (Akustikgeometrie) verwendet werden. Wählen Sie hier eines der Materialien aus, um alle Objekte der Szene auszuwählen, in denen dieses Material verwendet wird.
6. Die Akustikmaterialien, denen die Szenenmaterialien zugewiesen wurden. Wählen Sie ein beliebiges Element aus, um das Akustikmaterial zu ändern, das diesem Szenenmaterial zugewiesen ist. Verwenden Sie die Optionen unter **Sort Acoustics By** (Akustik sortieren nach) (4), um die Sortierreihenfolge dieser Menüs zu ändern.
7. Der Absorptionskoeffizient des ausgewählten Akustikmaterials, das links in der Spalte (6) ausgewählt wurde. Ein Wert von „0“ steht für „perfekte Reflektion“ (keine Absorption), während „1“ für „perfekte Absorption“ (keine Reflektion) steht. Der Absorptionskoeffizient kann nicht geändert werden, sofern das ausgewählte Material nicht vom Typ **Custom** (Benutzerdefiniert) ist.
8. Für ein Material, das als **Custom** (Benutzerdefiniert) markiert ist, ist der Schieberegler aktiviert. Sie können den Schieberegler verwenden oder einen Wert eingeben, um einen Absorptionskoeffizienten zuzuweisen.

## <a name="calculate-and-review-listener-probe-locations"></a>Berechnen und Überprüfen der Zuhörer-Prüfpunktpositionen
Wechseln Sie nach dem Zuweisen der Materialien zur Registerkarte **Probes** (Prüfpunkte). Wählen Sie **Calculate** (Berechnen) aus, um Zuhörerprüfpunkte für die Simulation zu platzieren.

### <a name="what-the-calculate-button-does"></a>Funktionsweise der Schaltfläche „Calculate“ (Berechnen)
Die Schaltfläche **Calculate** (Berechnen) bereitet Ihre Szene mithilfe Ihrer ausgewählten akustischen Szenengeometrie für die Simulation vor:

- Zunächst wird die Geometrie aus dem Szenengittermodell verwendet und ein *Voxelvolumen* berechnet. Das Voxelvolumen ist ein Volumen Ihrer gesamten Szene, das aus kleinen kubischen „Voxeln“ besteht. Die Größe der Voxels richtet sich nach der Häufigkeit der Simulation, die mit der Einstellung **Simulation Resolution** (Simulationsauflösung) gesteuert wird. Für jedes Voxel wird markiert, ob es „frei“ ist oder Szenengeometrie enthält. Wenn ein Voxel Geometrie enthält, wird es mit dem Absorptionskoeffizienten des Materials markiert, das dieser Geometrie zugewiesen ist.
- Die Zuhörerprüfpunkte werden mithilfe der Navigationsgittermodelle platziert. Der Algorithmus gleicht die konkurrierenden Aspekte der räumlichen Abdeckung und der Simulationszeit sowie der Dateigröße ab. Hierbei wird sichergestellt, dass schmale Korridore und kleine Räume gleichermaßen abgedeckt werden. Typische Prüfpunktanzahlen liegen zwischen 100 für kleine Szenen und einigen Tausend für große Szenen.

Je nach Größe Ihrer Szene und der Geschwindigkeit Ihres Computers können die Berechnungen mehrere Minuten dauern.

### <a name="review-voxel-and-probe-placement"></a>Überprüfen der Voxel und der Prüfpunktplatzierung
Zeigen Sie eine Vorschau der Datenvoxel und der Prüfpunktpositionen an, um sicherzustellen, dass Sie für das Baking der Szene bereit sind. Ein unvollständiges Navigationsgittermodell oder fehlende bzw. zusätzliche akustische Geometrie sind in der Vorschau normalerweise leicht erkennbar. Aktivieren bzw. deaktivieren Sie die Voxeldaten und die Prüfpunktpositionen, indem Sie das Menü **Gizmos** verwenden.

![Menü „Gizmos“ in Unity](media/gizmos-menu.png)

Voxel, die akustische Geometrie enthalten, werden als grüne Würfel angezeigt. Erkunden Sie die Szene, und stellen Sie sicher, dass alle Geometrieobjekte Voxel enthalten. Die Szenenkamera muss sich in einem Radius von etwa 5 Metern um das Objekt befinden, damit die Voxels angezeigt werden.

Wenn Sie die erstellten Voxel mit niedriger und hoher Auflösung vergleichen, sehen Sie, dass die Voxel mit niedriger Auflösung doppelt so groß sind.

![Vorschau für Voxel mit niedriger Auflösung im Unity-Editor](media/voxel-cubes-preview.png)

Die Simulationsergebnisse werden zur Laufzeit zwischen den Positionen der Zuhörerprüfpunkte interpoliert. Überprüfen Sie, ob Prüfpunkte in der Nähe aller Orte vorhanden sind, an denen sich der Spieler voraussichtlich durch die Szene bewegen wird.

![Vorschau der Prüfpunkte im Unity-Editor](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Vorsicht bei der Umbenennung von Szenen
Der Szenenname wird verwendet, um die Szene mit den Dateien zu verbinden, in denen die Position und die Voxels des Prüfpunkts gespeichert sind. Wenn Sie die Szene nach der Berechnung der Prüfpunkte umbenennen, gehen die Zuweisungs- und Platzierungsdaten des Materials verloren, und die Berechnung sollte erneut ausgeführt werden.

### <a name="for-reference-parts-of-the-probes-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Probes“ (Tests)
![Registerkarte „Acoustics Probes“ (Akustikprüfpunkte) in Unity](media/probes-tab-detail.png)

1. Diese Registerkarte wird mit der Schaltfläche **Probes** (Prüfpunkte) aufgerufen.
2. Eine kurze Beschreibung der Optionen, die Ihnen auf dieser Registerkarte zur Verfügung stehen.
3. Verwenden Sie diese Optionen, um für die Simulation eine grobe oder feine Auflösung festzulegen. Eine grobe Auflösung ist schneller, aber diese Option hat auch Nachteile. Ausführliche Informationen finden Sie unter [Bakingauflösung](bake-resolution.md).
4. Gibt an, wo die Akustikdatendateien abgelegt werden sollen. Wählen Sie die Schaltfläche „ **...** “ aus, um auf eine Ordnerauswahl zuzugreifen. Der Standardspeicherort ist *Assets/AcousticsData*. An diesem Speicherort wird auch der Unterordner *Editor* erstellt. Weitere Informationen finden Sie weiter unten in diesem Artikel unter [Durch den Bakingvorgang hinzugefügte Datendateien](#Data-Files).
5. Das hier angegebene Präfix wird verwendet, um die Datendateien für diese Szene zu benennen. Der Standardname lautet „Acoustics_ *[Szenenname]* “.
6. Nach dem Berechnen der Tests werden die soeben beschriebenen Steuerelemente deaktiviert. Wählen Sie die Schaltfläche **Clear** (Löschen) aus, um die Berechnungen zu löschen und die Steuerelemente zu aktivieren, damit Sie mit den neuen Einstellungen eine Neuberechnung vornehmen können.
7. Wählen Sie **Calculate** (Berechnen) aus, um die Szene in Voxel umzuwandeln und Prüfpunktpositionen zu berechnen. Die Berechnung erfolgt lokal auf Ihrem Computer. Dies muss vor einem Bakingvorgang durchgeführt werden.

In dieser Version von Projekt Akustik können Tests nicht manuell positioniert werden. Verwenden Sie den automatisierten Prozess auf der Registerkarte **Probes** (Prüfpunkte).

Weitere Informationen zur groben und feinen Auflösung finden Sie unter [Bakingauflösung](bake-resolution.md).

## <a name="bake-your-scene-by-using-azure-batch"></a>Ausführen des Bakings für Ihre Szene per Azure Batch
Sie können das Baking Ihrer Szene mit dem Azure Batch-Dienst in einem Computecluster in der Cloud ausführen. Das Unity-Plug-In für Projekt Akustik stellt eine direkte Verbindung mit Azure Batch her, um einen Azure Batch-Cluster für jeden Bakingvorgang zu instanziieren, zu verwalten und zu beenden. Geben Sie auf der Registerkarte **Bake** Ihre Azure-Anmeldeinformationen ein, wählen Sie Typ und Größe für den Clustercomputer aus, und klicken Sie anschließend auf **Bake**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Zur Referenz: Bestandteile der Registerkarte „Bake“
![Registerkarte „Acoustics Bake“ (Akustikbaking) in Unity](media/bake-tab-details.png)

1. Diese Registerkarte wird mit der Schaltfläche **Bake** angezeigt.
2. Eine kurze Beschreibung der Optionen, die Ihnen auf dieser Seite zur Verfügung stehen.
3. Geben Sie Ihre Azure-Anmeldeinformationen in diese Felder ein, nachdem Ihr Azure-Konto erstellt wurde. Weitere Informationen finden Sie unter [Erstellen eines Azure Batch-Kontos](create-azure-account.md).
4. Das Docker-Imagetag-Feld für das Akustiktoolset.
5. Öffnet das Azure-Portal, damit Sie Ihre Abonnements verwalten, die Nutzung überwachen und Abrechnungsinformationen anzeigen können.
6. Gibt den Azure Batch-Computeknotentyp an, der für die Berechnung verwendet werden soll. Der Knotentyp muss vom Standort Ihres Azure-Rechenzentrums unterstützt werden. Wenn Sie nicht sicher sind, können Sie **Standard_F8s_v2** übernehmen.
7. Die Anzahl von Knoten, die für die Berechnung verwendet werden sollen. Diese Anzahl wirkt sich auf die Bakingzeit aus. Sie ist durch Ihre Azure Batch-Kernzuteilung beschränkt. Die Standardzuteilung ermöglicht nur zwei Knoten mit 8 Kernen oder einen Knoten mit 16 Kernen, aber sie kann erweitert werden. Weitere Informationen zu Einschränkungen bei der Kernzuteilung finden Sie unter [Erstellen eines Azure Batch-Kontos](create-azure-account.md).
8. Aktivieren Sie dieses Kontrollkästchen, um Ihren Computepool so zu konfigurieren, dass er [Knoten mit niedriger Priorität](https://docs.microsoft.com/azure/batch/batch-low-pri-vms) verwendet. Die Kosten für Computeknoten mit niedriger Priorität sind deutlich geringer. Es kann aber sein, dass sie nicht immer verfügbar sind oder vorzeitig entfernt werden.
9. Die Anzahl der Tests für Ihre Szene, die auf der Registerkarte **Probe** (Tests) berechnet wurde. Die Anzahl von Prüfpunkten bestimmt die Anzahl von Simulationen, die in der Cloud ausgeführt werden müssen. Sie können nicht mehr Knoten als Prüfpunkte angeben.
10. Eine Schätzung der Zeit, die für die Ausführung Ihres Auftrags in der Cloud benötigt wird (ohne Startzeit der Knoten). Nachdem die Ausführung des Auftrags gestartet wurde, wird in diesem Feld eine Schätzung angezeigt, wie lange Sie auf die Ergebnisse warten müssen.
11. Die gesamte Computezeit, die für die Ausführung der Simulationen erforderlich ist. Dieser Wert entspricht der Computezeit für die Knoten, die in Azure verwendet werden. Weitere Informationen finden Sie unter [Schätzen der Kosten für den Azure-Bakingvorgang](#Estimating-bake-cost) weiter unten in diesem Artikel.
12. In dieser Meldung erhalten Sie den Hinweis, wo die Ergebnisse des Bakingvorgangs nach Abschluss des Auftrags gespeichert werden.
13. *(Nur für fortgeschrittene Benutzer:)* Mit dieser Schaltfläche wird für Unity erzwungen, ein von Ihnen übermitteltes Baking zu vergessen. Wenn Sie die Ergebnisse beispielsweise mit einem anderen Computer heruntergeladen haben, können Sie die Schaltfläche **Clear State** (Status löschen) auswählen, damit der Auftrag „vergessen“ wird. Die Ergebnisdatei wird nach Abschluss des Vorgangs dann *nicht* heruntergeladen. *Dies entspricht nicht dem Abbrechen des Auftrags. Wenn der Auftrag bereits ausgeführt wird, wird die Ausführung in der Cloud fortgesetzt.*
14. Wählen Sie diese Schaltfläche aus, um den Bakingvorgang an die Cloud zu übermitteln. Während der Ausführung eines Auftrags ändert sich diese Schaltfläche in **Cancel Job** (Auftrag abbrechen).
15. Wählen Sie diese Schaltfläche aus, um die Verarbeitung der [Akustiksimulation auf Ihrem PC](#Local-bake) vorzubereiten.
16. Dieser Bereich zeigt den Status des Bake-Vorgangs an. Nach Abschluss des Bakingvorgangs wird „Downloaded“ (Heruntergeladen) angezeigt.

Vollständige Informationen zu aktiven Aufträgen, Computepools und Speicherplatz können Sie jederzeit im [Azure-Portal](https://portal.azure.com) abrufen.

Während ein Auftrag ausgeführt wird, ändert sich die Bezeichnung der Schaltfläche **Bake** in **Cancel Job** (Auftrag abbrechen). Verwenden Sie diese Schaltfläche, um den ausgeführten Auftrag abzubrechen. Sie werden aufgefordert, den Vorgang zu bestätigen. Das Abbrechen eines Auftrags kann nicht rückgängig gemacht werden. Wenn Sie den Vorgang abbrechen, sind keine Ergebnisse verfügbar, aber Ihnen werden trotzdem Kosten für die genutzte Azure-Computezeit berechnet.

Nachdem Sie einen Bakingvorgang gestartet haben, können Sie Unity schließen. Je nach Projekt, Knotentyp und Anzahl der Knoten kann ein Bake-Vorgang in die Cloud mehrere Stunden dauern. Der Status des Bake-Auftrags wird aktualisiert, wenn Sie das Projekt neu laden und das Acoustics-Fenster öffnen. Wenn der Auftrag abgeschlossen ist, wird die Ausgabedatei heruntergeladen.

Aus Sicherheitsgründen werden die Azure-Anmeldeinformationen auf Ihrem lokalen Computer gespeichert und Ihrem Unity-Editor zugeordnet. Sie werden nur verwendet, um eine sichere Verbindung mit Azure herzustellen.

## <a name="find-the-status-of-a-running-job-on-the-azure-portal"></a>Ermitteln des Status eines ausgeführten Auftrags im Azure-Portal

1. Suchen Sie auf der Registerkarte **Bake** nach der ID des Bakingauftrags.

    ![Unity-ID des Bakingauftrags auf der Registerkarte „Bake“](media/unity-job-id.png)  

2. Öffnen Sie das [Azure-Portal](https://portal.azure.com), navigieren Sie zum Batch-Konto, das für das Baking verwendet wurde, und wählen Sie **Aufträge** aus.

    ![Link „Aufträge“ im Azure-Portal](media/azure-batch-jobs.png)  

3. Suchen Sie in der Auftragsliste nach der Auftrags-ID.

   ![Status des Bakingauftrags im Azure-Portal](media/azure-bake-job-status.png)  

4. Wählen Sie die Auftrags-ID aus, um den Status der zugehörigen Aufgaben und den Status des Gesamtauftrags anzuzeigen.

   ![Aufgabenstatus für Baking](media/azure-batch-task-state.png)  


### <a name="Estimating-bake-cost"></a> Schätzen der Kosten für den Azure-Bakingvorgang

Beginnen Sie mit dem Wert **Geschätzte Computekosten**. Dies ist ein Wert für eine Dauer. Multiplizieren Sie diesen Wert mit den stündlichen Kosten in Ihrer lokalen Währung für den gewählten **VM-Knotentyp**. Beachten Sie Folgendes: Das Ergebnis enthält nicht die Knotenzeit, die benötigt wird, um für die Knoten die Betriebsbereitschaft herzustellen.

Angenommen, Sie wählen **Standard_F8s_v2** als Knotentyp aus, sodass sich Kosten von 0,40 USD pro Stunde ergeben. Wenn der Wert für **Geschätzte Computekosten** 3 Stunden und 57 Minuten beträgt, ergeben sich für die Ausführung des Auftrags folgende Kosten: 0,40 USD · ~4 Stunden = ~1,60 USD. Die tatsächlichen Kosten liegen aufgrund der zusätzlichen Zeit für das Starten der Knoten wahrscheinlich etwas höher.

Informationen zu den stündlichen Kosten finden Sie auf der Seite mit den [Azure Batch-Preisen](https://azure.microsoft.com/pricing/details/virtual-machines/linux). (Wählen Sie **Optimiert für Compute** oder **Hochleistungscomputing**  als Kategorie aus.)

## <a name="Local-bake"></a> Baking Ihrer Szene auf Ihrem PC
Sie können das Baking Ihrer Szene auch auf Ihrem PC ausführen. Dieses Verfahren kann nützlich sein, um für kleine Szenen mit der Akustik zu experimentieren, bevor Sie ein Azure Batch-Konto erstellen. Beachten Sie aber, dass eine lokale Akustiksimulation je nach Größe der Szene sehr lange dauern kann.

### <a name="minimum-hardware-requirements"></a>Minimale Hardwareanforderungen
* Ein x86-64-Prozessor mit mindestens 8 Prozessorkernen und 32 GB RAM
* [Hyper-V-aktiviert](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) zur Ausführung von Docker

Beispiel für unseren Test auf einem Computer mit Intel Xeon-Prozessor E5-1660 mit 8 Prozessorkernen bei 3 GHz und 32 GB RAM:
* Hier hat eine kleine Szene mit 100 Prüfpunkten etwa 2 Stunden für einen einfachen Bakingvorgang und etwa 32 Stunden für einen Bakingvorgang mit hoher Auflösung in Anspruch genommen.
* Eine mittelgroße Szene mit 1.000 Prüfpunkten hat etwa 20 Stunden für einen einfachen Bakingvorgang und 21 Tage für einen Bakingvorgang mit hoher Auflösung in Anspruch genommen.

### <a name="set-up-docker"></a>Einrichten von Docker
Installieren und Konfigurieren von Docker auf dem PC, auf dem die Simulation verarbeitet werden soll:
1. Installieren Sie [Docker Desktop](https://www.docker.com/products/docker-desktop).
2. Öffnen Sie die Docker-Einstellungen, navigieren Sie zu **Advanced** (Erweitert), und konfigurieren Sie die Ressourcen für mindestens 8 GB RAM. Je mehr CPUs Sie Docker zuweisen können, desto schneller wird der Bakingvorgang abgeschlossen.  
![Beispiel für Docker-Einstellungen](media/docker-settings.png)
1. Navigieren Sie zu **Freigegebene Laufwerke**, und aktivieren Sie die Freigabe für das Laufwerk, das für die Verarbeitung verwendet wird.  
![Optionen für freigegebene Docker-Laufwerke](media/docker-shared-drives.png)

### <a name="run-the-local-bake"></a>Ausführen des lokalen Bakings
1. Wählen Sie auf der Registerkarte **Bake** die Schaltfläche **Prepare Local Bake** (Lokales Baking vorbereiten) aus. Wählen Sie anschließend einen Ordnerspeicherort aus, in dem die Eingabedateien und Ausführungsskripts gespeichert werden sollen. Sie können den Bakingvorgang dann auf jedem beliebigen Computer ausführen, solange er die minimalen Hardwareanforderungen erfüllt und Sie Docker installieren, indem Sie den Ordner auf diesen Computer kopieren.
2. Starten Sie die Simulation unter Windows mit dem Skript *runlocalbake.bat* bzw. unter macOS mit dem Skript *runlocalbake.sh*. Dieses Skript ruft das Docker-Image für Projekt Akustik mit dem für die Simulationsbearbeitung erforderlichen Toolset ab und startet die Simulation.
3. Kopieren Sie die resultierende *ACE*-Datei nach Abschluss der Simulation zurück in Ihr Unity-Projekt. Um sicherzustellen, dass Unity dies als Binärdatei erkennt, fügen Sie „.bytes“ an die Dateierweiterung an (z. B. „Scene1.ace.bytes“). Die ausführlichen Protokolle für die Simulation werden in der Datei *AcousticsLog.txt* gespeichert. Falls Probleme auftreten, sollten Sie diese Datei untersuchen, um Hilfe beim Diagnostizieren des Problems zu erhalten.

## <a name="Data-Files"></a> Durch den Bakingvorgang hinzugefügte Datendateien

Die folgenden vier Datendateien werden während des Bakingvorgangs erstellt. Eine enthält die Simulationsergebnisse und wird mit Ihrem Titel erstellt. In den anderen werden die zugehörigen Daten des Unity-Editors gespeichert.

Simulationsergebnis:
* *Assets/AcousticsData/Acoustics\_[Szenenname].ace.bytes:* Diese Datei ist die Nachschlagetabelle für die Runtime. Sie enthält die Simulationsergebnisse und die in Voxel umgewandelten Akustikszenenelemente. Sie können den Namen und den Speicherort dieser Datei auf der Registerkarte **Probes** (Prüfpunkte) ändern.

   *Achten Sie darauf, dass Sie die Simulationsergebnisdatei nicht löschen. Sie kann nicht wiederhergestellt werden, sondern nur durch eine Wiederholung des Bakingvorgangs neu generiert werden.*

Editor-Datendateien:
* *Assets/Editor/[Szenenname]\_AcousticsParameters.asset:* In dieser Datei werden die Daten gespeichert, die Sie in die Felder auf der Acoustics-Benutzeroberfläche eingeben. Sie können den Namen und Speicherort dieser Datei nicht ändern.
* *Assets/AcousticsData/Editor/Acoustics_[Szenenname].vox:* In dieser Datei werden die als Voxel aufbereitete Akustikgeometrie und die Materialeigenschaften gespeichert. Sie werden berechnet, wenn Sie auf der Registerkarte **Probes** (Prüfpunkte) die Schaltfläche **Calculate** (Berechnen) auswählen. Sie können den Namen und den Speicherort dieser Datei auf der Registerkarte **Probes** (Prüfpunkte) ändern.
* *Assets/AcousticsData/Editor/Acoustics\_[Szenenname]\_config.xml:* In dieser Datei werden die Simulationsparameter gespeichert, die beim Auswählen von **Calculate** (Berechnen) berechnet werden. Sie können den Namen und den Speicherort dieser Datei auf der Registerkarte **Probes** (Prüfpunkte) ändern.

## <a name="set-up-the-acoustics-lookup-table"></a>Einrichten der Akustik-Nachschlagetabelle
Ziehen Sie das Prefab **Project Acoustics** (Projekt Akustik) aus dem Projektbereich in Ihre Szene:

![Akustikprefab in Unity](media/acoustics-prefab.png)

Wählen Sie das Spielobjekt **ProjectAcoustics** aus, und wechseln Sie zum zugehörigen Inspectorbereich. Geben Sie den Speicherort für das Ergebnis Ihres Bakingvorgangs an (ACE-Datei in *Assets/AcousticsData*): Ziehen Sie ihn in das Acoustics Manager-Skript, oder wählen Sie neben dem Textfeld die Schaltfläche mit dem Kreis aus.

![Acoustics Manager-Prefab in Unity](media/acoustics-manager.png)

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die [Entwurfssteuerelemente für Unity](unity-workflow.md).
* Sehen Sie sich die [Entwurfskonzepte in Projekt Akustik](design-process.md) an.
