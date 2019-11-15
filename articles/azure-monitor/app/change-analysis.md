---
title: Verwenden der Anwendungsänderungsanalyse in Azure Monitor für die Suche nach Web-App-Problemen | Microsoft-Dokumentation
description: Verwenden der Anwendungsänderungsanalyse in Azure Monitor zum Behandeln von Anwendungsproblemen auf Live-Websites in Azure App Service.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/07/2019
ms.openlocfilehash: dc572d29b4e6d95525959becad0ed8069735e33c
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605995"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Verwenden der Anwendungsänderungsanalyse (Vorschau) in Azure Monitor

Wenn es zu einem Problem oder Ausfall einer Live-Website kommt, ist es wichtig, die Grundursache schnell bestimmen zu können. Die Standardüberwachungslösungen machen Sie möglicherweise auf ein Problem aufmerksam. Sie geben unter Umständen sogar an, bei welcher Komponente der Fehler aufgetreten ist. In den meisten Fällen geht aus dieser Warnung jedoch die Fehlerursache nicht direkt hervor. Sie wissen, dass Ihre Website vor fünf Minuten noch funktioniert hat, und jetzt nicht mehr. Was hat sich in den letzten fünf Minuten geändert? Für die Beantwortung dieser Frage ist die Anwendungsänderungsanalyse in Azure Monitor konzipiert.

Basierend auf dem Funktionsumfang von [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) liefert die Änderungsanalyse Erkenntnisse über Azure-Anwendungsänderungen, um die Transparenz zu erhöhen und die durchschnittliche Reparaturzeit (Mean Time To Repair, MTTR) zu verkürzen.

> [!IMPORTANT]
> Die Änderungsanalyse befindet sich derzeit in der Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt. Diese Version wird nicht für die Produktion empfohlen. Manche Funktionen werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Übersicht

Die Änderungsanalyse erkennt von der Infrastrukturebene bis hin zur Anwendungsbereitstellung verschiedene Arten von Änderungen. Es handelt sich um einen Azure-Ressourcenanbieter auf Abonnementebene, der Ressourcenänderungen im Abonnement überprüft. Die Änderungsanalyse stellt Daten für verschiedene Diagnosetools bereit, damit der Benutzer besser verstehen kann, welche Änderungen möglicherweise zu Problemen geführt haben.

Das folgende Diagramm zeigt die Architektur der Änderungsanalyse:

![Architekturdiagramm zum Abrufen von Änderungsdaten und Bereitstellung dieser Daten für Clienttools durch die Änderungsanalyse](./media/change-analysis/overview.png)

Derzeit ist die Änderungsanalyse in die **Diagnose und Problembehandlung** für App Service-Web-Apps integriert. Informationen zum Aktivieren der Änderungserkennung und zum Anzeigen von Änderungen in der Web-App finden Sie im Abschnitt *Änderungsanalyse für die Web-Apps-Funktion* weiter unten.

### <a name="azure-resource-manager-deployment-changes"></a>Bereitstellungsänderungen in Azure Resource Manager

Mithilfe von [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) stellt die Änderungsanalyse in Form von Verlaufsdaten dar, wie sich die Azure-Ressourcen, die Ihre Anwendung hosten, im Laufe der Zeit verändert haben. Die Änderungsanalyse kann beispielsweise Änderungen bei den IP-Konfigurationsregeln, den verwalteten Identitäten und den SSL-Einstellungen erkennen. Wenn also einer Web-App ein Tag hinzugefügt wird, gibt die Änderungsanalyse diese Änderung wieder. Diese Informationen sind so lange verfügbar, wie der `Microsoft.ChangeAnalysis`-Ressourcenanbieter im Azure-Abonnement aktiviert ist.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Änderungen bei Bereitstellung und Konfiguration einer Web-App (Änderungen auf Gastsystemen)

Die Änderungsanalyse erfasst alle vier Stunden den Bereitstellungs- und Konfigurationsstatus der Anwendung. Sie kann z. B. Änderungen in den Umgebungsvariablen der Anwendung erkennen. Das Tool berechnet die Unterschiede und zeigt die Änderungen an. Im Gegensatz zu Resource Manager-Änderungen sind die Informationen zu Änderungen an der Codebereitstellung möglicherweise nicht sofort im Tool verfügbar. Wählen Sie zum Anzeigen der jüngsten Änderungen in der Änderungsanalyse **Jetzt auf Änderungen prüfen** aus.

