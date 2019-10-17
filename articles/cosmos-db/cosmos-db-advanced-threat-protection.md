---
title: Advanced Threat Protection für Azure Cosmos DB
description: Erfahren Sie, wie Azure Cosmos DB eine Verschlüsselung ruhender Daten bereitstellt und wie diese implementiert wird.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/21/2019
ms.custom: seodec18
ms.author: memildin
author: memildin
manager: rkarlin
ms.openlocfilehash: dca653bfaf625ff31e4dd7f43752219b312779d1
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286759"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db"></a>Advanced Threat Protection für Azure Cosmos DB

Advanced Threat Protection für Azure Cosmos DB ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen. Mit dieser Schutzebene können Sie Bedrohungen begegnen, ohne ein Sicherheitsexperte sein zu müssen, und diese in zentrale Sicherheitsüberwachungssysteme integrieren.

Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert und werden mit Informationen zu der verdächtigen Aktivität und Empfehlungen zur Untersuchung und Beseitigung der Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet.

> [!NOTE]
>
> * Advanced Threat Protection für Azure Cosmos DB ist derzeit nur für die SQL-API verfügbar.
> * Advanced Threat Protection für Azure Cosmos DB ist derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

Für eine vollständige Umgebung zur Untersuchung von Sicherheitswarnungen empfiehlt es sich, die [Diagnoseprotokollierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging) zu aktivieren, die Vorgänge für die Datenbank selbst protokolliert, einschließlich CRUD-Vorgänge für alle Dokumente, Container und Datenbanken.

## <a name="set-up-advanced-threat-protection"></a>Einrichten von Advanced Threat Protection

### <a name="set-up-atp-using-the-portal"></a>Einrichten von ATP unter Verwendung des Portals

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com/).

2. Wählen Sie im Azure Cosmos DB-Konto im Menü **Einstellungen** die Option **Erweiterte Sicherheit** aus.

    ![Einrichten von ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. Führen Sie auf dem Blatt zur Konfiguration von **Erweiterte Sicherheit** folgende Schritte aus:

    * Klicken Sie auf die Option **Advanced Threat Protection**, um sie zu aktivieren (**EIN**).
    * Klicken Sie auf **Speichern**, um die neue oder aktualisierte Advanced Threat Protection-Richtlinie zu speichern.   

### <a name="set-up-atp-using-rest-api"></a>Einrichten von ATP mithilfe der REST-API

Verwenden Sie REST-API-Befehle, um die Advanced Threat Protection-Einstellung für ein bestimmtes Azure Cosmos DB-Konto zu erstellen, zu aktualisieren oder abzurufen.

* [Advanced Threat Protection – Create (Erstellen)](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Advanced Threat Protection – Get (Abrufen)](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Einrichten von ATP mithilfe von Azure PowerShell

Verwenden Sie die folgenden PowerShell-Cmdlets:

* [Enable Advanced Threat Protection (Advanced Threat Protection aktivieren)](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Get Advanced Threat Protection (Advanced Threat Protection abrufen)](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Disable Advanced Threat Protection (Advanced Threat Protection deaktivieren)](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

### <a name="using-azure-resource-manager-templates"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen

Verwenden Sie zum Einrichten von Cosmos DB mit aktiviertem Advanced Threat Protection eine Azure Resource Manager-Vorlage.
Weitere Informationen finden Sie unter [Create a CosmosDB Account with Advanced Threat Protection](https://azure.microsoft.com/en-us/resources/templates/201-cosmosdb-advanced-threat-protection-create-account/) (Erstellen eines Cosmos DB-Kontos mit Advanced Threat Protection).

### <a name="using-azure-policy"></a>Verwenden von Azure Policy

Verwenden Sie eine Azure-Richtlinie, um Advanced Threat Protection für Cosmos DB zu aktivieren.

1. Öffnen Sie die Azure-Seite **Richtlinie – Definitionen**, und suchen Sie die Richtlinie **Deploy Advanced Threat Protection for Cosmos DB** (Advanced Threat Protection für Cosmos DB bereitstellen).

    ![Richtlinie suchen](./media/cosmos-db-advanced-threat-protection/cosmos-db.png) 

1. Klicken Sie auf die Richtlinie **Deploy Advanced Threat Protection for Cosmos DB** (Advanced Threat Protection für Cosmos DB bereitstellen) und dann auf **Zuweisen**.

    ![Wählen Sie „Abonnement“ oder „Gruppe“ aus.](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-policy.png)


1. Klicken Sie im Feld **Bereich** auf die drei Punkte, wählen Sie ein Azure-Abonnement oder eine Ressourcengruppe aus, und klicken Sie dann auf **Auswählen**.

    ![Seite „Richtliniendefinitionen“](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-details.png)


1. Geben Sie die anderen Parameter ein, und klicken Sie auf **Zuweisen**.

## <a name="manage-atp-security-alerts"></a>Verwalten von ATP-Sicherheitswarnungen

Wenn Anomalien bei Azure Cosmos DB-Aktivitäten auftreten, wird eine Sicherheitswarnung mit Informationen zum verdächtigen Sicherheitsereignis ausgelöst. 

 Im Azure Security Center können Sie Ihre aktuellen [Sicherheitswarnungen ](../security-center/security-center-alerts-overview.md) anzeigen und verwalten.  Klicken Sie auf eine Warnung im [Security Center](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0), um für diese Warnung mögliche Ursachen und empfohlene Aktionen zur Untersuchung und Eindämmung der potenziellen Bedrohung anzuzeigen. Die folgende Abbildung zeigt ein Beispiel für Warnungsdetails, die in Security Center bereitgestellt werden.

 ![Bedrohungsdetails](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Außerdem wird eine E-Mail-Benachrichtigung mit den Warnungsdetails und empfohlenen Aktionen gesendet. Die folgende Abbildung zeigt ein Beispiel für eine Warnung per E-Mail.

 ![Warnungsdetails](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>ATP-Warnungen für Cosmos DB

 Eine Liste der Warnungen, die beim Überwachen von Azure Cosmos DB-Konten generiert werden, finden Sie im Abschnitt [Cosmos DB-Warnungen](../security-center/security-center-alerts-data-services.md#cosmos-db) in der Security Center-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Diagnoseprotokollierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging#turn-on-logging-in-the-azure-portal)
* Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
