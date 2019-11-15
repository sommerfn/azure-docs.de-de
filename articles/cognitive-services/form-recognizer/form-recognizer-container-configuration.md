---
title: Konfigurieren eines Containers für die Formularerkennung
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie den Container für die Formularerkennung konfigurieren, um Formular- und Tabellendaten zu analysieren.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: a0b0d0d95e1ffd50faba19f1665ea5dae737b124
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796131"
---
# <a name="configure-form-recognizer-containers"></a>Konfigurieren des Containers für die Formularerkennung

Mithilfe von Containern für die Formularerkennung können Sie eine Anwendungsarchitektur erstellen, die sowohl von widerstandsfähigen Cloudfunktionen als auch von der Edgeposition profitieren kann.

Sie können die Runtimeumgebung für Container für die Formularerkennung über die Argumente des Befehls `docker run` konfigurieren. Dieser Container verfügt über mehrere erforderliche Einstellungen sowie einige optionale Einstellungen. Einige Beispiele finden Sie im Abschnitt [„Beispiele für docker run-Befehle“](#example-docker-run-commands). Die containerspezifischen Einstellungen sind die für die Abrechnung.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> Die Einstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-setting) werden zusammen verwendet. Sie müssen für alle Einstellungen gültige Werte angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](form-recognizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>Konfigurationseinstellung „ApiKey“

Die `ApiKey`-Einstellung gibt den Schlüssel der Azure-Ressourcen an, mit dem die Abrechnungsinformationen für den Container verfolgt werden. Beim Wert für ApiKey muss es sich um einen gültigen Schlüssel für die Ressource vom Typ _Formularerkennung_ handeln, die für die Konfigurationseinstellung `Billing` im Abschnitt „Billing configuration setting“ (Konfigurationseinstellung für die Abrechnung) angegeben wurde.

Sie finden diese Einstellung im Azure-Portal in **Form Recognizer Resource Management** (Ressourcenverwaltung für die Formularerkennung) unter **Schlüssel**.

## <a name="applicationinsights-setting"></a>ApplicationInsights-Einstellung

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Konfigurationseinstellung „Billing“

Die Einstellung `Billing` gibt den Endpunkt-URI der Ressource für die _Formularerkennung_ in Azure an, der zum Messen der Abrechnungsinformationen für den Container verwendet wird. Beim Wert für diese Konfigurationseinstellung muss es sich um einen gültigen URI-Endpunkt für eine Ressource für die _Formularerkennung_ in Azure handeln. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten.

Sie finden diese Einstellung im Azure-Portal in **Form Recognizer Overview** (Übersicht über die Formularerkennung) unter **Endpunkt**.

|Erforderlich| NAME | Datentyp | BESCHREIBUNG |
|--|------|-----------|-------------|
|Ja| `Billing` | Zeichenfolge | URI des Abrechnungsendpunkts. Weitere Informationen zum Erhalt eines Abrechnungs-URI finden Sie unter [Ermitteln erforderlicher Parameter](form-recognizer-container-howto.md#gathering-required-parameters). Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>Eula-Einstellung

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-Einstellungen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Anmeldeinformationseinstellungen für HTTP-Proxy

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Logging-Einstellungen

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Einbindungseinstellungen

Verwenden Sie Bindungsbereitstellungen zum Lesen und Schreiben von Daten im Container. Sie können eine Eingabe- oder Ausgabeeinbindung über die Option `--mount` im [`docker run`-Befehl](https://docs.docker.com/engine/reference/commandline/run/) angeben.

Der Container für die Formularerkennung erfordert eine Ein- und Ausgabeeinbindung. Die Eingabeeinbindung kann schreibgeschützt sein und ist für den Zugriff auf Daten erforderlich, die für das Training und die Bewertung verwendet werden. Die Ausgabeeinbindung muss beschreibbar sein und wird zum Speichern der Modelle und temporären Daten verwendet.

Die genaue Syntax für den Bereitstellungspunkt auf dem Host variiert je nach Betriebssystem des Hosts. Darüber hinaus ist der Zugriff auf den Einbindungspunkt des [Hostcomputers](form-recognizer-container-howto.md#the-host-computer) möglicherweise aufgrund eines Konflikts zwischen den vom Docker-Dienstkonto verwendeten Berechtigungen und den für den Einbindungspunkt auf dem Host verwendeten Berechtigungen nicht möglich.

|Optional| NAME | Datentyp | BESCHREIBUNG |
|-------|------|-----------|-------------|
|Erforderlich| `Input` | Zeichenfolge | Das Ziel der Eingabeeinbindung. Standardwert: `/input`.    <br><br>Beispiel:<br>`--mount type=bind,src=c:\input,target=/input`|
|Erforderlich| `Output` | Zeichenfolge | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`.  <br><br>Beispiel:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Beispiele für den Befehl „docker run“

Die folgenden Beispiele verwenden die Konfigurationseinstellungen, um zu veranschaulichen, wie `docker run`-Befehle geschrieben und verwendet werden. Bei der Ausführung wird der Container so lange ausgeführt, bis Sie ihn [beenden](form-recognizer-container-howto.md#stop-the-container).

* **Zeilenfortsetzungszeichen:** In den Docker-Befehlen in den folgenden Abschnitten wird ein umgekehrter Schrägstrich (\\) als Zeilenfortsetzungszeichen verwendet. Ersetzen oder entfernen Sie dieses Zeichen je nach den Anforderungen des Hostbetriebssystems.
* **Argumentreihenfolge:** Ändern Sie die Reihenfolge der Argumente nur, wenn Sie mit Docker-Containern vertraut sind.

Ersetzen Sie in der folgenden Tabelle {_argument_name_} durch Ihre eigenen Werte:

| Platzhalter | Wert |
|-------------|-------|
| **{FORM_RECOGNIZER_API_KEY}** | Der Schlüssel, der zum Starten des Containers verwendet wird. Er steht im Azure-Portal auf der Seite „Form Recognizer Keys“ (Schlüssel für Formularerkennung) zur Verfügung. |
| **{FORM_RECOGNIZER_ENDPOINT_URI}** | Den Wert für den URI des Abrechnungsendpunkts finden Sie im Azure-Portal auf der Seite „Form Recognizer Overview“ (Formularerkennung – Übersicht).|
| **{COMPUTER_VISION_API_KEY}** | Der Schlüssel steht im Azure-Portal auf der Seite „Computer Vision API Keys“ (Schlüssel für die Maschinelles Sehen-API) zur Verfügung.|
| **{COMPUTER_VISION_ENDPOINT_URI}** | Der Abrechnungsendpunkt. Wenn Sie eine cloudbasierte Ressource für maschinelles Sehen verwenden, steht der Wert für den URI im Azure-Portal auf der Seite „Computer Vision API Overview“ (Maschinelles Sehen-API – Übersicht) zur Verfügung. Bei Verwendung eines *cognitive-services-recognize-text*-Containers verwenden Sie die URL des Abrechnungsendpunkts, die im Befehl `docker run` an den Container übergeben wird. |

Informationen dazu, wie Sie diese Werte erhalten, finden Sie unter [Ermitteln erforderlicher Parameter](form-recognizer-container-howto.md#gathering-required-parameters).

[!INCLUDE [cognitive-services-custom-subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Geben Sie zum Ausführen des Containers die Optionen `Eula`, `Billing` und `ApiKey` an. Andernfalls wird der Container nicht gestartet. Weitere Informationen finden Sie unter [Abrechnung](#billing-configuration-setting).

## <a name="form-recognizer-container-docker-examples"></a>Beispiele für Docker-Container zur Formularerkennung

Im Folgenden finden Sie Docker-Beispiele für den Container für die Formularerkennung.

### <a name="basic-example-for-form-recognizer"></a>Einfaches Beispiel für Formularerkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

### <a name="logging-example-for-form-recognizer"></a>Protokollierungsbeispiel für Formularerkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie den Artikel zum [Installieren und Ausführen von Containern](form-recognizer-container-howto.md).
