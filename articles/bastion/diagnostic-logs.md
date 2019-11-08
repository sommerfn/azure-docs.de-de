---
title: Aktivieren und Verwenden von Azure Bastion-Diagnoseprotokollen | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Azure Bastion-Diagnoseprotokolle aktivieren und verwenden.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 8ae421b48f3cfa8055f636052c990f99e0c775b4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510151"
---
# <a name="enable-and-work-with-bastion-diagnostic-logs"></a>Aktivieren und Verwenden von Bastion-Diagnoseprotokollen

Wenn Benutzer mithilfe von Azure Bastion eine Verbindung mit Workloads herstellen, können Diagnosedaten der Remotesitzungen von Bastion protokolliert werden. Anschließend können Sie anhand der Diagnose feststellen, welche Benutzer wann, von wo aus eine Verbindung mit welchen Workloads hergestellt haben, und weitere relevante Protokollinformationen einsehen. Um die Diagnose verwenden zu können, müssen Sie Diagnoseprotokolle in Azure Bastion aktivieren. Dieser Artikel unterstützt Sie dabei, Diagnoseprotokolle zu aktivieren und anschließend die Protokolle anzuzeigen.

## <a name="enable"></a>Aktivieren der Diagnoseprotokolle

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu ihrer Azure Bastion-Ressource, und wählen Sie auf der Azure Bastion-Seite **Diagnoseeinstellungen** aus.

   ![Diagnoseeinstellungen](./media/diagnostic-logs/1diagnostics-settings.png)
2. Wählen Sie **Diagnoseeinstellungen** und dann **+ Diagnoseeinstellung hinzufügen** aus, um ein Ziel für die Protokolle hinzuzufügen.

   ![Diagnoseeinstellung hinzufügen](./media/diagnostic-logs/2add-diagnostic-setting.png)
3. Wählen Sie auf der Seite **Diagnoseeinstellungen** den Typ des Speicherkontos aus, das zum Speichern der Diagnoseprotokolle verwendet werden soll.

   ![Speicherort auswählen](./media/diagnostic-logs/3add-storage-account.png)
4. Nachdem Sie die Einstellungen festgelegt haben, sieht das Ergebnis in etwa wie im folgenden Beispiel aus:

   ![Beispieleinstellungen](./media/diagnostic-logs/4example-settings.png)

## <a name="view"></a>Anzeigen des Diagnoseprotokolls

Um auf die Diagnoseprotokolle zuzugreifen, können Sie direkt das Speicherkonto verwenden, das Sie beim Aktivieren der Diagnoseeinstellungen angegeben haben.

1. Navigieren Sie zu ihrer Speicherkontoressource und dann zu **Container**. Sie sehen, dass im Blobcontainer Ihres Speicherkontos das **insights-logs-bastionauditlogs**-Blob erstellt wurde.

   ![Diagnoseeinstellungen](./media/diagnostic-logs/1-navigate-to-logs.png)
2. Wenn Sie innerhalb des Containers navigieren, sehen Sie verschiedene Ordner im Blob. Diese Ordner spiegeln die Ressourcenhierarchie für ihre Azure Bastion-Ressource wider.

   ![Diagnoseeinstellung hinzufügen](./media/diagnostic-logs/2-resource-h.png)
3. Navigieren Sie zur vollständigen Hierarchie der Azure Bastion-Ressource, deren Diagnoseprotokolle Sie aufrufen/anzeigen möchten. „y=“, „m=“, „d=“, „h=“ und „m=“ geben das Jahr, den Monat, den Tag, die Stunde und die Minute der Diagnoseprotokollerstellung an.

   ![Speicherort auswählen](./media/diagnostic-logs/3-resource-location.png)
4. Suchen Sie die von Azure Bastion erstellte JSON-Datei, die die Diagnoseprotokolldaten für den Zeitraum enthält, zu dem Sie navigiert sind.

5. Laden Sie die JSON-Datei aus Ihrem Speicherblobcontainer herunter. Im Folgenden ein Beispieleintrag aus der JSON-Datei zu Referenzzwecken:

   ```json
   { 
   "time":"2019-10-03T16:03:34.776Z",
   "resourceId":"/SUBSCRIPTIONS/<subscripionID>/RESOURCEGROUPS/MYBASTION/PROVIDERS/MICROSOFT.NETWORK/BASTIONHOSTS/MYBASTION-BASTION",
   "operationName":"Microsoft.Network/BastionHost/connect",
   "category":"BastionAuditLogs",
   "level":"Informational",
   "location":"eastus",
   "properties":{ 
      "userName":"<username>",
      "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36",
      "clientIpAddress":"131.107.159.86",
      "clientPort":24039,
      "protocol":"ssh",
      "targetResourceId":"/SUBSCRIPTIONS/<subscripionID>/RESOURCEGROUPS/MYBASTION/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/LINUX-KEY",
      "subscriptionId":"<subscripionID>",
      "message":"Successfully Connected.",
      "resourceType":"VM",
      "targetVMIPAddress":"172.16.1.5",
      "tunnelId":"<tunnelID>"
   },
   "FluentdIngestTimestamp":"2019-10-03T16:03:34.0000000Z",
   "Region":"eastus",
   "CustomerSubscriptionId":"<subscripionID>"
   }
   ```

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
