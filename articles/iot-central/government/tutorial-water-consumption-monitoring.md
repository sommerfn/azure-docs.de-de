---
title: 'Tutorial: Erstellen einer App zur Überwachung des Wasserverbrauchs mit Azure IoT Central'
description: 'Tutorial: Es wird beschrieben, wie Sie mit Azure IoT Central-Anwendungsvorlagen eine Anwendung zur Überwachung des Wasserverbrauchs erstellen.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 0b100e0bebc7c5b26449e396650da5434334075d
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112621"
---
# <a name="tutorial-create-a-water-consumption-monitoring-application-in-iot-central"></a>Tutorial: Erstellen einer Anwendung zur Überwachung des Wasserverbrauchs in IoT Central

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial wird Schritt für Schritt die Erstellung einer Anwendung zur Überwachung des Wasserverbrauchs mit Azure IoT Central beschrieben, indem die entsprechende IoT Central-Anwendungsvorlage verwendet wird. 

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Verwenden der Azure IoT Central-Vorlage **Überwachung des Wasserverbrauchs** zum Erstellen einer entsprechenden Anwendung
> * Erkunden und Anpassen des Bedienerdashboards 
> * Erkunden der Gerätevorlage
> * Erkunden von simulierten Geräten
> * Erkunden und Konfigurieren von Regeln
> * Konfigurieren von Aufträgen
> * Anpassen des Brandings Ihrer Anwendung per Whitelabeling

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:
-  Es wird ein Azure-Abonnement empfohlen. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eines erstellen.

## <a name="create-water-consumption-monitoring-app-in-iot-central"></a>Erstellen der App für die Überwachung des Wasserverbrauchs in IoT Central

In diesem Abschnitt verwenden wir die Azure IoT Central-Vorlage **Überwachung des Wasserverbrauchs** zum Erstellen einer entsprechenden Anwendung in IoT Central.

Erstellen Sie wie folgt eine neue Azure IoT Central-Anwendung zur Überwachung des Wasserverbrauchs:  

1. Navigieren Sie zur [Startseite von Azure IoT Central](https://aka.ms/iotcentral).

    Wenn Sie ein Azure-Abonnement haben, melden Sie sich mit den üblichen Anmeldeinformationen an. Verwenden Sie andernfalls ein Microsoft-Konto:

    ![Eingeben Ihres Organisationskontos](media/tutorial-waterconsumptionmonitoring/sign-in.png)

2. Klicken Sie im linken Bereich auf **Build**, und wählen Sie die Registerkarte **Regierung** aus. Auf der Seite „Regierung“ werden mehrere Anwendungsvorlagen für den Behördenbereich angezeigt.

   ![Erstellen von App-Vorlagen für Behörden](./media/tutorial-waterconsumptionmonitoring/iotcentral-government-tab-overview1.png)

1. Wählen Sie die Anwendungsvorlage **Überwachung des Wasserverbrauchs** aus. Diese Vorlage enthält ein Beispiel für eine Gerätevorlage zur Überwachung des Wasserverbrauchs, ein simuliertes Gerät, ein Bedienerdashboard und vorkonfigurierte Überwachungsregeln.    

2. Klicken Sie auf **App erstellen**. Das Formular **Neue Anwendung** für die Erstellung mit den folgenden Feldern wird geöffnet:
    * **Anwendungsname**: Standardmäßig wird für die Anwendung als Name *Überwachung des Wasserverbrauchs* verwendet, gefolgt von einer eindeutigen ID-Zeichenfolge, die von IoT Central generiert wird. Wählen Sie optional einen benutzerfreundlichen Anzeigenamen aus. Sie können den Anwendungsnamen bei Bedarf später noch ändern.
    * **URL**: IoT Central generiert basierend auf dem Anwendungsnamen automatisch eine URL für Sie. Sie können die URL wie gewünscht aktualisieren. Es ist auch möglich, die URL zu einem späteren Zeitpunkt zu ändern. 
    * Wenn Sie ein Azure-Abonnement haben, geben Sie die entsprechenden Angaben für *Verzeichnis, Azure-Abonnement und Region* ein. Wenn Sie kein Abonnement haben, können Sie eine **7-tägige kostenlose Testversion** aktivieren und die erforderlichen Kontaktinformationen eingeben.  

    Weitere Informationen zu Verzeichnissen und Abonnements finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](../preview/quick-deploy-iot-central.md).

