---
title: Migrieren von Azure AD Domain Services aus einem klassischen virtuellen Netzwerk | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine vorhandene Instanz einer verwalteten Azure AD Domain Services-Domäne vom klassischen VNET-Modell zu einem Resource Manager-basierten virtuellen Netzwerk migrieren.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: iainfou
ms.openlocfilehash: 8cba2cbf8fcbad1acae8c36892308c3249fc4181
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2019
ms.locfileid: "72674904"
---
# <a name="preview---migrate-azure-ad-domain-services-from-the-classic-virtual-network-model-to-resource-manager"></a>Vorschauversion: Migrieren von Azure AD Domain Services vom klassischen VNET-Modell zu Resource Manager

Für Azure Active Directory Domain Services (AD DS) wird für Kunden, die derzeit das klassische VNET-Modell nutzen, eine einmalige Umstellung auf das Resource Manager-basierte VNET-Modell unterstützt.

In diesem Artikel werden die Vorteile und Aspekte der Migration und anschließend die erforderlichen Schritte für eine erfolgreiche Migration zu einer vorhandenen Azure AD DS-Instanz beschrieben. Diese Funktion steht derzeit als Vorschau zur Verfügung.

## <a name="overview-of-the-migration-process"></a>Übersicht über den Migrationsprozess

Beim Migrationsprozess wird eine vorhandene Azure AD DS-Instanz, die in einem klassischen virtuellen Netzwerk ausgeführt wird, in ein vorhandenes Resource Manager-basiertes virtuelles Netzwerk verschoben. Die Migration wird mit PowerShell durchgeführt und verfügt über zwei Hauptausführungsphasen: *Vorbereitung* und *Migration*.

![Übersicht über den Migrationsprozess für Azure AD DS](media/migrate-from-classic-vnet/migration-overview.png)

In der Phase *Vorbereitung* wird in Azure AD DS eine Sicherung der Domäne verwendet, um die aktuelle Momentaufnahme mit den Benutzern, Gruppen und Kennwörtern, die für die verwaltete Domäne synchronisiert wurden, zu erhalten. Anschließend wird die Synchronisierung deaktiviert, und der Clouddienst, der die verwaltete Azure AD DS-Domäne hostet, wird gelöscht. Während der Vorbereitungsphase kann die verwaltete Azure AD DS-Domäne keine Benutzer authentifizieren.

![Vorbereitungsphase für die Migration von Azure AD DS](media/migrate-from-classic-vnet/migration-preparation.png)

In der Phase *Migration* werden die zugrunde liegenden virtuellen Datenträger für die Domänencontroller aus der klassischen verwalteten Azure AD DS-Domäne kopiert, um die VMs mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Anschließend wird die verwaltete Azure AD DS-Domäne, die die LDAPS- und DNS-Konfiguration enthält, neu erstellt. Die Synchronisierung mit Azure AD wird neu gestartet, und die LDAP-Zertifikate werden wiederhergestellt. Es ist nicht erforderlich, Computer erneut in eine verwaltete Azure AD DS-Domäne einzubinden. Sie sind weiterhin in die verwaltete Domäne eingebunden und werden ohne Änderungen ausgeführt.

![Migration von Azure AD DS](media/migrate-from-classic-vnet/migration-process.png)

## <a name="migration-benefits"></a>Vorteile der Migration

Wenn Sie eine verwaltete Azure AD DS-Domäne mit diesem Migrationsprozess verschieben, entfällt die Anforderung, Computer in die verwaltete Domäne einzubinden oder die Azure AD DS-Instanz zu löschen und eine neue zu erstellen. Nach Abschluss des Migrationsvorgangs bleiben virtuelle Computer in die verwaltete Azure AD DS-Domäne eingebunden.

Nach der Migration verfügt Azure AD DS über viele Features, die nur für Domänen verfügbar sind, für die virtuelle Resource Manager-Netzwerke verwendet werden, z. B.:

* Unterstützung einer differenzierten Kennwortrichtlinie
* Schutz vor AD-Kontosperrung
* E-Mail-Benachrichtigungen über Warnungen in der verwalteten Azure AD DS-Domäne
* Überwachungsprotokolle mit Verwendung von Azure Monitor
* Azure Files-Integration
* HDInsights-Integration

Mit verwalteten Azure AD DS-Domänen, für die ein virtuelles Resource Manager-Netzwerk verwendet wird, können Sie über die neuesten Features auf dem Laufenden bleiben. Die Unterstützung für Azure AD DS mit klassischen virtuellen Netzwerken wird in Kürze als veraltet eingestuft.

## <a name="example-scenarios-for-migration"></a>Beispielszenarien für die Migration

Hier sind einige Beispiele für häufige Szenarien zur Migration einer verwalteten Azure AD DS-Domäne angegeben.

> [!NOTE]
> Führen Sie die Konvertierung des klassischen virtuellen Netzwerks erst durch, nachdem Sie die Bestätigung über die erfolgreiche Migration erhalten haben. Mit dem Konvertieren des virtuellen Netzwerks entfällt die Option zur Ausführung eines Rollbacks oder einer Wiederherstellung der verwalteten Azure AD DS-Domäne, falls während der Migrations- und Überprüfungsphase Probleme auftreten.

