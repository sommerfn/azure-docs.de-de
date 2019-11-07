---
title: Umsetzen von Empfehlungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Empfehlungen in Azure Security Center umsetzen, um Ihre Ressourcen zu schützen und Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2019
ms.author: memildin
ms.openlocfilehash: 72212bf3687f891d3a04fe0ecddf415af72505a6
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73520718"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Umsetzen von Empfehlungen in Azure Security Center

Empfehlungen enthalten Vorschläge dazu, wie Sie Ihre Ressourcen besser schützen können. Sie implementieren eine Empfehlung, indem Sie die in der Empfehlung beschriebenen Schritte zur Bereinigung ausführen. 

## Schritte zur Bereinigung<a name="remediation-steps"></a>

Nach der Überprüfung aller Empfehlungen müssen Sie entscheiden, welche zuerst umgesetzt werden soll. Zur Priorisierung empfiehlt sich die Verwendung der [Secure Score-Auswirkung](security-center-recommendations.md#monitor-recommendations).

1. Klicken Sie in der Liste auf die gewünschte Empfehlung.

1. Befolgen Sie die Anweisungen im Abschnitt **Schritte zur Bereinigung**. Jede Empfehlung beinhaltet spezifische Anweisungen. Im folgenden Screenshot werden die Schritte angezeigt, die ausgeführt werden müssen, um Anwendungen so zu konfigurieren, dass der Datenverkehr nur über HTTPS zulässig ist.

    ![Empfehlungsdetails](./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png)

1. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, in der Sie darüber informiert werden, ob die Umstellung erfolgreich durchgeführt wurde.

## Schnelle Problembehebung<a name="one-click"></a>

Mithilfe der schnellen Problembehebung können Sie eine Empfehlung schnell für mehrere Ressourcen umsetzen. Diese Option ist nur für bestimmte Empfehlungen verfügbar. Die schnelle Problembehebung vereinfacht das Umsetzen von Empfehlungen und ermöglicht Ihnen eine schnelle Verbesserung Ihrer Sicherheitsbewertung und erhöht die Sicherheit in Ihrer Umgebung.

So implementieren Sie eine schnelle Problembehebung:

1. Klicken Sie in der Liste der Empfehlungen, die mit **Schnelle Problembehebung** gekennzeichnet sind, auf die entsprechende Empfehlung.

    [![Wählen Sie „Schnelle Problembehebung“ aus.](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png)](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png#lightbox)

1. Wählen Sie auf der Registerkarte **Fehlerhafte Ressourcen** die Ressourcen aus, für die Sie die Empfehlung implementieren möchten, und klicken Sie auf **Wartung ausführen**. 

    > [!NOTE]
    > Einige der aufgelisteten Ressourcen sind möglicherweise deaktiviert, da Sie nicht über die entsprechenden Berechtigungen zum Ändern verfügen.

1. Lesen Sie sich im Bestätigungsdialogfeld die Details und Auswirkungen der Korrektur durch. 

    ![Schnelle Problembehebung](./media/security-center-remediate-recommendations/security-center-one-click-fix-view.png)

    > [!NOTE]
    > Die Auswirkungen sind in dem grauen Feld im Fenster **Ressourcen warten** aufgelistet, das nach dem Klicken auf **Wartung ausführen** geöffnet wird. Hier ist angegeben, welche Änderungen auftreten, wenn Sie mit der schnellen Problembehebung fortfahren.

1. Fügen Sie ggf. die relevanten Parameter ein, und genehmigen Sie die Korrektur.

    > [!NOTE]
    > Es kann einige Minuten dauern, bis nach der Korrektur die Ressourcen auf der Registerkarte **Fehlerfreie Ressourcen** angezeigt werden. Zum Anzeigen der Korrekturaktionen sehen Sie sich das [Aktivitätsprotokoll](#activity-log) an.

1. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, in der Sie darüber informiert werden, ob die Umstellung erfolgreich durchgeführt wurde.

## Protokollierung der schnellen Problembehebung im Aktivitätsprotokoll <a name="activity-log"></a>

Beim Korrekturvorgang wird ein Vorlagenbereitstellungs- oder REST PATCH-API-Aufruf verwendet, um die Konfiguration auf die Ressource anzuwenden. Diese Vorgänge werden im [Azure-Aktivitätsprotokoll](../azure-resource-manager/resource-group-audit.md) protokolliert.


## <a name="recommendations-with-quick-fix-remediation"></a>Empfehlungen mit schneller Problembehebung

|Empfehlung|Auswirkung|
|---|---|
|Überwachung für SQL-Server aktivieren|Durch diese Aktion wird die SQL-Überwachung für diese Server und deren Datenbanken aktiviert. <br>**Hinweis**: <ul><li>Für jede Region der ausgewählten SQL-Server wird ein Speicherkonto zum Speichern von Überwachungsprotokollen erstellt und von allen Servern in dieser Region gemeinsam genutzt.</li><li>Um eine ordnungsgemäße Überwachung sicherzustellen, dürfen Sie die Ressourcengruppe oder die Speicherkonten weder löschen noch umbenennen.</li></ul>|
|Advanced Data Security für verwaltete SQL-Instanzen aktivieren|Durch diese Aktion wird SQL Advanced Data Security (ADS) für die ausgewählten verwalteten SQL-Instanzen aktiviert. <br>**Hinweis**: <ul><li>Für jede Region und Ressourcengruppe der ausgewählten verwalteten SQL-Instanzen wird ein Speicherkonto zum Speichern von Überprüfungsergebnissen erstellt und von allen Instanzen in dieser Region gemeinsam genutzt.</li><li> ADS wird mit 15 US-Dollar pro verwalteter SQL-Instanz abgerechnet.</li></ul>|
|Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren|Durch diese Aktion wird die SQL-Sicherheitsrisikobewertung für die ausgewählten verwalteten SQL-Instanzen aktiviert. <br>**Hinweis**:<ul><li>Die SQL-Sicherheitsrisikobewertung ist Teil des SQL-Pakets für Advanced Data Security (ADS). Wenn ADS noch nicht aktiviert ist, wird diese Funktion automatisch für die verwaltete Instanz aktiviert.</li><li>Für jede Region und Ressourcengruppe der ausgewählten verwalteten SQL-Instanzen wird ein Speicherkonto zum Speichern von Überprüfungsergebnissen erstellt und von allen Instanzen in dieser Region gemeinsam genutzt.</li><li>ADS wird mit 15 US-Dollar pro SQL-Server abgerechnet.</li></ul>||
|Advanced Data Security für SQL-Server aktivieren|Durch diese Aktion wird Advanced Data Security (ADS) für diese ausgewählten Server und deren Datenbanken aktiviert. <br>**Hinweis**:<ul><li>Für jede Region und Ressourcengruppe der ausgewählten SQL-Server wird ein Speicherkonto zum Speichern von Überprüfungsergebnissen erstellt und von allen Servern in dieser Region gemeinsam genutzt.</li><li>ADS wird mit 15 US-Dollar pro SQL-Server abgerechnet.</li></ul>||
|Bewertung von Sicherheitsrisiken für SQL-Server aktivieren|Durch diese Aktion wird die SQL-Sicherheitsrisikobewertung für diese ausgewählten verwalteten Server und deren Datenbanken aktiviert. <br>**Hinweis**:<ul><li>Die SQL-Sicherheitsrisikobewertung ist Teil des SQL-Pakets für Advanced Data Security (ADS). Wenn ADS noch nicht aktiviert ist, wird diese Funktion automatisch für den SQL-Server aktiviert.</li><li>Für jede Region und Ressourcengruppe der ausgewählten SQL-Server wird ein Speicherkonto zum Speichern von Überprüfungsergebnissen erstellt und von allen Instanzen in dieser Region gemeinsam genutzt.</li><li>ADS wird mit 15 US-Dollar pro SQL-Server abgerechnet.</li></ul>||
|Transparent Data Encryption für SQL-Datenbanken aktivieren|Durch diese Aktion wird Transparent Data Encryption (TDE) von SQL-Datenbank für die ausgewählten Datenbanken aktiviert. <br>**Hinweis**: Standardmäßig werden vom Dienst verwaltete TDE-Schlüssel verwendet. 
|Sichere Übertragung in Speicherkonten sollte aktiviert werden.|Durch diese Aktion wird die Sicherheit des Speicherkontos so aktualisiert, dass nur Anforderungen von sicheren Verbindungen (HTTPS) zugelassen werden. <br>**Hinweis**:<ul><li>Alle Anforderungen, die HTTP verwenden, werden abgelehnt.</li><li>Wenn Sie den Azure Files-Dienst verwenden, schlägt jede Verbindung ohne Verschlüsselung fehl. Dies schließt auch Szenarien ein, in denen SMB 2.1, SMB 3.0 ohne Verschlüsselung und einige Versionen des Linux SMB-Clients verwendet werden. Weitere Informationen</li></ul>|
|Zugriff auf Webanwendung nur über HTTPS gestatten|Durch diese Aktion wird der gesamte Datenverkehr auf den ausgewählten Ressourcen von HTTP zu HTTPS umgeleitet. <br>**Hinweis**:<ul><li>Ein HTTPS-Endpunkt, der nicht über ein SSL-Zertifikat verfügt, wird im Browser mit einem „Datenschutzfehler“ angezeigt. Daher müssen Benutzer mit einer benutzerdefinierten Domäne sicherstellen, dass sie ein SSL-Zertifikat eingerichtet haben.</li><li>Vergewissern Sie sich, dass die Firewalls für Pakete und Webanwendungen, die den App Service schützen, die Weiterleitung über HTTPS-Sitzungen zulassen.</li></ul>|
|Zugriff auf Funktions-App nur über HTTPS gestatten|Durch diese Aktion wird der gesamte Datenverkehr auf den ausgewählten Ressourcen von HTTP zu HTTPS umgeleitet. <br>**Hinweis**:<ul><li>Ein HTTPS-Endpunkt, der nicht über ein SSL-Zertifikat verfügt, wird im Browser mit einem „Datenschutzfehler“ angezeigt. Daher müssen Benutzer mit einer benutzerdefinierten Domäne sicherstellen, dass sie ein SSL-Zertifikat eingerichtet haben.</li><li>Vergewissern Sie sich, dass die Firewalls für Pakete und Webanwendungen, die den App Service schützen, die Weiterleitung über HTTPS-Sitzungen zulassen.</li></ul>|
|Auf API-Apps sollte nur über HTTPS zugegriffen werden können|Durch diese Aktion wird der gesamte Datenverkehr auf den ausgewählten Ressourcen von HTTP zu HTTPS umgeleitet. <br>**Hinweis**:<ul><li>Ein HTTPS-Endpunkt, der nicht über ein SSL-Zertifikat verfügt, wird im Browser mit einem „Datenschutzfehler“ angezeigt. Daher müssen Benutzer mit einer benutzerdefinierten Domäne sicherstellen, dass sie ein SSL-Zertifikat eingerichtet haben.</li><li>Vergewissern Sie sich, dass die Firewalls für Pakete und Webanwendungen, die den App Service schützen, die Weiterleitung über HTTPS-Sitzungen zulassen.</li></ul>|
|Remotedebuggen muss für Webanwendung deaktiviert werden|Durch diese Aktion wird das Remotedebuggen deaktiviert.|
|Remotedebuggen sollte für Funktions-Apps deaktiviert werden|Durch diese Aktion wird das Remotedebuggen deaktiviert.|
|Remotedebuggen für API-App deaktivieren|Durch diese Aktion wird das Remotedebuggen deaktiviert.|
|Nicht jeder Ressource den Zugriff auf Ihre Webanwendung über CORS gestatten|Diese Aktion blockiert den Zugriff auf Ihre Webanwendung durch andere Domänen. Um bestimmte Domänen zuzulassen, geben Sie diese im Feld „Zulässige Ursprünge“ ein (durch Kommas getrennt). <br>**Hinweis**: Wenn das Feld leer bleibt, werden alle ursprungsübergreifenden Aufrufe blockiert. Titel des Parameterfelds: „Zulässige Ursprünge“|
|Nicht jeder Ressource den Zugriff auf Ihre Funktions-App über CORS gestatten|Diese Aktion blockiert den Zugriff auf Ihre Funktionsanwendung durch andere Domänen. Um bestimmte Domänen zuzulassen, geben Sie diese im Feld „Zulässige Ursprünge“ ein (durch Kommas getrennt). <br>**Hinweis**: Wenn das Feld leer bleibt, werden alle ursprungsübergreifenden Aufrufe blockiert. Titel des Parameterfelds: „Zulässige Ursprünge“|
|Nicht jeder Ressource den Zugriff auf Ihre API-APP über CORS gestatten|Diese Aktion blockiert den Zugriff auf Ihre API-Anwendung durch andere Domänen. Um bestimmte Domänen zuzulassen, geben Sie diese im Feld „Zulässige Ursprünge“ ein (durch Kommas getrennt). <br>**Hinweis**: Wenn das Feld leer bleibt, werden alle ursprungsübergreifenden Aufrufe blockiert. Titel des Parameterfelds: „Zulässige Ursprünge“|
|Überwachungs-Agent auf virtuellen Computern aktivieren|Durch diese Aktion wird ein Überwachungs-Agent auf den ausgewählten virtuellen Computern installiert. Wählen Sie einen Arbeitsbereich aus, an den der Agent Berichte übermitteln soll.<ul><li>Wenn die Updaterrichtlinie auf automatisch festgelegt ist, wird sie für neue vorhandene Instanzen bereitgestellt.</li><li>Wenn die Updaterichtlinie auf manuell festgelegt ist und Sie den Agent für vorhandene Instanzen installieren möchten, aktivieren Sie das Kontrollkästchen. [Weitere Informationen](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set)</li></ul>|
|Diagnoseprotokolle in Key Vault aktivieren|Durch diese Aktion werden Diagnoseprotokolle für Schlüsseltresore aktiviert. Diagnoseprotokolle und Metriken werden im ausgewählten Arbeitsbereich gespeichert.|
|Diagnoseprotokolle in Service Bus aktivieren|Durch diese Aktion werden Diagnoseprotokolle in Service Bus aktiviert. Diagnoseprotokolle und Metriken werden im ausgewählten Arbeitsbereich gespeichert.|

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Empfehlungen in Security Center umsetzen. Weitere Informationen zu Security Center finden Sie in den folgenden Themen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.