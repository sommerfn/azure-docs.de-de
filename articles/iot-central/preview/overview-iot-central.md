---
title: Was ist Azure IoT Central? | Microsoft-Dokumentation
description: Azure IoT Central ist eine IoT-Anwendungsplattform, die das Erstellen von IoT-Lösungen vereinfacht und den Aufwand und die Kosten für IoT-Verwaltungsvorgänge und -Entwicklung senkt. Dieser Artikel enthält eine Übersicht über die Features von Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 11/12/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 5e84b8777fc7671a19b6d8974f1309eb5af35bd3
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048016"
---
# <a name="what-is-azure-iot-central-preview-features"></a>Was ist Azure IoT Central (Previewfunktionen)?

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> Die [IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md)-Funktionen in Azure IoT Central befinden sich derzeit in der Vorschauphase. Verwenden Sie für Produktionsworkloads keine IoT Central-[Anwendungsvorlage](../core/concepts-app-templates.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) mit aktivierter IoT Plug & Play-Funktion. Verwenden Sie für Produktionsumgebungen eine IoT Central-Anwendung, die auf Grundlage einer aktuellen, allgemein verfügbaren [Anwendungsvorlage](../core/concepts-app-templates.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) erstellt wurde.

IoT Central ist eine IoT-Anwendungsplattform, die Aufwand und Kosten für die Entwicklung, Verwaltung und Wartung von IoT-Lösungen auf Unternehmensniveau verringert. Die Entscheidung zur Entwicklung mit IoT Central gibt Ihnen die Möglichkeit, Zeit, Geld und Energie auf die Transformation Ihres Unternehmens mit IoT-Daten zu konzentrieren, anstatt sich lediglich mit der Wartung und Aktualisierung einer komplexen und sich ständig weiterentwickelnden IoT-Infrastruktur zu beschäftigen.

Mit der Webbenutzeroberfläche können Sie Gerätebedingungen überwachen, Regeln erstellen und Millionen von Geräten und zugehöriger Daten über ihren gesamten Lebenszyklus hinweg verwalten. Darüber hinaus können Sie auf Geräteerkenntnisse reagieren, indem Sie die IoT-Intelligenz auf Branchenanwendungen erweitern.

In diesem Artikel werden folgende Punkte für IoT Central behandelt:

- Typische Personas eines Projekts
- Erstellung einer Anwendung
- Herstellung einer Verbindung zwischen Ihren Geräten und Ihrer Anwendung
- Verwaltung Ihrer Anwendung
- Azure IoT Edge-Funktionen in IoT Central
- Vorgehensweise zum Verbinden Ihrer auf der Azure IoT Edge-Runtime basierenden Geräte mit Ihrer Anwendung

## <a name="known-issues"></a>Bekannte Probleme

> [!Note]
> Diese Probleme sind nur bei der IoT Central-Vorschauanwendung bekannt.

- Regeln unterstützen nicht alle Aktionen (nur E-Mail).
- Bei komplexen Typen werden Regeln, Analysen und Gerätegruppen nicht unterstützt.
- Der fortlaufende Datenexport unterstützt das Avro-Format nicht (Inkompatibilität).
- Simulierte Geräte unterstützen nicht alle komplexen Typen.
- GeoJSON wird derzeit nicht unterstützt.
- Kartenkachel wird derzeit nicht unterstützt.
- Aufträge unterstützen keine komplexen Typen.
- Array-Schematypen werden nicht unterstützt.
- Der Export von Anwendungsvorlagen und die Anwendungskopie werden nicht unterstützt.
- Nur das C-Geräte-SDK und die Node.js-Geräte-SDKs und -Dienst-SDKs werden unterstützt.
- Nur in den Regionen „Europa, Norden“ und „USA, Mitte“ verfügbar
- Für die Gerätefunktionsmodelle müssen alle Schnittstellen inline in derselben Datei definiert werden.

## <a name="personas"></a>Personas

In der Dokumentation von IoT Central werden vier Personas verwendet, die mit einer IoT Central-Anwendung interagieren:

- Ein _Lösungsentwickler_ definiert die Arten von Geräten, die eine Verbindung mit der Anwendung herstellen, und passt die Anwendung für den Bediener an.
- Ein _Bediener_ verwaltet die mit der Anwendung verbundenen Geräte.
- Ein _Administrator_ kümmert sich um administrative Aufgaben – etwa um die [Verwaltung von Benutzern und Rollen](howto-administer.md) innerhalb der Anwendung.
- Ein _Geräteentwickler_ erstellt den Code, der auf einem mit der Anwendung verbundenen Gerät oder IoT Edge-Modul ausgeführt wird.

## <a name="create-your-iot-central-application"></a>Erstellen der IoT Central-Anwendung

Als Lösungsentwickler verwenden Sie IoT Central, um eine benutzerdefinierte, in der Cloud gehostete IoT-Lösung für Ihre Organisation zu erstellen. Eine benutzerdefinierte IoT-Lösung umfasst in der Regel Folgendes:

- Eine cloudbasierte Anwendung, die Telemetriedaten von Ihren Geräten empfängt und die Verwaltung dieser Geräte ermöglicht
- Mehrere Geräte, auf denen benutzerdefinierter Code ausgeführt wird und die mit Ihrer cloudbasierten Anwendung verbunden sind

Sie können eine neue IoT Central-Anwendung schnell bereitstellen und anschließend in Ihrem Browser an Ihre spezifischen Anforderungen anpassen. Als Lösungsentwickler können Sie mithilfe der webbasierten Tools eine _Gerätevorlage_ für die Geräte erstellen, die eine Verbindung mit Ihrer Anwendung herstellen. Eine Gerätevorlage ist die Blaupause zum Definieren der Merkmale und des Verhaltens eines Gerätetyps. Hierzu zählt beispielsweise Folgendes:

- Telemetriedaten, die das Gerät sendet
- Geschäftliche Eigenschaften, die ein Bediener ändern kann
- Geräteeigenschaften, die von einem Gerät festgelegt werden und in der Anwendung schreibgeschützt sind
- Vom Bediener festgelegte Eigenschaften, die das Verhalten des Geräts bestimmen

Diese Gerätevorlage enthält Folgendes:

- Ein _Gerätefunktionsmodell_, das die Funktionen beschreibt, die ein Gerät implementieren soll, z. B. die gesendeten Telemetriedaten und die gemeldeten Eigenschaften.
- Cloudeigenschaften, die nicht auf dem Gerät gespeichert werden
- Anpassungen, Dashboards und Formulare, die Teil der IoT Central-Anwendung sind

### <a name="create-device-templates"></a>Erstellen von Gerätevorlagen

[IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md) ermöglicht IoT Central die Integration von Geräten, ohne dass Sie eingebetteten Gerätecode schreiben müssen. Das Herzstück von IoT Plug & Play ist ein Gerätefunktionsmodell-Schema, das Gerätefunktionen beschreibt. In einer IoT Central-Vorschauanwendung verwenden Gerätevorlagen diese IoT Plug & Play-Gerätefunktionsmodelle.

Als Lösungsentwickler haben Sie mehrere Möglichkeiten zum Erstellen von Gerätevorlagen:

- Entwerfen Sie die Gerätevorlage in IoT Central, und implementieren Sie dann das entsprechende Gerätefunktionsmodell in Ihrem Gerätecode.
- Importieren Sie ein Gerätefunktionsmodell aus dem [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat), und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie den Gerätecode aus dem Modell, und verbinden Sie Ihr Gerät mit Ihrer IoT Central-Anwendung. IoT Central ermittelt das Gerätefunktionsmodell aus einem Repository und erstellt eine einfache Gerätevorlage für Sie.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie Ihren Gerätecode aus dem Modell. Importieren Sie das Gerätefunktionsmodell manuell in Ihre IoT Central-Anwendung, und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.

Als Lösungsentwickler können Sie mit IoT Central Code für Testgeräte generieren, um Ihre Gerätevorlagen zu überprüfen.

### <a name="customize-the-ui"></a>Anpassen der Benutzeroberfläche

Als Lösungsentwickler können Sie auch die Benutzeroberfläche der IoT Central-Anwendung für die Bediener anpassen, die die Anwendung tagtäglich verwenden. Die Anpassungsmöglichkeiten für Lösungsentwickler umfassen Folgendes:

- Definieren des Layouts von Eigenschaften und Einstellungen für eine Gerätevorlage
- Konfigurieren benutzerdefinierter Dashboards, um Bediener bei der Gewinnung von Erkenntnissen sowie bei der schnelleren Behebung von Problemen zu unterstützen
- Konfigurieren benutzerdefinierter Analysen zur Untersuchung von Zeitreihendaten Ihrer verbundenen Geräte

