---
title: Hinzufügen eines Azure IoT Edge-Geräts zu Azure IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ihrer Azure IoT Central-Anwendung als Bediener ein Azure IoT Edge-Gerät hinzufügen.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: ae80a624ed1f85a1f59fea79b152a4bc31067ad1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892636"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application-preview-features"></a>Tutorial: Hinzufügen eines Azure IoT Edge-Geräts zu Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial erfahren Sie, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein *Azure IoT Edge-Gerät* hinzufügen und das Gerät konfigurieren. In diesem Tutorial wird ein virtueller, Azure IoT Edge-fähiger Linux-Computer aus Azure Marketplace verwendet.

Das Tutorial besteht aus zwei Teilen:

* Im ersten Teil erfahren Sie, wie Sie als Bediener eine Azure IoT Edge-Gerätebereitstellung mit Cloudschwerpunkt durchführen.
* Anschließend wird erläutert, wie Sie eine Azure IoT Edge-Gerätebereitstellung mit Geräteschwerpunkt durchführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen Azure IoT Edge-Geräts
> * Konfigurieren des Azure IoT Edge-Geräts zur Unterstützung der Bereitstellung unter Verwendung eines SAS-Schlüssels
> * Anzeigen von Dashboards und der Modulintegrität in IoT Central
> * Senden von Befehlen an ein Modul, das auf dem Azure IoT Edge-Gerät ausgeführt wird
> * Festlegen von Eigenschaften für ein Modul, das auf dem Azure IoT Edge-Gerät ausgeführt wird

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Gehen Sie wie in [dieser Schnellstartanleitung](./quick-deploy-iot-central.md) beschrieben vor, um eine Azure IoT Central-Anwendung zu erstellen.

## <a name="enable-azure-iot-edge-enrollment-group"></a>Aktivieren der Azure IoT Edge-Registrierungsgruppe
Aktivieren Sie auf der Verwaltungsseite SAS-Schlüssel für die Azure IoT Edge-Registrierungsgruppe.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/groupenrollment.png)

## <a name="cloud-first-azure-iot-edge-device-provisioning"></a>Azure IoT Edge-Gerätebereitstellung mit Cloudschwerpunkt   
In diesem Abschnitt erstellen Sie mithilfe der Umgebungssensorvorlage (**Environment Sensor Template**) ein neues Azure IoT Edge-Gerät und stellen ein Gerät bereit. Klicken Sie im linken Navigationsbereich auf „Geräte“ und anschließend auf „Environment Sensor Template“. 

![Gerätevorlage: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/deviceexplorer.png)

Klicken Sie auf **+ Neu**, und geben Sie eine Geräte-ID und einen Namen ein. 

![Gerätevorlage: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/cfdevicecredentials.png)

Das Gerät wird in den Modus **Registriert** versetzt.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/cfregistered.png)

## <a name="deploy-an-azure-iot-edge-enabled-linux-vm"></a>Bereitstellen eines virtuellen, Azure IoT Edge-fähigen Linux-Computers

>Hinweis: Sie können einen beliebigen Computer bzw. ein beliebiges Gerät verwenden. Betriebssystem: Linux oder Windows.

In diesem Tutorial wird ein virtueller, Azure IoT-fähiger Linux-Computer verwendet, der in Azure erstellt werden kann. Sie werden zum [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) weitergeleitet. Klicken Sie dort auf die Schaltfläche **JETZT HOLEN**. 

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfmarketplace.png)

Klicken Sie auf **Weiter**.

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfmarketplacecontinue.png)


Sie werden zum Azure-Portal weitergeleitet. Klicken Sie auf die Schaltfläche **Erstellen**.

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfubuntu.png)

Wählen Sie ein Abonnement aus, erstellen Sie nach Möglichkeit eine neue Ressourcengruppe, wählen Sie für die VM-Verfügbarkeit die Option „USA, Westen 2“ aus, und geben Sie Benutzer und Kennwort ein. Nicht vergessen: Benutzer und Kennwort werden in späteren Schritten benötigt. Klicken Sie auf **Überprüfen + erstellen**.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfvm.png)

Klicken Sie nach der Validierung auf **Erstellen**.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfvmvalidated.png)

Die Erstellung der Ressourcen dauert ein paar Minuten. Klicken Sie auf **Zu Ressource wechseln**.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfvmdeploymentcomplete.png)

