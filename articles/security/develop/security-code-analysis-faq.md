---
title: FAQ zur Microsoft Azure-Dokumentation zur Sicherheitscodeanalyse
description: Dieser Artikel enthält häufig gestellte Fragen zur Erweiterung „Sicherheitscodeanalyse“.
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
ms.openlocfilehash: 1fc5e83c2c46a7da2a4b56879a2d596405fd8bee
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718177"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
Haben Sie Fragen? Weitere Informationen finden Sie in den nachfolgenden FAQs.

## <a name="general-faqs"></a>Allgemeine FAQs

### <a name="can-i-install-the-extension-on-my-tfs-not-azure-devops-server"></a>Kann ich die Erweiterung auf meinem TFS-Server (nicht Azure DevOps-Server) installieren? 

Nein, die Erweiterung steht nicht zum Herunterladen und Installieren für TFS zur Verfügung.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>Muss ich die Microsoft-Sicherheitscodeanalyse mit meinem Build ausführen? 

Ja und Nein. Abhängig vom Typ des Analysetools ist der Quellcode selbst möglicherweise das Einzige, was erforderlich ist, oder die Ausgabe des Builds ist ggf. erforderlich. Da Credential Scanner beispielsweise Dateien innerhalb der Ordnerstruktur des Coderepositorys analysiert, können Sie Credential Scanner ausführen und Buildtasks der Sicherheitsanalyseprotokolle in einem eigenständigen Build veröffentlichen, um Ergebnisse abzurufen.
Für andere Tools, die Artefakte nach dem Build analysieren (etwa BinSkim), wird zuerst der Build benötigt.

### <a name="can-i-break-my-build-when-results-are-found"></a>Kann ich meinen Build abbrechen, wenn Ergebnisse gefunden werden? 
Ja, Sie können eine Buildunterbrechung vornehmen, wenn ein Tool ein Problem (einen Fund) in seiner Protokolldatei meldet. Fügen Sie einfach den Buildtask nach der Analyse hinzu, und aktivieren Sie das Kontrollkästchen für jedes Tool, das den Build unterbrechen soll. Sie können den Build unterbrechen, wenn ein beliebiges Tool Fehler oder Warnungen und Fehler in der Benutzeroberfläche des Tasks nach der Analyse meldet.

### <a name="how-are-the-command-line-arguments-different-in-azure-devops-than-they-are-in-the-standalone-desktop-tools"></a>Wie unterscheiden sich die Befehlszeilenargumente in Azure DevOps von den eigenständigen Desktoptools? 

In den meisten Fällen sind die Azure DevOps-Buildtasks direkte Wrapper um die Befehlszeilenargumente der Sicherheitstools. Alles, was Sie normalerweise in der Befehlszeile von Ihrem Desktop an das Tool übergeben würden, können Sie an die Argumenteingabe des Buildtasks übergeben.
Im Folgenden finden Sie eine Liste der wichtigsten Unterschiede:
 - Das Tool wird aus dem Quellordner des Agents „$(Build.SourcesDirectory)“ oder „%BUILD_SOURCESDIRECTORY%“ ausgeführt. Beispiel: C:\agent\_work\1\s 
 - Pfade in den Argumenten können relativ zum Stammverzeichnis des oben aufgeführten Quellverzeichnisses oder absolut sein, entweder durch Ausführen eines lokalen Agents mit bekannten Bereitstellungsspeicherorten lokaler Ressourcen oder durch Verwendung von Azure DevOps-Buildvariablen.
 - Tools stellen automatisch einen Ausgabedateipfad oder -ordner zur Verfügung. Wenn ein Ausgabepfad angegeben wird, dieser wird entfernt und durch einen Pfad zu unserem bekannten Protokollspeicherort für den Build-Agent ersetzt.
 - Einige zusätzliche Befehlszeilenparameter werden bei einigen Tools bereinigt und entfernt (z.B. das Hinzufügen oder Entfernen von Optionen), um sicherzustellen, dass keine grafische Benutzeroberfläche gestartet wird.

### <a name="can-i-run-a-build-task-for-example-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>Kann ich einen Buildtask (z.B. Credential Scanner) über mehrere Repositorys hinweg in einem Azure DevOps-Build ausführen? 

