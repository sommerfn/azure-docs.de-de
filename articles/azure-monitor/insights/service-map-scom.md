---
title: Integration von Azure Monitor für VMs in System Center Operations Manager | Microsoft-Dokumentation
description: Azure Monitor für VMs ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In diesem Artikel wird die Verwendung des Zuordnungsfeatures zum automatischen Erstellen von Diagrammen der verteilten Anwendungen in Operations Manager erörtert.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 3523756e89506e90407090db105fdced5853d9d9
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553961"
---
# <a name="system-center-operations-manager-integration-with-azure-monitor-for-vms-map-feature"></a>Integration des Zuordnungsfeatures von Azure Monitor für VMs in System Center Operations Manager

In Azure Monitor für VMs können Sie sich ermittelte Anwendungskomponenten für Windows- und Linux-VMs anzeigen lassen, die in Azure oder in Ihrer eigenen Umgebung ausgeführt werden. Durch diese Integration zwischen dem Zuordnungsfeature und System Center Operations Manager können Sie in Operations Manager automatisch Diagramme der verteilten Anwendungen erstellen, die auf den dynamischen Abhängigkeitszuordnungen in Azure Monitor für VMs basieren. 

>[!NOTE]
>Wenn Sie die Dienstzuordnung bereits bereitgestellt haben, können Sie Ihre Zuordnungen in Azure Monitor für VMs anzeigen. Diese Lösung enthält zusätzliche Funktionen zum Überwachen der Integrität und Leistung von virtuellen Computern. Das Zuordnungsfeature von Azure Monitor für VMs soll die eigenständige Dienstzuordnungslösung ersetzen. Weitere Informationen finden Sie in der [Übersicht über Azure Monitor für VMs](vminsights-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Eine System Center Operations Manager-Verwaltungsgruppe (2012 R2 oder höher).
* Ein für die Unterstützung von Azure Monitor für VMs konfigurierter Log Analytics-Arbeitsbereich.
* Ein oder mehrere virtuelle Windows- und Linux-Computer oder physische Computer, die von Operations Manager überwacht werden und Daten an Ihren Log Analytics Arbeitsbereich senden. Linux-Server, die Berichte an eine Operations Manager-Verwaltungsgruppe senden, müssen für eine direkte Verbindung mit Azure Monitor konfiguriert werden. Weitere Informationen finden Sie in der Übersicht unter [Sammeln von Protokolldaten mit dem Log Analytics-Agent](../platform/log-analytics-agent.md).
* Ein Dienstprinzipal mit Zugriff auf das Azure-Abonnement, das dem Log Analytics-Arbeitsbereich zugeordnet ist. Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals](#create-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Installieren des Service Map Management Pack

Sie aktivieren die Integration zwischen Operations Manager und dem Zuordnungsfeature durch Importieren des Management Pack-Pakets „Microsoft.SystemCenter.ServiceMap“ („Microsoft.SystemCenter.ServiceMap.mpb“). Sie können das Management Pack-Paket aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=55763) herunterladen. Das Paket enthält die folgenden Management Packs:

* Microsoft Service Map Application Views
* Microsoft System Center Service Map Internal
* Microsoft System Center Service Map Overrides
* Microsoft System Center Service Map

## <a name="configure-integration"></a>Konfigurieren der Integration

Nach der Installation des Management Packs der Dienstzuordnung wird in Ihrer Operations Manager-Betriebskonsole im Bereich **Verwaltung** unter **Operations Management Suite** ein neuer Knoten mit dem Namen **Dienstzuordnung** angezeigt.

