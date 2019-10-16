---
title: Erstellen einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erstellen Sie eine neue Azure IoT Central-Anwendung. Erstellen Sie unter Verwendung einer Anwendungsvorlage eine Testversion oder eine Anwendung mit nutzungsbasierter Zahlung.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 3d5dfe824d95506fa3f83f1fbbdca1e29e02d566
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001293"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Azure IoT Central-Anwendung erstellen, die Previewfunktionen nutzt, etwa IoT Plug & Play.

> [!WARNING]
> Die IoT Plug & Play-Funktionen in Azure IoT Central befinden sich derzeit in der Vorschauphase. Verwenden Sie keine IoT Central-Anwendung mit aktivierter IoT Plug & Play-Funktion für Produktionsworkloads. Verwenden Sie für Produktionsumgebungen eine IoT Central-Anwendung, die auf der Grundlage einer aktuellen, allgemein verfügbaren Anwendungsvorlage erstellt wurde.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Navigieren Sie zur Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral). Melden Sie sich dann mit einem persönlichen Microsoft-Konto oder mit einem Geschäfts-, Schul- oder Unikonto an.

Wählen Sie **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen. Über diesen Link gelangen Sie auf die Seite **Anwendung erstellen**.

![Azure IoT Central-Seite „Anwendung erstellen“](media/quick-deploy-iot-central-pnp/iotcentralcreate-pnp.png)

So erstellen Sie eine neue Azure IoT Central-Anwendung mit Previewfunktionen wie etwa IoT Plug & Play:

1. Wählen Sie einen Zahlungsplan aus:
   - Anwendungen mit einer **Testversion** können sieben Tage kostenlos genutzt werden, bevor die Version abläuft. Bevor das geschieht, können sie jederzeit auf die **nutzungsbasierte Zahlung** umgestellt werden. Bei der Erstellung einer **Testanwendung** müssen Sie Ihre Kontaktdaten eingeben und auswählen, ob Sie Informationen und Tipps von Microsoft erhalten möchten.
   - Anwendungen mit **nutzungsbasierter Zahlung** werden pro Gerät berechnet, wobei die ersten fünf Geräte kostenlos sind. Wenn Sie eine Anwendung mit **nutzungsbasierter Zahlung** erstellen, müssen Sie Ihr *Verzeichnis*, Ihr *Azure-Abonnement* und Ihre *Region* auswählen:
        - Bei *Verzeichnis* handelt es sich um die Azure Active Directory-Instanz (AD) für die Erstellung Ihrer Anwendung. Es enthält Benutzeridentitäten, Anmeldeinformationen und andere organisatorische Informationen. Falls Sie über keine Azure AD-Instanz verfügen, wird eine erstellt, wenn Sie ein Azure-Abonnement erstellen.
        - Mit einem *Azure-Abonnement* können Sie Instanzen von Azure-Diensten erstellen. IoT Central stellt Ressourcen in Ihrem Abonnement bereit. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eins erstellen. Nachdem Sie das Azure-Abonnement erstellt haben, navigieren Sie zurück zur Seite **Anwendung erstellen**. Ihr neues Abonnement wird in der Dropdownliste **Azure-Abonnement** angezeigt.
        - *Region* ist der physische Standort, an dem Sie Ihre Anwendung erstellen möchten. Normalerweise wählen Sie die Region aus, die Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Während der öffentlichen Vorschauphase sind für eine **Vorschauanwendung** nur die Regionen **Europa, Norden** und **USA, Mitte** verfügbar. Sobald Sie eine Region ausgewählt haben, können Sie Ihre Anwendung später nicht mehr in eine andere Region verschieben.

        Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).

1. Wählen Sie die Vorlage **Vorschauanwendung** aus. Eine Anwendungsvorlage kann vordefinierte Elemente wie Gerätevorlagen und Dashboards enthalten, die Ihnen den Einstieg erleichtern.

1. Azure IoT Central schlägt automatisch einen Anwendungsnamen entsprechend der von Ihnen ausgewählten Anwendungsvorlage vor. Sie können diesen Namen übernehmen oder einen eigenen aussagekräftigen Anwendungsnamen eingeben, z. B. **Contoso IoT**. Azure IoT Central generiert außerdem entsprechend dem Anwendungsnamen ein Präfix mit einer eindeutigen URL für Sie. Sie können dieses URL-Präfix in einen einprägsameren Wert ändern.

1. Geben Sie für den zuvor in Schritt 1 ausgewählten Zahlungsplan weitere erforderliche Informationen ein.

1. Wählen Sie am unteren Rand der Seite die Option **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine IoT Central-Anwendung erstellt, die die Previewfunktionen nutzt. Wir empfehlen, mit dem folgenden Schritt fortzufahren:

> [!div class="nextstepaction"]
> [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](./tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