Nein, das Ausführen der sicheren Entwicklungstools für mehrere Repositorys in einer einzigen Pipeline wird derzeit nicht unterstützt.

###  <a name="the-output-file-i-specified-is-not-being-created--i-cant-find-the-output-file-i-specified"></a>Die angegebene Ausgabedatei wird nicht erstellt bzw. ich kann die angegebene Ausgabedatei nicht finden 

Die BuildTasks bereinigen aktuell die Benutzereingabe und aktualisieren den Speicherort der generierten Ausgabedatei in einen allgemeinen Speicherort für den Build-Agent. Weitere Informationen zu diesem Speicherort finden Sie in den folgenden Fragen.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>Wo werden die von den Tools generierten Ausgabedateien gespeichert? 

Die Buildtasks fügen automatisch Ausgabepfade zum folgenden bekannten Speicherort für den Build-Agent („$(Agent.BuildDirectory)\_sdt\logs“) hinzu. Durch die Standardisierung an diesem Standort können wir garantieren, dass andere Teams, die Codeanalyseprotokolle erstellen oder nutzen, Zugriff haben.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>Kann ich einen Build in einer Warteschlange speichern, damit diese Tasks auf einem gehosteten Build-Agent ausgeführt werden? 

Ja, alle Tasks und Tools in der Erweiterung können auf einem gehosteten Build-Agent ausgeführt werden.

>[!NOTE]
> Der Buildtask für Antischadsoftware erfordert einen Build-Agent mit aktiviertem Windows Defender. Dies gilt für „Hosted VS2017“ Build-Agents oder höher. (Der Task wird nicht auf dem Legacy-“Hosted“- Agent VS2015 ausgeführt.) Signaturen können für diese Agents nicht aktualisiert werden, aber die Signatur sollte immer relativ aktuell und weniger als 3 Stunden alt sein.
>

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>Kann ich diese Buildtasks als Teil einer Releasepipeline ausführen (im Gegensatz zu einer Buildpipeline)? 
In den meisten Fällen ist dies möglich. Die Ausführung von Tasks in Releasepipelines, die Artefakte veröffentlichen, wird von Azure DevOps jedoch nicht unterstützt: „Die einzige Kategorie von Tasks, von denen nicht erwartet wird, dass sie mit Releasepipelines funktionieren, sind die Tasks, die Artefakte veröffentlichen. Dies liegt daran, dass wir zurzeit keine Unterstützung für die Veröffentlichung von Artefakten innerhalb von Releasepipelines bieten.“
Dadurch wird verhindert, dass der Task „Publish Security Analysis Logs“ (Sicherheitsanalyseprotokolle veröffentlichen) erfolgreich aus einer Releasepipeline ausgeführt werden kann. Es tritt ein Fehler mit einer beschreibenden Fehlermeldung auf.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>Von wo laden die Buildtasks die Tools herunter? 
Die Buildtasks laden a) NuGet-Pakete für die Tools aus dem folgenden [Azure DevOps-Paketverwaltungfeed](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json) oder mithilfe von npm (Node Package Manager) herunter, der auf dem Build-Agent vorinstalliert werden muss (Beispiel: „npm install tslint“).

### <a name="what-effect-will-installing-the-extension-have-on-my-azure-devops-organization"></a>Welche Auswirkung hat die Installation der Erweiterung auf meine Azure DevOps-Organisation? 

Nach der Installation werden die von der Erweiterung bereitgestellten Sicherheitsbuildtasks für alle Benutzer in Ihrem Unternehmen verfügbar. Beim Erstellen oder Bearbeiten einer Azure-Pipeline können diese Tasks aus der Liste der Buildtasksammlung hinzugefügt werden. Die Installation der Erweiterung in ihrer Azure DevOps-Organisation hat keinerlei sonstige Auswirkungen. Konto- bzw. Projekteinstellungen oder Pipelines werden nicht geändert.

### <a name="will-installing-the-extension-modify-my-existing-azure-pipelines"></a>Werden die vorhandenen Azure-Pipelines durch Installieren der Erweiterung geändert? 

