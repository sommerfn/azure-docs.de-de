---
title: Definieren eines neuen Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung einen neuen Gerätetyp definieren. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Einstellungen für den Typ.
author: dominicbetts
ms.author: dobett
ms.date: 06/07/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a3faa76c1506664a075648edc7d57fbba542b011
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960543"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Microsoft Azure IoT Central-Anwendung mithilfe einer Gerätevorlage eine neue Art von Gerät definieren. Eine Gerätevorlage definiert die Telemetriedaten, den Zustand, die Eigenschaften und die Einstellungen für Ihren Gerätetyp.

Damit Sie Ihre Anwendung testen können, bevor Sie eine Verbindung mit einem echten Gerät herstellen, generiert IoT Central auf der Grundlage der erstellten Vorlage ein simuliertes Gerät.

In diesem Tutorial erstellen Sie die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage). Für eine verbundene Klimaanlage gilt Folgendes:

* Sie sendet Telemetriedaten wie Temperatur und Luftfeuchtigkeit.
* Sie meldet den Zustand (etwa, ob sie eingeschaltet ist).
* Sie verfügt über Geräteeigenschaften wie Firmwareversion und Seriennummer.
* Sie verfügt über Einstellungen wie etwa die Zieltemperatur.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Gerätevorlage
> * Hinzufügen von Telemetriedaten zu Ihrem Gerät
> * Anzeigen simulierter Telemetriedaten
> * Definieren der Ereignismessung
> * Anzeigen simulierter Ereignisse
> * Definieren der Zustandsmessung
> * Anzeigen des simulierten Zustands
> * Verwenden von Einstellungen und Eigenschaften
> * Verwenden von Befehlen
> * Anzeigen Ihres simulierten Geräts auf dem Dashboard

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Wenn Sie die Schritte der Schnellstartanleitung [Create an Azure IoT Central application](quick-deploy-iot-central.md) (Erstellen einer Azure IoT Central-Anwendung) ausgeführt haben, können Sie die so erstellte Anwendung verwenden. Führen Sie andernfalls die folgenden Schritte aus, um eine leere Azure IoT Central-Anwendung zu erstellen:

1. Navigieren Sie zur Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral).

2. Geben Sie die E-Mail-Adresse und das Kennwort für den Zugriff auf Ihr Azure-Abonnement ein:

    ![Eingeben Ihres Organisationskontos](./media/tutorial-define-device-type/sign-in.png)

3. Wählen Sie **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen:

    ![Seite des Azure IoT Central-Anwendungs-Managers](./media/tutorial-define-device-type/iotcentralhome.png)

4. So erstellen Sie eine neue Azure IoT Central-Anwendung:
    
   * Wählen Sie **Testversion** aus. Sie benötigen kein Azure-Abonnement zum Erstellen einer Testanwendung.
    
      Weitere Informationen zu Verzeichnissen und Abonnements finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).
    
   * Wählen Sie **Benutzerdefinierte Anwendung** aus.
    
   * Sie können optional auch einen Anzeigenamen für die Anwendung angeben (beispielsweise **Contoso Air Conditioners**). Azure IoT Central generiert automatisch ein eindeutiges URL-Präfix. Dieses URL-Präfix kann in einen einprägsameren Wert geändert werden.
    
   * Klicken Sie auf **Erstellen**.

     ![Azure IoT Central-Seite „Anwendung erstellen“](./media/tutorial-define-device-type/iotcentralcreate.png)

     Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md).

## <a name="create-a-device-template"></a>Erstellen einer Gerätevorlage

Als Ersteller können Sie die Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Wenn Sie eine Gerätevorlage erstellen, generiert Azure IoT Central auf der Grundlage der Vorlage ein simuliertes Gerät. Das simulierte Gerät generiert Telemetriedaten, mit denen Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem echten Gerät herstellen.

Wenn Sie Ihrer Anwendung eine neue Gerätevorlage hinzufügen möchten, navigieren Sie zur Seite **Gerätevorlagen**. Wählen Sie hierzu im linken Navigationsmenü **Gerätevorlagen** aus.

![Seite „Gerätevorlagen“](./media/tutorial-define-device-type/devicetemplates.png)

## <a name="add-a-device-template"></a>Hinzufügen einer Gerätevorlage

