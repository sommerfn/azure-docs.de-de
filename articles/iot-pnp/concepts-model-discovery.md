---
title: Implementieren der IoT Plug & Play-Vorschaumodellermittlung | Microsoft-Dokumentation
description: Erfahren Sie als Lösungsentwickler, wie Sie die IoT Plug & Play-Vorschaumodellermittlung in Ihre Lösung implementieren können.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 07/17/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e4ab1d45e27762ef05ab7ec74c98ab0b0b934cbf
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879230"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>Implementieren der IoT Plug & Play-Vorschaumodellermittlung in eine IoT-Lösung

In diesem Artikel wird beschrieben, wie Sie als Lösungsentwickler die IoT Plug & Play-Vorschaumodellermittlung in eine IoT-Lösung implementieren können.  Die IoT Plug & Play-Modellermittlung beschreibt, wie IoT Plug & Play-Geräte ihre unterstützten Funktionsmodelle und Schnittstellen identifizieren und wie eine IoT-Lösung diese Funktionsmodelle und Schnittstellen abruft.

Es gibt zwei große Kategorien von IoT-Lösungen: zweckgebundene Lösungen, die mit einem bekannten Satz von IoT Plug & Play-Geräten funktionieren, und modellbasierte Lösungen, die mit jedem IoT Plug & Play-Gerät funktionieren.

In diesem Konzept Artikel wird beschrieben, wie Sie die Modellermittlung in beiden Lösungstypen implementieren.

## <a name="model-discovery"></a>Modellermittlung

Wenn sich ein IoT Plug & Play-Gerät zum ersten Mal mit Ihrem IoT Hub verbindet, sendet es eine Telemetriemeldung mit Modellinformationen. Diese Meldung enthält die IDs der Schnittstellen, die vom Gerät implementiert werden. Damit Ihre Lösung mit dem Gerät funktioniert, müssen diese IDs aufgelöst und die Definitionen für jede Schnittstelle abgerufen werden.

Hier finden Sie die Schritte, die ein IoT Plug & Play-Gerät bei der Verwendung des Diensts für die Gerätebereitstellung (DPS) zum Herstellen einer Verbindung mit einem Hub ausführt:

1. Wenn das Gerät eingeschaltet wird, verbindet es sich mit dem globalen Endpunkt für den DPS und authentifiziert sich mit einer der zulässigen Methoden.
1. DPS authentifiziert das Gerät und sucht die Regel, die angibt, welchem IoT Hub das Gerät zugewiesen werden soll. DPS registriert dann das Gerät bei diesem Hub.
1. DPS gibt eine IoT Hub-Verbindungszeichenfolge an das Gerät zurück.
1. Das Gerät sendet dann eine Ermittlungstelemetriemeldung an Ihren IoT Hub. Diese Meldung enthält die IDs der Schnittstellen, die vom Gerät implementiert werden.
1. Das IoT Plug & Play-Gerät kann jetzt mit einer Lösung arbeiten, die Ihren IoT-Hub nutzt.

Wenn sich das Gerät direkt mit Ihrem IoT Hub verbindet, erfolgt die Verbindung über eine Verbindungszeichenfolge, die im Gerätecode eingebettet ist. Das Gerät sendet dann eine Ermittlungstelemetriemeldung an Ihren IoT Hub.

Weitere Informationen zur Telemetriemeldung mit Modellinformationen finden Sie in den Informationen zur [ModelInformation](concepts-common-interfaces.md)-Schnittstelle.

### <a name="purpose-built-iot-solutions"></a>Zweckgebundene IoT-Lösungen

Eine zweckgebundene IoT-Lösung arbeitet mit einem bekannten Satz von IoT Plug & Play-Gerätefunktionsmodellen und Schnittstellen.

Sie erhalten das Funktionsmodell und Schnittstellen für die Geräte, die sich vorab mit Ihrer Lösung verbinden. Bereiten Sie Ihre Lösung mit den folgenden Schritten vor:

1. Speichern Sie die JSON-Dateien der Schnittstelle in Azure an einem Speicherort, an dem Ihre Lösung sie auslesen kann.
1. Schreiben Sie Logik in Ihre IoT-Lösung, basierend auf den erwarteten IoT Plug & Play-Funktionsmodellen und Schnittstellen.
1. Abonnieren Sie Benachrichtigungen von dem IoT Hub, den Ihre Lösung verwendet.

