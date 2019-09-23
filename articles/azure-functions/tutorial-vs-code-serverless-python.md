---
title: Erstellen und Bereitstellen von Azure Functions in Python mit Visual Studio Code
description: Verwenden der Visual Studio Code-Erweiterung für Azure Functions, um serverlose Funktionen in Python zu erstellen und sie in Azure bereitzustellen.
services: functions
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: glenga
ms.openlocfilehash: 590757f78086be894cdc2384bb4a4df380e91c27
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71098603"
---
# <a name="deploy-python-to-azure-functions-with-visual-studio-code"></a>Bereitstellen von Python für Azure Functions mit Visual Studio Code

In diesem Tutorial verwenden Sie Visual Studio Code und die Azure Functions-Erweiterung, um einen serverlosen HTTP-Endpunkt mit Python zu erstellen und auch eine Verbindung (oder „Bindung“) mit dem (bzw. an den) Speicher hinzuzufügen. Azure Functions führt Ihren Code in einer serverlosen Umgebung aus, ohne dass ein virtueller Computer bereitgestellt oder eine Web-App veröffentlicht werden muss. Die Azure Functions-Erweiterung für Visual Studio Code vereinfacht den Prozess der Verwendung von Funktionen erheblich, indem sie automatisch viele Konfigurationsprobleme löst.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren der Azure Functions-Erweiterung
> * Erstellen einer über HTTP ausgelösten Funktion
> * Lokales Debuggen
> * Synchronisieren von Anwendungseinstellungen
> * Anzeigen von Streamingprotokollen
> * Herstellen einer Verbindung mit Azure Storage