In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie für Geräte, die temperaturbezogene Telemetriedaten an Ihre Anwendung senden, eine neue Gerätevorlage namens **Connected Air Conditioner** erstellen:

1. Wählen Sie auf der Seite **Gerätevorlagen** den Befehl **+ Neu** aus:

    ![Seite „Gerätevorlagen“: Erstellen einer Gerätevorlage](./media/tutorial-define-device-type/newtemplate.png)

2. Auf der Seite werden die Vorlagen angezeigt, aus denen Sie auswählen können.

    ![Gerätevorlagenbibliothek](./media/tutorial-define-device-type/devicetemplatelibrary.png)

3. Wählen **Benutzerdefiniert** aus, geben Sie **Connected Air Conditioner** als Namen Ihrer Gerätevorlage ein, und wählen Sie anschließend **Erstellen** aus. Sie können auch ein Bild Ihres Geräts hochladen, das Bedienern im Device Explorer angezeigt wird:

    ![Benutzerdefiniertes Gerät](./media/tutorial-define-device-type/createcustomdevice.png)

4. Vergewissern Sie sich in der Gerätevorlage **Connected Air Conditioner**, dass Sie sich auf der Registerkarte **Messungen** befinden, um die Telemetriedaten zu definieren. Jede Gerätevorlage, die Sie definieren, verfügt über individuelle Registerkarten für Folgendes:

   * Angeben der _Messungen_, die von dem Gerät gesendet werden (etwa Telemetriedaten, Ereignisse und Zustände)

   * Definieren der _Einstellungen_ zum Steuern des Geräts

   * Definieren der _Eigenschaften_, die die Metadaten des Geräts bilden

   * Definieren der _Befehle_, die direkt auf dem Gerät ausgeführt werden sollen

   * Definieren der _Regeln_ für das Gerät

   * Anpassen des _Dashboards_ des Geräts für die Bediener

     ![Messungen für die Klimaanlage](./media/tutorial-define-device-type/airconmeasurements.png)

     > [!NOTE]
     > Wenn Sie den Namen der Gerätevorlage ändern möchten, wählen Sie oben auf der Seite den Vorlagennamen aus.

