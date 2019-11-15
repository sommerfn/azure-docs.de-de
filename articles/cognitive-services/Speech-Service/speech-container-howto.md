---
title: Installieren von Speech-Containern – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Installieren Sie Speech-Container, und führen Sie sie aus. Die Spracherkennung wandelt Audiodatenströme in Echtzeit in Text um, der von Ihren Anwendungen, Tools oder Geräten genutzt oder angezeigt werden kann. Die Sprachsynthese konvertiert Eingabetext in menschenähnliche synthetische Sprache.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: efb2fd8fd6b77a27130b834c2b192c1e88eec97c
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73578396"
---
# <a name="install-and-run-speech-service-containers"></a>Installieren und Ausführen von Containern für den Speech-Dienst

Container ermöglichen es Ihnen, einige der Speech-Dienst-APIs in Ihrer eigenen Umgebung auszuführen. Container eignen sich hervorragend für bestimmte Sicherheits- und Datengovernanceanforderungen. In diesem Artikel erfahren Sie, wie Sie einen Speech-Container herunterladen, installieren und ausführen.

Mit Speech-Containern können Kunden eine Speech-basierte Anwendungsarchitektur erstellen, die sowohl für stabile Cloudfunktionen als auch für das Edge optimiert ist. Es stehen vier verschiedene Container zur Verfügung. Die beiden Standardcontainer sind **Spracherkennung** und **Sprachsynthese**. Die beiden benutzerdefinierten Container sind **Benutzerdefinierte Spracherkennung** und **Benutzerdefinierte Sprachsynthese**.

