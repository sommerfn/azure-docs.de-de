---
title: 'Tutorial: Einrichten einer Azure Time Series Insights Preview-Umgebung'
description: 'Tutorial: Erfahren Sie, wie Sie Ihre Umgebung in Azure Time Series Insights Preview einrichten.'
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/11/2019
ms.custom: seodec18
ms.openlocfilehash: 49960ae91688b97a9d38a65e1b17fd277508b996
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114572"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Einrichten einer Azure Time Series Insights Preview-Umgebung

Dieses Tutorial führt Sie durch das Erstellen einer Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen einer Azure Time Series Insights Preview-Umgebung.
> * Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einem IoT Hub.
> * Ausführen eines Solution Accelerator-Beispiels zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
> * Durchführen einer grundlegenden Analyse für die Daten
> * Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und Zuordnen zu Ihren Instanzen
> * Verwenden Sie den Power BI-Connector, und visualisieren Sie Daten in Power BI.

>[!TIP]
> Mit [IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators) werden für Unternehmen konzipierte vorkonfigurierte Lösungen bereitgestellt, mit denen Sie die Entwicklung benutzerdefinierter IoT-Lösungen beschleunigen können.

Registrieren Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/), falls Sie noch keins besitzen.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen mindestens über die Rolle **Mitwirkender** für das Azure-Abonnement verfügen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

In diesem Abschnitt werden Sie drei simulierte Geräte erstellen, die Daten an eine Azure IoT Hub-Instanz senden.

