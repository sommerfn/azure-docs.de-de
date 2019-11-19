---
title: Sichern einer SAP HANA-Datenbank mit Azure Backup in Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird erläutert, wie Sie mit dem Azure Backup-Dienst eine SAP HANA-Datenbank in Azure sichern.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 08/27/2019
ms.author: dacurwin
ms.openlocfilehash: 8d99ff6f2d8a21a501631a3a062be6b05130c05b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931815"
---
# <a name="back-up-an-sap-hana-database-to-azure"></a>Sichern einer SAP HANA-Datenbank in Azure

[Azure Backup](backup-overview.md) unterstützt die Sicherung von SAP HANA-Datenbanken in Azure.

> [!NOTE]
> Dieses Feature ist zurzeit als öffentliche Preview verfügbar. Es ist derzeit nicht für den Einsatz in Produktionsumgebungen bereit und verfügt nicht über eine garantierte SLA.

## <a name="scenario-support"></a>Unterstützung von Szenarien

**Unterstützung** | **Details**
--- | ---
**Unterstützte geografische Räume** | Australien, Südosten; Australien, Osten <br> Brasilien Süd <br> Kanada, Mitte; Kanada, Osten <br> Asien, Südosten; Asien, Osten <br> USA, Osten; USA, Osten 2; USA, Westen-Mitte; USA, Westen; USA, Westen 2, USA, Norden-Mitte; USA, Mitte; USA, Süden-Mitte<br> Indien, Mitte; Indien, Süden <br> „Japan, Osten“, „Japan, Westen“<br> Korea, Mitte, Korea, Süden <br> „Europa, Norden“, „Europa, Westen“ <br> Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen
**Unterstützte VM-Betriebssysteme** | SLES 12 mit SP2, SP3 oder SP4.
**Unterstützte HANA-Versionen** | SDC auf HANA 1.x, MDC auf HANA 2.x <= SPS04 Rev 43

### <a name="current-limitations"></a>Aktuelle Einschränkungen

- Sie können nur auf Azure-VMs ausgeführte SAP HANA-Datenbanken sichern.
- Sie können nur eine SAP HANA-Instanz sichern, die auf einer einzelnen Azure-VM ausgeführt wird. Mehrere HANA-Instanzen auf derselben Azure-VM werden derzeit nicht unterstützt.
- Sie können nur Datenbanken im Modus für zentrales Hochskalieren sichern. Das horizontale Hochskalieren, d.h. eine HANA-Instanz auf mehreren Azure-VMs, wird derzeit nicht für die Sicherung unterstützt.
- Sie können eine SAP HANA-Instanz mit Dynamic Tiering auf einem erweiterten Server nicht sichern, d.h. mit Dynamic Tiering auf einem anderen Knoten. Dies ist im Wesentlichen ein horizontales Hochskalieren, das nicht unterstützt wird.
- Sie können eine SAP HANA-Instanz mit aktiviertem Dynamic Tiering auf demselben Server nicht sichern. Dynamic Tiering wird derzeit nicht unterstützt.
- Die SAP HANA-Sicherung kann nur im Azure-Portal konfiguriert werden. Das Feature kann nicht mit PowerShell oder der Befehlszeilenschnittstelle konfiguriert werden.
- Sie können Datenbankprotokolle alle 15 Minuten sichern. Protokollsicherungen werden erst nach dem erfolgreichen Abschluss einer vollständigen Sicherung der Datenbank übertragen.
- Sie können vollständige und differenzielle Sicherungen ausführen. Inkrementelle Sicherungen werden derzeit nicht unterstützt.
- Nachdem Sie die Sicherungsrichtlinie auf SAP HANA-Sicherungen angewendet haben, kann sie nicht mehr geändert werden. Wenn Sie Sicherungen mit unterschiedlichen Einstellungen ausführen möchten, müssen Sie eine neue Richtlinie erstellen oder eine andere Richtlinie zuweisen.
  - Klicken Sie zum Erstellen einer neuen Richtlinie im Tresor auf **Richtlinien** > **Sicherungsrichtlinien** >  **+ Hinzufügen** > **SAP HANA in Azure-VM**, und legen Sie die Richtlinieneinstellungen fest.
  - Wenn Sie eine andere Richtlinie zuweisen möchten, klicken Sie in den Eigenschaften des virtuellen Computers, auf dem die Datenbank ausgeführt wird, auf den Namen der aktuellen Richtlinie. Auf der Seite **Sicherungsrichtlinie** können Sie dann eine andere Richtlinie für die Sicherung auswählen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Konfigurieren von Sicherungen unbedingt die folgenden Schritte aus:

1. Installieren und aktivieren Sie auf dem virtuellen Computer, auf dem die SAP HANA-Datenbank ausgeführt wird, die im offiziellen SLES-Paket/auf den offiziellen SLES-Medien enthaltenen ODBC-Treiberpakete mithilfe von zypper. Gehen Sie dabei wie folgt vor:

    ```unix
    sudo zypper update
    sudo zypper install unixODBC
    ```

    > [!NOTE]
    > Wenn Sie die Repositorys nicht aktualisieren, stellen Sie sicher, dass mindestens die Version 2.3.4 von unixODBC verwendet wird. Die Version von unixODBC zu ermitteln, führen Sie ```odbcinst -j``` als Stamm aus.

2. Lassen Sie eine Verbindung des virtuellen Computers mit dem Internet zu, damit er Azure erreichen kann, wie [nachstehend](#set-up-network-connectivity) beschrieben.

3. Führen Sie das Vorregistrierungsskript auf dem virtuellen Computer aus, auf dem HANA als Root-Benutzer installiert ist. Das Skript wird im [Portal](#discover-the-databases) im Flow bereitgestellt und ist erforderlich, um die [richtigen Berechtigungen](backup-azure-sap-hana-database-troubleshoot.md#setting-up-permissions) einzurichten.

### <a name="set-up-network-connectivity"></a>Einrichten der Netzwerkkonnektivität

Der virtuelle SAP HANA-Computer benötigt für alle Vorgänge eine Verbindung, um auf die öffentlichen Azure-IP-Adressen zugreifen zu können. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) können ohne Konnektivität nicht ausgeführt werden. Richten Sie Konnektivität ein, indem Sie Zugriff auf die IP-Adressbereiche für Azure-Rechenzentren gewähren: 

- Sie können die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) für Azure-Rechenzentren herunterladen und dann Zugriff auf diese IP-Adressen gewähren.
- Wenn Sie Netzwerksicherheitsgruppen (Network Security Groups, NSGs) verwenden, können Sie mit dem [Diensttag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) „AzureCloud“ alle öffentlichen Azure-IP-Adressen zulassen. Mit dem Cmdlet [Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) können Sie NSG-Regeln ändern.
- Port 443 sollte in der Whitelist enthalten sein, da der Transport über HTTPS erfolgt.

## <a name="onboard-to-the-public-preview"></a>Onboarding in die öffentliche Vorschau

Führen Sie das Onboarding in die öffentliche Vorschau wie folgt durch:

- Registrieren Sie im Portal Ihre Abonnement-ID bei dem Recovery Services-Dienstanbieter [gemäß der Beschreibung in diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal). 
- Führen Sie bei Verwendung von PowerShell das folgende Cmdlet aus. Der Vorgang sollte mit „Registered“ (Registriert) abgeschlossen werden.

    ```powershell
    PS C:>  Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Ermitteln der Datenbanken

1. Klicken Sie im Tresor unter **Erste Schritte** auf **Sicherung**. Wählen Sie unter **Wo wird Ihre Workload ausgeführt?** den Eintrag **SAP HANA in Azure-VM** aus.
2. Klicken Sie auf **Ermittlung starten**. Dadurch wird die Ermittlung nicht geschützter Linux-VMs in der Tresorregion initiiert.

   - Nach der Ermittlung werden nicht geschützte VMs im Portal nach Name und Ressourcengruppe sortiert angezeigt.
   - Wenn ein virtueller Computer nicht wie erwartet aufgeführt wird, überprüfen Sie, ob er bereits in einem Tresor gesichert wird.
   - Mehrere virtuelle Computer können den gleichen Namen aufweisen, gehören dann aber verschiedenen Ressourcengruppen an.

3. Klicken Sie im Bereich **Virtuelle Computer auswählen** auf den Link zum Herunterladen des Skripts, das dem Azure Backup-Dienst die Berechtigungen zum Zugreifen auf die SAP HANA-VMs für die Ermittlung von Datenbanken erteilt.
4. Führen Sie das Skript auf jedem virtuellen Computer aus, der die zu sichernden SAP HANA-Datenbanken hostet.
5. Wählen Sie nach dem Ausführen des Skripts auf den VMs im Bereich **Virtuelle Computer auswählen** die VMs aus. Klicken Sie dann auf **DBs ermitteln**.
6. Azure Backup ermittelt alle SAP HANA-Datenbanken auf dem virtuellen Computer. Während der Ermittlung registriert Azure Backup den virtuellen Computer beim Tresor und installiert eine Erweiterung auf dem VM. Für die Datenbank wird kein Agent installiert.

    ![Ermitteln von SAP HANA-Datenbanken](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Konfigurieren der Sicherung  

Aktivieren Sie jetzt die Sicherung.

1. Klicken Sie unter „Schritt 2“ auf **Sicherung konfigurieren**.
2. Wählen Sie unter **Zu sichernde Elemente auswählen** alle Datenbanken aus, die Sie schützen möchten, und wählen Sie dann **OK** aus.
3. Wählen Sie **Sicherungsrichtlinie** > **Sicherungsrichtlinie auswählen** aus, und erstellen Sie eine neue Sicherungsrichtlinie für die Datenbanken gemäß den unten stehenden Anweisungen.
4. Klicken Sie nach dem Erstellen der Richtlinie im Menü **Sicherung** auf **Sicherung aktivieren**.
5. Verfolgen Sie den Fortschritt der Sicherungskonfiguration im Bereich **Benachrichtigungen** des Portals.

### <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie legt fest, wann Sicherungen erstellt und wie lange sie aufbewahrt werden.

- Eine Richtlinie wird auf Tresorebene erstellt.
- Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden.

Legen Sie die Richtlinieneinstellungen wie folgt fest:

1. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein.
2. Wählen Sie in **Richtlinie für vollständige Sicherung** für **Sicherungshäufigkeit** **Täglich** oder **Wöchentlich** aus.
   - **Daily** (Täglich): Wählen Sie die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.
   
       - Sie müssen eine vollständige Sicherung ausführen. Diese Option kann nicht deaktiviert werden.
       - Klicken Sie auf **Vollständige Sicherung**, um die Richtlinie anzuzeigen.
       - Sie können keine differenziellen Sicherungen für tägliche vollständige Sicherungen erstellen.
       
   - **Wöchentlich**: Wählen Sie den Wochentag, die Uhrzeit und die Zeitzone für die Ausführung des Sicherungsauftrags aus.
3. Konfigurieren Sie unter **Beibehaltungsdauer** die Aufbewahrungseinstellungen für die vollständige Sicherung.
    - Standardmäßig sind alle Optionen ausgewählt. Deaktivieren Sie alle Optionen für die Beibehaltungsdauer, die Sie nicht verwenden möchten, und legen Sie die gewünschten Grenzwerte fest.
    - Die Mindestbeibehaltungsdauer beträgt für alle Sicherungstypen (vollständig/differenziell/Protokoll) sieben Tage.
    - Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst.
    - Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und der entsprechenden Einstellung markiert und beibehalten.
    - Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

4. Klicken Sie im Menü **Vollständige Sicherung** auf **OK**, um die Einstellungen zu übernehmen.
5. Wählen Sie **Differenzielle Sicherung** aus, um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen.
6. Wählen Sie in **Richtlinie für differenzielle Sicherung** die Option **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen.
    - Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.
    - Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Wenn Sie eine längere Aufbewahrung wünschen, müssen Sie vollständige Sicherungen verwenden.

    > [!NOTE]
    > Inkrementelle Sicherungen werden derzeit nicht unterstützt. 

7. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
8. Wählen Sie **Protokollsicherung** aus, um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen.
    - Wählen Sie unter **Protokollsicherung** die Option **Aktivieren** aus.
    - Legen Sie die Häufigkeits- und Aufbewahrungssteuerelemente fest.

    > [!NOTE]
    > Protokollsicherungen werden erst nach dem erfolgreichen Abschluss einer vollständigen Sicherung übertragen.

9. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
10. Klicken Sie nach dem Definieren der Sicherungsrichtlinie auf **OK**.


## <a name="run-an-on-demand-backup"></a>Ausführen einer On-Demand-Sicherung

Sicherungen werden gemäß dem Richtlinienzeitplan ausgeführt. Eine bedarfsgesteuerte Sicherung können Sie wie folgt ausführen:


1. Klicken Sie im Tresormenü auf **Sicherungselemente**.
2. Wählen Sie unter **Sicherungselemente** den virtuellen Computer aus, auf dem die SAP HANA-Datenbank ausgeführt wird, und klicken Sie dann auf **Jetzt sichern**.
3. Verwenden Sie unter **Jetzt sichern** den Kalender, um den letzten Tag zur Beibehaltung des Wiederherstellungspunkts auszuwählen. Klicken Sie dann auf **OK**.
4. Überwachen Sie die Portalbenachrichtigungen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > „In Bearbeitung“ überwachen. Je nach Größe Ihrer Datenbank kann das Erstellen der ersten Sicherung einige Zeit dauern.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Ausführen einer SAP HANA Studio-Sicherung einer Datenbank, für die Azure Backup aktiviert ist

Führen Sie die folgenden Schritte aus, wenn Sie (mit HANA Studio) eine lokale Sicherung einer Datenbank ausführen möchten, die mit Azure Backup gesichert wird:

1. Warten Sie, bis alle vollständigen Sicherungen oder Protokollsicherungen für die Datenbank abgeschlossen sind. Überprüfen Sie den Status in SAP HANA Studio.
2. Deaktivieren Sie Protokollsicherungen, und legen Sie den Sicherungskatalog auf das Dateisystem für die entsprechende Datenbank fest.
3. Doppelklicken Sie hierzu auf **systemdb** > **Konfiguration** > **Datenbank auswählen** > **Filter (Protokoll)** .
4. Legen Sie **enable_auto_log_backup** auf **No** fest.
5. Legen Sie **log_backup_using_backint** auf **False** fest.
6. Erstellen Sie eine vollständige Ad-hoc-Sicherung der Datenbank.
7. Warten Sie, bis die vollständige Sicherung und die Katalogsicherung abgeschlossen sind.
8. Stellen Sie die vorherigen Einstellungen für Azure wieder her:
    - Legen Sie **enable_auto_log_backup** auf **Yes** fest.
    - Legen Sie **log_backup_using_backint** auf **True** fest.


## <a name="upgrading-protected-10-dbs-to-20"></a>Upgrade geschützter 1.0-Datenbanken auf 2.0

Wenn Sie SAP HANA 1.0-Datenbanken schützen und ein Upgrade auf 2.0 durchführen möchten, führen Sie die unten beschriebenen Schritte aus.

- Beenden Sie den Schutz mit Beibehaltung der Daten für die alte SDC-Datenbank.
- Führen Sie das Vorregistrierungsskript mit den richtigen Details (SID und MDC) erneut aus. 
- Registrieren Sie die Erweiterung erneut (Sicherung > Details anzeigen > relevante Azure-VM auswählen > erneut registrieren). 
- Klicken Sie auf die erneute Ermittlung von Datenbanken für denselben virtuellen Computer. Es sollten daraufhin die neuen Datenbanken in Schritt 2 mit den richtigen Details (Systemdatenbank und Mandantendatenbank, nicht SDC) angezeigt werden. 
- Schützen Sie diese neuen Datenbanken.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie](backup-azure-sap-hana-database-troubleshoot.md), wie Sie häufige Fehler bei der Verwendung von SAP HANA-Sicherungen auf Azure-VMs beheben können.
Erfahren Sie mehr über das [Sichern virtueller Azure-Computer](backup-azure-arm-vms-prepare.md).
