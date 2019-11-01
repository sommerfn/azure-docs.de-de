---
title: Einführung in Azure IoT (Internet der Dinge)
description: Einführung in die Grundlagen von Azure IoT und der IoT-Dienste, einschließlich Beispielen zur Veranschaulichung der Anwendung von IoT.
author: robinsh
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 10/11/2018
ms.author: robinsh
ms.openlocfilehash: 2f46ebcabd98c7a8c3376157c72da9ec5ed424a4
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935231"
---
# <a name="what-is-azure-internet-of-things-iot"></a>Was ist Azure IoT (Internet der Dinge)?

Azure IoT (Internet der Dinge) ist eine Sammlung von Clouddiensten, die von Microsoft verwaltet werden und Milliarden von IoT-Ressourcen verbinden, überwachen und steuern. Vereinfacht ausgedrückt besteht eine IoT-Lösung aus einem oder mehreren IoT-Geräten und einem oder mehreren in der Cloud ausgeführten Back-End-Diensten, die miteinander kommunizieren. 

In diesem Artikel werden die Grundlagen von IoT erörtert, Anwendungsfälle vorgestellt und die acht verfügbaren separaten Dienste erklärt. Wenn Sie wissen, welche Optionen verfügbar sind, können Sie entscheiden, mit welcher Option Sie sich zum Entwerfen Ihres Szenarios näher befassen möchten.

## <a name="introduction"></a>Einführung

Eine IoT-Lösung umfasst folgende Hauptkomponenten: Geräte, Back-End-Dienste und die Kommunikation zwischen diesen beiden Komponenten. 

### <a name="iot-devices"></a>IoT-Geräte

Geräte bestehen in der Regel aus einer Leiterplatte mit Sensoren, die mit dem Internet verbunden sind. Viele Geräte kommunizieren über einen WLAN-Chip. Hier sind einige Beispiele für IoT-Geräte:

* Drucksensoren an einer räumlich entfernten Ölpumpe
* Temperatur- und Luftfeuchtigkeitssensoren in einer Klimaanlage
* Beschleunigungsmesser in einem Fahrstuhl
* Präsenzmelder in einem Raum

Zwei Geräte, die häufig zum Erstellen von Prototypen verwendet werden, sind das einfache MXChip IoT DevKit von Microsoft und Raspberry PI-Geräte. Das MXChip DevKit verfügt über integrierte Sensoren für Temperatur, Druck und Feuchtigkeit sowie über ein Gyroskop, einen Beschleunigungsmesser, ein Magnetometer und einen WLAN-Chip. Raspberry PI ist ein IoT-Gerät, an das verschiedenste Sensoren angefügt werden können, sodass Sie genau die für Ihr Szenario benötigten Produkte auswählen können. 