### <a name="provision-vm-as-azure-iot-edge-device"></a>Bereitstellen eines virtuellen Computers als Azure IoT Edge-Gerät 

Klicken Sie im linken Navigationsbereich unter „Support + Problembehandlung“ auf „Serielle Konsole“.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfserialconsole.png)

Daraufhin wird ein Bildschirm wie der folgende angezeigt:

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsole.png)

Drücken Sie die EINGABETASTE, geben Sie den Benutzernamen und das Kennwort ein, wenn Sie dazu aufgefordert werden, und drücken Sie erneut die EINGABETASTE. 

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsolelogin.png)

Führen Sie den folgenden Befehl aus, um einen Befehl als Administrator/Root-Benutzer auszuführen: **sudo su –**

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfsudo.png)

Überprüfen Sie die Version der Azure IoT Edge-Runtime. Die aktuelle allgemein verfügbare Version ist 1.0.8.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsoleversion.png)

Installieren Sie den vim-Editor, oder verwenden Sie nano, falls Ihnen das lieber ist. 

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsolevim.png)

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfvim.png)

Bearbeiten Sie die Azure IoT Edge-Datei „config.yaml“.

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsoleconfig.png)

Scrollen Sie in der YAML-Datei nach unten, und kommentieren Sie den Teil mit der Verbindungszeichenfolge aus. 

**Vorher**

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioning.png)

**Nachher** (Drücken Sie ESC, und geben Sie ein kleines „a“ ein, um mit der Bearbeitung zu beginnen.)

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioningcomments.png)

Kommentieren Sie in der YAML-Datei den Teil mit dem symmetrischen Schlüssel aus. 

**Vorher**

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmcomments.png)

**Nachher**

![Virtueller Ubuntu-Computer](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmuncomments.png)

Navigieren Sie zu IoT Central, und rufen Sie die Bereichs-ID, die Geräte-ID und den symmetrischen Schlüssel des Azure IoT Edge-Geräts ab. ![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/cfdeviceconnect.png)

Navigieren Sie zum Linux-Feld, und ersetzen Sie die Bereichs-ID und die Registrierungs-ID durch die Geräte-ID und den symmetrischen Schlüssel.

Drücken Sie **ESC**, und geben Sie **:wq!** ein. Drücken Sie anschließend die **EINGABETASTE**, um Ihre Änderungen zu speichern.

Starten Sie Azure IoT Edge neu, um Ihre Änderungen zu verarbeiten, und drücken Sie anschließend die **EINGABETASTE**.

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/cfrestart.png)

Geben Sie **iotedge list** ein. Nach ein paar Minuten werden drei bereitgestellte Module angezeigt.

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/cfconsolemodulelist.png)


## <a name="iot-central-device-explorer"></a>Geräte-Explorer in IoT Central 

In IoT Central wird Ihr Gerät in den Zustand „Bereitgestellt“ versetzt.

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/cfprovisioned.png)

Auf der Registerkarte „Module“ wird der Status des Geräts und des Moduls in IoT Central angezeigt. 

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/cfiotcmodulestatus.png)


Cloudeigenschaften werden in einem Formular angezeigt (auf der Grundlage der Gerätevorlage, die Sie in den vorherigen Schritten erstellt haben). Geben Sie Werte ein, und klicken Sie auf **Speichern**. 

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/deviceinfo.png)

Dashboardkachel

![Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/dashboard.png)

In diesem Tutorial haben Sie Folgendes gelernt:

* Hinzufügen eines neuen Azure IoT Edge-Geräts
* Konfigurieren des Azure IoT Edge-Geräts zur Unterstützung der Bereitstellung unter Verwendung eines SAS-Schlüssels
* Anzeigen von Dashboards und der Modulintegrität in IoT Central
* Senden von Befehlen an ein Modul, das auf dem Azure IoT Edge-Gerät ausgeführt wird
* Festlegen von Eigenschaften für ein Modul, das auf dem Azure IoT Edge-Gerät ausgeführt wird

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie mit Azure IoT Edge-Geräten in IoT Central arbeiten, können Sie sich im nächsten Tutorial

<!-- Next how-tos in the sequence -->

mit der Konfiguration eines transparenten Gateways vertraut machen:

> [!div class="nextstepaction"]
> [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](../../iot-edge/how-to-create-transparent-gateway.md)