### <a name="migrate-azure-ad-ds-to-an-existing-resource-manager-virtual-network-recommended"></a>Migrieren von Azure AD DS zu einem vorhandenen virtuellen Resource Manager-Netzwerk (empfohlen)

Ein gängiges Szenario ist ein Fall, in dem Sie bereits andere vorhandene klassische Ressourcen auf ein Resource Manager-Bereitstellungsmodell und ein virtuelles Netzwerk umgestellt haben. Anschließend wird ein Peering vom virtuellen Resource Manager-Netzwerk zum klassischen virtuellen Netzwerk eingerichtet, in dem Azure AD DS weiter ausgeführt wird. Mit diesem Ansatz können die Resource Manager-Anwendungen und -Dienste die Authentifizierungs- und Verwaltungsfunktionen der verwalteten Azure AD DS-Domäne im klassischen virtuellen Netzwerk nutzen. Nach der Migration werden alle Ressourcen mit dem Resource Manager-Bereitstellungsmodell und dem virtuellen Netzwerk ausgeführt.

![Migrieren von Azure AD DS zu einem vorhandenen virtuellen Resource Manager-Netzwerk](media/migrate-from-classic-vnet/migrate-to-existing-vnet.png)

Die allgemeinen Schritte dieses Beispiels für ein Migrationsszenario umfassen Folgendes:

1. Entfernen vorhandener VPN-Gateways oder VNET-Peerings, die im klassischen virtuellen Netzwerk konfiguriert sind
1. Migrieren der verwalteten Azure AD DS-Domäne mit den Schritten in diesem Artikel
1. Testen und Bestätigen einer erfolgreichen Migration und Löschen des klassischen virtuellen Netzwerks

### <a name="migrate-multiple-resources-including-azure-ad-ds"></a>Migrieren mehrerer Ressourcen, einschließlich Azure AD DS

In diesem Beispielszenario migrieren Sie Azure AD DS und andere zugehörige Ressourcen aus dem klassischen Bereitstellungsmodell zum Resource Manager-Bereitstellungsmodell. Wenn einige Ressourcen im klassischen virtuellen Netzwerk parallel zur verwalteten Azure AD DS-Domäne weiter ausgeführt werden, können alle Ressourcen von der Migration zum Resource Manager-Bereitstellungsmodell profitieren.

![Migrieren mehrerer Ressourcen zum Resource Manager-Bereitstellungsmodell](media/migrate-from-classic-vnet/migrate-multiple-resources.png)

Die allgemeinen Schritte dieses Beispiels für ein Migrationsszenario umfassen Folgendes:

1. Entfernen vorhandener VPN-Gateways oder VNET-Peerings, die im klassischen virtuellen Netzwerk konfiguriert sind
1. Migrieren der verwalteten Azure AD DS-Domäne mit den Schritten in diesem Artikel
1. Einrichten des Peerings virtueller Netzwerke zwischen dem klassischen virtuellen Netzwerk und dem Resource Manager-Netzwerk
1. Testen und Bestätigen einer erfolgreichen Migration
1. [Verschieben von zusätzlichen klassischen Ressourcen, z. B. VMs][migrate-iaas]

### <a name="migrate-azure-ad-ds-but-keep-other-resources-on-the-classic-virtual-network"></a>Migrieren von Azure AD DS unter Beibehaltung anderer Ressourcen im klassischen virtuellen Netzwerk

In diesem Beispielszenario erzielen Sie die geringstmögliche Ausfallzeit in einer Sitzung. Sie führen nur die Migration von Azure AD DS zu einem virtuellen Resource Manager-Netzwerk durch und behalten vorhandene Ressourcen im klassischen Bereitstellungsmodell und virtuellen Netzwerk bei. In einem der folgenden Wartungszeiträume können Sie die zusätzlichen Ressourcen wie gewünscht aus dem klassischen Bereitstellungsmodell und dem virtuellen Netzwerk migrieren.

![Ausschließliches Migrieren von Azure AD DS zum Resource Manager-Bereitstellungsmodell](media/migrate-from-classic-vnet/migrate-only-azure-ad-ds.png)

Die allgemeinen Schritte dieses Beispiels für ein Migrationsszenario umfassen Folgendes:

1. Entfernen vorhandener VPN-Gateways oder VNET-Peerings, die im klassischen virtuellen Netzwerk konfiguriert sind
1. Migrieren der verwalteten Azure AD DS-Domäne mit den Schritten in diesem Artikel
1. Einrichten des Peerings virtueller Netzwerke zwischen dem klassischen virtuellen Netzwerk und dem neuen virtuellen Resource Manager-Netzwerk
1. Führen Sie je nach Bedarf zu einem späteren Zeitpunkt die [Migration der zusätzlichen Ressourcen][migrate-iaas] aus dem klassischen virtuellen Netzwerk durch.

## <a name="before-you-begin"></a>Voraussetzungen

Während der Vorbereitung und anschließenden Migration einer verwalteten Azure AD DS-Domäne sollten Sie einige Aspekte zur Verfügbarkeit der Authentifizierung und Verwaltungsdienste beachten. Die verwaltete Azure AD DS-Domäne ist während der Migration eine Zeit lang nicht verfügbar. Für Anwendungen und Dienste, die auf Azure AD DS basieren, kommt es während der Migration zu Ausfallzeiten.

