---
title: Quellcodeverwaltung in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Quellcodeverwaltung in Azure Data Factory konfigurieren
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: djpmsft
ms.author: daperlov
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: f3d443eed43cc1e131cd3dc47407ce2dfb023ce8
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326380"
---
# <a name="source-control-in-azure-data-factory"></a>Quellcodeverwaltung in Azure Data Factory

Die Benutzeroberfläche von Azure Data Factory bietet zwei Möglichkeiten zur visuellen Erstellung:

- Direktes Erstellen mit dem Data Factory-Dienst
- Erstellen mit Azure Repos Git oder GitHub-Integration

> [!NOTE]
> In der Azure Government-Cloud wird nur die direkte Erstellung mit dem Data Factory-Dienst unterstützt.

## <a name="author-directly-with-the-data-factory-service"></a>Direktes Erstellen mit dem Data Factory-Dienst

Beim direkten Erstellen mit dem Data Factory-Dienst können Änderungen nur über die Schaltfläche **Alle veröffentlichen** gespeichert werden. Nach Klick auf die Schaltfläche werden alle vorgenommenen Änderungen direkt im Data Factory-Dienst veröffentlicht. 

![Modus „Veröffentlichen“](media/author-visually/data-factory-publish.png)

Das direkte Erstellen mit dem Data Factory-Dienst weist die folgenden Einschränkungen auf:

- Der Data Factory-Dienst umfasst kein Repository zum Speichern der JSON-Entitäten für Ihre Änderungen.
- Der Data Factory-Dienst ist nicht für Zusammenarbeit oder Versionskontrolle optimiert.

> [!NOTE]
> Das direkte Erstellen mit dem Data Factory-Dienst wird auf der Azure Data Factory-Benutzeroberfläche deaktiviert, wenn ein Git-Repository konfiguriert ist. Änderungen am Dienst können direkt über PowerShell oder ein SDK vorgenommen werden.

## <a name="author-with-azure-repos-git-integration"></a>Erstellen mit der Azure Repos Git-Integration

Das visuelle Erstellen mit der Azure Repos Git-Integration unterstützt die Quellcodeverwaltung und Kollaboration beim Arbeiten an Data Factory-Pipelines. Sie können eine Data Factory einem Azure Repos Git-Organisationsrepository u.a. für die Quellcodeverwaltung, Kollaboration, Versionsverwaltung zuordnen. Eine einzelne Azure Repos Git-Organisation kann über mehrere Repositorys verfügen. Allerdings kann ein Azure Repos Git-Repository nur einer einzigen Data Factory zugeordnet werden. Wenn Sie noch nicht über eine Azure Repos-Organisation oder ein -Repository verfügen, folgen Sie [diesen Anweisungen](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) zum Erstellen der Ressourcen.

> [!NOTE]
> Sie können Skript- und Datendateien in einem Azure Repos Git-Repository speichern. Allerdings müssen Sie die Dateien manuell in Azure Storage hochladen. Eine Data Factory-Pipeline lädt Skript- oder Datendateien, die in einem Azure Repos Git-Repository gespeichert sind, nicht automatisch in Azure Storage hoch.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Konfigurieren eines Azure Repos Git-Repositorys mit Azure Data Factory

Es gibt zwei Methoden zum Konfigurieren eines Azure Repos Git-Repositorys mit einer Data Factory.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Konfigurationsmethode 1: Azure Data Factory-Startseite

Wählen Sie auf der Startseite von Azure Data Factory **Coderepository einrichten** aus.

