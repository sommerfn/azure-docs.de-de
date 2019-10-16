---
title: Integration von Projekt Akustik in Unreal und Wwise
titlesuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Sie die Projekt Akustik-Plug-Ins für Unreal und Wwise in Ihr Projekt integrieren.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: e57212a3002390754aaebc5f2aa9ffb10af230a2
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243057"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Integration von Projekt Akustik in Unreal und Wwise
In diesem Artikel wird beschrieben, wie Sie das Projekt Akustik-Plug-In-Paket in Ihr vorhandenes Unreal- und Wwise-Spieleprojekt integrieren.

Softwareanforderungen:
* [Unreal Engine](https://www.unrealengine.com/) 4.20+
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1
* [Wwise-Plug-In für Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  
  Wenn Sie das Wwise SDK direkt integriert haben, anstatt die Wwise-Plug-Ins für Unreal zu verwenden, lesen Sie die Dokumentation zum Projekt Akustik-Unreal-Plug-In, und passen Sie Wwise-API-Aufrufe an.

Wenn Sie Projekt Akustik mit einer anderen Audio-Engine als Wwise verwenden möchten, stellen Sie eine Erweiterungsanforderung im [Projekt Akustik-Diskussionsforum](https://github.com/microsoft/ProjectAcoustics/issues). Sie können das Projekt Akustik-Unreal-Plug-In verwenden, um Akustikdaten abzufragen und API-Aufrufe an Ihre Engine zu senden.

## <a name="download-project-acoustics"></a>Herunterladen von Projekt Akustik
Wenn dies noch nicht geschehen ist, laden Sie das [Projekt Akustik-Plug-In-Paket für Unreal und Wwise](https://www.microsoft.com/download/details.aspx?id=58090) herunter.

Das Paket enthält ein Plug-In für die Unreal-Engine und ein Plug-In für den Wwise-Mixer. Das Unreal-Plug-In bietet Editor- und Runtime-Integration. Während eines Spiels berechnet das Projekt Akustik-Unreal-Plug-In Parameter wie z. B. Verdeckung für jedes Spielobjekt in jedem Frame. Diese Parameter werden in Wwise-API-Aufrufe übersetzt.

## <a name="integration-steps"></a>Schritte für die Integration

Befolgen Sie diese Schritte, um das Paket zu installieren und in Ihrem Spiel bereitzustellen.

### <a name="install-the-project-acoustics-mixer-plug-in"></a>Installieren des Mixer-Plug-Ins für Projekt Akustik
1. Öffnen Sie das Wwise-Startprogramm. Wählen Sie auf der Registerkarte **Plugins** (Plug-Ins) unter **Install New Plug-ins** (Neue Plug-Ins installieren) die Option **Add from Directory** (Aus Verzeichnis hinzufügen) aus.

    ![Installieren eines Plug-Ins im Wwise-Startprogramm](media/wwise-install-new-plugin.png)

1. Wählen Sie das im Downloadpaket enthaltene Verzeichnis *AcousticsWwisePlugin\ProjectAcoustics* aus. Es enthält das Plug-In-Paket für den Wwise-Mixer.

   Wwise installiert das Plug-In. In Projekt Akustik wird die Liste der in Wwise installierten Plug-Ins angezeigt.  
![In Wwise installierte Plug-Ins nach der Installation von Projekt Akustik](media/unreal-integration-post-mixer-plugin-install.png)

### <a name="dedeploy-wwise-into-your-game"></a>Erneutes Bereitstellen von Wwise in Ihrem Spiel
Stellen Sie Wwise erneut in Ihrem Spiel bereit, auch wenn Sie Wwise bereits integriert haben. In diesem Schritt wird das Wwise-Plug-In für Projekt Akustik integriert.

   > [!NOTE]
   > **Engine-Plug-in:** Wenn Sie Wwise als Spiel-Plug-In in einem Unreal-C++-Projekt installiert haben, überspringen Sie diesen Schritt. Wenn Sie Wwise stattdessen als Engine-Plug-In installiert haben, z. B. weil Ihr Unreal-Projekt nur als Blaupause fungiert, ist die Wwise-Bereitstellung mit unserem Mixer-Plug-In komplexer. Erstellen Sie ein leeres Unreal C++-Dummyprojekt. Wenn der Unreal-Editor geöffnet wird, schließen Sie ihn, und führen Sie die verbleibenden Schritte zum Bereitstellen von Wwise im Dummyprojekt aus. Kopieren Sie dann das bereitgestellte Wwise-Plug-In.
 
1. Wählen Sie im Wwise-Startprogramm die Registerkarte **Unreal Engine** aus. Wählen Sie das Hamburger-Menü (Symbol) neben **Recent Unreal Engine Projects** (Letzte Unreal-Engine-Projekte) aus, und wählen Sie dann **Browse for project** (Nach Projekt suchen) aus. Öffnen Sie die Unreal-Projektdatei ( *.project*) Ihres Spiels.

    ![Unreal-Registerkarte im Wwise-Startprogramm](media/wwise-unreal-tab.png)

1. Wählen Sie **Integrate Wwise in Project** (Wwise in Projekt integrieren) oder **Modify Wwise in Project** (Wwise in Projekt ändern) aus. Mit diesem Schritt werden die Binärdateien von Wwise in Ihr Projekt integriert, einschließlich des Projekt Akustik-Mixer-Plug-Ins.

   > [!NOTE]
   > **Engine-Plug-in:** Wenn Sie Wwise als Engine-Plug-In verwendet und das Dummyprojekt gemäß der obigen Beschreibung erstellt haben, kopieren Sie den von Wwise bereitgestellten Ordner: *[DummyUProject]\Plugins\Wwise*. Fügen Sie ihn in *[UESource]\Engine\Plugins\Wwise* ein. *[DummyUProject]* ist der leere Unreal C++-Projektpfad, während in *[UESource]* die Unreal-Engine-Quellen installiert werden. Nach dem Kopieren des Ordners können Sie das Dummyprojekt löschen.

### <a name="add-the-project-acoustics-unreal-plug-in-to-your-game"></a>Hinzufügen des Projekt Akustik-Unreal-Plug-Ins zu Ihrem Spiel
 
1. Kopieren Sie den Ordner *Unreal\ProjectAcoustics* im Plug-In-Paket. Erstellen Sie einen neuen Ordner namens *[UProjectDir]\Plugins\ProjectAcoustics*, wobei *[UProjectDir]* der Projektordner des Spiels ist, welcher die *.uproject*-Datei enthält.

   > [!NOTE]
   > **Engine-Plug-in:** Wenn Sie Wwise als Engine-Plug-In verwenden, sollten Sie auch Projekt Akustik als Unreal-Engine-Plug-In nutzen. Verwenden Sie anstelle des zuvor erwähnten Zielverzeichnisses das Verzeichnis *[UESource]\Engine\Plugins\ProjectAcoustics*.

1. Vergewissern Sie sich, dass ein Ordner namens *Wwise* neben dem Ordner *ProjectAcoustics* angezeigt wird. Dieser enthält das Wwise-Plug-In sowie Binärdateien für das Mixer-Plug-In, das Sie bereits bereitgestellt haben.

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Erweitern der Funktionen des Unreal-Plug-Ins von Wwise
Für das Unreal-Plug-In für Projekt Akustik müssen zusätzliche Funktionen aus der Unreal-Plug-In-API von Wwise verfügbar gemacht werden. Befolgen Sie dazu [diese Anweisungen](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html) (in englischer Sprache). Wir haben eine Batchdatei bereitgestellt, um die Patchingprozedur zu automatisieren.

* Führen Sie in *Plugins\ProjectAcoustics\Resources* die Datei *PatchWwise.bat* aus. In der folgenden Beispielabbildung wird das Beispielprojekt „AcousticsGame“ verwendet.

    ![Windows Explorer-Fenster mit hervorgehobenen Skript zum Patchen von Wwise](media/patch-wwise-script.png)

* Wenn das DirectX SDK nicht installiert ist: Je nach der verwendeten Version von Wwise müssen Sie ggf. die Zeile mit `DXSDK_DIR` in *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* auskommentieren:

    ![Code-Editor mit der auskommentierten DXSDK-Zeile](media/directx-sdk-comment.png)

* Beim Kompilieren mit Visual Studio 2019: Zum Umgehen eines Verknüpfungsfehlers mit Wwise ändern Sie manuell den Standardwert `VSVersion` in *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* in den Wert **vc150**:

    ![Code-Editor mit VSVersion-Wert, der in vc150 geändert wurde](media/vsversion-comment.png)

### <a name="build-the-game-and-check-that-python-is-enabled"></a>Erstellen des Spiels und Überprüfen, ob Python aktiviert ist

1. Kompilieren Sie Ihr Spiel, und stellen Sie sicher, dass der Buildprozess ordnungsgemäß ausgeführt wird. Wird der Buildprozess nicht ordnungsgemäß ausgeführt, prüfen Sie sorgfältig die vorherigen Schritte, ehe Sie fortfahren.

1. Öffnen Sie Ihr Projekt im Unreal-Editor.

    > [!NOTE]
    > **Engine-Plug-in:** Wenn Sie Projekt Akustik als Engine-Plug-In verwenden, stellen Sie außerdem sicher, dass es unter den integrierten Plug-Ins aktiviert ist.

    Sie sehen einen neuen Modus, der darauf hinweist, dass Projekt Akustik integriert wurde.

    ![Project Acoustics-Modus „Vollständig“ in Unreal](media/acoustics-mode-full.png)

1. Vergewissern Sie sich, dass das Python-Plug-In für Unreal aktiviert ist, damit die Editor-Integration ordnungsgemäß funktioniert.

    ![Aktivierte Python-Erweiterungen im Unreal-Editor](media/ensure-python.png)

### <a name="set-up-your-wwise-project-to-use-project-acoustics"></a>Einrichten des Wwise-Projekts für die Verwendung von Projekt Akustik

Im Download mit den Beispielen ist auch ein Wwise-Beispielprojekt enthalten. Es wird empfohlen, dass Sie dieses beim Durcharbeiten dieser Anweisungen anzeigen. Die Screenshots weiter unten in diesem Artikel stammen aus diesem Projekt.

#### <a name="bus-setup"></a>Einrichten des Busses
Das Projekt Akustik-Unreal-Plug-In sucht nach dem zugehörigen Mixer-Plug-In in einem Bus, dessen Namen exakt `Project Acoustics Bus` lautet. Erstellen Sie einen neuen Audiobus mit diesem Namen. Das Mixer-Plug-In kann in verschiedenen Konfigurationen verwendet werden. Aber vorerst wird davon ausgegangen, dass es ausschließlich zur Hallverarbeitung verwendet wird. Dieser Bus trägt das abgemischte Hallsignal für alle Quellen, die Projekt Akustik verwenden. Er kann upstream in jede Bus-Mixingstruktur mischen. Dieses Beispiel stammt aus dem Wwise-Beispielprojekt, das im Download mit Beispielen enthalten ist.

![Wise-Busse mit dem Projekt Akustik-Bus](media/acoustics-bus.png)

1. Legen Sie die Kanalkonfiguration für den Bus auf *1.0*, *2.0*, *4.0*, *5.1* oder *7.1* fest. Bei jeder anderen Einstellung gibt es keine Ausgabe am Bus.

    ![Kanalkonfigurationsoptionen für den Projekt Akustik-Bus](media/acoustics-bus-channel-config.png)

1. Wechseln Sie zur Detailansicht für den Projekt Akustik-Bus, und vergewissern Sie sich, dass die Registerkarte **Mixer-Plug-In** angezeigt wird.

    ![Wwise mit Registerkarte „Mixer-Plug-In“ für aktivierten Projekt Akustik-Bus](media/mixer-tab-enable.png)

1. Wechseln Sie zur Registerkarte **Mixer-Plug-In**, und fügen Sie das Projekt Akustik-Mixer-Plug-In zum Bus hinzu.

    ![Abbildung zum Hinzufügen des Projekt Akustik-Mixer-Plug-Ins zum Wwise-Bus](media/add-mixer-plugin.png)

#### <a name="actor-mixer-hierarchy-setup"></a>Einrichten der Akteur-Mixer-Hierarchie
Zur Leistungsoptimierung wendet Projekt Akustik die digitale Audiosignalverarbeitung gleichzeitig auf alle Quellen an. Daher muss das Plug-In als Mixer-Plug-In fungieren. Bei Wwise müssen sich Mixer-Plug-Ins im Ausgabebus befinden, obwohl der Ausgabebus üblicherweise das reine Ausgabesignal ohne Effekte trägt. Für Projekt Akustik muss das reine Signal über Hilfsbusse geleitet werden, während `Project Acoustics Bus` das Signal mit Effekten trägt. Der folgende Prozess unterstützt die sukzessive Migration zu diesem Signalflow.

Angenommen, Sie verfügen über ein Projekt mit einer Akteur-Mixer-Hierarchie mit Klängen für Schritte (*Footsteps*), Waffen (*Weapons*) und weitere Elemente auf oberster Ebene. Jedes Element verfügt über einen entsprechenden Ausgabebus für die Mischung ohne Effekte. Nehmen wir nun an, dass Sie das Signal für die Schritte migrieren möchten, um akustische Effekte zu verwenden. Erstellen Sie zunächst einen entsprechenden Hilfsbus für die Mischung ohne Effekte, der ein untergeordnetes Element des Ausgabebusses „Footsteps“ ist. In der folgenden Abbildung sind die Busse durch Voranstellen des Präfixes „Dry“ organisiert, die genaue Bezeichnung ist jedoch nicht wichtig. Alle Zähler oder Effekte, die im Bus „Footsteps“ vorhanden sind, funktionieren weiter wie zuvor.

![Empfohlenes Wwise-Setup für Mischung ohne Effekte](media/wwise-dry-mix-setup.png)

Ändern Sie nun die Ausgabestruktur für den Akteur-Mixer-Bus für „Footsteps“ wie folgt: Der *Projekt Akustik-Bus* wird als **Output Bus** (Ausgabebus) festgelegt, *Dry_Footsteps* als benutzerdefinierter „Auxiliary Bus“ (Hilfsbus).

![Empfohlenes Wwise-Setup für Akteur-Mixer-Bus](media/actor-mixer-bus-settings.png)

Jetzt werden alle Schritte akustisch aufbereitet und geben ihren Halleffekt im Projekt Akustik-Bus aus. Das Signal ohne Effekte wird über „Dry_Footsteps“ weitergeleitet und wie gewohnt mit räumlicher Tiefe versehen.

Projekt Akustik wird nur auf Klänge angewendet, die eine 3D-Position in der Welt besitzen. Gemäß der [Wwise-Dokumentation](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/) (in englischer Sprache) müssen die Positionierungseigenschaften wie hier gezeigt festgelegt werden. Die Einstellung **3D Spatialization** (Erzeugung von 3D-Raumtiefe) kann nach Bedarf als *Position* oder *Position + Orientation* (Position und Ausrichtung) festgelegt werden.

![Empfohlene Wwise-Einstellungen für die Akteurpositionierung](media/wwise-positioning.png)

Der **Ausgabebus** kann nicht auf einen anderen Bus festgelegt werden, der vorab in den *Projekt Akustik-Bus* gemischt wird. Wwise erzwingt diese Anforderung für Mixer-Plug-Ins.

Wenn ein untergeordnetes Element in der Akteur-Mixer-Hierarchie für „Footsteps“ keine akustischen Effekte verwenden soll, können Sie für dieses Element die Option „Override parent“ (Übergeordnetes Element außer Kraft setzen) verwenden, um die Effekte zu deaktivieren.

Wenn Sie durch das Spiel oder den Benutzer definierte Sendebefehle für Hall in einem Akteur-Mixer-Element im Spiel verwenden, deaktivieren Sie diese Befehle in diesem Mixer, um die doppelte Anwendung von Hall zu verhindern.

#### <a name="spatialization"></a>Erzeugen von räumlicher Tiefe
Standardmäßig wendet das Projekt Akustik-Plug-In für den Wwise-Mixer einen Faltungshall an und überlässt Wwise das Erzeugen der räumlichen Tiefe durch Schwenken. Wenn Sie für Projekt Akustik diese Standardkonfiguration beibehalten, bei der lediglich Hall angewendet wird, können Sie eine beliebige Kanalkonfiguration und eine beliebige Methode zum Erzeugen der räumlichen Tiefe für die Mischung ohne Effekte verwenden. Auf diese Weise können Sie beinahe jeden Raumerzeuger (Spatializer) mit den Halleffekten in Projekt Akustik mischen und abgleichen. Ihnen stehen die Optionen [Ambisonics-based binaural spatializers](https://www.audiokinetic.com/products/ambisonics-in-wwise/) (Ambisonics-basierte binaurale Raumerzeuger) und [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound) zur Verfügung.
 
Projekt Akustik enthält einen optionalen Raumerzeuger, der sowohl objektbasiertes, hochauflösendes HRTF-Rendering als auch Schwenken unterstützt. Aktivieren Sie das Kontrollkästchen **Perform Spatialization** (Räumliche Aufteilung ausführen) in den Einstellungen des Mixer-Plug-Ins, und wählen Sie eine der Optionen *HRTF* und *Panning* aus. Das Deaktivieren des benutzerdefinierten Hilfsbusses wird auch jedem Bus ohne Effekte mitgeteilt, sodass eine doppelte Erzeugung der räumlichen Tiefe durch das Mixer-Plug-In von Projekt Akustik und Wwise vermieden wird. Der Raumerzeugungsmodus kann nicht in Echtzeit geändert werden, da hierfür eine Neugenerierung der Soundbank erforderlich ist. Starten Sie Unreal neu, und generieren Sie dann die Soundbänke neu, bevor Sie die Umgebung wiedergeben, um die Konfigurationsänderungen des Mixer-Plug-Ins zu übernehmen, z. B. die Einstellung des Kontrollkästchens **Perform Spatialization** (Räumliche Aufteilung ausführen).

![Wwise-Einstellungen für die Raumerzeugung im Mixer-Plug-In](media/mixer-spatial-settings.png)

Andere objektbasierte Spatializer-Plug-Ins werden derzeit leider nicht unterstützt. Sie werden als Mixer-Plug-Ins implementiert, und das Zuweisen von mehreren Mixer-Plug-Ins zu einem einzelnen Akteur-Mixer-Element wird von Wwise nicht gestattet.  

### <a name="audio-setup-in-unreal"></a>Einrichten der Audiofunktionen in Unreal
1. Zuerst müssen Sie ein Baking für Ihr Spiellevel ausführen, um ein akustisches Medienobjekt zu erhalten. Dieses wird in *Content\Acoustics* platziert. Lesen Sie das [Unreal-Bakingtutorial](unreal-baking.md). Das Paket enthält einige Level, für die bereits ein Baking ausgeführt wurde.

1. Erstellen Sie einen Acoustics Space-Akteur (akustischer Raum) in Ihrer Szene. Erstellen Sie pro Level nur einen Akteur, da dieser die Akustik für das gesamte Level darstellt.

    ![Erstellen eines Acoustics Space-Akteurs im Unreal-Editor](media/create-acoustics-space.png)

1. Weisen Sie das Akustikmedienobjekt, für das bereits ein Baking ausgeführt wurde, der Acoustics Data-Position im Acoustics Space-Akteur zu. Jetzt verfügt Ihre Szene über akustischen Klang!

    ![Zuweisung von Acoustics-Medienobjekt im Unreal-Editor](media/acoustics-asset-assign.png)

1. Fügen Sie einen leeren Akteur hinzu. Konfigurieren Sie ihn wie folgt.

    ![Unreal-Editor beim Verwenden der Acoustics-Komponente in einem leeren Akteur](media/acoustics-component-usage.png)

       
    <sup>1</sup> Fügen Sie dem Akteur eine Acoustics Audio-Komponente hinzu. Diese Komponente erweitert die Wwise-Audiokomponente um Funktionen für Projekt Akustik.
        
    <sup>2</sup> Die Option **Play on Start** (Beim Start wiedergeben) ist standardmäßig ausgewählt. Durch diese Einstellung wird beim Starten des Levels ein zugeordnetes Wwise-Ereignis ausgelöst.</li>
         
    <sup>3</sup> Aktivieren Sie das Kontrollkästchen **Show Acoustics Parameters** (Akustikparameter anzeigen), um Debuginformationen zur Quelle auf dem Bildschirm anzuzeigen.

    ![Bereich „Acoustics“ im Unreal-Editor für Soundquelle mit aktivierten Debugwerten](media/debug-values.png)

    <sup>4</sup> Weisen Sie mithilfe des üblichen Wwise-Workflows ein Wwise-Ereignis zu.
       
    <sup>5</sup> Stellen Sie sicher, dass die Option **Use Spatial Audio** (Raumklang verwenden) deaktiviert ist. Wenn Sie Projekt Akustik für eine bestimmte Audiokomponente verwenden, können Sie nicht gleichzeitig die Spatial Audio-Engine von Wwise für die Akustik verwenden.</li>
       
Sie sind fertig, Bewegen Sie sich in der Szene, und erkunden Sie die akustischen Effekte.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie das [Entwurfstutorial für Projekt Akustik in Unreal/Wwise](unreal-workflow.md) ein.
* Erfahren Sie mehr über [Bakingvorgänge](unreal-baking.md) für Ihre Spielszenen.
