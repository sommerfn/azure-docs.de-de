---
title: Hinzufügen eines Flyouts zur Benutzeroberfläche der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel zeigt Ihnen, wie Sie ein neues Flyout auf einer Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: ccb1a7ff6abbc68f42c7632a8ba7a392b2c48794
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447113"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Hinzufügen eines benutzerdefinierten Flyouts zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung

Dieser Artikel zeigt Ihnen, wie Sie ein neues Flyout auf einer Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können. Der Artikel beschreibt Folgendes:

- Vorbereiten einer lokalen Entwicklungsumgebung.
- Hinzufügen eines neuen Flyouts zu einer Seite in der Webbenutzeroberfläche.

Das Beispielflyout in diesem Artikel wird auf der Seite mit dem Raster angezeigt, über dessen Hinzufügen Sie mehr im Gewusst-wie-Artikel [Hinzufügen eines benutzerdefinierten Rasters zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-grid.md) erfahren.

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte in diesem Leitfaden abzuschließen, muss die folgende Software auf dem lokalen Entwicklungscomputer installiert sein:

- [Git-Client](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Vorbereitung

Sie sollten die Schritte in den folgenden Artikeln ausführen, bevor Sie fortfahren:

- [Hinzufügen einer benutzerdefinierten Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-page.md).
- [Hinzufügen eines benutzerdefinierten Dienstes zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-service.md).
- [Hinzufügen eines benutzerdefinierten Rasters zur Webbenutzeroberfläche des Accelerators für Remoteüberwachung](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Hinzufügen eines Flyouts

Um ein Flyout zur Webbenutzeroberfläche hinzuzufügen, müssen Sie die Quelldateien hinzufügen, die das Flyout definieren, und einige vorhandene Dateien ändern, um die Webbenutzeroberfläche über die neue Komponente zu informieren.

### <a name="add-the-new-files-that-define-the-flyout"></a>Hinzufügen der neuen Dateien, die das Flyout definieren

Für die ersten Schritte finden Sie im Ordner **src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout** Dateien, die ein Flyout definieren:

**exampleFlyout.container.js**

[!code-javascript[Example flyout container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example flyout container")]

**exampleFlyout.js**

[!code-javascript[Example flyout](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example flyout")]

Kopieren Sie den Ordner **src/walkthrough/components/pages/pageWithFlyout/flyouts** in den Ordner **src/components/pages/example**.

### <a name="add-the-flyout-to-the-page"></a>Hinzufügen des Flyouts zur Seite

Ändern Sie die Datei **src/components/pages/example/basicPage.js**, um das Flyout hinzuzufügen.

Fügen Sie **Btn** den Imports aus **components/shared** hinzu, und fügen Sie Imports für **svgs** und **ExampleFlyoutContainer** hinzu:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

Fügen Sie eine **const**-Definition für **closedFlyoutState** hinzu, und fügen Sie sie dem Status im Konstruktor hinzu:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

Fügen Sie die folgenden Funktionen zur Klasse **BasicPage** hinzu:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

Fügen Sie die folgenden **const**-Definitionen zur Funktion **render** hinzu:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Fügen Sie eine Schaltfläche hinzu, um das Flyout für das Kontextmenü zu öffnen:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Fügen Sie Text und den Flyoutcontainer zum Seiteninhalt hinzu:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>Testen des Flyouts

Wenn die Webbenutzeroberfläche nicht lokal ausgeführt wird, führen Sie den folgenden Befehl im Stammverzeichnis Ihrer lokalen Kopie im Repository durch:

```cmd/sh
npm start
```

Der vorherige Befehl führt die Benutzeroberfläche lokal unter [http://localhost:3000/dashboard](http://localhost:3000/dashboard) aus. Navigieren Sie zur Seite **Beispiel**, und klicken Sie auf **Flyout öffnen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie die Seiten Webbenutzeroberfläche im Solution Accelerator für die Remoteüberwachung hinzufügen oder anpassen können.

Nachdem Sie nun ein Flyout auf einer Seite definiert haben, besteht der nächste Schritt im [Hinzufügen eines Panels zum Dashboard auf der Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-panel.md).

Weitere konzeptuelle Informationen zum Solution Accelerator für die Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).
