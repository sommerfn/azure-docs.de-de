---
title: Dokumentation zur Microsoft-Sicherheitscodeanalyse – häufig gestellte Fragen
description: Dieser Artikel enthält häufig gestellte Fragen zur Erweiterung „Microsoft-Sicherheitscodeanalyse“.
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
ms.openlocfilehash: 846f0ecdd49fc1c501893209b60fa9acc8a32ed2
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70242331"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
Haben Sie Fragen? Weitere Informationen finden Sie in den folgenden häufig gestellten Fragen.

## <a name="general-faq"></a>Allgemeine häufig gestellte Fragen

### <a name="can-i-install-the-extension-on-my-visual-studio-team-foundation-server-instance-instead-of-on-an-azure-devops-instance"></a>Kann ich die Erweiterung für meine Visual Studio Team Foundation Server-Instanz statt für eine Azure DevOps-Instanz installieren?

Nein. Die Erweiterung kann nicht für Visual Studio Team Foundation Server heruntergeladen und installiert werden.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>Muss ich die Microsoft-Sicherheitscodeanalyse mit meinem Build ausführen? 

Vielleicht. Das hängt vom Typ des Analysetools ab. Möglicherweise ist der Quellcode das einzige, was erforderlich ist, ggf. wird aber auch die Buildausgabe benötigt.

Beispielsweise analysiert Credential Scanner (CredScan) Dateien in der Ordnerstruktur des Coderepositorys. Daher können Sie CredScan und Buildtasks für die Veröffentlichung von Sicherheitsanalyseprotokollen in einem eigenständigen Build ausführen, um Ergebnisse abzurufen.

Für andere Tools wie BinSkim, die Artefakte nach dem Build analysieren, wird zuerst der Build benötigt.

### <a name="can-i-break-my-build-when-results-are-found"></a>Kann ich meinen Build abbrechen, wenn Ergebnisse gefunden werden?

Ja. Sie können eine Buildunterbrechung vornehmen, wenn ein Tool einen Issue oder ein Problem in seiner Protokolldatei erfasst. Fügen Sie einfach den Buildtask nach der Analyse hinzu, und aktivieren Sie das Kontrollkästchen für jedes Tool, das den Build unterbrechen soll.

Sie können den Build in der Benutzeroberfläche des Tasks nach der Analyse unterbrechen, wenn ein beliebiges Tool nur Fehler oder sowohl Warnungen als auch Fehler meldet.

### <a name="how-do-the-command-line-arguments-in-azure-devops-differ-from-those-arguments-in-the-standalone-desktop-tools"></a>Wie unterscheiden sich die Befehlszeilenargumente in Azure DevOps von den Argumenten in den eigenständigen Desktoptools? 

In den meisten Fällen sind die Azure DevOps-Buildtasks direkte Wrapper um die Befehlszeilenargumente der Sicherheitstools. Alles, was Sie normalerweise an ein Befehlszeilentool übergeben würden, können Sie als Argumente an einen Buildtask übergeben.

Die wichtigsten Unterschiede:

- Tool werden aus dem Agent-Quellordner $(Build.SourcesDirectory) oder aus %BUILD_SOURCESDIRECTORY% ausgeführt. Beispiel: C:\agent\_work\1\s.
- Pfade in den Argumenten können relativ zum Stammverzeichnis des oben aufgeführten Quellverzeichnisses oder absolut sein. Sie erhalten absolute Pfade entweder mithilfe von Azure DevOps-Buildvariablen oder durch Ausführen eines lokalen Agents, der über bekannte Bereitstellungsspeicherorte für lokale Ressourcen verfügt.
- Tools stellen automatisch einen Ausgabedateipfad oder -ordner zur Verfügung. Wenn Sie einen Ausgabespeicherort für einen Buildtask angeben, wird dieser entfernt und durch einen Pfad zum bekannten Protokollspeicherort auf dem Build-Agent ersetzt.
- Für einige Tools werden einige zusätzliche Befehlszeilenargumente geändert. Beispielsweise werden Optionen hinzugefügt oder entfernt, durch die sichergestellt wird, dass keine GUI gestartet wird.

### <a name="can-i-run-a-build-task-like-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>Kann ich einen Buildtask wie Credential Scanner über mehrere Repositorys hinweg in einem Azure DevOps-Build ausführen?

