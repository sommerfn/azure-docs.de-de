---
title: include file
description: include file
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: dcfa65c655d1508510282fe66e90e4076278b29b
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949966"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie den folgenden Befehl aus, um die Funktions-App zu starten. Die App wird mit der gleichen Azure Functions-Runtime ausgeführt, die in Azure vorliegt. Der Startbefehl variiert abhängig von der Projektsprache.

### <a name="c"></a>C\#

```command
func start --build
```

### <a name="javascript"></a>JavaScript

```command
func start
```

### <a name="typescript"></a>TypeScript

```command
npm install
npm start     
```

Wenn der Functions-Host startet, schreibt er eine Ausgabe ähnlich der folgenden, die zur besseren Lesbarkeit beschnitten wurde:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieren Sie die URL Ihrer `HttpTrigger`-Funktion aus der Runtimeausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der lokalen Funktion zurückgegebenen GET-Anforderung abgebildet:

![Lokales Testen im Browser](./media/functions-run-function-test-local/functions-test-local-browser.png)

Jetzt haben Sie die Funktion lokal ausgeführt und können die Funktions-App und andere erforderliche Ressourcen in Azure erstellen.