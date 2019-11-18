---
title: Daten- und Speicherempfehlungen – Azure Security Center
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer Daten und des Azure SQL-Diensts sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2019
ms.author: memildin
ms.openlocfilehash: 21da7c49b158345894ee7fdc164d205bcefe1640
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73663891"
---
# <a name="protect-azure-data-and-storage-services-in-azure-security-center"></a>Schützen von Azure-Daten und Speicherdiensten in Azure Security Center
In diesem Thema erfahren Sie, wie Sie Sicherheitsempfehlungen für Daten und Speicherressourcen anzeigen und implementieren. Azure Security Center hat diese Empfehlungen bei der Analyse des Sicherheitsstatus Ihrer Azure-Ressourcen erkannt.

## <a name="view-your-data-security-information"></a>Anzeigen der Informationen zur Sicherheit Ihrer Daten

1. Klicken Sie im Abschnitt **Resource security hygiene** (Ressourcensicherheit) auf **Data and storage resources** (Daten- und Speicherressourcen).

    ![Daten- und Speicherressourcen](./media/security-center-monitoring/click-data.png)

    Die Seite **Data security** (Datensicherheit) wird mit Empfehlungen für Datenressourcen geöffnet.

    ![Datenressourcen](./media/security-center-monitoring/sql-overview.png)

    Auf der Seite haben Sie folgende Möglichkeiten:

    * Wenn Sie auf die Registerkarte **Übersicht** klicken, werden alle zu bereinigenden Empfehlungen zu Datenressourcen aufgelistet. 
    * Klicken Sie auf jede Registerkarte, und lassen Sie die Empfehlungen je nach Ressourcentyp anzeigen.

    > [!NOTE]
    > Weitere Informationen zur Speicherverschlüsselung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Bereinigen einer Empfehlung für eine Datenressource

1. Klicken Sie auf einer der Registerkarten auf eine Ressource. Die Informationsseite wird geöffnet und die zu bereinigenden Empfehlungen aufgelistet.

    ![Ressourceninformationen](./media/security-center-monitoring/sql-recommendations.png)

2. Klicken Sie auf eine Empfehlung. Die Seite „Empfehlungen“ wird geöffnet und zeigt die **Schritte zur Bereinigung** an, um die Empfehlung zu implementieren.

   ![Schritte zur Bereinigung](./media/security-center-monitoring/remediate1.png)

3. Klicken Sie auf **Aktion ausführen**. Die Seite für die Ressourceneinstellungen wird angezeigt.

    ![Aktivieren der Empfehlung](./media/security-center-monitoring/remediate2.png)

4. Führen Sie die **Schritte zur Bereinigung** aus, und klicken Sie auf **Speichern**.

## <a name="data-and-storage-recommendations"></a>Empfehlungen für Daten und Speicher

|Ressourcentyp|Sicherheitsbewertung|Empfehlung|BESCHREIBUNG|
|----|----|----|----|
|Speicherkonto|20|Sichere Übertragung in Speicherkonten sollte aktiviert werden.|Sichere Übertragung ist eine Option, die erzwingt, dass Ihr Storage-Konto nur Anforderungen von sicheren Verbindungen (HTTPS) akzeptiert. HTTPS stellt eine Authentifizierung zwischen dem Server und dem Dienst sicher, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z. B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|
|Redis|20|Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden|Aktivieren Sie nur Verbindungen über SSL mit Azure Cache for Redis. Durch die Verwendung sicheren Verbindungen wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z.B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|
|SQL|15|Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.|Aktivieren Sie die transparente Datenverschlüsselung, um ruhende Daten zu schützen und Konformitätsanforderungen zu erfüllen.|
|SQL|15|SQL Server-Überwachung sollte aktiviert werden.|Aktivieren Sie die Überwachung für Azure SQL-Server. (Nur Azure SQL-Dienst. Schließt nicht die Ausführung von SQL auf Ihren virtuellen Computern mit ein.)|
|Data Lake Analytics|5|Diagnoseprotokolle in Data Lake Analytics sollten aktiviert werden.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Data Lake Store|5|Diagnoseprotokolle in Azure Data Lake Store sollten aktiviert werden.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|SQL|30|Sicherheitsrisiken in SQL-Datenbanken sollten beseitigt werden.|Die SQL-Sicherheitsrisikobewertung überprüft Ihre Datenbank auf Sicherheitsrisiken und zeigt Abweichungen von bewährten Methoden wie z. B. Fehlkonfigurationen, übermäßige Berechtigungen und ungeschützte vertrauliche Daten an. Durch das Beseitigen der Sicherheitsrisiken kann die Sicherheit Ihrer Datenbank deutlich verbessert werden.|
|SQL|20|Azure AD-Administrator für SQL Server bereitstellen|Stellen Sie einen Azure AD-Administrator für Ihren SQL-Server bereit, um Azure AD-Authentifizierung zu aktivieren. Die Azure AD-Authentifizierung ermöglicht eine vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.|
|Speicherkonto|15|Der Zugriff auf Speicherkonten mit Konfiguration für Firewall und virtuelle Netzwerke sollte eingeschränkt werden.|Überwachen Sie uneingeschränkten Netzwerkzugriff in den Firewalleinstellungen Ihres Speicherkontos. Konfigurieren Sie stattdessen Netzwerkregeln, sodass nur Anwendungen aus zulässigen Netzwerken auf ein Speicherkonto zugreifen können. Um Verbindungen von bestimmten Internetclients oder lokalen Clients zuzulassen, können Sie Zugriff für Datenverkehr aus bestimmten virtuellen Azure-Netzwerken oder an IP-Adressbereiche im öffentlichen Internet gewähren.|
|Speicherkonto|1|Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.|Verwenden Sie den neuen Azure Resource Manager v2 für Ihre Speicherkonten, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, Resource Manager-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung sowie Unterstützung für Markierungen und Ressourcengruppen für eine einfachere Sicherheitsverwaltung.|

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Artikeln:

* [Schützen von Computern und Anwendungen im Azure Security Center](security-center-virtual-machine-protection.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Themen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
