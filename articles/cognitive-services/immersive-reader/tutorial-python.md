---
title: 'Tutorial: Starten des plastischen Readers unter Verwendung von Python'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine Python-Anwendung, die den plastischen Reader startet.
services: cognitive-services
author: dylankil
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 08/02/2019
ms.author: dylankil
ms.openlocfilehash: 6404a5d49bd7af1ed5d74299f03eda8d0bb14b89
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326444"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-python-sample-project"></a>Tutorial: Starten con Plastischer Reader unter Verwendung des Python-Beispielprojekts

In der [Übersicht](./overview.md) haben Sie gelernt, was Plastischer Reader ist und wie das Tool bewährte Techniken implementiert, um das Leseverständnis von Sprachenlernenden, Leseanfängern und Schülern mit Lernunterschieden zu verbessern. In diesem Tutorial erfahren Sie, wie Sie eine Python-Webanwendung erstellen, die Plastischer Reader startet. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Python-Web-App mit pip, Flask, Jinja und virtualenv unter Verwendung eines Beispielprojekts
> * Abrufen eines Zugriffstokens
> * Starten von Plastischer Reader mit Beispielinhalt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Ressource von Plastischer Reader, die für die Authentifizierung mit Azure Active Directory (Azure AD) konfiguriert ist. Befolgen Sie [diese Anweisungen](./azure-active-directory-authentication.md) für die Einrichtung. Einige der hier erstellten Werte benötigen Sie bei der Konfiguration der Umgebungseigenschaften. Speichern Sie die Ausgabe Ihrer Sitzung zur späteren Verwendung in einer Textdatei.
* [Git-Client](https://git-scm.com/)
* [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk)
* [Python](https://www.python.org/downloads/) und [pip](https://docs.python.org/3/installing/index.html). Ab Python 3.4 ist pip standardmäßig in den binären Python-Installationsprogrammen enthalten.
* [Flask](https://flask.palletsprojects.com/en/1.0.x/)
* [Jinja](http://jinja.pocoo.org/docs/2.10/)
* [virtualenv](https://virtualenv.pypa.io/en/latest/) und [virtualenvwrapper-win für Windows](https://pypi.org/project/virtualenvwrapper-win/) oder [virtualenvwrapper für OSX](https://virtualenvwrapper.readthedocs.io/en/latest/)
* [requests-Modul](https://pypi.org/project/requests/2.7.0/)
* Eine IDE (beispielsweise [Visual Studio Code](https://code.visualstudio.com/))

## <a name="acquire-an-azure-ad-authentication-token"></a>Abrufen eines Azure AD-Authentifizierungstokens

Schreiben Sie eine Back-End-API zum Abrufen eines Azure AD-Authentifizierungstokens.

Für diesen Teil benötigen Sie einige Werte aus dem obigen Schritt zur Konfiguration der Azure AD-Authentifizierung. Greifen Sie auf die Textdatei zurück, die Sie in dieser Sitzung gespeichert haben.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

Erstellen Sie mit diesen Werten eine neue Datei namens _.env_, und fügen Sie den folgenden Code ein. Geben Sie dabei Ihre benutzerdefinierten Eigenschaftswerte von oben an. Ersetzen Sie die _ENV-Datei_ in der Beispiel-App durch die neu erstellte Datei.

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Committen Sie diese Datei nicht in Ihre Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind.

Der API-Endpunkt **getimmersivereadertoken** muss durch eine Authentifizierung (beispielsweise [OAuth](https://oauth.net/2/)) geschützt werden, um zu verhindern, dass nicht autorisierte Benutzer Token abrufen und für Ihren Dienst „Plastischer Reader“ und die Abrechnung verwenden. Dies wird jedoch in diesem Tutorial nicht behandelt.

## <a name="create-a-python-web-app-on-windows"></a>Erstellen einer Python-Web-App unter Windows

Erstellen Sie mithilfe von `flask` eine Python-Web-App unter Windows.

Installieren Sie [Git](https://git-scm.com/).

Öffnen Sie im Anschluss an die Git-Installation eine Eingabeaufforderung, und klonen Sie das Git-Repository des SDK für Plastischer Reader in einem Ordner auf Ihrem Computer.

```cmd
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

Installieren Sie [Python](https://www.python.org/downloads/).

Aktivieren Sie das Kontrollkästchen für das Hinzufügen von Python zu PATH.

![Dialogfeld für die Python-Installation unter Windows: Schritt 1](./media/pythoninstallone.jpg)

Fügen Sie optionale Features hinzu, indem Sie die entsprechenden Kontrollkästchen aktivieren, und klicken Sie anschließend auf die Schaltfläche „Next“ (Weiter).

![Dialogfeld für die Python-Installation unter Windows: Schritt 2](./media/pythoninstalltwo.jpg)

Wählen Sie „Custom installation“ (Benutzerdefinierte Installation) aus, legen Sie den Installationspfad auf Ihren Stammordner (beispielsweise `C:\Python37-32\`) fest, und klicken Sie anschließend auf die Schaltfläche „Install“ (Installieren).

![Dialogfeld für die Python-Installation unter Windows: Schritt 3](./media/pythoninstallthree.jpg)

Öffnen nach Anschluss der Python-Installation eine Eingabeaufforderung, und wechseln Sie mithilfe von `cd` zum Ordner mit den Python-Skripts.

```cmd
cd C:\Python37-32\Scripts
```

Installieren Sie Flask.

```cmd
pip install flask
```

Installieren Sie Jinja2. Hierbei handelt es sich um eine umfassende Vorlagen-Engine für Python.

```cmd
pip install jinja2
```

Installieren Sie virtualenv. Hierbei handelt es sich um ein Tool zum Erstellen isolierter Python-Umgebungen.

```cmd
pip install virtualenv
```

Installieren Sie virtualenvwrapper-win. Virtualenvwrapper dient zur Vereinfachung der Verwendung von virtualenv.

```cmd
pip install virtualenvwrapper-win
```

Installieren Sie das requests-Modul. requests ist eine Apache2-lizenzierte, in Python geschriebene HTTP-Bibliothek.

```cmd
pip install requests
```

Installieren Sie das Modul „python-dotenv“. Dieses Modul liest das Schlüssel-Wert-Paar aus der ENV-Datei und fügt es der Umgebungsvariablen hinzu.

```cmd
pip install python-dotenv
```

Erstellen Sie eine virtuelle Umgebung.

```cmd
mkvirtualenv advanced-python
```

Wechseln Sie mithilfe von `cd` zum Stammordner des Beispielprojekts.

```cmd
cd C:\immersive-reader-sdk\js\samples\advanced-python
```

Stellen Sie eine Verbindung zwischen dem Beispielprojekt und der Umgebung her. Dadurch wird die neu erstellte virtuelle Umgebung dem Stammordner des Beispielprojekts zugeordnet.

```cmd
setprojectdir .
```

Aktivieren Sie die virtuelle Umgebung.

```cmd
activate
```

Das Projekt sollte nun aktiv sein, und an der Eingabeaufforderung wird in etwa Folgendes angezeigt: `(advanced-python) C:\immersive-reader-sdk\js\samples\advanced-python>`.

Deaktivieren Sie die Umgebung.

```cmd
deactivate
```

Das Präfix `(advanced-python)` sollte nun nicht mehr vorhanden sein, da die Umgebung deaktiviert ist.

Wenn Sie die Umgebung wieder aktivieren möchten, führen Sie `workon advanced-python` im Stammordner des Beispielprojekts aus.

```cmd
workon advanced-python
```

### <a name="launch-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

Wenn die Umgebung aktiv ist, geben Sie `flask run` im Stammordner des Beispielprojekts ein, um das Beispielprojekt auszuführen.

```cmd
flask run
```

Navigieren Sie in Ihrem Browser zu _http://localhost:5000_ .

## <a name="create-a-python-web-app-on-osx"></a>Erstellen einer Python-Web-App unter OSX

Erstellen Sie mithilfe von `flask` eine Python-Web-App unter OSX.

Installieren Sie [Git](https://git-scm.com/).

Öffnen Sie im Anschluss an die Git-Installation ein Terminal, und klonen Sie das Git-Repository des SDK für Plastischer Reader in einem Ordner auf Ihrem Computer.

```bash
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

Installieren Sie [Python](https://www.python.org/downloads/).

Der Python-Stammordner (beispielsweise `Python37-32`) sollte sich nun im Ordner „Applications“ befinden.

Öffnen nach Anschluss der Python-Installation ein Terminal, und wechseln Sie mithilfe von `cd` zum Ordner mit den Python-Skripts.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Installieren Sie pip.

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

Führen Sie zur Vermeidung von Berechtigungsproblemen anschließend Folgendes aus, um pip für den aktuell angemeldeten Benutzer zu installieren:

```bash
python get-pip.py --user
```

```bash
sudo nano /etc/paths
```

- Geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.
- Fügen Sie den Pfad Ihrer pip-Installation Ihrer PATH-Variablen hinzu.
- Geben Sie den hinzuzufügenden Pfad am Ende der Datei als letztes Listenelement ein (beispielsweise `PATH=$PATH:/usr/local/bin`).
- Drücken Sie zum Beenden die Tastenkombination CTRL+X.
- Geben Sie `Y` ein, um den geänderten Puffer zu speichern.
- Das ist alles! Geben Sie zu Testzwecken in einem neuen Terminalfenster Folgendes ein: `echo $PATH`.

Installieren Sie Flask.

```bash
pip install flask --user
```

Installieren Sie Jinja2. Hierbei handelt es sich um eine umfassende Vorlagen-Engine für Python.

```bash
pip install Jinja2 --user
```

Installieren Sie virtualenv. Hierbei handelt es sich um ein Tool zum Erstellen isolierter Python-Umgebungen.

```bash
pip install virtualenv --user
```

Installieren Sie virtualenvwrapper. virtualenvwrapper dient zur Vereinfachung der Verwendung von virtualenv.

```bash
pip install virtualenvwrapper --user
```

Installieren Sie das requests-Modul. requests ist eine Apache2-lizenzierte, in Python geschriebene HTTP-Bibliothek.

```bash
pip install requests --user
```

Installieren Sie das Modul „python-dotenv“. Dieses Modul liest das Schlüssel-Wert-Paar aus der ENV-Datei und fügt es der Umgebungsvariablen hinzu.

```bash
pip install python-dotenv --user
```

Wählen Sie einen Ordner für Ihre virtuellen Umgebungen, und führen Sie den folgenden Befehl aus:

```bash
mkdir ~/.virtualenvs
```

Wechseln Sie mithilfe von `cd` zum Ordner mit der Python-Beispielanwendung des SDK für Plastischer Reader.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Erstellen Sie eine virtuelle Umgebung.

```bash
mkvirtualenv -p /usr/local/bin/python3 advanced-python
```

Stellen Sie eine Verbindung zwischen dem Beispielprojekt und der Umgebung her. Dadurch wird die neu erstellte virtuelle Umgebung dem Stammordner des Beispielprojekts zugeordnet.

```bash
setprojectdir .
```

Aktivieren Sie die virtuelle Umgebung.

```bash
activate
```

Das Projekt sollte nun aktiv sein, und an der Eingabeaufforderung wird in etwa Folgendes angezeigt: `(advanced-python) /immersive-reader-sdk/js/samples/advanced-python>`.

Deaktivieren Sie die Umgebung.

```bash
deactivate
```

Das Präfix `(advanced-python)` sollte nun nicht mehr vorhanden sein, da die Umgebung deaktiviert ist.

Wenn Sie die Umgebung wieder aktivieren möchten, führen Sie `workon advanced-python` im Stammordner des Beispielprojekts aus.

```bash
workon advanced-python
```

## <a name="launch-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

Wenn die Umgebung aktiv ist, geben Sie `flask run` im Stammordner des Beispielprojekts ein, um das Beispielprojekt auszuführen.

```bash
flask run
```

Navigieren Sie in Ihrem Browser zu _http://localhost:5000_ .

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.
* Sehen Sie sich Codebeispiele auf [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/).
