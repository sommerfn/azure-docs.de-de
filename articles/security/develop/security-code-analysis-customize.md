---
title: Taskanpassungsleitfaden für die Microsoft Azure-Sicherheitscodeanalyse
description: In diesem Artikel wird beschrieben, wie die Tasks in der Erweiterung „Sicherheitscodeanalyse“ angepasst werden können.
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: ab219b71eb8cd6f6172b7d02a639301c67811b49
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718189"
---
# <a name="how-to-configure--customize-the-build-tasks"></a>Anleitung: Konfigurieren und Anpassen der Buildtasks

Diese Seite beschreibt ausführlich die Konfigurationsoptionen, die in jedem der Buildtasks verfügbar sind, beginnend mit den Tasks für Sicherheitscode-Analysetools, gefolgt von den Tasks nach der Verarbeitung.

## <a name="anti-malware-scanner-task"></a>Antischadsoftware-Scannertask

> [!NOTE]
> Der Buildtask für Antischadsoftware erfordert einen Build-Agent mit aktiviertem Windows Defender. Dies gilt für Build-Agents „Hosted VS2017“ oder höher. (Die Ausführung kann nicht mit dem gehosteten Legacy-/VS2015-Agent erfolgen.) Signaturen können für diese Agents nicht aktualisiert werden, aber die Signatur sollte immer relativ aktuell und weniger als 3 Stunden alt sein.

Screenshot und Details zum Konfigurieren finden Sie unten.

![Anpassen des Buildtasks für den Antischadsoftwarescanner](./media/security-tools/5-add-anti-malware-task600.png) 

- Einstellungen für „Type = **Basic**“
- Mit „Type = **Custom**“ können Befehlszeilenargumente bereitgestellt werden, um den Scan anzupassen.

Windows Defender verwendet den Windows Update-Client, um Signaturen herunterzuladen und zu installieren. Wenn bei der Signaturaktualisierung auf dem Build-Agent ein Fehler auftritt, stammt der HRESULT-Fehlercode wahrscheinlich aus Windows Update. Weitere Informationen zu Windows Update-Fehlern und zu deren Behandlung finden Sie auf [dieser Seite](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) sowie auf dieser [TechNet-Seite](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx).

## <a name="binskim-task"></a>BinSkim-Task

> [!NOTE]
> Als Voraussetzung für die Durchführung des BinSkim-Tasks sollte der Build eine der folgenden Bedingungen erfüllen.
>    - Der Build erstellt binäre Artefakte aus verwaltetem Code.
>    - Sie haben binäre Artefakte committet, die Sie mit BinSkim analysieren möchten.

Screenshot und Details zum Konfigurieren finden Sie unten. 

![BinSkim-Setup](./media/security-tools/7-binskim-task-details.png)  
1. Legen Sie die Buildkonfiguration auf „Debug“ fest, um * **.pdb**-Debugdateien zu erstellen. Sie werden von BinSkim verwendet, um in der Ausgabebinärdatei gefundene Probleme erneut dem Quellcode zuzuordnen. 
2. Wählen Sie „Type **=** Basic“ und „Function **=** Analyze“ aus, um zu vermeiden, dass eine eigene Befehlszeile erstellt wird. 
3. **Target (Ziel)** : Mindestens ein Spezifizierer für eine Datei, ein Verzeichnis oder ein Filtermuster, der in mindestens eine zu analysierende Binärdatei aufgelöst wird. 
    - Mehrere Ziele sollten durch ein **Semikolon (;)** getrennt werden. 
    - Kann eine einzelne Datei sein oder Platzhalter enthalten.
    - Verzeichnisse sollten immer auf \* enden.
    - Beispiele:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;
4. Wenn Sie „Type = **Command Line**“ auswählen: 
     - Stellen Sie sicher, dass das erste Argument für **BinSkim.exe** das Verb **analyze** ist, für das vollständige oder relative Pfade zum Quellverzeichnis verwendet werden.
     - Bei **Command Line**-Eingaben (Befehlszeile) sollten mehrere Ziele durch ein Leerzeichen voneinander getrennt werden.
     - Sie können den Dateiparameter **/o** oder **/output** auslassen. Er wird für Sie hinzugefügt oder ersetzt. 
     - **Standardkonfiguration der Befehlszeile** 

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose
          > [!NOTE]
          > Das nachstehende \* ist sehr wichtig, wenn Sie ein oder mehrere Verzeichnisse für das Ziel angeben. 

