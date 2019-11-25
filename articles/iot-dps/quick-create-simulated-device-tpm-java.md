---
title: 'Schnellstart: Bereitstellen eines simulierten TPM-Geräts für Azure IoT Hub mithilfe von Java'
description: 'Azure-Schnellstartanleitung: Erstellen und Bereitstellen eines simulierten TPM-Geräts mithilfe des Java-Geräte-SDK für Azure IoT Hub Device Provisioning Service In dieser Schnellstartanleitung werden individuelle Registrierungen verwendet.'
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: d8dfb9bb74e1159c9e37f60595a74ae0d0d8f2f6
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904875"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-java-device-sdk-for-azure-iot-hub-device-provisioning-service"></a>Schnellstart: Erstellen und Bereitstellen eines simulierten TPM-Geräts mithilfe des Java-Geräte-SDKs für den Azure IoT Hub Device Provisioning-Dienst

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Die folgenden Schritte zeigen, wie Sie auf Ihrem Entwicklungscomputer unter dem Windows-Betriebssystem ein simuliertes Gerät erstellen, den Windows-TPM-Simulator als [Hardwaresicherheitsmodul (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) des Geräts ausführen und dieses simulierte Gerät mithilfe des Codebeispiels mit dem Device Provisioning-Dienst und Ihrer IoT Hub-Instanz verbinden. 

Sollten Sie mit der automatischen Bereitstellung nicht vertraut sein, lesen Sie auch die Informationen unter [Konzepte für die automatische Bereitstellung](concepts-auto-provisioning.md). Vergewissern Sie sich außerdem, dass Sie die Schritte unter [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) ausgeführt haben, bevor Sie fortfahren. 

In Azure IoT Device Provisioning Service werden zwei Registrierungsarten unterstützt:
- [Registrierungsgruppen:](concepts-service.md#enrollment-group) Werden zum Registrieren mehrerer verwandter Geräte verwendet.
- [Individuelle Registrierungen](concepts-service.md#individual-enrollment): Werden zum Registrieren eines einzelnen Geräts verwendet.

In diesem Artikel werden individuelle Registrierungen veranschaulicht.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung 

1. Vergewissern Sie sich, dass das [Java SE Development Kit 8](https://aka.ms/azure-jdks) auf Ihrem Computer installiert ist.

1. Laden Sie [Maven](https://maven.apache.org/install.html) herunter, und installieren Sie es.

1. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Unter [Git-Clienttools von Software Freedom Conservancy](https://git-scm.com/download/) finden Sie die neueste Version der zu installierenden `git`-Tools. Hierzu zählt auch die Befehlszeilen-App **Git Bash**, über die Sie mit Ihrem lokalen Git-Repository interagieren können. 

1. Öffnen Sie eine Eingabeaufforderung. Klonen Sie das GitHub-Repository mit dem Codebeispiel für die Gerätesimulation.
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Führen Sie den [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)-Simulator aus. Klicken Sie auf **Zugriff zulassen**, um Änderungen an den Einstellungen der _Windows-Firewall_ zuzulassen. Dieser lauscht über einen Socket an den Ports 2321 und 2322. Lassen Sie dieses Fenster geöffnet, damit der Simulator bis zum Ende dieser Schnellstartanleitung aktiv bleibt. 

    ```cmd/sh
    .\azure-iot-sdk-java\provisioning\provisioning-tools\tpm-simulator\Simulator.exe
    ```

    ![TPM-Simulator](./media/java-quick-create-simulated-device/simulator.png)

1. Navigieren Sie an einer separaten Eingabeaufforderung zum Stammordner, und erstellen Sie die Beispielabhängigkeiten.

    ```cmd/sh
    cd azure-iot-sdk-java
    mvn install -DskipTests=true
    ```

1. Navigieren Sie zum Beispielordner.

    ```cmd/sh
    cd provisioning/provisioning-samples/provisioning-tpm-sample
    ```

1. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Device Provisioning-Dienst. Notieren Sie sich den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_.

    ![Information zu Device Provisioning Service](./media/java-quick-create-simulated-device/extract-dps-endpoints.png)

1. Bearbeiten Sie `src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningTpmSample.java`, um die zuvor notierten Werte für den _ID-Bereich_ und den _globalen Endpunkt für den Bereitstellungsdienst_ einzufügen.  

    ```java
    private static final String idScope = "[Your ID scope here]";
    private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
    private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
    ```

1. Erstellen Sie das Projekt. Navigieren Sie zum Zielordner, und führen Sie die erstellte JAR-Datei aus.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-tpm-sample-{version}-with-deps.jar
    ```

1. Die Programmausführung beginnt. Notieren Sie sich den _Endorsement Key_ und die _Registrierungs-ID_ für den nächsten Abschnitt, und lassen Sie das Programm weiter laufen.

    ![Java-TPM-Geräteprogramm](./media/java-quick-create-simulated-device/program.png)
    

## <a name="create-a-device-enrollment-entry"></a>Erstellen eines Geräteregistrierungseintrags

1. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Device Provisioning-Dienst.

1. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Registrierungen verwalten** aus. Wählen Sie die Registerkarte **Individuelle Registrierungen** aus, und klicken Sie oben auf die Schaltfläche **Individuelle Registrierung hinzufügen**. 

1. Geben Sie im Bereich **Registrierung hinzufügen** die folgenden Informationen ein:
   - Wählen Sie **TPM** als *Mechanismus* für den Nachweis der Identität.
   - Geben Sie die zuvor notierte *Registrierungs-ID* und den *Endorsement Key* für Ihr TPM-Gerät ein. 
   - Wählen Sie einen IoT Hub aus, der mit Ihrem Bereitstellungsdienst verknüpft ist.
   - Geben Sie eine eindeutige Geräte-ID ein. Achten Sie darauf, dass Sie beim Benennen Ihres Geräts keine sensiblen Daten angeben.
   - Aktualisieren Sie **Initial device twin state** (Anfänglicher Gerätezwillingsstatus) mit der gewünschten Anfangskonfiguration für das Gerät.
   - Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

     ![Eingeben von Geräteregistrierungsinformationen auf dem Portalblatt](./media/java-quick-create-simulated-device/enterdevice-enrollment.png)  

   Nach erfolgreicher Registrierung wird die *Registrierungs-ID* Ihres Geräts in der Liste auf der Registerkarte *Individuelle Registrierungen* angezeigt. 


## <a name="simulate-the-device"></a>Simulieren des Geräts

1. Drücken Sie im Befehlsfenster, in dem der Java-Beispielcode auf Ihrem Computer ausgeführt wird, die EINGABETASTE, um die Ausführung der Anwendung fortzusetzen. Beachten Sie die Nachrichten, die den Start und die Verbindungsherstellung des Geräts mit dem Device Provisioning-Dienst simulieren, um Ihre IoT Hub-Informationen abzurufen.  

    ![Fertiges Java-TPM-Geräteprogramm](./media/java-quick-create-simulated-device/program-final.png)

1. Nachdem das simulierte Gerät erfolgreich für die mit Ihrem Bereitstellungsdienst verknüpfte IoT Hub-Instanz bereitgestellt wurde, wird die Geräte-ID auf dem Blatt **Device Explorer** des Hubs angezeigt.

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/java-quick-create-simulated-device/hubregistration.png) 

    Wenn Sie den *anfänglichen Gerätezwillingsstatus* im Registrierungseintrag für Ihr Gerät gegenüber dem Standardwert geändert haben, kann der gewünschte Zwillingsstatus vom Hub abgerufen werden, und es können entsprechende Aktionen durchgeführt werden. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Geräteclientbeispiel weiter verwenden und erkunden möchten, überspringen Sie die Bereinigung der in dieser Schnellstartanleitung erstellten Ressourcen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle erstellten Ressourcen zu löschen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Schließen Sie auf Ihrem Computer das Ausgabefenster des Geräteclientbeispiels.
1. Schließen Sie auf Ihrem Computer das TPM-Simulatorfenster.
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihren Device Provisioning-Dienst aus. Öffnen Sie das Blatt **Registrierungen verwalten** für Ihren Dienst, und klicken Sie dann auf die Registerkarte **Individuelle Registrierungen**. Wählen Sie die *REGISTRIERUNGS-ID* des Geräts aus, das Sie mit dieser Schnellstartanleitung registriert haben, und klicken Sie im oberen Bereich auf die Schaltfläche **Löschen**. 
1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Öffnen Sie das Blatt **IoT-Geräte** für Ihren Hub, wählen Sie die *GERÄTE-ID* des Geräts aus, die Sie in dieser Schnellstartanleitung registriert haben, und klicken Sie dann im oberen Bereich auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie auf Ihrem Computer ein simuliertes TPM-Gerät erstellt und es mithilfe des IoT Hub Device Provisioning-Diensts für Ihre IoT Hub-Instanz bereitgestellt. Informationen zum programmgesteuerten Registrieren Ihres TPM-Geräts finden Sie in der Schnellstartanleitung für die programmgesteuerte Registrierung von TPM-Geräten. 

> [!div class="nextstepaction"]
> [Registrieren eines TPM-Geräts für den IoT Hub Device Provisioning-Dienst per Java-Dienst-SDK](quick-enroll-device-tpm-java.md)