1. Öffnen Sie die Seite [Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators). Auf dieser Seite finden Sie mehrere vorgefertigte Beispiele. Melden Sie sich mit Ihrem Azure-Konto an. Wählen Sie dann **Gerätesimulation** aus.

   [![Seite „Azure IoT Solution Accelerators“](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

1. Wählen Sie auf der nächsten Seite die Option **Jetzt testen** aus.

   [![Seite für Gerätesimulation](media/v2-update-provision/device-two-try.png)](media/v2-update-provision/device-two-try.png#lightbox)

1. Legen Sie auf der Seite **Create Device Simulation solution** (Gerätesimulationslösung erstellen) die folgenden Parameter fest:

    | Parameter | Aktion |
    | --- | --- |
    | **Bereitstellungsname** | Geben Sie für eine neue Ressourcengruppe einen eindeutigen Wert ein. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen. |
    | **Azure-Abonnement** | Wählen Sie das Abonnement aus, für das Sie Ihre Time Series Insights-Umgebung erstellen werden. |
    | **Azure-Standort** | Wählen Sie die Region aus, in der Sie Ihre Time Series Insights-Umgebung speichern werden. Beachten Sie, dass der Gerätesimulator nur in einer begrenzten Anzahl von Regionen angeboten wird. Wenn die gewünschte Region also nicht angezeigt wird, können Sie einen Standort ausschließlich für das Tutorial auswählen und dann eine neue TSI-Umgebung erstellen, wenn Sie bereit sind, in die nächste Phase des Onboarding überzugehen.  |
    | **Bereitstellungsoptionen** | Wählen Sie **Neuen IoT-Hub bereitstellen** aus. |

    1. Klicken Sie auf **Erstellen**.
    [![Seite für die Gerätesimulation](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. Nach etwa 20 Minuten ist Ihr Solution Accelerator bereit.

    [![Seite für die Gerätesimulation](media/v2-update-provision/device-two-ready.png)](media/v2-update-provision/device-two-ready.png#lightbox)

## <a name="create-a-preview-payg-environment"></a>Erstellen einer Vorschauumgebung für nutzungsbasierte Zahlung

In diesem Abschnitt wird beschrieben, wie Sie eine Azure Time Series Insights Preview-Umgebung erstellen und mit dem IoT-Hub verbinden, der vom IoT Solution Accelerator über das [Azure-Portal](https://portal.azure.com/) erstellt wurde.

1. Melden Sie sich unter Verwendung Ihres Abonnementkontos beim Azure-Portal an.

1. Wählen Sie **Ressource erstellen** > **Internet der Dinge** > **Time Series Insights**.

   [![„Internet der Dinge“ auswählen und dann „Time Series Insights“](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Legen Sie im Bereich **Time Series Insights-Umgebung erstellen** auf der Registerkarte **Grundlagen** die folgenden Parameter fest:

    | Parameter | Aktion |
    | --- | ---|
    | **Umgebungsname** | Geben Sie einen eindeutigen Namen für die Azure Time Series Insights-Umgebung (Vorschauversion) ein. |
    | **Abonnement** | Geben Sie das Abonnement ein, in dem Sie die Azure Time Series Insights-Umgebung (Vorschauversion) erstellen möchten. Eine bewährte Methode ist die Verwendung des Abonnements, das Sie auch für die anderen vom Gerätesimulator erstellten IoT-Ressourcen verwenden. |
    | **Ressourcengruppe** | Wählen Sie eine vorhandene Ressourcengruppe für die Ressource der Azure Time Series Insights-Umgebung (Vorschauversion) aus, oder erstellen Sie eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Eine bewährte Methode ist die Verwendung der Ressourcengruppe, die Sie auch für die anderen vom Gerätesimulator erstellten IoT-Ressourcen verwenden. |
    | **Location** | Wählen Sie eine Rechenzentrumsregion für Ihre Azure Time Series Insights Preview-Umgebung aus. Um zusätzliche Wartezeiten zu vermeiden, empfiehlt es sich, die Azure Time Series Insights Preview-Umgebung in derselben Region wie Ihre IoT Hub-Instanz zu erstellen, die vom Gerätesimulator erstellt wurde. |
    | **Tier** |  Wählen Sie ***Nutzungsbasierte Zahlung*** aus. Dies ist die SKU für das Azure Time Series Insights Preview-Produkt. |
    | **Eigenschafts-ID** | Geben Sie einen Wert ein, der Ihre Zeitreiheninstanz eindeutig identifiziert. Der Wert, den Sie in das Feld **Eigenschafts-ID** eingeben, kann später nicht geändert werden. Geben Sie für dieses Tutorial **iothub-connection-device-id** ein. Weitere Informationen zur Zeitreihen-ID finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./time-series-insights-update-how-to-id.md). |
    | **Speicherkontoname** | Geben Sie einen global eindeutigen Namen für ein neues Speicherkonto ein.|
    |**Warm Storage aktivieren**|Wählen Sie **Ja** aus, um den Warm Storage zu aktivieren.|
    |**Datenaufbewahrung in Tagen**|Wählen Sie die Standardoption von sieben Tagen aus. |

    Klicken Sie auf **Weiter: Ereignisquelle**.

   [![Bereich zum Erstellen einer Time Series Insights Umgebung](media/v2-update-provision/payg-two-1-create.png)](media/v2-update-provision/payg-two-1-create.png#lightbox)
   [![Bereich zum Erstellen einer Time Series Insights Umgebung](media/v2-update-provision/payg-two-2-create.png)](media/v2-update-provision/payg-two-2-create.png#lightbox)

1. Legen Sie auf der Registerkarte **Ereignisquelle** die folgenden Parameter fest:

   | Parameter | Aktion |
   | --- | --- |
   | **Ereignisquelle erstellen?** | Wählen Sie **Ja** aus.|
   | **Name** | Geben Sie einen eindeutigen Wert für den Namen der Ereignisquelle ein. |
   | **Quellentyp** | Wählen Sie **IoT Hub** aus. |
   | **Hub auswählen** | Wählen Sie **Vorhandenen auswählen** aus. |
   | **Abonnement** | Wählen Sie das Abonnement aus, das Sie für den Gerätesimulator verwendet haben. |
   | **IoT Hub-Name** | Wählen Sie den Namen des IoT-Hubs aus, den Sie für den Gerätesimulator erstellt haben. |
   | **Zugriffsrichtlinie für IoT Hub** | Wählen Sie **iothubowner** aus. |
   | **IoT Hub-Consumergruppe** | Wählen Sie **Neu** aus, geben Sie einen eindeutigen Namen ein, und wählen Sie dann **Hinzufügen** aus. Bei der Consumergruppe muss es sich um einen eindeutigen Wert in Azure Time Series Insights (Vorschauversion) handeln. |
   | **Timestamp property** (Timestamp-Eigenschaft) | Mit diesem Wert wird die Eigenschaft **Zeitstempel** in Ihren eingehenden Telemetriedaten identifiziert. Lassen Sie das Feld für dieses Tutorial leer. Dieser Simulator verwendet den aus IoT Hub eingehenden Zeitstempel, der standardmäßig von Time Series Insights verwendet wird. |

   Klicken Sie auf **Überprüfen + erstellen**.

   [![Konfigurieren einer Ereignisquelle](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Klicken Sie auf **Erstellen**.

    [![Seite „Überprüfen + erstellen“ mit Schaltfläche „Erstellen“](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

    Sie können den Status Ihrer Bereitstellung anzeigen:

    [![Benachrichtigung, dass die Bereitstellung abgeschlossen ist](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Wenn Sie ein Besitzer des Azure-Abonnements sind, haben Sie Zugriff auf Ihre Azure Time Series Insights Preview-Umgebung. Vergewissern Sie sich, dass Sie Zugriff haben:

   1. Suchen Sie nach Ihrer Ressourcengruppe, und wählen Sie dann Ihre neu erstellte Azure Time Series Insights Preview-Umgebung aus: 
      [![Ausgewählte Umgebung](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   1. Wählen Sie auf der Seite für die Azure Time Series Insights-Vorschauversion die Option **Datenzugriffsrichtlinien** aus: [![Datenzugriffsrichtlinien](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   1. Stellen Sie sicher, dass Ihre Anmeldeinformationen aufgelistet sind:

      [![Aufgeführte Anmeldeinformationen](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Falls Ihre Anmeldeinformationen nicht aufgelistet sind, müssen Sie sich selbst die Zugriffsberechtigung für die Umgebung erteilen, indem Sie „Hinzufügen“ auswählen und nach Ihren Anmeldeinformationen suchen. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [Gewähren von Datenzugriff für eine Umgebung](./time-series-insights-data-access.md).

## <a name="stream-data"></a>Streamen von Daten

Sie haben Ihre Time Series Insights-Umgebung bereitgestellt und können jetzt mit dem Streaming von Daten zur Analyse beginnen.

1. Navigieren Sie zurück zur Seite [Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators). Suchen Sie Ihre Lösung in Ihrem Solution Accelerator-Dashboard, und wählen Sie dann **Launch** (Starten) aus:

    [![Starten der Gerätesimulationslösung](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Wählen Sie **Go to your Solution Accelerator** (Zum Solution Accelerator wechseln) aus.

    [![Starten der Gerätesimulationslösung](media/v2-update-provision/device-accelerator.png)](media/v2-update-provision/device-accelerator.png#lightbox)

1. Sie werden zur Seite **Microsoft Azure IoT-Gerätesimulation** weitergeleitet. Wählen Sie rechts oben auf der Seite die Option **Neue Simulation** aus.

    [![Azure IoT-Simulationsseite](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Legen Sie im Bereich **Simulation setup** (Simulationssetup) die folgenden Parameter fest:

    | Parameter | Aktion |
    | --- | --- |
    | **Name** | Geben Sie einen eindeutigen Namen für einen Simulator ein. |
    | **Beschreibung** | Geben Sie eine Definition ein. |
    | **Simulationsdauer** | Verwenden Sie die Option **Run indefinitely** (Unbegrenzt). |
    | **Gerätemodell** | Klicken Sie auf **+ Add a device type** (Gerätetyp hinzufügen). <br />**Name**: Geben Sie **Elevator** (Aufzug) ein. <br />**Menge**: Geben Sie **3** ein. <br /> Belassen Sie die übrigen Werte in ihrer Standardeinstellung. |
    | **IoT-Zielhub** | Verwenden Sie die Option **Use pre-provisioned IoT Hub** (Vorab bereitgestellten IoT-Hub verwenden). |

    [![Festzulegende Parameter](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    Wählen Sie **Simulation starten** aus.

    Auf dem Gerätesimulationsdashboard werden die **aktiven Geräte** und die **Gesamtzahl der Nachrichten** angezeigt.

    [![Azure IoT-Simulationsdashboard](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="analyze-data"></a>Daten analysieren

In diesem Abschnitt führen Sie mit dem [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) grundlegende Analysen für Ihre Zeitreihendaten durch.

1. Navigieren Sie zu Ihrem Azure Time Series Insights Preview-Explorer, indem Sie im [Azure-Portal](https://portal.azure.com/) auf der Ressourcenseite die URL auswählen.

    [![Die URL des Time Series Insights Preview-Explorers](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Im Time Series Insights-Explorer wird ein Balken angezeigt, der den oberen Rand des Bildschirms umfasst. Dies ist Ihre Verfügbarkeitsauswahl. Stellen Sie sicher, dass Sie mindestens zwei 2 Minuten ausgewählt haben, und erweitern Sie bei Bedarf den Zeitrahmen, indem Sie die Auswahlhandles auswählen und nach links und rechts ziehen.

1. **Time Series-Instanzen** werden auf der linken Seite angezeigt.


    [![Liste von Instanzen ohne übergeordnete Elemente](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Wählen Sie die erste Zeitreiheninstanz aus. Wählen Sie dann **Show pressure** (Druck anzeigen) aus.

    [![Ausgewählte Zeitreiheninstanz mit Menübefehl zum Anzeigen des durchschnittlichen Drucks](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

    Ein Diagramm mit Zeitreihen wird angezeigt. Ändern Sie den Wert für das **Intervall** in **30** Sekunden.

    [![Zeitreihendiagramm](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Wiederholen Sie Schritt 3 mit den anderen beiden Zeitreiheninstanzen, damit Sie alle drei anzeigen können, wie in diesem Diagramm dargestellt:

    [![Diagramm mit allen Zeitreihen](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Wählen Sie die Zeitspannenauswahl in der oberen rechten Ecke aus. Hier können Sie bestimmte Start- und Endzeiten bis auf die Millisekunde genau auswählen oder aus vorkonfigurierten Optionen wie „Letzte Stunde“ auswählen. Sie können auch die Standardzeitzone ändern.

    [![Legen Sie den Zeitbereich auf eine Stunde fest](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

    Nachfolgend sehen Sie eine Bildschirmaufnahme des Diagrammfensters, nachdem Sie die Simulation eine Stunde lang ausgeführt haben:

    [![Diagrammbereich](media/v2-update-provision/analyze-seven-time.png)](media/v2-update-provision/analyze-seven-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definieren und Anwenden eines Modells

In diesem Abschnitt wenden Sie ein Modell zum Strukturieren der Daten an. Um das Modell zu vervollständigen, definieren Sie Typen, Hierarchien und Instanzen. Weitere Informationen zur Datenmodellierung finden Sie unter [Time Series-Modell](./time-series-insights-update-tsm.md).

1. Wählen Sie im Explorer die Registerkarte **Modell** aus:

   [![Registerkarte „Modell“ im Explorer](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Wählen Sie auf der Registerkarte **Typen** die Option **Hinzufügen** aus.

   [![Schaltfläche „Hinzufügen“ für Typen](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Legen Sie die folgenden Parameter fest:

    | Parameter | Aktion |
    | --- | ---|
    | **Name** | Geben Sie **Elevator** (Aufzug) ein. |
    | **Beschreibung** | Geben Sie **Dies ist eine Typdefinition für Aufzug** ein. |

    [![Schaltfläche „Hinzufügen“ für Typen](media/v2-update-provision/define-three-properties.png)](media/v2-update-provision/define-three-properties.png#lightbox)

1. Wählen Sie als nächstes die Registerkarte **Variablen** aus. [![Registerkarte „Variablen“ auswählen](media/v2-update-provision/define-four-variables.png)](media/v2-update-provision/define-four-variables.png#lightbox)

1. Wählen Sie **+ Add Variable** (Variable hinzufügen) aus, und geben Sie die folgenden Werte für die erste Variable vom Typ „Elevator“ (Aufzug) ein. Sie werden insgesamt drei Variablen erstellen.

    | Parameter | Aktion |
    | --- | --- |
    | **Name** | Geben Sie **Avg Temperature** ein. |
    | **Kind** | Wählen Sie **Numeric** (Numerisch) aus. |
    | **Wert** | Aus Voreinstellung auswählen: Wählen Sie **temperature (Double) (Temperatur (Double))** aus. <br /> Hinweis: Es kann einige Minuten dauern, bis das Feld **Wert** automatisch aufgefüllt wird, nachdem Azure Time Series Insights (Vorschauversion) damit beginnt, Ereignisse zu empfangen.|
    | **Aggregationsvorgang** | Erweitern Sie **Erweiterte Optionen**. <br /> Wählen Sie **AVG** aus. |

    [![Auswahloptionen zum Definieren der Temperatur](media/v2-update-provision/define-five-variable.png)](media/v2-update-provision/define-five-variable.png#lightbox)

    Wählen Sie **Übernehmen**.

    Wählen Sie **+ Add Variable** (Variable hinzufügen) erneut aus, und legen Sie die folgenden Werte fest:

    | Parameter | Aktion |
    | --- | --- |
    | **Name** | Geben Sie **Avg Vibration** (Durchschnittliche Vibration) ein. |
    | **Kind** | Wählen Sie **Numeric** (Numerisch) aus. |
    | **Wert** | Aus Voreinstellung auswählen: Wählen Sie **Vibration (Double)** aus. <br /> Hinweis: Es kann einige Minuten dauern, bis das Feld **Wert** automatisch aufgefüllt wird, nachdem Azure Time Series Insights (Vorschauversion) damit beginnt, Ereignisse zu empfangen.|
    | **Aggregationsvorgang** | Erweitern Sie **Erweiterte Optionen**. <br /> Wählen Sie **AVG** aus. |

    [![Auswahloptionen zum Definieren der Vibration](media/v2-update-provision/define-five-vibration.png)](media/v2-update-provision/define-five-vibration.png#lightbox)

    Wählen Sie **Übernehmen**.

    Wählen Sie **+ Add Variable** (Variable hinzufügen) erneut aus, und legen Sie die folgenden Werte für die dritte und letzte Variable fest:

    | Parameter | Aktion |
    | --- | --- |
    | **Name** | Geben Sie **Floor** (Etage) ein. |
    | **Kind** | Wählen Sie **Categorical** (Kategorisch) aus. |
    | **Wert** | Aus Voreinstellung auswählen: Wählen Sie **Floor (Double)** (Etage (Double)) aus. <br /> Hinweis: Es kann einige Minuten dauern, bis das Feld **Wert** automatisch aufgefüllt wird, nachdem Azure Time Series Insights (Vorschauversion) damit beginnt, Ereignisse zu empfangen.|
    | **Kategorien** | <span style="text-decoration: underline">Bezeichnung </span>  - <span style="text-decoration: underline">Werte</span> <br /> Unten: 1,2,3,4 <br /> Mitte: 5,6,7,8,9 <br /> Oben: 10,11,12,13,14,15 |
    | **Standardkategorie** | Geben Sie **Unbekannt** ein. |

    [![Auswahloptionen zum Definieren der Vibration](media/v2-update-provision/define-five-floor.png)](media/v2-update-provision/define-five-floor.png#lightbox)

    Wählen Sie **Übernehmen**. Es sollten drei erstellte Variablen angezeigt werden:

    [![Auswahloptionen zum Definieren der Vibration](media/v2-update-provision/define-five-floor2.png)](media/v2-update-provision/define-five-floor2.png#lightbox)

   Wählen Sie **Speichern** aus. Es wird der erstellte **Typ** angezeigt:

    [![Auswahloptionen zum Definieren der Vibration](media/v2-update-provision/define-five-floor3.png)](media/v2-update-provision/define-five-floor3.png#lightbox)

1. Wählen Sie die Registerkarte **Hierarchies** (Hierarchien) aus. Wählen Sie **+ Hinzufügen** aus.

    [![Registerkarte „Hierarchien“ mit Schaltfläche „Hinzufügen“](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Legen Sie im Bereich **Hierarchie bearbeiten** die folgenden Parameter fest:

   | Parameter | Aktion |
   | --- | ---|
   | **Name** | Geben Sie **Location Hierarchy** ein. |
   |**Ebenen**| Geben Sie **Country** (Land) als Namen der ersten Ebene ein. <br> Wählen Sie **+ Add Level** (Ebene hinzufügen) aus. <br> Geben Sie **City** (Ort) für die zweite Ebene ein, und wählen Sie dann **+ Add Level** (Ebene hinzufügen) aus. <br> Geben Sie **Building** (Gebäude) als Namen der dritten und letzten Ebene ein. |

   Wählen Sie **Speichern** aus.

    [![Hierarchiefelder mit Schaltfläche „Erstellen“](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

   Die erstellte Hierarchie wird angezeigt:

    [![Informationen zur Hierarchie](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Navigieren Sie zu **Instanzen**. Wählen Sie ganz rechts unter den **Aktionen** das Stiftsymbol aus, um die erste Instanz mit den folgenden Werten zu bearbeiten:

    | Parameter | Aktion |
    | --- | --- |
    | **Typ** | Wählen Sie **Elevator** (Aufzug) aus. |
    | **Name** | Geben Sie **Elevator 1** (Aufzug 1) ein.|
    | **Beschreibung** | Geben Sie **Instance for Elevator 1** (Instanz für Aufzug 1) ein. |

    [![Auswählen der Schaltfläche „Bearbeiten“ für eine Instanz](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

    Navigieren Sie zu **Instance Fields** (Instanzfelder), und geben Sie Folgendes ein:

    | Parameter | Aktion |
    | --- | --- |
    | **Hierarchien** | Wählen Sie **Location Hierarchy** (Standorthierarchie) aus. |
    | **Country** | Geben Sie **USA** ein. |
    | **City (Ort)** | Geben Sie **Seattle** ein. |
    | **Building (Gebäude)** | Geben Sie **Space Needle** ein. |

    [![Auswählen der Schaltfläche „Bearbeiten“ für eine Instanz](media/v2-update-provision/define-ten2-edit.png)](media/v2-update-provision/define-ten2-edit.png#lightbox)

    Wählen Sie **Speichern** aus.

1. Wiederholen Sie Schritt 8 mit den beiden anderen Instanzen mit den folgenden Werten:

    <span style="text-decoration: underline;">Für Aufzug 2</span>:

    | Parameter | Aktion |
    | --- | --- |
    | **Typ** | Wählen Sie **Elevator** (Aufzug) aus. |
    | **Name** | Geben Sie **Elevator 2** (Aufzug 2) ein.|
    | **Beschreibung** | Geben Sie **Instance for Elevator 2** (Instanz für Aufzug 2) ein. |
    | **Hierarchien** | Wählen Sie **Location Hierarchy** (Standorthierarchie) aus. |
    | **Country** | Geben Sie **USA** ein. |
    | **City (Ort)** | Geben Sie **Seattle** ein. |
    | **Building (Gebäude)** | Geben Sie **Pacific Science Center** ein. |

    <span style="text-decoration: underline;">Für Aufzug 3</span>:

    | Parameter | Aktion |
    | --- | --- |
    | **Typ** | Wählen Sie **Elevator** (Aufzug) aus. |
    | **Name** | Geben Sie **Elevator 3** (Aufzug 3) ein.|
    | **Beschreibung** | Geben Sie **Instance for Elevator 3** (Instanz für Aufzug 3) ein. |
    | **Hierarchien** | Wählen Sie **Location Hierarchy** (Standorthierarchie) aus. |
    | **Country** | Geben Sie **USA** ein. |
    | **City (Ort)** | Geben Sie **New York** ein. |
    | **Building (Gebäude)** | Geben Sie **Empire State Building** ein. |

1. Navigieren Sie zurück zur Registerkarte **Analyze** (Analysieren), um den Diagrammbereich anzuzeigen. Erweitern Sie unter **Location Hierarchy** alle Hierarchieebenen, um die Zeitreiheninstanzen anzuzeigen:

   [![Registerkarte „Analysieren“](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Wählen Sie unter **Pacific Science Center** die Zeitreiheninstanz **Elevator 2** (Aufzug 2) und anschließend **Show Average Temperature** (Durchschnittliche Temperatur anzeigen) aus.

    [![Zeitreiheninstanz „Durchschnittliche Temperatur anzeigen“](media/v2-update-provision/define-eleven-temperature.png)](media/v2-update-provision/define-eleven-temperature.png#lightbox)

1. Wählen Sie für dieselbe Instanz, **Elevator 2** (Aufzug 2), die Option **Show Floor** (Etage anzeigen) aus.

    [![Zeitreiheninstanz „Etage anzeigen“](media/v2-update-provision/define-twelve-floor.png)](media/v2-update-provision/define-twelve-floor.png#lightbox)

    Mit Ihrer Kategorievariablen können Sie bestimmen, wie viel Zeit der Aufzug auf den oberen, unteren und mittleren Etagen verbracht hat.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie nach dem Tutorial die von Ihnen erstellten Ressourcen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** aus, und suchen Sie Ihre Azure Time Series Insights-Ressourcengruppe.
1. Löschen Sie entweder die gesamte Ressourcengruppe (und alle darin enthaltenen Ressourcen), indem Sie auf **Löschen** klicken, oder entfernen Sie die Ressourcen einzeln.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:  

* Erstellen und Verwenden eines Accelerators zur Gerätesimulation.
* Erstellen einer Azure Time Series Insights Preview-PAYG-Umgebung.
* Herstellen einer Verbindung der Azure Time Series Insights Preview-Umgebung mit einem IoT Hub.
* Ausführen eines Solution Accelerator-Beispiels zum Streamen von Daten in die Azure Time Series Insights Preview-Umgebung.
* Durchführen einer grundlegenden Analyse der Daten.
* Definieren eines Zeitreihenmodell-Typs und einer Hierarchie und deren Zuordnung zu Ihren Instanzen.

Da Sie nun Ihre eigene Azure Time Series Insights Preview-Umgebung erstellen können, erfahren Sie mehr über die wichtigsten Konzepte in Azure Time Series Insights.

Informieren Sie sich über die Azure Time Series Insights-Speicherkonfiguration:

> [!div class="nextstepaction"]
> [Speicherung und Datenerfassung in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md)

Erfahren Sie mehr über Zeitreihenmodelle:

> [!div class="nextstepaction"]
> [Datenmodellierung in Azure Time Series Insights](./time-series-insights-update-tsm.md)
