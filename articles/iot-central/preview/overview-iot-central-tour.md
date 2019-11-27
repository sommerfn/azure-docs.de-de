---
title: Kennenlernen der Benutzeroberfläche von Azure IoT Central | Microsoft-Dokumentation
description: Machen Sie sich mit den wichtigsten Bereichen der Benutzeroberfläche von Azure IoT Central vertraut, die Sie zum Erstellen, Verwalten und Nutzen Ihrer IoT-Lösung verwenden.
author: lmasieri
ms.author: lmasieri
ms.date: 10/21/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: f8aa467f95bb97c42e726d1392deca53e15df624
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896488"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-preview-features"></a>Kennenlernen der Benutzeroberfläche von Azure IoT Central (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird die Benutzeroberfläche von Microsoft Azure IoT Central vorgestellt. Über die Benutzeroberfläche können Sie Azure IoT Central-Lösungen und die damit verbundenen Geräte erstellen, verwalten und verwenden.

Als _Lösungsentwickler_ verwenden Sie die Benutzeroberfläche von Azure IoT Central, um Ihre Azure IoT Central-Lösung zu definieren. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Definieren der Art von Gerät, das eine Verbindung mit Ihrer Lösung herstellt
* Konfigurieren der Regeln und Aktionen für Ihre Geräte 
* Anpassen der Benutzeroberfläche für einen _Bediener_, der Ihre Lösung verwendet

_Bediener_ verwenden die Benutzeroberfläche von Azure IoT Central, um ihre Azure IoT Central-Lösung zu verwalten. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Überwachen Ihrer Geräte
* Konfigurieren Ihrer Geräte
* Behandeln und Beheben von Problemen mit Ihren Geräten
* Bereitstellen von neuen Geräten

## <a name="iot-central-homepage"></a>IoT Central-Startseite

Auf der [IoT Central-Startseite](https://aka.ms/iotcentral-get-started) finden Sie weitere Informationen zu den aktuellen News und Features von IoT Central, zum Erstellen neuer Anwendungen und zum Anzeigen und Starten Ihrer vorhandenen Anwendung.

> [!div class="mx-imgBorder"]
> ![IoT Central-Startseite](media/overview-iot-central-tour/iot-central-homepage-pnp.png)

### <a name="create-an-application"></a>Erstellen einer Anwendung

Im Abschnitt für die Erstellung können Sie die Liste mit den branchenrelevanten IoT Central-Vorlagen durchsuchen, um schnell einzusteigen. Alternativ können Sie auch von Grund auf neu beginnen, indem Sie eine Vorlage für eine benutzerdefinierte App verwenden.  
> [!div class="mx-imgBorder"]
> ![IoT Central-Seite für die Erstellung](media/overview-iot-central-tour/iot-central-build-pnp.png)

Weitere Informationen finden Sie in der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

### <a name="launch-your-application"></a>Starten Ihrer Anwendung

Sie können Ihre IoT Central-Anwendung starten, indem Sie auf die URL zugreifen, die von Ihnen bzw. Ihrem Lösungsentwickler bei der App-Erstellung ausgewählt wurde. Darüber hinaus können Sie im [App-Manager von IoT Central](https://aka.ms/iotcentral-apps) eine Liste mit allen Anwendungen anzeigen, auf die Sie Zugriff haben.

> [!div class="mx-imgBorder"]
> ![App-Manager von IoT Central](media/overview-iot-central-tour/app-manager-pnp.png)

## <a name="navigate-your-application"></a>Navigieren in der Anwendung

In der IoT-Anwendung können Sie die Navigationselemente auf der linken Seite nutzen, um auf die verschiedenen Bereiche zuzugreifen. Sie können die Navigationsleiste erweitern oder reduzieren, indem Sie oben in der Leiste das Symbol mit den drei Linien auswählen:

> [!NOTE]
> Welche Elemente in der Navigationsleiste angezeigt werden, hängt von Ihrer Benutzerrolle ab. Lesen Sie die weiteren Informationen zum [Verwalten von Benutzern und Rollen](howto-manage-users-roles.md). 

:::row:::
  :::column span="":::
      > [!div class="mx-imgBorder"]
      > ![Navigationsbereich auf der linken Seite](media/overview-iot-central-tour/navigationbar-pnp.png)
  :::column-end:::
  :::column span="2":::
     Unter **Dashboard** wird Ihr Anwendungsdashboard angezeigt. Als *Lösungsentwickler* können Sie das globale Dashboard für Ihre Bediener anpassen. Je nach Benutzerrolle können Bediener auch eigene persönliche Dashboards erstellen.
     
     Unter **Geräte** können Sie Ihre verbundenen Geräte verwalten – real oder simuliert.

     Unter **Gerätegruppen** können Sie logische Sammlungen mit Geräten anzeigen und erstellen, die per Abfrage angegeben werden. Sie können diese Abfrage speichern und Gerätegruppen über die Anwendung nutzen, um Massenvorgänge durchzuführen.

     Unter **Regeln** können Sie Regeln zur Überwachung Ihrer Geräte erstellen und bearbeiten. Regeln werden anhand der Gerätetelemetrie ausgewertet und dienen zum Auslösen von anpassbaren Aktionen.

     Unter **Analyse** können Sie benutzerdefinierte Ansichten auf der Grundlage von Gerätedaten erstellen, um basierend auf Ihrer Anwendung Erkenntnisse zu gewinnen.

     Unter **Aufträge** können Sie Ihre Geräte bedarfsgesteuert verwalten, indem Sie Massenvorgänge durchführen.

     Die Option **Gerätevorlagen** ermöglicht die Erstellung und Verwaltung der Merkmale von Geräten, für die eine Verbindung mit Ihrer Anwendung hergestellt wird.

     Unter **Datenexport** können Sie den fortlaufenden Export in externe Dienste konfigurieren, z. B. Speicher und Warteschlangen.

     Die Option **Verwaltung** ermöglicht Ihnen das Verwalten der Bereiche Einstellungen, Anpassung, Abrechnung, Benutzer und Rollen Ihrer Anwendung.

     Mit **IoT Central** können *Administratoren* zurück zum App-Manager von IoT Central wechseln.
     
   :::column-end:::
:::row-end:::

### <a name="search-help-theme-and-support"></a>Suche, Hilfe, Design und Support

Das obere Menü wird auf jeder Seite angezeigt:

> [!div class="mx-imgBorder"]
> ![Symbolleiste](media/overview-iot-central-tour/toolbar-pnp.png)

* Wenn Sie nach Gerätevorlagen und Geräten suchen möchten, geben Sie einen Wert für die **Suche** ein.
* Wählen Sie zum Ändern der Sprache der Benutzeroberfläche oder des Designs das Symbol **Einstellungen** aus. Informieren Sie sich über das [Verwalten Ihrer Anwendungseinstellungen](../core/howto-manage-preferences.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).
* Wählen Sie das Symbol **Konto** aus, um sich von der Anwendung abzumelden.
* Wenn Sie Hilfe oder Unterstützung benötigen, klicken Sie auf das Dropdownmenü **Hilfe**, um eine Liste mit Ressourcen anzuzeigen. In einer Testanwendung beinhalten die Supportressourcen Zugriff auf den [Livechat](../core/howto-show-hide-chat.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).

Sie können zwischen einem hellen und einem dunklen Benutzeroberflächendesign wählen:

> [!NOTE]
> Die Option zum Wählen zwischen hellen und dunklen Designs ist nicht verfügbar, wenn Ihr Administrator ein benutzerdefiniertes Design für die Anwendung konfiguriert hat.

> [!div class="mx-imgBorder"]
> ![Auswählen eines Designs für die Benutzeroberfläche](media/overview-iot-central-tour/themes-pnp.png)

### <a name="dashboard"></a>Dashboard
> [!div class="mx-imgBorder"]
> ![Dashboard](media/overview-iot-central-tour/dashboard-pnp.png)

* Nach der Anmeldung bei Ihrer Azure IoT Central-Anwendung wird als Erstes das Dashboard angezeigt. Als *Lösungsentwickler* können Sie mehrere globale Anwendungsdashboards für andere Benutzer erstellen und anpassen. Informieren Sie sich über das [Hinzufügen von Kacheln zu Ihrem Dashboard](../core/howto-add-tiles-to-your-dashboard.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).

* Sofern Ihre Benutzerrolle als *Bediener* es zulässt, können Sie persönliche Dashboards erstellen, um die für Sie wichtigen Aspekte zu überwachen. Weitere Informationen finden Sie in der Anleitung zum [Erstellen und Verwalten mehrerer Dashboards](../core/howto-create-personal-dashboards.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).

### <a name="devices"></a>Geräte

> [!div class="mx-imgBorder"]
> ![Seite „Geräte“](media/overview-iot-central-tour/devices-pnp.png)

Auf der Explorer-Seite werden die _Geräte_ in Ihrer Azure IoT Central-Anwendung angezeigt (gruppiert nach _Gerätevorlage_). 

* Eine Gerätevorlage definiert einen Gerätetyp, der eine Verbindung mit Ihrer Anwendung herstellen kann.
* Bei einem Gerät handelt es sich um ein echtes oder simuliertes Gerät in Ihrer Anwendung.

Weitere Informationen finden Sie in der Schnellstartanleitung [Überwachen Ihrer Geräte](./quick-monitor-devices.md). 

### <a name="device-groups"></a>Gerätegruppen

> [!div class="mx-imgBorder"]
> ![Seite „Gerätegruppen“](media/overview-iot-central-tour/device-groups-pnp.png)

Bei einer Gerätegruppe handelt es sich um eine Sammlung mit verwandten Geräten. Ein *Lösungsentwickler* definiert eine Abfrage, um die in einer Gerätegruppe enthaltenen Geräte zu identifizieren. Sie verwenden Gerätegruppen, um in Ihrer Anwendung Massenvorgänge durchzuführen. Weitere Informationen finden Sie im Artikel [Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung](tutorial-use-device-groups.md).

### <a name="rules"></a>Regeln
> [!div class="mx-imgBorder"]
> ![Seite „Regeln“](media/overview-iot-central-tour/rules-pnp.png)

Auf der Seite „Regeln“ können Sie Regeln basierend auf den Telemetriedaten, dem Status oder den Ereignissen von Geräten definieren. Bei Auslösung einer Regel können mehrere Aktionen ausgelöst werden, z. B. das Senden einer E-Mail, das Benachrichtigen eines externen Systems über Webhookwarnungen usw. Weitere Informationen hierzu finden Sie im Tutorial zum [Konfigurieren von Regeln](tutorial-create-telemetry-rules.md). 

### <a name="analytics"></a>Analytics

> [!div class="mx-imgBorder"]
> ![Seite „Analyse“](media/overview-iot-central-tour/analytics-pnp.png)

Mit der Analyse können Sie benutzerdefinierte Ansichten auf der Grundlage von Gerätedaten erstellen, um mit Ihrer Anwendung Erkenntnisse zu gewinnen. Weitere Informationen finden Sie im Artikel zum Thema [Erstellen einer Analyse für Ihre Azure IoT Central-Anwendung](howto-create-analytics.md).

### <a name="jobs"></a>Aufträge

> [!div class="mx-imgBorder"]
> ![Seite „Aufträge“](media/overview-iot-central-tour/jobs-pnp.png)

Auf der Seite „Aufträge“ können Sie Massenvorgänge für die Geräteverwaltung auf Ihren Geräten ausführen. Sie können Geräteeigenschaften und Einstellungen aktualisieren und Befehle für Gerätegruppen ausführen. Weitere Informationen finden Sie im Artikel [Ausführen eines Auftrags](../core/howto-run-a-job.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).

### <a name="device-templates"></a>Gerätevorlagen

> [!div class="mx-imgBorder"]
> ![Seite „Gerätevorlagen“](media/overview-iot-central-tour/templates-pnp.png)

Auf der Seite „Gerätevorlagen“ können Ersteller die Gerätevorlagen in der Anwendung erstellen und verwalten. In einer Gerätevorlage werden Gerätemerkmale angegeben, z. B.:

* Telemetriedaten, Status und Ereignismessungen
* Properties
* Befehle
* Ansichten

Der *Lösungsentwickler* kann auch Formulare und Dashboards für Bediener erstellen, die zum Verwalten von Geräten verwendet werden.

Weitere Informationen finden Sie im Tutorial [Define a new device type in your Azure IoT Central application](howto-set-up-template.md) (Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung). 

### <a name="data-export"></a>Datenexport
> [!div class="mx-imgBorder"]
> ![Seite „Datenexport“](media/overview-iot-central-tour/export-pnp.png)

Per Datenexport können Sie Datenströme, z. B. für Telemetriedaten, von der Anwendung zu externen Systemen einrichten. Weitere Informationen finden Sie im Artikel [Exportieren von Daten in Azure IoT Central](./howto-export-data.md).

### <a name="administration"></a>Verwaltung
> [!div class="mx-imgBorder"]
> ![Seite „Verwaltung“](media/overview-iot-central-tour/administration-pnp.png)

Auf der Seite „Verwaltung“ können Sie Ihre IoT Central-Anwendung konfigurieren und anpassen. Sie können den Anwendungsnamen, die URL und das Design ändern, Benutzer und Rollen verwalten, API-Token erstellen und Ihre Anwendung exportieren. Weitere Informationen finden Sie im Artikel [How to administer your application](howto-administer.md) (Verwalten Ihrer Anwendung).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über Azure IoT Central verschafft und sich mit dem Layout der Benutzeroberfläche vertraut gemacht haben, empfehlen wir, mit der Schnellstartanleitung zum [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md) fortzufahren.
