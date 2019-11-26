---
title: 'Tutorial: Bereitstellen eines Geräts per Azure IoT Hub Device Provisioning Service'
description: 'Tutorial: Bereitstellen Ihres Geräts für eine einzelne IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts'
author: wesmc7777
ms.author: wesmc
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: be0b926b6beae2cb339ca232d2b792f50834d801
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112041"
---
# <a name="tutorial-provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Tutorial: Bereitstellen des Geräts für eine IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts

Im vorherigen Tutorial haben Sie erfahren, wie ein Gerät eingerichtet wird, um eine Verbindung mit Ihrem Device Provisioning-Dienst herzustellen. In diesem Tutorial erfahren Sie, wie Sie mithilfe dieses Diensts Ihr Gerät durch automatische Bereitstellung und **_Registrierungslisten_** für eine einzelne IoT Hub-Instanz bereitstellen. Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, konfigurieren Sie auf jeden Fall Ihr Gerät wie in dem Tutorial [Einrichten eines bereitzustellenden Geräts mithilfe des Azure IoT Hub Device Provisioning-Diensts](./tutorial-set-up-device.md) erläutert.

Wenn Sie mit der automatischen Bereitstellung nicht vertraut sind, lesen Sie die Informationen unter [Konzepte für die automatische Bereitstellung](concepts-auto-provisioning.md), bevor Sie fortfahren.

<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Registrieren des Geräts