## <a name="connect-your-devices"></a>Verbinden von Geräten

Nachdem der Ersteller die Arten von Geräten definiert hat, die eine Verbindung mit der Anwendung herstellen können, erstellt ein Geräteentwickler den Code, der auf den Geräten ausgeführt werden soll. Zur Erstellung des Gerätecodes verwenden Geräteentwickler quelloffene [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) von Microsoft. Mit umfassender Sprach-, Plattform- und Protokollunterstützung bieten diese SDKs alles, was Sie benötigen, um Ihre Geräte mit Ihrer IoT Central-Anwendung zu verbinden. Die SDKs helfen Ihnen beim Implementieren der folgenden Gerätefunktionen:

- Erstellen einer sicheren Verbindung
- Senden von Telemetriedaten
- Melden des Status
- Empfangen von Konfigurationsupdates

Weitere Informationen finden Sie im Blogbeitrag [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don’t](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Vorteile der Azure IoT SDKs und vermeidbare Fehler, wenn Sie die SDKs nicht verwenden).

### <a name="azure-iot-edge-devices"></a>Azure IoT Edge-Geräte

Ebenso wie Geräte, die mit den [Azure IoT-SDKs](https://github.com/Azure/azure-iot-sdks) erstellt wurden, können Sie auch [Azure IoT Edge Geräte](../../iot-edge/about-iot-edge.md) mit einer IoT Central-Anwendung verbinden. Azure IoT Edge ermöglicht die Ausführung von Cloud Intelligence und benutzerdefinierter Logik direkt auf IoT-Geräten, die von IoT Central verwaltet werden. Die IoT Edge-Laufzeit ermöglicht Ihnen Folgendes:

- Installieren und Aktualisieren von Workloads auf dem Gerät
- Aufrechterhalten von Azure IoT Edge-Sicherheitsstandards auf dem Gerät
- Sicherstellen, dass die IoT Edge-Module immer ausgeführt werden
- Melden der Modulintegrität an die Cloud für die Remoteüberwachung
- Verwalten der Kommunikation zwischen nachgeschalteten Blattknotengeräten und einem IoT Edge-Gerät, zwischen Modulen auf einem IoT Edge-Gerät sowie zwischen einem IoT Edge-Gerät und der Cloud.

Weitere Informationen finden Sie unter [Azure IoT Edge-Geräte und IoT Central](./concepts-architecture.md#azure-iot-edge-devices).

## <a name="manage-your-application"></a>Verwalten Ihrer Anwendung

IoT Central-Anwendungen werden vollständig von Microsoft gehostet, was den Verwaltungsaufwand für Ihre Anwendungen verringert.

Als Bediener verwenden Sie die Benutzeroberfläche der IoT Central-Anwendung, um die Geräte in Ihrer IoT Central-Lösung zu verwalten. Bediener führen Aufgaben wie die folgenden aus:

- Überwachen der mit der Anwendung verbundenen Geräte
- Behandeln und Beheben von Problemen mit Geräten
- Bereitstellen neuer Geräte

Als Lösungsentwickler können Sie benutzerdefinierte Regeln und Aktionen definieren, die über Datenstreaming von verbundenen Geräten ausgeführt werden. Bediener können diese Regeln auf der Geräteebene aktivieren oder deaktivieren, um Aufgaben innerhalb der Anwendung zu steuern und zu automatisieren.

Administratoren verwalten über [Benutzerrollen und Berechtigungen](howto-administer.md) den Zugriff auf Ihre Anwendung.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über IoT Central verschafft haben, können Sie als Nächstes mit folgenden Schritten fortfahren:

- Informieren Sie sich über die Unterschiede zwischen [IoT Central und Azure IoT Solution Accelerators](../core/overview-iot-options.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).
- Machen Sie sich mit der [Benutzeroberfläche von Azure IoT Central](overview-iot-central-tour.md) vertraut.
- [Erstellen Sie eine Azure IoT Central-Anwendung.](quick-deploy-iot-central.md)
- Weitere Informationen zu [IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md)
- Weitere Informationen zum [Erstellen einer Azure IoT Edge-Gerätevorlage](./tutorial-define-edge-device-type.md)
