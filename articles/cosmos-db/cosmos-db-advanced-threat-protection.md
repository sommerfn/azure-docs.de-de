---
title: Advanced Threat Protection für Azure Cosmos DB
description: Erfahren Sie, wie Azure Cosmos DB eine Verschlüsselung ruhender Daten bereitstellt und wie diese implementiert wird.
author: monhaber
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: v-mohabe
ms.custom: seodec18
ms.openlocfilehash: 0504da45cbbd60629954d3e3ca3230e05761c1d6
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640379"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db"></a>Advanced Threat Protection für Azure Cosmos DB

Advanced Threat Protection für Azure Cosmos DB ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen. Mit dieser Schutzebene können Sie Bedrohungen begegnen, ohne ein Sicherheitsexperte sein zu müssen, und diese in zentrale Sicherheitsüberwachungssysteme integrieren.

Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind in  [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert und werden mit Informationen zu der verdächtigen Aktivität und Empfehlungen zur Untersuchung und Beseitigung der Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet.

> [!NOTE]
>
> * Advanced Threat Protection für Azure Cosmos DB ist derzeit nur für die SQL-API verfügbar.
> * Advanced Threat Protection für Azure Cosmos DB ist derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

Für eine vollständige Umgebung zur Untersuchung von Sicherheitswarnungen empfiehlt es sich, die [Diagnoseprotokollierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging) zu aktivieren, die Vorgänge für die Datenbank selbst protokolliert, einschließlich CRUD-Vorgänge für alle Dokumente, Container und Datenbanken.

## <a name="set-up-advanced-threat-protection"></a>Einrichten von Advanced Threat Protection

### <a name="set-up-atp-using-the-portal"></a>Einrichten von ATP unter Verwendung des Portals

1. Starten Sie das Azure-Portal unter   [https://portal.azure.com](https://portal.azure.com/).

2. Wählen Sie im Azure Cosmos DB-Konto im Menü **Einstellungen** die Option **Erweiterte Sicherheit** aus.

    ![Einrichten von ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. Führen Sie bei der Konfiguration auf dem Blatt  **Erweiterte Sicherheit** folgende Schritte aus:

    * Klicken Sie auf die Option **Advanced Threat Protection**, um sie zu aktivieren.
    * Klicken Sie auf  **Speichern** , um die neue oder aktualisierte Advanced Threat Protection-Richtlinie zu speichern.   

### <a name="set-up-atp-using-rest-api"></a>Einrichten von ATP mithilfe der REST-API

Verwenden Sie REST-API-Befehle, um die Advanced Threat Protection-Einstellung für ein bestimmtes Azure Cosmos DB-Konto zu erstellen, zu aktualisieren oder abzurufen.

* [Advanced Threat Protection – Create (Erstellen)](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Advanced Threat Protection – Get (Abrufen)](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Einrichten von ATP mithilfe von Azure PowerShell

Verwenden Sie die folgenden PowerShell-Cmdlets:

* [Enable Advanced Threat Protection (Advanced Threat Protection aktivieren)](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Get Advanced Threat Protection (Advanced Threat Protection abrufen)](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Disable Advanced Threat Protection (Advanced Threat Protection deaktivieren)](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

## <a name="manage-atp-security-alerts"></a>Verwalten von ATP-Sicherheitswarnungen

Wenn Anomalien bei Azure Cosmos DB-Aktivitäten auftreten, wird eine Sicherheitswarnung mit Informationen zum verdächtigen Sicherheitsereignis ausgelöst. 

 Im Azure Security Center können Sie Ihre aktuellen [Sicherheitswarnungen ](../security-center/security-center-alerts-overview.md) anzeigen und verwalten.  Klicken Sie auf eine Warnung im [Security Center](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0), um für diese Warnung mögliche Ursachen und empfohlene Aktionen zur Untersuchung und Eindämmung der potenziellen Bedrohung anzuzeigen. Die folgende Abbildung zeigt ein Beispiel für Warnungsdetails, die in Security Center bereitgestellt werden.

 ![Bedrohungsdetails](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Außerdem wird eine E-Mail-Benachrichtigung mit den Warnungsdetails und empfohlenen Aktionen gesendet. Die folgende Abbildung zeigt ein Beispiel für eine Warnung per E-Mail.

 ![Warnungsdetails](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>ATP-Warnungen für Cosmos DB

 Eine Liste der Warnungen, die beim Überwachen von Azure Cosmos DB-Konten generiert werden, finden Sie im Abschnitt [Cosmos DB-Warnungen](../security-center/security-center-alerts-data-services.md#cosmos-db) in der Security Center-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur  [Diagnoseprotokollierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging#turn-on-logging-in-the-azure-portal)
* Weitere Informationen zu  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)