Nein. Das Ausführen der sicheren Entwicklungstools für mehrere Repositorys in einer einzigen Pipeline wird nicht unterstützt.

### <a name="the-output-file-i-specified-isnt-being-created-or-i-cant-find-the-output-file-i-specified"></a>Die angegebene Ausgabedatei wird nicht erstellt bzw. ich kann die angegebene Ausgabedatei nicht finden.

Durch Buildtasks werden einige Benutzereingaben gefiltert. Konkret zu dieser Frage: Buildtasks aktualisieren den Speicherort der generierten Ausgabedatei, sodass er einem üblichen Speicherort auf dem Build-Agent entspricht. Weitere Informationen zu diesem Speicherort finden Sie in den folgenden Fragen.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>Wo werden die von den Tools generierten Ausgabedateien gespeichert? 

Die Buildtasks fügen automatisch Ausgabepfade zum bekannten Speicherort auf dem Build-Agent hinzu: $(Agent.BuildDirectory)\_sdt\logs. Durch die standardmäßige Verwendung dieses Standort haben alle Teams, die Codeanalyseprotokolle erstellen oder nutzen, Zugriff auf die Ausgabe.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>Kann ich einen Build in einer Warteschlange speichern, damit diese Tasks auf einem gehosteten Build-Agent ausgeführt werden? 

Ja. Alle Tasks und Tools in der Erweiterung können auf einem gehosteten Build-Agent ausgeführt werden.

>[!NOTE]
> Der Schadsoftwarescanner-Buildtask erfordert einen Build-Agent mit aktiviertem Windows Defender. Ein solcher Agent ist ab Hosted Visual Studio 2017 verfügbar. Der Buildtask kann nicht unter Verwendung des gehosteten Visual Studio 2015-Agents ausgeführt werden.
>
> Obwohl Signaturen für diese Agents nicht aktualisiert werden können, sollten sie immer weniger als drei Stunden alt sein.

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>Kann ich diese Buildtasks als Teil einer Releasepipeline ausführen (im Gegensatz zu einer Buildpipeline)?

In den meisten Fällen ist dies möglich.

Die Taskausführung in Releasepipelines wird von Azure DevOps jedoch nicht unterstützt, wenn von diesen Tasks Artefakte veröffentlicht werden. Dadurch wird verhindert, dass der Task zur Veröffentlichung von Sicherheitsanalyseprotokollen erfolgreich in einer Releasepipeline ausgeführt werden kann. Es tritt ein Fehler auf, und eine Fehlermeldung wird ausgegeben.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>Von wo laden die Buildtasks die Tools herunter?

Buildtasks können die NuGet-Pakete der Tools aus dem [Azure DevOps-Paketverwaltungsfeed](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json) herunterladen. Buildtasks können auch den Knotenpaket-Manager verwenden, der auf dem Build-Agent vorinstalliert werden muss. Ein Beispiel für eine solche Installation ist der Befehl **npm install tslint**.

### <a name="what-effect-does-installing-the-extension-have-on-my-azure-devops-organization"></a>Welche Auswirkung hat die Installation der Erweiterung auf meine Azure DevOps-Organisation? 

Nach der Installation werden die von der Erweiterung bereitgestellten Sicherheitsbuildtasks für alle Benutzer im Unternehmen verfügbar. Beim Erstellen oder Bearbeiten einer Azure-Pipeline sind diese Tasks in der Buildtask-Sammlungsliste verfügbar. Die Installation der Erweiterung in Ihrer Azure DevOps-Organisation hat ansonsten keinerlei Auswirkungen. Bei der Installation werden Kontoeinstellungen, Projekteinstellungen oder Pipelines nicht geändert.

### <a name="does-installing-the-extension-modify-my-existing-azure-pipelines"></a>Werden die vorhandenen Azure-Pipelines durch Installieren der Erweiterung geändert? 

Nein. Wenn Sie die Erweiterung installieren, werden Sicherheitsbuildtasks verfügbar und können Pipelines hinzugefügt werden. Benutzer müssen weiterhin Builddefinitionen hinzufügen oder aktualisieren, damit die Tools mit dem Buildprozess interagieren können.

