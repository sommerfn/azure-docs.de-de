---
title: Tutorial zur IoT-vernetzten Logistik | Microsoft-Dokumentation
description: Ein Tutorial zur Anwendungsvorlage für vernetzte Logistik für Azure IoT Central
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: 9d55eca3df8897650f78a6bd85b649349c806e95
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615270"
---
# <a name="tutorial-deploy-and-walk-through-a-connected-logistics-application-template"></a>Tutorial: Bereitstellung und exemplarische Vorgehensweise für eine Anwendungsvorlage für vernetzte Logistik

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Dieses Tutorial zeigt, wie Sie durch Bereitstellen einer IoT Central-Vorlage Ihren Einstieg in **vernetzte Logistik** machen. Sie erfahren, wie Sie die Vorlage bereitstellen, was im Lieferumfang enthalten ist und welche nächsten Schritte sinnvoll sind.

In diesem Tutorial lernen Sie Folgendes: 
* Erstellen einer Anwendung für vernetzte Logistik 
* Einführung in die Anwendung 

## <a name="prerequisites"></a>Voraussetzungen
* Zum Bereitstellen dieser App sind keine speziellen Voraussetzungen erforderlich
* Ein Azure-Abonnement ist empfehlenswert, Sie können es aber auch ohne versuchen

## <a name="create-connected-logistics-application-template"></a>Erstellen einer Anwendungsvorlage für vernetzte Logistik
Mithilfe der folgenden Schritte können Sie eine Anwendung erstellen.
1. Navigate Sie zur Anwendungs-Manager-Website von Azure IoT Central. Wählen Sie in der linken Navigationsleiste **Erstellen** aus, und klicken Sie dann auf die Registerkarte **Retail**.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/iotc-retail-homepage.png)

2. Wählen Sie unter **Connected Logistics-Anwendung** die Option **App erstellen** aus.

3. **App erstellen** öffnet ein neues Anwendungsformular und setzt die erforderlichen Details ein, wie unten dargestellt.
   * **Anwendungsname**: Sie können den vorgeschlagenen Standardnamen verwenden oder einen eigenen Anwendungsnamen eingeben.
   * **URL**: Sie können die vorgeschlagene Standard-URL verwenden oder eine eigene, leicht merkbare und benutzerfreundliche URL eingeben. Im nächsten Schritt empfiehlt sich die Standardeinstellung, wenn Sie bereits über ein Azure-Abonnement verfügen. Andernfalls können Sie mit einer siebentägigen kostenlosen Testversion beginnen und jederzeit vor dem Ablauf der kostenlosen Testversion auf nutzungsbasierte Bezahlung umstellen.
   * **Abrechnungsinfo**: Die Detailangaben „Verzeichnis“, „Azure-Abonnement“ und „Region“ sind für die Bereitstellung der Ressourcen erforderlich.
   * **Erstellen** : Wählen Sie unten auf der Seite „Erstellen“ aus, um Ihre Anwendung bereitzustellen.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-app-create.png)

## <a name="walk-through-the-application"></a>Einführung in die Anwendung 

## <a name="dashboard"></a>Dashboard

Nach erfolgreicher Bereitstellung der App ist Ihr Standard-Dashboard ein Operatorportal mit dem Schwerpunkt vernetzte Logistik. Northwind Trader ist ein fiktiver Logistikanbieter, der eine Frachtflotte zur See und an Land verwaltet. Auf diesem Dashboard sehen Sie zwei verschiedene Gateways, die Telemetrie zu Sendungen zusammen mit zugeordneten Befehlen, Aufträgen und Aktionen bereitstellen, die Sie ausführen können. Dieses Dashboard ist vorkonfiguriert, um die Aktivität von kritischen Vorgängen bei Logistikgeräten darzustellen.
Das Dashboard ist logisch in zwei verschiedene Gateway-Geräteverwaltungsvorgänge geteilt. 
   * Logistikrouten für Transport per LKW und Standortdetails für den Schiffstransport sind ein wesentliches Element für jede Art von multimodalem Transport
   * Gatewaystatus und relevante Informationen anzeigen 

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard1.png)

   * Sie können die Gesamtzahl der Gateways, aktive und unbekannte Tags leicht nachverfolgen.
   * Sie können Vorgänge der Geräteverwaltung durchführen, beispielsweise Firmwareupdates, das Deaktivieren und Aktivieren von Sensoren, das Aktualisieren von Sensorschwellenwerten, Telemetrieintervallen und Geräteserviceverträgen.
   * Anzeigen des Geräteakkuverbrauchs

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard2.png)

