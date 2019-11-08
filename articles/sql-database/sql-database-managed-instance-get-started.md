---
title: 'Azure-Portal: Erstellen einer verwalteten Instanz'
description: Erstellen einer verwalteten SQL-Datenbank-Instanz, einer Netzwerkumgebung und eines virtuellen Clientcomputers für den Zugriff
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: danimir
ms.author: danil
ms.reviewer: sstein, carlrab
ms.date: 09/26/2019
ms.openlocfilehash: 578ab250d0759aa7216327124417019f2e5f1fc7
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825466"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Schnellstart: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz

In dieser Schnellstartanleitung wird Schritt für Schritt beschrieben, wie Sie eine [verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) im Azure-Portal erstellen.

> [!IMPORTANT]
> Informationen zu Einschränkungen finden Sie in den Abschnitten [Unterstützte Regionen](sql-database-managed-instance-resource-limits.md#supported-regions) und [Unterstützte Abonnementtypen](sql-database-managed-instance-resource-limits.md#supported-subscription-types).

## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

In den folgenden Schritten wird veranschaulicht, wie Sie eine verwaltete Instanz erstellen:

1. Wählen Sie im Azure-Portal im Menü links die Option **Azure SQL** aus. Falls **Azure SQL** nicht in der Liste aufgeführt ist, sollten Sie **Alle Dienste** auswählen und anschließend im Suchfeld den Suchbegriff **Azure SQL** eingeben.
2. Wählen Sie **+Hinzufügen** aus, um die Seite **SQL-Bereitstellungsoption auswählen** zu öffnen. Sie können weitere Informationen zu einer verwalteten Azure SQL-Datenbank-Instanz anzeigen, indem Sie auf der Kachel **Verwaltete Instanzen** die Option **Details anzeigen** auswählen.
3. Klicken Sie auf **Erstellen**.

   ![Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/create-managed-instance.png)

4. Verwenden Sie die Registerkarten im Bereitstellungsformular **Verwaltete Azure SQL-Datenbank-Instanz erstellen**, um die erforderlichen und optionalen Informationen hinzuzufügen. In den folgenden Abschnitten werden diese Registerkarten beschrieben.

### <a name="basics"></a>Grundlagen

- Geben Sie auf der Registerkarte **Grundlagen** die erforderlichen Informationen ein. Hierbei handelt es sich um die mindestens erforderlichen Informationen, die zum Bereitstellen einer verwalteten Instanz benötigt werden.

   ![Registerkarte „Grundlagen“ zum Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-basics.png)

   Nutzen Sie die Tabelle unten als Referenz für die Informationen, die auf dieser Registerkarte erforderlich sind.

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   | **Abonnement** | Ihr Abonnement | Ein Abonnement, mit dem Sie die Berechtigung zum Erstellen neuer Ressourcen erhalten |
   | **Ressourcengruppe** | Eine neue oder vorhandene Ressourcengruppe.|Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging) (Benennungsregeln und Einschränkungen).|
   | **Name der verwalteten Instanz** | Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).|
   | **Region** |Region, in der Sie die verwaltete Instanz erstellen möchten|Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   | **Administratoranmeldung für verwaltete Instanz** | Ein beliebiger gültiger Benutzername | Gültige Namen finden Sie unter [Benennungskonventionen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging). Verwenden Sie nicht „serveradmin“. Hierbei handelt es sich um eine reservierte Rolle auf Serverebene.|
   | **Kennwort** | Ein gültiges Kennwort| Das Kennwort muss mindestens 16 Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|

- Wählen Sie **Verwaltete Instanz konfigurieren** aus, um die Größe für die Compute- und Speicherressourcen anzugeben und sich die Tarife anzusehen. Verwenden Sie die Schieberegler oder Textfelder, um die Speichermenge und die Anzahl von virtuellen Kernen anzugeben. Wählen Sie abschließend **Anwenden** aus, um Ihre Auswahl zu speichern. 

   ![Formular für die verwaltete Instanz](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-configure-performance.png)

- Sie können **Überprüfen + erstellen** auswählen, um Ihre Auswahl vor dem Erstellen einer verwalteten Instanz zu überprüfen. Sie können auch Netzwerkoptionen konfigurieren, indem Sie **Weiter: Netzwerk** aus.

