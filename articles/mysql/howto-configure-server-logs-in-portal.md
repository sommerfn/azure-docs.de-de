---
title: Konfigurieren von Protokollen für langsame Abfragen für Azure Database for MySQL und Zugreifen auf diese Protokolle im Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie die Protokolle für langsame Abfragen in Azure Database for MySQL im Azure-Portal konfigurieren und aus dem Portal auf die Protokolle zugreifen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: b3986c19ec008437f3230b3674ce60d1dfba2024
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703435"
---
# <a name="configure-and-access-slow-query-logs-in-the-azure-portal"></a>Konfigurieren der und Zugreifen auf die Protokolle für langsame Abfragen im Azure-Portal

Sie können die [Protokolle für langsame Abfragen für Azure Database for MySQL](concepts-server-logs.md) im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das MySQL-Protokoll für langsame Abfragen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. 
   ![„Serverprotokolle“ auswählen und zum Konfigurieren klicken](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Wählen Sie die Überschrift **Klicken Sie hier, um Protokolle zu aktivieren und Protokollparameter zu konfigurieren** aus, um die Serverparameter anzuzeigen.

5. Ändern Sie die Parameter, die Sie anpassen müssen. Alle in dieser Sitzung vorgenommenen Änderungen werden in violett hervorgehoben. 

   Nachdem Sie die Parameter geändert haben, klicken Sie auf **Speichern**. Sie können Ihre Änderungen auch **verwerfen**.

   ![Auf „Speichern“ oder „Verwerfen“ klicken](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Kehren Sie zur Liste der Protokolle zurück, indem Sie auf der Seite **Serverparameter** auf die **Schaltfläche „Schließen“** (X-Symbol) klicken.

## <a name="view-list-and-download-logs"></a>Anzeigen der Liste und Herunterladen von Protokollen
Sobald die Protokollierung beginnt, können Sie eine Liste der verfügbaren Protokolle für langsame Abfragen anzeigen und einzelne Protokolldateien im Bereich „Serverprotokolle“ herunterladen.

1. Öffnen Sie das Azure-Portal.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. Die Seite zeigt wie im folgenden Beispiel eine Liste der Protokolldateien an:

   ![Liste der Protokolle](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Die Namenskonvention des Protokolls ist **mysql-slow-<Name_Ihres_Servers>-jjjjmmtthh.log**. Das im Dateinamen verwendete Datum und die Uhrzeit geben den Zeitpunkt an, zu dem das Protokoll ausgestellt wurde. Die Protokolldateien werden alle 24 Stunden oder bei Erreichen einer Größe von 7,5 GB rotiert.

4. Verwenden Sie bei Bedarf das **Suchfeld**, um schnell ein spezifisches Protokoll basierend auf Datum/Uhrzeit einzugrenzen. Die Suche erfolgt anhand des Namens des Protokolls.

5. Laden Sie einzelne Protokolldateien wie gezeigt über die Schaltfläche **Download** (nach unten gerichtetes Pfeilsymbol) neben jeder Protokolldatei in der Tabellenzeile herunter:

   ![Klicken Sie auf das Symbol „Download“.](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

1. Wählen Sie auf der Randleiste im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** aus.

1. Klicken Sie auf „+ Diagnoseeinstellung hinzufügen“ ![Diagnoseeinstellung hinzufügen](./media/howto-configure-server-logs-in-portal/add-diagnostic-setting.png)

1. Geben Sie einen Namen für die Diagnoseeinstellung ein.

1. Geben Sie an, an welche Datensenken die langsamen Abfrageprotokolle gesendet werden sollen (Speicherkonto, Event Hub und/oder Log Analytics-Arbeitsbereich).

1. Wählen Sie als Protokolltyp „MySqlSlowLogs“ aus.
![Konfigurieren der Diagnoseeinstellung](./media/howto-configure-server-logs-in-portal/configure-diagnostic-setting.png)

1. Nachdem Sie die Datensenken für die langsamen Abfrageprotokolle konfiguriert haben, können Sie auf **Speichern** klicken.
![Speichern der Diagnoseeinstellung](./media/howto-configure-server-logs-in-portal/save-diagnostic-setting.png)

1. Greifen Sie auf die langsamen Abfrageprotokolle zu, indem Sie sie in den von Ihnen konfigurierten Datensenken einsehen. Es kann bis zu 10 Minuten dauern, bis die Protokolle angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Konfigurieren der und Zugreifen auf die Protokolle für langsame Abfragen mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md), um zu erfahren, wie Protokolle für langsame Abfragen programmgesteuert heruntergeladen werden können.
- Erfahren Sie mehr über [Protokolle für langsame Abfragen in Azure Database for MySQL](concepts-server-logs.md).
- Weitere Informationen zu den Parameterdefinitionen und der MySQL-Protokollierung finden Sie in der MySQL-Dokumentation unter [Protokolle](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).