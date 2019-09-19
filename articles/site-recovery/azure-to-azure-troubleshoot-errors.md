---
title: Azure Site Recovery-Problembehandlung für Azure-zu-Azure-Replikationsfehler | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum Beheben von Fehlern beim Replizieren von virtuellen Azure-Computern für die Notfallwiederherstellung.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: asgang
ms.openlocfilehash: baf7a21d04e8f9bcf86c67abde302a558dfba01c
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910396"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-errors"></a>Problembehandlung für Azure-zu-Azure-VM-Replikationsfehler

In diesem Artikel wird beschrieben, wie Sie häufig auftretende Fehler in Azure Site Recovery während der Replikation und Wiederherstellung von virtuellen Azure-Computern (VMs) aus einer Region in einer anderen Region behandeln können. Weitere Informationen zu unterstützten Konfigurationen finden Sie unter [Azure Site Recovery support matrix for replicating from Azure to Azure](site-recovery-support-matrix-azure-to-azure.md) (Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Probleme mit dem Azure-Ressourcenkontingent (Fehlercode 150097)

Vergewissern Sie sich, dass es für Ihr Abonnement möglich ist, Azure-VMs in der Zielregion zu erstellen, die Sie als Region für die Notfallwiederherstellung nutzen möchten. Stellen Sie außerdem sicher, dass Ihr Abonnement über ein ausreichendes Kontingent zum Erstellen von VMs der erforderlichen Größen verfügt. Standardmäßig wählt Site Recovery eine Ziel-VM-Größe aus, die der Größe der Quell-VM entspricht. Falls die passende Größe nicht verfügbar ist, wählt Site Recovery automatisch die verfügbare Größe aus, die dieser am nächsten kommt.

Wenn keine Größe vorhanden ist, die die Konfiguration der Quell-VM unterstützt, wird die folgende Meldung angezeigt:

> „Die Replikation konnte für den virtuellen Computer *VmName* nicht aktiviert werden.“

### <a name="possible-causes"></a>Mögliche Ursachen

- Für Ihre Abonnement-ID ist die Erstellung von VMs am Standort der Zielregion nicht aktiviert.
- Für Ihre Abonnement-ID ist die Erstellung von bestimmten VM-Größen am Standort der Zielregion nicht aktiviert, oder es ist kein ausreichendes Kontingent vorhanden.
- Für die Abonnement-ID wird am Standort der Zielregion keine geeignete Größe der Ziel-VM gefunden, die mit der Anzahl (2) der Netzwerkschnittstellenkarten (NIC) der Quell-VM übereinstimmt.

### <a name="fix-the-problem"></a>Beheben des Problems

Wenden Sie sich an den [Azure-Abrechnungssupport](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request), um Ihr Abonnement für die Erstellung von VMs der erforderlichen Größen am Zielstandort aktivieren zu lassen. Wiederholen Sie dann den fehlerhaften Vorgang.

Wenn für den Zielstandort eine Kapazitätsbeschränkung gilt, deaktivieren Sie die Replikation an diesen Standort. Dann aktivieren Sie die Replikation an einen anderen Standort, an dem für Ihr Abonnement ein ausreichendes Kontingent zum Erstellen von VMs der erforderlichen Größen zur Verfügung steht.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrauenswürdige Stammzertifikate (Fehlercode: 151066)

Wenn auf der VM nicht alle aktuellen vertrauenswürdigen Stammzertifikate vorhanden sind, ist der Site Recovery-Auftrag „Replikation aktivieren“ möglicherweise nicht erfolgreich. Ohne diese Zertifikate tritt für die Authentifizierung und Autorisierung von Site Recovery-Dienstaufrufen über die VM ein Fehler auf. 

Wenn der Auftrag „Replikation aktivieren“ fehlschlägt, wird die folgende Meldung angezeigt:

> „Fehler bei der Site Recovery-Konfiguration.“

### <a name="possible-cause"></a>Mögliche Ursache

Die für die Autorisierung und Authentifizierung erforderlichen vertrauenswürdigen Stammzertifikate sind auf dem virtuellen Computer nicht vorhanden.

### <a name="fix-the-problem"></a>Beheben des Problems

#### <a name="windows"></a>Windows

Bei einer VM mit Windows-Betriebssystem installieren Sie die aktuellen Windows-Updates auf der VM, damit alle vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind. Führen Sie den üblichen Prozess Ihrer Organisation für die Verwaltung von Windows-Updates oder Zertifikatupdates durch, um die aktuellen Stammzertifikate und die aktualisierte Zertifikatssperrliste auf den VMs abzurufen.

Wenn Sie sich in einer nicht verbundenen Umgebung befinden, können Sie den Standardprozess für Windows-Updates in Ihrer Organisation durchführen, um die Zertifikate abzurufen. Falls die erforderlichen Zertifikate auf der VM nicht vorhanden sind, schlagen die Aufrufe des Site Recovery-Diensts aus Sicherheitsgründen fehl.

Navigieren Sie mit einem Browser auf Ihrer VM zu „login.microsoftonline.com“, um zu überprüfen, ob das Problem behoben wurde.

Weitere Informationen finden Sie unter [Konfigurieren vertrauenswürdiger Stämme und unzulässiger Zertifikate](https://technet.microsoft.com/library/dn265983.aspx).

#### <a name="linux"></a>Linux

Befolgen Sie die Anweisungen, die vom Anbieter Ihrer Linux-Betriebssystemversion bereitgestellt werden, um die aktuellen vertrauenswürdigen Stammzertifikate und die aktuelle Zertifikatssperrliste auf der VM abzurufen.

Führen Sie die folgenden Schritte aus, da SuSE Linux zum Verwalten einer Zertifikatliste symbolische Verknüpfungen (oder *symlinks*) verwendet:

1. Melden Sie sich als Stammbenutzer an.

1. Führen Sie den folgenden Befehl aus, um das Verzeichnis zu wechseln:

    **# cd /etc/ssl/certs**

1. Überprüfen Sie, ob das Symantec-Zertifikat der Stammzertifizierungsstelle vorhanden ist:

    **# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Wurde das Symantec-Zertifikat der Stammzertifizierungsstelle nicht gefunden, führen Sie den folgenden Befehl aus, um die Datei herunterzuladen. Überprüfen Sie, ob Fehler aufgetreten sind, und führen Sie die empfohlenen Maßnahmen für Netzwerkfehler aus.

    **# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Überprüfen Sie, ob das Baltimore-Zertifikat der Stammzertifizierungsstelle vorhanden ist:

    **# ls Baltimore_CyberTrust_Root.pem**

1. Wurde das Baltimore-Zertifikat der Stammzertifizierungsstelle nicht gefunden, führen Sie den folgenden Befehl aus, um das Zertifikat herunterzuladen:

    **# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem**

1. Überprüfen Sie, ob das Zertifikat „DigiCert_Global_Root_CA“ vorhanden ist:

    **# ls DigiCert_Global_Root_CA.pem**

1. Wurde „DigiCert_Global_Root_CA“ nicht gefunden, führen Sie die folgenden Befehle aus, um das Zertifikat herunterzuladen:

    **# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt**

    **# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem**

1. Führen Sie das Skript für das erneute Ausführen von Hashvorgängen aus, um die Zertifikatantragstellerhashes für die neu heruntergeladenen Zertifikate zu aktualisieren:

    **# c_rehash**

1. Führen Sie die folgenden Befehle aus, um zu überprüfen, ob die Antragstellerhashes als Symlinks für die Zertifikate erstellt wurden:

    - Befehl:

        **# ls -l | grep Baltimore**

    - Ausgabe:

        `lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem`

        `-rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem`

    - Befehl:

        **# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5**

    - Ausgabe:

        `-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

        `lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

    - Befehl:

        **# ls -l | grep DigiCert_Global_Root**

    - Ausgabe:

        `lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem`

        `-rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem`

1. Erstellen Sie eine Kopie der Datei „VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem“ mit dem Dateinamen „b204d74a.0“:

    **# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0**

1. Erstellen Sie eine Kopie der Datei „Baltimore_CyberTrust_Root.pem“ mit dem Dateinamen „653b494a.0“:

    **# cp Baltimore_CyberTrust_Root.pem 653b494a.0**

1. Erstellen Sie eine Kopie der Datei „DigiCert_Global_Root_CA.pem“ mit dem Dateinamen „3513523f.0“:

    **# cp DigiCert_Global_Root_CA.pem 3513523f.0**

1. Überprüfen Sie, ob die Dateien vorhanden sind:

    - Befehl:

        **# ls -l 653b494a.0 b204d74a.0 3513523f.0**

    - Output

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0`

        `-rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0`

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0`

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Ausgehende Konnektivität für Site Recovery-URLs oder IP-Bereiche (Fehlercode 151037 oder 151072)

Damit die Site Recovery-Replikation funktioniert, ist ausgehende Konnektivität von der VM zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihr virtueller Computer hinter einer Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Problem 1: Fehler beim Registrieren des virtuellen Azure-Computers bei Site Recovery (Fehlercode 151195)

#### <a name="possible-cause"></a>Mögliche Ursache 

Die Verbindung mit Site Recovery-Endpunkten kann aufgrund eines Fehlers bei der DNS-Auflösung nicht hergestellt werden.

Dieses Problem tritt am häufigsten während des erneuten Schützens auf, wenn Sie für den virtuellen Computer ein Failover ausgeführt haben, der DNS-Server in der Notfallwiederherstellungsregion jedoch nicht erreichbar ist.

#### <a name="fix-the-problem"></a>Beheben des Problems

Stellen Sie bei Verwendung eines benutzerdefinierten DNS sicher, dass der DNS-Server in der Notfallwiederherstellungsregion erreichbar ist. Um herauszufinden, ob Sie über ein benutzerdefiniertes DNS verfügen, navigieren Sie auf der VM zum *Notfallwiederherstellungsnetzwerk* > **DNS-Server**.

![Liste der benutzerdefinierten DNS-Server](./media/azure-to-azure-troubleshoot-errors/custom_dns.PNG)

Versuchen Sie, über den virtuellen Computer auf den DNS-Server zuzugreifen. Wenn auf den Server nicht zugegriffen werden kann, können Sie entweder ein Failover für den DNS-Server ausführen oder eine Verbindung zwischen dem Notfallwiederherstellungsnetzwerk und dem DNS herstellen.

### <a name="issue-2-site-recovery-configuration-failed-error-code-151196"></a>Problem 2: Fehler bei der Site Recovery-Konfiguration (Fehlercode 151196)

#### <a name="possible-cause"></a>Mögliche Ursache

Die Verbindung mit IP4-Endpunkten für die Authentifizierung und Identität von Office 365 kann nicht hergestellt werden.

#### <a name="fix-the-problem"></a>Beheben des Problems

Site Recovery benötigt für die Authentifizierung Zugriff auf Office 365-IP-Adressbereiche.
Wenn Sie zum Steuern der ausgehenden Netzwerkkonnektivität auf dem virtuellen Computer Azure-NSG-Regeln oder einen Firewallproxy verwenden, müssen Sie die Kommunikation mit Office 365-IP-Adressbereichen zulassen. Erstellen Sie eine NSG-Regel basierend auf einem [Azure Active Directory-Diensttag (Azure AD)](../virtual-network/security-overview.md#service-tags) für den Zugriff auf alle IP-Adressen für Azure AD. Wenn Azure AD später neue Adressen hinzugefügt werden, müssen Sie neue NSG-Regeln erstellen.

> [!NOTE]
> Wenn sich VMs hinter einem internen *Standard*-Lastenausgleich befinden, hat der Lastenausgleich standardmäßig keinen Zugriff auf Office 365-IP-Adressbereiche (d.h. „login.microsoftonline.com“). Ändern Sie entweder den Typ des internen Lastenausgleichs in *Basic*, oder erstellen Sie einen ausgehenden Zugriff, wie es im Artikel [Konfigurieren von Lastenausgleichs- und Ausgangsregeln](https://aka.ms/lboutboundrulescli) beschrieben ist.

### <a name="issue-3-site-recovery-configuration-failed-error-code-151197"></a>Problem 3: Fehler bei der Site Recovery-Konfiguration (Fehlercode 151197)

#### <a name="possible-cause"></a>Mögliche Ursache

Die Verbindung mit Site Recovery-Dienstendpunkten kann nicht hergestellt werden.

#### <a name="fix-the-problem"></a>Beheben des Problems

Site Recovery benötigt abhängig von der Region Zugriff auf [Site Recovery-IP-Adressbereiche](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). Stellen Sie sicher, dass über den virtuellen Computer auf die erforderlichen IP-Adressbereiche zugegriffen werden kann.

### <a name="issue-4-azure-to-azure-replication-failed-when-the-network-traffic-goes-through-an-on-premises-proxy-server-error-code-151072"></a>Problem 4: Fehler bei der Azure-zu-Azure-Replikation, wenn der Netzwerkdatenverkehr einen lokalen Proxyserver durchläuft (Fehlercode 151072)

#### <a name="possible-cause"></a>Mögliche Ursache

Die benutzerdefinierten Proxyeinstellungen sind ungültig, und der Agent des Site Recovery-Mobilitätsdiensts hat die Proxyeinstellungen von Internet Explorer nicht automatisch erkannt.

#### <a name="fix-the-problem"></a>Beheben des Problems

Der Mobilitätsdienst-Agent erkennt die Proxyeinstellungen von Internet Explorer unter Windows und von „/etc/environment“ unter Linux.

Wenn der Proxy nur für den Mobilitätsdienst festgelegt werden soll, können Sie die Proxydetails in der Datei „ProxyInfo.conf“an folgenden Speicherorten angeben:

- **Linux**: /usr/local/InMage/config/
- **Windows:** C:\ProgramData\Microsoft Azure Site Recovery\Config

Geben Sie in „ProxyInfo.conf“ die Proxyeinstellungen im folgenden Initialisierungsdateiformat an:

> [*proxy*]

> Address= *http://1.2.3.4*

> Port=*567*


> [!NOTE]
> Der Agent des Site Recovery-Mobilitätsdiensts unterstützt nur *nicht authentifizierte Proxys*.

### <a name="more-information"></a>Weitere Informationen

Um [erforderliche URLs](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) oder [erforderliche IP-Adressbereiche](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) anzugeben, befolgen Sie die Anweisungen unter [Netzwerkkonzepte für die Replikation zwischen Azure-Standorten](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Datenträger wurde auf dem Computer nicht gefunden (Fehlercode 150039)

Es muss ein neuer Datenträger initialisiert werden, der an die VM angefügt ist. Wenn der Datenträger nicht gefunden wurde, wird die folgende Meldung angezeigt:

> „Der Azure-Datenträger *DiskName* *DiskURI* mit der logischen Gerätenummer *LUN* *LUNValue* wurde keinem entsprechenden Datenträger zugeordnet, der vom virtuellen Computer gemeldet wird und denselben LUN-Wert aufweist.

### <a name="possible-causes"></a>Mögliche Ursachen

- Es wurde ein neuer Datenträger an den virtuellen Computer angefügt, aber nicht initialisiert.
- Der Datenträger innerhalb des virtuellen Computers meldet nicht den richtigen LUN-Wert (logische Gerätenummer), mit dem der Datenträger an den virtuellen Computer angefügt wurde.

### <a name="fix-the-problem"></a>Beheben des Problems

Stellen Sie sicher, dass die Datenträger initialisiert wurden, und wiederholen Sie den Vorgang.

- **Windows:** [Anfügen und Initialisieren eines neuen Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).

- **Linux:** [Initialisieren eines neues Datenträgers unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Wenden Sie sich an den Support, wenn das Problem weiterhin besteht.

## <a name="one-or-more-disks-are-available-for-protection-error-code-153039"></a>Mindestens ein Datenträger ist für Schutz verfügbar (Fehlercode 153039)

### <a name="possible-causes"></a>Mögliche Ursachen

- Mindestens ein Datenträger wurde dem virtuellen Computer vor Kurzem nach dem Schutz hinzugefügt.
- Mindestens ein Datenträger wurde nach dem Schutz des virtuellen Computers initialisiert.

### <a name="fix-the-problem"></a>Beheben des Problems

Damit der Replikationsstatus des virtuellen Computers wieder fehlerfrei ist, können Sie entweder die Datenträger schützen oder die Warnung ignorieren.

#### <a name="to-protect-the-disks"></a>So schützen Sie die Datenträger

1. Wechseln Sie zu **Replizierte Elemente** > *Name des virtuellen Computers* > **Datenträger**.
1. Wählen Sie den ungeschützten Datenträger und dann **Replikation aktivieren** aus:

    ![Aktivieren der Replikation auf VM-Datenträgern](./media/azure-to-azure-troubleshoot-errors/add-disk.png)

#### <a name="to-dismiss-the-warning"></a>So ignorieren Sie die Warnung

1. Wechseln Sie zu **Replizierte Elemente** > *Name des virtuellen Computers*.
1. Wählen Sie die Warnung im Abschnitt **Übersicht** aus, und klicken Sie dann auf **OK**.

    ![Ignorieren der Warnung zu neuem Datenträger](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)

## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information-error-code-150225"></a>Das Entfernen des virtuellen Computers aus dem Tresor wurde mit Informationen abgeschlossen (Fehlercode 150225)

Beim Schützen des virtuellen Computers erstellt Site Recovery einige Links für den virtuellen Quellcomputer. Wenn Sie den Schutz entfernen oder die Replikation deaktivieren, entfernt Site Recovery diese Links im Rahmen des Bereinigungsauftrags. Wenn für den virtuellen Computer eine Ressourcensperre festgelegt ist, wird der Bereinigungsauftrag mit Informationen abgeschlossen. Die Informationen geben an, dass der virtuelle Computer aus dem Recovery Services-Tresor entfernt wurde, dass aber einige der veralteten Links auf dem Quellcomputer nicht bereinigt werden konnten.

Sie können diese Warnung ignorieren, wenn Sie diesen virtuellen Computer nicht wieder schützen möchten. Wenn Sie diesen virtuellen Computer jedoch später schützen müssen, führen Sie die Schritte unter „Beheben des Problems“ aus, um die Links zu bereinigen.

> [!WARNING]
> Wenn Sie keine Bereinigung ausführen, wirkt sich das wie folgt aus:
>
> - Wenn Sie die Replikation mithilfe des Recovery Services-Tresors aktivieren, wird der virtuelle Computer nicht aufgelistet.
> - Bei dem Versuch, den virtuellen Computer mithilfe von **Virtueller Computer** > **Einstellungen** > **Notfallwiederherstellung** zu schützen, tritt ein Fehler beim Vorgang mit folgender Meldung auf: „Die Replikation kann aufgrund der vorhandenen veralteten Ressourcenlinks für die VM nicht aktiviert werden.“

### <a name="fix-the-problem"></a>Beheben des Problems

> [!NOTE]
> Site Recovery löscht oder beeinträchtigt nicht den virtuellen Quellcomputer, während Sie diese Schritte ausführen.

1. Entfernen Sie die Sperre von der VM oder VM-Ressourcengruppe. In der folgenden Abbildung muss beispielsweise die Ressourcensperre für den virtuellen Computer mit dem Namen „MoveDemo“ gelöscht werden:

    ![Entfernen der Sperre für den virtuellen Computer](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Laden Sie das Skript zum [Entfernen einer veralteten Site Recovery-Konfiguration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) herunter.
1. Führen Sie das Skript mit dem Namen „Cleanup-stale-asr-config-Azure-VM.ps1“ aus. Geben Sie Abonnement-ID, VM-Ressourcengruppe und Name der VM als Parameter an.
1. Wenn Sie nach Azure-Anmeldeinformationen gefragt werden, geben Sie diese an. Überprüfen Sie dann, ob das Skript ohne Fehler ausgeführt wird.

## <a name="replication-cant-be-enabled-because-of-stale-resource-links-on-the-vm-error-code-150226"></a>Die Replikation kann aufgrund veralteter Ressourcenlinks für die VM nicht aktiviert werden (Fehlercode 150226)

### <a name="possible-cause"></a>Mögliche Ursache

Auf dem virtuellen Computer ist eine veraltete Konfiguration von einem früheren Site Recovery-Schutz vorhanden.

Eine veraltete Konfiguration kann auf einem virtuellen Azure-Computer vorhanden sein, wenn Sie die Replikation für den virtuellen Azure-Computer mithilfe von Site Recovery aktiviert haben und dann wie folgt vorgegangen sind:

- Sie haben die Replikation deaktiviert, aber für die Quell-VM war eine Ressourcensperre vorhanden.
- Sie haben den Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.
- Sie haben die Ressourcengruppe gelöscht, die den Site Recovery-Tresor enthält, ohne die Replikation auf der VM explizit zu deaktivieren.

### <a name="fix-the-problem"></a>Beheben des Problems

> [!NOTE]
> Site Recovery löscht oder beeinträchtigt nicht den virtuellen Quellcomputer, während Sie diese Schritte ausführen.

1. Entfernen Sie die Sperre von der VM oder VM-Ressourcengruppe. In der folgenden Abbildung muss beispielsweise die Ressourcensperre für den virtuellen Computer mit dem Namen „MoveDemo“ gelöscht werden:

    ![Entfernen der Sperre vom virtuellen Computer](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Laden Sie das Skript zum [Entfernen einer veralteten Site Recovery-Konfiguration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) herunter.
1. Führen Sie das Skript mit dem Namen „Cleanup-stale-asr-config-Azure-VM.ps1“ aus. Geben Sie Abonnement-ID, VM-Ressourcengruppe und Name der VM als Parameter an.
1. Wenn Sie nach Azure-Anmeldeinformationen gefragt werden, geben Sie diese an. Überprüfen Sie dann, ob das Skript ohne Fehler ausgeführt wird.

## <a name="unable-to-see-the-azure-vm-or-resource-group-for-the-selection-in-the-enable-replication-job"></a>Die Azure-VM oder Ressourcengruppe wird nicht zur Auswahl im Auftrag „Replikation aktivieren“ angezeigt

### <a name="cause-1-the-resource-group-and-source-virtual-machine-are-in-different-locations"></a>Ursache 1: Die Ressourcengruppe und der virtuelle Quellcomputer befinden sich jeweils an einem anderen Standort.

Site Recovery erfordert derzeit, dass sich die Quellressourcengruppe für die Region und der virtuelle Computer am selben Standort befinden müssen. Wenn das nicht der Fall ist, können Sie den virtuellen Computer oder die Ressourcengruppe nicht finden, wenn Sie versuchen, einen Schutz anzuwenden.

Als Problemumgehung können Sie die Replikation über den virtuellen Computer statt aus dem Recovery Services-Tresor aktivieren. Navigieren Sie zu **Quell-VM** > **Eigenschaften** > **Notfallwiederherstellung**, und aktivieren Sie die Replikation.

### <a name="cause-2-the-resource-group-is-not-part-of-the-selected-subscription"></a>Ursache 2: Die Ressourcengruppe ist nicht Teil des ausgewählten Abonnements.

Möglicherweise können Sie die Ressourcengruppe zum Zeitpunkt des Schutzes nicht finden, wenn sie nicht Teil des ausgewählten Abonnements ist. Stellen Sie sicher, dass die Ressourcengruppe zum verwendeten Abonnement gehört.

### <a name="cause-3-stale-configuration"></a>Ursache 3: Veraltete Konfiguration

Möglicherweise wird der virtuelle Computer, für den Sie die Replikation aktivieren möchten, nicht angezeigt, wenn eine veraltete Site Recovery-Konfiguration auf dem virtuellen Azure-Computer verblieben ist. Dieser Fall kann eintreten, wenn Sie die Replikation für den virtuellen Azure-Computer mithilfe von Site Recovery aktiviert haben und dann wie folgt vorgegangen sind:

- Sie haben den Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.
- Sie haben die Ressourcengruppe gelöscht, die den Site Recovery-Tresor enthält, ohne die Replikation auf der VM explizit zu deaktivieren.
- Sie haben die Replikation deaktiviert, aber für die Quell-VM war eine Ressourcensperre vorhanden.

### <a name="fix-the-problem"></a>Beheben des Problems

> [!NOTE]
> Aktualisieren Sie unbedingt das Modul „AzureRM.Resources“, bevor Sie das in diesem Abschnitt genannte Skript verwenden.  Site Recovery löscht oder beeinträchtigt nicht den virtuellen Quellcomputer, während Sie diese Schritte ausführen.

1. Entfernen Sie ggf. die Sperre von der VM oder VM-Ressourcengruppe. In der folgenden Abbildung muss beispielsweise die Ressourcensperre für den virtuellen Computer mit dem Namen „MoveDemo“ gelöscht werden:

    ![Entfernen der Sperre vom virtuellen Computer](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Laden Sie das Skript zum [Entfernen einer veralteten Site Recovery-Konfiguration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) herunter.
1. Führen Sie das Skript mit dem Namen „Cleanup-stale-asr-config-Azure-VM.ps1“ aus. Geben Sie Abonnement-ID, VM-Ressourcengruppe und Name der VM als Parameter an.
1. Wenn Sie nach Azure-Anmeldeinformationen gefragt werden, geben Sie diese an. Überprüfen Sie dann, ob das Skript ohne Fehler ausgeführt wird.

## <a name="unable-to-select-a-virtual-machine-for-protection"></a>Auswählen eines virtuellen Computers zum Schutz nicht möglich

### <a name="cause-1-the-virtual-machine-has-an-extension-installed-in-a-failed-or-unresponsive-state"></a>Ursache 1: Auf dem virtuellen Computer ist eine fehlerhafte oder nicht reagierende Erweiterung installiert.

Wechseln Sie zu **Virtuelle Computer** > **Einstellung** > **Erweiterungen**, und überprüfen Sie, ob es fehlerhafte Erweiterungen gibt. Deinstallieren Sie alle fehlerhaften Erweiterungen, und versuchen Sie dann erneut, den virtuellen Computer zu schützen.

### <a name="cause-2-the-vms-provisioning-state-is-not-valid"></a>Ursache 2: Der Bereitstellungsstatus des virtuellen Computers ist ungültig.

Schritte zur Problembehandlung finden Sie weiter unten in diesem Artikel unter [Der Bereitstellungsstatus des virtuellen Computers ist ungültig](#the-vms-provisioning-state-is-not-valid-error-code-150019).

## <a name="the-vms-provisioning-state-is-not-valid-error-code-150019"></a>Der Bereitstellungsstatus des virtuellen Computers ist ungültig (Fehlercode 150019)

Um die Replikation auf dem virtuellen Computer zu aktivieren, muss dessen Bereitstellungsstatus **Erfolgreich** lauten. Gehen Sie folgendermaßen vor, um den Bereitstellungsstatus zu überprüfen:

1. Wählen Sie im Azure-Portal unter **Alle Dienste** die Option **Ressourcen-Explorer** aus.
1. Erweitern Sie die Liste **Abonnements**, und wählen Sie Ihr Abonnement aus.
1. Erweitern Sie die Liste **Ressourcengruppen**, und wählen Sie die Ressourcengruppe des virtuellen Computers aus.
1. Erweitern Sie die Liste **Ressourcen**, und wählen Sie den virtuellen Computer aus.
1. Überprüfen Sie das Feld **provisioningState** in der Instanzansicht auf der rechten Seite.

### <a name="fix-the-problem"></a>Beheben des Problems

- Wenn **provisioningState** den Status **Fehler** aufweist, wenden Sie sich mit den Informationen zur Problembehandlung an den Support.
- Wenn **provisioningState** den Status **Wird aktualisiert** aufweist, wird möglicherweise gerade eine andere Erweiterung bereitgestellt. Überprüfen Sie, ob gerade Vorgänge für den virtuellen Computer ausgeführt werden, warten Sie, bis diese beendet sind, und wiederholen Sie dann den fehlerhaften Site Recovery-Auftrag „Replikation aktivieren“.

## <a name="unable-to-select-target-vm-network-selection-tab-is-unavailable"></a>Virtueller Zielcomputer kann nicht ausgewählt werden (Registerkarte für die Netzwerkauswahl ist nicht verfügbar)

### <a name="cause-1-your-vm-is-attached-to-a-network-thats-already-mapped-to-a-target-network"></a>Ursache 1: Ihr virtueller Computer ist mit einem Netzwerk verbunden, das bereits einem Zielnetzwerk zugeordnet ist.

Wenn der virtuelle Quellcomputer Teil eines virtuellen Netzwerks ist und ein anderer virtueller Computer des gleichen virtuellen Netzwerks bereits einem Netzwerk in der Zielressourcengruppe zugeordnet ist, steht das Dropdown-Listenfeld für die Netzwerkauswahl standardmäßig nicht zur Verfügung (wird abgeblendet angezeigt).

![Netzwerkauswahlliste nicht verfügbar](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

### <a name="cause-2-you-previously-protected-the-vm-by-using-site-recovery-and-then-you-disabled-the-replication"></a>Ursache 2: Sie haben den virtuellen Computer zuvor mithilfe von Site Recovery geschützt und dann die Replikation deaktiviert.

Durch das Deaktivieren der Replikation eines virtuellen Computers wird die Netzwerkzuordnung nicht gelöscht. Die Zuordnung muss in dem Recovery Services-Tresor gelöscht werden, in dem der virtuelle Computer geschützt wurde. Navigieren Sie zu *Recovery Services-Tresor* > **Site Recovery-Infrastruktur** > **Netzwerkzuordnung**.

![Löschen der Netzwerkzuordnung](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)

Das Zielnetzwerk, das während der Notfallwiederherstellungseinrichtung konfiguriert wurde, kann nach dem erstmaligen Einrichten und nach dem Schützen des virtuellen Computers geändert werden:

![Ändern der Netzwerkzuordnung](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)

Beachten Sie, dass sich die Änderung der Netzwerkzuordnung auf alle geschützten virtuellen Computer auswirkt, die dieselbe Netzwerkzuordnung verwenden.

## <a name="com-or-volume-shadow-copy-service-error-error-code-151025"></a>COM+ oder Volumeschattenkopie: Dienstfehler (Fehlercode 151025)

Wenn dieser Fehler auftritt, wird die folgende Meldung angezeigt:

> „Fehler beim Installieren der Site Recovery-Erweiterung“

### <a name="possible-causes"></a>Mögliche Ursachen

- Der COM+-Systemanwendungsdienst ist deaktiviert.
- Der Volumeschattenkopie-Dienst ist deaktiviert.

### <a name="fix-the-problem"></a>Beheben des Problems

Legen Sie für den COM+-Systemanwendungsdienst und den Volumeschattenkopie-Dienst den automatischen oder manuellen Startmodus fest.

1. Öffnen Sie die Konsole „Dienste“ unter Windows.
1. Vergewissern Sie sich, dass für die Dienste „COM+-Systemanwendung“ und „Volumeschattenkopie“ die Option **Starttyp** nicht auf **Deaktiviert** festgelegt ist.

    ![Überprüfen des Starttyps der Dienste „COM+-Systemanwendung“ und „Volumeschattenkopie“](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Nicht unterstützte Größe verwalteter Datenträger (Fehlercode 150172)

Wenn dieser Fehler auftritt, wird die folgende Meldung angezeigt:

> „Schutz konnte für den virtuellen Computer nicht aktiviert werden, da dieser einen *DiskName* mit einer Größe von *DiskSize*) besitzt. Das ist kleiner als das unterstützte Minimum von 1024 MB.“

### <a name="possible-cause"></a>Mögliche Ursache

Der Datenträger ist kleiner als die unterstützte Größe von 1024 MB.

### <a name="fix-the-problem"></a>Beheben des Problems

Stellen Sie sicher, dass die Datenträgergröße innerhalb des unterstützten Größenbereichs liegt, und wiederholen Sie dann den Vorgang.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>Schutz wurde nicht aktiviert, weil die GRUB-Konfiguration den Gerätenamen anstelle der UUID enthält (Fehlercode 151126)

### <a name="possible-cause"></a>Mögliche Ursache

Die GRUB-Konfigurationsdateien unter Linux („/boot/grub/menu.lst“, „/boot/grub/grub.cfg“, „/boot/grub2/grub.cfg“ oder „/etc/default/grub“) geben möglicherweise die tatsächlichen Gerätenamen anstelle der UUID-Werte für die Parameter *root* und *resume* an. Site Recovery erfordert UUIDs, da sich Gerätenamen ändern können. Beim Neustart wird ein virtueller Computer möglicherweise nicht mit demselben Namen im Failover angezeigt, was zu Problemen führt.

Die folgenden Beispiele sind Zeilen aus GRUB-Dateien, bei denen Gerätenamen (in Fettformatierung) anstelle der erforderlichen UUIDs angezeigt werden:

- Datei „/boot/grub2/grub.cfg“

  > linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts

- Datei „/boot/grub/menu.lst“

  > kernel /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314


### <a name="fix-the-problem"></a>Beheben des Problems

Ersetzen Sie jeden Gerätenamen durch die entsprechende UUID:

1. Suchen Sie die UUID des Geräts durch Ausführen des Befehls **blkid** ***Gerätename***. Beispiel:

    ```
    blkid /dev/sda1
    /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
    blkid /dev/sda2
    /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Ersetzen Sie den Gerätenamen durch die entsprechende UUID in den Formaten **root=UUID**=*UUID* und **resume=UUID**=*UUID*. Nach der Ersetzung würde z.B. die Zeile in „boot/grub/menu.lst“ (weiter oben erläutert) wie folgt aussehen:

    > kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314

1. Wiederholen Sie den Schutzvorgang.

## <a name="enable-protection-failed-because-the-device-mentioned-in-the-grub-configuration-doesnt-exist-error-code-151124"></a>Fehler beim Aktivieren des Schutzes, weil das in der GRUB-Konfiguration genannte Gerät nicht vorhanden ist (Fehlercode 151124)

### <a name="possible-cause"></a>Mögliche Ursache

Die GRUB-Konfigurationsdateien („/boot/grub/menu.lst“, „/boot/grub/grub.cfg“, „/boot/grub2/grub.cfg“ oder „/etc/default/grub“) enthalten möglicherweise die Parameter *rd.lvm.lv* oder *rd_LVM_LV*. Mit diesen Parametern werden die LVM-Geräte (Logical Volume Manager) angegeben, die zum Startzeitpunkt erkannt werden sollen. Wenn diese LVM-Geräte nicht vorhanden sind, wird das geschützte System selbst nicht gestartet und bleibt im Startprozess hängen. Dasselbe Problem tritt auch bei der Failover-VM auf. Hier sind einige Beispiele angegeben:

- Datei „/boot/grub2/grub.cfg“ für RHEL7:

    > linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M\@64M **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet LANG=en_US.UTF-8

- Datei „/etc/default/grub“ für RHEL7:

    > GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet"

- Datei „/boot/grub/menu.lst“ für RHEL6:

    > kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto **rd_LVM_LV=rootvg/lv_root**  KEYBOARDTYPE=pc KEYTABLE=us **rd_LVM_LV=rootvg/lv_swap** rd_NO_DM rhgb quiet

Der Teil in Fettformatierung in jedem Beispiel zeigt, dass GRUB zwei LVM-Geräte mit den Namen „root“ und „swap“ aus der Volumegruppe „rootvg“ erkennen muss.

### <a name="fix-the-problem"></a>Beheben des Problems

Wenn das LVM-Gerät nicht vorhanden ist, erstellen Sie es, oder entfernen Sie die entsprechenden Parameter aus den GRUB-Konfigurationsdateien. Versuchen Sie dann erneut, den Schutz zu aktivieren.

## <a name="a-site-recovery-mobility-service-update-finished-with-warnings-error-code-151083"></a>Ein Update des Site Recovery-Mobilitätsdiensts wurde mit Warnungen beendet (Fehlercode 151083)

Der Site Recovery-Mobilitätsdienst hat viele Komponenten, von denen einer der Filtertreiber ist. Der Filtertreiber wird nur bei einem Systemneustart in den Systemspeicher geladen. Wenn ein Update des Mobilitätsdiensts Änderungen des Filtertreibers enthält, wird der Computer aktualisiert, doch wird weiterhin eine Warnung angezeigt, dass für einige Korrekturen ein Neustart erforderlich ist. Die Warnung wird angezeigt, weil die Korrekturen des Filtertreibers nur wirksam werden können, wenn der neue Filtertreiber geladen wird. Dies geschieht nur während eines Neustarts.

> [!NOTE]
> Dies ist nur eine Warnung. Die bestehende Replikation funktioniert auch nach dem neuen Agent-Update. Sie können jederzeit einen Neustart durchführen, um die Vorteile des neuen Filtertreibers zu nutzen. Wenn Sie keinen Neustart durchführen, funktioniert der alte Filtertreiber jedoch weiterhin.
>
> Abgesehen vom Filtertreiber werden die Vorteile aller anderen Erweiterungen und Korrekturen im Update des Mobilitätsdiensts wirksam, ohne dass ein Neustart erforderlich ist.  

## <a name="protection-couldnt-be-enabled-because-the-replica-managed-disk-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>Der Schutz konnte nicht aktiviert werden, weil der verwaltete Replikatdatenträger bereits ohne erwartete Tags in der Zielressourcengruppe vorhanden ist (Fehlercode 150161)

### <a name="possible-cause"></a>Mögliche Ursache

Dieses Problem kann auftreten, wenn der virtuelle Computer zuvor geschützt war und der Replikatdatenträger beim Deaktivieren der Replikation nicht bereinigt wurde.

### <a name="fix-the-problem"></a>Beheben des Problems

Löschen Sie den in der Fehlermeldung angegebenen Replikatdatenträger, und wiederholen Sie den fehlerhaften Schutzauftrag.

## <a name="next-steps"></a>Nächste Schritte

[Replizieren virtueller Azure-Computer](site-recovery-replicate-azure-to-azure.md)
