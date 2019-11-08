---
title: Informationen zum Überwachen der Inhalte virtueller Computer
description: Erfahren Sie, wie Azure Policy mithilfe der Gastkonfiguration Einstellungen auf einem Azure-Computer überprüft.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/20/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: efe929a6ea38a8df7ad9fe37a92c181e3d409b25
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464057"
---
# <a name="understand-azure-policys-guest-configuration"></a>Informationen zu Guest Configuration von Azure Policy

Mit Azure Policy können Sie nicht nur Azure-Ressourcen überprüfen und [korrigieren](../how-to/remediate-resources.md), sondern auch Einstellungen auf einem Computer. Für die Überprüfung verwenden Sie die Erweiterung und den Client Guest Configuration. Die Erweiterung überprüft über den Client u. a. die folgenden Einstellungen:

- Die Konfiguration des Betriebssystems
- Die Konfiguration oder das Vorhandensein der Anwendung
- Umgebungseinstellungen

Derzeit führt die Azure Policy-Gastkonfiguration nur eine Überprüfung der Einstellungen auf dem Computer durch. Es werden keine Konfigurationen angewandt.

## <a name="extension-and-client"></a>Erweiterung und Client

Zur Überprüfung von Einstellungen auf einem Computer wird eine [VM-Erweiterung](../../../virtual-machines/extensions/overview.md) aktiviert. Mit der Erweiterung werden anwendbare Richtlinienzuweisungen sowie die entsprechende Konfigurationsdefinition heruntergeladen.

### <a name="limits-set-on-the-extension"></a>Für die Erweiterung festgelegte Grenzwerte

Um die Auswirkungen der Erweiterung auf die auf dem Computer ausgeführten Anwendungen zu beschränken, darf die Gastkonfiguration höchstens 5 % der CPU-Auslastung verursachen. Diese Einschränkung gilt sowohl für integrierte als auch für angepasste Definitionen.

## <a name="register-guest-configuration-resource-provider"></a>Registrieren des Guest Configuration-Ressourcenanbieters

Bevor Sie Guest Configuration verwenden können, müssen Sie den Ressourcenanbieter registrieren. Hierfür können Sie über das Portal oder mit PowerShell registrieren. Der Ressourcenanbieter wird automatisch registriert, wenn die Zuweisung einer Guest Configuration-Richtlinie über das Portal erfolgt.

### <a name="registration---portal"></a>Registrierung – Portal

Gehen Sie wie folgt vor, um den Ressourcenanbieter für Guest Configuration über das Azure-Portal zu registrieren:

1. Rufen Sie das Azure-Portal auf, und klicken Sie auf **Alle Dienste**. Suchen Sie nach **Abonnements**, und wählen Sie diese Option aus.

1. Suchen Sie nach dem Abonnement, das Sie für Guest Configuration aktivieren möchten, und klicken Sie darauf.

1. Klicken Sie im linken Menü der Seite **Abonnement** auf **Ressourcenanbieter**.

1. Suchen Sie durch Filtern oder Blättern nach **Microsoft.GuestConfiguration**, und klicken Sie dann in derselben Zeile auf **Registrieren**.

### <a name="registration---powershell"></a>Registrierung – PowerShell

Führen Sie zum Registrieren des Ressourcenanbieters für Guest Configuration über PowerShell den folgenden Befehl aus:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

## <a name="validation-tools"></a>Überprüfungstools

Auf dem Computer verwendet der Gastkonfigurationsclient lokale Tools zum Ausführen der Überprüfung.

In der folgenden Tabelle sind die lokalen Tools aufgeführt, die unter den jeweiligen unterstützten Betriebssystemen verwendet werden:

|Betriebssystem|Überprüfungstool|Notizen|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby und Python werden durch die Erweiterung Guest Configuration installiert. |

### <a name="validation-frequency"></a>Validierungshäufigkeit

