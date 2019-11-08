---
title: Abbrechen der Operations-API | Azure Marketplace
description: Abbrechen von Vorgängen.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 374425dbd2abacb2114b5792d7476bc341fa353a
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819788"
---
# <a name="cancel-operation"></a>Abbrechen eines Vorgangs 

Diese API bricht einen Vorgang ab, der gerade für das Angebot ausgeführt wird. Verwenden Sie die [API zum Abrufen von Vorgängen](./cloud-partner-portal-api-retrieve-operations.md), um eine Vorgangs-ID (`operationId`) zu erhalten, die an diese API übergeben werden kann. Das Abbrechen eines Vorgangs ist in der Regel ein synchroner Vorgang. In bestimmten komplexen Szenarien wird zum Abbrechen eines vorhandenen Vorgangs jedoch unter Umständen ein neuer Vorgang benötigt. In diesem Fall enthält der HTTP-Antworttext den Ort des Vorgangs, der zum Abfragen des Status verwendet werden muss.

Sie können für die Anforderung eine kommagetrennte Liste mit E-Mail-Adressen angeben, an die die API Benachrichtigungen zum Status des Vorgangs senden soll.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>URI-Parameter
--------------

|  **Name**    |      **Beschreibung**                                  |    **Datentyp**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Herausgeber-ID (Beispiel: `contoso`)         |   Zeichenfolge          |
| offerId      |  Angebots-ID                                     |   Zeichenfolge          |
| api-version  |  Aktuelle Version der API                               |    Date           |
|  |  |  |


<a name="header"></a>Header
------

|  **Name**              |  **Wert**         |
|  ---------             |  ----------        |
|  Content-Type          |  Anwendung/json  |
|  Authorization         |  Bearer <IHR TOKEN> |
|  |  |


<a name="body-example"></a>Beispiel für Hauptteil
------------

### <a name="request"></a>Anforderung

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Eigenschaften des Anforderungstexts

|  **Name**                |  **Beschreibung**                                               |
|  --------                |  ---------------                                               |
|  notification-emails     | Kommagetrennte Liste mit den E-Mail-IDs, die über den Fortschritt des Veröffentlichungsvorgangs benachrichtigt werden sollen. |
|  |  |


### <a name="response"></a>response

  `Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Antwortheader

|  **Name**             |    **Wert**                       |
|  ---------            |    ----------                      |
| Operation-Location    | URL, die abgefragt werden kann, um den aktuellen Status des Vorgangs zu ermitteln. |
|  |  |


### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |  **Beschreibung**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK: Die Anforderung wurde erfolgreich verarbeitet, und der Vorgang wird synchron abgebrochen. |
|  202      | Akzeptiert: Die Anforderung wurde erfolgreich verarbeitet, und der Vorgang wird abgebrochen. Der Ort des Abbruchvorgangs wird im Antwortheader zurückgegeben. |
|  400      | Ungültige/falsch formatierte Anforderung: Der Fehlerantworttext enthält ggf. weitere Informationen.  |
|  403      | Zugriff verweigert: Der Client hat keinen Zugriff auf den in der Anforderung angegebenen Namespace. |
|  404      | Nicht gefunden: Die angegebene Entität ist nicht vorhanden. |
|  |  |
