---
title: Konfigurieren von und Zugreifen auf Überwachungsprotokolle für Azure Database for MySQL im Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie die Überwachungsprotokolle in Azure Database for MySQL im Azure-Portal konfigurieren und darauf zugreifen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: a2f44b52c6d34adb1bebf666ff8453b17f7778a5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448486"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Konfigurieren von und Zugreifen auf Überwachungsprotokolle im Azure-Portal

Sie können im Azure-Portal die [Azure Database for MySQL-Überwachungsprotokolle](concepts-audit-logs.md) und -Diagnoseeinstellungen konfigurieren.

> [!IMPORTANT]
> Diese Überwachungsprotokollfunktion ist derzeit in der Vorschauphase.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:

- [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Konfigurieren der Überwachungsprotokollierung

Aktivieren und konfigurieren Sie die Überwachungsprotokollierung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie Ihren Server für Azure Database for MySQL aus.

1. Wählen Sie auf der Randleiste im Abschnitt **Einstellungen** den Eintrag **Serverparameter** aus.
    ![Serverparameter](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Ändern Sie den Parameter **audit_log_enabled** in ON.
    ![Aktivieren von Überwachungsprotokollen](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Wählen Sie die zu protokollierenden [Ereignistypen](concepts-audit-logs.md#configure-audit-logging) aus, indem Sie den Parameter **audit_log_events** aktualisieren.
    ![Überwachungsprotokollereignisse](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. Fügen Sie alle MySQL-Benutzer hinzu, die von der Protokollierung ausgeschlossen werden sollen, indem Sie den Parameter **audit_log_exclude_users** aktualisieren. Geben Sie Benutzer an, indem Sie ihre MySQL-Benutzernamen bereitstellen.
    ![Vom Überwachungsprotokoll auszuschließende Benutzer](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Nachdem Sie die Parameter geändert haben, klicken Sie auf **Speichern**. Sie können Ihre Änderungen auch **verwerfen**.
    ![Speichern](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

1. Wählen Sie auf der Randleiste im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** aus.

1. Klicken Sie auf „+ Diagnoseeinstellung hinzufügen“ ![Diagnoseeinstellung hinzufügen](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Geben Sie einen Namen für die Diagnoseeinstellung ein.

1. Geben Sie an, an welche Datensenken die Überwachungsprotokolle gesendet werden sollen (Speicherkonto, Event Hub und/oder Log Analytics-Arbeitsbereich).

1. Wählen Sie als Protokolltyp „MySqlAuditLogs“ aus.
![Konfigurieren der Diagnoseeinstellung](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Nachdem Sie die Datensenken für die Überwachungsprotokolle konfiguriert haben, können Sie auf **Speichern** klicken.
![Speichern der Diagnoseeinstellung](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Greifen Sie auf die Überwachungsprotokolle zu, indem Sie sie in den von Ihnen konfigurierten Datensenken einsehen. Es kann bis zu 10 Minuten dauern, bis die Protokolle angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Überwachungsprotokollen](concepts-audit-logs.md) in Azure Database for MySQL.