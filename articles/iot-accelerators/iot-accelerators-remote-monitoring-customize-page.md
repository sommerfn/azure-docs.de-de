---
title: Hinzufügen einer Seite zur Benutzeroberfläche der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel zeigt Ihnen, wie Sie eine neue Seite auf der Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: ec0b9fbdfdb96317e1e7f6fe00384ba4f8c42bcc
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607948"
---
# <a name="add-a-custom-page-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Hinzufügen einer benutzerdefinierten Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung

Dieser Artikel zeigt Ihnen, wie Sie eine neue Seite in die Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung hinzufügen können. Der Artikel beschreibt Folgendes:

- Vorbereiten einer lokalen Entwicklungsumgebung.
- Hinzufügen einer neuen Seite zur Webbenutzeroberfläche.

Andere Anleitungen erweitern dieses Szenario, um der Seite, die Sie hinzufügen, weitere Funktionen hinzuzufügen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte in diesem Leitfaden abzuschließen, muss die folgende Software auf dem lokalen Entwicklungscomputer installiert sein:

- [Git-Client](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Vorbereiten einer lokalen Entwicklungsumgebung für die Benutzeroberfläche

Der Code der Benutzeroberfläche des Solution Accelerators für die Remoteüberwachung wird mithilfe des JavaScript-Frameworks [React](https://reactjs.org/) implementiert. Sie finden den Quellcode im GitHub-Repository für die [Webbenutzeroberfläche für die Remoteüberwachung](https://github.com/Azure/pcs-remote-monitoring-webui).

Um Änderungen an der Benutzeroberfläche vorzunehmen und zu testen, können Sie sie auf Ihrem lokalen Entwicklungscomputer ausführen. Optional kann sich die lokale Kopie mit einer bereitgestellten Instanz des Solution Accelerators verbinden, damit sie mit Ihren realen oder simulierten Geräten interagieren kann.

Um Ihre lokale Entwicklungsumgebung vorzubereiten, verwenden Sie Git, um das Repository für die [Webbenutzeroberfläche für die Remoteüberwachung](https://github.com/Azure/pcs-remote-monitoring-webui) auf Ihren lokalen Computer zu kopieren:

```cmd/sh
git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
```

## <a name="add-a-page"></a>Hinzufügen einer Seite

Um eine Seite zur Webbenutzeroberfläche hinzuzufügen, müssen Sie die Quelldateien hinzufügen, die die Seite definieren, und einige vorhandene Dateien ändern, um die Webbenutzeroberfläche über die neue Seite zu informieren.

### <a name="add-the-new-files-that-define-the-page"></a>Hinzufügen der neuen Dateien, die die Seite definieren

Für die ersten Schritte finden Sie im Ordner **src/walkthrough/components/pages/basicPage** vier Dateien, die eine einfache Seite definieren:

**basicPage.container.js**

[!code-javascript[Page container source](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.container.js?name=container "Page container source")]

**basicPage.js**

[!code-javascript[Basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.js?name=page "Basic page")]

**basicPage.scss**

[!code-javascript[Page styling](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.scss?name=styles "Page styling")]

**basicPage.test.js**

[!code-javascript[Test code for basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.test.js?name=test "Test code for basic page")]

Erstellen eines neuen Ordners **src/components/pages/example** und kopieren Sie diese vier Dateien hinein.

### <a name="add-the-new-page-to-the-web-ui"></a>Hinzufügen einer neuen Seite zur Webbenutzeroberfläche

Um die neue Seite zur Webbenutzeroberfläche hinzuzufügen, nehmen Sie die folgenden Änderungen an vorhandenen Dateien vor:

1. Fügen Sie den neuen Seitendatencontainer zur Datei **src/components/pages/index.js** hinzu:

    ```js
    export * from './example/basicPage.container';
    ```

1. (Optional) Fügen Sie ein SVG-Symbol für die neue Seite hinzu. Weitere Informationen finden Sie in [webui/src/utilities/README.md](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/utilities/README.md). Sie können eine vorhandene SVG-Datei verwenden.

1. Fügen Sie den Seitennamen zur Übersetzungsdatei **public/locales/en/translations.json** hinzu. Die Webbenutzeroberfläche verwendet [i18next](https://www.i18next.com/) für die Internationalisierung.

    ```json
    "tabs": {
      "basicPage": "Example",
    },
    ```

1. Öffnen Sie die Datei **src/components/app.js**, die die Anwendungsseite auf der obersten Ebene definiert. Fügen Sie die neue Seite zur Liste der Importe hinzu:

    ```javascript
    // Page Components
    import  {
      //...
      BasicPageContainer
    } from './pages';
    ```

1. Fügen Sie die neue Seite in derselben Datei zum `pagesConfig`-Array hinzu. Legen Sie die `to`-Adresse für die Route fest, verweisen Sie auf das SVG-Symbol und die zuvor hinzugefügten Übersetzungen, und stellen Sie die `component` auf den Container der Seite ein:

    ```js
    const pagesConfig = [
      //...
      {
        to: '/basicpage',
        exact: true,
        svg: svgs.tabs.example,
        labelId: 'tabs.basicPage',
        component: BasicPageContainer
      },
      //...
    ];
    ```

1. Fügen Sie neue Breadcrumbs zum `crumbsConfig`-Array hinzu:

    ```js
    const crumbsConfig = [
      //...
      {
        path: '/basicpage', crumbs: [
          { to: '/basicpage', labelId: 'tabs.basicPage' }
        ]
      },
      //...
    ];
    ```

    Diese Beispielseite hat nur ein Breadcrumb, aber einige Seiten können mehr haben.

Speichern Sie alle Änderungen. Jetzt können Sie die Webbenutzeroberfläche mit Ihrer neuen Seite auszuführen.

### <a name="test-the-new-page"></a>Testen der neuen Seite

Navigieren Sie in der Eingabeaufforderung zum Stammverzeichnis der lokalen Kopie des Repositorys, und führen Sie die folgenden Befehle zum Installieren der erforderlichen Bibliotheken und lokalen Ausführen der Webbenutzeroberfläche aus:

```cmd/sh
npm install
npm start
```

Der vorherige Befehl führt die Benutzeroberfläche lokal unter [http://localhost:3000/dashboard](http://localhost:3000/dashboard) aus.

Wenn Sie Ihre lokale Instanz der Webbenutzeroberfläche nicht mit einer bereitgestellten Instanz des Solution Accelerators verbinden, werden im Dashboard Fehler angezeigt. Diese Fehler haben keinen Einfluss auf Ihre Fähigkeit, Ihre neue Seite zu testen.

Sie können nun den Code bearbeiten, während die Website lokal ausgeführt wird, und sehen, wie die Webbenutzeroberfläche dynamisch aktualisiert wird.

## <a name="optional-connect-to-deployed-instance"></a>[Optional] Verbinden mit der bereitgestellten Instanz

Optional können Sie Ihre lokal ausgeführte Kopie der Webbenutzeroberfläche mit dem Solution Accelerator für die Remoteüberwachung in der Cloud verbinden:

1. Bereitstellen einer **grundlegenden** Instanz des Solution Accelerators mit der **pcs**-CLI. Notieren Sie den Namen der Bereitstellung und die Anmeldeinformationen, die Sie für die VM angegeben haben. Weitere Informationen finden Sie unter [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung über die Befehlszeilenschnittstelle](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Aktivieren Sie über das Azure-Portal oder mit [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) den SSH-Zugriff auf den virtuellen Computer, der die Microservices in der Lösung hostet. Beispiel:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Sie sollten nur während der Test- und Entwicklungsphase SSH-Zugriff aktivieren. Wenn Sie SSH aktivieren, [sollten Sie es so bald wie möglich wieder deaktivieren](../security/fundamentals/network-best-practices.md).

1. Suchen Sie über das Azure-Portal oder mit [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) den Namen und die öffentliche IP-Adresse des virtuellen Computers. Beispiel:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Stellen Sie mit SSH die Verbindung mit dem virtuellen Computer mithilfe der IP-Adresse aus dem vorherigen Schritt und den Anmeldeinformationen her, die Sie bei der Ausführung von **pcs** zum Bereitstellen der Lösung angegeben haben.

1. Damit der lokale Computer eine Verbindung herstellen kann, führen Sie die folgenden Befehle in der Bash-Shell des virtuellen Computers aus:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Sobald der Befehl abgeschlossen ist und die Website gestartet wird, können Sie den virtuellen Computer trennen.

1. Bearbeiten Sie in Ihrer lokalen Kopie des Repositorys für die [Webbenutzeroberfläche für die Remoteüberwachung](https://github.com/Azure/pcs-remote-monitoring-webui) die **.env**-Datei, um die URL für die bereitgestellte Lösung hinzuzufügen:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie die Webbenutzeroberfläche im Solution Accelerator für die Remoteüberwachung anpassen können.

Nachdem Sie eine Seite definiert haben, ist der nächste Schritt das [Hinzufügen eines benutzerdefinierten Dienstes zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize-service.md), der Daten abruft, die auf der Benutzeroberfläche angezeigt werden sollen.

Weitere konzeptuelle Informationen zum Solution Accelerator für die Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).
