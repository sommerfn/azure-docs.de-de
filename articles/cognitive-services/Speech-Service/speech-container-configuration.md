---
title: Konfigurieren von Speech-Containern
titleSuffix: Azure Cognitive Services
description: Speech Services stellt für jeden Container ein allgemeines Konfigurationsframework bereit, sodass Sie ganz einfach Aspekte wie Speicher, Protokollierung und Telemetrie sowie Sicherheitseinstellungen für Ihre Container konfigurieren und verwalten können.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: f68bf989202c209f89ea273fee8d7610a49415ed
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075737"
---
# <a name="configure-speech-service-containers"></a>Konfigurieren von Containern für den Speech-Dienst

Mit Speech-Containern können Kunden eine Speech-basierte Anwendungsarchitektur erstellen, die sowohl von widerstandsfähigen Cloudfunktionen als auch von der Edgeposition profitieren kann. Derzeit werden die vier Speech-Container **speech-to-text**, **custom-speech-to-text**, **text-to-speech** und **custom-text-to-speech** unterstützt.

Die Runtimeumgebung für **Speech**-Container wird über die Argumente des Befehls `docker run` konfiguriert. Dieser Container verfügt über mehrere erforderliche Einstellungen sowie einige optionale Einstellungen. Es sind noch viele [Beispiele](#example-docker-run-commands) für den Befehl verfügbar. Die containerspezifischen Einstellungen sind die für die Abrechnung.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> Die Einstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](speech-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>Konfigurationseinstellung „ApiKey“

Die `ApiKey`-Einstellung gibt den Schlüssel der Azure-Ressourcen an, mit dem die Abrechnungsinformationen für den Container verfolgt werden. Sie müssen einen Wert für „ApiKey“ angeben. Bei diesem Wert muss es sich um einen gültigen Schlüssel für die _Speech_-Ressource handeln, die für die Konfigurationseinstellung [`Billing`](#billing-configuration-setting) angegeben wurde.

Diese Einstellung finden Sie hier:

- Azure-Portal: **Speech**-Ressourcenverwaltung (unter **Schlüssel**)

## <a name="applicationinsights-setting"></a>ApplicationInsights-Einstellung

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Konfigurationseinstellung „Billing“

Die Einstellung `Billing` gibt den Endpunkt-URI der _Speech_-Ressource in Azure an, der zum Messen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen URI-Endpunkt für eine _Speech_-Ressource in Azure handeln. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten.

Diese Einstellung finden Sie hier:

- Azure-Portal: Übersicht über **Speech** mit der Bezeichnung `Endpoint`

| Erforderlich | NAME | Datentyp | BESCHREIBUNG |
| -------- | ---- | --------- | ----------- |
| Ja | `Billing` | Zeichenfolge | URI des Abrechnungsendpunkts. Weitere Informationen zum Erhalt eines Abrechnungs-URI finden Sie unter [Ermitteln erforderlicher Parameter](speech-container-howto.md#gathering-required-parameters). Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>Eula-Einstellung

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-Einstellungen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Anmeldeinformationseinstellungen für HTTP-Proxy

[!INCLUDE [Container shared HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Logging-Einstellungen

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Einbindungseinstellungen

Verwenden Sie Bindungsbereitstellungen zum Lesen und Schreiben von Daten im Container. Sie können eine Eingabe- oder Ausgabebereitstellung über die Option `--mount` im Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben.

Die Speech-Standardcontainer verwenden keine Eingabe- oder Ausgabeeinbindungen zum Speichern von Trainings- oder Dienstdaten. Benutzerdefinierte Speech-Container basieren jedoch auf Volumebereitstellungen.

Die genaue Syntax für den Bereitstellungspunkt auf dem Host variiert je nach Betriebssystem des Hosts. Darüber hinaus ist es eventuell nicht möglich, auf den Bereitstellungspunkt auf dem [Hostcomputer](speech-container-howto.md#the-host-computer) zuzugreifen, wenn ein Konflikt zwischen den vom Docker-Dienstkonto verwendeten Berechtigungen und den für den Bereitstellungspunkt auf dem Host verwendeten Berechtigungen besteht.

| Optional | NAME | Datentyp | BESCHREIBUNG |
| -------- | ---- | --------- | ----------- |
| Nicht zulässig | `Input` | Zeichenfolge | Wird von Speech-Standardcontainern nicht verwendet. Benutzerdefinierte Speech-Container verwenden [Volumebereitstellungen](#volume-mount-settings).                                                                                    |
| Optional | `Output` | Zeichenfolge | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`. Dies ist der Speicherort der Protokolle. Beinhaltet Containerprotokolle. <br><br>Beispiel:<br>`--mount type=bind,src=c:\output,target=/output` |

## <a name="volume-mount-settings"></a>Einstellungen für Volumebereitstellungen

Die benutzerdefinierten Speech-Container verwenden [Volumebereitstellungen](https://docs.docker.com/storage/volumes/), um benutzerdefinierte Modelle beizubehalten. Sie können eine Volumebereitstellung angeben, indem Sie dem Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) die Option `-v` (oder `--volume`) hinzufügen.

Benutzerdefinierte Modelle werden bei der erstmaligen Erfassung eines neuen Modells als Teil des Befehls „docker run“ für den benutzerdefinierten Speech-Container heruntergeladen. Bei nachfolgenden Ausführungen derselben `ModelId` für einen benutzerdefinierten Speech-Container wird das zuvor heruntergeladene Modell verwendet. Wenn keine Volumebereitstellung vorhanden ist, können benutzerdefinierte Modelle nicht dauerhaft gespeichert werden.

Die Einstellung für die Volumebereitstellung besteht aus drei durch Doppelpunkte (`:`) voneinander getrennten Feldern:

1. Das erste Feld ist der Name des Volumes auf dem Hostcomputer, z. B. _C:\input_.
2. Das zweite Feld ist das Verzeichnis im Container, z. B. _/usr/local/models_.
3. Das dritte (optionale) Feld ist eine durch Kommas getrennte Liste von Optionen. Weitere Informationen finden Sie unter [Verwenden von Volumes](https://docs.docker.com/storage/volumes/).

### <a name="volume-mount-example"></a>Beispiel für Volumebereitstellung

```bash
-v C:\input:/usr/local/models
```

Mit diesem Befehl wird das Verzeichnis _C:\input_ vom Hostcomputer in das Verzeichnis _/usr/local/models_ des Containers eingebunden.

> [!IMPORTANT]
> Die Einstellungen der Volumebereitstellung gelten nur für die benutzerdefinierten Container **speech-to-text** und **text-to-speech**. Die Standardcontainer **speech-to-text** und **text-to-speech** verwenden keine Volumebereitstellungen.

## <a name="example-docker-run-commands"></a>Beispiele für den Befehl „docker run“

Die folgenden Beispiele verwenden die Konfigurationseinstellungen, um zu veranschaulichen, wie `docker run`-Befehle geschrieben und verwendet werden. Nach dem Ausführen wird der Container so lange ausgeführt, bis Sie ihn [beenden](speech-container-howto.md#stop-the-container).

- **Zeilenfortsetzungszeichen:** In den Docker-Befehlen in den folgenden Abschnitten wird der umgekehrte Schrägstrich (`\`) als Zeilenfortsetzungszeichen verwendet. Ersetzen oder entfernen Sie diesen je nach den Anforderungen des Hostbetriebssystems.
- **Argumentreihenfolge:** Ändern Sie die Reihenfolge der Argumente nur, wenn Sie mit Docker-Containern vertraut sind.

Ersetzen Sie {_argument_name_} durch Ihre eigenen Werte:

| Platzhalter | Wert | Format oder Beispiel |
| ----------- | ----- | ----------------- |
| **{API_KEY}** | Der Endpunktschlüssel der `Speech`-Ressource auf der Azure `Speech`-Schlüsselseite.   | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`                                                                                  |
| **{ENDPOINT_URI}** | Den Wert des Abrechnungsendpunkts finden Sie auf der Übersichtsseite von Azure `Speech`. | Ausführliche Beispiele finden Sie unter [Ermitteln erforderlicher Parameter](speech-container-howto.md#gathering-required-parameters). |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet. Weitere Informationen finden Sie unter [Abrechnung](#billing-configuration-setting).
> Der ApiKey-Wert ist der **Schlüssel** von der Schlüsselseite der Azure Speech-Ressource.

## <a name="speech-container-docker-examples"></a>Beispiele für Docker-Speech-Container

Im Folgenden finden Sie Docker-Beispiele für den Speech-Container.

## <a name="speech-to-texttabstt"></a>[Spracherkennung](#tab/stt)

### <a name="basic-example-for-speech-to-text"></a>Einfaches Beispiel für Spracherkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-speech-to-text"></a>Protokollierungsbeispiel für Spracherkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-speech-to-texttabcstt"></a>[Benutzerdefinierte Spracherkennung](#tab/cstt)

### <a name="basic-example-for-custom-speech-to-text"></a>Einfaches Beispiel für benutzerdefinierte Spracherkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-speech-to-text"></a>Protokollierungsbeispiel für benutzerdefinierte Spracherkennung

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="text-to-speechtabtss"></a>[Sprachsynthese](#tab/tss)

### <a name="basic-example-for-text-to-speech"></a>Einfaches Beispiel für Sprachsynthese

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-text-to-speech"></a>Protokollierungsbeispiel für Sprachsynthese

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-text-to-speechtabctts"></a>[Benutzerdefinierte Sprachsynthese](#tab/ctts)

### <a name="basic-example-for-custom-text-to-speech"></a>Einfaches Beispiel für benutzerdefinierte Sprachsynthese

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-text-to-speech"></a>Protokollierungsbeispiel für benutzerdefinierte Sprachsynthese

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

---

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Installieren und Ausführen von Containern](speech-container-howto.md).