Bei diesem Schritt werden die einzigartigen Sicherheitsartefakte des Geräts zum Device Provisioning-Dienst hinzugefügt. Diese Sicherheitsartefakte basieren auf dem [Nachweismechanismus](concepts-device.md#attestation-mechanism) des Geräts:

- Für TPM-basierte Geräte benötigen Sie Folgendes:
    - Der *Endorsement Key* ist für jeden TPM-Chip bzw. jede Simulation eindeutig, und Sie erhalten ihn vom Hersteller des TPM-Chips.  Weitere Informationen finden Sie im Artikel [Grundlegendes zum TPM Endorsement Key](https://technet.microsoft.com/library/cc770443.aspx).
    - Die *Registrierungs-ID*, die zur eindeutigen Identifizierung eines Geräts im Namespace/Bereich verwendet wird. Diese ID ist eventuell mit der Geräte-ID identisch. Die ID ist für jedes Gerät erforderlich. Bei TPM-basierten Geräten kann die Registrierungs-ID vom TMP selbst abgeleitet werden, z.B. ein SHA-256-Hash des TPM Endorsement Key.

      [![Registrierungsinformationen für das TPM im Portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png#lightbox)  

- Für X.509-basierte Geräte benötigen Sie Folgendes:
    - Das [für den X.509-Chip oder die X.509-Simulation ausgestellte Zertifikat](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) in Form einer *PEM*- oder *CER*-Datei. Für die individuelle Registrierung müssen Sie für Ihr X.509-System das *Signaturzertifikat* auf Gerätebasis verwenden, für Registrierungsgruppen dagegen das *Stammzertifikat*. 

      [![Hinzufügen einer individuellen Registrierung für den X.509-Nachweis im Portal](./media/tutorial-provision-device-to-hub/individual-enrollment.png)](./media/tutorial-provision-device-to-hub/individual-enrollment.png#lightbox)

Es gibt zwei Möglichkeiten zum Registrieren des Geräts beim Device Provisioning-Dienst:

- **Registrierungsgruppen** Diese stellen Gruppen von Geräten dar, die einen bestimmten Nachweismechanismus gemeinsam nutzen. Es wird empfohlen, eine Registrierungsgruppe für eine große Anzahl von Geräten, die eine gewünschte Erstkonfiguration gemeinsam nutzen, oder für Geräte zu verwenden, die alle demselben Mandanten zugeordnet sind. Weitere Informationen zum Identitätsnachweis für Registrierungsgruppen finden Sie unter [Sicherheit](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

    [![Hinzufügen einer Gruppenregistrierung für den X.509-Nachweis im Portal](./media/tutorial-provision-device-to-hub/group-enrollment.png)](./media/tutorial-provision-device-to-hub/group-enrollment.png#lightbox)

- **Individuelle Registrierungen** Diese stellen Einträge für ein einzelnes Gerät dar, das beim Device Provisioning-Dienst registriert werden kann. Individuelle Registrierungen verwenden entweder X.509-Zertifikate oder SAS-Token (in einem physischen oder virtuellen TPM) als Nachweismechanismen. Individuelle Registrierungen sollten für Geräte, die besondere Erstkonfigurationen erfordern, und für Geräte verwendet werden, die nur SAS-Token über das TPM oder das virtuelle TPM als Nachweismechanismus verwenden können. Bei individuellen Registrierungen ist möglicherweise die gewünschte IoT Hub-Geräte-ID angegeben.

Nun registrieren Sie das Gerät mithilfe der erforderlichen Sicherheitsartefakte basierend auf dem Nachweismechanismus des Geräts bei der Device Provisioning-Dienstinstanz: 

1. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Device Provisioning-Dienst.

2. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Registrierungen verwalten** aus. Wählen Sie je nach Ihrer Geräteinstallation entweder die Registerkarte **Individuelle Registrierungen** oder **Registrierungsgruppen**. Klicken Sie oben auf die Schaltfläche **Hinzufügen**. Wählen Sie **TPM** oder **X.509** als *Mechanismus* zum Identitätsnachweis aus, und geben Sie wie zuvor erläutert die entsprechenden Sicherheitsartefakte ein. Sie können eine neue **IoT Hub-Geräte-ID** eingeben. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

3. Wenn das Gerät erfolgreich registriert ist, sollte es wie folgt im Portal angezeigt werden:

    ![Erfolgreiche TPM-Registrierung im Portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)

Nach der Registrierung wartet der Bereitstellungsdienst, bis das Gerät gestartet und zu einem beliebigen Zeitpunkt eine Verbindung mit diesem hergestellt wird. Wenn Ihr Gerät zum ersten Mal gestartet wird, interagiert die Client-SDK-Bibliothek zur Extraktion der Sicherheitsartefakte vom Gerät mit Ihrem Chip und überprüft die Registrierung bei Ihrem Device Provisioning-Dienst. 

## <a name="start-the-iot-device"></a>Starten des IoT-Geräts

Bei Ihrem IoT-Gerät kann es sich um ein echtes Gerät oder ein simuliertes Gerät handeln. Da das IoT-Gerät jetzt bei einer Device Provisioning Service-Instanz registriert ist, kann es nun gestartet werden und zur Erkennung mithilfe des Nachweismechanismus den Bereitstellungsdienst aufrufen. Nach der Erkennung des Geräts durch den Bereitstellungsdienst wird es einem IoT-Hub hinzugefügt. 

Beispiele für simulierte Geräte, die sowohl TPM- als auch X.509-Nachweise nutzen, stehen für C, Java, C#, Node.js und Python zur Verfügung. Für ein simuliertes Gerät, das TPM und das [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) verwendet, wird beispielsweise der im Abschnitt [Simulieren der ersten Startsequenz für das Gerät](quick-create-simulated-device.md#simulate-first-boot-sequence-for-the-device) erläuterte Prozess ausgeführt. Das gleiche Gerät bezieht sich bei Verwendung des X.509-Zertifikats auf diesen Abschnitt zur [Startsequenz](quick-create-simulated-device-x509.md#simulate-first-boot-sequence-for-the-device).

Ein Beispiel für ein echtes Gerät finden Sie unter [Verwenden der automatischen Bereitstellung des Azure IoT Hub Device Provisioning Service zum Registrieren des MXChip IoT DevKit bei IoT Hub](how-to-connect-mxchip-iot-devkit.md).

Starten Sie das Gerät, damit die Clientanwendung Ihres Geräts mit der Registrierung bei Ihrer Device Provisioning Service-Instanz beginnen kann.  

## <a name="verify-the-device-is-registered"></a>Sicherstellen, dass das Gerät registriert ist

Nach dem Starten Ihres Geräts sollten die folgenden Aktionen ausgeführt werden:

1. Das Gerät sendet eine Registrierungsanforderung an Ihren Device Provisioning-Dienst.
2. Bei TPM-Geräten sendet der Device Provisioning-Dienst eine Registrierungsherausforderung zurück, auf die Ihr Gerät antwortet. 
3. Nach erfolgreicher Registrierung sendet der Device Provisioning-Dienst den IoT Hub-URI, die Geräte-ID und den verschlüsselten Schlüssel an das Gerät zurück. 
4. Die IoT Hub-Clientanwendung auf dem Gerät stellt dann eine Verbindung mit Ihrem Hub her. 
5. Nachdem eine Verbindung mit dem Hub hergestellt wurde, sollte das Gerät im IoT Hub-Explorer für **IoT-Geräte** angezeigt werden. 

    ![Erfolgreiche Herstellung einer Verbindung mit dem Hub im Portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

Weitere Informationen finden Sie unter dem Beispiel zur Bereitstellung eines Geräteclients: [prov_dev_client_sample.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c). In diesem Beispiel wird die Bereitstellung eines simulierten Geräts mit TPM, X.509-Zertifikaten und symmetrischen Schlüsseln veranschaulicht. Schritt-für-Schritt-Anleitungen zur Verwenden des Beispiels finden Sie in den Schnellstartanleitungen zu [TPM](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device), zu [X.509](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) und zu [symmetrischen Schlüsseln](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-symm-key).

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie mehrere Geräte für Hubs mit Lastausgleich bereitgestellt werden. 

> [!div class="nextstepaction"]
> [Bereitstellen von Geräten für IoT Hubs mit Lastausgleich](./tutorial-provision-multiple-hubs.md)
