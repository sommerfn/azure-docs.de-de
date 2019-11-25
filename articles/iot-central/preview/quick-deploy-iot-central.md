---
title: Erstellen einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erstellen Sie eine neue Azure IoT Central-Anwendung. Erstellen Sie unter Verwendung einer Anwendungsvorlage eine Testversion oder eine Anwendung mit nutzungsbasierter Zahlung.
author: lmasieri
ms.author: lmasieri
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: c639cb059d773042b7f45160dea18bfc2130cae9
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896278"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Schnellstart erfahren Sie, wie Sie eine Azure IoT Central-Anwendung erstellen, die Previewfunktionen enthält, etwa IoT Plug & Play.

> [!WARNING]
> Die IoT Plug & Play-Funktionen in Azure IoT Central befinden sich derzeit in der Vorschauphase. Verwenden Sie keine IoT Central-Anwendung mit aktivierter IoT Plug & Play-Funktion für Produktionsworkloads. Verwenden Sie für Produktionsumgebungen eine IoT Central-Anwendung, die auf der Grundlage einer aktuellen, allgemein verfügbaren Anwendungsvorlage erstellt wurde.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Navigieren Sie zur Buildwebsite für [Azure IoT Central](https://aka.ms/iotcentral). Melden Sie sich dann mit einem persönlichen Microsoft-Konto oder mit einem Geschäfts-, Schul- oder Unikonto an.

Sie können eine neue Anwendung entweder anhand der Liste mit den branchenrelevanten IoT Central-Vorlagen erstellen, um schnell einzusteigen, oder Sie starten von Grund auf neu, indem Sie eine Vorlage für eine **benutzerdefinierte App** verwenden.

![Azure IoT Central-Seite „Anwendung erstellen“](media/quick-deploy-iot-central/iotcentralcreate-templates-pnp.png)

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Um eine neue Azure IoT Central-Anwendung anhand einer *Branchenvorlage* zu erstellen, wählen Sie eine Anwendungsvorlage aus der Liste der verfügbaren Vorlagen unter einer der Branchen aus. Sie können auch von Grund auf neu starten, indem Sie *Benutzerdefinierte App* auswählen.
1. Azure IoT Central schlägt automatisch einen **Anwendungsnamen** entsprechend der von Ihnen ausgewählten Anwendungsvorlage vor. Sie können diesen Namen übernehmen oder einen eigenen aussagekräftigen Namen eingeben.
1. Azure IoT Central generiert außerdem für Sie entsprechend dem Anwendungsnamen ein Präfix mit einer eindeutigen **Anwendungs-URL**. Über diese URL können Sie auf Ihre Anwendung zugreifen. Sie können dieses URL-Präfix in einen einprägsameren Wert ändern.

    ![Azure IoT Central-Seite „Anwendung erstellen“](media/quick-deploy-iot-central/iotcentralcreate-industry-pnp.png)

    > [!NOTE]
    > Wenn Sie die Vorlage für eine benutzerdefinierte App verwenden, wird das Dropdownfeld **Anwendungsvorlage** angezeigt. Darüber können Sie zwischen Vorlagen, die sich in der Vorschauphase befinden, und allgemein verfügbaren Vorlagen wechseln. Möglicherweise werden auch andere Vorlagen angezeigt, die für Ihre Organisation zur Verfügung gestellt wurden.

1. Wählen Sie aus, ob Sie diese Anwendung mit einer kostenlosen 7-tägigen Testversion erstellen möchten, oder verwenden Sie ein Abonnement mit nutzungsbasierter Bezahlung.
    - **Testanwendungen** sind sieben Tage lang kostenlos und unterstützen bis zu fünf Geräte. Bevor das geschieht, können sie jederzeit auf die nutzungsbasierte Zahlung umgestellt werden. Bei der Erstellung einer Testanwendung müssen Sie Ihre Kontaktdaten eingeben und auswählen, ob Sie Informationen und Tipps von Microsoft erhalten möchten.
    - Anwendungen mit **nutzungsbasierter Bezahlung** werden pro Gerät berechnet, wobei die ersten zwei Geräte kostenlos sind. Weitere Informationen finden Sie unter [IoT Central – Preise](https://aka.ms/iotcentral-pricing). Wenn Sie eine Anwendung mit nutzungsbasierter Bezahlung erstellen, müssen Sie Ihr *Verzeichnis*, Ihr *Azure-Abonnement* und Ihre *Region* auswählen:
        - Bei *Verzeichnis* handelt es sich um das Azure Active Directory (AAD), in dem Sie Ihre Anwendung erstellen. Ein Azure AD enthält Benutzeridentitäten, Anmeldeinformationen und andere organisatorische Informationen. Falls Sie über keine Azure AD-Instanz verfügen, wird eine erstellt, wenn Sie ein Azure-Abonnement erstellen.
        - Mit einem *Azure-Abonnement* können Sie Instanzen von Azure-Diensten erstellen. IoT Central stellt Ressourcen in Ihrem Abonnement bereit. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eins erstellen. Nachdem Sie das Azure-Abonnement erstellt haben, navigieren Sie zurück zur Seite **Anwendung erstellen**. Ihr neues Abonnement wird in der Dropdownliste **Azure-Abonnement** angezeigt.
        - *Region* ist der physische Speicherort, an dem die Daten Ihrer Geräte gespeichert werden. In der Regel wählen Sie die Region aus, die Ihren Geräten geografisch am nächsten liegt, um sowohl eine optimale Leistung zu erzielen als auch Datenhoheitskonformität zu gewährleisten. Sobald Sie eine Region ausgewählt haben, können Sie Ihre Anwendung später nicht mehr in eine andere Region verschieben.

        > [!NOTE]
        > Während der öffentlichen Vorschauphase sind für eine **Vorschauanwendung** nur die Regionen **Europa, Norden** und **USA, Mitte** verfügbar.

1. Lesen Sie die Geschäftsbedingungen, und wählen Sie **Erstellen** im unteren Bereich der Seite aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine IoT Central-Anwendung erstellt. Wir empfehlen, mit dem folgenden Schritt fortzufahren:

> [!div class="nextstepaction"]
> [Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung](./quick-create-pnp-device.md)
