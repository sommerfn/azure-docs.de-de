---
title: Rollen und Anforderungen für Azure Data Share
description: Erfahren Sie etwas über die Zugriffssteuerungsrollen und Anforderungen für Datenanbieter und Datenconsumer für die Freigabe von Daten in Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 34c73a6bd400da076c68f308a2100a0f4569bd04
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490578"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Rollen und Anforderungen für Azure Data Share 

In diesem Artikel werden die Rollen beschrieben, die erforderlich sind, um Daten mithilfe von Azure Data Share freizugeben, zu akzeptieren und zu empfangen. 

## <a name="roles-and-requirements"></a>Rollen und Anforderungen

Azure Data Share verwendet verwaltete Identitäten für Azure-Dienste (früher als MSIs bezeichnet), um sich bei den zugrunde liegenden Speicherkonten zu authentifizieren. Auf diese Weise können von einem Datenanbieter freigegebene Daten gelesen und von einem Datenconsumer freigegebene Daten empfangen werden. Zwischen dem Datenanbieter und dem Datenconsumer findet somit kein Austausch von Anmeldeinformationen statt. 

Der verwalteten Dienstidentität muss Zugriff auf das zugrunde liegende Speicherkonto oder die zugrunde liegende SQL-Datenbank gewährt werden. Der Azure Data Share-Dienst verwendet die verwaltete Dienstidentität der Azure Data Share-Ressource, um Daten zu lesen und zu schreiben. Der Azure Data Share-Benutzer muss für die verwaltete Dienstidentität eine Rollenzuweisung für das Speicherkonto oder die SQL-Datenbank erstellen können, von denen oder für die Daten freigegeben werden. 

Bei der Speicherung ist die Berechtigung zum Erstellen von Rollenzuweisungen in der Rolle **Besitzer**, der Rolle „Benutzerzugriffsadministrator“ sowie in benutzerdefinierten Rollen mit zugewiesener Berechtigung „Microsoft.Authorization/role assignments/write“ enthalten. 

Wenn Sie kein Besitzer des entsprechenden Speicherkontos sind und keine Rollenzuweisung für die verwaltete Identität der Azure Data Share-Ressource erstellen können, bitten Sie einen Azure-Administrator, eine Rollenzuweisung für Sie zu erstellen. 

Im Folgenden finden Sie eine Zusammenfassung der Rollen, die verwalteten Identitäten der Data Share-Ressource zugewiesen sind:

| |  |  |
|---|---|---|
|**Speichertyp**|**Speicher des Datenanbieters**|**Zielspeicher des Datenconsumers**|
|Azure Blob Storage| Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
|Azure Data Lake Gen1 | Owner (Besitzer) | Nicht unterstützt
|Azure Data Lake Gen2 | Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
|Azure SQL | dbo | dbo 
|

### <a name="data-providers"></a>Datenanbieter 
Um in Azure Data Share ein Dataset hinzuzufügen, muss der Rolle „Storage-Blobdatenleser“ die verwaltete Identität der Datenfreigaberessource des Datenanbieters hinzugefügt werden. Dies erfolgt automatisch durch den Azure Data Share-Dienst, wenn der Benutzer Datasets über Azure hinzufügt und Besitzer des Speicherkontos oder Mitglied einer benutzerdefinierten Rolle ist, der die Berechtigung „Microsoft.Authorization/role assignments/write“ zugewiesen ist. 

Alternativ kann der Benutzer der Rolle „Storage-Blobdatenleser“ die verwaltete Identität der Datenfreigaberessource durch einen Azure-Administrator manuell hinzufügen lassen. Wenn diese Rollenzuweisung manuell durch den Administrator erstellt wird, ist es nicht mehr erforderlich, Besitzer des Speicherkontos zu sein oder über eine benutzerdefinierte Rollenzuweisung zu verfügen. Dies betrifft Daten, die von Azure Storage oder Azure Data Lake Gen2 freigegeben werden. 

Bei der Freigabe von Daten aus Azure Data Lake Gen1 muss die Rolle „Besitzer“ zugewiesen werden. 