Weitere Informationen zu BinSkim hinsichtlich Befehlszeilenargumenten, Regeln nach ID oder Exitcodes finden Sie im [BinSkim-Benutzerhandbuch](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md).

## <a name="credential-scanner-task"></a>Credential Scanner-Task
Screenshot und Details zum Konfigurieren finden Sie unten.
 
![Anpassung von Credential Scanner](./media/security-tools/3-taskdetails.png)

Folgende Optionen sind verfügbar: 
  - **Output Format (Ausgabeformat)** : TSV/CSV/Sarif/PREfast
  - **Tool Version (Toolversion)** (Recommended: Latest) (Empfohlen: Aktuellste)
  - **Scan Folder (Zu überprüfender Ordner)** : Der Ordner in Ihrem Repository, der überprüft werden soll.
  - **Searchers File Type (Suchroutinen-Dateityp)** : Optionen zum Suchen nach der zum Scannen verwendeten Suchroutinendatei.
  - **Suppressions File (Unterdrückungsdatei)** : Eine [JSON](https://json.org/)-Datei kann zum Unterdrücken von Problemen im Ausgabeprotokoll verwendet werden (weitere Informationen finden Sie im Abschnitt „Ressourcen“). 
  - **Verbose Output (Ausführliche Ausgabe)** : Selbsterklärend. 
  - **Batch Size (Batchgröße)** : Die Anzahl von parallelen Threads, die verwendet werden, um Credential Scanner parallel auszuführen. Der Standardwert ist 20 (der Wert muss im Bereich zwischen 1 und 2.147.483.647 liegen).
  - **Match Timeout (Übereinstimmungstimeout)** : Die Zeitspanne, in der die Suchroutine versucht, eine Übereinstimmung zu finden, bevor die Überprüfung abgebrochen wird. 
  - **File Scan Read Buffer Size (Lesepuffergröße der Dateiüberprüfung)** : Die Puffergröße beim Lesen des Inhalts in Bytes. (Der Standardwert ist 524.288.) 
  - **Maximum File Scan Read Bytes (Maximale Anzahl gelesene Bytes der Dateiüberprüfung)** : Die maximale Anzahl von Bytes, die während der Inhaltsanalyse aus einer bestimmten Datei gelesen werden. (Der Standardwert ist 104.857.600.) 
  - **Run this task (Diesen Task ausführen)** (unter **Control Options (Steuerungsoptionen)** ): Gibt an, wann der Task ausgeführt werden soll. Wählen Sie „Custom Conditions“ (Benutzerdefinierte Bedingungen) aus, um komplexere Bedingungen festzulegen. 
  - **Version**: Die Buildtaskversion in Azure DevOps. Wird nicht häufig verwendet. 

## <a name="microsoft-security-risk-detection-task"></a>Microsoft-Task zum Erkennen von Sicherheitsrisiken
> [!NOTE]
> Sie müssen ein Konto mit dem Risikoerkennungsdienst erstellen und konfigurieren, um diesen Task verwenden zu können. Dieser Dienst erfordert einen separaten Onboardingprozess. Er ist nicht von Typ Plug-and-Play“ wie die meisten anderen Tasks in dieser Erweiterung. Anleitungen dazu finden Sie unter [Microsoft-Sicherheitsrisikoerkennung](https://aka.ms/msrddocs) und [Microsoft-Sicherheitsrisikoerkennung: Vorgehensweise](https://docs.microsoft.com/security-risk-detection/how-to/).

Details zum Konfigurieren finden Sie unten.

Geben Sie die erforderlichen Daten ein. Jede Option verfügt über Hovertexthilfe.
   - **Azure DevOps-Dienstendpunktname für MSRD**: Wenn Sie einen generischen Typ von Azure DevOps-Dienstendpunkt erstellt haben, um die URL der MSRD-Instanz (für die Sie das Onboarding durchgeführt haben) und das REST-API-Zugriffstoken zu speichern, können Sie diesen Dienstendpunkt auswählen. Wenn dies nicht der Fall ist, klicken Sie auf den Link „Manage“ (Verwalten), um einen neuen Dienstendpunkt für diesen MSRD-Task zu erstellen und zu konfigurieren. 
   - **Account ID (Konto-ID)** : Dabei handelt es sich um eine GUID, die aus der URL des MSRD-Kontos abgerufen werden kann.
   - **URLs to Binaries (URLs für Binärdateien)** : Eine durch Semikolons getrennte Liste der öffentlich verfügbaren URLs (die vom Fuzzingcomputer zum Herunterladen der Binärdateien verwendet werden).
   - **URLs of the Seed Files (URLs der Seeddateien)** : Eine durch Semikolons getrennte Liste der öffentlich verfügbaren URLs (die vom Fuzzingcomputer zum Herunterladen der Seeds verwendet werden). Dieses Feld ist optional, wenn die Seeddateien zusammen mit den Binärdateien heruntergeladen werden.
   - **OS Platform Type (Typ der Betriebssystemplattform)** : Der Typ der Betriebssystemplattform (Windows oder Linux) der Computer, auf denen der Fuzzingauftrag ausgeführt werden soll.
   - **Windows Edition/Linux Edition**: Die Betriebssystemedition der Computer, auf denen der Fuzzingauftrag ausgeführt werden soll. Sie können den Standardwert überschreiben, wenn Ihre Computer eine andere Betriebssystemedition aufweisen.
   - **Package Installation Script (Paketinstallationsskript)** : Stellen Sie Ihr Skript zur Verfügung, das auf einem Testcomputer ausgeführt werden soll, um das Testzielprogramm und seine Abhängigkeiten zu installieren, bevor Sie den Fuzzingauftrag senden.
   - **Job Submission Parameters (Parameter für die Auftragsübermittlung)** :
       - **Seed Directory (Seedverzeichnis)** : Der Pfad zum Verzeichnis auf dem Fuzzingcomputer, das die Seeds enthält.
       - **Seed Extension (Seederweiterung)** : Die Dateierweiterung der Seeds.
       - **Test Driver Executable (Ausführbare Testtreiberdatei)** : Der Pfad zur ausführbaren Zieldatei auf dem Fuzzingcomputer.
       - **Test Driver Executable Architecture (Architektur der ausführbaren Testtreiberdatei)** : Die Architektur der ausführbaren Zieldatei (x86 oder amd64).
       - **Test Driver Arguments (Testtreiberargumente)** : Die Befehlszeilenargumente, die an die ausführbare Zieltestdatei übermittelt werden. Beachten Sie, dass das Symbol "%testfile%" (einschließlich der doppelten Anführungszeichen) automatisch durch den vollständigen Pfad zur Zieldatei ersetzt wird, die der Testtreiber analysieren soll. Es ist erforderlich.
       - **Test Driver Process Exits Upon Test Completion (Testtreiberprozess wird nach Abschluss des Tests beendet)** : Aktivieren Sie diese Option, um den Testtreiber nach Abschluss zu beenden. Deaktivieren Sie sie, wenn der Testtreiber zwangsweise geschlossen werden muss.
       - **Maximum Duration (in seconds) (Maximale Dauer (in Sekunden))** : Stellen Sie eine Schätzung der längsten sinnvollerweise erwarteten Zeit zur Verfügung, die das Zielprogramm benötigt, um eine Eingabedatei zu analysieren. Je genauer diese Schätzung ist, desto effizienter ist die Fuzzingausführung.
       - **Test Driver Can Be Run Repeatedly (Testtreiber kann wiederholt ausgeführt werden)** : Aktivieren Sie diese Option, wenn der Testtreiber wiederholt ohne Abhängigkeit von einem persistent gespeicherten bzw. freigegebenen globalen Zustand ausgeführt werden kann.
       - **Test Driver Can Be Renamed (Testtreiber kann umbenannt werden)** : Aktivieren Sie diese Option, wenn die ausführbare Datei des Testtreibers umbenannt werden kann und trotzdem weiterhin ordnungsgemäß funktioniert.
       - **The Fuzzing Application Runs as a Single OS Process (Die Fuzzinganwendung wird als einzelner Betriebssystemprozess ausgeführt)** : Aktivieren Sie diese Option, wenn der Testtreiber unter einem einzelnen Betriebssystemprozess ausgeführt wird. Deaktivieren Sie diese Option, wenn der Testtreiber weitere Prozesse erzeugt.


## <a name="roslyn-analyzers-task"></a>Roslyn Analyzers-Task
> [!NOTE]
> Als Voraussetzung für die Ausführung des Roslyn Analyzer-Tasks sollte der Build die folgenden Bedingungen erfüllen.
>  - Ihre Builddefinition enthält den integrierten MSBuild- oder VSBuild-Buildtask zum Kompilieren von C#-Code (oder VB-Code). Dieser Task basiert auf der Eingabe und Ausgabe dieses spezifischen Buildtasks, um die MSBuild-Kompilierung mit aktivierten Roslyn Analyzers erneut auszuführen.
>  - Auf dem Build-Agent, der diese Buildaufgabe durchführt, ist Visual Studio 2017 v 15.5 oder höher (Compilerversion 2.6.x) installiert.
>

Details zum Konfigurieren finden Sie unten.

Folgende Optionen sind verfügbar: 
- **Ruleset (Regelsatz)** : SDL Required (SDL erforderlich), SDL Recommended (SDL empfohlen), oder Sie können einen eigenen benutzerdefinierten Regelsatz verwenden.
- **Analyzers Version** (Recommended: Latest) (Empfohlen: Aktuellste)
- **Compiler Warnings Suppressions File (Unterdrückungsdatei für Compilerwarnungen)** : Eine Textdatei mit einer Liste von Warnungs-IDs, die unterdrückt werden sollen. 
- **Run this task (Diesen Task ausführen)** (unter **Control Options (Steuerungsoptionen)** ): Gibt an, wann der Task ausgeführt werden soll. Wählen Sie **Custom Conditions** (Benutzerdefinierte Bedingungen) aus, um komplexere Bedingungen festzulegen. 

> [!NOTE]
> - Roslyn-Analysen sind in den Compiler integriert und können nur im Rahmen der Kompilierung von „csc.exe“ ausgeführt werden. Daher muss für diesen Task der Compilerbefehl, der zuvor im Build ausgeführt wurde, erneut wiedergegeben bzw. ausgeführt werden. Dies geschieht durch Abfragen der MSBuild-Buildtaskprotokolle aus VSTS (es gibt keine andere Möglichkeit für den Task, die MSBuild-Kompilierungsbefehlszeile zuverlässig aus der Builddefinition abzurufen). Wir haben in Erwägung gezogen, ein Freiformtextfeld hinzuzufügen, damit Benutzer ihre Befehlszeilen eingeben können, aber es wäre schwierig, diese aktuell und synchron mit dem Hauptbuild zu halten. Benutzerdefinierte Builds erfordern das Wiedergeben des gesamten Befehlssatzes, nicht nur der Compilerbefehle. Es ist nicht trivial/zuverlässig, in diesen Fällen Roslyn-Analysen zu aktivieren. 
> - Roslyn-Analysen sind in den Compiler integriert und erfordern, dass die Kompilierung aufgerufen wird. Dieser Buildtask wird durch erneutes Kompilieren von C#-Projekten implementiert, die bereits nur mit dem MSBuild-/VSBuild-Buildtask im gleichen Build bzw. der gleichen Builddefinition erstellt wurden, in diesem Fall jedoch mit aktivierten Analyzern. Wenn dieser Buildtask auf dem gleichen Agent wie der ursprüngliche Buildtask ausgeführt wird, wird die Ausgabe des ursprünglichen MSBuild-/VSBuild-Buildtasks im Quellordner „s“ durch die Ausgabe dieses Buildtasks überschrieben. Die Buildausgabe ist identisch, aber es wird empfohlen, dass Sie MSBuild ausführen, die Ausgabe in das Artefaktenstagingverzeichnis kopieren und dann Roslyn ausführen.
>

Weitere Ressourcen für den Roslyn Analyzers-Task finden Sie unter [Roslyn Analyzers in der Microsoft-Dokumentation](https://docs.microsoft.com/dotnet/standard/analyzers/)

Das von diesem Buildtask installierte und verwendete Analyzer-Paket finden Sie [hier](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers). 

## <a name="tslint-task"></a>TSLint-Task

Weitere Informationen zu TSLint finden Sie im [TSLint GitHub-Repository](https://github.com/palantir/tslint).
>[!NOTE] 
>Wie Sie vielleicht wissen, wird TSLint im Jahr 2019 als veraltet markiert (Quelle: [TSLint GitHub-Repository](https://github.com/palantir/tslint)). Das Team untersucht derzeit [ESLint](https://github.com/eslint/eslint) als Alternative, und das Erstellen eines neuen Tasks für ESLint ist in der Roadmap enthalten.

## <a name="publish-security-analysis-logs-task"></a>Task zum Veröffentlichen von Sicherheitsanalyseprotokollen
Screenshot und Details zum Konfigurieren finden Sie unten.

![Anpassen der Veröffentlichung von Sicherheitsanalysen](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Artifact name (Artefaktname)** : Kann ein beliebiger Zeichenfolgenbezeichner sein.
- **Artifact Type (Artefakttyp)** : Sie können Protokolle auf dem Azure DevOps-Server oder auf einer Dateifreigabe veröffentlichen, die für den Build-Agent zugänglich ist. 
- **Tools**: Sie können auswählen, ob Sie Protokolle für einzelne/spezifische Tools beibehalten möchten oder **All Tools** (Alle Tools) auswählen, um alle Protokolle beizubehalten. 

## <a name="security-report-task"></a>Sicherheitsbericht-Task
Screenshot und Details zum Konfigurieren finden Sie unten.  
![Optionen nach der Analyse](./media/security-tools/4-createsecurityanalysisreport600.png) 
- **Reports (Berichte)** : Wählen Sie die zu erstellenden Berichtsdateien aus. Es wird jeweils eine Datei im Format **Console**, **TSV** und/oder **HTML erstellt**. 
- **Tools**: Wählen Sie die Tools in der Builddefinition aus, für die eine Zusammenfassung der erkannten Probleme angezeigt werden soll. Für jedes ausgewählte Tool gibt es möglicherweise eine Option, mit der Sie auswählen können, ob Sie nur Fehler oder sowohl Fehler als auch Warnungen im Bericht anzeigen möchten. 
- **Advanced Options (Erweiterte Optionen)** : Sie können auswählen, ob eine Warnung oder ein Fehler protokolliert werden soll (und damit der Task fehlschlägt), wenn keine Protokolle für eines der ausgewählten Tools vorhanden sind.
Sie können den Ordner für Basisprotokolle anpassen, in dem die Protokolle gespeichert werden. Dies ist jedoch kein typisches Szenario. 

## <a name="post-analysis-task"></a>Task nach der Analyse
Screenshot und Details zum Konfigurieren finden Sie unten.

![Anpassen der Optionen nach der Analyse](./media/security-tools/a-post-analysis600.png) 
- **Tools**: Wählen Sie die Tools in der Builddefinition aus, für die Sie eine Buildunterbrechung auf der Grundlage der Funde veranlassen möchten. Für jedes ausgewählte Tool gibt es möglicherweise eine Option, mit der Sie auswählen können, ob Sie nur bei Fehlern oder sowohl bei Fehlern als auch bei Warnungen eine Unterbrechung veranlassen möchten. 
- **Report (Bericht)** : Sie können optional die gefundenen Ergebnisse, die den Buildabbruch verursachen, in das Azure DevOps-Konsolenfenster und die Protokolldatei schreiben. 
- **Advanced Options (Erweiterte Optionen)** : Sie können auswählen, ob eine Warnung oder ein Fehler protokolliert werden soll (und damit der Task fehlschlägt), wenn keine Protokolle für eines der ausgewählten Tools vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Fragen zur Erweiterung und zu den angebotenen Tools haben, [lesen Sie unsere FAQs-Seite.](security-code-analysis-faq.md)


