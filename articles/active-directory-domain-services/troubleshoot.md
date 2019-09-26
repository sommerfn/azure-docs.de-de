---
title: Problembehandlung für Azure Active Directory Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie häufige Fehler beim Erstellen oder Verwalten von Azure Active Directory Domain Services beheben.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: iainfou
ms.openlocfilehash: 5c2a8c8cfa2425985a22b93d4ade509320c48564
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998731"
---
# <a name="common-errors-and-troubleshooting-steps-for-azure-active-directory-domain-services"></a>Häufige Fehler und Schritte zur Problembehandlung für Azure Active Directory Domain Services

Als zentraler Bestandteil der Identität und Authentifizierung für Anwendungen treten bei Azure Active Directory Domain Services (Azure AD DS) manchmal Probleme auf. Wenn das der Fall ist, können Sie mithilfe einiger häufiger Fehlermeldungen und zugehöriger Schritte zur Problembehandlung diese Fehler beheben. Sie können auch jederzeit [eine Azure-Supportanfrage öffnen][azure-support], um weitere Hilfe bei der Problembehandlung zu erhalten.

Dieser Artikel enthält Schritte zur Behandlung von häufig auftretenden Problemen in Azure AD DS.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Sie können Azure AD Domain Services für Ihr Azure AD-Verzeichnis nicht aktivieren

Wenn Probleme beim Aktivieren von Azure AD DS auftreten, sehen Sie sich die folgenden häufigen Fehler und die Schritte zu deren Lösung an:

| **Beispielfehlermeldung** | **Lösung** |
| --- |:--- |
| *Der Name contoso.com wird in diesem Netzwerk bereits verwendet. Geben Sie einen Namen an, der nicht verwendet wird.* |[Domänennamenskonflikt im virtuellen Netzwerk](troubleshoot.md#domain-name-conflict) |
| *Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Der Dienst verfügt nicht über die erforderlichen Berechtigungen für die Anwendung „Azure AD Domain Services Sync“. Löschen Sie die Anwendung „Azure AD Domain Services Sync“, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.* |[Domänendienste verfügen nicht über die erforderlichen Berechtigungen für die Anwendung „Azure AD Domain Services Sync“](troubleshoot.md#inadequate-permissions) |
| *Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Die Domänendiensteanwendung in Ihrem Azure AD-Mandanten verfügt nicht über die erforderlichen Berechtigungen zum Aktivieren der Domänendienste. Löschen Sie die Anwendung mit dem Anwendungsbezeichner d87dcbc6-a371-462e-88e3-28ad15ec4e64, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.* |[Die Domänendiensteanwendung ist in Ihrem Azure AD-Mandanten nicht ordnungsgemäß konfiguriert](troubleshoot.md#invalid-configuration) |
| *Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Die Microsoft Azure AD-Anwendung in Ihrem Azure AD-Mandanten ist deaktiviert. Aktivieren Sie die Anwendung mit dem Anwendungsbezeichner 00000002-0000-0000-c000-000000000000, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.* |[Die Microsoft Graph-Anwendung in Ihrem Azure AD-Mandanten ist deaktiviert.](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Domänennamenskonflikt

**Fehlermeldung**

*Der Name contoso.com wird in diesem Netzwerk bereits verwendet. Geben Sie einen Namen an, der nicht verwendet wird.*

**Lösung**

Vergewissern Sie sich, dass im virtuellen Netzwerk keine AD DS-Umgebung mit dem gleichen Domänennamen vorhanden ist. Sie können beispielsweise über eine AD DS-Domäne namens *contoso.com* verfügen, die auf Azure-VMs ausgeführt wird. Wenn Sie versuchen, im virtuellen Netzwerk eine durch Azure AD DS verwaltete Domäne mit dem gleichen Domänennamen *contoso.com* zu aktivieren, tritt beim angeforderten Vorgang ein Fehler auf.

Der Grund für diesen Fehler ist ein Namenskonflikt in Bezug auf den Domänennamen im virtuellen Netzwerk. Mit einem DNS-Lookup wird geprüft, ob eine vorhandene AD DS-Umgebung auf den angeforderten Domänennamen antwortet. Zur Behebung dieses Fehlers verwenden Sie einen anderen Namen beim Einrichten der durch Azure AD DS verwalteten Domäne, oder heben Sie die Bereitstellung der vorhandenen AD DS-Domäne auf, und versuchen Sie dann erneut, Azure AD DS zu aktivieren.

### <a name="inadequate-permissions"></a>Ungeeignete Berechtigungen

**Fehlermeldung**

*Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Der Dienst verfügt nicht über die erforderlichen Berechtigungen für die Anwendung „Azure AD Domain Services Sync“. Löschen Sie die Anwendung „Azure AD Domain Services Sync“, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.*

**Lösung**

Prüfen Sie, ob sich in Ihrem Azure AD-Verzeichnis eine Anwendung namens *Azure AD Domain Services Sync* befindet. Falls diese Anwendung vorhanden ist, löschen Sie sie, und versuchen Sie dann erneut, Azure AD DS zu aktivieren. Führen Sie die folgenden Schritte aus, um nach einer vorhandenen Anwendung zu suchen und sie bei Bedarf zu löschen:

1. Wählen Sie im Azure-Portal im linken Navigationsmenü die Option **Azure Active Directory** aus.
1. Wählen Sie **Unternehmensanwendungen**. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option *Alle Anwendungen* und dann **Anwenden** aus.
1. Geben Sie im Suchfeld *Azure AD Domain Services Sync* ein. Wenn die Anwendung vorhanden ist, wählen Sie die Anwendung und dann **Löschen** aus.
1. Sobald Sie die Anwendung gelöscht haben, versuchen Sie erneut, Azure AD DS zu aktivieren.

### <a name="invalid-configuration"></a>Ungültige Konfiguration

**Fehlermeldung**

*Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Die Domänendiensteanwendung in Ihrem Azure AD-Mandanten verfügt nicht über die erforderlichen Berechtigungen zum Aktivieren der Domänendienste. Löschen Sie die Anwendung mit dem Anwendungsbezeichner d87dcbc6-a371-462e-88e3-28ad15ec4e64, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.*

**Lösung**

Prüfen Sie, ob eine Anwendung namens *AzureActiveDirectoryDomainControllerServices* mit dem Anwendungsbezeichner *d87dcbc6-a371-462e-88e3-28ad15ec4e64* in Ihrem Azure AD-Verzeichnis vorhanden ist. Falls diese Anwendung vorhanden ist, löschen Sie sie, und versuchen Sie dann erneut, Azure AD DS zu aktivieren.

Verwenden Sie das folgende PowerShell-Skript, um nach einer vorhandenen Anwendungsinstanz zu suchen und diese bei Bedarf zu löschen.

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```

### <a name="microsoft-graph-disabled"></a>Microsoft Graph ist deaktiviert

**Fehlermeldung**

*Die Domänendienste konnten in diesem Azure AD-Mandanten nicht aktiviert werden. Die Microsoft Azure AD-Anwendung in Ihrem Azure AD-Mandanten ist deaktiviert. Aktivieren Sie die Anwendung mit dem Anwendungsbezeichner 00000002-0000-0000-c000-000000000000, und versuchen Sie dann, die Domänendienste für Ihren Azure AD-Mandanten zu aktivieren.*

**Lösung**

Prüfen Sie, ob Sie eine Anwendung mit dem Bezeichner *00000002-0000-0000-c000-000000000000* deaktiviert haben. Diese Anwendung ist die Microsoft Azure AD-Anwendung und stellt den Graph-API-Zugriff für Ihren Azure AD-Mandanten bereit. Diese Anwendung muss aktiviert sein, um Ihren Azure AD-Mandanten zu synchronisieren.

Führen Sie die folgenden Schritte aus, um den Status dieser Anwendung zu prüfen und sie bei Bedarf zu aktivieren:

1. Wählen Sie im Azure-Portal im linken Navigationsmenü die Option **Azure Active Directory** aus.
1. Wählen Sie **Unternehmensanwendungen**. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option *Alle Anwendungen* und dann **Anwenden** aus.
1. Geben Sie im Suchfeld *00000002-0000-0000-c000-00000000000* ein. Wählen Sie die Anwendung und dann **Eigenschaften** aus.
1. Wenn **Aktiviert für die Benutzeranmeldung?** auf *Nein* festgelegt ist, ändern Sie den Wert in *Ja*, und wählen Sie dann **Speichern** aus.
1. Sobald Sie die Anwendung aktiviert haben, versuchen Sie erneut, Azure AD DS zu aktivieren.

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Benutzer können sich nicht bei der verwalteten Domäne der Azure AD Domain Services anmelden

Falls sich mindestens ein Benutzer innerhalb Ihres Azure AD-Mandanten nicht bei der durch Azure AD DS verwalteten Domäne anmelden kann, führen Sie die folgenden Schritte zur Problembehandlung aus:

* **Format der Anmeldeinformationen**: Geben Sie die Anmeldeinformationen im UPN-Format an, z.B. als `dee@contoso.onmicrosoft.com`. Das UPN-Format wird zur Angabe von Anmeldeinformationen in Azure AD DS empfohlen. Stellen Sie sicher, dass dieser UPN in Azure AD ordnungsgemäß konfiguriert ist.

    Der *SAMAccountName* für Ihr Konto (z.B. *CONTOSO\driley*) wird möglicherweise automatisch generiert, wenn mehrere Benutzer in Ihrem Mandanten das gleiche UPN-Präfix verwenden oder wenn Ihr UPN-Präfix übermäßig lang ist. Das Format *SAMAccountName* für Ihr Konto ist möglicherweise anders als Sie erwarten bzw. unterscheidet sich von dem, was Sie in Ihrer lokalen Domäne verwenden.

* **Kennwortsynchronisierung**: Stellen Sie sicher, dass Sie die Kennwortsynchronisierung für [reine Cloudbenutzer][cloud-only-passwords] oder für [Hybridumgebungen mit Azure AD Connect][hybrid-phs] aktiviert haben.
    * **Hybride synchronisierte Konten:** Falls die betroffenen Benutzerkonten über ein lokales Verzeichnis synchronisiert werden, überprüfen Sie Folgendes:
    
      * Sie haben die [neueste empfohlene Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) bereitgestellt bzw. das Update darauf durchgeführt.
      * Sie haben Azure AD Connect so konfiguriert, dass [eine vollständige Synchronisierung ausgeführt wird][hybrid-phs].
      * Je nach Größe Ihres Verzeichnisses kann es einige Zeit dauern, bis die Benutzerkonten und Anmeldeinformationshashes in Azure AD DS verfügbar sind. Vergewissern Sie sich, dass Sie lange genug warten, bevor Sie versuchen, sich bei der verwalteten Domäne zu authentifizieren.
      * Wenn das Problem nach Überprüfung der vorherigen Schritte weiterhin auftritt, starten Sie den *Microsoft Azure AD Sync-Dienst* neu. Öffnen Sie auf der [Verwaltungs-VM][management-vm] eine Eingabeaufforderung, und führen Sie die folgenden Befehle aus:
    
        ```console
        net stop 'Microsoft Azure AD Sync'
        net start 'Microsoft Azure AD Sync'
        ```

    * **Reine Cloudkonten**: Falls es sich bei dem betroffenen Benutzerkonto um ein reines Cloudbenutzerkonto handelt, vergewissern Sie sich, dass der [Benutzer sein Kennwort geändert hat, nachdem Sie Azure AD DS aktiviert haben][cloud-only-passwords]. Durch diese Kennwortzurücksetzung werden die erforderlichen Anmeldeinformationshashes für Azure AD Domain Services generiert.

* **Vergewissern Sie sich, dass das Benutzerkonto aktiv ist**: Standardmäßig bewirken fünf erfolglose Kennworteingaben in der verwalteten Domäne innerhalb von zwei Minuten, dass das Benutzerkonto für 30 Minuten gesperrt wird. Der Benutzer kann sich nicht anmelden, während das Konto gesperrt ist. Nach 30 Minuten wird das Benutzerkonto automatisch entsperrt.
  * Durch ungültige Kennworteingaben in der durch Azure AD DS verwalteten Domäne wird das Benutzerkonto in Azure AD nicht gesperrt. Das Benutzerkonto wird nur in der verwalteten Domäne gesperrt. Überprüfen Sie den Status des Benutzerkontos in der *Active Directory-Verwaltungskonsole (Active Directory Administrative Console, ADAC)* über die [Verwaltungs-VM][management-vm] und nicht in Azure AD.
  * Sie können auch [differenzierte Kennwortrichtlinien konfigurieren][password-policy], um den Standardsperrschwellenwert und die entsprechende Dauer zu ändern.

* **Externe Konten**: Stellen Sie sicher, dass das betroffene Benutzerkonto kein externes Konto im Azure AD-Mandanten ist. Beispiele für externe Konten sind Microsoft-Konten wie `dee@live.com` oder Benutzerkonten aus einem externen Azure AD-Verzeichnis. Azure AD DS speichert keine Anmeldeinformationen für externe Benutzerkonten, sodass sich diese nicht bei der verwalteten Domäne anmelden können.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Es gibt mindestens eine Warnung zu Ihrer verwalteten Domäne

Wenn aktive Warnungen für die durch Azure AD DS verwaltete Domäne vorhanden sind, funktioniert der Authentifizierungsprozess möglicherweise nicht ordnungsgemäß.

[Überprüfen Sie den Integritätsstatus einer durch Azure AD DS verwalteten Domäne][check-health], um festzustellen, ob aktive Warnungen vorhanden sind. Wenn Warnungen angezeigt werden, [behandeln und lösen Sie die Probleme][troubleshoot-alerts].

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Aus Ihrem Azure AD-Mandanten entfernte Benutzer werden nicht aus Ihrer verwalteten Domäne entfernt

Azure AD schützt vor versehentlichem Löschen von Benutzerobjekten. Wenn Sie ein Benutzerkonto aus einem Azure AD-Mandanten löschen, wird das zugehörige Benutzerobjekt in den Papierkorb verschoben. Wenn dieser Löschvorgang mit Ihrer durch Azure AD DS verwalteten Domäne synchronisiert wird, wird das zugehörige Benutzerkonto als deaktiviert markiert. Dieses Feature unterstützt Sie dabei, das Benutzerkonto wiederherzustellen oder den Löschvorgang rückgängig zu machen.

Das Benutzerkonto behält in der durch Azure AD DS verwalteten Domäne auch dann den deaktivierten Status bei, wenn Sie im Azure AD-Verzeichnis ein Benutzerkonto mit demselben UPN neu erstellen. Um das Benutzerkonto aus der durch Azure AD DS verwalteten Domäne zu entfernen, müssen Sie die Löschung aus dem Azure AD-Mandanten erzwingen.

Um ein Benutzerkonto vollständig aus einer durch Azure AD DS verwalteten Domäne zu entfernen, löschen Sie den Benutzer dauerhaft aus Ihrem Azure AD-Mandanten, indem Sie das PowerShell-Cmdlet [Remove-MsolUser][Remove-MsolUser] mit dem Parameter `-RemoveFromRecycleBin` verwenden.

## <a name="next-steps"></a>Nächste Schritte

Falls weiterhin Probleme auftreten, [öffnen Sie eine Azure-Supportanfrage][azure-support], um weitere Hilfe bei der Problembehandlung zu erhalten.

<!-- INTERNAL LINKS -->
[cloud-only-passwords]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[hybrid-phs]: tutorial-configure-password-hash-sync.md
[management-vm]: tutorial-create-management-vm.md
[password-policy]: password-policy.md
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
[Remove-MsolUser]: /powershell/module/MSOnline/Remove-MsolUser
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