5. Klicken Sie am unteren Rand der Seite auf die Schaltfläche **Erstellen**. 

    ![Azure IoT Central-Seite „Anwendung erstellen“](./media/tutorial-waterconsumptionmonitoring/new-application-waterconsumptionmonitoring.png)

6. Sie haben nun mit der Azure IoT Central-Vorlage **Überwachung des Wasserverbrauchs** eine entsprechende App erstellt.

Glückwunsch! Sie haben die Erstellung der Anwendung zur Überwachung der Wasserqualität nun abgeschlossen. Für die Anwendung ist Folgendes vorab konfiguriert:
* Beispieldashboards für Bediener
* Beispiel für eine vordefinierte Wasserversorgung und Vorlagen für Ventile
* Simulation von Wasserversorgung und intelligenten Ventilen
* Vorkonfigurierte Regeln und Aufträge
* Beispiel für Branding mit Whitelabeling 

Es ist Ihre Anwendung, und Sie können sie jederzeit ändern. Wir erkunden die Anwendung jetzt und nehmen einige Anpassungen vor.  

## <a name="explore-and-customize-operator-dashboard"></a>Erkunden und Anpassen des Bedienerdashboards 
Nachdem Sie die Anwendung erstellt haben, gelangen Sie zum Beispiel-Bedienerdashboard: **Wide World Water-Dashboard zur Überwachung des Wasserverbrauchs**.

   ![Dashboard zur Überwachung des Wasserverbrauchs](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboardfull.png)

Als Lösungsentwickler können Sie Ansichten im Dashboard für Bediener erstellen und anpassen. Bevor wir auf die Anpassungen eingehen, erkunden wir zunächst das Dashboard. 

> [!NOTE]
> Alle im Dashboard angezeigten Daten basieren auf simulierten Gerätedaten, die im nächsten Abschnitt beschrieben werden. 
  
Das Dashboard besteht aus unterschiedlichen Arten von Kacheln:

* **Wide World Water Utility-Bildkachel**: Die erste Kachel im Dashboard ist eine Bildkachel des fiktiven Wasserversorgungsunternehmens „Wide World Water“. Sie können die Kachel anpassen und Ihr eigenes Bild einfügen oder das Bild entfernen. 

* **Kachel mit KPI zur durchschnittlichen Wassermenge**: Die KPI-Kachel ist so konfiguriert, dass als Beispiel der *Mittelwert der letzten 30 Minuten* angezeigt wird. Sie können KPI-Kacheln anpassen und dafür einen anderen Typ und Zeitbereich festlegen.

* Darüber hinaus befinden sich direkt im Dashboard Kacheln vom Typ *Device Command* (Gerätebefehl): **Close valve** (Ventil schließen), **Open valve** (Ventil öffnen) und **Set valve position** (Ventilposition festlegen). Wenn Sie auf die Befehle klicken, gelangen Sie zur Seite mit den Befehlen für das simulierte Gerät. In IoT Central ist ein *Befehl* eine *Gerätefunktion*. Dies wird später im **Abschnitt zu Gerätevorlagen** dieses Tutorials beschrieben.

*  **Karte mit Wasserversorgungsbereich**: Für die Karte wird Azure Maps verwendet, und Sie können die Konfiguration direkt in Azure IoT Central durchführen. Auf der Kachel mit der Karte wird der Gerätestandort angezeigt. Bewegen Sie den Mauszeiger auf die Karte, und probieren Sie die Steuerelemente aus, z. B. *Vergrößern*, *Verkleinern* und *Erweitern*. 

    ![Karte im Dashboard zur Überwachung des Wasserverbrauchs](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboard-map.png)

* **Liniendiagramm zur durchschnittlichen Wassermenge** und **Liniendiagramm zu Umgebungsbedingungen**: Sie können eine oder mehrere Gerätetelemetrien visualisieren, die als Liniendiagramm mit einem bestimmten Zeitbereich dargestellt werden.  

* **Wärmebild zum durchschnittlichen Ventildruck**: Sie können die Visualisierung per Wärmebild für Gerätetelemetriedaten auswählen, die für Sie interessant sind, um die Versorgung für einen Zeitbereich anhand eines Farbindex anzuzeigen.