Weitere Informationen zu verfügbaren IoT-Geräten finden Sie im branchenweit größten [Katalog mit für IoT zertifizierten Geräten](https://catalog.azureiotsolutions.com/alldevices).

Die [IoT-Geräte-SDKs](../iot-hub/iot-hub-devguide-sdks.md) ermöglichen Ihnen das Erstellen von Apps, die auf Ihren Geräten ausgeführt werden, damit sie die erforderlichen Aufgaben ausführen können. Mit den SDKs können Sie Telemetriedaten an Ihren IoT-Hub senden, Nachrichten und Updates vom IoT-Hub empfangen usw.

### <a name="communication"></a>Kommunikation

Ihr Gerät kann in beide Richtungen mit Back-End-Diensten kommunizieren. Hier sind einige Beispiele für Methoden, mit denen das Gerät mit der Back-End-Lösung kommunizieren kann.

#### <a name="examples"></a>Beispiele 

* Ihr Gerät sendet alle fünf Minuten die Temperatur von einem Kühllaster an einen IoT Hub. 

* Der Back-End-Dienst kann das Gerät zum häufigeren Senden von Telemetriedaten auffordern, um ein Problem zu diagnostizieren. 

* Ihr Gerät kann Warnungen basierend auf den von den Sensoren ausgelesenen Werten senden. Beispiel: Bei der Überwachung eines Chargenreaktors in einer Chemieanlage sollte eine Warnung gesendet werden, wenn die Temperatur einen bestimmten Wert überschreitet.

* Ihr Gerät kann Informationen an ein Dashboard zur Anzeige für Bedienpersonal senden. Beispielsweise kann ein Kontrollraum in einer Raffinerie die Temperatur und den Druck der einzelnen Rohre sowie die Durchflussmenge eines bestimmten Rohrs anzeigen, sodass die Bediener es beobachten können. 

Diese und weitere Aufgaben können mithilfe der [IoT-Geräte-SDKs](../iot-hub/iot-hub-devguide-sdks.md) implementiert werden.

#### <a name="connection-considerations"></a>Überlegungen zur Verbindung

Das Herstellen von sicheren und zuverlässigen Verbindungen zwischen Geräten ist bei IoT-Lösungen häufig die größte Herausforderung. Der Grund hierfür ist, dass IoT-Geräte im Vergleich zu anderen Clients, z.B. Browsern und mobilen Apps, andere Merkmale aufweisen. Für IoT-Geräte gilt beispielsweise Folgendes:

* Es handelt sich häufig um eingebettete Systeme ohne menschlichen Bediener (im Gegensatz zu einem Smartphone).

* Sie können an Remotestandorten bereitgestellt werden, an denen der physische Zugriff mit hohen Kosten verbunden ist.

* Sie sind unter Umständen nur über das Back-End der Lösung erreichbar. Es gibt keine andere Möglichkeit zur Interaktion mit dem Gerät.

* Sie verfügen ggf. über eingeschränkte Stromversorgungs- und Verarbeitungsressourcen.

* Sie verfügen ggf. über intermittierende, langsame oder teure Netzwerkverbindungen.

* Sie müssen unter Umständen proprietäre, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.

### <a name="back-end-services"></a>Back-End-Dienste 

Hier sind einige der Funktionen, die ein Back-End-Dienst bereitstellen kann.

* Bedarfsgesteuertes Empfangen von Telemetriedaten von Ihren Geräten und Bestimmen, wie diese Daten verarbeitet und gespeichert werden sollen

* Analysieren der Telemetriedaten entweder in Echtzeit oder später, um Erkenntnisse zu gewinnen

* Senden von Befehlen aus der Cloud an ein bestimmtes Gerät 

* Bereitstellen von Geräten und Steuern, welche Geräte eine Verbindung mit Ihrer Infrastruktur herstellen können

* Kontrollieren des Zustands Ihrer Geräte und Überwachung deren Aktivitäten

In einem Szenario mit der vorhersagbaren Wartung werden mit dem Cloud-Back-End beispielsweise Telemetrieverlaufsdaten gespeichert. Von der Lösung werden diese Daten verwendet, um mögliches anomales Verhalten für bestimmte Pumpen zu identifizieren, bevor ein echtes Problem entsteht. Per Datenanalyse kann identifiziert werden, dass die Vorbeugungslösung darin besteht, einen Befehl zurück an das Gerät zu senden, um eine Korrekturmaßnahme durchzuführen. Bei diesem Prozess wird eine automatisierte Feedbackschleife zwischen dem Gerät und der Cloud generiert, mit der die Effizienz der Lösung deutlich gesteigert wird.

## <a name="an-iot-example"></a>Ein IoT-Beispiel

Hier ist ein Beispiel dafür, wie ein Unternehmen IoT verwendet, um Millionen von Dollar zu sparen. 

Es handelt sich um eine riesige Rinderfarm mit hunderttausenden von Kühen. Es ist sehr aufwendig, den Überblick über so viele Kühe zu behalten, um zu wissen, wie es ihnen geht, da man dafür viel herumfahren muss. Daher wurden alle Kühe mit einem Sensor versehen, der Daten wie GPS-Koordinaten und Temperatur an einen Back-End-Dienst sendet. Diese Daten werden in eine Datenbank geschrieben.

Ein Analytikdienst überprüft dann die eingehenden Daten und analysiert sie für jede Kuh, um Fragen wie die folgenden zu prüfen:

* Hat die Kuh Fieber? Wie lange hat die Kuh schon Fieber? Wenn es mehr als einen Tag ist, werden die GPS-Koordinaten abgerufen, die Kuh wird gesucht und gegebenenfalls mit Antibiotika behandelt. 

* Befindet sich die Kuh länger als einen Tag an derselben Stelle? Wenn dies der Fall ist, werden die GPS-Koordinaten abgerufen, und die Kuh wird gesucht. Ist die Kuh von einer Klippe gefallen? Ist die Kuh verletzt? Benötigt die Kuh Hilfe? 

Durch die Implementierung dieser IoT-Lösung ist es dem Unternehmen möglich, die Kühe schnell zu überprüfen und zu behandeln. Die Fahrtzeit, um nach den Kühen zu sehen, wurde dadurch erheblich reduziert, und das Unternehmen spart viel Geld. Weitere Praxisbeispiele dafür, wie Unternehmen IoT verwenden, finden Sie in den [technischen Fallstudien von Microsoft zu IoT](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured). 

## <a name="iot-services"></a>IoT-Dienste

In Azure gibt es mehrere IoT-bezogene Dienste, und fällt es häufig nicht leicht zu entscheiden, welchen Sie verwenden sollten. Einige Dienste wie IoT Central und die IoT Solution Accelerators bieten Vorlagen, mit denen Sie Ihre eigene Lösung erstellen und schnell loslegen können. Sie können andere verfügbare Dienste nutzen, um Ihre eigenen vollständigen Lösungen zu entwickeln. Alles hängt davon ab, wie viel Hilfe und wie viel Kontrolle Sie möchten. Hier ist eine Liste mit den verfügbaren Diensten und ihren Verwendungsmöglichkeiten.

1. [**IoT Central**](../iot-central/core/overview-iot-central.md): Mit dieser SaaS-Lösung können Sie Ihre IoT-Geräte verbinden, überwachen und verwalten. Zuerst wählen Sie eine Vorlage für Ihren Gerätetyp und erstellen und testen eine einfache IoT Central-Anwendung, die von den Bedienern des Geräts verwendet wird. Die IoT Central-Anwendung ermöglicht Ihnen auch das Überwachen der Geräte und das Bereitstellen von neuen Geräten. Dieser Dienst ist für einfach aufgebaute IoT-Lösungen vorgesehen, für die keine eingehende Dienstanpassung erforderlich ist. 

2. [**IoT Solution Accelerators**](/azure/iot-suite): Dies ist eine Sammlung von PaaS-Lösungen, mit denen Sie die Entwicklung einer IoT-Lösung beschleunigen können. Sie beginnen mit einer bereitgestellten IoT-Lösung und passen diese Lösung dann umfassend Ihren Anforderungen an. Sie benötigen Java- oder .NET-Kenntnisse, um das Back-End anzupassen, und JavaScript-Kenntnisse, um die Visualisierung anzupassen. 

3. [**IoT Hub**](/azure/iot-hub/): Dieser Dienst ermöglicht Ihnen das Verbinden Ihrer Geräte mit einem IoT Hub sowie das Überwachen und Kontrollieren von Milliarden von IoT-Geräten. Dies ist besonders nützlich, wenn Sie bidirektionale Kommunikation zwischen Ihren IoT-Geräten und Ihrem Back-End benötigen. Hierbei handelt es sich um den zugrunde liegenden Dienst für IoT Central und IoT-Solution Accelerators. 

4. [**IoT Hub Device Provisioning Service**](/azure/iot-dps/): Dies ist ein Hilfsdienst für IoT Hub, den Sie zur sicheren Bereitstellung von Geräten an Ihren IoT Hub verwenden können. Mit diesem Dienst können Sie problemlos Millionen von Geräten schnell bereitstellen, anstatt sie einzeln bereitzustellen. 

5. [**IoT Edge**](/azure/iot-edge/): Dieser Dienst basiert auf IoT Hub. Er kann zum Analysieren von Daten auf den IoT-Geräten anstatt in der Cloud verwendet werden. Durch das teilweise Verlagern Ihrer Workload in den Edge-Bereich müssen weniger Nachrichten in die Cloud gesendet werden. 

6. [**Azure Digital Twins**](../digital-twins/index.yml): Dieser Dienst ermöglicht Ihnen das Erstellen umfassender Modelle der physischen Umgebung. Damit können Sie die Beziehungen und Interaktionen zwischen Menschen, Räumen und Geräten modellieren. Beispielsweise können Sie den Wartungsbedarf für eine Fabrik vorhersagen, den Energiebedarf für ein Stromnetz in Echtzeit analysieren oder die Verwendung der verfügbaren Fläche für ein Büro optimieren.

7. [**Time Series Insights**](/azure/time-series-insights): Dieser Dienst ermöglicht Ihnen das Speichern, Visualisieren und Abfragen großer Mengen an Zeitreihendaten, die von IoT-Geräten generiert werden. Sie können diesen Dienst mit IoT Hub verwenden. 

8. [**Azure Maps**](/azure/azure-maps): Dieser Dienst stellt geografische Informationen für Webanwendungen und mobile Anwendungen bereit. Es gibt einen vollständigen Satz von REST-APIs sowie ein webbasiertes JavaScript-Steuerelement, die zum Erstellen von flexiblen Anwendungen genutzt werden können, die für Desktopanwendungen oder mobile Anwendungen für Apple- und Windows-Geräte funktionieren.

## <a name="next-steps"></a>Nächste Schritte

Einige konkrete Anwendungsfälle und die entsprechenden Architekturen finden Sie in den [technischen Fallstudien zu Microsoft Azure IoT](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured).

Einige Beispielprojekte, die Sie mit einem IoT-DevKit testen können, finden Sie im [IoT DevKit-Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/). 

Eine ausführlichere Erläuterung der verschiedenen Dienste deren Verwendung finden Sie unter [Azure IoT-Dienste und -Technologien](iot-services-and-technologies.md).

Ausführliche Informationen zur IoT-Architektur finden Sie unter [Microsoft Azure IoT Reference Architecture](https://aka.ms/iotrefarchitecture) (Microsoft Azure IoT-Referenzarchitektur).
