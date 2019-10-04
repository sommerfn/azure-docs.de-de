---
title: Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services | Microsoft-Dokumentation
description: Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: 3105296b3c670d3d44789c93878fa1fc6076973b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566705"
---
# <a name="enable-security-audits-for-azure-ad-domain-services-preview"></a>Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services (Vorschauversion)
Die Sicherheitsüberwachung von Azure Active Directory Domain Services ermöglicht Kunden die Verwendung des Azure AD Domain Services-Portals, um Sicherheitsüberwachungsereignisse an bestimmte Ressourcen zu streamen. Diese Ereignisse können unter anderem von Azure Storage, Azure Log Analytics-Arbeitsbereichen und von Azure Event Hub empfangen werden. Kurz nach der Aktivierung von Sicherheitsüberwachungsereignissen sendet Azure AD Domain Services alle überwachten Ereignisse für die ausgewählte Kategorie an die Zielressource. Mithilfe von Sicherheitsüberwachungsereignissen können Kunden überwachte Ereignisse in Azure Storage archivieren. Darüber hinaus können Kunden Ereignisse unter Verwendung von Event Hubs an SIEM-Software (Security Information & Event Management) oder an eine vergleichbare Lösung streamen oder über das Azure-Portal eigene Analysen und Untersuchungen mithilfe von Azure Log Analytics durchführen. 

> [!IMPORTANT]
> Die Sicherheitsüberwachung von Azure AD Domain Services steht nur in Azure Resource Manager-basierten Instanzen für Azure AD Domain Services zur Verfügung.
>
>

## <a name="auditing-event-categories"></a>Kategorien für Überwachungsereignisse
Die Sicherheitsüberwachung von Azure AD Domain Services ist auf die herkömmliche Überwachung von Active Directory Domain Services-Domänencontrollern abgestimmt. Dank der Wiederverwendung bereits vorhandener Überwachungsmuster kann bei der Ereignisanalyse die gleiche Logik verwendet werden. Für die Sicherheitsüberwachung von Azure AD Domain Services stehen folgende Ereigniskategorien zur Verfügung:

| Name der Überwachungskategorie | Beschreibung |
|:---|:---|
| Kontoanmeldung|Dient zur Überwachung von Versuchen, Kontodaten auf einem Domänencontroller oder in einer lokalen Sicherheitskontenverwaltung (Security Accounts Manager, SAM) zu authentifizieren.</p>Versuche, auf einen bestimmten Computer zuzugreifen, werden anhand von An- und Abmelderichtlinieneinstellungen und entsprechenden Ereignissen nachverfolgt. Bei den Einstellungen und Ereignissen in dieser Kategorie steht die verwendete Kontodatenbank im Mittelpunkt. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Überprüfung der Anmeldeinformationen überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Kerberos-Authentifizierungsdienst überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Ticketvorgänge des Kerberos-Diensts überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Andere Anmelde-/Abmeldeereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Kontoverwaltung|Dient zur Überwachung von Änderungen an Benutzer- und Computerkonten/-gruppen. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Anwendungsgruppenverwaltung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Computerkontoverwaltung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Verteilergruppenverwaltung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Andere Kontoverwaltungsereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Sicherheitsgruppenverwaltung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Benutzerkontenverwaltung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| Detaillierte Nachverfolgung|Dient zur Überwachung der Aktivitäten einzelner Anwendungen und Benutzer auf dem Computer, um die Nutzung eines Computers nachzuvollziehen. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[DPAPI-Aktivität überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[PNP-Aktivität überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[Prozesserstellung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[Prozessbeendung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[RPC-Ereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Directory Services-Zugriff|Dient zur Überwachung von Zugriffs- und Änderungsversuchen für Objekte in Active Directory Domain Services (AD DS). Diese Überwachungsereignisse werden nur auf Domänencontrollern protokolliert. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Detaillierte Verzeichnisdienstreplikation überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Verzeichnisdienstzugriff überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Verzeichnisdienständerungen überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Verzeichnisdienstreplikation überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Anmelden/Abmelden|Dient zur Überwachung von Anmeldeversuchen bei einem Computer (interaktiv oder über ein Netzwerk). Diese Ereignisse ermöglichen die Nachverfolgung von Benutzeraktivitäten sowie die Erkennung potenzieller Angriffe auf Netzwerkressourcen. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Kontosperrung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Benutzer-/Geräteansprüche überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[IPsec-Erweiterungsmodus überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Mitgliedschaft in der Überwachungsgruppe](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[IPsec-Hauptmodus überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[IPsec-Schnellmodus überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Abmelden überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Anmelden überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Netzwerkrichtlinienserver überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Andere Anmelde-/Abmeldeereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Spezielle Anmeldung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Objektzugriff| Dient zur Überwachung von Zugriffsversuchen auf bestimmte Objekte oder Objekttypen in einem Netzwerk oder auf einem Computer. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Anwendung generiert überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Zertifizierungsdienste überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Detaillierte Dateifreigabe überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Dateifreigabe überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Dateisystem überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Filterplattformverbindung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Filterplattform: Verworfene Pakete überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Handleänderung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Kernelobjekt überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Andere Objektzugriffsereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Registrierung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Wechselmedien überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[SAM überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Staging zentraler Zugriffsrichtlinien überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|Richtlinienänderung|Dient zur Überwachung von Änderungen an wichtigen Sicherheitsrichtlinien auf einem lokalen System oder in einem lokalen Netzwerk. Richtlinien werden in der Regel von Administratoren eingerichtet, um Netzwerkressourcen zu schützen. Die Überwachung von Änderungen an diesen Richtlinien bzw. von entsprechenden Änderungsversuchen kann ein wichtiger Aspekt der Sicherheitsverwaltung für ein Netzwerk sein. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Richtlinienänderungen überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Authentifizierungsrichtlinienänderung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Autorisierungsrichtlinienänderung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Filterplattform-Richtlinienänderung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[MPSSVC-Richtlinienänderung auf Regelebene überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Andere Richtlinienänderungsereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Berechtigungen| Dient zur Überwachung der Verwendung bestimmter Berechtigungen auf einem oder mehreren Systemen. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[Nicht sensible Verwendung von Rechten überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Sensible Verwendung von Rechten überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Andere Rechteverwendungsereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|System| Dient zur Überwachung von Änderungen auf der Systemebene eines Computers, die nicht durch die anderen Kategorien abgedeckt sind und potenzielle Auswirkungen auf die Sicherheit haben. Diese Kategorie umfasst folgende Unterkategorien:<ul><li>[IPsec-Treiber überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Andere Systemereignisse überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Sicherheitsstatusänderung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Sicherheitssystemerweiterung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Systemintegrität überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Ereignis-IDs nach Kategorie
 Die Sicherheitsüberwachung von Azure AD Domain Services erfasst die folgenden Ereignis-IDs, wenn die spezifische Aktion ein überwachbares Ereignis auslöst:

| Name der Ereigniskategorie | Ereignis-IDs |
|:---|:---|
|Kontoanmeldung|4767, 4774, 4775, 4776, 4777|
|Kontoverwaltung|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Detaillierte Nachverfolgung|Keine|
|DS-Zugriff|5136, 5137, 5138, 5139, 5141|
|Anmelden/Abmelden|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Objektzugriff|Keine|
|Richtlinienänderung|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Berechtigungen|4985|
|System|4612, 4621|

## <a name="enable-security-audit-events"></a>Aktivieren von Sicherheitsüberwachungsereignissen
In diesem Abschnitt erfahren Sie, wie Sie erfolgreich Sicherheitsüberwachungsereignisse von Azure AD Domain Services abonnieren.

> [!IMPORTANT]
> Die Sicherheitsüberwachungen von Azure AD Domain Services sind nicht rückwirkend. Sie können also keine Ereignisse aus der Vergangenheit abrufen oder wiedergeben. Der Dienst kann nur Ereignisse senden, die nach seiner Aktivierung auftreten.
>

### <a name="choose-the-target-resource"></a>Auswählen der Zielressource
Sie können eine beliebige Kombination aus Azure Storage, Azure Event Hubs oder Azure Log Analytics-Arbeitsbereichen als Zielressource für Ihre Sicherheitsüberwachungen verwenden. Orientieren Sie sich an der folgenden Tabelle, um die am besten geeignete Ressource für Ihren Anwendungsfall zu finden.

> [!IMPORTANT]
> Die Zielressource muss vor der Aktivierung der Sicherheitsüberwachungen von Azure AD Domain Services erstellt werden.
>

| Zielressource | Szenario |
|:---|:---|
|Azure Storage|Dieses Ziel kann verwendet werden, wenn Sie in erster Linie Sicherheitsüberwachungsereignisse zu Archivierungszwecken speichern möchten. Andere Ziele können zwar ebenfalls zu Archivierungszwecken verwendet werden, diese Ziele bieten jedoch Funktionen, die über die reine Archivierung hinausgehen. Eine Anleitung zum Erstellen eines Azure Storage-Kontos finden Sie unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal#create-a-storage-account-1).|
|Azure Event Hubs|Dieses Ziel kann verwendet werden, wenn Sie in erster Linie Sicherheitsüberwachungsereignisse an Zusatzsoftware weitergeben möchten – beispielsweise an eine Datenanalysesoftware oder an eine SIEM-Software (Security Information & Event Management). Eine Anleitung zum Erstellen eines Event Hubs finden Sie unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).|
|Azure Log Analytics-Arbeitsbereich|Dieses Ziel kann verwendet werden, wenn Sie in erster Linie Sicherheitsüberwachungen direkt über das Azure-Portal analysieren und überprüfen möchten.  Eine Anleitung zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).|

## <a name="using-the-azure-portal-to-enable-security-audit-events"></a>Aktivieren von Sicherheitsüberwachungsereignissen über das Azure-Portal 
1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.  Klicken Sie im Azure-Portal auf „Alle Dienste“. Geben Sie in der Liste mit den Ressourcen die Zeichenfolge **Domain** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Azure AD Domain Services**.
2. Klicken Sie in der Liste auf die Azure AD Domain Services-Instanz.
3. Klicken Sie auf der linken Seite in der Liste mit Aktionen auf **Diagnoseeinstellungen (Vorschauversion)** .</p>
![Aktion „Diagnoseeinstellung“](./media/security-audit-events/diagnostic-settings-action.png)
4. Geben Sie den Namen der Diagnosekonfiguration ein (beispielsweise **aadds-auditing**).</p>
![Seite „Diagnoseeinstellungen“](./media/security-audit-events/diagnostic-settings-page.png)
5. Aktivieren Sie das Kontrollkästchen neben den gewünschten Ressourcen, die Sie mit Sicherheitsüberwachungsereignissen verwenden möchten.
    > [!NOTE]
    > Auf dieser Seite können keine Zielressourcen erstellt werden.
    >
    
    **Azure Storage:**</p>
    Aktivieren Sie das Kontrollkästchen **In einem Speicherkonto archivieren**. Klicken Sie auf **Konfigurieren**. Wählen Sie das **Abonnement** und das **Speicherkonto** aus, die Sie zum Archivieren von Sicherheitsüberwachungsereignissen verwenden möchten. Klicken Sie auf **OK**.</p>
    
    ![Diagnosespeichereinstellungen](./media/security-audit-events/diag-settings-storage.png)
    
    **Azure Event Hubs:**</p>
    Aktivieren Sie das Kontrollkästchen **An einen Event Hub streamen**. Klicken Sie auf **Konfigurieren**. Wählen Sie auf der Seite **Event Hub auswählen** das **Abonnement** aus, das verwendet wurde, um den Event Hub zu erstellen. Wählen Sie als Nächstes Werte für **Event Hub-Namespace**, **Event Hub-Name** und **Event Hub-Richtlinienname** aus. Klicken Sie auf **OK**.</p>
    ![Diagnoseeinstellungen für Event Hub](./media/security-audit-events/diag-settings-eventhub.png)
    
    **Azure Log Analytics-Arbeitsbereiche:**</p>
    Wählen Sie **An Log Analytics senden** aus. Wählen Sie das **Abonnement** und den **Log Analytics-Arbeitsbereich** zum Speichern von Sicherheitsüberwachungsereignissen aus.</p>
    ![Diagnoseeinstellungen für Arbeitsbereich](./media/security-audit-events/diag-settings-log-analytics.png)

6. Wählen Sie die Protokollkategorien aus, die für die betreffende Zielressource einbezogen werden sollen. Bei Verwendung von Speicherkonten können Sie Aufbewahrungsrichtlinien konfigurieren.

    > [!NOTE]
    > Innerhalb einer einzelnen Konfiguration können für jede Zielressource verschiedene Protokollkategorien ausgewählt werden. Dadurch können Sie wählen, welche Protokollkategorien für Log Analytics gespeichert werden sollen und welche Protokollkategorien Sie archivieren möchten.
    >

7. Klicken Sie auf **Speichern**, um die vorgenommenen Änderungen zu speichern. Nach dem Speichern der Konfiguration dauert es kurz, bis die Zielressourcen Sicherheitsüberwachungsereignisse von Azure AD Domain Services erhalten.

## <a name="using-azure-powershell-to-enable-security-audit-events"></a>Aktivieren von Sicherheitsüberwachungsereignissen mithilfe von Azure PowerShell
 
### <a name="prerequisites"></a>Voraussetzungen

Folgen Sie den Anweisungen im Artikel zum [Installieren des Azure PowerShell-Moduls und Herstellen einer Verbindung mit Ihrem Azure-Abonnement](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="enable-security-audits"></a>Aktivieren von Sicherheitsüberwachungen

1. Authentifizieren Sie sich mithilfe des Azure PowerShell-Cmdlets **Connect-AzAccount** bei Azure Resource Manager für den entsprechenden Mandanten und das entsprechende Abonnement.
2. Erstellen Sie die Zielressource für die Sicherheitsüberwachungsereignisse.</p>
    **Azure Storage:**</p>
    Gehen Sie wie unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-powershell) beschrieben vor, um Ihr Speicherkonto zu erstellen.</p>
    **Azure Event Hubs:**</p>
    Gehen Sie wie unter [Schnellstart: Erstellen einer Event Hub-Instanz mit Azure PowerShell](https://docs.microsoft.com/azure/event-hubs/event-hubs-quickstart-powershell) beschrieben vor, um Ihren Event Hub zu erstellen. Gegebenenfalls müssen Sie auch mithilfe des Azure PowerShell-Cmdlets [New-AzEventHubAuthorizationRule](https://docs.microsoft.com/powershell/module/az.eventhub/new-azeventhubauthorizationrule?view=azps-2.3.2) eine Autorisierungsregel erstellen, um Active Directory AD Domain Services-Berechtigungen für den **Namespace** des Event Hubs zu erteilen. Die Autorisierungsregel muss die Rechte **Verwalten**, **Lauschen** und **Senden** enthalten.
    > [!IMPORTANT]
    > Wichtig: Die Autorisierungsregel muss für den Event Hub-Namespace (nicht für den Event Hub) festgelegt werden.
       
    </p>
    
    **Azure Log Analytics-Arbeitsbereiche:**</p>
    Gehen Sie wie unter [Erstellen eines Log Analytics-Arbeitsbereichs mit Azure PowerShell](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace-posh) beschrieben vor, um Ihren Arbeitsbereich zu erstellen.
3. Rufen Sie die Ressourcen-ID für Ihre Azure AD Domain Services-Instanz ab. Geben Sie in einer geöffneten, authentifizierten Windows PowerShell-Konsole den folgenden Befehl ein. Verwenden Sie bei zukünftigen Cmdlets die Variable **$aadds.ResourceId** als Parameter für die Azure AD Domain Services-Ressourcen-ID.
    ```powershell
    $aadds = Get-AzResource -name aaddsDomainName
    ``` 
4. Konfigurieren Sie mithilfe des Cmdlets **Set-AzDiagnosticSetting** die Azure-Diagnoseeinstellungen, um die Zielressource für Sicherheitsüberwachungsereignisse von Azure AD Domain Services zu verwenden. In den folgenden Beispielen stellt die Variable „$aadds.ResourceId“ die Ressourcen-ID Ihrer Azure AD Domain Services-Instanz dar (siehe Schritt 3).</p>
    **Azure Storage:**
    ```powershell
    Set-AzDiagnosticSetting `
    -ResourceId $aadds.ResourceId` 
    -StorageAccountId storageAccountId `
    -Enabled $true
    ```
    Ersetzen Sie *storageAccountId* durch die ID Ihres Speicherkontos.</p>
    
    **Azure Event Hubs:**
    ```powershell
    Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -EventHubName eventHubName `
    -EventHubAuthorizationRuleId eventHubRuleId `
    -Enabled $true
    ```
    Ersetzen Sie *eventHubName* durch den Namen Ihres Event Hubs. Ersetzen Sie *eventHubRuleId* durch Ihre zuvor erstellte Autorisierungsregel-ID.</p>
    
    **Azure Log Analytics-Arbeitsbereiche:**
    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -WorkspaceID workspaceId `
    -Enabled $true
    ```
    Ersetzen Sie *workspaceId* durch die ID des zuvor erstellten Log Analytics-Arbeitsbereichs. 

## <a name="view-security-audit-events-using-azure-monitor"></a>Anzeigen von Sicherheitsüberwachungsereignissen unter Verwendung von Azure Monitor
Log Analytics-Arbeitsbereiche ermöglichen das Anzeigen und Analysieren der Sicherheitsüberwachungsereignisse unter Verwendung von Azure Monitor und der Kusto-Abfragesprache. Die Abfragesprache ist für die schreibgeschützte Verwendung konzipiert und bietet leistungsstarke Analysefunktionen sowie eine übersichtliche Syntax.
Im Anschluss finden Sie einige Ressourcen, die Sie bei Ihren ersten Schritten mit der Kusto-Abfragesprache unterstützen:
* [Azure Monitor-Dokumentation](https://docs.microsoft.com/azure/azure-monitor/)
* [Erste Schritte mit Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)
* [Erste Schritte mit Protokollabfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)
* [Erstellen und Freigeben von Dashboards von Log Analytics-Daten](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards)

## <a name="sample-queries"></a>Beispielabfragen

### <a name="sample-query-1"></a>Beispielabfrage 1
Alle Kontosperrungsereignisse der letzten sieben Tage:
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Beispielabfrage 2
Alle Kontosperrungsereignisse (4740) zwischen dem 26. Juni 2019, 9 Uhr, und dem 1. Juli 2019, 0 Uhr, aufsteigend sortiert nach Datum und Uhrzeit:
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2019-06-26 09:00) and TimeGenerated <= datetime(2019-07-01) 
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Beispielabfrage 3
Kontoanmeldeereignisse vor sieben Tagen (ab dem aktuellen Zeitpunkt) für das Konto „user“:
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Beispielabfrage 4
Kontoanmeldeereignisse vor sieben Tagen (ab dem aktuellen Zeitpunkt) für das Konto „user“, bei denen ein falsches Kennwort verwendet wurde (0xC0000006a):
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Beispielabfrage 5
Kontoanmeldeereignisse vor sieben Tagen (ab dem aktuellen Zeitpunkt) für das Konto „user“, bei denen versucht wurde, eine Anmeldung durchzuführen, während das Konto gesperrt war (0xC0000234):
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Beispielabfrage 6
Die Anzahl von Kontoanmeldeereignissen vor sieben Tagen (ab dem aktuellen Zeitpunkt) für alle Anmeldeversuche aller gesperrten Benutzer.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="related-content"></a>Verwandte Inhalte
* [Übersicht](https://docs.microsoft.com/azure/kusto/query/) über die Kusto-Abfragesprache
* [Kusto-Tutorial](https://docs.microsoft.com/azure/kusto/query/tutorial) zur Vermittlung von Abfragegrundlagen
* [Beispielabfragen](https://docs.microsoft.com/azure/kusto/query/samples) zur Vermittlung neuer Sichtweisen auf Ihre Daten
* [Bewährte Methoden](https://docs.microsoft.com/azure/kusto/query/best-practices) für Kusto zur Optimierung Ihrer Abfragen














 
