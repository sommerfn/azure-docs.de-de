---
title: Konfigurieren von Advanced Threat Protection – Verwaltete Instanz
description: Advanced Threat Protection identifiziert anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank in einer verwalteten Instanz hinweisen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 69292a934af8b8777f11ab58ed3fe306abf8b408
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822547"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>Konfigurieren von Advanced Threat Protection in einer verwalteten Azure SQL-Datenbank-Instanz

[Advanced Threat Protection](sql-database-threat-detection-overview.md) für eine [verwaltete Instanz](sql-database-managed-instance-index.yml) identifiziert anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder sie missbräuchlich zu verwenden. Mithilfe von Advanced Threat Protection können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Advanced Threat Protection-Warnungen](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Sie können sich über [E-Mail-Benachrichtigungen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) oder im [Azure-Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

[Advanced Threat Protection](sql-database-threat-detection-overview.md) ist Teil des Angebots [Advanced Data Security](sql-database-advanced-data-security.md) (ADS), bei dem es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen handelt. Der Zugriff auf Advanced Threat Protection und dessen Verwaltung sind über das zentrale SQL ADS-Portal möglich.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Einrichten von Advanced Threat Protection im Azure-Portal

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite der verwalteten Instanz, die geschützt werden soll. Wählen Sie auf der Seite **Einstellungen** die Einstellung **Advanced Data Security** aus.
3. Auf der Konfigurationsseite „Advanced Data Security“ müssen Sie
   - „Advanced Data Security“ **AKTIVIEREN**.
   - Konfigurieren Sie die **Liste der E-Mail-Empfänger**, die bei Erkennung anomaler Datenbankaktivitäten Sicherheitswarnungen erhalten sollen.
   - Wählen Sie das **Azure Storage-Konto** aus, in dem Überwachungsdatensätze zu anomalen Bedrohungen gespeichert werden.
   - Wählen Sie die **Advanced Threat Protection-Typen** aus, die Sie konfigurieren möchten. Erfahren Sie mehr über [Advanced Threat Protection-Warnungen](sql-database-threat-detection-overview.md).
4. Klicken Sie auf **Speichern**, um die neue oder aktualisierte Advanced Data Security-Richtlinie zu speichern.

   ![Erweiterter Schutz vor Bedrohungen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > Preise in Screenshots entsprechen nicht immer dem aktuellen Preis, und es handelt sich um ein Beispiel.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Threat Protection](sql-database-threat-detection-overview.md).
- Informationen zu verwalteten Instanzen finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)
- Erfahren Sie mehr über [Advanced Threat Protection für eine einzelne Datenbank](sql-database-threat-detection.md).
- Weitere Informationen zur [Überwachung einer verwalteten Instanz](https://go.microsoft.com/fwlink/?linkid=869430).
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