## <a name="task-specific-faq"></a>Taskbezogene FAQs

In diesem Abschnitt werden Fragen zu Buildtasks behandelt.

### <a name="credential-scanner"></a>Credential Scanner

#### <a name="what-are-common-suppression-scenarios-and-examples"></a>Was sind allgemeine Unterdrückungsszenarien und wie sehen sie in der Praxis aus?

Im Folgenden finden Sie Details zu zwei der gängigsten Unterdrückungsszenarien:

##### <a name="to-suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Unterdrücken aller Vorkommen eines angegebenen Geheimnisses innerhalb des angegebenen Pfads

Der Hashschlüssel des Geheimnisses aus der CredScan-Ausgabedatei ist erforderlich, wie im folgenden Beispiel gezeigt.

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
> Der Hashschlüssel wird von einem Teil des entsprechenden Werts oder Dateiinhalts generiert. Durch jede Quellcoderevision kann der Hashschlüssel geändert und die Unterdrückungsregel deaktiviert werden.

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>So unterdrücken Sie alle Geheimnisse in einer angegebenen Datei oder die Geheimnisdatei selbst

Der Dateiausdruck kann ein Dateiname sein. Darüber hinaus kann er der Basename-Teil eines vollständigen Dateipfads oder eines Dateinamens sein. Platzhalter werden nicht unterstützt.

In den folgenden Beispielen wird gezeigt, wie Sie die Datei „\<Eingabepfad>\src\JS\lib\angular.js“ unterdrücken.

Beispiele für gültige Unterdrückungsregeln:

- \<Eingabepfad>\src\JS\lib\angular.js: unterdrückt die Datei im angegebenen Pfad
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- angular.js: unterdrückt alle Dateien mit dem gleichen Namen

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

#### <a name="what-are-recommended-guidelines-for-managing-secrets"></a>Welche Verwaltungsrichtlinien für Geheimnisse werden empfohlen?

Es ist hilfreich, hartcodierte Geheimnisse rechtzeitig zu erkennen und die Risiken zu minimieren. Aber noch besser ist es, das Einchecken von Geheimnissen ganz zu verhindern.

