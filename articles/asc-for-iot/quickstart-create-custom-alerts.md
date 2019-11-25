---
title: 'Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen für Azure Security Center für IoT'
description: In diesem Schnellstart erfahren Sie, wie Sie benutzerdefinierte Benachrichtigungen für Azure Security Center für IoT erstellen und zuweisen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: eca5d69efb04cf8210b0b2aa502bcee5cd4f5264
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904135"
---
# <a name="quickstart-create-custom-alerts"></a>Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen


Mithilfe benutzerdefinierter Sicherheitsgruppen und Benachrichtigungen können Sie End-to-End-Sicherheitsinformationen und kategorische Geräteinformationen optimal nutzen, um die Sicherheit Ihrer gesamten IoT-Lösung zu verbessern. 

## <a name="why-use-custom-alerts"></a>Vorteile benutzerdefinierter Benachrichtigungen 

Sie kennen Ihre IoT-Geräte am besten.

Kunden, die umfassend mit dem erwarteten Verhalten ihrer Geräte vertraut sind, können mit Azure Security Center für IoT auf der Grundlage dieses Wissens eine Verhaltensrichtlinie für Geräte erstellen, sodass bei jeder Abweichung vom erwarteten Normalverhalten eine Benachrichtigung ausgelöst wird.

## <a name="security-groups"></a>Sicherheitsgruppen

Mithilfe von Sicherheitsgruppen können Sie logische Gruppen von Geräten definieren und deren Sicherheitsstatus zentral verwalten.

Diese Gruppen können Geräte mit bestimmter Hardware, bereitgestellte Geräte eines bestimmten Standorts oder eine beliebige andere Gruppe darstellen, die Ihren spezifischen Anforderungen entspricht.

Sicherheitsgruppen werden durch eine Tag-Eigenschaft namens **SecurityGroup** für Gerätezwillinge definiert. Standardmäßig besitzt jede IoT-Lösung in IoT Hub eine Sicherheitsgruppe namens **Standard**. Ändern Sie den Wert der Eigenschaft **SecurityGroup**, um die Sicherheitsgruppe eines Geräts zu ändern.
 
Beispiel:

```
{
  "deviceId": "VM-Contoso12",
  "etag": "AAAAAAAAAAM=",
  "deviceEtag": "ODA1BzA5QjM2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 4,
  "tags": {
    "SecurityGroup": "default"
  }, 
```

Verwenden Sie Sicherheitsgruppen, um Ihre Geräte in logischen Kategorien zu gruppieren. Die erstellten Gruppen können Sie dann den benutzerdefinierten Benachrichtigungen Ihrer Wahl zuweisen, um eine möglichst effektive End-to-End-IoT-Sicherheitslösung zu erhalten. 

## <a name="customize-an-alert"></a>Anpassen einer Benachrichtigung

1. Öffnen Sie Ihre IoT Hub-Instanz. 
2. Klicken Sie im Abschnitt **Sicherheit** auf **Benutzerdefinierte Benachrichtigungen**. 
3. Wählen Sie eine Sicherheitsgruppe aus, auf die Sie die Anpassung anwenden möchten. 
4. Klicken Sie auf **Add a custom alert** (Benutzerdefinierte Benachrichtigung hinzufügen).
5. Wählen Sie in der Dropdownliste eine benutzerdefinierte Benachrichtigung aus. 
6. Bearbeiten Sie die erforderlichen Eigenschaften, und klicken Sie anschließend auf **OK**.
7. Klicken Sie auf **SPEICHERN**. Wenn Sie die neue Benachrichtigung nicht speichern, wird sie beim nächsten Schließen von IoT Hub gelöscht.

 
## <a name="alerts-available-for-customization"></a>Anpassbare Benachrichtigungen

Die folgende Tabelle enthält eine Zusammenfassung der anpassbaren Benachrichtigungen:


| severity | NAME | Data source | BESCHREIBUNG | Vorschlag zur Problemlösung|
|---|---|---|---|---|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs.          | IoT Hub     | Die Anzahl von Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs.      | IoT Hub     | Die Menge der Gerät-zu-Cloud-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl direkter Methodenaufrufe liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der direkten Methodenaufrufen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Dateiuploads liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge von Dateiuploads innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.| |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) in einem Zeitfenster liegt nicht innerhalb des konfigurierten zulässigen Bereichs.                                  |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt nicht innerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub| Die Menge der Gerät-zu-Cloud-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|    |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs.          | IoT Hub     | Die Menge der Gerät-zu-Cloud-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Befehlswarteschlangenbereinigungen liegt außerhalb des zulässigen Bereichs.                               | IoT Hub     | Die Menge der Befehlswarteschlangenbereinigungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Modulzwillingsupdates liegt außerhalb des zulässigen Bereichs.                                       | IoT Hub     | Die Menge der Modulzwillingsupdates innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl nicht autorisierter Vorgänge liegt außerhalb des zulässigen Bereichs.  | IoT Hub     | Die Menge nicht autorisierter Vorgänge innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl aktiver Verbindungen liegt außerhalb des zulässigen Bereichs.  | Agent       | Die Anzahl aktiver Verbindungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|  Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen Verbindungen hinzu.  |
| Niedrig      | Benutzerdefinierte Warnung: Es wurde eine Verbindung mit einer unzulässigen IP-Adresse erstellt.                             | Agent       | Es wurde eine Verbindung mit einer IP-Adresse erstellt, die nicht in der Liste zulässiger IP-Adressen enthalten ist. |Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen IP-Adressen hinzu.                        |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl nicht erfolgreicher lokaler Anmeldungen liegt außerhalb des zulässigen Bereichs.                               | Agent       | Die Menge nicht erfolgreicher lokaler Anmeldungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Anmeldung eines Benutzers, der nicht in der Liste mit zugelassenen Benutzern enthalten ist | Agent       | Ein lokaler Benutzer, der nicht in Ihrer Liste mit zugelassenen Benutzern enthalten und beim Gerät angemeldet ist|  Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Zulassungs-/Blockierungsliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Ein unzulässiger Prozesses wurde ausgeführt. | Agent       | Auf dem Gerät wurde ein unzulässiger Prozess ausgeführt. |Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Zulassungs-/Blockierungsliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen.  |
|


## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie einen Sicherheits-Agent bereitstellen:

> [!div class="nextstepaction"]
> [Select and deploy a security agent on your IoT device](how-to-deploy-agent.md) (Auswählen und Bereitstellen eines Sicherheits-Agents auf Ihrem IoT-Gerät)
