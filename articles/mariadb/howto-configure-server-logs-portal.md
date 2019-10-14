---
title: Konfigurieren von Serverprotokollen in Azure Database for MariaDB und Zugreifen auf diese über das Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal die Serverprotokolle in Azure Database for MariaDB konfigurieren und auf diese zugreifen.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c7e95521e503ad2d92bf13ae7e410b71ed6dfcd3
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71844676"
---
# <a name="configure-and-access-server-logs-from-the-azure-portal"></a>Konfigurieren von und Zugreifen auf Serverprotokolle über das Azure-Portal

Sie können die [Protokolle für langsame Abfragen für Azure Database for MariaDB](concepts-server-logs.md) im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Für die Schritte in diesem Artikel ist es erforderlich, dass Sie über [Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md) verfügen.

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das Protokoll für langsame Abfragen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie Ihren Server für Azure Database for MariaDB aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. 
   ![Screenshot der Optionen für Serverprotokolle](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Um die Serverparameter anzuzeigen, wählen Sie **Klicken Sie hier, um Protokolle zu aktivieren und Protokollparameter zu konfigurieren** aus.

5. Ändern Sie die Parameter, die Sie anpassen müssen, und legen Sie dabei **slow_query_log** auf **ON** (EIN) fest. Alle in dieser Sitzung vorgenommenen Änderungen werden in violett hervorgehoben. 

   Wählen Sie **Speichern** aus, nachdem Sie die Parameter geändert haben. Sie können Ihre Änderungen auch verwerfen.

   ![Screenshot der Optionen für Serverparameter](./media/howto-configure-server-logs-portal/3-save-discard.png)

Von der Seite **Serverparameter** können Sie zur Liste der Protokolle zurückkehren, indem Sie die Seite schließen.

## <a name="view-list-and-download-logs"></a>Anzeigen der Liste und Herunterladen von Protokollen
Nachdem die Protokollierung begonnen hat, können Sie eine Liste der verfügbaren Protokolle für langsame Abfragen anzeigen und einzelne Protokolldateien herunterladen. 

1. Öffnen Sie das Azure-Portal.

2. Wählen Sie Ihren Server für Azure Database for MariaDB aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. Die Seite zeigt eine Liste der Protokolldateien an.

   ![Screenshot der Seite „Serverprotokolle“ mit hervorgehobener Liste der Protokolle](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > Die Namenskonvention des Protokolls ist **mysql-slow-<Name_Ihres_Servers>-jjjjmmtthh.log**. Das im Dateinamen verwendete Datum und die Uhrzeit geben den Zeitpunkt an, zu dem das Protokoll ausgestellt wurde. Die Protokolldateien werden alle 24 Stunden oder bei Erreichen einer Größe von 7,5 GB rotiert.

4. Verwenden Sie bei Bedarf das Suchfeld, um schnell ein spezifisches Protokoll basierend auf Datum und Uhrzeit einzugrenzen. Die Suche erfolgt anhand des Namens des Protokolls.

5. Um einzelne Protokolldateien herunterzuladen, wählen Sie das Pfeilsymbol nach unten neben den einzelnen Protokolldateien in der Tabellenzeile aus.

   ![Screenshot der Seite „Serverprotokolle“ mit hervorgehobenem Pfeilsymbol nach unten](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

1. Wählen Sie auf der Randleiste im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** > **Diagnoseeinstellung hinzufügen** aus.

   ![Screenshot der Optionen für Diagnoseeinstellungen](./media/howto-configure-server-logs-portal/add-diagnostic-setting.png)

1. Geben Sie einen Namen für die Diagnoseeinstellung ein.

1. Geben Sie an, an welche Datensenken die langsamen Abfrageprotokolle gesendet werden sollen (Speicherkonto, Event Hub oder Log Analytics-Arbeitsbereich).

1. Wählen Sie als Protokolltyp **MySqlSlowLogs** aus.
![Screenshot der Konfigurationsoptionen für Diagnoseeinstellungen](./media/howto-configure-server-logs-portal/configure-diagnostic-setting.png)

1. Nachdem Sie die Datensenken für die langsamen Abfrageprotokolle konfiguriert haben, wählen Sie **Speichern** aus.
![Screenshot der Konfigurationsoptionen für Diagnoseeinstellungen mit hervorgehobener Option „Speichern“](./media/howto-configure-server-logs-portal/save-diagnostic-setting.png)

1. Greifen Sie auf die langsamen Abfrageprotokolle zu, indem Sie sie in den von Ihnen konfigurierten Datensenken einsehen. Es kann bis zu 10 Minuten dauern, bis die Protokolle angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Konfigurieren von Protokollen und Zugreifen auf Protokolle für langsame Abfragen mithilfe der Azure CLI](howto-configure-server-logs-cli.md), um zu erfahren, wie Protokolle für langsame Abfragen programmgesteuert heruntergeladen werden können.
- Erfahren Sie mehr über [Protokolle für langsame Abfragen](concepts-server-logs.md) in Azure Database for MariaDB.
- Weitere Informationen zu den Parameterdefinitionen und der Protokollierung finden Sie in der MariaDB-Dokumentation unter [Protokolle](https://mariadb.com/kb/en/library/slow-query-log-overview/).