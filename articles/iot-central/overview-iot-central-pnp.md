---
title: Was ist Azure IoT Central? | Microsoft-Dokumentation
description: Azure IoT Central ist eine End-to-End-SaaS-Lösung zur Erstellung und Verwaltung benutzerdefinierter IoT-Lösungen. Dieser Artikel enthält eine Übersicht über die Features von Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/06/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 0675be988e7e9560560dd07338563c9fa22fcb4c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881649"
---
# <a name="what-is-azure-iot-central-preview-features"></a>Was ist Azure IoT Central (Previewfunktionen)?

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> Die [IoT Plug & Play](https://aka.ms/iot-pnp-docs)-Funktionen in Azure IoT Central befinden sich derzeit in der Vorschauphase. Verwenden Sie keine IoT Central-Anwendung mit aktivierter IoT Plug & Play-Funktion für Produktionsworkloads. Verwenden Sie für Produktionsumgebungen eine IoT Central-Anwendung, die auf der Grundlage einer aktuellen, allgemein verfügbaren Anwendungsvorlage erstellt wurde.

Azure IoT Central ist eine vollständig verwaltete IoT-SaaS-Lösung (Software-as-a-Service) zur einfachen Erstellung von Produkten, die die physische mit der digitalen Welt verbinden. Setzen Sie Ihre Vision für verbundene Produkte um, indem Sie:

- auf der Grundlage neuer Erkenntnisse, die Sie von verbundenen Geräten gewinnen, bessere Produkte und Erfahrungen für Ihre Kunden entwickeln.
- neue Verkaufschancen für Ihre Organisation generieren.

Im Vergleich zu einem typischen IoT-Projekt bietet Azure IoT Central Folgendes:

- Reduziert den Verwaltungsaufwand.
- Senkt Betriebs- und Fixkosten.
- Vereinfacht die Anpassung Ihrer Anwendung bei gleichzeitiger Nutzung von:
  - branchenführenden Technologien wie [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) und [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/).
  - für Unternehmen konzipierten Sicherheitsfeatures (etwa End-to-End-Verschlüsselung).

Das folgende Video bietet einen Überblick über Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

In diesem Artikel werden folgende Punkte für Azure IoT Central behandelt:

- Typische Personas eines Projekts
- Erstellung einer Anwendung
- Herstellung einer Verbindung zwischen Ihren Geräten und Ihrer Anwendung
- Verwaltung Ihrer Anwendung

## <a name="known-issues"></a>Bekannte Probleme

> [!Note]
> Diese Probleme sind nur bei der IoT Central Preview-Anwendung bekannt.

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
- Nur in ausgewählten Regionen verfügbar

## <a name="personas"></a>Personas

In der Dokumentation von Azure IoT Central werden vier Personas verwendet, die mit einer Azure IoT Central-Anwendung interagieren:

- Ein _Ersteller_ definiert die Arten von Geräten, die eine Verbindung mit der Anwendung herstellen, und passt die Anwendung für den Bediener an.
- Ein _Bediener_ verwaltet die mit der Anwendung verbundenen Geräte.
- Ein _Administrator_ kümmert sich um administrative Aufgaben – etwa um die Verwaltung von Benutzern und Rollen innerhalb der Anwendung.
- Ein _Geräteentwickler_ erstellt den Code, der auf einem mit der Anwendung verbundenen Gerät ausgeführt wird.

## <a name="create-your-azure-iot-central-application"></a>Erstellen Ihrer Azure IoT Central-Anwendung

Als Ersteller verwenden Sie Azure IoT Central, um eine benutzerdefinierte, in der Cloud gehostete IoT-Lösung für Ihre Organisation zu erstellen. Eine benutzerdefinierte IoT-Lösung umfasst in der Regel Folgendes:

- Eine cloudbasierte Anwendung, die Telemetriedaten von Ihren Geräten empfängt und die Verwaltung dieser Geräte ermöglicht
- Mehrere Geräte, auf denen benutzerdefinierter Code ausgeführt wird und die mit Ihrer cloudbasierten Anwendung verbunden sind

Sie können eine neue Azure IoT Central-Anwendung schnell bereitstellen und anschließend in Ihrem Browser an Ihre spezifischen Anforderungen anpassen. Als Ersteller können Sie mithilfe der webbasierten Tools eine _Gerätevorlage_ für die Geräte erstellen, die eine Verbindung mit Ihrer Anwendung herstellen. Eine Gerätevorlage ist die Blaupause zum Definieren der Merkmale und des Verhaltens eines Gerätetyps. Hierzu zählt beispielsweise Folgendes:

- Telemetriedaten, die das Gerät sendet
- Geschäftliche Eigenschaften, die ein Bediener ändern kann
- Geräteeigenschaften, die von einem Gerät festgelegt werden und in der Anwendung schreibgeschützt sind
- Eigenschaften, die von einem Bediener festgelegt werden, der das Verhalten des Geräts bestimmt

Diese Gerätevorlage enthält Folgendes:

- Ein _Gerätefunktionsmodell_, das die Funktionen beschreibt, die ein Gerät implementieren soll, z. B. die gesendeten Telemetriedaten und die gemeldeten Eigenschaften.
- Cloudeigenschaften, die nicht auf dem Gerät gespeichert werden
- Anpassungen, Dashboards und Formulare, die Teil der IoT Central-Anwendung sind

### <a name="create-device-templates"></a>Erstellen von Gerätevorlagen

[IoT Plug & Play](https://aka.ms/iot-pnp-docs) ermöglicht IoT Central die Integration von Geräten, ohne dass Sie eingebetteten Gerätecode schreiben müssen. Das Herzstück von IoT Plug & Play ist ein Gerätefunktionsmodell-Schema, das Gerätefunktionen beschreibt. In einer IoT Central Preview-Anwendung verwenden Gerätevorlagen diese IoT Plug & Play-Gerätefunktionsmodelle.

Als Ersteller haben Sie mehrere Möglichkeiten zum Erstellen von Gerätevorlagen:

- Entwerfen Sie die Gerätevorlage in IoT Central, und implementieren Sie dann das entsprechende Gerätefunktionsmodell in Ihrem Gerätecode.
- Importieren Sie ein Gerätefunktionsmodell aus dem [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat), und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie den Gerätecode aus dem Modell, und verbinden Sie Ihr Gerät mit Ihrer IoT Central-Anwendung. IoT Central ermittelt das Gerätefunktionsmodell aus einem Repository und erstellt eine einfache Gerätevorlage für Sie.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie Ihren Gerätecode aus dem Modell. Importieren Sie das Gerätefunktionsmodell manuell in Ihre IoT Central-Anwendung, und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die ihre IoT Central-Anwendung benötigt.

Als Ersteller können Sie mit IoT Central Code für Testgeräte generieren, um Ihre Gerätevorlagen zu überprüfen.

### <a name="customize-the-ui"></a>Anpassen der Benutzeroberfläche

Als Ersteller können Sie auch die Benutzeroberfläche der Azure IoT Central-Anwendung für die Bediener anpassen, die die Anwendung tagtäglich verwenden. Die Anpassungsmöglichkeiten für Ersteller umfassen Folgendes:

- Definieren des Layouts von Eigenschaften und Einstellungen für eine Gerätevorlage
- Konfigurieren benutzerdefinierter Dashboards, um Bediener bei der Gewinnung von Erkenntnissen sowie bei der schnelleren Behebung von Problemen zu unterstützen
- Konfigurieren benutzerdefinierter Analysen zur Untersuchung von Zeitreihendaten Ihrer verbundenen Geräte

## <a name="connect-your-devices"></a>Verbinden von Geräten

Nachdem der Ersteller die Arten von Geräten definiert hat, die eine Verbindung mit der Anwendung herstellen können, erstellt ein Geräteentwickler den Code, der auf den Geräten ausgeführt werden soll. Zur Erstellung des Gerätecodes verwenden Geräteentwickler quelloffene [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) von Microsoft. Mit umfassender Sprach-, Plattform- und Protokollunterstützung bieten diese SDKs alles, was Sie benötigen, um Ihre Geräte mit Ihrer Azure IoT Central-Anwendung zu verbinden. Die SDKs helfen Ihnen beim Implementieren der folgenden Gerätefunktionen:

- Erstellen einer sicheren Verbindung
- Senden von Telemetriedaten
- Melden des Status
- Empfangen von Konfigurationsupdates

Weitere Informationen finden Sie im Blogbeitrag [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don’t](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Vorteile der Azure IoT SDKs und vermeidbare Fehler, wenn Sie die SDKs nicht verwenden).

## <a name="manage-your-application"></a>Verwalten Ihrer Anwendung

Azure IoT Central-Anwendungen werden vollständig von Microsoft gehostet, was den Verwaltungsaufwand für Ihre Anwendungen verringert.

Als Bediener verwenden Sie die Benutzeroberfläche der Azure IoT Central-Anwendung, um die Geräte in Ihrer Azure IoT Central-Lösung zu verwalten. Bediener führen Aufgaben wie die folgenden aus:

- Überwachen der mit der Anwendung verbundenen Geräte
- Behandeln und Beheben von Problemen mit Geräten
- Bereitstellen neuer Geräte

Als Ersteller können Sie benutzerdefinierte Regeln und Aktionen definieren, die über Datenstreaming von verbundenen Geräten ausgeführt werden. Bediener können diese Regeln auf der Geräteebene aktivieren oder deaktivieren, um Aufgaben innerhalb der Anwendung zu steuern und zu automatisieren.

Administratoren verwalten über [Benutzerrollen und Berechtigungen](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) den Zugriff auf Ihre Anwendung.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über Azure IoT Central verschafft haben, können Sie als Nächstes mit folgenden Schritten fortfahren:

- Informieren Sie sich über die Unterschiede zwischen [Azure IoT Central und Azure IoT Solution Accelerators](overview-iot-options.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
- Machen Sie sich mit der [Benutzeroberfläche von Azure IoT Central](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) vertraut.
- [Erstellen Sie eine Azure IoT Central-Anwendung.](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- Machen Sie sich anhand einer Reihe von Tutorials mit Folgendem vertraut:
  - [Erstellen einer Gerätevorlage](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Tutorial für Ersteller)
  - [Hinzufügen von Regeln zum Automatisieren Ihrer Lösung](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Tutorial für Ersteller)
  - [überwachen Ihrer Geräte](tutorial-monitor-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Tutorial für Bediener)
  - [Hinzufügen eines Geräts zu Ihrer Lösung](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Tutorial für Bediener)
- Weitere Informationen zu [IoT Plug & Play](https://aka.ms/iot-pnp-docs)
