---
title: Installieren und Ausführen von Containern – Textanalyse
titleSuffix: Azure Cognitive Services
description: Informationen zum Herunterladen, Installieren und Ausführen von Containern für die Textanalyse in diesem Schritt-für-Schritt-Tutorial.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: dapine
ms.openlocfilehash: ddbe586c03d9f722d844d06968aa25e4b4a5aac0
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815298"
---
# <a name="install-and-run-text-analytics-containers"></a>Installieren und Ausführen von Containern für die Textanalyse

Container ermöglichen es Ihnen, die Textanalyse-APIs in ihrer eigenen Umgebung auszuführen und eignen sich hervorragend für Ihre spezifischen Anforderungen bezüglich Sicherheit und Datengovernance. Container für die Textanalyse ermöglichen eine erweiterte Verarbeitung von natürlicher Sprache anhand von unformatiertem Text und bieten drei Hauptfunktionen: Standpunktanalyse, Schlüsselbegriffserkennung und Sprachenerkennung. Die Entitätsverknüpfung wird derzeit in Containern nicht unterstützt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen eines Containers für die Textanalyse benötigen Sie den Hostcomputer und Containerumgebungen.

## <a name="preparation"></a>Vorbereitung

Zur Verwendung des Containers für die Textanalyse müssen die folgenden Voraussetzungen erfüllt sein:

|Erforderlich|Zweck|
|--|--|
|Docker-Engine| Die Docker-Engine muss auf einem [Hostcomputer](#the-host-computer) installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können. <br><br> **Unter Windows** muss Docker auch für die Unterstützung von Linux-Containern konfiguriert werden.<br><br>|
|Kenntnisse zu Docker | Sie sollten über Grundkenntnisse der Konzepte von Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen.| 
|Textanalyseressource |Um den Container zu verwenden, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ [Textanalyse](../../cognitive-services-apis-create-account.md), um den entsprechenden API-Schlüssel und den URI des Endpunkts zu erhalten. Beide Werte stehen im Azure-Portal auf der Übersichts- und auf der Schlüsselseite der Textanalyse zur Verfügung und werden zum Starten des Containers benötigt.<br><br>**{API_KEY}** : Einer der beiden verfügbaren Ressourcenschlüssel auf der Seite **Schlüssel**<br><br>**{ENDPOINT_URI}** : Der Endpunkt, der auf der **Übersichtsseite** angegeben ist|

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Der Hostcomputer

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

In der folgenden Tabelle werden die Mindestanforderungen und Empfehlungen für CPU-Kerne (mindestens 2,6 GHz) und Arbeitsspeicher in Gigabyte (GB) angegeben, die für jeden Container für die Textanalyse zugewiesen werden müssen.

| Container | Minimum | Empfohlen | TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
|Schlüsselwortextraktion | Ein Kern, 2 GB Arbeitsspeicher | Ein Kern, 4 GB Arbeitsspeicher |15, 30|
|Spracherkennung | Ein Kern, 2 GB Arbeitsspeicher | Ein Kern, 4 GB Arbeitsspeicher |15, 30|
|Standpunktanalyse | Ein Kern, 2 GB Arbeitsspeicher | Ein Kern, 4 GB Arbeitsspeicher |15, 30|

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.
* TPS: Transaktionen pro Sekunde

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.

## <a name="get-the-container-image-with-docker-pull"></a>Abrufen des Containerimages mit `docker pull`

In der Microsoft Container Registry stehen Containerimages für die Textanalyse zur Verfügung.

| Container | Repository |
|-----------|------------|
|Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
|Standpunktanalyse| `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Container für die Textanalyse finden Sie in den folgenden Containern im Docker-Hub:

* [Schlüsselbegriffserkennung](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Standpunktanalyse](https://go.microsoft.com/fwlink/?linkid=2018654)

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage herunterzuladen.

### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker-Pullvorgang für den Schlüsselbegriffserkennungs-Container

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>Docker-Pullvorgang für den Sprachenerkennungscontainer

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-container"></a>Docker-Pullvorgang für den Stimmungscontainer

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Verwenden des Containers

Wenn sich der Container auf dem [Hostcomputer](#the-host-computer) befindet, können Sie über den folgenden Prozess mit dem Container arbeiten.

1. [Führen Sie den Container aus](#run-the-container-with-docker-run), und verwenden Sie dabei die erforderlichen Abrechnungseinstellungen. Es sind noch weitere [Beispiele](../text-analytics-resource-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.
1. [Fragen Sie den Vorhersageendpunkt des Containers ab.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Ausführen des Containers mit `docker run`

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen der drei Container auszuführen. Genaue Informationen dazu, wie Sie die Werte `{Endpoint_URI}` und `{API_Key}` abrufen, erhalten Sie unter [Gathering required parameters (Ermitteln der erforderlichen Parameter)](#gathering-required-parameters).

Es sind [Beispiele](../text-analytics-resource-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.

### <a name="run-container-example-of-docker-run-command"></a>Beispiel zum Ausführen eines Containers mit dem „docker run“-Befehl

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen Schlüsselbegriffserkennungs-Container auf der Grundlage des Containerimages aus.
* Weist einen einzelnen CPU-Kern und 4 GB Arbeitsspeicher zu
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.


> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Abfragen des Vorhersageendpunkts des Containers

Der Container stellt REST-basierte Endpunkt-APIs für die Abfragevorhersage bereit.

Verwenden Sie für Container-APIs den Host `https://localhost:5000`.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Beenden des Containers

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie den Container mit einer [Ausgabenbereitstellung](../text-analytics-resource-container-config.md#mount-settings) ausführen und die Protokollierung aktiviert ist, generiert der Container Protokolldateien. Diese sind hilfreich, um Probleme beim Starten oder Ausführen des Containers zu beheben.

## <a name="billing"></a>Abrechnung

Der Container für die Textanalyse sendet Abrechnungsinformationen an Azure und verwendet dafür eine Ressource vom Typ _Textanalyse_ in Ihrem Azure-Konto. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](../text-analytics-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für die Textanalyse kennengelernt. Zusammenfassung:

* Die Textanalyse stellt drei Linux-Container für Docker bereit: Schlüsselbegriffserkennung und Sprachenerkennung und Standpunktanalyse.
* Containerimages werden aus Microsoft Container Registry (MCR) in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Containern für die Textanalyse über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (z.B. das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](../text-analytics-resource-container-config.md).
* Unter [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktion.
