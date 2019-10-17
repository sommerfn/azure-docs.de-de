---
title: Installieren und Verwenden des Azure-IoT-Explorers | Microsoft-Dokumentation
description: Installieren Sie den Azure-IoT-Explorer, um mit den Geräten in IoT Plug & Play (Vorschau) zu interagieren, die mit Ihrem IoT-Hub verbunden sind.
author: miagdp
ms.author: miag
ms.date: 07/02/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: f59e449589c7f3027dc8a9daf9d8d12f04831dd7
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960568"
---
# <a name="install-and-use-azure-iot-explorer"></a>Installieren und Verwenden des Azure-IoT-Explorers

Der Azure-IoT-Explorer ist ein grafisches Tool, über das Sie mit Ihren Geräten in IoT Plug & Play (Vorschau) interagieren und diese Geräte testen können. Nach der Installation des Tools auf Ihrem lokalen Computer können Sie es verwenden, um eine Verbindung mit einem Gerät herzustellen. Sie können mit diesem Tool die Telemetriedaten anzeigen, die das Gerät sendet, Geräteeigenschaften einrichten und Befehle aufrufen.

In diesem Artikel erfahren Sie Folgendes:

- Installieren und Konfigurieren des Azure-IoT-Explorers
- Verwenden des Tools zum Interagieren mit Ihren Geräte und zum Testen Ihrer Geräte

## <a name="prerequisites"></a>Voraussetzungen

Um den Azure-IoT-Explorer zu verwenden, benötigen Sie Folgendes:

- Einen Azure IoT Hub. Es gibt viele Möglichkeiten, um einen IoT-Hub zu Ihrem Azure-Abonnement hinzuzufügen, z. B. das [Erstellen eines IoT-Hubs mit der Azure-Befehlszeilenschnittstelle](../iot-hub/iot-hub-create-using-cli.md). Sie benötigen die Verbindungszeichenfolge für den IoT-Hub, um den Azure-IoT-Explorer auszuführen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Ein in Ihrem IoT-Hub registriertes Gerät. Sie können den folgenden Azure CLI-Befehl verwenden, um ein Gerät zu registrieren. Ersetzen Sie die Platzhalter `{YourIoTHubName}` und `{YourDeviceID}` durch Ihre Werte:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Installieren des Azure-IoT-Explorers

Navigieren Sie auf GitHub zu den [Releases von Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases), und erweitern Sie die Liste der Assets, um die neueste Version anzuzeigen. Laden Sie die neueste Version der Anwendung herunter, und installieren Sie sie.

## <a name="use-azure-iot-explorer"></a>Verwenden des Azure-IoT-Explorers

Sie können entweder ein eigenes Gerät verbinden oder eins unserer simulierten Beispielgeräte verwenden. Befolgen Sie [diese Anweisungen](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples), um das Beispiel für ein simuliertes Gerät auszuführen.

### <a name="connect-to-your-hub"></a>Herstellen einer Verbindung mit Ihrem Hub

Wenn Sie den Azure-IoT-Explorer zum ersten Mal ausführen, werden Sie aufgefordert, die Verbindungszeichenfolge für den IoT-Hub einzugeben. Nachdem Sie die Verbindungszeichenfolge eingegeben haben, klicken Sie auf **Verbinden**. Sie können die Einstellungen des Tools verwenden, um durch Aktualisieren der Verbindungszeichenfolge zu einem anderen IoT-Hub zu wechseln.

Die Modelldefinition für ein IoT Plug & Play-Gerät wird im öffentlichen Repository, in einem Unternehmensrepository oder auf Ihrem verbundenen Gerät gespeichert. Standardmäßig sucht das Tool im öffentlichen Modellrepository und auf Ihrem verbundenen Gerät nach Ihrer Modelldefinition. In den **Einstellungen** können Sie Quellen hinzufügen oder entfernen oder die Priorität der Quellen konfigurieren:

So fügen Sie eine Quelle hinzu:

1. Wechseln Sie zu **Einstellungen**.
1. Klicken Sie auf **Neu**, und wählen Sie Ihre Quelle aus.
1. Wenn Sie das Modellrepository Ihres Unternehmens hinzufügen, geben Sie die Verbindungszeichenfolge an.

So entfernen Sie eine Quelle:

1. Wechseln Sie zu **Einstellungen**.
1. Suchen Sie die Quelle, die Sie entfernen möchten.
1. Klicken Sie auf **X**, um sie zu entfernen. Sie können das öffentliche Modellrepository nicht entfernen, weil die allgemeinen Schnittstellendefinitionen aus diesem Repository stammen.

Ändern der Prioritäten der Quellen:

Sie können eine Modelldefinitionsquelle per Drag & Drop an eine andere Position in der Rangliste ziehen. Wenn ein Konflikt auftritt, setzen Definitionsquellen mit höherem Rang Quellen mit niedrigerem Rang außer Kraft.

### <a name="view-devices"></a>Anzeigen von Geräten