In diesem Zusammenhang hat Microsoft eine frühe Vorschau von Credential Scanner Code Analyzer als Teil der [Microsoft DevLabs-Erweiterung](https://marketplace.visualstudio.com/items?itemName=VSIDEDevOpsMSFT.ContinuousDeliveryToolsforVisualStudio) für Visual Studio veröffentlicht. Das Analysetool ist eine frühe Vorschauversion. Es bietet Entwicklern eine Inline-Erfahrung, um potenzielle Geheimnisse im Code zu entdecken. Auf diese Weise gibt das Analysetool Entwicklern auch die Möglichkeit, diese Probleme in Echtzeit zu beheben.

Weitere Informationen finden Sie im Blogbeitrag zum [sicheren Verwalten von Geheimnissen in der Cloud](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/).

Die folgenden Ressourcen helfen Ihnen, Geheimnisse sicher zu verwalten und von Anwendungen aus sicher auf vertrauliche Informationen zuzugreifen:

 - [Azure Key Vault](../../key-vault/index.yml)
 - [Azure Active Directory (Azure AD)](../../sql-database/sql-database-aad-authentication.md)
 - [Verwaltete Dienstidentität (Managed Service Identity, MSI) in Azure AD](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Verwaltete Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md)
 - [Verwaltete Identitäten im Azure App Service und Azure Functions](../../app-service/overview-managed-identity.md)
 - [AppAuthentication-Bibliothek](../../key-vault/service-to-service-authentication.md)

#### <a name="can-i-write-my-own-custom-searchers"></a>Kann ich meine eigenen benutzerdefinierten Suchroutinen schreiben?

Credential Scanner basiert auf einem Satz von Inhaltssuchroutinen, die üblicherweise in der Datei „buildsearchers.xml“ definiert sind. Die Datei enthält ein Array von serialisierten XML-Objekten, die ein **ContentSearcher**-Objekt darstellen. Das Programm wird mit einer Reihe von Suchroutinen verteilt, die eingehend getestet wurden. Sie können jedoch auch eigene benutzerdefinierte Suchroutinen implementieren.

Eine Inhaltssuchroutine wird wie folgt definiert:

- **Name**: Der beschreibende Name der Suchroutine, der in den Ausgabedateien von Credential Scanner verwendet werden soll. Es wird empfohlen, die Camel-Case-Namenskonvention für Namen von Suchroutinen zu verwenden.
- **RuleId**: Die stabile, opake ID der Suchroutine:
    - Standardsuchroutinen von Credential Scanner wird ein **RuleId**-Wert wie CSCAN0010, CSCAN0020 oder CSCAN0030 zugewiesen. Die letzte Ziffer ist für das Mergen oder Unterteilen von Suchroutinengruppen durch reguläre Ausdrücke (RegEx) reserviert.
    - Der **RuleId**-Wert für eine angepasste Suchroutine sollte über einen eigenen Namespace verfügen. Beispiele sind: CSCAN-\<Namespace\>0010, CSCAN-\<Namespace\>0020 und CSCAN-\<Namespace\>0030.
    - Der vollqualifizierte Name der Suchroutine besteht aus der Kombination von **RuleId**-Wert und Suchroutinennamen. Beispiele sind: CSCAN0010.KeyStoreFiles und CSCAN0020.Base64EncodedCertificate.
- **ResourceMatchPattern**: RegEx der Dateierweiterungen, die anhand der Suchroutine überprüft werden sollen.
- **ContentSearchPatterns**: Ein Array von Zeichenfolgen mit RegEx-Anweisungen, die abgeglichen werden sollen. Wenn keine Suchmuster definiert sind, werden alle Dateien zurückgegeben, die mit dem **ResourceMatchPattern**-Wert übereinstimmen.
- **ContentSearchFilters**: Ein Array von Zeichenfolgen, das RegEx-Anweisungen enthält, um suchroutinenspezifische falsch positive Ergebnisse zu filtern.
- **MatchDetails**: Eine beschreibende Nachricht und/oder Risikominderungsanweisungen, die für jede Übereinstimmung mit der Suchroutine hinzugefügt werden sollen.
- **Recommendation**: Stellt den Inhalt des Vorschlagsfelds für einen Abgleich im PREfast-Berichtsformat zur Verfügung.
- **Severity**: Ein Integerwert, der den Schweregrad eines Problems angibt. Der höchste Schweregrad hat den Wert 1.

  ![XML für Credential Scanner-Setup](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers"></a>Roslyn Analyzers

#### <a name="what-are-common-errors-when-using-the-roslyn-analyzers-task"></a>Welche Fehler treten am häufigsten bei der Verwendung des Roslyn Analyzers-Tasks auf?

##### <a name="the-project-was-restored-using-a-wrong-microsoftnetcoreapp-version"></a>Das Projekt wurde mit einer falschen Microsoft.NETCore.App-Version wiederhergestellt.

Vollständige Fehlermeldung:

„Fehler: The project was restored using Microsoft.NETCore.App version *x.x.x*, but with current settings, version *y.y.y* would be used instead. (Das Projekt wurde mit Version „x.x.x“ von Microsoft.NETCore.App wiederhergestellt, aber mit den aktuellen Einstellungen würde Version „y.y.y“ verwendet werden). Um dieses Problem zu beheben, stellen Sie sicher, dass die gleichen Einstellungen für die Wiederherstellung und für nachfolgende Vorgänge wie Build- oder Veröffentlichungsvorgänge verwendet werden. Normalerweise tritt dieses Problem auf, wenn die RuntimeIdentifier-Eigenschaft während des Builds oder der Veröffentlichung festgelegt wird, jedoch nicht während der Wiederherstellung.“

Roslyn Analyzers-Tasks werden als Teil der Kompilierung ausgeführt, sodass sich die Quellstruktur auf dem Buildcomputer in einem buildfähigen Zustand befinden muss.

Durch einen Schritt zwischen dem Hauptbuild und den Roslyn Analyzers-Schritten wurde die Quellstruktur u. U. in einen nicht buildfähigen Zustand versetzt. Dieser zusätzliche Schritt ist wahrscheinlich **dotnet.exe publish**. Versuchen Sie, den Schritt zu duplizieren, durch den kurz vor dem Roslyn Analyzers-Schritt eine NuGet-Wiederherstellung ausgeführt wird. In diesem duplizierten Schritt kann die Quellstruktur ggf. wieder in einen buildfähigen Zustand zurückversetzt werden.

##### <a name="cscexe-cant-create-an-analyzer-instance"></a>„csc. exe“ kann keine Analysetoolinstanz erstellen

Vollständige Fehlermeldung:

„'csc.exe' exited with error code 1 -- An instance of analyzer *AAAA* cannot be created from C:\\*BBBB*.dll: Could not load file or assembly 'Microsoft.CodeAnalysis, Version=*X.X.X.X*, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. („csc. exe“ wurde mit dem Fehlercode 1 beendet – Eine Instanz von Analyzer AAAA kann nicht aus „C:\BBBB.dll“ erstellt werden: Die Datei oder Assembly „Microsoft.CodeAnalysis, Version=X.X.X.X, Culture=neutral, PublicKeyToken=31bf3856ad364e35“ oder eine Abhängigkeit davon wurde nicht geladen. Das System kann die angegebene Datei nicht finden.“

Stellen Sie sicher, dass der Compiler Roslyn Analyzers unterstützt. Beim Ausführen des Befehls **csc.exe /version** sollte ein Wert mit der Version 2.6 oder höher zurückgegeben werden.

In einigen Fällen können einzelne CSPROJ-Dateien die Visual Studio-Installation des Buildcomputers überschreiben, indem sie auf ein Paket aus Microsoft.Net.Compilers verweisen. Um eine bestimmte Version des Compilers zu verwenden, entfernen Sie Verweise auf Microsoft.Net.Compilers. Stellen Sie andernfalls sicher, dass das referenzierte Paket die Version 2.6 oder höher hat.

Versuchen Sie, den in der Option **csc.exe /errorlog** angegebenen Fehlerprotokollpfad abzurufen. Die Option und der Pfad werden im Protokoll des Roslyn Analyzers-Buildtasks angezeigt. Der Pfad könnte in etwa so aussehen: **/errorlog:F:\ts-services-123\_work\456\s\Some\Project\Code\Code.csproj.sarif**

##### <a name="the-c-compiler-version-isnt-recent-enough"></a>Die C# Compilerversion ist nicht aktuell genug.

Die neuesten Versionen des C#-Compilers werden hier veröffentlicht: [Microsoft.Net.Compilers](https://www.nuget.org/packages/Microsoft.Net.Compilers). Um die installierte Version abzurufen, führen Sie **csc.exe /version** an der Eingabeaufforderung aus. Stellen Sie sicher, dass Sie auf ein NuGet-Paket von Microsoft.Net.Compilers verweisen, das mindestens die Version 2.6 aufweist.

##### <a name="msbuild-and-vsbuild-logs-arent-found"></a>MSBuild- und VSBuild-Protokolle nicht gefunden

Durch den Roslyn Analyzers-Buildtask müssen Azure DevOps für das MSBuild-Protokoll aus dem MSBuild-Buildtask abgefragt werden. Wenn der Analysetool-Task unmittelbar nach dem MSBuild-Task ausgeführt wird, ist das Protokoll noch nicht verfügbar. Platzieren Sie andere Tasks zwischen der MSBuild-Task und dem Roslyn Analyzers-Task. Beispiele für andere Tasks sind BinSkim und Schadsoftwarescanner.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie zusätzliche Unterstützung benötigen, ist Support für die Microsoft-Sicherheitscodeanalyse von Montag bis Freitag zwischen 9:00 Uhr und 17:00 Uhr (Pazifik-Standardzeit) verfügbar.

  - Onboarding: Wenden Sie sich an Ihren Technical Account Manager, um zu beginnen.
  
  - Support: Senden Sie eine E-Mail an unser Team unter [Support für die Microsoft-Sicherheitscodeanalyse](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request).

  >[!NOTE] 
  >Möglicherweise verfügen Sie nicht über eine kostenpflichtige Supportbeziehung mit Microsoft. Oder Sie nutzen ein Supportangebot, das Sie daran hindert, Dienste aus dem Phoenix-Katalog zu erwerben. Wenn eine dieser Bedingungen zutrifft, besuchen Sie unsere [Support Services-Startseite](https://www.microsoft.com/enterprise/services/support), um weitere Informationen zu erhalten.
