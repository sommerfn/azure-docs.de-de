---
title: Verwenden der Anwendungsänderungsanalyse in Azure Monitor für die Suche nach Web-App-Problemen | Microsoft-Dokumentation
description: Verwenden der Anwendungsänderungsanalyse in Azure Monitor zum Behandeln von Anwendungsproblemen auf Live-Websites in Azure App Service.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: a08fc7d7822b4aeddafb588fdb73e86559ce2b12
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68849166"
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

### <a name="changes-in-web-app-deployment-and-configuration"></a>Änderungen bei Bereitstellung und Konfiguration einer Web-App

Die Änderungsanalyse erfasst alle vier Stunden den Bereitstellungs- und Konfigurationsstatus der Anwendung. Sie kann z. B. Änderungen in den Umgebungsvariablen der Anwendung erkennen. Das Tool berechnet die Unterschiede und zeigt die Änderungen an. Im Gegensatz zu Resource Manager-Änderungen sind die Informationen zu Änderungen an der Codebereitstellung möglicherweise nicht sofort im Tool verfügbar. Wählen Sie zum Anzeigen der jüngsten Änderungen in der Änderungsanalyse **Jetzt auf Änderungen prüfen** aus.

![Screenshot der Schaltfläche „Jetzt auf Änderungen prüfen“](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Abhängigkeitsänderungen

Änderungen an Ressourcenabhängigkeiten können ebenfalls zu Problemen in einer Web-App führen. Wenn eine Web-App beispielsweise Aufrufe an einen Redis Cache richtet, kann die Web-App-Leistung durch die Redis Cache-SKU beeinträchtigt werden. Zum Erkennen von Änderungen bei Abhängigkeiten überprüft die Änderungsanalyse dem DNS-Eintrag der Web-App. Auf diese Weise identifiziert sie Änderungen in allen App-Komponenten, die Probleme verursachen könnten.
Derzeit werden die folgenden Abhängigkeiten unterstützt:
- Web-Apps
- Azure Storage
- Azure SQL


## <a name="change-analysis-for-the-web-apps-feature"></a>Änderungsanalyse für die Web-Apps-Funktion

In Azure Monitor ist die Änderungsanalyse derzeit in die Self-Service-Umgebung **Diagnose und Problembehandlung** integriert. Der Zugriff auf diese Umgebung erfolgt von der Seite **Übersicht** Ihrer App Service-Anwendung.

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

Wenn Ihr Abonnement zahlreiche Web-Apps enthält, wäre das Aktivieren des Diensts auf Web-App-Ebene nicht sehr effizient. Führen Sie in diesem Fall die folgenden alternativen Anweisungen aus.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Registrieren des Änderungsanalyse-Ressourcenanbieters für Ihr Abonnement

1. Registrieren Sie das Änderungsanalyse-Featureflag (Vorschau). Da sich das Featureflag in der Vorschauphase befindet, müssen Sie es registrieren, um es für Ihr Abonnement sichtbar zu machen:

   1. Öffnen Sie [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

      ![Screenshot der Cloud Shell-Änderung](./media/change-analysis/cloud-shell.png)

   1. Ändern Sie den Shelltyp in **PowerShell**.

      ![Screenshot der Cloud Shell-Änderung](./media/change-analysis/choose-powershell.png)

   1. Führen Sie den folgenden PowerShell-Befehl aus:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Registrieren Sie den Änderungsanalyse-Ressourcenanbieter für das Abonnement.

   - Wechseln Sie zu **Abonnements**, und wählen Sie das Abonnement aus, für das Sie den Änderungsdienst aktivieren möchten. Wählen Sie dann Ressourcenanbieter aus:

        ![Screenshot zum Registrieren des Änderungsanalyse-Ressourcenanbieters](./media/change-analysis/register-rp.png)

       - Wählen Sie **Microsoft.ChangeAnalysis** aus. Wählen Sie dann oben auf der Seite die Option **Registrieren** aus.

       - Nachdem Sie den Ressourcenanbieter aktiviert haben, können Sie ein ausgeblendetes Tag für die Web-App festlegen, um Änderungen auf Bereitstellungsebene festzustellen. Zum Festlegen eines ausgeblendeten Tags befolgen Sie die Anweisungen unter **Die Informationen der Änderungsanalyse können nicht abgerufen werden**.

   - Alternativ können Sie den Ressourcenanbieter mithilfe eines PowerShell-Skripts registrieren:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Zum Festlegen eines ausgeblendeten Tags für eine Web-App mithilfe von PowerShell führen Sie den folgenden Befehl aus:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Nachdem Sie das ausgeblendete Tag hinzugefügt haben, müssen Sie möglicherweise noch bis zu vier Stunden warten, bevor Änderungen angezeigt werden. Die Ergebnisse werden mit einer Verzögerung angezeigt, da die Änderungsanalyse Ihre Web-App nur alle vier Stunden scannt. Durch den 4-Stunden-Zeitplan werden die Leistungseinbußen durch den Scan begrenzt.

## <a name="next-steps"></a>Nächste Schritte

- Aktivieren von Application Insights für [Azure App Services-Apps](azure-web-apps.md)
- Aktivieren von Application Insights für [in IIS gehostete virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen](azure-vm-vmss-apps.md)
- Erfahren Sie mehr über [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), der die Änderungsanalyse unterstützt.
