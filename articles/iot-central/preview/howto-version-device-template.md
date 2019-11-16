---
title: Grundlegendes zur Versionsverwaltung von Gerätevorlagen für Ihre Azure IoT Central-Apps | Microsoft-Dokumentation
description: Durchlaufen Sie Ihre Gerätevorlagen, indem Sie neue Versionen erstellen, ohne Ihre gerade angeschlossenen Geräte zu beeinträchtigen.
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 666c3030d91fba3c3bdfe23f808c4e830dff6ec1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894402"
---
# <a name="create-a-new-device-template-version-preview-features"></a>Erstellen einer neuen Gerätevorlagenversion (Vorschaufeatures)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Azure IoT Central ermöglicht die schnelle Entwicklung von IoT-Anwendungen. Sie können Ihre Entwürfe für Gerätevorlagen schnell durchlaufen, indem Sie Gerätefunktionen, Ansichten und Anpassungen hinzufügen, bearbeiten oder löschen. Nachdem Sie Ihre Gerätevorlage veröffentlicht haben, wird das Gerätefunktionsmodell als **Veröffentlicht** angezeigt, und neben dem Modell befinden sich Sperrsymbole. Um Änderungen am Gerätefunktionsmodell vorzunehmen, müssen Sie eine neue Version der Gerätevorlage erstellen. Cloudeigenschaften, Anpassungen und Ansichten dagegen können jederzeit bearbeitet werden, ohne dass eine neue Version der Gerätevorlage erstellt werden muss. Sobald Sie Ihre Änderungen gespeichert haben, können Sie die Gerätevorlage veröffentlichen, um dem Operator die neuesten Änderungen zur Ansicht im Geräte-Explorer zur Verfügung zu stellen.

> [!NOTE]
> Weitere Informationen zum Erstellen einer Gerätevorlage finden Sie unter [Einrichten und Verwalten einer Gerätevorlage](howto-set-up-template.md).

## <a name="add-customizations-to-the-device-template-without-versioning"></a>Hinzufügen von Anpassungen zur Gerätevorlage ohne Versionsverwaltung

Bestimmte Elemente Ihrer Gerätefunktionen können bearbeitet werden, ohne dass Sie eine neue Version Ihrer Gerätevorlage und Schnittstellen erstellen müssen. Hierzu gehören beispielsweise folgende Felder: Anzeigename, Semantiktyp, Mindest- und Höchstwerte, Dezimalstellen, Farbe, Einheit, Anzeigeeinheit, Kommentar und Beschreibung. So fügen Sie diese Anpassungen hinzu:

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie das Gerät aus, das Sie anpassen möchten.
1. Wählen Sie die Registerkarte **Anpassen** aus.
1. Hier werden alle Funktionen aufgeführt, die in Ihrem Gerätefunktionsmodell definiert sind. Alle Felder, die Sie hier bearbeiten können, können gespeichert und für Ihre gesamte Anwendung verwendet werden, ohne dass Sie eine neue Version Ihrer Gerätevorlage erstellen müssen. Wenn Sie schreibgeschützte Felder bearbeiten möchten, müssen Sie eine neue Version der Gerätevorlage erstellen, um diese Felder ändern zu können. Wählen Sie ein Feld aus, das Sie bearbeiten möchten, und geben Sie neue Werte ein.
1. Klicken Sie auf **Speichern**. Jetzt überschreiben diese Werte alle Werte, die ursprünglich in Ihrer Gerätevorlage gespeichert waren, und werden in der gesamten Anwendung verwendet.

## <a name="versioning-a-device-template"></a>Versionsverwaltung für eine Gerätevorlage

Wenn Sie eine neue Version Ihrer Gerätevorlage erstellen, wird eine Entwurfsversion der Vorlage erstellt, in der das Gerätefunktionsmodell bearbeitet werden kann. Alle veröffentlichten Schnittstellen bleiben veröffentlicht, bis für sie eine individuelle Version erstellt wurde. Um eine veröffentlichte Schnittstelle zu ändern, müssen Sie zuerst eine neue Version der Gerätevorlage erstellen.

Für die Gerätevorlage sollte nur dann eine neue Version erstellt werden, wenn Sie einen Teil des Gerätefunktionsmodells bearbeiten möchten, das Sie im Abschnitt für Anpassungen in der Gerätevorlage nicht bearbeiten können. 

So erstellen Sie eine neue Version einer Gerätevorlage:

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie die Gerätevorlage aus, für die Sie eine neue Version erstellen möchten.
1. Klicken Sie oben auf der Seite auf die Schaltfläche **Version**, und geben Sie der Vorlage einen neuen Namen. Es wird ein neuer Name vorgeschlagen, den Sie ändern können.
1. Klicken Sie auf **Create**.
1. Jetzt befindet sich Ihre Gerätevorlage im Entwurfsmodus. Sie werden feststellen, dass die Schnittstellen noch gesperrt sind. Zur Bearbeitung müssen Sie für jede einzelne Schnittstelle eine neue Version erstellen. 

### <a name="versioning-an-interface"></a>Versionsverwaltung für eine Schnittstelle

Indem Sie eine neue Version für eine Schnittstelle erstellen, können Sie Funktionen in dieser bereits erstellten Schnittstelle hinzufügen, aktualisieren und entfernen. 

So erstellen Sie eine neue Version einer Schnittstelle:

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie Ihre Gerätevorlage aus, die sich im Entwurfsmodus befindet.
1. Wählen Sie die zu bearbeitende Schnittstelle aus, die sich im veröffentlichten Modus befindet.
1. Klicken Sie oben auf der Seite der Schnittstelle auf die Schaltfläche **Version**. 
1. Klicken Sie auf **Create**.
1. Jetzt befindet sich Ihre Schnittstelle im Entwurfsmodus. Sie können in dieser Schnittstelle Funktionen hinzufügen oder bearbeiten, ohne vorhandene Anpassungen und Ansichten zu beeinträchtigen. 

> [!NOTE]
> Für von Azure IoT veröffentlichte Standardschnittstellen können keine Versionen erstellt werden, und eine Bearbeitung dieser Schnittstellen ist nicht möglich. Diese Standardschnittstellen werden für die Gerätezertifizierung verwendet.

> [!NOTE]
> Sobald eine Schnittstelle veröffentlicht wurde, können Sie die Funktionen dieser Schnittstelle auch im Entwurfsmodus nicht löschen. Funktionen können im Entwurfsmodus nur bearbeitet oder hinzugefügt werden.


## <a name="migrate-a-device-across-device-template-versions"></a>Migrieren eines Geräts zwischen Gerätevorlageversionen

Es lassen sich mehrere Versionen der Gerätevorlage erstellen. Im Laufe der Zeit werden Sie über mehrere angeschlossene Geräte verfügen, die diese Gerätevorlagen verwenden. Sie können Geräte von einer Version Ihrer Gerätevorlage zu einer anderen migrieren. Die folgenden Schritte beschreiben, wie Sie ein Gerät migrieren:

1. Wechseln Sie zur **Device Explorer**-Seite.
1. Wählen Sie das Gerät aus, das Sie zu einer anderen Version migrieren möchten.
1. Wählen Sie **Migrieren** aus.
1. Wählen Sie die Gerätevorlage mit der Versionsnummer aus, zu der Sie das Gerät migrieren möchten, und klicken Sie auf **Migrieren**.

![Migrieren eines Geräts](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Gerätevorlageversionen in Ihrer Azure IoT Central-Anwendung verwendet werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](tutorial-create-telemetry-rules.md)
