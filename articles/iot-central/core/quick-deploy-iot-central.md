---
title: Erstellen einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erstellen Sie eine neue Azure IoT Central-Anwendung. Erstellen Sie unter Verwendung einer Anwendungsvorlage eine Testversion oder eine Anwendung mit nutzungsbasierter Zahlung.
author: viv-liu
ms.author: viviali
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 5cb6beca56cac94ca05671cdf34c45913a710c39
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72936889"
---
# <a name="create-an-azure-iot-central-application"></a>Erstellen einer Azure IoT Central-Anwendung

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Als _Ersteller_ verwenden Sie die Benutzeroberfläche von Azure IoT Central, um Ihre Microsoft Azure IoT Central-Anwendung zu definieren. In dieser Schnellstartanleitung wird gezeigt, wie Sie eine Azure IoT Central-Anwendung mit einer exemplarischen _Gerätevorlage_ erstellen. Die von Ihnen erstellte Anwendung verwendet keine Previewfunktionen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Navigieren Sie zur Buildwebsite für [Azure IoT Central](https://aka.ms/iotcentral). Melden Sie sich dann mit einem persönlichen Microsoft-Konto oder mit einem Geschäfts-, Schul- oder Unikonto an.

Wählen Sie **Erstellen** aus, um mit der Erstellung einer Azure IoT Central-Anwendung ohne Previewfunktionen zu beginnen. Über diesen Link gelangen Sie auf die Seite **Eigene IoT-Anwendung erstellen**.

![Seite zur Erstellung einer Azure IoT Central-Anwendung](media/quick-deploy-iot-central/iotcentralcreate.png)

Wählen Sie dann **Benutzerdefinierte App** aus.

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Azure IoT Central schlägt automatisch einen Anwendungsnamen entsprechend der von Ihnen ausgewählten Anwendungsvorlage vor. Sie können diesen Namen übernehmen oder einen eigenen aussagekräftigen Anwendungsnamen eingeben, z. B. **Contoso IoT**. Azure IoT Central generiert außerdem basierend auf dem Anwendungsnamen eine eindeutige URL für Sie. Sie können dieses URL-Präfix in einen einprägsameren Wert ändern.

1. Wählen Sie eine Anwendungsvorlage aus, für die keine Previewfunktionen verwendet werden. Eine Anwendungsvorlage kann vordefinierte Elemente wie Gerätevorlagen und Dashboards enthalten, die Ihnen den Einstieg erleichtern.

    | Anwendungsvorlage | BESCHREIBUNG |
    | -------------------- | ----------- |
    | Beispiel „Contoso“       | Erstellt eine Anwendung mit einer Gerätevorlage, die bereits für einen gekühlten Verkaufsautomaten erstellt wurde. Verwenden Sie diese Vorlage, um mit der Erkundung von Azure IoT Central zu beginnen. |
    | Beispiel-Entwickler-Kits       | Erstellt eine Anwendung mit Gerätevorlagen, an die Sie ein MXChip- oder Raspberry Pi-Gerät anschließen können. Verwenden Sie diese Vorlage, wenn Sie als Geräteentwickler mit einem dieser Geräte experimentieren. |
    | Benutzerdefinierte Anwendung   | Erstellt eine leere Anwendung, die Sie mit Ihren eigenen Gerätevorlagen und Geräten füllen können. |

1. Wählen Sie einen Zahlungsplan aus:
   - Anwendungen mit einer **kostenlosen 7-Tage-Testversion** können sieben Tage kostenlos genutzt werden, bevor sie ablaufen. Bevor das geschieht, können sie jederzeit auf die **nutzungsbasierte Zahlung** umgestellt werden. Bei der Erstellung einer **Testanwendung** müssen Sie Ihre Kontaktdaten eingeben und auswählen, ob Sie Informationen und Tipps von Microsoft erhalten möchten.
   - Anwendungen mit **nutzungsbasierter Zahlung** werden pro Gerät berechnet, wobei die ersten fünf Geräte kostenlos sind. Wenn Sie eine Anwendung mit **nutzungsbasierter Zahlung** erstellen, müssen Sie Ihr *Verzeichnis*, Ihr *Azure-Abonnement* und Ihre *Region* auswählen:
        - Bei *Verzeichnis* handelt es sich um die Azure Active Directory-Instanz (AD) für die Erstellung Ihrer Anwendung. Es enthält Benutzeridentitäten, Anmeldeinformationen und andere organisatorische Informationen. Falls Sie über keine Azure AD-Instanz verfügen, wird eine erstellt, wenn Sie ein Azure-Abonnement erstellen.
        - Mit einem *Azure-Abonnement* können Sie Instanzen von Azure-Diensten erstellen. IoT Central stellt Ressourcen in Ihrem Abonnement bereit. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eins erstellen. Nachdem Sie das Azure-Abonnement erstellt haben, navigieren Sie zurück zur Seite **Anwendung erstellen**. Ihr neues Abonnement wird in der Dropdownliste **Azure-Abonnement** angezeigt.
        - *Region* ist der physische Standort oder die [geografische Region](https://azure.microsoft.com/global-infrastructure/geographies/), an dem bzw. in der Sie Ihre Anwendung erstellen möchten. Normalerweise wählen Sie die Region aus, die Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Die Regionen, in denen Azure IoT Central verfügbar ist, finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-central). Sobald Sie eine Region ausgewählt haben, können Sie Ihre Anwendung später nicht mehr in eine andere Region verschieben.

        Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).

1. Geben Sie für den zuvor in Schritt 1 ausgewählten Zahlungsplan weitere erforderliche Informationen ein.

1. Wählen Sie am unteren Rand der Seite die Option **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine IoT Central-Anwendung erstellt. Wir empfehlen, mit dem folgenden Schritt fortzufahren:

> [!div class="nextstepaction"]
> [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](./tutorial-define-device-type.md)