Nein. Wenn Sie die Erweiterung installieren, werden die Sicherheitsbuildtasks zum Hinzufügen zu Ihren Azure-Pipelines verfügbar. Benutzer müssen weiterhin Builddefinitionen hinzufügen oder aktualisieren, um die Tools in den Buildprozess zu integrieren.

## <a name="task-specific-faqs"></a>Taskspezifische FAQs

In diesem Abschnitt werden häufig gestellte Fragen zu Buildtasks aufgeführt.

### <a name="credential-scanner-faqs"></a>FAQ zum Credential Scanner

#### <a name="what-are-common-suppressions-scenarios-and-examples"></a>Was sind allgemeine Unterdrückungsszenarien und Beispiele? 
Im Folgenden werden zwei der gängigsten Unterdrückungsszenarien beschrieben:
##### <a name="suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Unterdrücken aller Vorkommen eines angegebenen Geheimnisses innerhalb des angegebenen Pfads 
Der Hashschlüssel des Geheimnisses aus der Ausgabedatei von Credential Scanner ist erforderlich, wie im folgenden Beispiel gezeigt.
   
        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "hash": "CLgYxl2FcQE8XZgha9/UbKLTkJkUh3Vakkxh2CAdhtY=",
                "_justification": "Secret used by MSDN sample, it is fake."
            }
          ]
        }

>[!WARNING]
> Der Hashschlüssel wird von einem Teil des entsprechenden Werts oder Dateiinhalts generiert. Jede Quellcoderevision kann den Hashschlüssel ändern und die Unterdrückungsregel deaktivieren. 

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>So unterdrücken Sie alle Geheimnisse in einer angegebenen Datei (oder die Geheimnisdatei selbst) 
Der Dateiausdruck kann ein Dateiname oder ein beliebiger Postfixteil des vollständigen Dateipfads oder -namens sein. Platzhalter werden nicht unterstützt. 

**Beispiel** 

Zu unterdrückende Datei: [Eingabepfad]\src\JS\lib\angular.js 

Gültige Unterdrückungsregeln: 
- [Eingabepfad] \src\JS\lib\angular.js: Unterdrücken der Datei im angegebenen Pfad
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- angular.js: Unterdrücken aller Dateien mit demselben Namen

        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "file": "\\files\\AdditonalSearcher.xml", 
                "_justification": "Additional CredScan searcher specific to my team"
            },
            {
                "file": "\\files\\unittest.pfx", 
                "_justification": "Legitimate UT certificate file with private key"
            }
          ]
        }      

>[!WARNING] 
> Alle zukünftigen Geheimnisse, die der Datei hinzugefügt werden, werden ebenfalls automatisch unterdrückt. 