Wenn Sie eine Benachrichtigung zu einer neuen Geräteverbindung erhalten, führen Sie die folgenden Schritte aus:

1. Lesen Sie die Ermittlungstelemetriemeldung, um die IDs der vom Gerät implementierten Funktionsmodelle und Schnittstellen abzurufen.
1. Vergleichen Sie die ID des Funktionsmodells mit den IDs der von Ihnen im Vorfeld gespeicherten Funktionsmodellen.
1. Nun wissen Sie, mit welchem Gerätetyp eine Verbindung hergestellt wurde. Verwenden Sie die zuvor geschriebene Logik, damit Benutzer entsprechend mit dem Gerät interagieren können.

### <a name="model-driven-solutions"></a>Modellgesteuerte Lösungen

Eine modellgesteuerte IoT-Lösung kann mit jedem IoT Plug & Play-Gerät arbeiten. Die Entwicklung einer modellgesteuerten IoT-Lösung ist komplexer. Der Vorteil ist jedoch, dass Ihre Lösung mit allen zukünftig hinzugefügten Geräten funktioniert.

Um eine modellgesteuerte IoT-Lösung zu erstellen, müssen Sie eine Logik für die IoT Plug and Play-Schnittstellenprimitive erstellen: Telemetrie, Eigenschaften und Befehle. Die Logik Ihrer IoT-Lösung stellt ein Gerät dar, indem sie mehrere Telemetrie-, Eigenschafts- und Befehlsfunktionen kombiniert.

Ihre Lösung muss ebenfalls Benachrichtigungen von dem von ihr verwendeten IoT-Hub abonnieren.

Wenn Ihre Lösung eine Benachrichtigung zu einer neuen Geräteverbindung erhält, führen Sie die folgenden Schritte aus:

1. Lesen Sie die Ermittlungstelemetriemeldung, um die IDs der vom Gerät implementierten Funktionsmodelle und Schnittstellen abzurufen.
1. Lesen Sie für jede ID die vollständige JSON-Datei aus, um die Funktionen des Geräts zu ermitteln.
1. Überprüfen Sie, ob jede Schnittstelle in den von Ihnen erstellten Caches für die Speicherung der zuvor von Ihrer Lösung abgerufenen JSON-Dateien vorhanden ist.
1. Überprüfen Sie dann, ob im globalen Modellrepository eine Schnittstelle mit dieser ID vorhanden ist. Weitere Informationen finden Sie in unserem [globalen Modellrepository](howto-manage-models.md).
1. Wenn die Schnittstelle nicht im globalen Modellrepository vorhanden ist, suchen Sie sie in allen Ihrer Lösung bekannten privaten Modellrepositorys. Für den Zugriff auf ein privates Modellrepository benötigen Sie eine Verbindungszeichenfolge. Weitere Informationen finden Sie in unserem [privaten Modellrepository](howto-manage-models.md).
1. Wenn Sie weder im globalen noch in einem privaten Modellrepository alle Schnittstellen finden, können Sie überprüfen, ob das Gerät die Schnittstellendefinition bereitstellen kann. Ein Gerät kann die Standardschnittstelle [ModelDefinition](concepts-common-interfaces.md) implementieren, um Informationen darüber zu veröffentlichen, wie Schnittstellendateien mit einem Befehl abgerufen werden.
1. Wenn Sie JSON-Dateien für jede vom Gerät implementierte Schnittstelle gefunden haben, können Sie die Funktionen des Geräts auflisten. Verwenden Sie die zuvor geschriebene Logik, damit Benutzer mit dem Gerät interagieren können.
1. Sie können die API der digitalen Zwillinge jederzeit aufrufen, um die Modell-ID und die Schnittstellen-IDs für das Gerät abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Modellermittlung einer IoT-Lösung erfahren haben, können Sie sich mit der [Azure IoT-Plattform](overview-iot-plug-and-play.md) vertraut machen, um andere Funktionen für Ihre Lösung zu nutzen.