> [!IMPORTANT]
> Lesen Sie sich diesen Artikel zur Migration und den Leitfaden durch, bevor Sie mit dem Migrationsprozess beginnen. Der Migrationsprozess wirkt sich zu bestimmten Zeiten auf die Verfügbarkeit der Azure AD DS-Domänencontroller aus. Benutzer, Dienste und Anwendungen können während des Migrationsvorgangs nicht für die verwaltete Domäne authentifiziert werden.

### <a name="ip-addresses"></a>IP-Adressen

Die Domänencontroller-IP-Adressen für eine verwaltete Azure AD DS-Domäne ändern sich nach der Migration. Dies gilt auch für die öffentliche IP-Adresse des Secure LDAP-Endpunkts. Die neuen IP-Adressen liegen innerhalb des Adressbereichs für das neue Subnetz im virtuellen Resource Manager-Netzwerk.

Bei einem Rollback können sich die IP-Adressen nach dem Rollbackvorgang ändern.

Von Azure AD DS werden normalerweise die ersten beiden verfügbaren IP-Adressen im Adressbereich verwendet, aber dies ist nicht garantiert. Sie können derzeit nicht die IP-Adressen angeben, die nach der Migration verwendet werden sollen.

### <a name="downtime"></a>Ausfallzeit

Während des Migrationsprozesses sind die Domänencontroller eine Zeit lang offline. Auf Domänencontroller kann nicht zugegriffen werden, während für Azure AD DS die Migration zum Resource Manager-Bereitstellungsmodell und virtuellen Netzwerk durchgeführt wird. Im Durchschnitt hat die Ausfallzeit eine Länge von ein bis drei Stunden. Dieser Zeitraum reicht von dem Moment, in dem die Domänencontroller in den Offlinezustand versetzt werden, bis zum dem Zeitpunkt, zu dem der erste Domänencontroller wieder online ist. Dieser Durchschnittswert enthält nicht die Zeit, die zum Replizieren des zweiten Domänencontrollers benötigt wird, oder die Zeit, die ggf. für die Migration zusätzlicher Ressourcen zum Resource Manager-Bereitstellungsmodell aufgewendet werden muss.

### <a name="account-lockout"></a>Kontosperrung

Verwaltete Azure AD DS-Domänen, die in klassischen virtuellen Netzwerken ausgeführt werden, verfügen nicht über AD-Kontosperrungsrichtlinien. Wenn VMs im Internet verfügbar gemacht werden, können Angreifer ggf. Kennwortspray-Methoden verwenden, um einen Brute-Force-Angriff auf Konten durchzuführen. Es gibt keine Kontosperrungsrichtlinie, mit der diese Versuche verhindert werden können. Bei verwalteten Azure AD DS-Domänen, für die das Resource Manager-Bereitstellungsmodell und virtuelle Netzwerke verwendet werden, werden AD-Kontosperrungsrichtlinien als Schutz vor diesen Kennwortspray-Angriffen eingesetzt.

Standardmäßig führen fünf Versuche mit fehlerhaften Kennwörtern innerhalb von zwei Minuten zu einer Kontosperrung von 30 Minuten.

Für ein gesperrtes Konto ist die Anmeldung nicht möglich. Dies kann zu einer Störung bei der Verwaltung der verwalteten Azure AD DS-Domäne oder der mit dem Konto verwalteten Anwendungen führen. Nachdem eine verwaltete Azure AD DS-Domäne migriert wurde, kann es für Konten zu einer gefühlten dauerhaften Sperrung aufgrund von wiederholten fehlerhaften Anmeldeversuchen kommen. Zwei gängige Szenarien nach der Migration sind:

* Ein Dienstkonto, für das ein abgelaufenes Kennwort verwendet wird.
    * Das Dienstkonto versucht wiederholt, sich mit einem abgelaufenen Kennwort anzumelden, sodass das Konto gesperrt wird. Greifen Sie zum Beheben dieses Problems auf die Anwendung oder VM zu, für die die Anmeldeinformationen abgelaufen sind, und aktualisieren Sie das Kennwort.
* Eine schädliche Entität versucht, Brute-Force-Angriffe durchzuführen, um sich an Konten anzumelden.
    * Wenn VMs für den Zugriff über das Internet verfügbar gemacht werden, probieren Angreifer bei Anmeldeversuchen oft häufig verwendete Kombinationen aus Benutzername und Kennwort aus. Diese wiederholten Anmeldeversuche, die nicht erfolgreich sind, können zur Sperrung des Kontos führen. Wir raten Ihnen davon ab, Administratorkonten mit generischen Namen zu verwenden (z. B. *admin* oder *administrator*), um für diese Konten die Gefahr einer Sperrung zu verringern.
    * Reduzieren Sie die Anzahl von VMs, die für den Zugriff über das Internet verfügbar gemacht werden. Sie können [Azure Bastion (derzeit in der Vorschauphase)][azure-bastion] verwenden, um über das Azure-Portal eine sichere Verbindung mit VMs herzustellen.