* **Inhaltskachel zum Zurücksetzen des Warnungsschwellenwerts**: Sie können Inhaltskacheln mit Handlungsaufforderungen bereitstellen und den Link zu einer Seite zum Durchführen einer Aktion einbetten. In diesem Fall gelangen Sie mit „Warnungsschwellenwert zurücksetzen“ zur Anwendung **Aufträge**, mit der Sie Updates für Geräteeigenschaften durchführen können. Dies wird später im Abschnitt **Konfigurieren von Aufträgen** dieses Tutorials beschrieben.

* **Kacheln für Eigenschaften**: Im Dashboard werden die Kacheln **Valve operational info** (Betriebsinformationen zum Ventil), **Flow alert thresholds** (Warnungsschwellenwerte für Wasserfluss) und **Maintenance info** (Wartungsinformationen) angezeigt. Hierbei handelt es sich um Geräteeigenschaften.  


### <a name="customize-dashboard"></a>Anpassen des Dashboards 

Als Lösungsentwickler können Sie Ansichten im Dashboard für Ihre Bediener anpassen. Sie können Folgendes ausprobieren:
1. Klicken Sie auf **Bearbeiten**, um das **Wide World Water-Dashboard zur Überwachung des Wasserverbrauchs** anzupassen. Sie können das Dashboard anpassen, indem Sie auf das Menü **Bearbeiten** klicken. Wenn sich das Dashboard im Modus **Bearbeiten** befindet, können Sie neue Kacheln hinzufügen oder Konfigurationen vornehmen. 

     ![Bearbeiten des Dashboards](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-edit-dashboard.png)

2. Klicken Sie auf **+ Neu**, um ein neues Dashboard zu erstellen und es neu zu konfigurieren. Sie können mehrere Dashboards nutzen und über das entsprechende Menü zwischen Ihren Dashboards navigieren. 

## <a name="explore-device-template"></a>Erkunden der Gerätevorlage
Mit einer Gerätevorlage in Azure IoT Central wird die Funktion eines Geräts definiert, z. B. Telemetriedaten, Eigenschaft oder Befehl. Als Lösungsentwickler können Sie in IoT Central eine oder mehrere Gerätevorlagen definieren, die für die Funktionen der zu verbindenden Geräte stehen. 
 

Die Anwendung zur **Überwachung des Wasserverbrauchs** verfügt über zwei Vorlagen für Referenzgeräte: *Flow Meter* (Strömungsmesser) und *Smart Valve* (Intelligentes Ventil). 

Zeigen Sie die Gerätevorlage wie folgt an:

1. Klicken Sie in IoT Central im linken Navigationsbereich Ihrer Anwendung auf **Gerätevorlagen**. 
    In der Liste mit den Gerätevorlagen werden die beiden Gerätevorlagen für den **Strömungsmesser** und das **intelligente Ventil** angezeigt.

   ![Gerätevorlage](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate.png)

2. Klicken Sie auf die Gerätevorlage **Flow meter** (Strömungsmesser), und machen Sie sich mit den Gerätefunktionen vertraut. 

     ![Gerätevorlage für Strömungsmesser](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-flowmeter.png)

### <a name="customizing-the-device-template"></a>Anpassen der Gerätevorlage

Versuchen Sie, Folgendes anzupassen:
1. Navigieren Sie im Menü für Gerätevorlagen zu **Anpassen**.
2. Suchen Sie nach dem Telemetrietyp `Temperature`.
3. Aktualisieren Sie **Anzeigename**, indem Sie `Temperature` in `Reported temperature` ändern.
4. Aktualisieren Sie die Maßeinheit, oder legen Sie Werte für *Minimalwert* und *Maximalwert* fest.
5. Wählen Sie **Speichern**, um die Änderungen zu speichern. 

### <a name="add-a-cloud-property"></a>Hinzufügen einer Cloudeigenschaft 
1. Navigieren Sie über das Gerätevorlagenmenü zu **Cloudeigenschaft**.
2. Fügen Sie eine neue Cloudeigenschaft hinzu, indem Sie auf **+ Cloudeigenschaft hinzufügen** klicken. 
    In IoT Central können Sie eine Eigenschaft hinzufügen, die für das Gerät relevant ist. Bei einer Cloudeigenschaft kann es sich beispielsweise um einen Warnungsschwellenwert handeln, der für einen bestimmten Installationsbereich oder bestimmte Ressourcen- oder Wartungsinformationen gilt. 
