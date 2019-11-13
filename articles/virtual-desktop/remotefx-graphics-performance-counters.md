---
title: Diagnostizieren von Problemen mit der Grafikleistung in Remotedesktop – Azure
description: Dieser Artikel beschreibt, wie Sie Leistungsindikatoren in RemoteFX-Grafik in Remotedesktopprotokoll-Sitzungen verwenden, um Leistungsprobleme mit Grafiken in Windows Virtual Desktop zu diagnostizieren.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/23/2019
ms.author: helohr
ms.openlocfilehash: c41a433ee19969546e1db2aa583c72ed166b7ebf
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607466"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Diagnostizieren von Problemen mit der Grafikleistung in Remotedesktop

Um Qualitätsprobleme bei Ihren Remotesitzungen zu diagnostizieren, wurden im Systemmonitor im Abschnitt „RemoteFX-Grafik“ Leistungsindikatoren bereitgestellt. Dieser Artikel hilft Ihnen beim Erkennen und Beheben von Leistungsengpässen im Zusammenhang mit Grafiken während Remotedesktopprotokoll-Sitzungen (RDP) mithilfe dieser Leistungsindikatoren.

## <a name="find-your-remote-session-name"></a>Suchen nach dem Namen der Remotesitzung

Sie benötigen den Namen der Remotesitzung, um Grafikleistungsindikatoren zu identifizieren. Befolgen Sie die Anweisungen in diesem Abschnitt, um Ihre Instanz jedes Leistungsindikators zu identifizieren.

1. Öffnen Sie die Windows-Eingabeaufforderung aus der Remotesitzung.
2. Führen Sie den Befehl **qwinsta** aus, und suchen Sie den Namen Ihrer Sitzung.
    - Wenn die Sitzung auf einem virtuellen Computer (VM) mit mehreren Sitzungen gehostet wird: Ihrer Instanz jedes Leistungsindikators wird das gleiche Suffix angehängt, das auch Ihr Sitzungsname hat (z.B. “rdp-tcp 37”).
    - Wenn die Sitzung auf einem virtuellen Computer gehostet wird, der virtuelle Grafikprozessoren (vGPU) unterstützt: Ihre Instanz jedes Leistungsindikators wird auf dem Server und nicht auf Ihrem virtuellen Computer gespeichert. Ihre Instanzen der Leistungsindikatoren enthalten den Namen des virtuellen Computers anstelle der Zahl im Sitzungsnamen, z.B. „Win8 Enterprise VM“.

>[!NOTE]
> Während Leistungsindikatoren RemoteFX in ihrem Namen enthalten, sind in vGPU-Szenarien auch Remotedesktopgrafiken enthalten.

## <a name="access-performance-counters"></a>Zugreifen auf Leistungsindikatoren

Nachdem Sie den Namen der Remotesitzung ermittelt haben, folgen Sie den nachstehenden Anweisungen, um die Leistungsindikatoren in RemoteFX-Grafik für die Remotesitzung zu erfassen.

1. Wählen Sie **Start** > **Verwaltungstools** > **Systemmonitor** aus.
2. Erweitern Sie im Dialogfeld **Systemmonitor** die Option **Überwachungstools**, und wählen Sie **Systemmonitor** und anschließend **Hinzufügen** aus.
3. Erweitern Sie im Dialogfeld **Leistungsindikatoren hinzufügen** in der Liste **Verfügbare Leistungsindikatoren** den Abschnitt für RemoteFX-Grafik.
4. Wählen Sie die zu überwachenden Leistungsindikatoren aus.
5. Wählen Sie in der Liste **Instanzen des ausgewählten Objekts** die jeweiligen Instanzen aus, die für die ausgewählten Leistungsindikatoren überwacht werden sollen, und wählen Sie dann **Hinzufügen** aus. Zum Auswählen aller verfügbaren Leistungsindikatorinstanzen wählen Sie **Alle Instanzen** aus.
6. Klicken Sie nach dem Hinzufügen der Leistungsindikatoren auf **OK**.

Die ausgewählten Leistungsindikatoren werden auf dem Bildschirm „Systemmonitor“ angezeigt.

>[!NOTE]
>Jede aktive Sitzung auf einem Host verfügt über eine eigene Instanz jedes Leistungsindikators.

## <a name="diagnose-issues"></a>Diagnostizieren von Problemen

Leistungsprobleme im Zusammenhang mit Grafiken werden im Allgemeinen in vier Kategorien unterteilt:

- Niedrige Framerate
- Zufällige Unterbrechungen
- Hohe Eingabelatenz
- Schlechte Framequalität

### <a name="addressing-low-frame-rate-random-stalls-and-high-input-latency"></a>Behandeln von niedriger Framerate, zufälligen Unterbrechungen und hoher Eingabelatenz.

Überprüfen Sie zuerst den Leistungsindikator „Ausgabeframes/Sekunde“. Er misst die Anzahl der Frames, die dem Client zur Verfügung gestellt werden. Wenn dieser Wert niedriger als beim Leistungsindikator „Eingabeframes/Sekunde“ ist, werden Frames übersprungen. Zur Ermittlung des Engpasses verwenden Sie die Leistungsindikatoren „Übersprungene Frames/Sekunde“.

