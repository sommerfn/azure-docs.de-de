---
title: Konfigurieren von Serverprotokollen für Azure Database for MariaDB und Zugreifen auf diese im Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal die Serverprotokolle in Azure Database for MariaDB konfigurieren und auf diese zugreifen.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 3dbf7064e409230916668e62ef861c0ce149fdbb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065635"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle im Azure-Portal

Sie können die [Protokolle für langsame Abfragen für Azure Database for MariaDB](concepts-server-logs.md) im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MariaDB-Server](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das Protokoll für langsame Abfragen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie Ihren Server für Azure Database for MariaDB aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. 
   ![„Serverprotokolle“ auswählen und zum Konfigurieren klicken](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Wählen Sie die Überschrift **Klicken Sie hier, um Protokolle zu aktivieren und Protokollparameter zu konfigurieren** aus, um die Serverparameter anzuzeigen.

5. Ändern Sie die Parameter, die Sie anpassen möchten, und legen Sie dabei „slow_query_log“ auch auf „EIN“ fest. Alle in dieser Sitzung vorgenommenen Änderungen werden in violett hervorgehoben. 

   Nachdem Sie die Parameter geändert haben, klicken Sie auf **Speichern**. Sie können Ihre Änderungen auch **verwerfen**.

   ![Auf „Speichern“ oder „Verwerfen“ klicken](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Kehren Sie zur Liste der Protokolle zurück, indem Sie auf der Seite **Serverparameter** auf die **Schaltfläche „Schließen“** (X-Symbol) klicken.

## <a name="view-list-and-download-logs"></a>Anzeigen der Liste und Herunterladen von Protokollen
Sobald die Protokollierung beginnt, können Sie eine Liste der verfügbaren Protokolle für langsame Abfragen anzeigen und einzelne Protokolldateien im Bereich „Serverprotokolle“ herunterladen. 

1. Öffnen Sie das Azure-Portal.

2. Wählen Sie Ihren Server für Azure Database for MariaDB aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. Die Seite zeigt wie im folgenden Beispiel eine Liste der Protokolldateien an:

   ![Liste der Protokolle](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > Die Namenskonvention des Protokolls ist **mysql-slow-<Name_Ihres_Servers>-jjjjmmtthh.log**. Das im Dateinamen verwendete Datum und die Uhrzeit geben den Zeitpunkt an, zu dem das Protokoll ausgestellt wurde. Die Protokolldateien werden alle 24 Stunden oder bei Erreichen einer Größe von 7,5 GB rotiert.

4. Verwenden Sie bei Bedarf das **Suchfeld**, um schnell ein spezifisches Protokoll basierend auf Datum/Uhrzeit einzugrenzen. Die Suche erfolgt anhand des Namens des Protokolls.

5. Laden Sie einzelne Protokolldateien wie gezeigt über die Schaltfläche **Download** (nach unten gerichtetes Pfeilsymbol) neben jeder Protokolldatei in der Tabellenzeile herunter:

   ![Klicken Sie auf das Symbol „Download“.](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokolle für langsame Abfragen](concepts-server-logs.md) in Azure Database for MariaDB.
- Weitere Informationen zu den Parameterdefinitionen und der Protokollierung finden Sie in der MariaDB-Dokumentation unter [Protokolle](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!--- See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->