### <a name="networking"></a>Netzwerk

- Geben Sie auf der Registerkarte **Netzwerk** optionale Informationen ein. Wenn Sie diese Informationen weglassen, werden im Portal die Standardeinstellungen angewendet.

   ![Registerkarte „Netzwerk“ zum Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-networking.png)

   Nutzen Sie die Tabelle unten als Referenz für die Informationen, die auf dieser Registerkarte erforderlich sind.

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   | **Virtuelles Netzwerk** | Wählen Sie entweder **Neues virtuelles Netzwerk erstellen** oder ein gültiges virtuelles Netzwerk und ein Subnetz aus.| Wenn ein Netzwerk oder Subnetz nicht verfügbar ist, muss es [modifiziert werden, um die Netzwerkanforderungen zu erfüllen](sql-database-managed-instance-configure-vnet-subnet.md), bevor Sie es als Ziel für die neue verwaltete Instanz auswählen. Informationen zu den Anforderungen für das Konfigurieren der Netzwerkumgebung für eine verwaltete Instanz finden Sie unter [Konnektivitätsarchitektur für eine verwaltete Instanz in Azure SQL-Datenbank](sql-database-managed-instance-connectivity-architecture.md). |
   | **Verbindungstyp** | Wählen Sie zwischen einem Proxy und einer Umleitung als Verbindungstyp.|Weitere Informationen zu Verbindungstypen finden Sie unter [Verbindungsarchitektur von Azure SQL](sql-database-connectivity-architecture.md#connection-policy).|
   | **Öffentlicher Endpunkt**  | Wählen Sie **Aktivieren** aus. | Sie müssen diese Option aktivieren, damit über den öffentlichen Datenendpunkt auf eine verwaltete Instanz zugegriffen werden kann. | 
   | **Zugriff erlauben von** (wenn **Öffentlicher Endpunkt** aktiviert ist) | Wählen Sie eine der Optionen aus.   |Im Portal können Sie eine Sicherheitsgruppe mit einem öffentlichen Endpunkt konfigurieren. </br> </br> Wählen Sie basierend auf Ihrem Szenario eine der folgenden Optionen aus: </br> <ul> <li>**Azure-Dienste**: Wir empfehlen Ihnen die Nutzung dieser Option, wenn Sie über Power BI oder einen anderen mehrinstanzenfähigen Dienst eine Verbindung herstellen. </li> <li> **Internet**: Verwenden Sie diese Option zu Testzwecken, wenn Sie schnell eine verwaltete Instanz einrichten möchten. Von der Verwendung in Produktionsumgebungen raten wir ab. </li> <li> **Kein Zugriff**: Mit dieser Option wird eine Sicherheitsregel vom Typ **Ablehnen** erstellt. Ändern Sie diese Regel, um zu ermöglichen, dass über einen öffentlichen Endpunkt auf eine verwaltete Instanz zugegriffen wird. </li> </ul> </br> Weitere Informationen zur Sicherheit öffentlicher Endpunkte finden Sie unter [Sicheres Verwenden einer verwalteten Azure SQL-Datenbank-Instanz mit öffentlichen Endpunkten](sql-database-managed-instance-public-endpoint-securely.md).|

- Wählen Sie **Überprüfen + erstellen** aus, um Ihre Auswahl vor dem Erstellen einer verwalteten Instanz zu überprüfen. Sie können auch weitere benutzerdefinierte Einstellungen konfigurieren, indem Sie **Weiter: Zusätzliche Einstellungen**.

### <a name="additional-settings"></a>Zusätzliche Einstellungen

- Geben Sie auf der Registerkarte **Zusätzliche Einstellungen** optionale Informationen ein. Wenn Sie diese Informationen weglassen, werden im Portal die Standardeinstellungen angewendet.

   ![Registerkarte „Zusätzliche Einstellungen“ zum Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-additional-settings.png)

   Nutzen Sie die Tabelle unten als Referenz für die Informationen, die auf dieser Registerkarte erforderlich sind.

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   | **Sortierung** | Wählen Sie die Sortierung aus, die Sie für Ihre verwaltete Instanz verwenden möchten. Wenn Sie Datenbanken von SQL Server migrieren, überprüfen Sie die Quellsortierung mit `SELECT SERVERPROPERTY(N'Collation')`, und verwenden Sie diesen Wert.| Informationen zu Sortierungen finden Sie unter [Festlegen oder Ändern der Serversortierung](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|   
   | **Zeitzone** | Wählen Sie die Zeitzone aus, die von der verwalteten Instanz verwendet wird.|Weitere Informationen finden Sie unter [Zeitzone in einer verwalteten Azure SQL-Datenbank-Instanz (Vorschauversion)](sql-database-managed-instance-timezone.md).|
   | **Als sekundären Failoverserver verwenden** | Wählen Sie **Ja** aus. | Aktivieren Sie diese Option, um die verwaltete Instanz als sekundäre Failovergruppe zu verwenden.|
   | **Primäre verwaltete Instanz** (wenn **Als sekundären Failoverserver verwenden** auf **Ja** festgelegt ist) | Wählen Sie eine vorhandene primäre verwaltete Instanz aus, die in dieselbe DNS-Zone wie die von Ihnen erstellte verwaltete Instanz eingebunden wird. | Mit diesem Schritt wird die Konfiguration der Failovergruppe nach der Erstellung aktiviert. Weitere Informationen finden Sie unter [Tutorial: Hinzufügen einer verwalteten SQL-Datenbank-Instanz zu einer Failovergruppe](sql-database-managed-instance-failover-group-tutorial.md).|

### <a name="review--create"></a>Bewerten + erstellen

5. Wählen Sie die Registerkarte **Überprüfen + erstellen** aus, um Ihre Auswahl vor dem Erstellen der verwalteten Instanz zu überprüfen.

   ![Registerkarte zum Überprüfen und Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-review-create.png)

6. Wählen Sie **Erstellen** aus, um die Bereitstellung der verwalteten Instanz zu starten.

> [!IMPORTANT]
> Die Bereitstellung einer verwalteten Instanz ist ein Vorgang mit langer Ausführungsdauer. Die Bereitstellung der ersten Instanz im Subnetz dauert normalerweise deutlich länger als die Bereitstellung in einem Subnetz, das bereits verwaltete Instanzen enthält. Informationen zur durchschnittlichen Bereitstellungsdauer finden Sie unter [Verwaltungsvorgänge für verwaltete Instanzen](sql-database-managed-instance.md#managed-instance-management-operations).

### <a name="monitor-deployment-progress"></a>Überwachen des Bereitstellungsstatus

7. Wählen Sie das Symbol **Benachrichtigungen** aus, um den Status der Bereitstellung anzuzeigen.

   ![Status der Bereitstellung einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/in-progress/mi-create-deployment-in-progress.png)

8. Wählen Sie in der Benachrichtigung **Die Bereitstellung wird ausgeführt** aus, um das Fenster für die verwaltete Instanz zu öffnen und den Bereitstellungsstatus weiter zu überwachen. 

> [!TIP]
> Falls Sie den Webbrowser geschlossen haben oder sich nicht mehr im Fenster mit dem Bereitstellungsstatus befinden, führen Sie die folgenden Schritte aus, um zum Bildschirm mit dem Bereitstellungsstatus zu wechseln:
> 1. Öffnen Sie im Azure-Portal die Ressourcengruppe (auf der Registerkarte **Grundlagen**), für die Sie eine verwaltete Instanz bereitstellen.
> 2. Wählen Sie **Bereitstellungen** aus.
> 3. Wählen Sie den Vorgang für die Bereitstellung der verwalteten Instanz aus, der derzeit durchgeführt wird.

## <a name="post-deployment-operations"></a>Vorgänge nach der Bereitstellung

Führen Sie die in diesem Abschnitt beschriebenen Schritte aus, um die erstellten Ressourcen zu überprüfen, Netzwerkeinstellungen zu optimieren und Hostverbindungsdetails (FQDN) abzurufen.

### <a name="view-resources-created"></a>Anzeigen von erstellten Ressourcen

Nach erfolgreicher Bereitstellung der verwalteten Instanz können Sie erstellte Ressourcen wie folgt anzeigen:

1. Öffnen Sie die Ressourcengruppe für Ihre verwaltete Instanz. Zeigen Sie die Ressourcen an, die in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](#create-a-managed-instance) erstellt wurden.

   ![Ressourcen der verwalteten Instanz](./media/sql-database-managed-instance-get-started/resources.png)

### <a name="view-and-fine-tune-network-settings"></a>Anzeigen und Optimieren von Netzwerkeinstellungen

Untersuchen Sie die folgenden Punkte, um Netzwerkeinstellungen zu optimieren (optional):

1. Wählen Sie die Routingtabelle aus, um die benutzerdefinierte Route anzuzeigen, die für Sie erstellt wurde.

   ![Routingtabelle](./media/sql-database-managed-instance-get-started/route-table.png)

2. Sehen Sie sich in der Routingtabelle die Einträge für das Leiten von Datenverkehr aus dem bzw. im virtuellen Netzwerk der verwalteten Instanz an. Wenn Sie Ihre Routingtabelle manuell erstellen oder konfigurieren, achten Sie darauf, dass Sie diese Einträge in der Routingtabelle der verwalteten Instanz erstellen.

   ![Eintrag für „Subnetz der verwalteten Instanz zu lokal“](./media/sql-database-managed-instance-get-started/udr.png)

3. Wechseln Sie zurück zur Ressourcengruppe, und wählen Sie die Netzwerksicherheitsgruppe aus.

   ![Netzwerksicherheitsgruppe](./media/sql-database-managed-instance-get-started/network-security-group.png)

4. Überprüfen Sie die Eingangs- und Ausgangssicherheitsregeln. 

   ![Sicherheitsregeln](./media/sql-database-managed-instance-get-started/security-rules.png)

> [!IMPORTANT]
> Wenn Sie einen öffentlichen Endpunkt für die verwaltete Instanz konfiguriert haben, müssen Sie Ports öffnen, um Netzwerkdatenverkehr für Verbindungen mit der verwalteten Instanz aus dem öffentlichen Internet zuzulassen. Weitere Informationen erhalten Sie unter [Konfigurieren eines öffentlichen Endpunkts für eine verwaltete Instanz](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group).
>

### <a name="retrieve-connection-details-to-managed-instance"></a>Abrufen von Verbindungsdetails zur verwalteten Instanz

Wenn Sie eine Verbindung mit der verwalteten Instanz herstellen möchten, führen Sie die folgenden Schritte aus, um den Hostnamen und den vollqualifizierten Domänennamen (FQDN) abzurufen:

1. Wechseln Sie zurück zur Ressourcengruppe, und wählen Sie Ihre verwaltete Instanz aus.

   ![Verwaltete Instanz in der Ressourcengruppe](./media/sql-database-managed-instance-get-started/managed-instance.png)

2. Suchen Sie auf der Registerkarte **Übersicht** nach der Eigenschaft **Host**. Kopieren Sie den Hostnamen für die verwaltete Instanz zur Verwendung in der nächsten Schnellstartanleitung.

   ![Hostname](./media/sql-database-managed-instance-get-started/host-name.png)

   Der kopierte Wert stellt einen vollqualifizierten Domänennamen (FQDN) dar, mit dem eine Verbindung mit einer verwalteten Instanz hergestellt werden kann. Er ähnelt der folgenden Beispieladresse: *your_host_name.a1b2c3d4e5f6.database.windows.net*.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verbinden mit einer verwalteten Instanz:
- Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connect-app.md).
- Eine Schnellstartanleitung, die zeigt, wie Sie eine Verbindung zwischen einem virtuellen Azure-Computer und einer verwalteten Instanz herstellen, finden Sie unter [Schnellstart: Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-vm.md).
- Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung mit einer verwalteten Instanz herstellen, finden Sie unter [Schnellstart: Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer mit einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-p2s.md).

Wiederherstellen einer vorhandenen SQL Server-Datenbank aus einer lokalen Instanz in einer verwalteten Instanz: 
- Verwenden Sie zur Wiederherstellung auf der Grundlage einer Datenbanksicherungsdatei [Azure Database Migration Service für die Migration](../dms/tutorial-sql-server-to-managed-instance.md). 
- Verwenden Sie zur Wiederherstellung auf der Grundlage einer Datenbanksicherungsdatei den Befehl [T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md).

Informationen zur erweiterten Überwachung der Datenbankleistung verwalteter Instanzen mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../azure-monitor/insights/azure-sql.md).