![Screenshot der Schaltfläche „Jetzt auf Änderungen prüfen“](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Abhängigkeitsänderungen

Änderungen an Ressourcenabhängigkeiten können ebenfalls zu Problemen in einer Web-App führen. Wenn eine Web-App beispielsweise Aufrufe an einen Redis Cache richtet, kann die Web-App-Leistung durch die Redis Cache-SKU beeinträchtigt werden. Zum Erkennen von Änderungen bei Abhängigkeiten überprüft die Änderungsanalyse dem DNS-Eintrag der Web-App. Auf diese Weise identifiziert sie Änderungen in allen App-Komponenten, die Probleme verursachen könnten.
Derzeit werden die folgenden Abhängigkeiten unterstützt:
- Web-Apps
- Azure Storage
- Azure SQL

## <a name="viewing-changes-for-all-resources-in-azure"></a>Anzeigen von Änderungen für alle Ressourcen in Azure
In Azure Monitor gibt es ein eigenständiges Blatt für die Änderungsanalyse, auf dem alle Änderungen mit Ressourcen für Einblicke und Anwendungsabhängigkeiten angezeigt werden.

Suchen Sie im Azure-Portal in der Suchleiste nach „Änderungsanalyse“, um das Blatt zu öffnen.

![Screenshot der Suche nach „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/search-change-analysis.png)

Wählen Sie die Ressourcengruppe und Ressourcen aus, um Änderungen anzuzeigen.

![Screenshot des Blatts „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/change-analysis-standalone-blade.png)

Sie können Ressourcen für Einblicke und zugehörige Abhängigkeiten sehen, die Ihre Anwendung hosten. Diese Sicht wurde anwendungsorientiert entwickelt, damit Entwickler Probleme beheben können.

Derzeit unterstützte Ressourcen:
- Virtual Machines
- VM-Skalierungsgruppe
- Azure-Netzwerkressourcen
- Web-App mit Nachverfolgung von Dateien und Änderungen von Umgebungsvariablen auf Gastsystemen

Zum Senden von Feedback verwenden Sie die entsprechende Schaltfläche auf dem Blatt, oder senden Sie eine E-Mail an changeanalysisteam@microsoft.com. 

![Screenshot der Feedbackschaltfläche auf dem Blatt „Änderungsanalyse“](./media/change-analysis/change-analysis-feedback.png)

## <a name="change-analysis-for-the-web-apps-feature"></a>Änderungsanalyse für die Web-Apps-Funktion

In Azure Monitor ist die Änderungsanalyse auch in die Self-Service-Umgebung **Diagnose und Problembehandlung** integriert. Der Zugriff auf diese Umgebung erfolgt von der Seite **Übersicht** Ihrer App Service-Anwendung.

![Screenshot der Schaltflächen „Übersicht“ und „Diagnose und Problembehandlung“](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Aktivieren der Änderungsanalyse im Tool „Diagnose und Problembehandlung“

1. Wählen Sie **Verfügbarkeit und Leistung** aus.

    ![Screenshot der Problembehandlungsoptionen von „Verfügbarkeit und Leistung“](./media/change-analysis/availability-and-performance.png)

1. Wählen Sie **Anwendungsänderungen** aus. Diese Funktion ist auch unter **Anwendungsabstürze** verfügbar.

   ![Screenshot der Schaltfläche „Anwendungsabstürze“](./media/change-analysis/application-changes.png)

1. Wählen Sie zum Aktivieren der Änderungsanalyse die Option **Jetzt aktivieren** aus.

   ![Screenshot der Optionen von „Anwendungsabstürze“](./media/change-analysis/enable-changeanalysis.png)

1. Aktivieren Sie **Änderungsanalyse**, und wählen Sie **Speichern** aus.

    ![Screenshot der Benutzeroberfläche zum Aktivieren der Änderungsanalyse](./media/change-analysis/change-analysis-on.png)


1. Wählen Sie **Diagnose und Problembehandlung** > **Verfügbarkeit und Leistung** > **Anwendungsabstürze** aus, um auf die Änderungsanalyse zuzugreifen. Im angezeigten Diagramm sind die Arten der Änderungen im Laufe der Zeit und Details zu diesen Änderungen zusammengefasst:

     ![Screenshot der Gegenüberstellung der Änderungen](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Aktivieren der Änderungsanalyse in größerem Umfang

Wenn Ihr Abonnement zahlreiche Web-Apps enthält, wäre das Aktivieren des Diensts auf Web-App-Ebene nicht sehr effizient. Führen Sie das folgende Skript aus, um alle Web-Apps in Ihrem Abonnement zu aktivieren.

Voraussetzungen:
* PowerShell Az-Modul. Folgen Sie den Anweisungen unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0).

Führen Sie das folgende Skript aus:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"


# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```



## <a name="next-steps"></a>Nächste Schritte

- Aktivieren von Application Insights für [Azure App Services-Apps](azure-web-apps.md)
- Aktivieren von Application Insights für [in IIS gehostete virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen](azure-vm-vmss-apps.md)
- Erfahren Sie mehr über [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), der die Änderungsanalyse unterstützt.
