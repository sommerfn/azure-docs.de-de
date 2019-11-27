---
title: 'Schnellstart: Registrieren eines TPM-Geräts für den Azure Device Provisioning-Dienst per Python'
description: 'Azure-Schnellstart: Registrieren eines TPM-Geräts in Azure IoT Hub Device Provisioning Service mit dem Python Provisioning Service SDK In dieser Schnellstartanleitung werden individuelle Registrierungen verwendet.'
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.devlang: python
ms.custom: mvc
ms.openlocfilehash: 49e7961200a2ab523997350fffa6606d65f60c8c
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903398"
---
# <a name="quickstart-enroll-tpm-device-to-iot-hub-device-provisioning-service-using-python-provisioning-service-sdk"></a>Schnellstart: Registrieren eines TPM-Geräts für den IoT Hub Device Provisioning-Dienst per Python Provisioning-Dienst-SDK
[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]

Anhand der Schritte in diesem Artikel wird veranschaulicht, wie Sie programmgesteuert eine individuelle Registrierung für ein TPM-Gerät im Azure IoT Hub Device Provisioning-Dienst erstellen, indem Sie das [Python Provisioning-Dienst-SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client) und eine Python-Beispielanwendung verwenden. Das Python-Dienst-SDK funktioniert auf Windows- und Linux-Computern, aber in diesem Artikel wird nur ein Windows-Entwicklungscomputer verwendet, um die einzelnen Schritte des Registrierungsprozesses zu veranschaulichen.

Führen Sie vor dem Fortfahren zunächst das [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) durch.


<a id="prepareenvironment"></a>

## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung 