3. Wählen Sie **Speichern**, um die Änderungen zu speichern. 
 
### <a name="views"></a>Ansichten 
Die Gerätevorlage zur Überwachung des Wasserverbrauchs verfügt über vordefinierte Ansichten. Sie können die Ansichten erkunden und Updates vornehmen. In den Ansichten ist definiert, wie Bedienern die Gerätedaten angezeigt werden, aber es können auch Cloudeigenschaften eingegeben werden. 

  ![Ansichten der Gerätevorlage](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-views.png)

### <a name="publish"></a>Veröffentlichen 
Falls Sie Änderungen vorgenommen haben, sollten Sie für die Gerätevorlage die Option **Veröffentlichen** verwenden. 

### <a name="create-a-new-device-template"></a>Erstellen einer neuen Gerätevorlage 
- Wählen Sie die Option **+ Neu** aus, um eine neue Gerätevorlage zu erstellen, und führen Sie die Schritte des Erstellungsprozesses aus. Sie können eine benutzerdefinierte Gerätevorlage von Grund auf neu erstellen oder im Azure-Gerätekatalog eine Gerätevorlage auswählen. 

## <a name="explore-simulated-devices"></a>Erkunden von simulierten Geräten
In IoT Central können Sie simulierte Geräte erstellen, um Ihre Gerätevorlage und die Anwendung zu testen. Die Anwendung zur **Überwachung des Wasserverbrauchs** verfügt über zwei simulierte Geräte, die den Gerätevorlagen für den *Strömungsmesser* und das *intelligente Ventil* zugeordnet sind. 

### <a name="to-view-the-devices"></a>Zeigen Sie die Geräte wie folgt an:
1. Navigieren Sie im linken Navigationsbereich von IoT Central zu **Gerät**. 

   ![Geräte](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devices.png)

2. Klicken Sie auf ein intelligentes Ventil (**Smart Valve 1**). 

    ![Gerät 1](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitor-device1.png)

3.  Unter **Gerätebefehle** werden die drei Gerätebefehle *Open Valve* (Ventil öffnen), *Close Valve* (Ventil schließen) und *Set Valve position* (Ventilposition festlegen) angezeigt. Diese Funktionen sind in der Gerätevorlage für intelligente Ventile (*Smart Valve*) definiert. 
4. Erkunden Sie die Registerkarten **Geräteeigenschaften** und **Gerätedashboard**. 

> [!NOTE]
> Beachten Sie, dass alle Registerkarten über die Ansichten der Gerätevorlage konfiguriert wurden.

### <a name="add-new-devices"></a>Hinzufügen neuer Geräte
* Fügen Sie neue Geräte hinzu, indem Sie auf der Registerkarte **Geräte** auf **+ Neu** klicken. 

## <a name="explore-and-configure-rules"></a>Erkunden und Konfigurieren von Regeln

In Azure IoT Central können Sie Regeln zum automatischen Überwachen der Gerätetelemetrie erstellen und festlegen, dass Aktionen ausgelöst werden, wenn eine oder mehrere Bedingungen erfüllt sind. Die Aktionen können das Senden von E-Mail-Benachrichtigungen oder das Auslösen einer Microsoft Flow- oder Webhookaktion zum Senden von Daten an andere Dienste umfassen.

Die Anwendung **Überwachung des Wasserverbrauchs**, die Sie mit der Vorlage erstellt haben, verfügt über drei vorkonfigurierte Regeln.

### <a name="to-view-rules"></a>Zeigen Sie die Regeln wie folgt an:
1. Navigieren Sie im linken Navigationsbereich von IoT Central zu **Regeln**. 

   ![Regeln](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-rules.png)