Es gibt drei Leistungsindikatoren des Typs „Übersprungene Frames/Sekunde“:

- Übersprungene Frames/Sekunde (Unzureichende Serverressourcen)
- Übersprungene Frames/Sekunde (Unzureichende Netzwerkressourcen)
- Übersprungene Frames/Sekunde (Unzureichende Clientressourcen)

Ein hoher Wert bei einem der Leistungsindikatoren „Übersprungene Frames/Sekunde“ zeigt an, dass das Problem mit der Ressource in Zusammenhang steht, die vom Leistungsindikator überwacht wird. Wenn beispielsweise der Client Frames nicht mit der gleichen Rate decodiert und darstellt, mit der Frames vom Server bereitgestellt werden, weist der Leistungsindikator „Übersprungene Frames/Sekunde (Unzureichende Clientressourcen)“ einen hohen Wert auf.

Wenn der Wert des Leistungsindikators „Ausgabeframes/Sekunde“ dem des Leistungsindikators „Eingabeframes/Sekunde“ entspricht, aber dennoch ungewöhnliche Verzögerungen oder Unterbrechungen auftreten, kann die durchschnittliche Codierungszeit die Ursache des Problems sein. Die Codierung ist ein synchroner Vorgang, der auf dem Server im Szenario mit einer Einzelsitzung (vGPU) und auf dem virtuellen Computer im Szenario mit mehreren Sitzungen stattfindet. Die durchschnittliche Codierungszeit sollte unter 33 ms liegen. Beträgt die durchschnittliche Codierungszeit weniger als 33 ms und treten dennoch Leistungsprobleme auf, kann ein Problem mit der verwendeten App oder dem verwendeten Betriebssystem vorliegen.

Weitere Informationen zum Diagnostizieren von Problemen im Zusammenhang mit einer App finden Sie unter [Leistungsindikatoren für verzögerte Benutzereingabe](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

Da RDP eine durchschnittliche Codierungszeit von 33 ms unterstützt, wird eine Eingabeframerate von bis zu 30 Frames pro Sekunde unterstützt. Beachten Sie, dass 33 ms die maximale unterstützte Framerate ist. In vielen Fällen ist die Framerate für den Benutzer geringer, je nachdem, wie oft ein Frame für RDP von der Quelle bereitgestellt wird. Beispielsweise erfordern Aufgaben wie das Ansehen eines Videos eine volle Eingangsbildrate von 30 Frames/Sekunde, aber weniger rechenintensive Aufgaben wie das seltene Bearbeiten eines Dokuments führen ohne Beeinträchtigung der Benutzerfreundlichkeit zu einem viel niedrigeren Wert für die Eingangsbildrate in Frames/Sekunde.

### <a name="addressing-poor-frame-quality"></a>Behandeln von schlechter Framequalität

Mit dem Leistungsindikator „Framequalität“ können Sie Probleme der Framequalität diagnostizieren. Dieser Leistungsindikator gibt die Qualität des Ausgabeframes als Prozentsatz der Qualität des Quellframes an. Der Qualitätsverlust ist möglicherweise auf RemoteFX zurückzuführen oder bereits in der Grafikquelle vorhanden sein. Wenn RemoteFX den Qualitätsverlust verursacht, kann das Problem fehlende Netzwerk- oder Serverressourcen zum Senden von Inhalten mit höherer Genauigkeit sein.

## <a name="mitigation"></a>Lösung

Wenn Serverressourcen den Engpass verursachen, versuchen Sie die Leistung mit einem der folgenden Ansätze zu verbessern:

- Reduzieren Sie die Anzahl von Sitzungen pro Host.
- Erhöhen Sie die Speicher- und Computeressourcen auf dem Server.
- Verringern Sie die Auflösung der Verbindung.

Wenn Netzwerkressourcen den Engpass verursachen, versuchen Sie die Netzwerkverfügbarkeit pro Sitzung mit einem der folgenden Ansätze zu verbessern:

- Reduzieren Sie die Anzahl von Sitzungen pro Host.
- Verwenden Sie ein Netzwerk mit höherer Bandbreite.
- Verringern Sie die Auflösung der Verbindung.

Wenn Clientressourcen den Engpass verursachen, versuchen Sie die Leistung mit einem der folgenden Ansätze zu verbessern:

- Installieren Sie den neuesten Remotedesktopclient.
- Erhöhen Sie die Speicher- und Computeressourcen auf dem Clientcomputer.

> [!NOTE]
> Der Leistungsindikator „Quellframes/Sekunde“ wird derzeit nicht unterstützt. Aktuell zeigt der Leistungsindikator „Quellframes/Sekunde“ immer 0 an.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen eines GPU-optimierten virtuellen Azure-Computers finden Sie unter [Konfigurieren der Beschleunigung durch Graphics Processing Units (GPU) für die Windows Virtual Desktop-Umgebung](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Eine Übersicht über die Problembehandlung und Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Weitere Informationen zum Dienst finden Sie unter [Windows Desktop-Umgebung](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