1. Laden Sie [Python 2.x oder 3.x](https://www.python.org/downloads/) herunter, und installieren Sie es. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihren plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. 

1. Wählen Sie eine der folgenden Optionen:

    - Erstellen und kompilieren Sie das **Azure IoT Python SDK**. Eine Anleitung für die Erstellung der Python-Pakete finden Sie [hier](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md). Installieren Sie bei Verwendung des Windows-Betriebssystems auch das [Visual C++ Redistributable Package](https://www.microsoft.com/download/confirmation.aspx?id=48145), um die Verwendung nativer DLLs aus Python zu ermöglichen.

    - [Installieren oder aktualisieren Sie *pip*, das Python-Paketverwaltungssystem](https://pip.pypa.io/en/stable/installing/), und installieren Sie das Paket mit dem folgenden Befehl:

        ```cmd/sh
        pip install azure-iothub-provisioningserviceclient
        ```

1. Sie benötigen den Endorsement Key für Ihr Gerät. Falls Sie die Schritte der Schnellstartanleitung unter [Erstellen und Bereitstellen eines simulierten Geräts mithilfe von IoT Hub Device Provisioning-Diensten](quick-create-simulated-device.md) befolgt haben, um ein simuliertes TPM-Gerät zu erstellen, können Sie den für dieses Gerät erstellten Schlüssel verwenden. Andernfalls können Sie den folgenden Endorsement Key verwenden, der mit dem SDK bereitgestellt wird:

    ```
    AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAtW6MOyCu/Nih47atIIoZtlYkhLeCTiSrtRN3q6hqgOllA979No4BOcDWF90OyzJvjQknMfXS/Dx/IJIBnORgCg1YX/j4EEtO7Ase29Xd63HjvG8M94+u2XINu79rkTxeueqW7gPeRZQPnl1xYmqawYcyzJS6GKWKdoIdS+UWu6bJr58V3xwvOQI4NibXKD7htvz07jLItWTFhsWnTdZbJ7PnmfCa2vbRH/9pZIow+CcAL9mNTNNN4FdzYwapNVO+6SY/W4XU0Q+dLMCKYarqVNH5GzAWDfKT8nKzg69yQejJM8oeUWag/8odWOfbszA+iFjw3wVNrA5n8grUieRkPQ==
    ```


## <a name="modify-the-python-sample-code"></a>Ändern des Python-Beispielcodes

In diesem Abschnitt wird veranschaulicht, wie Sie die Bereitstellungsdetails Ihres TPM-Geräts dem Beispielcode hinzufügen. 

1. Erstellen Sie mit einem Text-Editor die neue Datei **TpmEnrollment.py**.

1. Fügen Sie am Anfang der Datei **TpmEnrollment.py** die folgenden `import`-Anweisungen und Variablen hinzu. Ersetzen Sie anschließend `dpsConnectionString` durch Ihre Verbindungszeichenfolge, die sich im **Azure-Portal** unter **Device Provisioning Service** unter **Freigegebene Zugriffsrichtlinien** befindet. Ersetzen Sie `endorsementKey` durch den Wert, den Sie zuvor unter [Vorbereiten der Umgebung](quick-enroll-device-tpm-python.md#prepareenvironment) notiert haben. Erstellen Sie abschließend eine eindeutige `registrationid`, und achten Sie darauf, dass sie nur Kleinbuchstaben, Zahlen und Bindestriche enthält.  
   
    ```python
    from provisioningserviceclient import ProvisioningServiceClient
    from provisioningserviceclient.models import IndividualEnrollment, AttestationMechanism

    CONNECTION_STRING = "{dpsConnectionString}"

    ENDORSEMENT_KEY = "{endorsementKey}"

    REGISTRATION_ID = "{registrationid}"
    ```

1. Fügen Sie die folgende Funktion und den Funktionsaufruf hinzu, um die Erstellung der Gruppenregistrierung zu implementieren:
   
    ```python
    def main():
        print ( "Starting individual enrollment..." )

        psc = ProvisioningServiceClient.create_from_connection_string(CONNECTION_STRING)

        att = AttestationMechanism.create_with_tpm(ENDORSEMENT_KEY)
        ie = IndividualEnrollment.create(REGISTRATION_ID, att)

        ie = psc.create_or_update(ie)
    
        print ( "Individual enrollment successful." )
    
    if __name__ == '__main__':
        main()
    ```

1. Speichern und schließen Sie die Datei **TpmEnrollment.py**.
 

## <a name="run-the-sample-tpm-enrollment"></a>Ausführen der TPM-Beispielregistrierung

1. Öffnen Sie eine Eingabeaufforderung, und führen Sie das Skript aus.

    ```cmd/sh
    python TpmEnrollment.py
    ```

1. Überprüfen Sie die Ausgabe darauf, ob die Registrierung erfolgreich ist.

1. Navigieren Sie im Azure-Portal zu Ihrem Provisioning-Dienst. Klicken Sie auf **Registrierungen verwalten**. Beachten Sie, dass Ihr TPM-Gerät auf der Registerkarte **Individuelle Registrierungen** unter dem zuvor erstellten Namen `registrationid` aufgeführt ist. 

    ![Überprüfen der erfolgreichen TPM-Registrierung im Portal](./media/quick-enroll-device-tpm-python/1.png)  


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie planen, sich das Beispiel des Java-Diensts näher anzusehen, sollten Sie die in dieser Schnellstartanleitung erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle erstellten Ressourcen zu löschen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Schließen Sie das Ausgabefenster des Python-Beispiels auf Ihrem Computer.
1. Falls Sie ein simuliertes TPM-Gerät erstellt haben, können Sie das TPM-Simulatorfenster schließen.
1. Navigieren Sie im Azure-Portal zu Ihrem Device Provisioning-Dienst, klicken Sie auf **Registrierungen verwalten**, und klicken Sie anschließend auf die Registerkarte **Individuelle Registrierungen**. Wählen Sie die *Registrierungs-ID* für den Registrierungseintrag aus, den Sie mit dieser Schnellstartanleitung erstellt haben, und klicken Sie oben auf dem Blatt auf die Schaltfläche **Löschen**.  


## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie programmgesteuert einen Eintrag für die individuelle Registrierung für ein TPM-Gerät erstellt und optional ein simuliertes TPM-Gerät auf Ihrem Computer erstellt und für Ihren IoT Hub bereitgestellt, indem Sie den Azure IoT Hub Device Provisioning-Dienst verwendet haben. Ausführlichere Informationen zur Gerätebereitstellung finden Sie im Tutorial zur Einrichtung des Device Provisioning-Diensts über das Azure-Portal.

> [!div class="nextstepaction"]
> [Tutorials für den Azure IoT Hub Device Provisioning-Dienst](./tutorial-set-up-cloud.md)
