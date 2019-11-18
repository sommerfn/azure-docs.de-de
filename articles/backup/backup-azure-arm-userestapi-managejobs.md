---
title: 'Azure Backup: Verwalten von Sicherungsaufträgen mit der REST-API'
description: In diesem Artikel erfahren Sie, wie Sie Sicherungs- und Wiederherstellungsaufträge von Azure Backup mit der REST-API nachverfolgen und verwalten.
ms.reviewer: pullabhk
author: dcurwin
manager: carmonm
keywords: REST-API; Azure-VM-Sicherung; Azure-VM-Wiederherstellung;
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: dacurwin
ms.assetid: b234533e-ac51-4482-9452-d97444f98b38
ms.openlocfilehash: 5fe0255a8f304061dc970907c651261832fee614
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73929098"
---
# <a name="track-backup-and-restore-jobs-using-rest-api"></a>Nachverfolgen von Sicherungs- und Wiederherstellungsaufträgen mit der REST-API

Der Azure Backup-Dienst löst Aufträge aus, die in unterschiedlichen Szenarien im Hintergrund ausgeführt werden, z. B. Auslösen von Sicherungen, Durchführen von Wiederherstellungsvorgängen und Deaktivieren der Sicherung. Diese Aufträge können anhand ihrer IDs nachverfolgt werden.

## <a name="fetch-job-information-from-operations"></a>Abrufen von Auftragsinformationen aus Vorgängen

Für einen Vorgang, z.B. das Auslösen einer Sicherung, wird immer eine jobID zurückgegeben. Beispiel:  Die letzte Antwort eines [REST-API-Vorgangs zum Auslösen einer Sicherung](backup-azure-arm-userestapi-backupazurevms.md#example-responses-3) lautet beispielsweise wie folgt:

```http
{
  "id": "cd153561-20d3-467a-b911-cc1de47d4763",
  "name": "cd153561-20d3-467a-b911-cc1de47d4763",
  "status": "Succeeded",
  "startTime": "2018-09-12T02:16:56.7399752Z",
  "endTime": "2018-09-12T02:16:56.7399752Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "41f3e94b-ae6b-4a20-b422-65abfcaf03e5"
  }
}
```

Der Azure VM-Sicherungsauftrag wird über das Feld „jobId“ identifiziert und kann wie [hier](https://docs.microsoft.com/rest/api/backup/jobdetails/) beschrieben nachverfolgt werden, indem eine einfache *GET*-Anforderung genutzt wird.

## <a name="tracking-the-job"></a>Nachverfolgen des Auftrags

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2019-05-13
```

`{jobName}` ist die oben erwähnte „jobId“. Die Antwort lautet immer „200 OK“, und im Feld „status“ wird der aktuelle Status des Auftrags angegeben. Wenn er „Completed“ oder „CompletedWithWarnings“ lautet, werden im Abschnitt „extendedInfo“ weitere Details zum Auftrag angegeben.

### <a name="response"></a>response

|NAME  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     | [JobResource](https://docs.microsoft.com/rest/api/backup/jobdetails/get#jobresource)        | OK        |

#### <a name="example-response"></a>Beispielantwort

Wenn der *GET*-URI übermittelt wird, wird die Antwort „200 (OK)“ zurückgegeben.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-client-request-id: ba4dff71-1655-4c1d-a71f-c9869371b18b; ba4dff71-1655-4c1d-a71f-c9869371b18b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14989
x-ms-correlation-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-routing-request-id: SOUTHINDIA:20180521T102317Z:e9702101-9da2-4681-bdf3-a54e17329a56
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:23:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-RecoveryServices-ResourceGroup-centralindia/providers/microsoft.recoveryservices/vaults/abdemovault/backupJobs/7ddead57-bcb9-4269-ac31-6a1b57588700",
  "name": "7ddead57-bcb9-4269-ac31-6a1b57588700",
  "type": "Microsoft.RecoveryServices/vaults/backupJobs",
  "properties": {
    "jobType": "AzureIaaSVMJob",
    "duration": "00:20:23.0896697",
    "actionsInfo": [
      1
    ],
    "virtualMachineVersion": "Compute",
    "extendedInfo": {
      "tasksList": [
        {
          "taskId": "Take Snapshot",
          "duration": "00:00:00",
          "status": "Completed"
        },
        {
          "taskId": "Transfer data to vault",
          "duration": "00:00:00",
          "status": "Completed"
        }
      ],
      "propertyBag": {
        "VM Name": "uttestvmub1",
        "Backup Size": "2332 MB"
      }
    },
    "entityFriendlyName": "uttestvmub1",
    "backupManagementType": "AzureIaasVM",
    "operation": "Backup",
    "status": "Completed",
    "startTime": "2018-05-21T08:35:40.9488967Z",
    "endTime": "2018-05-21T08:56:04.0385664Z",
    "activityId": "7df8e874-1d66-4f81-8e91-da2fe054811d"
  }
}
}

```