Nachdem das Tool eine Verbindung mit Ihrem IoT-Hub hergestellt hat, zeigt es die Listenseite **Geräte** mit allen Geräteidentitäten an, die in Ihrer IoT Hub-Instanz registriert sind. Sie können einen beliebigen Eintrag in der Liste erweitern, um weitere Informationen anzuzeigen.

Auf der Listenseite **Geräte** können Sie folgende Aktionen ausführen:

- Wählen Sie **Hinzufügen** aus, um ein neues Gerät in Ihrem Hub zu registrieren. Geben Sie dann eine Geräte-ID ein. Verwenden Sie die Standardeinstellungen, um Authentifizierungsschlüssel automatisch zu generieren und die Verbindung mit Ihrem Hub zu aktivieren.
- Wählen Sie ein Gerät und dann **Löschen** aus, um eine Geräteidentität zu löschen. Überprüfen Sie die Gerätedetails, bevor Sie diese Aktion abschließen, um sicherzustellen, dass Sie die richtige Geräteidentität löschen.
- Führen Sie eine Abfrage nach `capabilityID` und `interfaceID` durch. Fügen Sie die `capabilityID` oder die `interfaceID` als Parameter zu den Abfragen Ihrer Geräte hinzu.

## <a name="interact-with-a-device"></a>Interagieren mit einem Gerät

Wählen Sie auf der Seite **Geräte** einen Wert in der Spalte **Geräte-ID** aus, um die Detailseite für das registrierte Gerät anzuzeigen. Für das Gerät gibt es zwei Abschnitte: **Gerät** und **Digitaler Zwilling**.

### <a name="device"></a>Gerät

Dieser Abschnitt enthält die Registerkarten **Geräteidentität**, **Gerätezwilling** und **Telemetrie**.

- Sie können die Informationen zur [Geräteidentität](../iot-hub/iot-hub-devguide-identity-registry.md) auf der Registerkarte **Geräteidentität** anzeigen und aktualisieren.
- Über die Registerkarte **Gerätezwilling** können Sie auf Informationen zum [Gerätezwilling](../iot-hub/iot-hub-devguide-device-twins.md) zugreifen.
- Wenn ein Gerät verbunden ist und aktiv Daten sendet, können Sie diese [Telemetriedaten](../iot-hub/iot-hub-devguide-messages-read-builtin.md) auf der Registerkarte **Telemetrie** anzeigen.

### <a name="digital-twin"></a>Digitaler Zwilling

Sie können das Tool verwenden, um eine Instanz eines digitalen Zwillings des Geräts anzuzeigen. Bei IoT Plug & Play-Geräten werden hier alle Schnittstellen angezeigt, die dem Gerätefunktionsmodell zugeordnet sind. Wählen Sie eine Schnittstelle aus, um die zugehörigen [primitiven Datentypen für IoT Plug & Play](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL) zu erweitern.

#### <a name="properties"></a>Properties

Auf der Seite **Eigenschaften** können Sie die schreibgeschützten Eigenschaften anzeigen, die in einer Schnittstelle definiert sind. Auf der Seite **Schreibbare Eigenschaften** können Sie die in einer Schnittstelle definierten Eigenschaften anzeigen, für die Schreibvorgänge durchgeführt werden können.

1. Wechseln Sie zur Seite **Schreibbare Eigenschaften**.
1. Klicken Sie auf die Eigenschaft, die Sie aktualisieren möchten.
1. Geben Sie den neuen Wert für die Eigenschaft ein.
1. Sehen Sie sich eine Vorschau der Nutzlast an, die an das Gerät gesendet wird.
1. Übermitteln Sie die Änderung.

Nachdem Sie eine Änderung übermittelt haben, können Sie den Aktualisierungsstatus nachverfolgen: **Wird synchronisiert**, **Erfolg** oder **Fehler**. Wenn die Synchronisierung abgeschlossen ist, wird der neue Wert der Eigenschaft in der Spalte **Gemeldete Eigenschaft** angezeigt. Falls Sie vor Beendigung der Synchronisierung zu anderen Seiten navigieren, werden Sie vom Tool benachrichtigt, wenn die Aktualisierung abgeschlossen ist. Sie können sich den Benachrichtigungsverlauf auch in der Mitteilungszentrale des Tools ansehen.

#### <a name="commands"></a>Befehle

Um einen Befehl an ein Gerät zu senden, wechseln Sie zur Seite **Befehle**:

1. Erweitern Sie in der Befehlsliste den Befehl, den Sie auslösen möchten.
1. Geben Sie alle erforderlichen Werte für den Befehl ein.
1. Sehen Sie sich eine Vorschau der Nutzlast an, die an das Gerät gesendet wird.
1. Übermitteln Sie den Befehl.

#### <a name="telemetry"></a>Telemetrie

Um die Telemetrie für die ausgewählte Schnittstelle anzuzeigen, wechseln Sie zur Seite **Telemetrie** für die Schnittstelle.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie den Azure-IoT-Explorer installieren und verwenden, um mit Ihren IoT Plug & Play-Geräten zu interagieren. Als Nächstes empfehlen wir, sich darüber zu informieren, wie Sie die [Azure CLI-Erweiterung installieren und verwenden](./howto-install-pnp-cli.md).
