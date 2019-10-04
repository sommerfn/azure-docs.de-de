---
title: Verwenden von Docker Compose zum Bereitstellen mehrerer Container
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mehrere Cognitive Services-Container bereitstellen. In diesem Artikel wird beschrieben, wie Sie mehrere Docker-Containerimages mit Docker Compose orchestrieren.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 95ec80af88e0b89f61bebed08f4b96a09947f401
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311552"
---
# <a name="use-docker-compose-to-deploy-multiple-containers"></a>Verwenden von Docker Compose zum Bereitstellen mehrerer Container

In diesem Artikel wird beschrieben, wie Sie mehrere Azure Cognitive Services-Container bereitstellen. Vor allem erfahren Sie, wie Sie Docker Compose zum Orchestrieren von mehreren Docker-Containerimages verwenden.

> [Docker Compose](https://docs.docker.com/compose/) ist ein Tool zum Definieren und Ausführen von Docker-Anwendungen mit mehreren Containern. In Compose verwenden Sie eine YAML-Datei, um die Dienste Ihrer Anwendung zu konfigurieren. Anschließend erstellen und starten Sie alle Dienste Ihrer Konfiguration mit nur einem Befehl.

Es kann hilfreich sein, mehrere Containerimages auf nur einem Hostcomputer zu orchestrieren. In diesem Artikel werden die Container „Texterkennung“ und „Formularerkennung“ zusammengeführt.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Verfahren müssen mehrere Tools lokal installiert und ausgeführt werden:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* [Docker-Engine](https://www.docker.com/products/docker-engine). Vergewissern Sie sich, dass die Docker-CLI in einem Konsolenfenster funktioniert.
* Eine Azure-Ressource mit dem korrekten Tarif. Für diesen Container können nur die folgenden Tarife genutzt werden:
  * Ressource **Maschinelles Sehen** nur mit F0- oder Standard-Tarif.
  * Ressource **Formularerkennung** nur mit F0- oder Standard-Tarif.
  * Ressource **Cognitive Services** mit dem S0-Tarif.

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Füllen Sie das [Formular zum Anfordern von Cognitive Services-Speech-Containern](https://aka.ms/speechcontainerspreview/) aus, und senden Sie es ab. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Docker Compose-Datei

Die YAML-Datei definiert alle bereitzustellenden Dienste. Diese Dienste basieren entweder auf einer `DockerFile` oder einem vorhandenen Containerimage. In diesem Fall verwenden wir zwei Vorschauimages. Kopieren Sie die folgende YAML-Datei, fügen Sie sie ein, und speichern Sie sie als *docker-compose.yaml*. Geben Sie die entsprechenden Werte für **apikey**, **billing** und **EndpointUri** in die Datei ein.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: E:\publicpreview\output
         target: /output
       - type: bind
         source: E:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: # < Your recognize text API key >
      billing: # < Your recognize text billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> Erstellen Sie die Verzeichnisse auf dem Hostcomputer, die unter dem Knoten **volumes** angegeben sind. Dieser Ansatz ist erforderlich, weil die Verzeichnisse vorhanden sein müssen, bevor Sie versuchen, ein Image mithilfe von Volumebindungen bereitzustellen.

## <a name="start-the-configured-docker-compose-services"></a>Starten der konfigurierten Docker Compose-Dienste

Eine Docker Compose-Datei ermöglicht die Verwaltung aller Stufen des Lebenszyklus eines definierten Diensts: Starten, Beenden und Neuerstellen der Dienste, Anzeigen des Dienststatus und Durchführen des Protokollstreamings. Öffnen Sie über das Projektverzeichnis (in dem sich die Datei „docker-compose.yaml“ befindet) eine Befehlszeilenschnittstelle.

> [!NOTE]
> Stellen Sie zur Vermeidung von Fehlern sicher, dass der Hostcomputer die Laufwerke korrekt für die Docker-Engine freigibt. Wenn beispielsweise „E:\publicpreview“ als Verzeichnis in der Datei „docker-compose.yaml“ verwendet wird, geben Sie das Laufwerk „E“ für Docker frei.

Führen Sie über die Befehlszeilenschnittstelle den folgenden Befehl aus, um alle in der Datei „docker-compose.yaml“ definierten Dienste zu starten (oder neu zu starten):

```console
docker-compose up
```

Wenn der Befehl **docker-compose up** von Docker mit dieser Konfiguration zum ersten Mal ausgeführt wird, werden die unter dem Knoten **services** konfigurierten Images per Pullvorgang abgerufen und dann heruntergeladen und bereitgestellt:

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:)...
latest: Pulling from microsoft/cognitive-services-recognize-text
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

Nachdem die Images heruntergeladen wurden, werden die Imagedienste gestartet:

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Überprüfen der Dienstverfügbarkeit

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

Hier ist eine Beispielausgabe angegeben:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text    latest
```

### <a name="test-the-recognize-text-container"></a>Testen des Containers „Texterkennung“

Öffnen Sie einen Browser auf dem Hostcomputer, und navigieren Sie zu **localhost**, indem Sie den angegebenen Port aus der Datei „docker-compose.yaml“ verwenden, z. B. http://localhost:5021/swagger/index.html. Sie können die Funktion „Testen“ der API verwenden, um den Endpunkt von „Texterkennung“ zu testen.

![Container „Texterkennung“](media/recognize-text-swagger-page.png)

### <a name="test-the-form-recognizer-container"></a>Testen des Containers „Formularerkennung“

Öffnen Sie einen Browser auf dem Hostcomputer, und navigieren Sie zu **localhost**, indem Sie den angegebenen Port aus der Datei „docker-compose.yaml“ verwenden, z. B. http://localhost:5010/swagger/index.html. Sie können die Funktion „Testen“ der API verwenden, um den Endpunkt von „Formularerkennung“ zu testen.

![Container „Formularerkennung“](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Containerunterstützung in Azure Cognitive Services](../cognitive-services-container-support.md)