Falls Sie vermuten, dass einige Konten nach der Migration unter Umständen gesperrt sind, helfen Ihnen die abschließenden Migrationsschritte weiter. Darin wird beschrieben, wie Sie die Überprüfung aktivieren oder die Einstellungen für differenzierte Kennwortrichtlinien ändern.

### <a name="roll-back-and-restore"></a>Rollback und Wiederherstellung

Wenn die Migration nicht erfolgreich ist, können Sie den Prozess zum Ausführen eines Rollbacks oder Wiederherstellen einer verwalteten Azure AD DS-Domäne verwenden. Ein Rollback ist eine Self-Service-Option, mit der Sie den Status für die verwaltete Domäne sofort auf den Stand vor der versuchten Migration zurücksetzen können. Azure-Supporttechniker können als letzte Möglichkeit auch eine Wiederherstellung der verwalteten Domäne aus einer Sicherung durchführen. Weitere Informationen finden Sie unter [Ausführen eines Rollbacks oder einer Wiederherstellung nach einer fehlgeschlagenen Migration](#roll-back-and-restore-from-migration).

### <a name="restrictions-on-available-virtual-networks"></a>Einschränkungen für verfügbare virtuelle Netzwerke

Für die virtuellen Netzwerke, zu denen eine verwaltete Azure AD DS-Domäne migriert werden kann, gelten einige Einschränkungen. Für das virtuelle Resource Manager-Zielnetzwerk müssen die folgenden Anforderungen erfüllt sein:

* Das virtuelle Resource Manager-Netzwerk muss sich unter demselben Azure-Abonnement wie das klassische virtuelle Netzwerk befinden, in dem Azure AD DS derzeit bereitgestellt ist.
* Das virtuelle Resource Manager-Netzwerk muss sich in derselben Region wie das klassische virtuelle Netzwerk befinden, in dem Azure AD DS derzeit bereitgestellt ist.
* Das Subnetz des virtuellen Resource Manager-Netzwerks sollte mindestens über drei bis fünf verfügbare IP-Adressen verfügen.
* Das Subnetz des virtuellen Resource Manager-Netzwerks sollte ein dediziertes Subnetz für Azure AD DS sein und keine anderen Workloads hosten.

Weitere Informationen zu den Anforderungen virtueller Netzwerke finden Sie unter [Überlegungen zum Entwurf virtueller Netzwerke und Konfigurationsoptionen für Azure AD Domain Services][network-considerations].

## <a name="migration-steps"></a>Schritte bei der Migration

Die Migration zum Resource Manager-Bereitstellungsmodell und virtuellen Netzwerk ist in fünf Hauptschritte unterteilt:

| Schritt    | Durchgeführt von  | Geschätzte Zeit  | Ausfallzeit  | Rollback/Wiederherstellung? |
|---------|--------------------|-----------------|-----------|-------------------|
| [Schritt 1: Aktualisieren und Ermitteln des neuen virtuellen Netzwerks](#update-and-verify-virtual-network-settings) | Azure-Portal | 15 Minuten | Keine Ausfallzeit erforderlich | – |
| [Schritt 2: Vorbereiten der verwalteten Azure AD DS-Domäne für die Migration](#prepare-the-managed-domain-for-migration) | PowerShell | 15 bis 30 Minuten (Durchschnittswert) | Die Ausfallzeit von Azure AD DS beginnt, nachdem dieser Befehl abgeschlossen wurde. | Rollback und Wiederherstellung sind verfügbar. |
| [Schritt 3: Verschieben der verwalteten Azure AD DS-Domäne in ein vorhandenes virtuelles Netzwerk](#migrate-the-managed-domain) | PowerShell | 1 bis 3 Stunden (Durchschnittswert) | Ein Domänencontroller ist verfügbar, sobald dieser Befehl abgeschlossen ist. Damit endet die Ausfallzeit. | Bei einem Ausfall stehen sowohl der Rollback (Self Service) als auch die Wiederherstellung zur Verfügung. |
| [Schritt 4: Durchführen eines Tests und Warten auf den Replikatdomänencontroller](#test-and-verify-connectivity-after-the-migration)| PowerShell und Azure-Portal | 1 Stunde oder mehr, je nach Testanzahl | Beide Domänencontroller sind verfügbar und sollten normal funktionieren. | N/V. Nachdem die Migration des ersten virtuellen Computers erfolgreich abgeschlossen wurde, ist keine Option für ein Rollback oder eine Wiederherstellung mehr verfügbar. |
| [Schritt 5: Optionale Konfigurationsschritte](#optional-post-migration-configuration-steps) | Azure-Portal und VMs | – | Keine Ausfallzeit erforderlich | – |

> [!IMPORTANT]
> Lesen Sie sich diesen Artikel zur Migration und den Leitfaden durch, bevor Sie mit dem Migrationsprozess beginnen, um weitere Ausfallzeiten zu vermeiden. Der Migrationsprozess wirkt sich eine Zeit lang auf die Verfügbarkeit der Azure AD DS-Domänencontroller aus. Benutzer, Dienste und Anwendungen können während des Migrationsvorgangs nicht für die verwaltete Domäne authentifiziert werden.

## <a name="update-and-verify-virtual-network-settings"></a>Aktualisieren und Überprüfen der Einstellungen des virtuellen Netzwerks

Führen Sie die folgenden vorbereitenden Prüfungen und Aktualisierungen durch, bevor Sie mit der Migration beginnen. Sie können diese Schritte jederzeit vor der Migration ausführen. Diese wirken sich nicht negativ auf den Betrieb der verwalteten Azure AD DS-Domäne aus.

1. Aktualisieren Sie Ihre lokale Azure PowerShell-Umgebung auf die neueste Version. Zum Durchführen der Migrationsschritte benötigen Sie mindestens Version *2.3.2*.

    Informationen zur Überprüfung und Aktualisierung finden Sie unter [Übersicht über Azure PowerShell][azure-powershell].

1. Erstellen Sie ein virtuelles Resource Manager-Netzwerk, oder wählen Sie ein vorhandenes aus.

    Stellen Sie sicher, dass durch die Netzwerkeinstellungen nicht die Ports blockiert werden, die für Azure AD DS erforderlich sind. Ports müssen im klassischen virtuellen Netzwerk und im virtuellen Resource Manager-Netzwerk geöffnet sein. Hierzu gehören auch Einstellungen für Routingtabellen (obwohl die Nutzung von Routingtabellen nicht zu empfehlen ist) und Netzwerksicherheitsgruppen.

    Informationen zu den erforderlichen Ports finden Sie unter [Netzwerksicherheitsgruppen und erforderliche Ports][network-ports]. Zur Reduzierung von Problemen mit der Netzwerkkommunikation empfehlen wir Ihnen, vorerst zu warten und eine Netzwerksicherheitsgruppe oder Routingtabelle erst auf das virtuelle Resource Manager-Netzwerk anzuwenden, nachdem die Migration erfolgreich abgeschlossen wurde.

    Notieren Sie sich die Angaben zu dieser Zielressourcengruppe, zum virtuellen Zielnetzwerk und zum zugehörigen Subnetz. Diese Ressourcennamen werden während des Migrationsvorgangs verwendet.

1. Überprüfen Sie im Azure-Portal die Integrität der verwalteten Azure AD DS-Domäne. Falls für die verwaltete Domäne Warnungen bestehen, sollten Sie sie beheben, bevor Sie den Migrationsprozess starten.
1. Optional: Wenn Sie planen, andere Ressourcen auf das Resource Manager-Bereitstellungsmodell und das virtuelle Netzwerk umzustellen, sollten Sie sich vergewissern, dass diese Ressourcen migriert werden können. Weitere Informationen finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager][migrate-iaas].

    > [!NOTE]
    > Führen Sie für das klassische virtuelle Netzwerk keine Konvertierung in ein virtuelles Resource Manager-Netzwerk durch. Wenn Sie dies doch tun, ist kein Rollback bzw. keine Wiederherstellung für die verwaltete Azure AD DS-Domäne mehr möglich.

## <a name="prepare-the-managed-domain-for-migration"></a>Vorbereiten der verwalteten Domäne für die Migration

Azure PowerShell wird verwendet, um die verwaltete Azure AD DS-Domäne für die Migration vorzubereiten. Diese Schritte umfassen das Erstellen einer Sicherung, das Anhalten der Synchronisierung und das Löschen des Clouddiensts, von dem Azure AD DS gehostet wird. Nach Abschluss dieses Schritts wird Azure AD DS eine Zeit lang in den Offlinezustand versetzt. Wenn der Vorbereitungsschritt nicht erfolgreich ist, können Sie einen [Rollback zum vorherigen Zustand ausführen](#roll-back).

Führen Sie die folgenden Schritte aus, um die verwaltete Azure AD DS-Domäne für die Migration vorzubereiten:

1. Installieren Sie das Skript `Migrate-Aaads` aus dem [PowerShell-Katalog][powershell-script]. Dieses PowerShell-Migrationsskript ist vom Azure AD-Entwicklerteam digital signiert.

    ```powershell
    Install-Script -Name Migrate-Aadds
    ```

1. Erstellen Sie eine Variable für die Anmeldeinformationen des Migrationsskripts, indem Sie das Cmdlet [Get-Credential][get-credential] verwenden.

    Für das von Ihnen angegebene Benutzerkonto werden auf Ihrem Azure AD-Mandanten Rechte vom Typ *Globaler Administrator* benötigt, um Azure AD DS zu aktivieren, und anschließend in Ihrem Azure-Abonnement Rechte vom Typ *Mitwirkender*, um die erforderlichen Azure AD DS-Ressourcen zu erstellen.

    Geben Sie bei entsprechender Aufforderung ein Benutzerkonto und Kennwort ein:

    ```powershell
    $creds = Get-Credential
    ```

1. Führen Sie nun das Cmdlet `Migrate-Aadds` mit dem Parameter *-Prepare* aus. Geben Sie *-ManagedDomainFqdn* für Ihre eigene verwaltete Azure AD DS-Domäne ein, z. B. *contoso.com*:

    ```powershell
    Migrate-Aadds `
        -Prepare -ManagedDomainFqdn contoso.com `
        -Credentials $creds
    ```

## <a name="migrate-the-managed-domain"></a>Migrieren der verwalteten Domäne

Nachdem die verwaltete Azure AD DS-Domäne vorbereitet und gesichert wurde, kann sie migriert werden. In diesem Schritt werden die Domänencontroller-VMs von Azure AD Domain Services basierend auf dem Resource Manager-Bereitstellungsmodell neu erstellt. Es kann ein bis drei Stunden dauern, bis dieser Schritt abgeschlossen ist.

Führen Sie das Cmdlet `Migrate-Aadds` mit dem Parameter *-Commit* aus. Geben Sie *-ManagedDomainFqdn* für Ihre eigene verwaltete Azure AD DS-Domäne ein, für die im vorherigen Schritt die Vorbereitung durchgeführt wurde, z. B. *contoso.com*:

Geben Sie die Zielressourcengruppe an, in der das virtuelle Netzwerk enthalten ist, zu dem Azure AD DS migriert werden soll, z. B. *myResourceGroup*. Geben Sie das virtuelle Zielnetzwerk, z. B. *myVnet*, und das Subnetz, z. B. *DomainServices*, an.

Nach Ausführung dieses Befehls ist kein Rollback mehr möglich:

```powershell
Migrate-Aadds `
    -Commit `
    -ManagedDomainFqdn contoso.com `
    -VirtualNetworkResourceGroupName myResourceGroup `
    -VirtualNetworkName myVnet `
    -VirtualSubnetName DomainServices `
    -Credentials $creds
```

Nach der Überprüfung des Skripts ist die verwaltete Domäne für die Migration vorbereitet. Geben Sie *J* ein, um den Migrationsprozess zu starten.

> [!IMPORTANT]
> Führen Sie während des Migrationsprozesses für das klassische virtuelle Netzwerk keine Konvertierung in ein virtuelles Resource Manager-Netzwerk durch. Wenn Sie das virtuelle Netzwerk konvertieren, ist für die verwaltete Azure AD DS-Domäne kein Rollback und keine Wiederherstellung mehr möglich, da das ursprüngliche virtuelle Netzwerk nicht mehr vorhanden ist.

Während des Migrationsprozesses wird von einer Statusanzeige alle zwei Minuten der aktuelle Status gemeldet. Dies ist in der folgenden Beispielausgabe dargestellt:

![Statusanzeige der Migration von Azure AD DS](media/migrate-from-classic-vnet/powershell-migration-status.png)

Der Migrationsprozess wird auch dann weiter ausgeführt, wenn Sie das PowerShell-Skript schließen. Im Azure-Portal wird für die verwaltete Domäne der Status *Migration wird ausgeführt* gemeldet.

Nachdem die Migration erfolgreich abgeschlossen wurde, können Sie die IP-Adresse Ihres ersten Domänencontrollers im Azure-Portal oder per Azure PowerShell anzeigen. Außerdem wird eine Schätzung angezeigt, wann der zweite Domänencontroller voraussichtlich verfügbar ist.

In dieser Phase können Sie optional andere vorhandene Ressourcen aus dem klassischen Bereitstellungsmodell und dem virtuellen Netzwerk verschieben. Alternativ können Sie die Ressourcen auch unter dem klassischen Bereitstellungsmodell belassen und die virtuellen Netzwerke per Peering miteinander verbinden, nachdem die Azure AD DS-Migration abgeschlossen ist.

## <a name="test-and-verify-connectivity-after-the-migration"></a>Testen und Überprüfen der Konnektivität nach der Migration

Es kann einige Zeit dauern, bis der zweite Domänencontroller erfolgreich bereitgestellt wurde und in der verwalteten Azure AD DS-Domäne zur Verfügung steht.

Beim Resource Manager-Bereitstellungsmodell werden die Netzwerkressourcen für die verwaltete Azure AD DS-Domäne im Azure-Portal oder in Azure PowerShell angezeigt. Weitere Informationen zur Beschaffenheit und zum Zweck dieser Netzwerkressourcen finden Sie unter [Von Azure AD DS verwendete Netzwerkressourcen][network-resources].

Führen Sie die folgenden Konfigurationsschritte aus, um die Netzwerkkonnektivität mit VMs herzustellen, wenn mindestens ein Domänencontroller verfügbar ist.

* **Aktualisieren von DNS-Servereinstellungen** Aktualisieren Sie die DNS-Einstellungen mit den IP-Adressen der neuen Domänencontroller, damit andere Ressourcen im virtuellen Resource Manager-Netzwerk die verwaltete Azure AD DS-Domäne auflösen und nutzen können. Diese Einstellungen können im Azure-Portal automatisch für Sie konfiguriert werden. Weitere Informationen zum Konfigurieren des virtuellen Resource Manager-Netzwerks finden Sie unter [Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk][update-dns].
* **Neustarten von in die Domäne eingebundenen VMs**: Wenn sich die IP-Adressen des DNS-Servers für die Azure AD DS-Domänencontroller ändern, sollten Sie alle in die Domäne eingebundenen VMs neu starten, damit diese dann die neuen DNS-Servereinstellungen verwenden. Falls Anwendungen oder VMs über manuell konfigurierte DNS-Einstellungen verfügen, müssen Sie diese manuell mit den neuen DNS-Server-IP-Adressen der Domänencontroller aktualisieren, die im Azure-Portal angezeigt werden.

Testen Sie jetzt die Verbindung und Namensauflösung für das virtuelle Netzwerk. Führen Sie auf einer VM, die mit dem virtuellen Resource Manager-Netzwerk verbunden oder per Peering damit verknüpft ist, die folgenden Tests zur Netzwerkkommunikation durch:

1. Überprüfen Sie, ob Sie die IP-Adresse von einem der Domänencontroller pingen können, z. B. `ping 10.1.0.4`.
    * Die IP-Adressen der Domänencontroller werden im Azure-Portal auf der Seite **Eigenschaften** für die verwaltete Azure AD DS-Domäne angezeigt.
1. Überprüfen Sie die Namensauflösung der verwalteten Domäne, z. B. `nslookup contoso.com`.
    * Geben Sie den DNS-Namen für Ihre eigene verwaltete Azure AD DS-Domäne an, um sicherzustellen, dass die DNS-Einstellungen stimmen und die Auflösung richtig erfolgt.

Der zweite Domänencontroller sollte ein bis zwei Stunden nach Abschluss des Cmdlets für die Migration verfügbar sein. Sehen Sie sich im Azure-Portal auf der Seite **Eigenschaften** die Angaben für die verwaltete Azure AD DS-Domäne an, um zu überprüfen, ob der zweite Domänencontroller verfügbar ist. Wenn zwei IP-Adressen angezeigt werden, ist der zweite Domänencontroller bereit.

## <a name="optional-post-migration-configuration-steps"></a>Optionale Konfigurationsschritte nach der Migration

Nach dem erfolgreichen Abschluss des Migrationsprozesses können Sie optionale Konfigurationsschritte ausführen, z. B. das Aktivieren von Überwachungsprotokollen oder E-Mail-Benachrichtigungen oder das Aktualisieren der differenzierten Kennwortrichtlinie.

#### <a name="subscribe-to-audit-logs-using-azure-monitor"></a>Abonnieren von Überwachungsprotokollen mit Azure Monitor

Von Azure AD DS werden Überwachungsprotokolle verfügbar gemacht, um als Hilfe bei der Problembehandlung und Anzeige von Ereignissen auf den Domänencontrollern zu dienen. Weitere Informationen finden Sie im Artikel zum [Aktivieren und Verwenden von Überwachungsprotokollen][security-audits].

Sie können Vorlagen verwenden, um wichtige Informationen zu überwachen, die in den Protokollen verfügbar gemacht werden. Mit der Arbeitsmappenvorlage für Überwachungsprotokolle können mögliche Kontosperrungen in der verwalteten Azure AD DS-Domäne überwacht werden.

#### <a name="configure-azure-ad-domain-services-email-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen für Azure AD Domain Services

Aktualisieren Sie die Einstellungen für E-Mail-Benachrichtigungen im Azure-Portal, um benachrichtigt zu werden, wenn in der verwalteten Azure AD DS-Domäne ein Problem erkannt wird. Weitere Informationen finden Sie unter [Konfigurieren von Benachrichtigungseinstellungen][notifications].

#### <a name="update-fine-grained-password-policy"></a>Aktualisieren der differenzierten Kennwortrichtlinie

Bei Bedarf können Sie die differenzierte Kennwortrichtlinie aktualisieren, damit sie weniger restriktiv als in der Standardkonfiguration ist. Sie können anhand der Überwachungsprotokolle ermitteln, ob eine weniger restriktive Einstellung sinnvoll ist, und die Richtlinie dann je nach Bedarf konfigurieren. Führen Sie die folgenden allgemeinen Schritte aus, um die Richtlinieneinstellungen für Konten zu überprüfen und zu aktualisieren, die nach der Migration wiederholt gesperrt werden:

1. [Konfigurieren Sie die Kennwortrichtlinie][password-policy] so, dass für die verwaltete AD DS-Domäne weniger Einschränkungen gelten, und sehen Sie sich die Ereignisse in den Überwachungsprotokollen an.
1. Wenn für Dienstkonten abgelaufene Kennwörter gemäß Identifizierung in den Überwachungsprotokollen verwendet werden, sollten Sie diese Konten aktualisieren und das richtige Kennwort angeben.
1. Wenn die VM für den Zugriff über das Internet verfügbar gemacht wird, sollten Sie überprüfen, ob für generische Kontonamen wie *Administrator*, *Benutzer* oder *Gast* eine hohe Zahl von Anmeldeversuchen zu verzeichnen ist. Aktualisieren Sie diese VMs nach Möglichkeit so, dass weniger Konten mit generischen Namen verwendet werden.
1. Verwenden Sie auf der VM eine Netzwerkablaufverfolgung, um die Quelle der Angriffe zu ermitteln, und blockieren Sie diese IP-Adressen für Anmeldeversuche.
1. Falls es nur zu geringfügigen Problemen mit Sperrungen kommt, sollten Sie die differenzierte Kennwortrichtlinie so aktualisieren, dass sie so restriktiv wie nötig ist.

#### <a name="creating-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Für Azure AD DS wird eine Netzwerksicherheitsgruppe benötigt, um die Ports zu schützen, die für die verwaltete Domäne erforderlich sind, und den gesamten anderen Datenverkehr in eingehender Richtung zu blockieren. Diese Netzwerksicherheitsgruppe fungiert als zusätzliche Schutzschicht zum Sperren des Zugriffs auf die verwaltete Domäne und wird nicht automatisch erstellt. Sehen Sie sich die folgenden Schritte an, mit denen die Netzwerksicherheitsgruppe erstellt und die erforderlichen Ports geöffnet werden:

1. Wählen Sie im Azure-Portal Ihre Azure AD DS-Ressource aus. Die Übersichtsseite enthält eine Schaltfläche, mit der Sie eine Netzwerksicherheitsgruppe erstellen können, falls Azure AD Domain Services noch keine zugeordnet ist.
1. Fügen Sie der Netzwerksicherheitsgruppe bei Verwendung von Secure LDAP eine Regel hinzu, um Datenverkehr in eingehender Richtung für *TCP*-Port *636* zuzulassen. Weitere Informationen finden Sie unter [Konfigurieren von Secure LDAP][secure-ldap].

## <a name="roll-back-and-restore-from-migration"></a>Rollback und Wiederherstellung nach der Migration

### <a name="roll-back"></a>Rollback

Falls beim Ausführen des PowerShell-Cmdlets zur Vorbereitung der Migration in Schritt 2 oder für die eigentliche Migration in Schritt 3 ein Fehler auftritt, kann für die verwaltete Azure AD DS-Domäne ein Rollback zur ursprünglichen Konfiguration ausgeführt werden. Für dieses Rollback ist das ursprüngliche klassische virtuelle Netzwerk erforderlich. Beachten Sie, dass sich die IP-Adressen nach dem Rollback weiterhin ändern können.

Führen Sie das Cmdlet `Migrate-Aadds` mit dem Parameter *-Abort* aus. Geben Sie *-ManagedDomainFqdn* für Ihre eigene verwaltete Azure AD DS-Domäne ein, für die in einem der vorherigen Schritte die Vorbereitung durchgeführt wurde, z. B. *contoso.com*:

```powershell
Migrate-Aadds `
    -Abort `
    -ManagedDomainFqdn contoso.com `
    -Credentials $creds
```

### <a name="restore"></a>Restore

Die letzte Möglichkeit besteht darin, Azure AD Domain Services aus der letzten verfügbaren Sicherung wiederherzustellen. Im ersten Schritt der Migration wird eine Sicherung erstellt, um sicherzustellen, dass eine aktuelle Sicherung verfügbar ist. Diese Sicherung wird 30 Tage lang gespeichert.

[Erstellen Sie über das Azure-Portal ein Supportticket][azure-support], um die verwaltete Azure AD DS-Domäne aus der Sicherung wiederherzustellen. Geben Sie Ihre Verzeichnis-ID, den Domänennamen und den Grund für die Wiederherstellung an. Es kann mehrere Tage dauern, bis der Support- und Wiederherstellungsprozess abgeschlossen ist.

## <a name="troubleshooting"></a>Problembehandlung

Informieren Sie sich über einige der folgenden häufigen Problembehandlungsbereiche, falls bei Ihnen nach der Migration zum Resource Manager-Bereitstellungsmodell Probleme auftreten:

* [Behandeln von Problemen beim Einbinden in eine Domäne][troubleshoot-domain-join]
* [Troubleshooting für Probleme mit gesperrten Konten][troubleshoot-account-lockout]
* [Troubleshooting für Probleme bei der Kontoanmeldung][troubleshoot-sign-in]
* [Troubleshooting für Probleme mit der Secure LDAP-Konnektivität][tshoot-ldaps]

## <a name="next-steps"></a>Nächste Schritte

Nachdem für Ihre verwaltete Azure AD DS-Domäne die Migration zum Resource Manager-Bereitstellungsmodell durchgeführt wurde, können Sie [eine Windows-VM erstellen und in die Domäne einbinden][join-windows] und anschließend [Verwaltungstools installieren][tutorial-create-management-vm].

<!-- INTERNAL LINKS -->
[azure-bastion]: ../bastion/bastion-overview.md
[network-considerations]: network-considerations.md
[azure-powershell]: /powershell/azure/overview
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Set-AzContext]: /powershell/module/az.accounts/set-azcontext
[Get-AzResource]: /powershell/module/az.resources/get-azresource
[Set-AzResource]: /powershell/module/az.resources/set-azresource
[network-resources]: network-considerations.md#network-resources-used-by-azure-ad-ds
[update-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[security-audits]: security-audit-events.md
[notifications]: notifications.md
[password-policy]: password-policy.md
[secure-ldap]: tutorial-configure-ldaps.md
[migrate-iaas]: ../virtual-machines/windows/migration-classic-resource-manager-overview.md
[join-windows]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[troubleshoot-domain-join]: troubleshoot-domain-join.md
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[troubleshoot-sign-in]: troubleshoot-sign-in.md
[tshoot-ldaps]: tshoot-ldaps.md
[get-credential]: /powershell/module/microsoft.powershell.security/get-credential

<!-- EXTERNAL LINKS -->
[powershell-script]: https://www.powershellgallery.com/packages/Migrate-Aadds/1.0
