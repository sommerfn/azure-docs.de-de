---
title: 'Tutorial: Einrichten einer Cloud für Azure IoT Hub Device Provisioning Service im Portal'
description: 'Tutorial: Einrichten der Cloudressourcen für die Gerätebereitstellung im [Azure-Portal](https://portal.azure.com) per IoT Hub Device Provisioning Service (DPS)'
author: wesmc7777
ms.author: wesmc
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: e91627f934357cd67965ea8880db37ff9bfb3fb2
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112796"
---
# <a name="tutorial-configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Tutorial: Konfigurieren von Cloudressourcen für die Gerätebereitstellung mit dem IoT Hub Device Provisioning-Dienst

In diesem Tutorial wird gezeigt, wie die Cloud für die automatische Gerätebereitstellung mithilfe des IoT Hub Device Provisioning-Diensts eingerichtet wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines IoT Hub Device Provisioning-Diensts und Abrufen des ID-Bereichs mit dem Azure-Portal
> * Erstellen eines IoT Hubs
> * Verknüpfen von IoT Hub mit dem Device Provisioning-Dienst
> * Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Erstellen einer Device Provisioning-Dienstinstanz und Abrufen des ID-Bereichs

Führen Sie zum Erstellen einer neuen Device Provisioning-Dienstinstanz folgende Schritte durch:

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

2. Geben Sie in das Suchfeld **Gerätebereitstellung** ein. 

3. Klicken Sie auf **IoT Hub Device Provisioning-Dienst**.

4. Geben Sie in das Formular **IoT Hub Device Provisioning-Dienst** folgende Informationen ein:
    
   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Name** | Ein beliebiger eindeutiger Name | -- | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](/azure/architecture/best-practices/resource-naming) (Benennungsregeln und Einschränkungen). |
   | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |   

   ![Eingeben grundlegender Informationen zu Ihrem Device Provisioning-Dienst im Portal](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Klicken Sie auf **Create**. Nach einigen Augenblicken wird die Device Provisioning Service-Instanz erstellt und die Seite **Übersicht** angezeigt.

6. Kopieren Sie auf der Seite **Übersicht** für die neue Dienstinstanz den Wert für **ID-Bereich** zur späteren Verwendung. Der Wert dient zum Identifizieren von Registrierungs-IDs und gewährleistet, dass die Registrierungs-ID eindeutig ist.

7. Kopieren Sie außerdem den Wert unter **Dienstendpunkt** zur späteren Verwendung. 

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Abrufen der Verbindungszeichenfolge für den IoT-Hub

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

Sie haben nun Ihren IoT-Hub erstellt und verfügen über den Hostnamen und die IoT Hub-Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Verknüpfen des Device Provisioning-Diensts mit einer IoT Hub-Instanz

Der nächste Schritt besteht darin, den Device Provisioning-Dienst und IoT Hub zu verknüpfen, damit der IoT Hub Device Provisioning-Dienst bei diesem Hub Geräte registrieren kann. Der Dienst kann nur Geräte mit IoT Hubs bereitstellen, die mit dem Device Provisioning-Dienst verknüpft wurden. Führen Sie folgende Schritte durch:

1. Klicken Sie auf der Seite **Alle Ressourcen** auf die Device Provisioning-Dienstinstanz, die Sie zuvor erstellt haben.

2. Klicken Sie auf der Seite „Device Provisioning-Dienst“ auf **Verknüpfte IoT Hubs**.

3. Klicken Sie auf **Hinzufügen**.

4. Geben Sie auf der Seite **Verknüpfung zu IoT Hub hinzufügen** die folgenden Informationen ein, und klicken Sie auf **Speichern**:

    * **Abonnement:** Stellen Sie sicher, dass das Abonnement mit der IoT Hub-Instanz ausgewählt ist. Sie können eine Verknüpfung mit einer IoT Hub-Instanz erstellen, die in einem anderen Abonnement enthalten ist.

    * **IoT Hub**: Wählen Sie den Namen der IoT Hub-Instanz aus, die Sie mit dieser Device Provisioning-Dienst-Instanz verknüpfen möchten.

    * **Zugriffsrichtlinie:** Wählen Sie **iothubowner** als Anmeldeinformationen zum Erstellen der Verknüpfung mit der IoT Hub-Instanz aus.

   ![Verknüpfen des Hubnamens mit der Device Provisioning Service-Instanz im Portal](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Die Zuordnungsrichtlinie ist eine Einstellung des IoT Hub Device Provisioning-Diensts, die festlegt, wie Geräte einer IoT Hub-Instanz zugewiesen werden. Es gibt drei unterstützte Zuordnungsrichtlinien: 

1. **Niedrigste Latenz**: Geräte werden basierend auf dem Hub mit der geringsten Latenz auf dem Gerät für eine IoT Hub-Instanz bereitgestellt.

2. **Gleichmäßig gewichtete Verteilung** (Standard): Bei verknüpften IoT Hubs ist die Wahrscheinlichkeit gleich hoch, dass Geräte für sie bereitgestellt werden. Dies ist die Standardeinstellung. Wenn Sie nur für eine IoT Hub-Instanz Geräte bereitstellen, können Sie diese Einstellung beibehalten. 

3. **Statische Konfiguration per Registrierungsliste**: Die Angabe der gewünschten IoT Hub-Instanz in der Registrierungsliste hat gegenüber der Zuordnungsrichtlinie auf Ebene des Device Provisioning-Diensts Vorrang.

Klicken Sie zum Festlegen der Zuordnungsrichtlinie auf der Seite „Device Provisioning-Dienst“ auf **Zuordnungsrichtlinie verwalten**. Stellen Sie sicher, dass die Zuordnungsrichtlinie auf **Gleichmäßig gewichtete Verteilung** (Standard) festgelegt ist. Wenn Sie Änderungen vornehmen, klicken Sie danach auf **Speichern**.

![Verwalten von Zuordnungsrichtlinien](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Tutorials in dieser Sammlung bauen auf diesem Tutorial auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Tutorial erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte durchführen, um alle in diesem Tutorial erstellten Ressourcen im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub Device Provisioning-Dienstinstanz aus. Klicken Sie im oberen Bereich der Seite **Alle Ressourcen** auf **Löschen**.  

2. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Klicken Sie im oberen Bereich der Seite **Alle Ressourcen** auf **Löschen**.
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines IoT Hub Device Provisioning-Diensts und Abrufen des ID-Bereichs mit dem Azure-Portal
> * Erstellen eines IoT Hubs
> * Verknüpfen von IoT Hub mit dem Device Provisioning-Dienst
> * Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihr Gerät für die Bereitstellung konfigurieren.

> [!div class="nextstepaction"]
> [Einrichten von Geräten für die Bereitstellung](tutorial-set-up-device.md)