#### <a name="what-are-recommended-secrets-management-guidelines"></a>Welche Verwaltungsrichtlinien für Geheimnisse werden empfohlen? 
Es ist zwar hilfreich, hartcodierte Geheimnisse rechtzeitig zu erkennen und die Risiken zu minimieren, aber noch besser, wenn verhindert werden könnte, dass Geheimnisse überhaupt eingecheckt werden. In diesem Zusammenhang hat Microsoft CredScan Code Analyzer als Teil der [Microsoft DevLabs-Erweiterung](https://marketplace.visualstudio.com/items?itemName=VSIDEDevOpsMSFT.ContinuousDeliveryToolsforVisualStudio) für Visual Studio veröffentlicht. In der frühen Vorschau bietet er Entwicklern eine Inlineerfahrung, um potenzielle Geheimnisse in ihrem Code zu erkennen und ihnen die Möglichkeit zu geben, diese Probleme in Echtzeit zu beheben. Weitere Informationen finden Sie in [diesem](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/) Blog zum sicheren Verwalten von Geheimnissen in der Cloud. Unten finden Sie einige zusätzliche Ressourcen, mit denen Sie Geheimnisse verwalten und auf sichere Weise in Ihren Anwendungen auf sensible Informationen zugreifen können: 
 - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
 - [Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)
 - [Verwaltete Azure AD-Dienstidentität](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
 - [Verwaltete Azure-Dienstidentität](https://docs.microsoft.com/azure/app-service/overview-managed-identity)
 - [AppAuthentication-Bibliothek](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication)

#### <a name="can-i-write-my-own-custom-searchers"></a>Kann ich meine eigenen benutzerdefinierten Suchroutinen schreiben?

Credential Scanner basiert auf einem Satz von Inhaltssuchroutinen, die allgemein in der Datei **buildsearchers.xml** definiert sind. Die Datei enthält ein Array von serialisierten XML-Objekten, die ein ContentSearcher-Objekt darstellen. Das Programm wird mit einer Reihe von Suchroutinen verteilt, die ausführlich getestet wurden. Sie können jedoch auch eigene benutzerdefinierte Suchroutinen implementieren. 

Eine Inhaltssuchroutine wird wie folgt definiert: 

- **Name**: Der beschreibende Name der Suchroutine, der in der Ausgabedatei von Credential Scanner verwendet werden soll. Es wird empfohlen, die Camel-Case-Namenskonvention für Namen von Suchroutinen zu verwenden. 
- **RuleId**: Die stabile, opake ID der Suchroutine. 
    - Standardsuchroutinen von Credential Scanner werden RuleIds wie CSCAN0010, CSCAN0020, CSCAN0030 usw. zugewiesen. Die letzte Ziffer ist für das Mergen oder Unterteilen der RegEx-Gruppe der Suchroutine reserviert.
    - Die RuleId für angepasste Suchroutinen sollte über einen eigenen Namespace im folgenden Format verfügen: CSCAN-{Namespace}0010, CSCAN-{Namespace}0020, CSCAN-{Namespace}0030 usw.
    - Der vollqualifizierte Name der Suchroutine besteht aus der Kombination der RuleId mit dem Namen der Suchroutine. Beispiel: CSCAN0010.KeyStoreFiles, CSCAN0020.Base64EncodedCertificate usw.
- **ResourceMatchPattern**: RegEx der Dateierweiterungen, die anhand der Suchroutine überprüft werden sollen.
- **ContentSearchPatterns**: Array von Zeichenfolgen mit RegEx-Anweisungen, die abgeglichen werden sollen. Wenn keine Suchmuster definiert sind, werden alle Dateien zurückgegeben, die mit dem Ressourcenübereinstimmungsmuster übereinstimmen.
- **ContentSearchFilters**: Array von Zeichenfolgen, das RegEx-Anweisungen enthält, um suchroutinenspezifische falsch positive Ergebnisse zu filtern.
- **Matchdetails**: Eine beschreibende Nachricht und/oder Entschärfungsanweisungen, die für jede Übereinstimmung mit der Suchroutine hinzugefügt werden sollen.
- **Recommendation**: Stellt den Inhalt des Felds mit den Vorschlägen für eine Übereinstimmung im PREfast-Berichtsformat zur Verfügung.
- **Severity**: Ein Integerwert, der den Schweregrad des Problems angibt (höchster Wert = 1).
![Credential Scanner-Setup](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers-faqs"></a>FAQ zu Roslyn Analyzers

#### <a name="what-are-the-most-common-errors-when-using-the-roslyn-analyzers-task"></a>Welche Fehler treten am häufigsten bei der Verwendung des Roslyn Analyzers-Tasks auf?

**Error: The project was restored using Microsoft.NETCore.App version x.x.x, but with current settings, version y.y.y would be used instead (Das Projekt wurde mit Version x.x.x von Microsoft.NETCore.App wiederhergestellt, aber mit den aktuellen Einstellungen würde stattdessen Version y.y.y verwendet). Um dieses Problem zu beheben, stellen Sie sicher, dass die gleichen Einstellungen für die Wiederherstellung und für nachfolgende Vorgänge wie Build- oder Veröffentlichungsvorgänge verwendet werden. In der Regel kann dieses Problem auftreten, wenn die RuntimeIdentifier-Eigenschaft während des Builds oder der Veröffentlichung festgelegt wird, jedoch nicht während der Wiederherstellung:**

Roslyn-Analysen werden als Teil der Kompilierung ausgeführt, sodass sich die Quellstruktur auf dem Buildcomputer in einem buildfähigen Zustand befinden muss. Ein Schritt (wahrscheinlich „dotnet.exe publish“) zwischen Ihrem Hauptbuild und den Roslyn-Analysen hat möglicherweise die Quellstruktur in einen nicht buildfähigen Zustand versetzt. Wenn Sie den Schritt, der eine NuGet-Wiederherstellung durchführt, kurz vor dem Schritt „Roslyn Analyzers“ duplizieren, wird die Quellstruktur ggf. in einen buildbaren Zustand zurückversetzt.

**"csc.exe" exited with error code 1 -- An instance of analyzer AAAA cannot be created from C:\BBBB.dll: Could not load file or assembly 'Microsoft.CodeAnalysis, Version=X.X.X.X, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified. („csc. exe“ wurde mit dem Fehlercode 1 beendet -- eine Instanz von Analyzer AAAA kann nicht aus „C:\BBBB.dll“ erstellt werden: Die Datei oder Assembly „Microsoft.CodeAnalysis, Version=X.X.X.X, Culture=neutral, PublicKeyToken=31bf3856ad364e35“ oder eine Abhängigkeit davon wurde nicht gefunden. Das System kann die angegebene Datei nicht finden.)**

Stellen Sie sicher, dass der Compiler Roslyn Analyzers unterstützt. „csc.exe /version“ sollte mindestens v2.6.x melden. In einigen Fällen können einzelne CSPROJ-Dateien die Visual Studio-Installation des Buildcomputers überschreiben, indem sie auf ein Paket aus Microsoft.Net.Compilers verweisen. Wenn die Verwendung einer bestimmten Version des Compilers unbeabsichtigt war, entfernen Sie Verweise auf Microsoft.Net.Compilers. Stellen Sie andernfalls sicher, dass das referenzierte Paket auch v2.6.x oder höher ist. Versuchen Sie, das Fehlerprotokoll abzurufen, das Sie im Parameter /errorlog: in der csc.exe-Befehlszeile finden (im Protokoll des Roslyn-Buildtasks zu finden). Es könnte in etwa so aussehen: /errorlog:F:\ts-services-123\_work\456\s\Some\Project\Code\Code.csproj.sarif

**Der C#-Compiler ist nicht aktuell genug (er muss Version 2.6 oder höher aufweisen)**

Die neuesten Versionen des C#-Compilers werden hier veröffentlicht: https://www.nuget.org/packages/Microsoft.Net.Compilers. Um die installierte Version abzurufen, die Sie ausführen, führen Sie `C:\>csc.exe /version` über die Befehlszeile aus. Stellen Sie sicher, dass kein Verweis auf ein NuGet-Paket Microsoft.Net.Compilers vorhanden ist, das eine niedrigere Version als v 2.6 aufweist.

**MSBuild/VSBuild-Protokolle nicht gefunden**

Aufgrund der Funktionsweise des Tasks muss dieser Task Azure DevOps für das MSBuild-Protokoll aus dem MSBuild-Buildtask abfragen. Wenn dieser Task unmittelbar nach dem MSBuild-Buildtask ausgeführt wird, ist das Protokoll noch nicht verfügbar. Platzieren Sie andere Buildtasks (einschließlich SecDevTools-Buildtasks wie Binskim, Antimalware Scan und andere) zwischen dem MSBuild-Buildtask und dem Roslyn Analyzers-Buildtask. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie zusätzliche Unterstützung benötigen, ist Support für die Microsoft-Sicherheitscodeanalyse von Montag bis Freitag zwischen 9:00 Uhr und 17:00 Uhr Pacific Normalzeit verfügbar.

  - Onboarding: Wenden Sie sich an Ihre technischen Konto-Manager, um zu beginnen. 
  >[!NOTE] 
  >Wenn Sie noch keine kostenpflichtige Supportbeziehung zu Microsoft haben oder ein Supportangebot nutzen, das es Ihnen nicht erlaubt, Dienste aus dem Phoenix-Katalog zu erwerben, besuchen Sie unsere [Supportservices-Homepage](https://www.microsoft.com/enterprise/services/support), um weitere Informationen zu erhalten.

  - Support: Senden Sie eine E-Mail an unser Team unter [Support für die Microsoft-Sicherheitscodeanalyse](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request)