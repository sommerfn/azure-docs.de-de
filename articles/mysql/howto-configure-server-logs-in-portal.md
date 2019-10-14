---
title: Konfigurieren von Protokollen für langsame Abfragen in Azure Database for MySQL und Zugreifen auf diese Protokolle im Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie die Protokolle für langsame Abfragen in Azure Database for MySQL im Azure-Portal konfigurieren und aus dem Portal auf die Protokolle zugreifen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: 7eeeb729973e484e9acb26f3ac8cc42693f72eea
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841591"
---
# <a name="configure-and-access-slow-query-logs-from-the-azure-portal"></a>Konfigurieren von und Zugreifen auf Protokolle für langsame Abfragen im Azure-Portal

Sie können die [Protokolle für langsame Abfragen für Azure Database for MySQL](concepts-server-logs.md) im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Für die Schritte in diesem Artikel ist es erforderlich, dass Sie über [Azure Database for MySQL Server](quickstart-create-mysql-server-database-using-azure-portal.md) verfügen.

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das MySQL-Protokoll für langsame Abfragen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. 
   ![Screenshot der Optionen für Serverprotokolle](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Um die Serverparameter anzuzeigen, wählen Sie **Klicken Sie hier, um Protokolle zu aktivieren und Protokollparameter zu konfigurieren** aus.

5. Ändern Sie die Parameter, die Sie anpassen müssen. Alle in dieser Sitzung vorgenommenen Änderungen werden in violett hervorgehoben. 

   Wählen Sie **Speichern** aus, nachdem Sie die Parameter geändert haben. Sie können Ihre Änderungen auch verwerfen.

   ![Screenshot der Optionen für Serverparameter](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

Von der Seite **Serverparameter** können Sie zur Liste der Protokolle zurückkehren, indem Sie die Seite schließen.

## <a name="view-list-and-download-logs"></a>Anzeigen der Liste und Herunterladen von Protokollen
Nachdem die Protokollierung begonnen hat, können Sie eine Liste der verfügbaren Protokolle für langsame Abfragen anzeigen und einzelne Protokolldateien herunterladen.

1. Öffnen Sie das Azure-Portal.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. Die Seite zeigt eine Liste der Protokolldateien an.

   ![Screenshot der Seite „Serverprotokolle“ mit hervorgehobener Liste der Protokolle](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Die Namenskonvention des Protokolls ist **mysql-slow-<Name_Ihres_Servers>-jjjjmmtthh.log**. Das im Dateinamen verwendete Datum und die Uhrzeit geben den Zeitpunkt an, zu dem das Protokoll ausgestellt wurde. Die Protokolldateien werden alle 24 Stunden oder bei Erreichen einer Größe von 7,5 GB rotiert. 

4. Verwenden Sie bei Bedarf das Suchfeld, um schnell ein spezifisches Protokoll basierend auf Datum und Uhrzeit einzugrenzen. Die Suche erfolgt anhand des Namens des Protokolls.

5. Um einzelne Protokolldateien herunterzuladen, wählen Sie das Pfeilsymbol nach unten neben den einzelnen Protokolldateien in der Tabellenzeile aus.

   ![Screenshot der Seite „Serverprotokolle“ mit hervorgehobenem Pfeilsymbol nach unten](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

1. Wählen Sie auf der Randleiste im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** > **Diagnoseeinstellungen hinzufügen** aus.

   ![Screenshot der Optionen für Diagnoseeinstellungen](./media/howto-configure-server-logs-in-portal/add-diagnostic-setting.png)

1. Geben Sie einen Namen für die Diagnoseeinstellung ein.

1. Geben Sie an, an welche Datensenken die langsamen Abfrageprotokolle gesendet werden sollen (Speicherkonto, Event Hub oder Log Analytics-Arbeitsbereich).

1. Wählen Sie als Protokolltyp **MySqlSlowLogs** aus.
![Screenshot der Konfigurationsoptionen für Diagnoseeinstellungen](./media/howto-configure-server-logs-in-portal/configure-diagnostic-setting.png)

1. Nachdem Sie die Datensenken für die langsamen Abfrageprotokolle konfiguriert haben, wählen Sie **Speichern** aus.
![Screenshot der Konfigurationsoptionen für Diagnoseeinstellungen mit hervorgehobener Option „Speichern“](./media/howto-configure-server-logs-in-portal/save-diagnostic-setting.png)

1. Greifen Sie auf die langsamen Abfrageprotokolle zu, indem Sie sie in den von Ihnen konfigurierten Datensenken einsehen. Es kann bis zu 10 Minuten dauern, bis die Protokolle angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Konfigurieren von und Zugreifen auf Protokolle für langsame Abfragen mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md), um zu erfahren, wie Protokolle für langsame Abfragen programmgesteuert heruntergeladen werden können.
- Erfahren Sie mehr über [Protokolle für langsame Abfragen in Azure Database for MySQL](concepts-server-logs.md).
- Weitere Informationen zu den Parameterdefinitionen und der MySQL-Protokollierung finden Sie in der MySQL-Dokumentation unter [Protokolle](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).