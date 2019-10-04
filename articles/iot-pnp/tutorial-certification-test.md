---
title: Zertifizieren Ihres IoT Plug & Play-Geräts (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Ihre Produktinformationen dem Azure Certified for IoT-Gerätekatalog hinzufügen, Ihr Gerät mit dem Azure IoT-Zertifizierungsdienst verbinden und anschließend die IoT Plug & Play-Zertifizierungstests durchführen.
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: tutorial
ms.author: koichih
author: konichi3
ms.date: 06/21/2019
ms.openlocfilehash: 1516a132372a81d06d82de2409c48220f27b8d87
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878731"
---
# <a name="tutorial-certify-your-iot-plug-and-play-preview-device"></a>Tutorial: Zertifizieren Ihres IoT Plug & Play-Geräts (Vorschauversion)

Sie müssen einige Zertifizierungstests durchführen, um ein IoT Plug & Play-Gerät (Vorschauversion) im [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat) zu veröffentlichen. Verwenden Sie das [Azure Certified for IoT-Portal](https://aka.ms/ACFI), um Ihr Gerät zur Zertifizierung einzureichen. Der [Azure IoT-Zertifizierungsdienst](https://aka.ms/azure-iot-aics) führt die Zertifizierungstests durch.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Welche Anforderungen gelten für die IoT Plug & Play-Zertifizierungen?
> * Hinzufügen Ihres Produktnamens und der Informationen zum Portal
> * Verbinden und Ermitteln von IoT Plug & Play-Schnittstellen
> * Überprüfen von IoT Plug & Play-Schnittstellen und Durchführen von Zertifizierungstests
> * Veröffentlichen des zertifizierten IoT Plug & Play-Geräts im Katalog

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Partner Center-Konto
* Microsoft Partner Network-ID

Weitere Informationen finden unter [Onboard to the Azure Certified for IoT portal](howto-onboard-portal.md) (Onboarding für das Azure Certified for IoT-Portal).

## <a name="certification-requirements"></a>Zertifizierungsanforderungen

Für die Zertifizierung Ihres IoT Plug & Play-Geräts muss Ihr Gerät die folgenden Anforderungen erfüllen:

* Ihr IoT Plug & Play-Gerätecode muss auf Ihrem Gerät installiert sein.
* Ihr IoT Plug & Play-Gerätecode muss per Azure IoT SDK erstellt werden.
* Ihr Gerätecode muss den [Azure IoT Hub Device Provisioning-Dienst](../iot-dps/about-iot-dps.md) unterstützen.
* Mit Ihrem Gerätecode muss die [Schnittstelle „Geräteinformationen“](concepts-common-interfaces.md) implementiert werden.
* Das Funktionsmodell und der Gerätecode sind mit IoT Central kompatibel.

Alle Geräte, die derzeit im Katalog enthalten sind, werden als vorab zertifizierte IoT Plug & Play-Geräte betrachtet. Die Qualität und Konformität des Endprodukts von IoT Plug & Play-Softwarekomponenten, z. B. SDK und Digital Twin Definition Language, können nicht garantiert werden.

Alle vorab zertifizierten IoT Plug & Play-Geräte müssen erneut zertifiziert werden, wenn die allgemeine Verfügbarkeit von IoT Plug & Play erreicht ist, und zwar basierend auf der endgültigen Version der Zertifizierungsanforderungen und Softwarekomponenten von Microsoft.

## <a name="add-product-name"></a>Hinzufügen des Produktnamens

**Produkt** ist ein Anzeigename für ein Produktmodell, das ein Kunde erwerben kann. Fügen Sie ein Produkt wie folgt hinzu:

1. Melden Sie sich am [Azure Certified for IoT-Portal](https://aka.ms/ACFI) an.
1. Navigieren Sie über das Menü links zur Seite **Produkte** im Portal, und wählen Sie **+ Neu**.
1. Geben Sie Ihren Anzeigenamen für das Produkt ein, und wählen Sie **Erstellen**. Der hier eingegebene Name unterscheidet sich von dem Namen, der im Gerätekatalog angezeigt wird.

## <a name="add-product-information"></a>Hinzufügen von Produktinformationen

Nachdem Sie das Produkt im Portal erfolgreich erstellt haben, wird es auf der Seite **Produkte** angezeigt. Fügen Sie die Produktinformationen wie folgt hinzu:

1. Wählen Sie auf der Seite **Produkt** in der Produktspalte den erstellten Produktlink. Es sollte sich im Entwurfsstatus befinden.
1. Wählen Sie den Link **Bearbeiten** neben der Überschrift **Produktinformationen**. Geben Sie auf der Seite mit den Produktinformationen die Informationen zu Ihrem Produkt ein, und achten Sie darauf, dass alle erforderlichen Felder ausgefüllt sind.
1. Wählen Sie **Speichern** aus. Die Schaltfläche **Speichern** wird nur angezeigt, wenn Sie alle erforderlichen Felder ausfüllen. Falls die Felder unvollständig sind, wird auf der Schaltfläche der Text **Save and finish later** (Speichern und später fertig stellen) angezeigt.
1. Überprüfen Sie den von Ihnen eingegebenen Inhalt. Füllen Sie alle erforderlichen Felder aus, um das Gerät im Gerätekatalog zu veröffentlichen. Sie können auf der Seite mit den Produktdetails jederzeit Änderungen an den Produktinformationen vornehmen, indem Sie neben der Überschrift **Produktinformationen** auf den Link **Bearbeiten** klicken.

## <a name="connect-and-discover-interfaces"></a>Verbinden und Ermitteln von Schnittstellen

Verbinden Sie Ihr Gerät zum Durchführen der Zertifizierungstests mit dem [Azure IoT-Zertifizierungsdienst](https://aka.ms/azure-iot-aics) (Azure IoT Certification Service, AICS), der im Portal verfügbar ist.

Diese Schritte dienen zur Durchführung von Zertifizierungstests. Es ist nicht erforderlich, dass Sie hierfür Ihren Produktgerätecode ändern. Führen Sie diese Schritte aus, um eine Verbindung mit dem AICS herzustellen:

1. Melden Sie sich mit Ihrem Partner Center-Konto beim Portal an.
1. Klicken Sie auf **Connect + test** (Verbinden und testen), um den Zertifizierungsablauf zu starten.
1. Wählen Sie die [Authentifizierungsmethode](../iot-dps/concepts-security.md#attestation-mechanism) aus, um Ihr Gerät über den [Azure IoT Hub Device Provisioning Service](../iot-dps/about-iot-dps.md) für den AICS bereitzustellen.
   * Laden Sie Ihr generiertes X.509-Zertifikat hoch, wenn Sie ein [X.509-Zertifikat](../iot-hub/iot-hub-security-x509-get-started.md#prerequisites) verwenden. Es kann ratsam sein, sich den Beispielcode anzusehen, mit dem die Nutzung von X.509-Zertifikaten veranschaulicht wird: [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/iothub_ll_client_x509_sample/iothub_ll_client_x509_sample.c), [C#](../iot-hub/iot-hub-security-x509-get-started.md).
   * Wenn Sie einen [symmetrischen Schlüssel](../iot-dps/concepts-symmetric-key-attestation.md) verwenden, müssen Sie ihn kopieren und in Ihren Gerätecode einfügen.
   * Die TPM-Authentifizierungsmethode wird derzeit nicht unterstützt.
1. Kopieren Sie die folgenden generierten IDs, und fügen Sie sie in Ihren Gerätecode ein.
   * [Registrierungs-ID](../iot-dps/use-hsm-with-sdk.md)
   * [DPS-ID](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
   * [DPS-Endpunkt](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
1. Wählen Sie **Verbinden**, wenn Ihr Gerätecode erstellt und auf dem Gerät bereitgestellt wurde, um die IoT Plug & Play-Schnittstellen zu ermitteln.
1. Wählen Sie nach der erfolgreichen Verbindungsherstellung mit dem AICS die Option **Weiter**, um die Ermittlung der IoT Plug & Play-Schnittstellen zu überprüfen.

## <a name="run-tests-and-publish-the-device"></a>Durchführen von Tests und Veröffentlichen des Geräts

Auf der Übersichtsseite können Sie die ermittelten IoT Plug & Play-Schnittstellen überprüfen. Verwenden Sie diese Seite, um zu überprüfen, ob die in der Schnittstelle implementierten Primitive richtig angezeigt werden. Sie können auch den Speicherort der Schnittstelle überprüfen.

1. Stellen Sie sicher, dass die Nutzlasteingaben für die erforderlichen Felder vorgenommen wurden. Diese Felder enthalten Nutzlastinformationen zu den Befehlsprimitiven für die angegebene Schnittstelle.
1. Wählen Sie **Weiter**, wenn Sie alle erforderlichen Informationen eingegeben haben.
1. Wählen Sie **Tests ausführen**, um die Tests für die implementierten IoT Plug & Play-Schnittstellen durchzuführen.
1. Alle Tests werden automatisch ausgeführt. Wählen Sie bei einem nicht erfolgreichen Test die Option **Protokolle anzeigen**, um die AICS-Fehlermeldungen und die unformatierten Telemetriedaten anzuzeigen, die an Azure IoT Hub gesendet werden.
1. Wählen Sie **Fertig stellen**, um die Zertifizierungstests abzuschließen.
1. Veröffentlichen Sie das zertifizierte IoT Plug & Play-Gerät im Katalog. Wählen Sie zum Hinzufügen des zertifizierten Geräts zum Katalog in der Symbolleiste die Option **Add to catalog** (Zu Katalog hinzufügen). Wenn **Add to catalog** (Zu Katalog hinzufügen) abgeblendet ist, sind entweder die Produktinformationen unvollständig, oder bei den Tests ist ein Fehler aufgetreten. 
1. Wählen Sie den Link „CERTIFIED AND IN THE CATALOG“ (ZERTIFIZIERT UND IM KATALOG), um Ihr veröffentlichtes Gerät im Gerätekatalog anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun über die Zertifizierung eines IoT Plug & Play-Geräts informiert haben, ist der vorgeschlagene nächste Schritt die Verwaltung von Funktionsmodellen:

> [!div class="nextstepaction"]
> [Verwalten von Modellen](./howto-manage-models.md)