>[!NOTE]
>[Operations Management Suite war eine Sammlung von Diensten](../terminology.md#april-2018---retirement-of-operations-management-suite-brand), zu der auch Log Analytics gehörte, das jetzt Teil von [Azure Monitor](../overview.md) ist.

Gehen Sie zum Konfigurieren der Integration des Zuordnungsfeatures von Azure Monitor für VMs wie folgt vor:

1. Um den Konfigurations-Assistenten zu öffnen, klicken Sie im Bereich **Service Map Overview** (Service Map-Übersicht) auf **Arbeitsbereich hinzufügen**.  

    ![Bereich „Service Map Overview“ (Service Map-Übersicht)](media/service-map-scom/scom-configuration.png)

2. Geben Sie im Fenster **Verbindungskonfiguration** den Mandantennamen oder die Mandanten-ID, die Anwendungs-ID (auch als Benutzername bzw. clientID bezeichnet) und das Kennwort des Dienstprinzipals ein, und klicken Sie auf **Weiter**. Weitere Informationen finden Sie unter „Erstellen eines Dienstprinzipals“.

    ![Das Fenster „Verbindungskonfiguration“](media/service-map-scom/scom-config-spn.png)

3. Wählen Sie im Fenster **Abonnementauswahl** das Azure-Abonnement, die Azure-Ressourcengruppe (diejenige, die den Log Analytics-Arbeitsbereich enthält) und den Log Analytics-Arbeitsbereich, und klicken Sie dann auf **Weiter**.

    ![Der Operations Manager-Konfigurationsarbeitsbereich](media/service-map-scom/scom-config-workspace.png)

4. Wählen Sie im Fenster **Computergruppenauswahl**, welche Service Map-Computergruppen Sie mit Operations Manager synchronisieren möchten. Klicken Sie auf **Computergruppen hinzufügen/entfernen**, wählen Sie Gruppen aus der Liste der **Verfügbaren Computergruppen**, und klicken Sie auf **Hinzufügen**.  Klicken Sie nach Auswahl der Gruppen auf **OK**, um den Vorgang abzuschließen.

    ![Die Operations Manager-Konfigurationscomputergruppen](media/service-map-scom/scom-config-machine-groups.png)

5. Konfigurieren Sie im Fenster **Serverauswahl** die Servergruppe für die Dienstzuordnung mit den Servern, die Sie zwischen Operations Manager und dem Zuordnungsfeature synchronisieren möchten. Klicken Sie auf **Server hinzufügen/entfernen**.

    Damit die Integration ein Diagramm der verteilten Anwendung für einen Server erstellen kann, muss der Server folgende Voraussetzungen erfüllen:

   * Er muss von Operations Manager überwacht werden.
   * Er muss für das Senden von Berichten an den mit Azure Monitor für VMs konfigurierten Log Analytics-Arbeitsbereich konfiguriert sein.
   * In der „Service Map-Servergruppe“ aufgelistet

     ![Die Operations Manager-Konfigurationsgruppe](media/service-map-scom/scom-config-group.png)

6. Optional: Wählen Sie den Ressourcenpool für alle Verwaltungsserver für die Kommunikation mit Log Analytics aus, und klicken Sie dann auf **Arbeitsbereich hinzufügen**.

    ![Der Operations Manager-Konfigurationsressourcenpool](media/service-map-scom/scom-config-pool.png)

    Es kann eine Minute dauern, den Log Analytics-Arbeitsbereich zu konfigurieren und registrieren. Nach Abschluss der Konfiguration des Arbeitsbereichs initiiert Operations Manager die erste Zuordnungssynchronisierung.

    ![Der Operations Manager-Konfigurationsressourcenpool](media/service-map-scom/scom-config-success.png)

## <a name="monitor-integration"></a>Überwachen der Integration

Nachdem die Verbindung mit dem Log Analytics-Arbeitsbereich hergestellt wurde, wird in der Operations Manager-Betriebskonsole im Bereich **Überwachung** ein neuer Ordner mit dem Namen „Dienstzuordnung“ angezeigt.

![Operations Manager-Bereich „Überwachung“](media/service-map-scom/scom-monitoring.png)

Der Ordner „Service Map“ umfasst vier Knoten:

* **Aktive Warnungen**: Listet alle aktiven Warnungen zur Kommunikation zwischen Operations Manager und Azure Monitor auf.  

  >[!NOTE]
  >Bei diesen Warnungen handelt es sich nicht um Log Analytics-Warnungen, die mit Operations Manager synchronisiert werden. Sie werden in der Verwaltungsgruppe basierend auf Workflows generiert, die im Management Pack der Dienstzuordnung definiert sind.

* **Server**: Listet die überwachten Server auf, die für die Synchronisierung über das Zuordnungsfeature von Azure Monitor für VMs konfiguriert sind.

    ![Der Serverüberwachungsbereich von Operations Manager](media/service-map-scom/scom-monitoring-servers.png)

* **Computerabhängigkeitsansichten**: Listet alle Computergruppen auf, die über das Zuordnungsfeature synchronisiert werden. Sie können auf eine beliebige Gruppe klicken, um das entsprechende Diagramm der verteilten Anwendungen anzuzeigen.

    ![Das Diagramm der verteilten Anwendungen im Operations Manager](media/service-map-scom/scom-group-dad.png)

* **Serverabhängigkeitsansichten**: Listet alle Server auf, die über das Zuordnungsfeature synchronisiert werden. Sie können auf einen beliebigen Server klicken, um das entsprechende Diagramm der verteilten Anwendungen anzuzeigen.

    ![Das Diagramm der verteilten Anwendungen im Operations Manager](media/service-map-scom/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Bearbeiten oder Löschen des Arbeitsbereichs

Sie können den konfigurierten Arbeitsbereich im Bereich der **Service Map-Übersicht** (Bereich **Verwaltung** > **Operations Management Suite** > **Service Map**) bearbeiten oder löschen.

>[!NOTE]
>[Operations Management Suite war eine Sammlung von Diensten](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand), zu der auch Log Analytics, das jetzt Teil von [Azure Monitor](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md) ist, gehörte.

In diesem aktuellen Release können Sie nur einen Log Analytics-Arbeitsbereich konfigurieren.

![Der Bereich zum Bearbeiten des Arbeitsbereichs im Operations Manager](media/service-map-scom/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Konfigurieren von Regeln und Außerkraftsetzungen

Die Regel *Microsoft.SystemCenter.ServiceMapImport.Rule* ruft regelmäßig Informationen aus dem Zuordnungsfeature von Azure Monitor für VMs ab. Wenn Sie das Synchronisierungsintervall ändern möchten, können Sie die Regel außer Kraft setzen und den Wert für den Parameter **IntervalMinutes** ändern.

![Das Operations Manager-Fenster für Eigenschaften von Außerkraftsetzungen](media/service-map-scom/scom-overrides.png)

* **Enabled**: Aktivieren oder Deaktivieren der automatischen Updates.
* **IntervalMinutes**: Gibt die Zeitspanne zwischen Updates an. Das Standardintervall beträgt eine Stunde. Wenn Sie Zuordnungen in kürzeren Abständen synchronisieren möchten, können Sie den Wert ändern.
* **TimeoutSeconds**: Gibt die Zeitspanne bis zum Timeout der Anforderung an.
* **TimeWindowMinutes**: Gibt das Zeitfenster zum Abfragen von Daten an. Der Standardwert ist 60 Minuten. Dies ist das maximal zulässige Intervall.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Das aktuelle Design weist folgende Probleme und Einschränkungen auf:

* Sie können nur eine Verbindung mit einem einzigen Log Analytics-Arbeitsbereich herstellen.
* Sie können im Bereich **Erstellen** der „Service Map-Servergruppe“ zwar manuell Server hinzufügen, aber die Zuordnungen für diese Server werden nicht sofort synchronisiert. Sie werden beim nächsten Synchronisierungszyklus vom Zuordnungsfeature von Azure Monitor für VMs synchronisiert.
* Wenn Sie Änderungen an den vom Management Pack erstellten Diagrammen der verteilten Anwendungen vornehmen, werden diese Änderungen wahrscheinlich bei der nächsten Synchronisierung mit Azure Monitor für VMs überschrieben.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Die offizielle Azure-Dokumentation zum Erstellen eines Dienstprinzipals finden Sie in den folgenden Dokumenten:

* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Möchten Sie uns Feedback zur Integration des Zuordnungsfeatures von Azure Monitor für VMs oder zu dieser Dokumentation geben? Besuchen Sie unsere [UserVoice-Webseite](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), auf der Sie Features vorschlagen oder vorhandene Vorschläge unterstützen können.