Wenn Sie Probleme mit einem der Schritte in diesem Tutorial haben, würden wir uns über nähere Informationen freuen. Verwenden Sie die **Ich bin auf ein Problem gestoßen**-Schaltfläche Ende jedes Abschnitts, um detailliertes Feedback zu geben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure-Abonnement](#azure-subscription).
- [Visual Studio Code mit der Azure Functions](#visual-studio-code-python-and-the-azure-functions-extension)-Erweiterung.
- Die [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Azure-Abonnement

Wenn Sie kein Azure-Abonnement haben, [registrieren Sie sich jetzt](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) für ein kostenloses 30-tägiges Konto mit 200 US-Dollar in Form einer Azure-Gutschrift, um eine beliebige Kombination von Diensten auszuprobieren.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python und die Azure Functions-Erweiterung

Installieren Sie folgende Software:

- Python 3.6.x wie für Azure Functions erforderlich. [Python 3.6.9](https://www.python.org/downloads/release/python-369/) ist die neueste 3.6.x-Version.
- [Visual Studio Code](https://code.visualstudio.com/).
- Die [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) wie in [Erste Schritte in Python mit Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial) beschrieben.
- Die [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Allgemeine Informationen finden Sie im [vscode-azurefunctions-GitHub-Repository](https://github.com/Microsoft/vscode-azurefunctions).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Befolgen Sie die Anweisungen für Ihr Betriebssystem unter [Arbeiten mit Azure Functions Core Tools](functions-run-local.md#v2). Die Tools selbst sind in .NET Core geschrieben, und das Core Tools-Paket wird am besten mit dem Node.js-Paketmanager npm installiert, weshalb Sie derzeit auch für Python-Code .NET Core und Node.js installieren müssen. Sie können jedoch die .NET Core-Anforderung mithilfe von „Erweiterungsbundles“ umgehen, wie in der vorgenannten Dokumentation beschrieben. Wie auch immer, Sie müssen diese Komponenten nur einmal installieren, danach fordert Visual Studio Code Sie automatisch auf, alle Updates zu installieren.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Sobald die Functions-Erweiterung installiert ist, melden Sie sich bei Ihrem Azure-Konto an. Wechseln Sie dazu zum **Azure: Functions**-Explorer, wählen Sie **Bei Azure anmelden** aus, und befolgen Sie die Eingabeaufforderungen.

![Anmelden bei Azure über Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-sign-in.png)

Vergewissern Sie sich nach der Anmeldung, dass das E-Mail-Konto Ihres Azure-Abonnements in der Statusleiste angezeigt wird:

![Visual Studio Code-Statusleiste mit Azure-Konto](media/tutorial-vs-code-serverless-python/azure-account-status-bar.png)

Der Name, den Sie Ihrem Abonnement zugewiesen haben, wird auch angezeigt im **Azure: Functions**-Explorer („Primär“ in der Abbildung unten):

![Azure App Service-Explorer in Visual Studio Code mit Abonnements](media/tutorial-vs-code-serverless-python/azure-subscription-view.png)

> [!NOTE]
> Wenn Sie auf den Fehler **Abonnement mit dem Namen [Abonnement-ID] nicht gefunden** stoßen, kann dies daran liegen, dass Sie sich hinter einem Proxy befinden und die Azure-API nicht erreichen können. Konfigurieren Sie die Umgebungsvariablen `HTTP_PROXY` und `HTTPS_PROXY` in Ihrem Terminal mit Ihren Proxyinformationen:
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```ps
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```

### <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen

Um sicherzustellen, dass alle Azure Functions-Tools installiert sind, öffnen Sie die Visual Studio Code-Befehlspalette (F1), wählen Sie den Befehl **Terminal: Neues integriertes Terminal erstellen** aus, und führen Sie nach dem Öffnen des Terminals den Befehl `func` aus:

![Überprüfen der Voraussetzungen für Azure Functions Core Tools](media/tutorial-vs-code-serverless-python/check-prereqs.png)

Die Ausgabe, die mit dem Azure Functions-Logo beginnt (Sie müssen die Ausgabe nach oben scrollen), gibt an, dass die Azure Functions Core Tools vorhanden sind.

Wenn der `func`-Befehl nicht erkannt wird, vergewissern Sie sich, dass der Ordner, in dem Sie die Azure Functions Core Tools installiert haben, in der PATH-Umgebungsvariablen enthalten ist.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=01-verify-prerequisites)

## <a name="create-the-function"></a>Erstellen der Funktion

1. Code für Azure Functions wird innerhalb eines Functions-_Projekts_ verwaltet, das Sie erstellen, bevor Sie den Code erstellen. Im **Azure: Functions**-Explorer (wird mit dem Azure-Symbol auf der linken Seite geöffnet) wählen Sie das Befehlssymbol **Neues Projekt** aus, oder öffnen Sie die Befehlspalette (F1), und wählen Sie **Azure Functions: Neues Projekt erstellen** aus.

    ![Schaltfläche „Neues Projekt erstellen“ im Functions-Explorer](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. In den folgenden Eingabeaufforderungen:

    | Prompt | Wert | BESCHREIBUNG | 
    | --- | --- | --- |
    | Angeben eines Ordners für das Projekt | Aktuell geöffneter Ordner | Der Ordner, in dem das Projekt erstellt wird. Sie sollten das Projekt in einem Unterordner erstellen. |
    | Auswählen einer Sprache für Ihr Funktions-App-Projekt | **Python** | Die Sprache, die für die Funktion verwendet werden soll, die die für den Code verwendete Vorlage bestimmt. |
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | **HTTP-Trigger** | Eine Funktion, die einen HTTP-Trigger verwendet, wenn eine HTTP-Anforderung an den Endpunkt der Funktion gerichtet wird. (Für Azure Functions gibt es eine Reihe von anderen Triggern. Weitere Informationen finden Sie unter [Welche Möglichkeiten bestehen mit Functions?](functions-overview.md#what-can-i-do-with-functions).) |
    | Angeben eines Funktionsnamens | HttpExample | Der Name wird für einen Unterordner verwendet, der den Code der Funktion zusammen mit Konfigurationsdaten enthält, und definiert auch den Namen des HTTP-Endpunkts. Verwenden Sie „HttpExample“ anstelle der Standardeinstellung „HTTPTrigger“, um die Funktion selbst vom Trigger zu unterscheiden. |
    | Autorisierungsstufe | **Function** | Aufrufe, die an den Funktionsendpunkt gerichtet sind, erfordern einen [Funktionsschlüssel](functions-bindings-http-webhook.md#authorization-keys). |
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | **In aktuellem Fenster öffnen** | Öffnet das Projekt im aktuellen Visual Studio Code-Fenster. |

1. Nach kurzer Zeit wird eine Meldung angezeigt, dass das neue Projekt erstellt wurde. Im **Explorer** gibt es den Unterordner, der für die Funktion erstellt wurde. 

1. Öffnen Sie die *\_\_init\_\_.py*-Datei, die den Standardfunktionscode enthält:

    [![Ergebnis der Erstellung eines neuen Python-Functions-Projekts](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > Wenn Visual Studio Code Ihnen beim Öffnen von *\_\_init\_\_.py* mitteilt, dass Sie keinen Python-Interpreter ausgewählt haben, öffnen Sie die Befehlspalette (F1), und wählen Sie den **Python: Interpreter auswählen**-Befehl und dann die virtuelle Umgebung im lokalen `.env`-Ordner aus (der als Teil des Projekts erstellt wurde). Die Umgebung muss speziell auf Python 3.6x basieren, wie bereits zuvor unter [Voraussetzungen](#prerequisites) erwähnt.
    >
    > ![Auswählen der mit dem Projekt erstellten virtuellen Umgebung](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=02-create-function)

## <a name="examine-the-code-files"></a>Untersuchen der Codedateien

Im neu erstellten Funktionsunterordner _HttpExample_ sind drei Dateien enthalten: *\_\_init\_\_.py* enthält den Funktionscode, *function.json* beschreibt die Funktion für Azure Functions, und *sample.dat* ist eine Beispieldatendatei. Sie können *sample.dat* löschen, wenn Sie möchten, da sie nur dazu dient, Ihnen zu zeigen, dass Sie dem Unterordner weitere Dateien hinzufügen können.

Wir betrachten zuerst *function.json* und dann den Code in *\_\_init\_\_* .py.

### <a name="functionjson"></a>function.json

Die Datei „function.json“ enthält die erforderlichen Konfigurationsinformationen für den Azure Functions-Endpunkt:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Die `scriptFile`-Eigenschaft identifiziert die Startdatei für den Code, und dieser Code muss eine Python-Funktion mit dem Namen `main` enthalten. Sie können Ihren Code in mehrere Dateien einbeziehen, sofern die hier angegebene Datei eine `main`-Funktion enthält.

Das `bindings`-Element enthält zwei Objekte, eines zum Beschreiben eingehender Anforderungen und das andere, um die HTTP-Antwort zu beschreiben. Bei eingehenden Anforderungen (`"direction": "in"`) antwortet die Funktion auf HTTP-GET- oder -POST-Anforderungen und erfordert, dass Sie den Funktionsschlüssel angeben. Die Antwort (`"direction": "out"`) ist eine HTTP-Antwort, die jeden Wert zurückgibt, der von der `main`-Python-Funktion zurückgegeben wird.

### <a name="__initpy__"></a>\_\_init.py\_\_

Wenn Sie eine neue Funktion erstellen, stellt Azure Functions standardmäßigen Python-Code in *\_\_init\_\_* .py bereit:

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Die wichtigen Teile des Codes lauten wie folgt:

- Sie müssen `func` aus `azure.functions` importieren; das Importieren des Protokollierungsmoduls ist optional, wird jedoch empfohlen.
- Die erforderliche `main`-Python-Funktion empfängt ein `func.request`-Objekt mit dem Namen `req` und gibt einen Wert vom Typ `func.HttpResponse` zurück. Weitere Informationen zu den Funktionen dieser Objekte finden Sie in den Referenzen zu [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) und [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python).
- Der Text von `main` verarbeitet dann die Anforderung und generiert eine Antwort. In diesem Fall sucht der Code in der URL nach einem `name`-Parameter. Wenn dies nicht gelingt, wird überprüft, ob der Anforderungstext JSON-Code enthält (mit `func.HttpRequest.get_json`) und ob der JSON-Code einen `name`-Wert enthält (mit der `get`-Methode des von `get_json` zurückgegebenen JSON-Objekts).
- Wenn ein Name gefunden wird, gibt der Code die Zeichenfolge „Hello“ mit angefügtem Namen zurück. Andernfalls wird eine Fehlermeldung zurückgegeben.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=03-examine-code-files)

## <a name="debug-locally"></a>Lokales Debuggen

1. Wenn Sie das Functions-Projekt erstellen, erstellt die Visual Studio Code-Erweiterung außerdem eine Startkonfiguration in `.vscode/launch.json`, die eine einzelne Konfiguration mit dem Namen **Attach to Python Functions** (Anfügen an Python-Functions) enthält. Diese Konfiguration bedeutet, dass Sie einfach **F5** drücken oder den Debug-Explorer verwenden können, um das Projekt zu starten:

    ![Debug-Explorer mit Functions-Startkonfiguration](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. Wenn Sie den Debugger starten, wird ein Terminal geöffnet, das die Ausgabe von Azure Functions anzeigt, einschließlich einer Zusammenfassung der verfügbaren Endpunkte. Die URL ist möglicherweise anders, wenn Sie einen anderen Namen als „HttpExample“ verwendet haben:

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Klicken Sie mit **STRG+Klicken** oder **BEFEHLSTASTE+Klicken** auf die URL im Visual Studio Code-Fenster **Ausgabe**, um einen Browser mit dieser Adresse zu öffnen, oder starten Sie einen Browser, und fügen Sie die gleiche URL ein. Der Endpunkt ist in jedem Fall `api/<function_name>`, in diesem Fall `api/HttpExample`. Da diese URL jedoch keinen Namensparameter enthält, sollte im Browserfenster nur „Geben Sie einen Namen in der Abfragezeichenfolge oder im Anforderungstext ein“ angezeigt werden, wie für diesen Pfad im Code angebracht ist.

1. Versuchen Sie nun, zur Verwendung einen Namensparameter hinzuzufügen, z.B. `http://localhost:7071/api/HttpExample?name=VS%20Code`, und im Browserfenster sollte die Meldung „Hello Visual Studio Code!“ angezeigt werden, um zu zeigen, dass Sie diesen Codepfad ausgeführt haben.

1. Wenn Sie den Namenswert einem JSON-Anforderungstext übergeben möchten, können Sie ein Tool wie curl mit dem JSON-Code inline verwenden:

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    Erstellen Sie alternativ eine Datei wie *data.json*, die `{"name":"Visual Studio Code"}`enthält, und verwenden Sie den Befehl `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Um die Funktion zu debuggen, legen Sie einen Haltepunkt in der Zeile fest, die `name = req.params.get('name')` liest, und richten Sie erneut eine Anforderung an die URL. Der Visual Studio Code-Debugger sollte in dieser Zeile stoppen, sodass Sie Variablen untersuchen und den Code schrittweise durchlaufen können. (Eine kurze exemplarische Vorgehensweise zum grundlegenden Debuggen finden Sie unter [Visual Studio Code Tutorial – Configure and run the debugger](https://code.visualstudio.com/docs/python/python-tutorial.md#configure-and-run-the-debugger) (Visual Studio Code-Tutorial: Konfigurieren und Ausführen des Debuggers).)

1. Wenn Sie die Funktion gründlich lokal getestet haben, stoppen Sie den Debugger (mit dem Menübefehl **Debuggen** > **Debuggen stoppen** oder dem Befehl **Trennen** in der Debugsymbolleiste).

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=04-test-debug)

## <a name="deploy-to-azure-functions"></a>Bereitstellen in Azure Functions

In diesen Schritten verwenden Sie die Functions-Erweiterung, um eine Funktions-App in Azure zusammen mit anderen erforderlichen Azure-Ressourcen zu erstellen. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Außerdem sind ein Azure Storage-Konto für Daten und ein [Hostingplan](functions-scale.md#hosting-plan-support) erforderlich. Alle diese Ressourcen sind in einer einzelnen Ressourcengruppe angeordnet.

1. Wählen Sie im Bereich **Azure: Functions**-Explorer den Befehl **Für Funktions-App bereitstellen** aus, oder öffnen Sie die Befehlspalette (F1), und wählen Sie den Befehl **Azure Functions: Für Funktions-App** bereitstellen aus. Auch hier befindet sich die Funktions-App dort, wo Ihr Python-Projekt in Azure ausgeführt wird.

    ![Befehl „Für Funktions-App bereitstellen“](media/tutorial-vs-code-serverless-python/deploy-command.png)

1. Wenn Sie dazu aufgefordert werden, wählen Sie **Neue Funktions-App in Azure erstellen** aus, und geben Sie einen Namen an, der in Azure eindeutig ist (in der Regel verwenden Sie Ihren persönlichen Namen oder Firmennamen zusammen mit anderen eindeutigen Bezeichnern; Sie können Buchstaben, Ziffern und Bindestriche verwenden). Wenn Sie zuvor eine Funktions-App erstellt haben, wird der zugehörige Name in dieser Liste von Optionen angezeigt.

1. Die Erweiterung führt die folgenden Aktionen aus, die Sie in Visual Studio Code-Popupmeldungen und im Fenster **Ausgabe** beobachten können (der Vorgang dauert einige Minuten):

    - Erstellen Sie eine Ressourcengruppe mit dem von ihnen vergebenen Namen (ohne Bindestriche).
    - Erstellen Sie in dieser Ressourcengruppe das Speicherkonto, den Hostingplan und die Funktions-App. Standardmäßig wird ein [Verbrauchstarif](functions-scale.md#consumption-plan) erstellt. Um Ihre Funktionen in einem dedizierten Plan ausführen zu können, müssen Sie die [Veröffentlichung mit erweiterten Erstellungsoptionen aktivieren](functions-develop-vs-code.md).
    - Stellen Sie Ihren Code für die Funktions-App bereit.

    Der **Azure: Functions**-Explorer zeigt außerdem den Status an:

    ![Bereitstellungsstatusanzeige im Azure: Functions-Explorer](media/tutorial-vs-code-serverless-python/deploy-progress.png)

1. Nach Abschluss der Bereitstellung zeigt die Azure Functions-Erweiterung eine Meldung mit Schaltflächen für drei weitere Aktionen an:

    ![Meldung über eine erfolgreiche Bereitstellung mit zusätzlichen Aktionen](media/tutorial-vs-code-serverless-python/deployment-popup.png)

    Informationen zum **Streamen von Protokollen** und zu **Uploadeinstellungen** finden Sie in den nächsten Abschnitten. Informationen zur **Anzeige der Ausgabe** finden Sie im folgenden Schritt 5.

1. Nach der Bereitstellung wird im Fenster **Ausgabe** auch der öffentliche Endpunkt in Azure angezeigt:

    ```output
    HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    ```

    Verwenden Sie diesen Endpunkt, um die gleichen Tests auszuführen, die Sie lokal durchgeführt haben, und verwenden Sie dabei URL-Parameter und/oder Anforderungen mit JSON-Daten im Anforderungstext. Die Ergebnisse des öffentlichen Endpunkts sollten mit den Ergebnissen des lokalen Endpunkts, den Sie zuvor getestet haben, identisch sein.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=05-deploy)

### <a name="stream-logs"></a>Streaming von Protokollen

Die Unterstützung für das Streamen von Protokollen befindet sich zurzeit in der Entwicklung, wie in [Issue 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) (Problem 589) für die Azure Functions-Erweiterung beschrieben. Die Schaltfläche **Protokolle streamen** in der Bereitstellungspopupmeldung stellt schließlich die Verbindung der Protokollausgabe in Azure mit Visual Studio Code her. Sie können den Protokollstream auch im **Azure Functions**-Explorer starten und stoppen, indem Sie mit der rechten Maustaste auf das Functions-Projekt klicken und **Protokollstreaming starten** oder **Protokollstreaming stoppen** auswählen.

Derzeit sind diese Befehle jedoch noch nicht funktionstüchtig. Das Protokollstreaming ist stattdessen in einem Browser verfügbar, indem Sie den folgenden Befehl ausführen und dabei `<app_name>` durch den Namen Ihrer Functions-App in Azure ersetzen:

```bash
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

### <a name="sync-local-settings-to-azure"></a>Synchronisieren lokaler Einstellungen mit Azure

Die Schaltfläche **Uploadeinstellungen** in der Bereitstellungspopupmeldung wendet alle Änderungen, die Sie an Ihrer Datei *local.settings.json* vorgenommen haben, in Azure an. Sie können den Befehl auch im **Azure Functions**-Explorer aufrufen, indem Sie den Functions-Projektknoten erweitern, mit der rechten Maustaste auf **Anwendungseinstellungen** klicken und **Lokale Einstellungen hochladen** auswählen. Sie können auch die Befehlspalette verwenden, um den Befehl **Azure Functions: Lokale Einstellungen hochladen** auszuwählen.

Beim Hochladen von Einstellungen werden alle vorhandenen Einstellungen aktualisiert und neue, in *local.settings.json* definierte Einstellungen hinzugefügt. Beim Hochladen werden keine Einstellungen aus Azure entfernt, die nicht in der lokalen Datei aufgeführt sind. Um diese Einstellungen zu entfernen, erweitern Sie den Knoten **Anwendungseinstellungen** im **Azure Functions**-Explorer, klicken Sie mit der rechten Maustaste auf die Einstellung, und wählen Sie **Einstellung löschen** aus. Sie können Einstellungen auch direkt im Azure-Portal bearbeiten.

Um alle Änderungen, die Sie über das Portal oder den **Azure-Explorer** in der Datei *local.settings.json* vornehmen, zu übernehmen, klicken Sie mit der rechten Maustaste auf den Knoten **Anwendungseinstellungen**, und wählen Sie den Befehl **Remoteeinstellungen herunterladen** aus. Sie können auch die Befehlspalette verwenden, um den Befehl **Azure Functions: Remoteeinstellungen herunterladen** auszuwählen.

## <a name="add-a-second-function"></a>Hinzufügen einer zweiten Funktion

Nach der ersten Bereitstellung können Sie Änderungen am Code vornehmen, z.B. zusätzliche Funktionen hinzufügen, und ihn erneut für die gleiche Functions-App bereitstellen.

1. Wählen Sie im Bereich **Azure: Functions**-Explorer den Befehl **Funktion erstellen** aus, oder verwenden Sie **Azure Functions: Funktion erstellen** aus der Befehlspalette. Geben Sie die folgenden Details für die Funktion an:

    - Vorlage: HTTP-Trigger
    - Name: „DigitsOfPi“
    - Autorisierungsstufe: Anonym

1. Im Visual Studio Code-Datei-Explorer befindet sich ein Unterordner mit Ihrem Funktionsnamen, der wiederum Dateien mit dem Namen *\_\_init\_\_.py*, *function.json* und *sample.dat* enthält.

1. Ersetzen Sie den Inhalt von *\_\_init\_\_.py*, sodass er dem folgenden Code entspricht, der eine Zeichenfolge mit dem Wert von PI für eine Anzahl von Ziffern generiert, die in der URL angegeben sind (dieser Code verwendet nur einen URL-Parameter.)

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. Da der Code nur HTTP-GET unterstützt, ändern Sie *function.json* so, dass die `"methods"`-Sammlung nur `"get"` enthält (d.h. entfernen Sie `"post"`). Die Datei sollte wie folgt aussehen:

    ```json
    {
      "scriptFile": "__init__.py",
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ]
        },
        {
          "type": "http",
          "direction": "out",
          "name": "$return"
        }
      ]
    }
    ```

1. Starten Sie den Debugger, indem Sie **F5** drücken oder den Menübefehl **Debuggen** > **Debuggen starten** auswählen. Im **Ausgabe**-Fenster sollten nun beide Endpunkte im Projekt angezeigt werden:

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Richten Sie in einem Browser oder von curl aus eine Anforderung an `http://localhost:7071/api/DigitsOfPi?digits=125`, und beobachten Sie die Ausgabe. (Möglicherweise bemerken Sie, dass der Codealgorithmus nicht ganz genau ist, aber wir überlassen Ihnen die Verbesserungen!) Stoppen Sie den Debugger, wenn Sie fertig sind.

1. Stellen Sie den Code mithilfe des Befehls **Für Funktions-App bereitstellen** erneut im **Azure: Functions**-Explorer bereit. Wenn Sie dazu aufgefordert werden, wählen Sie die zuvor erstellte Funktions-App aus.

1. Nachdem die Bereitstellung abgeschlossen ist (es dauert einige Minuten!), werden im **Ausgabe**-Fenster die öffentlichen Endpunkte angezeigt, mit denen Sie die Tests wiederholen können.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=06-second-function)

## <a name="add-a-binding-to-write-messages-to-azure-storage"></a>Hinzufügen einer Bindung zum Schreiben von Meldungen in den Azure-Speicher

Mit einer _Bindung_ können Sie eine Verbindung Ihres Funktionscodes mit Ressourcen wie z.B. Azure-Speicher herstellen, ohne Datenzugriffscode schreiben zu müssen. Eine Bindung wird in der Datei *function.json* definiert und kann sowohl die Eingabe als auch die Ausgabe darstellen. Eine Funktion kann mehrere Ein- und Ausgabebindungen, aber nur einen Trigger haben. Weitere Informationen finden Sie unter [Konzepte der Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md).

In diesem Abschnitt fügen Sie der HttpExample-Funktion, die Sie zuvor in diesem Tutorial erstellt haben, eine Speicherbindung hinzu. Die Funktion verwendet diese Bindung, um mit jeder Anforderung Meldungen in den Speicher zu schreiben. Der fragliche Speicher verwendet das gleiche Standardspeicherkonto, das von der Funktions-App verwendet wird. Wenn Sie jedoch den Speicher stark nutzen möchten, sollten Sie das Erstellen eines separaten Kontos in Erwägung ziehen.

1. Synchronisieren Sie die Remoteeinstellungen für das Azure Functions-Projekt in Ihrer *local.settings.json*-Datei, indem Sie die Befehlspalette öffnen und **Azure Functions: Remoteeinstellungen herunterladen** auswählen. Öffnen Sie *local.settings.json*, und überprüfen Sie, ob sie einen Wert für `AzureWebJobsStorage` enthält. Der Wert ist die Verbindungszeichenfolge für das Speicherkonto.

1. Klicken Sie im Ordner `HttpExample` mit der rechten Maustaste auf *function.json*, und wählen Sie **Bindung hinzufügen** aus:

    ![Befehl „Bindung hinzufügen“ im Visual Studio Code-Explorer](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. Wählen Sie in den folgenden Eingabeaufforderungen in Visual Studio Code die Werte aus, oder geben Sie sie ein:

    | Prompt | Bereitzustellender Wert |
    | --- | --- |
    | Bindungsrichtung festlegen | out |
    | Bindung mit ausgehender Richtung auswählen | Azure Queue Storage |
    | Der Name, der zum Identifizieren dieser Bindung in Ihrem Code verwendet wird | msg |
    | Die Warteschlange, an die die Nachricht gesendet wird | outqueue |
    | Einstellung aus *local.settings.json* wählen (Fragen nach der Speicherverbindung) | AzureWebJobsStorage |

1. Nachdem Sie diese Auswahl getroffen haben, vergewissern Sie sich, dass die folgende Bindung der Datei *function.json* hinzugefügt wurde:

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. Ersetzen Sie den Inhalt von *host.json* durch Folgendes, und fügen Sie den [Erweiterungsbundleverweis](functions-bindings-register.md#extension-bundles) hinzu.

    ```json
    {
        "version": "2.0",
        "extensionBundle": {
            "id": "Microsoft.Azure.Functions.ExtensionBundle",
            "version": "[1.*, 2.0.0)"
        }
    }
    ```

1. Nachdem Sie die Bindung konfiguriert haben, können Sie sie in Ihrem Funktionscode verwenden. Wiederum wird die neu definierte Bindung in Ihrem Code als Argument der `main`-Funktion in *\_\_init\_\_.py* angezeigt. Beispielsweise können Sie die Datei *\_\_init\_\_.py* in HttpExample so ändern, dass sie der folgenden entspricht, die die Verwendung des `msg`-Arguments zum Schreiben einer Meldung mit Zeitstempel mit dem in der Anforderung verwendeten Namen zeigt. In den Kommentaren werden die spezifischen Änderungen erläutert:

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
        logging.info('Python HTTP trigger function processed a request.')

        name = req.params.get('name')
        if not name:
            try:
                req_body = req.get_json()
            except ValueError:
                pass
            else:
                name = req_body.get('name')

        if name:
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. Um diese Änderungen lokal zu testen, starten Sie den Debugger erneut in Visual Studio Code, indem Sie **F5** drücken oder den Menübefehl **Debuggen** > **Debuggen starten** auswählen. Im **Ausgabe**-Fenster sollten nun wie zuvor die Endpunkte im Projekt angezeigt werden.

1. Rufen Sie in einem Browser die URL `http://localhost:7071/api/HttpExample?name=VS%20Code` auf, um eine Anforderung an den HttpExample-Endpunkt zu erstellen, die auch in eine Meldung in der Warteschlange schreiben sollte.

1. Um zu überprüfen, ob die Nachricht in die Warteschlange „outqueue“ (wie in der Bindung benannt) geschrieben wurde, können Sie eine der drei folgenden Methoden verwenden:

    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie zu der Ressourcengruppe, die ihr Functions-Projekt enthält. Suchen Sie in dieser Ressourcengruppe nach dem Speicherkonto für das Projekt, öffnen Sie dieses Speicherkonto, und wechseln Sie dann zu **Warteschlangen**. Wechseln Sie auf dieser Seite zu „outqueue“, wo alle protokollierten Meldungen angezeigt werden sollten.

    1. Öffnen und untersuchen Sie die Warteschlange mit dem in Visual Studio integrierten Azure Storage-Explorer wie unter [Verbinden von Funktionen mit Azure Storage mithilfe von Visual Studio Code](functions-add-output-binding-storage-queue-vs-code.md) beschrieben, insbesondere im Abschnitt [Überprüfen der Ausgabewarteschlange](functions-add-output-binding-storage-queue-vs-code.md#examine-the-output-queue).

    1. Verwenden Sie die Azure CLI, um die Speicherwarteschlange abzufragen, wie unter [Abfragen der Speicherwarteschlange](functions-add-output-binding-storage-queue-python.md#query-the-storage-queue) beschrieben.
    
1. Stellen Sie den Code zum Testen in der Cloud mithilfe des Befehls **Für Funktions-App bereitstellen** erneut im **Azure: Functions**-Explorer bereit. Wenn Sie dazu aufgefordert werden, wählen Sie die zuvor erstellte Funktions-App aus. Nachdem die Bereitstellung abgeschlossen ist (es dauert einige Minuten!), werden im **Ausgabe**-Fenster erneut die öffentlichen Endpunkte angezeigt, mit denen Sie die Tests wiederholen können.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Der Funktions-APP, die Sie erstellt haben, umfasst Ressourcen, die minimale Kosten verursachen können (siehe [Functions – Preise](https://azure.microsoft.com/pricing/details/functions/)). Um die Ressourcen zu bereinigen, klicken Sie mit der rechten Maustaste auf die Funktions-App im **Azure: Functions**-Explorer, und wählen Sie **Funktions-App löschen** aus. Sie können auch das [Azure-Portal](https://portal.azure.com) aufrufen, **Ressourcengruppen** im Navigationsbereich auf der linken Seite auswählen, die im Rahmen dieses Tutorials erstellte Ressourcengruppe auswählen und dann den Befehl **Ressourcengruppe löschen** verwenden.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch zum Absolvieren dieser exemplarische Vorgehensweise zum Bereitstellen von Python-Code für Azure Functions! Sie sind jetzt bereit, viele weitere serverlose Funktionen zu erstellen.

Wie bereits erwähnt, können Sie mehr über die Functions-Erweiterung erfahren, indem Sie das zugehörige GitHub-Repository [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions) besuchen. Beschreibungen von Problemen und sonstige Beiträge sind ebenfalls willkommen.

Lesen Sie die [Einführung in Azure Functions](functions-overview.md), um die verschiedenen Trigger zu untersuchen, die Sie verwenden können.

Weitere Informationen zu Azure-Diensten, die Sie von Python aus verwenden können, einschließlich Datenspeicherung sowie KI und Machine Learning-Diensten, finden Sie unter [Azure für Python-Cloudentwickler](/azure/python/?view=azure-python).

Es gibt auch andere Azure-Erweiterungen für Visual Studio Code, die möglicherweise für Sie hilfreich sind. Suchen Sie einfach im Erweiterungen-Explorer nach „Azure“:

![Azure-Erweiterungen für Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions.png)

Einige beliebte Erweiterungen sind:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
- [Azure CLI-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
