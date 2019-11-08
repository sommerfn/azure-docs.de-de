---
title: Installieren und Ausführen von Containern – Textübersetzung
titleSuffix: Azure Cognitive Services
description: In diesem Schritt-für-Schritt-Tutorial erhalten Sie Informationen zum Herunterladen, Installieren und Ausführen von Containern für die Textübersetzung.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 7b2b07f71d00e6da66062d1826f8c5c88bb6be7a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506821"
---
# <a name="install-and-run-translator-text-containers"></a>Installieren und Ausführen von Containern für die Textübersetzung

<!--
    ACOM forward link:
    https://docs.microsoft.com/azure/cognitive-services/translator/howto-install-containers
-->

Container ermöglichen eine Ausführung der Textübersetzungs-APIs in Ihrer eigenen Umgebung und eignen sich hervorragend für spezifische Anforderungen bezüglich Sicherheit und Datengovernance.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung von Containern für die Textübersetzung müssen die folgenden Voraussetzungen erfüllt sein:

|Erforderlich|Zweck|
|--|--|
|Docker-Engine| Die Docker-Engine muss auf einem [Hostcomputer](#the-host-computer) installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).<br><br> **Unter Windows** muss Docker auch für die Unterstützung von Linux-Containern konfiguriert werden.<br><br>|
|Kenntnisse zu Docker | Sie sollten über Grundkenntnisse der Konzepte von Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen.|

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern von Containern für die Textübersetzung](https://aka.ms/translatorcontainerform) ausfüllen und übermitteln, um Zugriff auf den Container anzufordern.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

### <a name="the-host-computer"></a>Der Hostcomputer

Der Host ist ein x64-basierter Computer mit einem Linux-Betriebssystem, auf dem der Docker-Container ausgeführt wird. Dies kann ein lokaler Computer oder ein Docker-Hostingdienst in Azure sein, z. B.:

* Azure Kubernetes Service
* Azure Container Instances
* Ein in Azure Stack bereitgestellter [Kubernetes](https://kubernetes.io/)-Cluster

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

In der folgenden Tabelle werden die Mindestanforderungen und Empfehlungen für CPU-Kerne (mindestens 2,6 GHz) und Arbeitsspeicher in Gigabyte (GB) angegeben, die für jeden Container für die Textübersetzung zugewiesen werden müssen.

| Container | Minimum | Language pair (Sprachpaar) |
|-----------|---------|---------------|
| Textübersetzung | 4 Kerne, 4 GB Arbeitsspeicher | 4 |

Es wird empfohlen, für jedes Sprachpaar 1 GB an Arbeitsspeicher bereitzustellen. Standardmäßig enthält der Container für die Textübersetzung abhängig vom ausgeführten `<image-tag>` 3 oder 4 Sprachpaare. Weitere Informationen finden Sie unter [Unterstützte Sprachen und Übersetzung](#supported-languages-and-translation). Kernanzahl und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.

## <a name="get-the-container-image-with-docker-pull"></a>Abrufen des Containerimages mit `docker pull`

Containerimages für die Textübersetzung stehen im folgenden Containerrepository zur Verfügung. Die Tabelle zeigt außerdem die Containerimagetags und die entsprechenden unterstützten Sprachen.

| Container | Imagetag | Unterstützte Sprachen |
|-----------|-----------|---------------------|
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `ar_de_en_ru_zh_1.0.0` | `ar-SA`, `zh-CN`, `de-DE` und `ru-RU` |
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `de_en_es_fr_1.0.0` | `de-DE`, `fr-FR` und `es-ES` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-translator-text-container"></a>docker pull-Befehl für den Container für die Textübersetzung

Um den [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)-Befehl auszuführen, benötigen Sie zunächst Zugriff auf die Containerregistrierung. Mithilfe der Azure CLI können Sie sich über den [`az acr login`](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-login)-Befehl bei der Azure Container Registry-Instanz anmelden.

```azureinteractive
az acr login --name containerpreview.azurecr.io
```

Dieser Befehl authentifiziert Ihren aktuellen Benutzer bei der entsprechenden Azure Container Registry-Instanz. Anschließend können Sie den `docker pull`-Befehl ausführen.

> [!NOTE]
> Je nach benötigter Sprachunterstützung geben Sie das entsprechende `<image-tag>` an.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:<image-tag>
```

## <a name="how-to-use-the-container"></a>Verwenden des Containers

Wenn sich der Container auf dem [Hostcomputer](#the-host-computer) befindet, können Sie über den folgenden Prozess mit dem Container arbeiten.

1. [Führen Sie den Container aus](#run-the-container-with-docker-run). Es sind noch weitere [Beispiele](translator-text-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.
1. [Fragen Sie den Übersetzungsendpunkt des Containers ab](#query-the-containers-translate-endpoint).

## <a name="run-the-container-with-docker-run"></a>Ausführen des Containers mit `docker run`

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen der drei Container auszuführen. Es sind [Beispiele](translator-text-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.

### <a name="run-container-example-of-docker-run-command"></a>Beispiel zum Ausführen eines Containers mit dem „docker run“-Befehl

```Docker
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:ar_de_en_ru_zh_1.0.0 \
Eula=accept
```

Dieser Befehl:

* Führt einen Container für die Textübersetzung aus dem Containerimage aus.
* Ordnet 4 CPU-Kerne und 4 GB Arbeitsspeicher zu.
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Akzeptiert die Endbenutzer-Lizenzbedingungen.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist weiterhin auf dem Hostcomputer verfügbar.

### <a name="how-to-collect-docker-logs"></a>Erfassen von Docker-Protokollen

Im Rahmen der Problembehandlung kann es sinnvoll sein, sich die Docker-Protokolle für die Containerausführung anzusehen. Führen Sie zuerst den Befehl [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) mit dem Formatierungsflag aus, um die für alle Container angezeigten Details einzuschränken.

```Docker
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Status}}"

CONTAINER ID    IMAGE                                                                       STATUS
ed6f115697f3    containerpreview.azurecr.io/microsoft/cognitive-services-translator-text    Up 4 minutes
```

Führen Sie dann den Befehl [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) mit der `<Container ID>` für den gewünschten Container aus, um die zugehörigen Protokolle anzuzeigen.

```Docker
docker logs <Container ID> --timestamps --since=4h | grep Error
```

Der obige docker logs-Befehl erfasst die Fehlerprotokolle der letzten vier Stunden.

## <a name="supported-languages-and-translation"></a>Unterstützte Sprachen und Übersetzung

Die `POST /translate`-Methode unterstützt die folgenden Sprachumwandlungen, von *Englisch* in eine Zielsprache und umgekehrt. Beachten Sie, dass Sie zwar aus dem Englischen in eine der aufgeführten Sprachen und umgekehrt übersetzen können, *nicht jedoch* von einer *nicht englischen* Sprache in eine andere *nicht englische* Sprache.

> [!NOTE]
> Für optimale Qualität sollten Consumer nur einen Satz pro Anforderung senden.

| Sprachumwandlung | Umwandlung der ISO-Codes | Bildtags |
|--|--|:--|
| Englisch :left_right_arrow: Chinesisch | `en-US` :left_right_arrow: `zh-CN` | `ar_de_en_ru_zh_1.0.0` |
| Englisch :left_right_arrow: Russisch | `en-US` :left_right_arrow: `ru-RU` | `ar_de_en_ru_zh_1.0.0` |
| Englisch :left_right_arrow: Arabisch | `en-US` :left_right_arrow: `ar-SA` | `ar_de_en_ru_zh_1.0.0` |
| Englisch :left_right_arrow: Deutsch | `en-US` :left_right_arrow: `de-DE` | `ar_de_en_ru_zh_1.0.0` und `de_en_es_fr_1.0.0` |
| Englisch :left_right_arrow: Spanisch | `en-US` :left_right_arrow: `es-ES` | `de_en_es_fr_1.0.0` |
| Englisch :left_right_arrow: Französisch | `en-US` :left_right_arrow: `fr-FR` | `de_en_es_fr_1.0.0` |

> [!IMPORTANT]
> Die `Eula` für die Containerausführung müssen angegeben werden, andernfalls wird der Container nicht gestartet.

## <a name="query-the-containers-translate-endpoint"></a>Abfrage des Übersetzungsendpunkts des Containers

Der Container stellt eine REST-basierte Übersetzungsendpunkt-API bereit. Nachstehend werden einige Nutzungsbeispiele für diesen Endpunkt aufgeführt:

# <a name="curltabcurl"></a>[cURL](#tab/curl)


Führen Sie den folgenden cURL-Befehl über Ihre bevorzugte CLI aus.

```curl
curl -X POST "http://localhost:5000/translate?api-version=3.0&from=en-US&to=de-DE"
    -H "Content-Type: application/json" -d "[{'Text':'hello, how are you'}]"
```

> [!TIP]
> Wenn Sie diesen cURL POST-Befehl ausführen, bevor der Container bereit ist, erhalten Sie eine Antwort vom Typ „Der Dienst ist vorübergehend nicht verfügbar“. Warten Sie einfach, bis der Container bereit ist, und versuchen Sie es dann noch mal.

Die folgende Ausgabe wird an die Konsole ausgegeben.

```console
"translations": [
    {
        "text": "hallo, wie geht es dir",
        "to": "de-DE"
    }
]
```

# <a name="swaggertabswagger"></a>[Swagger](#tab/Swagger)

Navigieren Sie zur Swagger-Seite http://localhost:5000/swagger/index.html.

1. Wählen Sie **POST /translate** aus.
1. Klicken Sie auf **Try it out** (Ausprobieren).
1. Geben Sie für den **From**-Parameter den Wert `en-US` ein.
1. Geben Sie für den **To**-Parameter den Wert `de-DE` ein.
1. Geben Sie für den **api-version**-Parameter den Wert `3.0` ein.
1. Ersetzen Sie unter **texts** den folgenden `string`-Wert durch den unten folgenden JSON-Code:
    ```json
    [
        {
            "text": "hello, how are you"
        }
    ]
    ```
1. Klicken Sie auf **Execute** (Ausführen). Die Übersetzungen werden im **Response Body** (Antworttext) ausgegeben. Es sollte ein Text ähnlich dem folgenden angezeigt werden:
    ```json
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

# <a name="net-coretabnetcore"></a>[.NET Core](#tab/netcore)

1. Starten Sie Visual Studio, und erstellen Sie eine neue Konsolenanwendung.
1. Bearbeiten Sie das Feld `*.csproj`, um den `<LangVersion>7.1</LangVersion>`-Knoten hinzuzufügen – hiermit wird C# 7.1 angegeben.
1. Fügen Sie das NuGet-Paket [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) der Version 11.0.2 hinzu.
1. Ersetzen Sie in `Program.cs` den vorhandenen Code durch den folgenden:
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    namespace TranslateContainer
    {
        class Program
        {
            const string ApiHostEndpoint = "http://localhost:5000";
            const string TranslateApi = "/translate?api-version=3.0&from=en-US&to=de-DE";

            static async Task Main(string[] args)
            {
                var textToTranslate = "hello, how are you";
                var result = await TranslateTextAsync(textToTranslate);

                Console.WriteLine(result);
                Console.ReadLine();
            }

            static async Task<string> TranslateTextAsync(string textToTranslate)
            {
                var body = new object[] { new { Text = textToTranslate } };
                var requestBody = JsonConvert.SerializeObject(body);

                var client = new HttpClient();
                using (var request =
                    new HttpRequestMessage
                    {
                        Method = HttpMethod.Post,
                        RequestUri = new Uri($"{ApiHostEndpoint}{TranslateApi}"),
                        Content = new StringContent(requestBody, Encoding.UTF8, "application/json")
                    })
                {
                    // Send the request and await a response.
                    var response = await client.SendAsync(request);

                    return await response.Content.ReadAsStringAsync();
                }
            }
        }
    }
    ```
1. Drücken Sie die Taste **F5** , um das Programm auszuführen.
1. Die folgende Ausgabe wird an die Konsole ausgegeben.
    ```console
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

***

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Beenden des Containers

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie den Container mit einer [Ausgabenbereitstellung](translator-text-container-config.md#mount-settings) ausführen und die Protokollierung aktiviert ist, generiert der Container Protokolldateien. Diese sind hilfreich, um Probleme beim Starten oder Ausführen des Containers zu beheben.

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für die Textübersetzung kennengelernt. Zusammenfassung:

* Textübersetzung bietet mehrere Linux-Container für Docker mit verschiedenen Sprachpaaren.
* Containerimages werden aus der Registrierung „Containervorschau“ heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Containern für die Textübersetzung über den Host-URI des Containers aufzurufen.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](translator-text-container-config.md).
* Unter [Häufig gestellte Fragen (FAQ) zu Containern](../containers/container-faq.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktion.