5. Wählen Sie **+ Neue Messung** aus, um die Messung für die Temperaturtelemetrie hinzuzufügen. Wählen Sie **Telemetrie** als Messungstyp aus:

    ![Messungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Jede Art von Telemetrie, die Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

   * Anzeigeoptionen

   * Telemetriedetails

   * Simulationsparameter

     Verwenden Sie zum Konfigurieren der Telemetrie **Temperature** (Temperatur) die Informationen aus der folgenden Tabelle:

     | Einstellung              | Wert         |
     | -------------------- | -----------   |
     | Anzeigename         | Temperatur   |
     | Feldname           | Temperatur   |
     | Units                | F             |
     | Min                  | 60            |
     | max                  | 110           |
     | Dezimalstellen       | 0             |

     Sie können auch eine Farbe für die Telemetrieanzeige auswählen. Wählen Sie **Speichern** aus, um die Telemetriedefinition zu speichern:

     ![Konfigurieren der Temperatursimulation](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Nach kurzer Zeit wird auf der Registerkarte **Messungen** ein Diagramm der Temperaturtelemetriedaten Ihrer simulierten verbundenen Klimaanlage angezeigt. Mithilfe der Steuerelemente können Sie die Sichtbarkeit sowie die Aggregation verwalten und die Telemetriedefinition bearbeiten:
 
    > [!NOTE]
    > Für die Telemetrie ist **Durchschnitt** als Standardaggregation festgelegt. 

    ![Anzeigen der Temperatursimulation](./media/tutorial-define-device-type/viewsimulation.png)

8. Darüber hinaus können Sie das Diagramm mithilfe der Steuerelemente **Linie**, **Gestapelt** und **Zeitbereich bearbeiten** anpassen:

    ![Anpassen des Diagramms](./media/tutorial-define-device-type/customizechart.png)

## <a name="add-an-event-measurement"></a>Hinzufügen einer Ereignismessung

Mithilfe von Ereignissen können Sie Zeitpunktdaten definieren, die vom Gerät gesendet werden, wenn ein Ereignis wie etwa ein Fehler oder der Ausfall einer Komponente eintritt. Geräteereignisse können von Azure IoT Central simuliert werden, sodass Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem echten Gerät herstellen. Ereignismessungen für Ihre Gerätevorlage werden in der Ansicht **Messungen** definiert.

1. Wählen Sie **+ Neue Messung** aus, um die Ereignismessung **Fan Motor Error** (Lüftermotorfehler) hinzuzufügen. Wählen Sie dann **Ereignis** als Messungstyp aus:

    ![Messungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/eventnew.png)

2. Jede Art von Ereignis, das Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

   * Anzeigename

   * Feldname

   * Schweregrad

     Verwenden Sie zum Konfigurieren des Ereignisses **Fan Motor Error** die Informationen aus der folgenden Tabelle:

     | Einstellung              | Wert             |
     | -------------------- | -----------       |
     | Anzeigename         | Fan Motor Error   |
     | Feldname           | fanmotorerr       |
     | severity             | Error             |

     Wählen Sie **Speichern** aus, um die Ereignisdefinition zu speichern:

     ![Konfigurieren der Ereignismessung](./media/tutorial-define-device-type/eventconfiguration.png)

3. Nach kurzer Zeit wird auf der Registerkarte **Messungen** ein Diagramm der Ereignisse angezeigt, die nach dem Zufallsprinzip auf der Grundlage Ihrer simulierten verbundenen Klimaanlage generiert wurden. Mithilfe der Steuerelemente können Sie die Sichtbarkeit verwalten und die Ereignisdefinition bearbeiten:

    ![Anzeigen der Ereignissimulation](./media/tutorial-define-device-type/eventview.png)

1. Wählen Sie im Diagramm das Ereignis aus, um zusätzliche Ereignisdetails anzuzeigen:

    ![Anzeigen von Ereignisdetails](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>Definieren einer Zustandsmessung

Mithilfe der Zustandsoption können Sie den Zustand des Geräts oder der dazugehörigen Komponenten über einen Zeitraum definieren und visualisieren. Der Gerätezustand kann von Azure IoT Central simuliert werden, sodass Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem echten Gerät herstellen. Zustandsmessungen für den Gerätetyp werden in der Ansicht **Measurements** (Messungen) definiert.

1. Wählen Sie **+ Neue Messung** aus, um die Zustandsmessung **Fan Mode** (Lüftermodus) hinzuzufügen. Wählen Sie dann **Zustand** als Messungstyp aus:

    ![Zustandsmessungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/statenew.png)

2. Jede Art von Zustand, den Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

   * Anzeigename

   * Feldname

   * Werte mit optionalen Anzeigebeschriftungen

   * Wertspezifische Farben

     Verwenden Sie zum Konfigurieren des Zustands **Fan Mode** (Lüftermodus) die Informationen aus der folgenden Tabelle:

     | Einstellung              | Wert             |
     | -------------------- | -----------       |
     | Anzeigename         | Fan Mode          |
     | Feldname           | fanmode           |
     | Wert                | 1                 |
     | Anzeigebeschriftung        | Operating         |
     | Wert                | 0                 |
     | Anzeigebeschriftung        | Beendet           |

     Wählen Sie **Speichern** aus, um die Definition der Zustandsmessung zu speichern:

     ![Konfigurieren der Zustandsmessung](./media/tutorial-define-device-type/stateconfiguration.png)

3. Nach kurzer Zeit wird auf der Registerkarte **Messungen** ein Diagramm der Zustände angezeigt, die nach dem Zufallsprinzip auf der Grundlage Ihrer simulierten verbundenen Klimaanlage generiert wurden. Mithilfe der Steuerelemente können Sie die Sichtbarkeit verwalten und die Zustandsdefinition bearbeiten:

    ![Anzeigen der Zustandssimulation](./media/tutorial-define-device-type/stateview.png)

4. Sollte das Gerät innerhalb kurzer Zeit zu viele Datenpunkte senden, wird die Zustandsmessung anders dargestellt. Wählen Sie das Diagramm aus, um alle Datenpunkte innerhalb dieses Zeitraums in chronologischer Reihenfolge anzuzeigen. Sie können den Zeitbereich auch einschränken, um detailliertere Messungen anzuzeigen.

## <a name="settings-properties-and-commands"></a>Einstellungen, Eigenschaften und Befehle

Einstellungen Eigenschaften und Befehle sind unterschiedliche Werte, die in einer Gerätevorlage definiert und jedem einzelnen Gerät zugeordnet werden:

* _Einstellungen_ dienen dazu, Konfigurationsdaten aus Ihrer Anwendung an ein Gerät zu senden. Mithilfe einer Einstellung kann ein Bediener beispielsweise das Telemetrieintervall des Geräts von zwei Sekunden in fünf Sekunden ändern. Wenn ein Bediener eine Einstellung ändert, wird diese auf der Benutzeroberfläche als ausstehend markiert, bis das Gerät eine Bestätigung sendet.

* _Eigenschaften_ dienen zum Definieren von Metadaten, die mit Ihrem Gerät verknüpft sind. Es gibt zwei Kategorien von Eigenschaften:
    
  * _Anwendungseigenschaften_ dienen zum Erfassen von Geräteinformationen in Ihrer Anwendung. Mithilfe von Anwendungseigenschaften können Sie beispielsweise den Standort eines Geräts und das Datum der letzten Wartung erfassen. Diese Eigenschaften werden in der Anwendung gespeichert und nicht mit dem Gerät synchronisiert. Ein Bediener kann Eigenschaften Werte zuweisen.

  * _Geräteeigenschaften_ dienen dazu, einem Gerät das Senden von Eigenschaftswerten an Ihre Anwendung zu ermöglichen. Diese Eigenschaften können nur durch das Gerät geändert werden. Für Bediener sind Geräteeigenschaften schreibgeschützt. In diesem Szenario für eine verbundene Klimaanlage werden vom Gerät die Geräteeigenschaften „Firmwareversion“ und „Seriennummer des Geräts“ gemeldet.
    
    Weitere Informationen finden Sie in der Schrittanleitung zum Einrichten einer Gerätevorlage unter [Eigenschaften](howto-set-up-template.md#properties).

* Verwenden Sie _Befehle_, um Ihr Gerät aus Ihrer Anwendung remote zu verwalten. Sie können Befehle auf Ihrem Gerät direkt aus der Cloud ausführen, um die Geräte zu steuern. Operatoren können z.B. Befehle wie den Neustart ausführen, um das Gerät sofort neu zu starten.

## <a name="use-settings"></a>Verwenden von Einstellungen

*Einstellungen* ermöglichen es einem Bediener, Konfigurationsdaten an ein Gerät zu senden. In diesem Abschnitt fügen Sie der Gerätevorlage **Connected Air Conditioner** eine Einstellung hinzu, die es Bedienern ermöglicht, die Zieltemperatur der verbundenen Klimaanlage festzulegen.

1. Navigieren Sie zur Registerkarte **Einstellungen** für die Gerätevorlage **Connected Air Conditioner**.

2. Sie können Einstellungen unterschiedlicher Art erstellen – beispielsweise Zahlen oder Text. Wählen Sie **Zahl** aus, um Ihrem Gerät eine Zahleneinstellung hinzuzufügen.

3. Verwenden Sie zum Konfigurieren der Einstellung **Set Temperature** (Sollwerttemperatur) die Informationen aus der folgenden Tabelle:

    | Feld                | Wert           |
    | -------------------- | -----------     |
    | Anzeigename         | Set Temperature |
    | Feldname           | setTemperature  |
    | Berechnungseinheit      | F               |
    | Dezimalstellen       | 1               |
    | Mindestwert        | 20              |
    | Maximalwert        | 200             |
    | Anfangswert        | 80              |
    | BESCHREIBUNG          | Set the target temperature for the air conditioner |

    Klicken Sie dann auf **Speichern**:

    ![Konfigurieren der Einstellung für die Sollwerttemperatur](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Wenn das Gerät eine Einstellungsänderung bestätigt, ändert sich der Status der Einstellungsänderung in **Synchronisiert**.

4. Sie können die Einstellungskacheln verschieben und ihre Größe ändern, um das Layout der Registerkarte **Einstellungen** anzupassen:

    ![Anpassen des Layouts der Einstellungen](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Verwenden von Eigenschaften

*Anwendungseigenschaften* dienen dazu, Geräteinformationen in der Anwendung zu speichern. In diesem Abschnitt fügen Sie der Gerätevorlage **Connected Air Conditioner** Anwendungseigenschaften hinzu, um den Standort des Geräts und das Datum der letzten Wartung zu speichern. Diese Eigenschaften können in der Anwendung bearbeitet werden. Das Gerät meldet auch Eigenschaften wie Seriennummer und Firmwareversion, die in der Anwendung schreibgeschützt sind.

1. Navigieren Sie zur Registerkarte **Eigenschaften** für die Gerätevorlage **Connected Air Conditioner**.

1. Sie können Geräteeigenschaften unterschiedlicher Art erstellen, wie Zahlen oder Text. Klicken Sie auf **Standort**, um Ihrer Gerätevorlage eine Standorteigenschaften hinzuzufügen. Verwenden Sie zum Konfigurieren der Standorteigenschaft die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                |
    | -------------------- | -------------------- |
    | Anzeigename         | Location             |
    | Feldname           | location             |
    | Anfangswert        | Seattle, WA          |
    | BESCHREIBUNG          | Gerätestandort      |

    Behalten Sie für die anderen Felder die Standardwerte bei.

    ![Konfigurieren der Geräteeigenschaften](./media/tutorial-define-device-type/configureproperties.png)

    Wählen Sie **Speichern** aus.

1. Wählen Sie auf **Datum** aus, um Ihrer Gerätevorlage eine Eigenschaft für das Datum der letzten Wartung hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Eigenschaft für das Datum der letzten Wartung die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Datum der letzten Wartung       |
    | Feldname           | serviceDate             |
    | Anfangswert        | 1\.1.2019                |
    | BESCHREIBUNG          | Datum der letzten Wartung           |

    ![Konfigurieren der Geräteeigenschaften](./media/tutorial-define-device-type/configureproperties2.png)

    Wählen Sie **Speichern** aus.

1. Sie können das Layout der Registerkarte **Eigenschaften** anpassen, indem Sie die Eigenschaftenkacheln verschieben und ihre Größe ändern.

1. Klicken Sie auf **Geräteeigenschaft**, um Ihrer Gerätevorlage eine Geräteeigenschaft wie etwa die Firmwareversion hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Firmwareversion die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Firmware version        |
    | Feldname           | firmwareVersion         |
    | Datentyp            | text                    |
    | BESCHREIBUNG          | Die Firmwareversion der Klimaanlage |

    ![Konfigurieren der Firmwareversion](./media/tutorial-define-device-type/configureproperties3.png)

    Wählen Sie **Speichern** aus.

1. Klicken Sie auf **Geräteeigenschaft**, um Ihrer Gerätevorlage eine Geräteeigenschaft wie etwa eine Seriennummer hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Seriennummer die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Serial number           |
    | Feldname           | serialNumber            |
    | Datentyp            | text                    |
    | BESCHREIBUNG          | Die Seriennummer der Klimaanlage  |

    ![Konfigurieren der Seriennummer](./media/tutorial-define-device-type/configureproperties4.png)

    Wählen Sie **Speichern** aus.

    > [!NOTE]
    > Die Geräteeigenschaft wird vom Gerät an die Anwendung gesendet. Die Werte der Firmwareversion und der Seriennummer werden aktualisiert, wenn Ihr echtes Gerät eine Verbindung mit IoT Central herstellt.

## <a name="use-commands"></a>Verwenden von Befehlen

Verwenden Sie _Befehle_, damit ein Operator Befehle direkt auf dem Gerät ausführen kann. In diesem Abschnitt fügen Sie Ihrer Gerätevorlage **Connected Air Conditioner** einen Befehl hinzu, der es einem Bediener ermöglicht, eine bestimmte Nachricht an der verbundenen Klimaanlage auszugegeben.

1. Navigieren Sie zur Registerkarte **Befehle** für die Gerätevorlage **Connected Air Conditioner**, um die Vorlage zu bearbeiten.

1. Wählen Sie **+ Neuer Befehl** aus, um Ihrem Gerät einen Befehl hinzuzufügen, und konfigurieren Sie Ihren neuen Befehl.

1. Um Ihren neuen Befehl zu konfigurieren, verwenden Sie die Informationen aus der folgenden Tabelle:

    | Feld                | Wert           |
    | -------------------- | -----------     |
    | Anzeigename         | Echo-Befehl    |
    | Feldname           | Echo            |
    | Standardzeitlimit      | 30              |
    | Datentyp         | text            |
    | BESCHREIBUNG          | Gerätebefehl  |  

    Sie können zusätzliche Eingaben für den Befehl hinzufügen, indem Sie unter **Eingabefelder** **+** auswählen.

    ![Vorbereiten des Hinzufügens einer Einstellung](./media/tutorial-define-device-type/commandsecho1.png)

     Wählen Sie **Speichern** aus.

1. Sie können das Layout der Registerkarte **Befehle** anpassen, indem Sie die Befehlskacheln verschieben und ihre Größe ändern.

## <a name="view-your-simulated-device"></a>Anzeigen Ihres simulierten Geräts

Nachdem Sie die Gerätevorlage **Connected Air Conditioner** definiert haben, können Sie das dazugehörige **Dashboard** anpassen, um ihm die definierten Messungen, Einstellungen und Eigenschaften hinzuzufügen. Anschließend können Sie eine Dashboardvorschau als Bediener anzeigen:

1. Wählen Sie die Registerkarte **Dashboard** für die Gerätevorlage **Connected Air Conditioner** aus.

1. Wählen Sie **Liniendiagramm** aus, um die Komponente dem **Dashboard** hinzuzufügen.

1. Konfigurieren Sie die Komponente **Liniendiagramm** mit den Informationen aus der folgenden Tabelle:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Temperatur |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures     | Temperature (wählen Sie neben **Temperature** (Temperatur) **Sichtbarkeit** aus) |

    ![Einstellungen für Liniendiagramm](./media/tutorial-define-device-type/linechartsettings.png)

    Klicken Sie dann auf **Speichern**.

1. Wählen Sie die Komponente **Ereignisverlauf** unter Verwendung der Informationen aus der folgenden Tabelle aus:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Fan Motor Events |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures     | Fan Motor Error (wählen Sie neben **Fan Motor Error** (Lüftermotorfehler) **Sichtbarkeit** aus) |

    ![Einstellungen für Ereignisdiagramm](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Klicken Sie dann auf **Speichern**.

1. Konfigurieren Sie die Komponente **Statusverlauf** mit den Informationen aus der folgenden Tabelle:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Fan Mode |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures | Fan Mode (wählen Sie neben **Fan Mode** (Lüftermodus) **Sichtbarkeit** aus) |

    ![Einstellungen für Liniendiagramm](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Klicken Sie dann auf **Speichern**.

1. Klicken Sie auf **Einstellungen und Eigenschaften**, um dem Dashboard die Geräteeinstellungen und -eigenschaften hinzuzufügen. Wählen Sie **Hinzufügen/Entfernen** aus, um dem Dashboard die gewünschten Einstellungen oder Eigenschaften hinzuzufügen.

1. Konfigurieren Sie die Komponente **Settings and Properties** (Einstellungen und Eigenschaften) mit den Informationen aus der folgenden Tabelle:

    | Einstellung                 | Wert         |
    | ----------------------- | ------------- |
    | Titel                   | Geräteeigenschaften |
    | Settings and Properties (Einstellungen und Eigenschaften) | Set Temperature<br/>Serial number<br/>Firmware version |

    **Einstellungen und Eigenschaften**, die Sie zuvor auf den entsprechenden Seiten definiert haben, werden unter **Verfügbare Spalten** angezeigt.

    ![Eigenschaftseinstellungen für „Set Temperature“](./media/tutorial-define-device-type/propertysettings4.png)

    Klicken Sie dann auf **Speichern**.

1. Nun werden simulierte Daten für Ihre verbundene Klimaanlage auf dem Dashboard angezeigt. Sie können die Kacheln und das Layout für das Dashboard bearbeiten:

    ![Anzeigen des Dashboards](./media/tutorial-define-device-type/dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Erstellen einer neuen Gerätevorlage
> * Hinzufügen von Telemetriedaten zu Ihrem Gerät
> * Anzeigen simulierter Telemetriedaten
> * Definieren von Geräteereignissen
> * Anzeigen simulierter Ereignisse
> * Definieren des Zustands
> * Anzeigen des simulierten Zustands
> * Verwenden von Einstellungen und Eigenschaften
> * Verwenden von Befehlen
> * Anzeigen Ihres simulierten Geräts auf dem Dashboard

Nachdem Sie nun eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung definiert haben, können Sie mit folgenden Schritten fortfahren:

* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md)
* [Anpassen der Azure IoT Central-Ansicht für Bediener](tutorial-customize-operator.md)