Um eine Rollenzuweisung für die verwaltete Identität der Datenfreigaberessource zu erstellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zum Speicherkonto.
1. Wählen Sie **Access Control (IAM)** aus.
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie unter *Rolle* die Option *Storage-Blobdatenleser* aus.
1. Geben Sie unter *Auswählen* den Namen Ihres Azure Data Share-Kontos ein.
1. Klicken Sie auf *Speichern*.

Bei SQL-basierten Quellen muss ein Benutzer über einen externen Anbieter in der SQL-Datenbank erstellt werden, von der Daten freigegeben werden, und zwar mit demselben Namen wie das Azure Data Share-Konto. Ein Beispielskript sowie weitere Voraussetzungen für die SQL-basierte Freigabe finden Sie im Tutorial [Freigeben von Daten](share-your-data.md). 

### <a name="data-consumers"></a>Datenconsumer
Zum Empfangen von Daten muss der Rolle „Mitwirkender an Storage-Blobdaten“ und/oder der Rolle „dbo“ einer SQL-Datenbank (wenn Daten in einer SQL-Datenbank empfangen werden) die verwaltete Identität der Datenfreigaberessource des Datenconsumers hinzugefügt werden. 

Bei der Speicherung erfolgt dies automatisch durch den Azure Data Share-Dienst, wenn der Benutzer Datasets über Azure hinzufügt und Besitzer des Speicherkontos oder Mitglied einer benutzerdefinierten Rolle ist, der die Berechtigung „Microsoft.Authorization/role assignments/write“ zugewiesen ist. 

Alternativ kann der Benutzer der Rolle „Mitwirkender an Storage-Blobdaten“ die verwaltete Identität der Datenfreigaberessource durch einen Azure-Administrator manuell hinzufügen lassen. Wenn diese Rollenzuweisung manuell durch den Administrator erstellt wird, ist es nicht mehr erforderlich, Besitzer des Speicherkontos zu sein oder über eine benutzerdefinierte Rollenzuweisung zu verfügen. Beachten Sie, dass dies Daten betrifft, die für Azure Storage oder Azure Data Lake Gen2 freigegeben werden. Das Empfangen von Daten in Azure Data Lake Gen1 wird nicht unterstützt. 

Um manuell eine Rollenzuweisung für die verwaltete Identität der Datenfreigaberessource zu erstellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zum Speicherkonto.
1. Wählen Sie **Access Control (IAM)** aus.
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie unter *Rolle* die Option *Mitwirkender an Storage-Blobdaten* aus. 
1. Geben Sie unter *Auswählen* den Namen Ihres Azure Data Share-Kontos ein.
1. Klicken Sie auf *Speichern*.

Wenn Sie Daten mithilfe unserer REST-APIs freigeben, müssen Sie diese Rollenzuweisungen manuell erstellen, indem Sie das Data Share-Konto den entsprechenden Rollen hinzufügen. 

Wenn Daten in einer SQL-basierten Quelle empfangen werden, stellen Sie sicher, dass ein neuer Benutzer über einen externen Anbieter mit dem gleichen Namen wie Ihr Azure Data Share-Konto erstellt wird. Informationen zu den Voraussetzungen finden Sie im Tutorial [Akzeptieren und Empfangen von Daten](subscribe-to-data-share.md). 

Weitere Informationen zum Hinzufügen einer Rollenzuweisung finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment), in der das Hinzufügen einer Rollenzuweisung zu einer Azure-Ressource beschrieben wird. 

## <a name="resource-provider-registration"></a>Ressourcenanbieterregistrierung 

Wenn Sie eine Azure Data Share-Einladung annehmen, müssen Sie den Microsoft.DataShare-Ressourcenanbieter manuell in Ihrem Abonnement registrieren. Führen Sie diese Schritte aus, um den Microsoft.DataShare-Ressourcenanbieter in Ihrem Azure-Abonnement zu registrieren. 

1. Navigieren Sie im Azure-Portal zu **Abonnements**.
1. Wählen Sie das Abonnement aus, das Sie für Azure Data Share verwenden.
1. Klicken Sie auf **Ressourcenanbieter**.
1. Suchen Sie nach „Microsoft.DataShare“.
1. Klicken Sie auf **Registrieren**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Rollen in Azure: [Grundlegendes zu Rollendefinitionen](../role-based-access-control/role-definitions.md)

