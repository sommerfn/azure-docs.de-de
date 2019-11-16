---
title: Verbinden eines Rigado Cascade 500-Geräts in Azure IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Rigado Cascade 500-Gatewaygerät mit ihrer IoT Central-Anwendung verbinden.
services: iot-central
ms.service: iot-central
ms.topic: conceptual
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 10/19/2019
ms.openlocfilehash: 4559bb87369309882ebdaa0d3b408786feb586b5
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894538"
---
# <a name="connect-a-rigado-cascade-500-gateway-device-to-your-azure-iot-central-application"></a>Verbinden eines Rigado Cascade 500-Gatewaygeräts mit Ihrer Azure IoT Central-Anwendung


In diesem Artikel wird beschrieben, wie Sie als Lösungsersteller ein Rigado Cascade 500-Gatewaygerät mit Ihrer Microsoft Azure IoT Central-Anwendung verbinden können. 

## <a name="what-is-cascade-500"></a>Was ist Cascade 500?

Cascade 500 IoT-Gateway ist ein Hardwareangebot von Rigado, das in deren Lösung „Cascade Edge-as-a-Service“ enthalten ist. Es bietet kommerziellen IoT-Projekt- und -Produktteams flexible Edge-Computeleistung, eine stabile Anwendungsumgebung in Containern sowie eine Vielzahl von Konnektivitätsoptionen für drahtlose Geräte, darunter Bluetooth 5, LTE und WLAN.

Cascade 500 ist für Azure IoT Plug & Play (PnP) vorab zertifiziert und ermöglicht es unseren Lösungserstellern, das Gerät problemlos in ihre End-to-End-Lösungen zu integrieren. Mit dem Cascade-Gateway können Sie eine drahtlose Verbindung zu einer Vielzahl von Sensoren für die Zustandsüberwachung herstellen, die sich in der Nähe des Gatewaygeräts befinden. Diese Sensoren können über das Gatewaygerät in IoT Central integriert werden.

## <a name="prerequisites"></a>Voraussetzungen
Zum schrittweisen Durcharbeiten dieser Anleitung benötigen Sie die folgenden Ressourcen:

* Ein Rigado Cascade 500-Gerät. Weitere Informationen finden Sie unter [Rigado](https://www.rigado.com/).
* Eine Azure IoT Central-Anwendung, die mit einer der Vorschauanwendungsvorlagen erstellt wurde. Weitere Informationen finden Sie unter [Erstellen einer neuen Anwendung](./quick-deploy-iot-central.md).

## <a name="add-a-device-template"></a>Hinzufügen einer Gerätevorlage

Zum Integrieren eines Cascade 500-Gatewaygeräts in Ihre Azure IoT Central-Anwendungsinstanz müssen Sie eine entsprechende Gerätevorlage in Ihrer Anwendung konfigurieren.

So fügen Sie eine Cascade 500-Gerätevorlage hinzu: 

1. Navigieren Sie im linken Bereich zur Registerkarte ***Gerätevorlagen***, und wählen Sie aus **+ Neue**: ![Neue Gerätevorlage erstellen](./media/howto-connect-rigado-cascade500/device-template-new.png)
1. Auf der Seite gibt es eine Option zum ***Erstellen einer benutzerdefinierten Vorlage*** oder zum ***Verwenden einer vorkonfigurierten Gerätevorlage***.
1. Wählen Sie die Gerätevorlage „C500“ aus der Liste von vorkonfigurierten Gerätevorlagen aus, wie nachstehend gezeigt wird: ![Auswählen der Gerätevorlage „C500“](./media/howto-connect-rigado-cascade500/device-template-preconfigured.png)
1. Klicken Sie auf ***Weiter: Anpassen***, um mit dem nächsten Schritt fortzufahren. 
1. Wählen Sie im nächsten Bildschirm ***Erstellen*** aus, um die Gerätevorlage „C500“ in Ihre IoT Central-Anwendung zu integrieren.

## <a name="retrieve-application-connection-details"></a>Abrufen von Details zur Anwendungsverbindung

Jetzt müssen Sie die **Bereichs-ID** und den **Primärschlüssel** für Ihre Azure IoT Central-Anwendung abrufen, um eine Verbindung mit dem Cascade 500-Gerät herzustellen. 

1. Navigieren Sie im linken Bereich zu **Verwaltung**, und klicken Sie auf **Geräteverbindung**. 
2. Notieren Sie sich die **Bereichs-ID** für Ihre IoT Central-Anwendung.
![Anwendungs-Bereichs-ID](./media/howto-connect-rigado-cascade500/app-scope-id.png)
3. Klicken Sie jetzt auf **Schlüssel anzeigen**, und notieren Sie sich den **Primärschlüssel**
![Primärschlüssel](./media/howto-connect-rigado-cascade500/primary-key-sas.png).  

## <a name="contact-rigado-to-connect-the-gateway"></a>Kontaktieren von Rigado, um eine Verbindung mit dem Gateway herzustellen 

Zum Herstellen einer Verbindung zwischen dem Cascade 500-Gerät und Ihrer IoT Central-Anwendung müssen Sie Rigado kontaktieren und ihnen die Details zur Anwendungsverbindung aus den vorstehenden Schritten mitteilen. 

Sobald das Gerät mit dem Internet verbunden ist, kann Rigado über einen sicheren Kanal ein Konfigurationsupdate per Pushdown auf das Cascade 500-Gatewaygerät herunterladen. 

Nachdem dieses Update die IoT Central-Verbindungsdetails auf das Cascade 500-Gerät angewendet hat, wird es in Ihrer Geräteliste angezeigt. 

![Primärschlüssel](./media/howto-connect-rigado-cascade500/devices-list-c500.png)  

Jetzt können Sie Ihr C500-Gerät in Ihrer IoT Central-Anwendung einsetzen!

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie ein Rigado Cascade 500-Gerät mit Ihrer Azure IoT Central-Anwendung verbinden können, sollten Sie sich im nächsten Schritt informieren, wie [eine In-Store-Analyse-Anwendung erstellt wird](../retail/tutorial-in-store-analytics-create-app-pnp.md), um eine End-to-End-Lösung zu entwickeln. 