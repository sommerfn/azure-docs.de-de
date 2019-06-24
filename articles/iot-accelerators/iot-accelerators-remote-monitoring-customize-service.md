---
title: Hinzufügen eines Diensts zur Benutzeroberfläche der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel zeigt Ihnen, wie Sie einen neuen Dienst zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447045"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Hinzufügen eines benutzerdefinierten Diensts zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung

Dieser Artikel zeigt Ihnen, wie Sie einen neuen Dienst zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können. Der Artikel beschreibt Folgendes:

- Vorbereiten einer lokalen Entwicklungsumgebung.
- Hinzufügen eines neuen Diensts zur Webbenutzeroberfläche.

Der Beispieldienst in diesem Artikel stellt die Daten für ein Raster zur Verfügung, das Sie anhand des Artikels [Hinzufügen eines benutzerdefinierten Dienstes zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-grid.md) hinzugefügt haben.

In einer React-Anwendung interagiert ein Dienst normalerweise mit einem Back-End-Dienst. Zu den Beispielen im Solution Accelerator für die Remoteüberwachung gehören Dienste, die mit den IoT-Hub-Manager- und Konfiguration-Microservices interagieren.

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte in diesem Leitfaden abzuschließen, muss die folgende Software auf dem lokalen Entwicklungscomputer installiert sein:

- [Git-Client](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Vorbereitung

Sie sollten die Schritte im Gewusst-wie-Artikel [Add a custom page to the Remote Monitoring solution accelerator web UI](iot-accelerators-remote-monitoring-customize-page.md) (Hinzufügen einer benutzerdefinierten Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung) ausführen, bevor Sie fortfahren.

## <a name="add-a-service"></a>Hinzufügen eines Diensts

Um einen Dienst zur Webbenutzeroberfläche hinzuzufügen, müssen Sie die Quelldateien hinzufügen, die den Dienst definieren, und einige vorhandene Dateien ändern, um die Webbenutzeroberfläche über den neuen Dienst zu informieren.

### <a name="add-the-new-files-that-define-the-service"></a>Hinzufügen der neuen Dateien, die den Dienst definieren

Für den Einstieg enthält der Ordner **src/walkthrough/services** die Dateien zur Definition eines einfachen Diensts:

**exampleService.js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Weitere Informationen über die Art der Implementierung von Diensten finden Sie unter [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) (Die Einführung in reaktive Programmierung, die Ihnen bisher gefehlt hat).

**model/exampleModels.js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

Kopieren Sie **exampleService.js** in den Ordner **src/services**, und kopieren Sie **exampleModels.js** in den Ordner **src/services/models**.

Aktualisieren Sie die Datei **index.js** im Ordner **src/services**, um den neuen Dienst zu exportieren:

```js
export * from './exampleService';
```

Aktualisieren Sie die Datei **index.js** im Ordner **src/services/models**, um das neue Modell zu exportieren:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Einrichten der Aufrufe an den Dienst aus dem Store

Um Ihnen den Einstieg zu erleichtern, enthält der Ordner **src/walkthrough/store/reducers** einen Beispielreducer:

**exampleReducer.js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

Kopieren Sie **exampleReducer.js** in den Ordner **src/store/reducers**.

Weitere Informationen über den Reducer und **Epics** finden Sie unter [redux-observable](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>Konfigurieren der Middleware

Fügen Sie zum Konfigurieren der Middleware den Reducer zur Datei **rootReducer.js** im Ordner **src/store** hinzu:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

Fügen Sie die Epics der Datei **rootEpics.js** im Ordner **src/store** hinzu:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie Dienste zur Webbenutzeroberfläche im Solution Accelerator für die Remoteüberwachung hinzufügen oder diese anpassen können.

Nachdem Sie einen Dienst definiert haben, ist der nächste Schritt das [Hinzufügen eines benutzerdefinierten Rasters zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-grid.md), das die vom Dienst zurückgegebenen Daten anzeigt.

Weitere konzeptuelle Informationen zum Solution Accelerator für die Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).