2. Wählen Sie **High pH alert** (Warnung: Hoher pH-Wert) aus, und klicken Sie darauf. Dies ist eine der vorkonfigurierten Regeln in der Anwendung.

     ![Warnung: Hoher pH-Wert](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-highflowalert.png)

    Die Regel `High flow alert` ist so konfiguriert, dass die Bedingung `Acidity (pH)` auf Folgendes überprüft wird: `greater than` `Max flow threshold`. Der Schwellenwert für die maximale Flussmenge (Max flow threshold) ist eine Cloudeigenschaft, die in der Gerätevorlage für intelligente Ventile (*Smart Valve*) definiert ist. Der Wert von `Max flow threshold` wird pro Geräteinstanz festgelegt. 

Als Nächstes erstellen wir eine E-Mail-Aktion.

So fügen Sie der Regel eine Aktion hinzu:

1. Wählen Sie **+ E-Mail** aus. 
1. Geben Sie unter **Anzeigename** den Namen *High pH alert* (Warnung: Hoher pH-Wert) für die Aktion ein.
    * Geben Sie unter **An** die E-Mail-Adresse ein, die Ihrem IoT Central-Konto zugeordnet ist. 
1. Geben Sie optional eine Notiz ein, die in den Text der E-Mail aufgenommen werden soll.
1. Wählen Sie **Fertig** aus, um die Aktion fertig zu stellen.
1. Wählen Sie **Speichern** aus, um die neue Regel zu speichern und zu aktivieren. 

Sie sollten nach einigen Minuten eine E-Mail mit dem Hinweis erhalten, dass die konfigurierte **Bedingung** erfüllt wurde.

> [!NOTE]
> Von der Anwendung wird jedes Mal eine E-Mail gesendet, wenn eine Bedingung erfüllt ist. **Deaktivieren** Sie die Regel, um den Empfang von E-Mails über die automatisierte Regel zu beenden. 
  
Erstellen Sie wie folgt eine neue Regel: 
- Wählen Sie im Navigationsbereich auf der linken Seite unter **Regeln** die Option **+ Neu** aus.

## <a name="configure-jobs"></a>Konfigurieren von Aufträgen

In IoT Central können Sie über Aufträge für mehrere Geräte Updates für Geräte- oder Cloudeigenschaften auslösen. Sie können Aufträge nicht nur für Eigenschaften verwenden, sondern auch zum Auslösen von Gerätebefehlen auf mehreren Geräten. Der Workflow wird von IoT Central für Sie automatisiert. 

1. Navigieren Sie im Navigationsbereich auf der linken Seite zu **Aufträge**. 
2. Klicken Sie auf **+ Neu**, und konfigurieren Sie einen oder mehrere Aufträge. 


## <a name="customize-your-application"></a>Anpassen Ihrer Anwendung 
Als Lösungsersteller können Sie verschiedene Einstellungen ändern, um die Benutzeroberfläche in Ihrer Anwendung anzupassen.

1.  Navigieren Sie zu **Verwaltung > Anwendung anpassen**.
3. Verwenden Sie die Schaltfläche **Ändern**, um ein Bild auszuwählen, das als **Anwendungslogo** hochgeladen werden soll.
4. Verwenden Sie die Schaltfläche **Ändern**, um ein Bild für das **Browsersymbol** auszuwählen, das auf den Registerkarten des Browsers angezeigt wird.
5. Sie können auch die standardmäßigen **Browserfarben** ersetzen, indem Sie hexadezimale HTML-Farbcodes hinzufügen.

   ![Azure IoT Central: Anpassen Ihrer Anwendung](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-customize-your-application.png)

1.  Sie können Anwendungsbilder ändern, indem Sie unter **Verwaltung > Anwendungseinstellungen** auf die Schaltfläche **Bild auswählen** klicken, um ein Bild für den Upload als Anwendungsbild auszuwählen. 
2. Abschließend können Sie noch das **Design** ändern, indem Sie im Mastertitel der Anwendung auf **Einstellungen** klicken. 

  
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, sollten Sie sie mit den folgenden Schritten löschen:

1. Öffnen Sie im linken Bereich Ihrer IoT Central-Anwendung die Registerkarte „Verwaltung“. 
2. Wählen Sie „Application settings“ (Anwendungseinstellungen) aus, und klicken Sie unten auf der Seite auf die Schaltfläche „Delete“ (Löschen). 


## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die [Konzepte im Zusammenhang mit der Überwachung des Wasserverbrauchs](./concepts-waterconsumptionmonitoring-architecture.md).