## <a name="device-template"></a>Gerätevorlage

Klicken Sie auf die Registerkarte „Gerätevorlagen“, dann wird das Gatewayfunktionsmodell angezeigt. Ein Funktionsmodell ist um zwei verschiedene Oberflächen herum strukturiert, **Gatewaytelemetrie und -eigenschaft** und **Gatewaybefehle**

**Gatewaytelemetrie und -eigenschaft**: Diese Oberfläche stellt die gesamte Telemetrie im Zusammenhang mit Sensor-, Standort- und Geräteinformationen sowie Gerätezwillings-Eigenschaftsfunktionen, wie Sensorschwellenwerte und Aktualisierungsintervalle, dar.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate1.png)

**Gatewaybefehle**: Diese Schnittstelle organisiert alle Gatewaybefehlsfunktionen.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate2.png)

## <a name="rules"></a>Regeln
Wählen Sie die Registerkarte „Regeln“ aus, um zwei verschiedene Regeln anzuzeigen, die in dieser Anwendungsvorlage vorhanden sind. Diese Regeln sind so konfiguriert, dass sie Benachrichtigungen an die Operatoren zwecks weiterer Untersuchungen senden.
 
**Gateway-Diebstahlwarnung**: Diese Regel wird ausgelöst, wenn von den Sensoren während des Transports unerwarteter Lichteinfall erkannt wird. Operatoren müssen schnellstmöglich benachrichtigt werden, um einen möglichen Diebstahl zu untersuchen.
 
**Nicht reagierendes Gateway**: Diese Regel wird ausgelöst, wenn das Gateway für einen längeren Zeitraum keine Meldung an die Cloud macht. Das Gateway reagiert möglicherweise aufgrund von niedrigem Akkuladestand, Verbindungsverlust oder Gerätezustand nicht mehr.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-rules.png)

## <a name="jobs"></a>Aufträge
Wählen Sie die Registerkarte „Aufträge“ aus, um die fünf verschiedenen Aufträge in dieser Anwendungsvorlage anzuzeigen:

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-jobs.png)

Sie können die Funktion „Aufträge“ nutzen, um projektmappenweite Vorgänge auszuführen. Hier verwenden die Aufträge die Gerätebefehle und die Zwillingsfunktion, um Aufgaben wie das Deaktivieren bestimmter Sensoren für das gesamte Gateway oder das Ändern von Sensorschwellenwerten abhängig von der Transportart und -route durchzuführen. 
   * Es ist ein Standardvorgang, Aufprallsensoren beim Transport auf See zu deaktivieren, um Akkukapazität zu sparen, oder die Temperaturschwelle bei Kühlkettentransporten abzusenken. 
 
   * Mithilfe von Aufträgen können Sie systemweite Vorgänge wie das Updaten der Gatewayfirmware oder das Aktualisieren des Dienstvertrags durchführen, um bei Wartungsaktivitäten auf dem aktuellen Stand zu bleiben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Anwendung nicht weiter verwenden möchten, löschen Sie die Anwendungsvorlage, indem Sie **Verwaltung** > **Anwendungseinstellungen** aufsuchen und auf **Löschen** klicken.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/tutorial-iot-central-connected-logistics/connected-logistics-cleanup.png)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Konzept der vernetzten Logistik](./architecture-connected-logistics-pnp.md)
* Weitere Informationen zu anderen [IoT Central-Einzelhandelsvorlagen](./overview-iot-central-retail-pnp.md)
* Erfahren Sie mehr in der [IoT Central-Übersicht](../core/overview-iot-central-pnp.md)