Der Guest Configuration-Client prüft alle 5 Minuten, ob neuer Inhalt vorliegt. Sobald eine Gastzuweisung empfangen wird, werden die Einstellungen in einem 15-Minuten-Intervall überprüft. Die Ergebnisse werden an den Guest Configuration-Ressourcenanbieter gesendet, sobald die Überwachung abgeschlossen ist. Wenn eine [Auswertungsauslöser](../how-to/get-compliance-data.md#evaluation-triggers)-Richtlinie auftritt, wird der Zustand des Computers an den Guest Configuration-Ressourcenanbieter geschrieben. Dieses Update bewirkt, dass Azure Policy die Azure Resource Manager-Eigenschaften auswertet. Eine bedarfsgesteuerte Auswertung durch Azure Policy ruft den aktuellen Wert beim Guest Configuration-Ressourcenanbieter ab. Es wird jedoch keine neue Überwachung der Konfiguration auf dem Computer ausgelöst.

## <a name="supported-client-types"></a>Unterstützte Clienttypen

In der folgenden Tabelle sind die in Azure-Images unterstützten Betriebssysteme aufgeführt:

|Herausgeber|NAME|Versionen|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|Credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Windows-Client|Windows 10|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SUSE|SLES|12 SP3|

> [!IMPORTANT]
> Per Guest Configuration können Knoten überwacht werden, auf denen ein unterstütztes Betriebssystem ausgeführt wird. Wenn Sie virtuelle Computer überwachen möchten, für die ein benutzerdefiniertes Image verwendet wird, müssen Sie die **DeployIfNotExists**-Definition duplizieren und den **If**-Abschnitt so ändern, dass er Ihre Imageeigenschaften enthält.

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

Windows Server Nano Server wird in keiner Version unterstützt.

## <a name="guest-configuration-extension-network-requirements"></a>Netzwerkanforderungen für die Gastkonfigurationserweiterung

Für die Kommunikation mit dem Gastkonfigurations-Ressourcenanbieter in Azure benötigen Computer ausgehenden Zugriff auf Azure-Rechenzentren über Port **443**. Wenn Sie ein privates virtuelles Netzwerk in Azure verwenden und keinen ausgehenden Datenverkehr zulassen, müssen Ausnahmen über [Netzwerksicherheitsgruppen](../../../virtual-network/manage-network-security-group.md#create-a-security-rule)-Regeln konfiguriert werden. Derzeit ist für die Azure Policy-Gastkonfiguration kein Diensttag vorhanden.

Für Listen mit IP-Adressen können Sie [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei mit den jeweils aktuellen bereitgestellten Bereichen und allen anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht. Sie müssen nur Zugriff in ausgehender Richtung auf die IP-Adressen in den Regionen zulassen, in denen Ihre VMs bereitgestellt werden.

> [!NOTE]
> Die XML-Datei mit den IP-Adressen der Azure-Rechenzentren enthält die IP-Adressbereiche, die in den Microsoft Azure-Rechenzentren verwendet werden. Die Datei enthält die Bereiche für Compute, SQL und Storage. Eine aktualisierte Datei wird wöchentlich veröffentlicht. Die Datei enthält die derzeit bereitgestellten Bereichen und alle anstehenden Änderungen an den IP-Adressbereichen. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Rechenzentren verwendet. Sie sollten die neue XML-Datei jede Woche herunterladen. Führen Sie damit dann eine Aktualisierung an Ihrem Standort durch, um in Azure ausgeführte Dienste ordnungsgemäß zu identifizieren. Benutzer von Azure ExpressRoute sollten beachten, dass diese Datei zum Aktualisieren der BGP-Ankündigung (Border Gateway Protocol) von Azure-Bereichen jeweils in der ersten Woche des Monats verwendet wird.

## <a name="guest-configuration-definition-requirements"></a>Anforderungen an die Guest Configuration-Definition

Für jede mit Guest Configuration ausgeführte Überprüfung werden zwei Richtliniendefinitionen benötigt: **DeployIfNotExists** und **AuditIfNotExists**. Die Definition **DeployIfNotExists** dient zum Vorbereiten des Computers mit dem Gastkonfigurations-Agent und anderen Komponenten zur Unterstützung der [Validierungstools](#validation-tools).

Mit der Richtliniendefinition **DeployIfNotExists** werden die folgenden Elemente überprüft und korrigiert:

- Stellen Sie sicher, dass dem Computer eine auszuwertende Konfiguration zugewiesen wurde. Wenn derzeit keine Zuweisung vorhanden ist, gehen Sie wie folgt vor, um die Zuweisung abzurufen und den Computer vorzubereiten:
  - Authentifizieren Sie sich auf dem Computer mithilfe einer [verwalteten Identität](../../../active-directory/managed-identities-azure-resources/overview.md).
  - Installieren Sie die neueste Version der Erweiterung **Microsoft.GuestConfiguration**.
  - Installieren Sie [Überprüfungstools](#validation-tools) und ggf. Abhängigkeiten.

Wenn die **DeployIfNotExists**-Zuweisung nicht konform ist, kann ein [Wartungstask](../how-to/remediate-resources.md#create-a-remediation-task) verwendet werden.

Sobald die **DeployIfNotExists**-Zuweisung konform ist, wird die Richtliniendefinition **AuditIfNotExists** verwendet, um mithilfe der lokalen Überprüfungstools zu ermitteln, ob die Konfigurationszuweisung konform ist. Das Überprüfungstool stellt die Ergebnisse dem Guest Configuration-Client zur Verfügung. Der Client leitet die Ergebnisse an die Guest-Erweiterung weiter, die sie über den Guest Configuration-Ressourcenanbieter bereitstellt.

Azure Policy verwendet die Eigenschaft **complianceStatus** des Guest Configuration-Ressourcenanbieters, um die Konformität im Knoten **Konformität** zu melden. Weitere Informationen finden Sie unter [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).

> [!NOTE]
> Die Richtlinie **DeployIfNotExists** ist erforderlich, damit die Richtlinie **AuditIfNotExists** Ergebnisse zurückgibt. Ohne die Richtlinie **DeployIfNotExists** gibt die Richtlinie **AuditIfNotExists** „0 von 0“ Ressourcen als Status an.

Alle integrierten Richtlinien für Guest Configuration sind in einer Initiative zum Gruppieren der Definitionen zur Verwendung in Zuweisungen enthalten. Der integrierte Initiative mit dem Namen *[Vorschau]: Kennwortsicherheitseinstellungen auf Linux- und Windows-Computern überwachen* umfasst 18 Richtlinien. Es gibt sechs **DeployIfNotExists**- und **AuditIfNotExists**-Paare für Windows und drei für Linux. Die Logik der [Richtliniendefinition](definition-structure.md#policy-rule) stellt sicher, dass nur das Zielbetriebssystem ausgewertet wird.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Überwachen von Betriebssystemeinstellungen anhand von Branchenrichtlinien

Eine der in Azure Policy verfügbaren Initiativen bietet die Möglichkeit, Betriebssystemeinstellungen auf virtuellen Computern anhand einer „Baseline“ von Microsoft zu überwachen.  Die Definition *[Vorschau]: Überwachen von Windows-VMs, die nicht den Einstellungen für Azure-Sicherheitsbaselines entsprechen* enthält einen umfassenden Satz von Überwachungsregeln, die auf Einstellungen der Active Directory-Gruppenrichtlinie basieren.

Die meisten Einstellungen sind als Parameter verfügbar.  Mit dieser Funktion können Sie anpassen, was überwacht werden soll, um die Richtlinie an die Anforderungen Ihrer Organisation anzupassen, oder die Richtlinie Drittanbieterinformationen (z.B. branchenspezifischen Standards) zuordnen.

Einige Parameter unterstützen einen ganzzahligen Wertebereich.  Beispielsweise kann der Parameter „Maximales Kennwortalter“ mit einem Bereichsoperator festgelegt werden, um den Computerbesitzern Flexibilität zu bieten.  Sie können überwachen, dass die gültige Gruppenrichtlinieneinstellung, bei der Benutzer ihre Kennwörter ändern müssen, nicht mehr als 70 Tage, aber nicht weniger als 1 Tag betragen darf.  Wie in der Infoblase für den Parameter beschrieben, muss der Wert auf „1,70“ festgelegt werden, damit er zum gültigen Überwachungswert wird.

Wenn Sie die Richtlinie mithilfe einer Azure Resource Manager-Bereitstellungsvorlage zuweisen, können Sie mithilfe einer Parameterdatei diese Einstellungen aus der Quellcodeverwaltung verwalten.
Mithilfe eines Tools wie Git zum Verwalten von Änderungen an Überwachungsrichtlinien mit Kommentaren bei jedem Einchecken werden Nachweise dafür dokumentiert, warum eine Zuweisung eine Ausnahme vom erwarteten Wert sein sollte.

#### <a name="applying-configurations-using-guest-configuration"></a>Anwenden von Konfigurationen mithilfe der Gastkonfiguration

Mit der neuesten Funktion von Azure Policy werden Einstellungen in Computern konfiguriert.
Mit der Definition *Konfigurieren der Zeitzone auf Windows-Computern* werden Änderungen am Computer durch Konfigurieren der Zeitzone vorgenommen.

Beim Zuweisen von Definitionen, die mit *Konfigurieren* beginnen, müssen Sie auch die Definition *Bereitstellen von Voraussetzungen, um die Gastkonfigurationsrichtlinie auf Windows-VMs zu aktivieren* zuweisen.
Sie können diese Definitionen auf Wunsch in einer Initiative kombinieren.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Zuweisen von Richtlinien zu Computern außerhalb von Azure

Die für die Gastkonfiguration verfügbaren Überwachungsrichtlinien umfassen den Ressourcentyp **Microsoft.HybridCompute/machines**.  Alle Computer, die in Azure Arc integriert sind und sich im Bereich der Zuweisung befinden, werden automatisch eingeschlossen.

### <a name="multiple-assignments"></a>Mehrere Zuweisungen

Aufgrund der Gastkonfigurationsrichtlinien kann die gleiche Gastzuweisung derzeit lediglich einmal pro Computer zugewiesen werden, auch wenn bei der Richtlinienzuweisung andere Parameter verwendet werden.

## <a name="built-in-resource-modules"></a>Integrierte Ressourcenmodule

Beim Installieren der Gastkonfigurationserweiterung ist das PowerShell-Modul „GuestConfiguration“ in der neuesten Version von DSC-Ressourcenmodulen enthalten. Dieses Modul kann über den PowerShell-Katalog mithilfe des Links „Manueller Download“ auf der Modulseite [GuestConfiguration](https://www.powershellgallery.com/packages/GuestConfiguration/) heruntergeladen werden.
Das Dateiformat „.nupkg“ kann zur Dekomprimierung und Überprüfung in „.zip“ umbenannt werden.

## <a name="client-log-files"></a>Protokolldateien des Clients

Die Guest Configuration-Erweiterung schreibt Protokolldateien an die folgenden Speicherorte:

Windows: `C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\<version>\dsc\logs\dsc.log`

Linux: `/var/lib/waagent/Microsoft.GuestConfiguration.ConfigurationforLinux-<version>/GCAgent/logs/dsc.log`

Dabei bezieht sich `<version>` auf die aktuelle Versionsnummer.

### <a name="collecting-logs-remotely"></a>Remotesammeln von Protokollen

Im ersten Schritt bei der Problembehandlung von Konfigurationen oder Modulen durch die Gastkonfiguration sollte das Cmdlet `Test-GuestConfigurationPackage` anhand der Schritte in [Testen eines Gastkonfigurationspakets](../how-to/guest-configuration-create.md#test-a-guest-configuration-package) ausgeführt werden.
Wenn dies nicht erfolgreich ist, kann das Sammeln von Clientprotokollen helfen, Probleme zu diagnostizieren.

#### <a name="windows"></a>Windows

Das folgende PowerShell-Beispielskript kann hilfreich sein, um die Funktion „Befehl ausführen“ auf der Azure-VM zum Erfassen von Informationen aus Protokolldateien auf Windows-Computern zu verwenden. Weitere Informationen finden Sie unter [Ausführen von PowerShell-Skripts auf Ihrer Windows-VM mit „Befehl ausführen“](../../../virtual-machines/windows/run-command.md).

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$latestVersion = Get-ChildItem -Path 'C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\' | ForEach-Object {$_.FullName} | Sort-Object -Descending | Select-Object -First 1
Select-String -Path "$latestVersion\dsc\logs\dsc.log" -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Das folgende Bash-Beispielskript kann hilfreich sein, um die Funktion „Befehl ausführen“ auf der Azure-VM zum Erfassen von Informationen aus Protokolldateien auf Linux-Computern zu verwenden. Weitere Informationen finden Sie unter [Ausführen von PowerShell-Skripts auf Ihrer Linux-VM mit „Befehl ausführen“](../../../virtual-machines/linux/run-command.md).

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
latestVersion=$(find /var/lib/waagent/ -type d -name "Microsoft.GuestConfiguration.ConfigurationforLinux-*" -maxdepth 1 -print | sort -z | sed -n 1p)
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' "$latestVersion/GCAgent/logs/dsc.log" | tail
```

## <a name="guest-configuration-samples"></a>Beispiele für Guest Configuration

Beispiele für Guest Configuration von Policy finden Sie unter:

- [Beispielindex: Guest Configuration](../samples/index.md#guest-configuration)
- [Azure Policy-Beispiele: GitHub-Repository](https://github.com/Azure/azure-policy/tree/master/samples/GuestConfiguration)

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
