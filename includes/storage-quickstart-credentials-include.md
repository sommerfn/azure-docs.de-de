---
title: include file
description: include file
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: include
ms.date: 11/23/2019
ms.author: mhopkins
ms.custom: include file
ms.openlocfilehash: 7e6bdf8f2cede2d55163932bfe8ab58e9ee96e0f
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422036"
---
### <a name="copy-your-credentials-from-the-azure-portal"></a>Kopieren Ihrer Anmeldeinformationen aus dem Azure-Portal

Wenn die Beispielanwendung eine Anforderung an Azure Storage sendet, muss diese autorisiert werden. Fügen Sie zum Autorisieren einer Anforderung die Anmeldeinformationen für Ihr Speicherkonto in Form einer Verbindungszeichenfolge hinzu. Führen Sie zum Anzeigen der Anmeldeinformationen Ihres Speicherkontos die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie im Abschnitt **Einstellungen** der Speicherkontoübersicht die Option **Zugriffsschlüssel**. Hier können Sie Ihre Kontozugriffsschlüssel und die vollständige Verbindungszeichenfolge für jeden Schlüssel anzeigen.
4. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und wählen Sie dann die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren. Der Wert der Verbindungszeichenfolge wird in einem späteren Schritt einer Umgebungsvariablen hinzugefügt.

    ![Screenshot: Kopieren einer Verbindungszeichenfolge aus dem Azure-Portal](./media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Schreiben Sie die Verbindungszeichenfolge nach dem Kopieren in eine neue Umgebungsvariable auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Festlegen der Umgebungsvariablen ein Konsolenfenster, und befolgen Sie die Anleitung für Ihr Betriebssystem. Ersetzen Sie `<yourconnectionstring>` durch Ihre Verbindungszeichenfolge.

#### <a name="windows"></a>Windows

```cmd
setx CONNECT_STR "<yourconnectionstring>"
```

Sie müssen nach dem Hinzufügen der Umgebungsvariablen unter Windows eine neue Instanz des Befehlsfensters öffnen.

#### <a name="linux"></a>Linux

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="macos"></a>macOS

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="restart-programs"></a>Neustarten von Programmen

Nachdem Sie die Umgebungsvariable hinzugefügt haben, starten Sie alle ausgeführten Programme neu, in denen die Umgebungsvariable gelesen werden muss. Starten Sie beispielsweise die Entwicklungsumgebung oder den Editor neu, bevor Sie fortfahren.