![Konfigurieren eines Coderepositorys für Azure Repos](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Konfigurationsmethode 2: UX-Canvas für die Erstellung
Wählen Sie auf der Azure Data Factory-Benutzeroberfläche im Erstellungsbereich das Dropdownmenü **Data Factory** und dann **Coderepository einrichten** aus.

![Konfigurieren der Repositoryeinstellungen für Erstellung in der UX-Canvas](media/author-visually/configure-repo-2.png)

Bei beiden Methoden wird der Konfigurationsbereich für die Repositoryeinstellungen geöffnet.

![Konfigurieren der Repositoryeinstellungen](media/author-visually/repo-settings.png)

Im Konfigurationsbereich werden die folgenden Einstellungen für das Coderepository für Azure Repos angezeigt:

| Einstellung | BESCHREIBUNG | Wert |
|:--- |:--- |:--- |
| **Repositorytyp** | Der Typ des Coderepositorys für Azure Repos.<br/> | Azure DevOps Git oder GitHub |
| **Azure Active Directory** | Ihr Name des Azure AD-Mandanten. | `<your tenant name>` |
| **Azure Repos-Organisation** | Der Name Ihrer Azure Repos-Organisation. Sie können den Namen Ihrer Azure Repos-Organisation unter `https://{organization name}.visualstudio.com` finden. Sie können sich [bei Ihrer Azure Repos-Organisation anmelden](https://www.visualstudio.com/team-services/git/), um auf Ihr Visual Studio-Profil zuzugreifen und Ihre Repositorys und Projekte anzuzeigen. | `<your organization name>` |
| **Projektname** | Der Name Ihres Azure Repos-Projekts. Sie können den Namen Ihres Azure Repos-Projekts unter `https://{organization name}.visualstudio.com/{project name}` finden. | `<your Azure Repos project name>` |
| **Repositoryname** | Der Name Ihres Azure Repos-Coderepositorys. Azure Repos-Projekte enthalten Git-Repositorys zum Verwalten Ihres Quellcodes, wenn Ihr Projekt umfangreicher wird. Sie können ein neues Repository erstellen oder ein vorhandenes Repository verwenden, das sich bereits in Ihrem Projekt befindet. | `<your Azure Repos code repository name>` |
| **Collaboration branch** (Kollaborationsbranch) | Ihr Branch für die Azure Repos-Kollaboration, der für die Veröffentlichung verwendet wird. Die Standardeinstellung ist `master`. Ändern Sie diese Einstellung, falls Sie Ressourcen eines anderen Branchs veröffentlichen möchten. | `<your collaboration branch name>` |
| **Stammordner** | Ihr Stammordner im Branch für die Azure Repos-Kollaboration. | `<your root folder name>` |
| **Vorhandene Data Factory-Ressourcen in Repository importieren** | Gibt an, ob vorhandene Data Factory-Ressourcen aus dem **Dokumenterstellungsbereich** in ein Azure Repos Git-Repository importiert werden sollen. Aktivieren Sie das Kontrollkästchen, um Ihre Data Factory-Ressourcen in das zugehörige Git-Repository im JSON-Format zu importieren. Diese Aktion exportiert jede Ressource einzeln (d. h. die verknüpften Dienste und Datasets werden in separate JSONs exportiert). Ist dieses Kontrollkästchen nicht aktiviert, werden die vorhandenen Ressourcen nicht importiert. | Aktiviert (Standardeinstellung) |
| **Branch zum Importieren der Ressource** | Gibt an, in welchen Branch die Data Factory-Ressourcen (Pipelines, Datasets, verknüpfte Dienste usw.) importiert werden. Sie können Ressourcen in einen der folgenden Branches importieren: a. Kollaboration b. Neu erstellen c. Vorhandene verwenden |  |

> [!NOTE]
> Wenn Sie Microsoft Edge verwenden und in der Dropdownliste Ihres Azure DevOps-Kontos keine Werte sehen, fügen Sie https://*.visualstudio.com zur Liste der vertrauenswürdigen Sites hinzu.

### <a name="use-a-different-azure-active-directory-tenant"></a>Verwenden eines anderen Azure Active Directory-Mandanten

Sie können ein Azure Repos Git-Repository in einem anderen Azure Active Directory-Mandanten erstellen. Zum Angeben eines anderen Azure AD-Mandanten müssen Sie über Administratorberechtigungen für das verwendete Azure-Abonnement verfügen.

### <a name="use-your-personal-microsoft-account"></a>Verwenden Ihres persönlichen Microsoft-Kontos

Um ein persönliches Microsoft-Konto für die Git-Integration zu verwenden, können Sie Ihr persönliches Azure-Repository mit dem Active Directory Ihrer Organisation verknüpfen.

1. Fügen Sie Ihr persönliches Microsoft-Konto als Gast zum Active Directory Ihrer Organisation hinzu. Weitere Informationen finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](../active-directory/b2b/add-users-administrator.md).

2. Melden Sie sich mit Ihrem persönlichen Microsoft-Konto beim Azure-Portal an. Wechseln Sie dann zum Active Directory Ihrer Organisation.

3. Wechseln Sie zum Abschnitt für Azure DevOps – dort wird jetzt Ihr persönliches Repository angezeigt. Wählen Sie das Repository aus, und verbinden Sie es mit Active Directory.

Nach diesen Konfigurationsschritten ist Ihr persönliches Repository verfügbar, wenn Sie die Git-Integration auf der Data Factory-Benutzeroberfläche einrichten.

Weitere Informationen zum Verbinden von Azure Repos mit dem Active Directory Ihrer Organisation finden Sie unter [Verbinden Ihrer Azure DevOps-Organisation mit Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad).

## <a name="author-with-github-integration"></a>Erstellen der GitHub-Integration

Das visuelle Erstellen mit GitHub-Integration unterstützt Quellcodeverwaltung und Kollaboration beim Arbeiten an den Data Factory-Pipelines. Sie können eine Data Factory einem GitHub-Kontorepository für die Quellcodeverwaltung, Kollaboration und Versionsverwaltung zuordnen. Ein einzelnes GitHub-Konto kann über mehrere Repositorys verfügen. Allerdings kann ein GitHub-Repository nur einer einzigen Data Factory zugeordnet werden. Wenn Sie noch nicht über ein GitHub-Konto oder -Repository verfügen, folgen Sie  [diesen Anweisungen](https://github.com/join)  zum Erstellen der Ressourcen.

Die GitHub-Integration von Data Factory unterstützt sowohl das öffentliche GitHub (also [https://github.com](https://github.com)) als auch GitHub Enterprise. Sie können öffentliche und private GitHub-Repositorys mit Data Factory verwenden, sofern Sie über Lese- und Schreibberechtigungen für das Repository in GitHub verfügen.

Zum Konfigurieren eines GitHub-Repositorys müssen Sie über Administratorberechtigungen für das verwendete Azure-Abonnement verfügen.

Das folgende Video enthält eine neun-minütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="configure-a-github-repository-with-azure-data-factory"></a>Konfigurieren eines GitHub-Repositorys mit Azure Data Factory

Es gibt zwei Methoden zum Konfigurieren eines GitHub-Repositorys mit einer Data Factory.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Konfigurationsmethode 1: Azure Data Factory-Startseite

Wählen Sie auf der Startseite von Azure Data Factory **Coderepository einrichten** aus.

![Konfigurieren eines Coderepositorys für Azure Repos](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Konfigurationsmethode 2: UX-Canvas für die Erstellung

Wählen Sie auf der Azure Data Factory-Benutzeroberfläche im Erstellungsbereich das Dropdownmenü **Data Factory** und dann **Coderepository einrichten** aus.

![Konfigurieren der Repositoryeinstellungen für Erstellung in der UX-Canvas](media/author-visually/configure-repo-2.png)

Bei beiden Methoden wird der Konfigurationsbereich für die Repositoryeinstellungen geöffnet.

![GitHub-Repositoryeinstellungen](media/author-visually/github-integration-image2.png)

Im Konfigurationsbereich werden die folgenden Einstellungen für das GitHub-Repository angezeigt:

| **Einstellung** | **Beschreibung**  | **Wert**  |
|:--- |:--- |:--- |
| **Repositorytyp** | Der Typ des Coderepositorys für Azure Repos. | GitHub |
| **GitHub Enterprise verwenden** | Kontrollkästchen zum Auswählen von GitHub Enterprise | nicht ausgewählt (Standard) |
| **GitHub Enterprise-URL** | Die Stamm-URL von GitHub Enterprise. Beispiel: https://github.mydomain.com. Nur erforderlich, wenn **GitHub Enterprise verwenden** ausgewählt ist | `<your GitHub enterprise url>` |                                                           
| **GitHub-Konto** | Ihr GitHub-Kontoname. Sie finden diesen Namen unter https:\//github.com/{Kontoname}/{Repositoryname}. Beim Navigieren zu dieser Seite werden Sie zur Eingabe der GitHub-OAuth-Anmeldeinformationen zu Ihrem GitHub-Konto aufgefordert. | `<your GitHub account name>` |
| **Repositoryname**  | Der Name Ihres GitHub-Coderepositorys. GitHub-Konten enthalten Git-Repositorys zum Verwalten Ihres Quellcodes. Sie können ein neues Repository erstellen oder ein vorhandenes Repository verwenden, das sich bereits in Ihrem Konto befindet. | `<your repository name>` |
| **Collaboration branch** (Kollaborationsbranch) | Ihr Branch für die GitHub-Kollaboration, der für die Veröffentlichung verwendet wird. Die Standardeinstellung ist „master“. Ändern Sie diese Einstellung, falls Sie Ressourcen eines anderen Branchs veröffentlichen möchten. | `<your collaboration branch>` |
| **Stammordner** | Ihr Stammordner im Branch für die GitHub-Kollaboration. |`<your root folder name>` |
| **Vorhandene Data Factory-Ressourcen in Repository importieren** | Gibt an, ob vorhandene Data Factory-Ressourcen aus dem Erstellungsbereich auf der Benutzeroberfläche in ein GitHub-Repository importiert werden sollen. Aktivieren Sie das Kontrollkästchen, um Ihre Data Factory-Ressourcen in das zugehörige Git-Repository im JSON-Format zu importieren. Diese Aktion exportiert jede Ressource einzeln (d. h. die verknüpften Dienste und Datasets werden in separate JSONs exportiert). Ist dieses Kontrollkästchen nicht aktiviert, werden die vorhandenen Ressourcen nicht importiert. | Aktiviert (Standardeinstellung) |
| **Branch zum Importieren der Ressource** | Gibt an, in welchen Branch die Data Factory-Ressourcen (Pipelines, Datasets, verknüpfte Dienste usw.) importiert werden. Sie können Ressourcen in einen der folgenden Branches importieren: a. Kollaboration b. Neu erstellen c. Vorhandene verwenden |  |

### <a name="known-github-limitations"></a>Bekannte GitHub-Einschränkungen

- Sie können Skript- und Datendateien in einem GitHub-Repository speichern. Allerdings müssen Sie die Dateien manuell in Azure Storage hochladen. Eine Data Factory-Pipeline lädt Skript- oder Datendateien, die in einem GitHub-Repository gespeichert sind, nicht automatisch in Azure Storage hoch.

- GitHub Enterprise-Versionen vor 2.14.0 funktionieren nicht im Browser Microsoft Edge.

- Die GitHub-Integration in die visuellen Data Factory-Erstellungstools funktioniert nur in der allgemein verfügbaren Version von Data Factory.

## <a name="switch-to-a-different-git-repo"></a>Wechseln zu einem anderen Git-Repository

Um zu einem anderen Git-Repository zu wechseln, klicken Sie auf der Data Factory-Übersichtseite rechts oben auf das Symbol **Git-Repositoryeinstellungen**. Wenn das Symbol nicht angezeigt wird, löschen Sie den lokalen Browsercache. Wählen Sie das Symbol aus, um die Zuordnung zum aktuellen Repository zu entfernen.

![Git-Symbol](media/author-visually/remove-repo.png)

Klicken Sie im eingeblendeten Bereich „Repositoryeinstellungen“ auf **Git entfernen**. Geben Sie Ihren Data Factory Namen ein, und klicken Sie auf **Bestätigen**, um das Git-Repository zu entfernen, das Ihrer Data Factory-Instanz zugeordnet ist.

![Entfernen der Zuordnung zum aktuellen Git-Repository](media/author-visually/remove-repo2.png)

Nachdem Sie die Zuordnung zum aktuellen Repository entfernt haben, können Sie Ihre Git-Einstellungen zur Verwendung eines anderen Repositorys konfigurieren und dann vorhandene Data Factory-Ressourcen in das neue Repository importieren. 

## <a name="version-control"></a>Versionskontrolle

Versionskontrollsysteme (auch als _Quellcodeverwaltung_ bezeichnet) ermöglichen Entwicklern die Zusammenarbeit an Code und das Nachverfolgen von an der Codebasis vorgenommenen Änderungen. Quellcodeverwaltung ist ein unverzichtbares Tool für Projekte, an denen mehrere Entwickler arbeiten.

### <a name="creating-feature-branches"></a>Erstellen von Featurebranches

Jedes Azure Repos Git-Repository, das einer Data Factory zugeordnet ist, verfügt über einen Branch für die Kollaboration. (`master` ist der Standardbranch für die Kollaboration.) Benutzer können auch Featurebranches erstellen, indem sie im Dropdownmenü „Branch“ auf **+ Neuer Branch** klicken. Sobald der Bereich „Neuer Branch“ angezeigt wird, geben Sie den Namen Ihres Featurebranches ein.

![Neuen Branch erstellen](media/author-visually/new-branch.png)

Wenn Sie bereit sind, die Änderungen in Ihrem Featurebranch mit Ihrem Kollaborationsbranch zu mergen, klicken Sie auf das Dropdownmenü „Branch“ und dann auf **Pull Request erstellen**. Dann gelangen Sie zu Azure Repos Git und können Pull Requests auslösen, Codereviews durchführen und Änderungen an Ihrem Kollaborationsbranch zusammenführen. (`master` ist die Standardeinstellung.) Sie können nur Veröffentlichungen für den Data Factory-Dienst Ihres Kollaborationsbranchs durchführen. 

![Erstellen eines neuen Pull Requests](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Konfigurieren von Veröffentlichungseinstellungen

Fügen Sie dem Stammordner im Kollaborationsbranch eine `publish_config.json`-Datei hinzu, um den Branch für die Veröffentlichung zu konfigurieren (der Branch, in dem Resource Manager-Vorlage gespeichert werden). Die Data Factory liest die Datei, sucht das Feld `publishBranch` und erstellt wenn nötig einen neuen Branch, mit dem angegebenen Wert. Anschließend speichert sie die Resource Manager-Vorlagen am angegebenen Ort. Beispiel:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Wenn Sie einen neuen Branch für die Veröffentlichung angeben, löscht die Data Factory den vorherigen Branch für die Veröffentlichung nicht. Wenn Sie den vorherigen Branch für die Veröffentlichung über eine Remoteverbindung ausführen möchten, löschen Sie ihn manuell.

> [!NOTE]
> Die Data Factory liest die `publish_config.json`-Datei nur, wenn diese die Factory lädt. Wenn die Factory bereits im Portal geladen ist, aktualisieren Sie den Browser, damit Ihre Änderungen übernommen werden.

### <a name="publish-code-changes"></a>Veröffentlichen von Codeänderungen

Nachdem Sie die Änderungen mit dem Kollaborationsbranch gemergt haben (`master` ist die Standardeinstellung), klicken Sie auf **Veröffentlichen**, um Ihre Codeänderungen manuell im Masterbranch im Data Factory-Dienst zu veröffentlichen.

![Veröffentlichen von Änderungen für den Data Factory-Dienst](media/author-visually/publish-changes.png)

Ein Seitenbereich wird geöffnet, in dem Sie bestätigen, dass der Branch für die Veröffentlichung und ausstehende Änderungen korrekt sind. Nachdem Sie Ihre Änderungen überprüft haben, klicken Sie auf **OK**, um die Veröffentlichung zu bestätigen.

![Überprüfen des Branch für die Veröffentlichung](media/author-visually/configure-publish-branch.png)

> [!IMPORTANT]
> Der Masterbranch ist nicht repräsentativ für das, was im Data Factory-Dienst bereitgestellt wird. Der Masterbranch *muss* manuell im Data Factory-Dienst veröffentlicht werden.

## <a name="advantages-of-git-integration"></a>Vorteile der Git-Integration

-   **Quellcodeverwaltung.** Wenn Ihre Data Factory-Workloads sehr wichtig sind, sollten Sie Ihre Factory in Git integrieren, um verschiedene Vorteile der Quellcodeverwaltung wie die folgenden nutzen zu können:
    -   Möglichkeit zum Nachverfolgen/Überwachen von Änderungen
    -   Möglichkeit zum Rückgängigmachen von Änderungen, die Fehler verursacht haben
-   **Partielle Speicherungen.** Wenn Sie viele Änderungen an Ihrer Factory vornehmen, werden Sie feststellen, dass Sie im regulären LIVE-Modus Ihre Änderungen nicht als Entwurf speichern können, da Sie nicht bereit sind oder Änderungen nicht verlieren möchten, falls der Computer abstürzt. Mit der Git-Integration können Sie Ihre Änderungen weiterhin inkrementell speichern und erst dann in der Factory veröffentlichen, wenn Sie dazu bereit sind. Git fungiert als Stagingstelle für Ihre Arbeit, bis Sie Ihre Änderungen zu Ihrer Zufriedenheit getestet haben.
-   **Kollaboration und Steuerung.** Wenn mehrere Ihrer Teammitglieder an derselben Factory arbeiten, empfiehlt es sich, Ihre Teamkollegen über einen Code Review-Prozess miteinander zusammenarbeiten zu lassen. Sie können Ihre Factory auch so einrichten, dass nicht jeder Mitwirkende an der Factory Berechtigungen zum Bereitstellen in der Factory hat. Teammitglieder sind möglicherweise nur berechtigt, Änderungen über Git vorzunehmen, und nur bestimmte Personen im Team dürfen die Änderungen an der Factory „veröffentlichen“.
-   **Anzeigen von Unterschieden als Diff.** Im Git-Modus können Sie ein übersichtliches Diff der Nutzlast anzeigen, die in der Factory veröffentlicht werden soll. In diesem Diff sehen Sie alle Ressourcen/Entitäten, die seit der letzten Veröffentlichung in der Factory geändert/hinzugefügt/gelöscht wurden. Basierend auf diesem Diff können Sie entweder mit der Veröffentlichung fortfahren oder zurückkehren, die Änderungen überprüfen und den Vorgang später wiederaufnehmen.
-   **Bessere CI/CD.** Wenn Sie den Git-Modus verwenden, können Sie Ihre Releasepipeline so konfigurieren, dass sie automatisch ausgelöst wird, sobald Änderungen an der Entwicklungsfactory vorgenommen wurden. Sie können auch die Eigenschaften in Ihrer Factory anpassen, die als Parameter in der Resource Manager-Vorlage verfügbar sind. Es kann hilfreich sein, nur die erforderlichen Eigenschaften als Parameter beizubehalten und alles andere fest zu codieren.
-   **Höhere Leistung.** Eine durchschnittliche Factory wird im Git-Modus 10-mal schneller geladen als im normalen LIVE-Modus, da die Ressourcen über Git heruntergeladen werden.

## <a name="best-practices-for-git-integration"></a>Bewährte Methoden für die Git-Integration

### <a name="permissions"></a>Berechtigungen

Normalerweise möchten Sie nicht, dass jedes Teammitglied die Berechtigung hat, die Factory zu aktualisieren. Die folgenden Berechtigungseinstellungen werden empfohlen:

*   Alle Teammitglieder sollten über Leseberechtigungen für die Data Factory verfügen.
*   Nur eine ausgewählte Gruppe von Personen sollte in der Factory veröffentlichen dürfen. Zu diesem Zweck müssen Sie über die Rolle **Data Factory-Mitwirkender** für die Factory verfügen. Weitere Informationen zu Berechtigungen finden Sie unter [Rollen und Berechtigungen für Azure Data Factory](concepts-roles-permissions.md).
   
Es wird empfohlen, keine direkten Eincheckvorgänge im Kollaborationsbranch zuzulassen. Diese Einschränkung kann helfen, Fehler zu vermeiden, da jeder Eincheckvorgang einen Pull Request-Prozess durchläuft.

### <a name="using-passwords-from-azure-key-vault"></a>Verwenden von Kennwörtern aus Azure Key Vault

Es wird empfohlen, Azure Key Vault zum Speichern von Verbindungszeichenfolgen oder Kennwörtern für verknüpfte Data Factory-Dienste zu verwenden. Aus Sicherheitsgründen speichern wir diese geheimen Informationen nicht in Git, sodass Änderungen an verknüpften Diensten sofort im Azure Data Factory-Dienst veröffentlicht werden.

Durch die Verwendung von Key Vault werden Continuous Integration und Continuous Deployment vereinfacht, da Sie diese Geheimnisse nicht während der Bereitstellung von Ressourcen-Manager-Vorlagen angeben müssen.

## <a name="troubleshooting-git-integration"></a>Problembehandlung bei der Git-Integration

### <a name="stale-publish-branch"></a>Veralteter Branch für die Veröffentlichung

Wenn der Branch für die Veröffentlichung nicht mit dem Masterbranch synchron ist und trotz einer aktuellen Veröffentlichung veraltete Ressourcen enthält, versuchen Sie die folgenden Schritte:

1. Entfernen des aktuellen Git-Repositorys
1. Konfigurieren Sie Git mit denselben Einstellungen neu. Stellen Sie jedoch sicher, dass **Vorhandene Data Factory Ressourcen in Repository importieren** ausgewählt ist, und wählen Sie **Neuer Branch** aus.
1. Löschen aller Ressourcen aus dem Kollaborationsbranch
1. Erstellen eines Pull Requests zum Mergen der Änderungen in den Kollaborationsbranch 

## <a name="provide-feedback"></a>Feedback geben
Wählen Sie **Feedback** aus, um Kommentare zu Funktionen abzugeben oder um Microsoft Probleme mit dem Tool zu melden:

![Feedback](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Nächste Schritte

* Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
* Informationen zum Implementieren von Continuous Integration und Deployment finden Sie unter [Continuous Integration und Delivery (CI/CD) in Azure Data Factory](continuous-integration-deployment.md).
