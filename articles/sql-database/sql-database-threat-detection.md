---
title: Konfigurieren von Advanced Threat Protection
description: Advanced Threat Protection erkennt anormale Datenbankaktivitäten, die in einer einzelnen Datenbank oder in einem Pool für elastische Datenbanken auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 08/05/2019
ms.openlocfilehash: 8eb8e4fccc17fe31def671cf6e8edb19d867b244
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822503"
---
# <a name="azure-sql-database-advanced-threat-protection-for-single-or-pooled-databases"></a>Azure SQL-Datenbank Advanced Threat Protection für Einzel- oder Pooldatenbanken

[Advanced Threat Protection](sql-database-threat-detection-overview.md) für Einzel- und Pooldatenbanken identifiziert anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder sie missbräuchlich zu verwenden. Mithilfe von Advanced Threat Protection können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Advanced Threat Protection-Warnungen](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Sie können sich über [E-Mail-Benachrichtigungen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) oder im [Azure-Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

[Advanced Threat Protection](sql-database-threat-detection-overview.md) ist Teil des Angebots [Advanced Data Security](sql-database-advanced-data-security.md) (ADS), bei dem es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen handelt. Der Zugriff auf Advanced Threat Protection und dessen Verwaltung sind über das zentrale SQL ADS-Portal möglich.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Einrichten von Advanced Threat Protection im Azure-Portal

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite des Azure SQL-Datenbank-Servers, den Sie schützen möchten. Wählen Sie in den Sicherheitseinstellungen **Advanced Data Security** aus.
3. Wählen Sie auf der Seite für die **Advanced Data Security**-Konfiguration

   - die Option „Advanced Data Security auf dem Server aktivieren“ aus.
   - Geben Sie unter **Advanced Threat Protection Settings** (Advanced Threat Protection-Einstellungen) im Textfeld **Warnungen senden an** eine Liste von E-Mail-Adressen an, die Sicherheitswarnungen bei der Erkennung von anomalen Datenbankaktivitäten empfangen sollen.
  
   ![Einrichten von Advanced Threat Protection](./media/sql-database-threat-detection/set_up_threat_detection.png)

   > [!NOTE]
   > Preise in Screenshots entsprechen nicht immer dem aktuellen Preis, und es handelt sich um ein Beispiel.

## <a name="set-up-advanced-threat-protection-using-powershell"></a>Einrichten von Advanced Threat Protection mit PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung und Advanced Threat Protection mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Threat Protection](sql-database-threat-detection-overview.md).
- Erfahren Sie mehr über [Advanced Threat Protection in einer verwalteten Instanz](sql-database-managed-instance-threat-detection.md).  
- Erfahren Sie mehr über [Advanced Data Security](sql-database-advanced-data-security.md).
- Weitere Informationen zur [Überwachung](sql-database-auditing.md).
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
- Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank: Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