> [!IMPORTANT]
> Alle Speech-Container werden zurzeit im Rahmen einer [öffentlichen eingeschränkten Vorschau](../cognitive-services-container-support.md#public-gated-preview-container-registry-containerpreviewazurecrio) angeboten. Wenn Speech-Container in die allgemeine Verfügbarkeit übergehen, erfolgt eine entsprechende Ankündigung.

| Funktion | Features | Neueste Version |
|--|--|--|
| Spracherkennung | Transkribiert kontinuierliche Echtzeitsprache oder Batchaudioaufzeichnungen in Text mit Zwischenergebnissen. | 2.0.0 |
| Benutzerdefinierte Spracherkennung | Verwendet ein benutzerdefiniertes Modell aus dem [Custom Speech-Portal](https://speech.microsoft.com/customspeech) und transkribiert kontinuierliche Echtzeitsprach- oder Batchaudioaufzeichnungen in Text mit Zwischenergebnissen. | 2.0.0 |
| Text-zu-Sprache | Konvertiert Text in natürlich klingende Sprache mit Nur-Text-Eingaben oder SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese). | 1.3.0 |
| Benutzerdefinierte Sprachsynthese | Verwendet ein benutzerdefiniertes Modell aus dem [Custom Voice-Portal](https://aka.ms/custom-voice-portal) und konvertiert Text in natürlich klingende Sprache mit Nur-Text-Eingaben oder SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese). | 1.3.0 |

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Verwendung von Speech-Containern müssen folgende Voraussetzungen erfüllt sein:

| Erforderlich | Zweck |
|--|--|
| Docker-Engine | Die Docker-Engine muss auf einem [Hostcomputer](#the-host-computer) installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können. <br><br> **Unter Windows** muss Docker auch für die Unterstützung von Linux-Containern konfiguriert werden.<br><br> |
| Kenntnisse zu Docker | Sie sollten über Grundkenntnisse der Konzepte von Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen. |
| Speech-Ressource | Um diese Container zu verwenden, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ _Speech_, um den entsprechenden API-Schlüssel und den URI des Endpunkts zu erhalten. Beide Werte stehen im Azure-Portal auf den Seiten „Übersicht“ und „Schlüssel“ von **Speech** zur Verfügung. Beide sind zum Starten des Containers erforderlich.<br><br>**{API_KEY}** : Einer der beiden verfügbaren Ressourcenschlüssel auf der Seite **Schlüssel**<br><br>**{ENDPOINT_URI}** : Der Endpunkt, der auf der Seite **Übersicht** angegeben ist |

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Füllen Sie das [Formular zum Anfordern von Cognitive Services-Speech-Containern](https://aka.ms/speechcontainerspreview/) aus, und übermitteln Sie es, um Zugriff auf den Container anzufordern. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Der Hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Unterstützung von Advanced Vector Extensions

Der **Host** ist der Computer, auf dem der Docker-Container ausgeführt wird. Der Host *muss Unterstützung* für [Advanced Vector Extensions (AVX2)](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) bieten. Sie können die AVX2-Unterstützung auf Linux-Hosts mit dem folgenden Befehl überprüfen:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> Der Hostcomputer *muss* AVX2 unterstützen. Ohne AVX2-Unterstützung funktioniert der Container *nicht* ordnungsgemäß.

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

Die folgende Tabelle beschreibt die minimale und empfohlene Zuordnung von Ressourcen für jeden Speech-Container.

# <a name="speech-to-texttabstt"></a>[Spracherkennung](#tab/stt)

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
| Spracherkennung | 2 Kerne, 2 GB Arbeitsspeicher | 4 Kerne, 4 GB Arbeitsspeicher |

# <a name="custom-speech-to-texttabcstt"></a>[Benutzerdefinierte Spracherkennung](#tab/cstt)

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
| Benutzerdefinierte Spracherkennung | 2 Kerne, 2 GB Arbeitsspeicher | 4 Kerne, 4 GB Arbeitsspeicher |

# <a name="text-to-speechtabtts"></a>[Sprachsynthese](#tab/tts)

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
| Text-zu-Sprache | Ein Kern, 2 GB Arbeitsspeicher | 2 Kerne, 3 GB Arbeitsspeicher |

# <a name="custom-text-to-speechtabctts"></a>[Benutzerdefinierte Sprachsynthese](#tab/ctts)

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
| Benutzerdefinierte Sprachsynthese | Ein Kern, 2 GB Arbeitsspeicher | 2 Kerne, 3 GB Arbeitsspeicher |

***

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.

> [!NOTE]
> Die Mindestanforderungen und Empfehlungen basieren auf Docker-Grenzwerten, *nicht* auf den Ressourcen des Hostcomputers. Spracherkennungscontainer ordnen beispielsweise Teile des Arbeitsspeichers eines großen Sprachmodells zu, und es wird *empfohlen*, dass die gesamte Datei in den Arbeitsspeicher passt (was zusätzlich 4 bis 6 GB ausmacht). Außerdem dauert die erste Ausführung des Containers unter Umständen länger, da Modelle in den Arbeitsspeicher ausgelagert werden.

## <a name="get-the-container-image-with-docker-pull"></a>Abrufen des Containerimages mit `docker pull`

Containerimages für Speech stehen in der folgenden Container Registry zur Verfügung:

# <a name="speech-to-texttabstt"></a>[Spracherkennung](#tab/stt)

| Container | Repository |
|-----------|------------|
| Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |

# <a name="custom-speech-to-texttabcstt"></a>[Benutzerdefinierte Spracherkennung](#tab/cstt)

| Container | Repository |
|-----------|------------|
| Benutzerdefinierte Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest` |

# <a name="text-to-speechtabtts"></a>[Sprachsynthese](#tab/tts)

| Container | Repository |
|-----------|------------|
| Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

# <a name="custom-text-to-speechtabctts"></a>[Benutzerdefinierte Sprachsynthese](#tab/ctts)

| Container | Repository |
|-----------|------------|
| Benutzerdefinierte Sprachsynthese | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest` |

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-speech-containers"></a>Docker-Pullvorgang für die Speech-Container

# <a name="speech-to-texttabstt"></a>[Spracherkennung](#tab/stt)

#### <a name="docker-pull-for-the-speech-to-text-container"></a>Docker-Pullvorgang für den Spracherkennungscontainer

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus der Registrierung der Containervorschau herunterzuladen.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

> [!IMPORTANT]
> Das Tag `latest` pullt das Gebietsschema `en-US` und die Stimme `jessarus`. Weitere Gebietsschemas finden Sie unter [Gebietsschemas für die Spracherkennung](#speech-to-text-locales).

#### <a name="speech-to-text-locales"></a>Gebietsschemas für die Spracherkennung

Mit Ausnahme von `latest` liegen alle Tags im folgenden Format vor, wobei `<culture>` den Gebietsschemacontainer angibt:

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

Das folgende Tag ist ein Beispiel für das Format:

```
2.0.0-amd64-en-us-preview
```

Informationen zu allen unterstützten Gebietsschemas in Version 2.0.0 des Containers für die **Spracherkennung** finden Sie in der Tabelle mit den [für die Spracherkennung unterstützten Sprachen](language-support.md#speech-to-text) in der Spalte **Containerunterstützung**.

# <a name="custom-speech-to-texttabcstt"></a>[Benutzerdefinierte Spracherkennung](#tab/cstt)

#### <a name="docker-pull-for-the-custom-speech-to-text-container"></a>Docker-Pullvorgang für den benutzerdefinierten Spracherkennungscontainer

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus der Registrierung der Containervorschau herunterzuladen.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest
```

> [!NOTE]
> Die Werte für `locale` und `voice` für benutzerdefinierte Speech-Container werden durch das benutzerdefinierte Modell bestimmt, das vom Container erfasst wird.

# <a name="text-to-speechtabtts"></a>[Sprachsynthese](#tab/tts)

#### <a name="docker-pull-for-the-text-to-speech-container"></a>Docker-Pullvorgang für den Sprachsynthesecontainer

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus der Registrierung der Containervorschau herunterzuladen.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

> [!IMPORTANT]
> Das Tag `latest` pullt das Gebietsschema `en-US` und die Stimme `jessarus`. Weitere Gebietsschemas finden Sie unter [Gebietsschemas für die Sprachsynthese](#text-to-speech-locales).

#### <a name="text-to-speech-locales"></a>Gebietsschemas für die Sprachsynthese

Mit Ausnahme von `latest` liegen alle Tags im folgenden Format vor, wobei `<culture>` das Gebietsschema und `<voice>` die Stimme des Containers angibt:

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

Das folgende Tag ist ein Beispiel für das Format:

```
1.3.0-amd64-en-us-jessarus-preview
```

Informationen zu allen unterstützten Gebietsschemas und entsprechenden Stimmen in Version 1.3.0 des Containers für die **Sprachsynthese** finden Sie in der Tabelle mit den [für Standardstimmen unterstützten Sprachen](language-support.md#standard-voices) in der Spalte **Containerunterstützung**.

> [!IMPORTANT]
> Beim Erstellen einer HTTP POST-Anforderung für die *standardmäßige Sprachsynthese* erfordert die Meldung der [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) ein `voice`-Element mit einem `name`-Attribut. Der Wert ist das entsprechende Gebietsschema des Containers und die Stimme, auch bekannt als „[Kurzname](language-support.md#standard-voices)“. Das Tag `latest` beispielsweise weist den Sprachnamen `en-US-JessaRUS` auf.

# <a name="custom-text-to-speechtabctts"></a>[Benutzerdefinierte Sprachsynthese](#tab/ctts)

#### <a name="docker-pull-for-the-custom-text-to-speech-container"></a>Docker-Pullvorgang für den benutzerdefinierten Sprachsynthesecontainer

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus der Registrierung der Containervorschau herunterzuladen.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest
```

> [!NOTE]
> Die Werte für `locale` und `voice` für benutzerdefinierte Speech-Container werden durch das benutzerdefinierte Modell bestimmt, das vom Container erfasst wird.

***

## <a name="how-to-use-the-container"></a>Verwenden des Containers

Wenn sich der Container auf dem [Hostcomputer](#the-host-computer) befindet, können Sie über den folgenden Prozess mit dem Container arbeiten.

1. [Führen Sie den Container aus](#run-the-container-with-docker-run), und verwenden Sie dabei die erforderlichen Abrechnungseinstellungen. Es sind noch weitere [Beispiele](speech-container-configuration.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.
1. [Fragen Sie den Vorhersageendpunkt des Containers ab.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Ausführen des Containers mit `docker run`

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um den Container auszuführen. Genaue Informationen dazu, wie Sie die Werte `{Endpoint_URI}` und `{API_Key}` abrufen, erhalten Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters). Es sind auch weitere [Beispiele](speech-container-configuration.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.

# <a name="speech-to-texttabstt"></a>[Spracherkennung](#tab/stt)

Zum Ausführen des Containers für die *Spracherkennung* führen Sie den folgenden `docker run`-Befehl aus.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen Container für die *Spracherkennung* aus dem Containerimage aus.
* Ordnet 4 CPU-Kerne und 4 GB Arbeitsspeicher zu.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

# <a name="custom-speech-to-texttabcstt"></a>[Benutzerdefinierte Spracherkennung](#tab/cstt)

Der Container für *benutzerdefinierte Spracherkennung* basiert auf einem benutzerdefinierten Sprachmodell. Das benutzerdefinierte Modell muss über das [Custom Speech-Portal](https://speech.microsoft.com/customspeech) [trainiert](how-to-custom-speech-train-model.md) worden sein. Die **Modell-ID** für Custom Speech ist zur Ausführung des Containers erforderlich. Sie finden diese auf der Seite **Training** des Custom Speech-Portals. Navigieren Sie im Custom Speech-Portal zur Seite **Training**, und wählen Sie das Modell aus.
<br>

![Seite „Training“ im Custom Speech-Portal](media/custom-speech/custom-speech-model-training.png)

Rufen Sie die **Modell-ID** ab, um diese als Argument für den `ModelId`-Parameter des `docker run`-Befehls zu verwenden.
<br>

![Details zum benutzerdefinierten Sprachmodell](media/custom-speech/custom-speech-model-details.png)

Die folgende Tabelle zeigt die verschiedenen `docker run`-Parameter und die entsprechenden Beschreibungen:

| Parameter | BESCHREIBUNG |
|---------|---------|
| `{VOLUME_MOUNT}` | Die [Volumebereitstellung](https://docs.docker.com/storage/volumes/) des Hostcomputers, die Docker zum dauerhaften Speichern des benutzerdefinierten Modells verwendet. Beispiel: *C:\CustomSpeech*, wobei sich *Laufwerk „C“* auf dem Hostcomputer befindet. |
| `{MODEL_ID}` | Die **Modell-ID** für Custom Speech von der Seite **Training** des Custom Speech-Portals. |
| `{ENDPOINT_URI}` | Der Endpunkt ist zur Messung und Abrechnung erforderlich. Weitere Informationen finden Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters). |
| `{API_KEY}` | Der API-Schlüssel ist erforderlich. Weitere Informationen finden Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters). |

Zum Ausführen des Containers für die *benutzerdefinierte Spracherkennung* führen Sie den folgenden `docker run`-Befehl aus:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen Container für die *benutzerdefinierte Spracherkennung* aus dem Containerimage aus.
* Ordnet 4 CPU-Kerne und 4 GB Arbeitsspeicher zu.
* Lädt das Modell für die *benutzerdefinierte Spracherkennung* aus der Volumebereitstellung für die Eingabe, z. B. *C:\CustomSpeech*.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Lädt das Modell anhand der `ModelId` herunter (sofern diese in der Volumebereitstellung nicht gefunden wird).
* Wenn das benutzerdefinierte Modell zuvor bereits heruntergeladen wurde, wird die `ModelId` ignoriert.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

# <a name="text-to-speechtabtts"></a>[Sprachsynthese](#tab/tts)

Zum Ausführen des Containers für die *Sprachsynthese* führen Sie den folgenden `docker run`-Befehl aus.

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen Container für die *Sprachsynthese* aus dem Containerimage aus.
* Ordnet 2 CPU-Kerne und 1 GB Arbeitsspeicher zu.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

# <a name="custom-text-to-speechtabctts"></a>[Benutzerdefinierte Sprachsynthese](#tab/ctts)

Der Container für die *benutzerdefinierte Sprachsynthese* basiert auf einem benutzerdefinierten Sprachmodell. Das benutzerdefinierte Modell muss über das [Custom Voice-Portal](https://aka.ms/custom-voice-portal) [trainiert](how-to-custom-voice-create-voice.md) worden sein. Die **Modell-ID** für Custom Voice ist zur Ausführung des Containers erforderlich. Sie finden diese auf der Seite **Training** des Custom Voice-Portals. Navigieren Sie im Custom Voice-Portal zur Seite **Training**, und wählen Sie das Modell aus.
<br>

![Seite „Training“ im Custom Voice-Portal](media/custom-voice/custom-voice-model-training.png)

Rufen Sie die **Modell-ID** ab, um diese als Argument für den `ModelId`-Parameter des docker run-Befehls zu verwenden.
<br>

![Details des Custom Voice-Modells](media/custom-voice/custom-voice-model-details.png)

Die folgende Tabelle zeigt die verschiedenen `docker run`-Parameter und die entsprechenden Beschreibungen:

| Parameter | BESCHREIBUNG |
|---------|---------|
| `{VOLUME_MOUNT}` | Die [Volumebereitstellung](https://docs.docker.com/storage/volumes/) des Hostcomputers, die Docker zum dauerhaften Speichern des benutzerdefinierten Modells verwendet. Beispiel: *C:\CustomSpeech*, wobei sich *Laufwerk „C“* auf dem Hostcomputer befindet. |
| `{MODEL_ID}` | Die **Modell-ID** für Custom Speech von der Seite **Training** des Custom Voice-Portals. |
| `{ENDPOINT_URI}` | Der Endpunkt ist zur Messung und Abrechnung erforderlich. Weitere Informationen finden Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters). |
| `{API_KEY}` | Der API-Schlüssel ist erforderlich. Weitere Informationen finden Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters). |

Zum Ausführen des Containers für die *benutzerdefinierte Sprachsynthese* führen Sie den folgenden `docker run`-Befehl aus:

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen Container für die *benutzerdefinierte Sprachsynthese* aus dem Containerimage aus.
* Ordnet 2 CPU-Kerne und 1 GB Arbeitsspeicher zu.
* Lädt das Modell für die *benutzerdefinierte Sprachsynthese* aus der Volumebereitstellung für die Eingabe, z. B. *C:\CustomVoice*.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Lädt das Modell anhand der `ModelId` herunter (sofern diese in der Volumebereitstellung nicht gefunden wird).
* Wenn das benutzerdefinierte Modell zuvor bereits heruntergeladen wurde, wird die `ModelId` ignoriert.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

***

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Abfragen des Vorhersageendpunkts des Containers

| Container | Endpunkt | Protocol |
|--|--|--|
| Spracherkennung | `ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1` | WS |
| Benutzerdefinierte Spracherkennung | `ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1` | WS |
| Text-zu-Sprache | `http://localhost:5000/speech/synthesize/cognitiveservices/v1` | HTTP |
| Benutzerdefinierte Sprachsynthese | `http://localhost:5000/speech/synthesize/cognitiveservices/v1` | HTTP |

Weitere Informationen zur Verwendung der Protokolle WSS und HTTPS finden Sie unter [Containersicherheit](../cognitive-services-container-support.md#azure-cognitive-services-container-security).

[!INCLUDE [Query Speech-to-text container endpoint](includes/speech-to-text-container-query-endpoint.md)]

### <a name="text-to-speech-or-custom-text-to-speech"></a>Sprachsynthese oder benutzerdefinierte Sprachsynthese

[!INCLUDE [Query Text-to-speech container endpoint](includes/text-to-speech-container-query-endpoint.md)]

### <a name="run-multiple-containers-on-the-same-host"></a>Ausführen mehrerer Container auf dem gleichen Host

Wenn Sie beabsichtigen, mehrere Container mit offengelegten Ports auszuführen, stellen Sie sicher, dass jeder Container mit einem anderen offengelegten Port ausgeführt wird. Führen Sie beispielsweise den ersten Container an Port 5000 und den zweiten Container an Port 5001 aus.

Sie können diesen Container und einen anderen Azure Cognitive Services-Container zusammen auf dem Host ausführen. Sie können auch mehrere Container desselben Cognitive Services-Containers ausführen.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Beenden des Containers

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problembehandlung

Beim Starten oder Ausführen des Containers können Probleme auftreten. Verwenden Sie eine [Bereitstellung](speech-container-configuration.md#mount-settings) für die Ausgabe, und aktivieren Sie die Protokollierung. Auf diese Weise kann der Container Protokolldateien generieren, die bei der Behebung von Problemen helfen.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Abrechnung

Der Speech-Container sendet Abrechnungsinformationen an Azure und verwendet dafür eine Ressource vom Typ *Speech* in Ihrem Azure-Konto.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](speech-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Speech-Containern kennengelernt. Zusammenfassung:

* Speech stellt vier Linux-Container für Docker bereit, die verschiedene Funktionen kapseln:
  * *Spracherkennung*
  * *Benutzerdefinierte Spracherkennung*
  * *Sprachsynthese*
  * *Benutzerdefinierte Sprachsynthese*
* Containerimages werden aus der Containerregistrierung in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Speech-Containern über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
>  Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (z.B. das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich unter [Konfigurieren von Containern](speech-container-configuration.md) über Konfigurationseinstellungen.
* Erfahren Sie, wie Sie [Container für den Speech-Dienst mit Kubernetes und Helm verwenden](speech-container-howto-on-premises.md).
* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)
