---
title: Rollen und Anforderungen für Azure Data Share (Vorschau)
description: Erfahren Sie etwas über die Zugriffssteuerungsrollen und die Anforderungen für Datenanbieter und Datenconsumer für die Freigabe von Daten in der Vorschauversion von Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: c0841f6386440776c6ea719f9932a53cada9d9c4
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166376"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Rollen und Anforderungen für Azure Data Share (Vorschau)

Dieser Artikel beschreibt die Rollen, die erforderlich sind, um Daten mithilfe von Azure Data Share (Vorschau) freizugeben sowie zu akzeptieren und zu empfangen. 

## <a name="roles-and-requirements"></a>Rollen und Anforderungen

Azure Data Share verwendet verwaltete Identitäten für Azure-Dienste (früher als MSIs bezeichnet), um sich bei den zugrunde liegenden Speicherkonten zu authentifizieren. Auf diese Weise können von einem Datenanbieter freigegebene Daten gelesen und von einem Datenconsumer freigegebene Daten empfangen werden. Zwischen dem Datenanbieter und dem Datenconsumer findet somit kein Austausch von Anmeldeinformationen statt. 

Der verwalteten Dienstidentität muss Zugriff auf die zugrunde liegenden Speicherkonten gewährt werden. Der Azure Data Share-Dienst verwendet die verwaltete Dienstidentität der Azure Data Share-Ressource, um Daten zu lesen und zu schreiben. Der Azure Data Share-Benutzer muss für die verwaltete Dienstidentität eine Rollenzuweisung zu dem Speicherkonto erstellen können, mit dem Daten gemeinsam genutzt werden. Die Berechtigung zum Erstellen von Rollenzuweisungen ist in der Rolle **Besitzer**, der Rolle „Benutzerzugriffsadministrator“ sowie benutzerdefinierten Rollen mit zugewiesener Berechtigung „Microsoft.Authorization/role assignments/write“ enthalten. 

Wenn Sie kein Besitzer des entsprechenden Speicherkontos sind und keine Rollenzuweisung für die verwaltete Identität der Azure Data Share-Ressource erstellen können, bitten Sie einen Azure-Administrator, eine Rollenzuweisung für Sie zu erstellen. 

Im Folgenden finden Sie eine Zusammenfassung der Rollen, die verwalteten Identitäten der Data Share-Ressource zugewiesen sind:

| |  |  |
|---|---|---|
|**Speichertyp**|**Quellspeicherkonto des Datenanbieters**|**Zielspeicherkonto des Datenconsumers**|
|Azure Blob Storage| Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
|Azure Data Lake Gen1 | Owner (Besitzer) | Nicht unterstützt
|Azure Data Lake Gen2 | Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
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

### <a name="data-consumers"></a>Datenconsumer
Zum Empfangen von Daten muss der Rolle „Mitwirkender an Storage-Blobdaten“ die verwaltete Identität der Datenfreigaberessource des Datenconsumers hinzugefügt werden. Diese Rolle ist erforderlich, damit der Azure Data Share-Dienst in das Speicherkonto schreiben kann. Dies erfolgt automatisch durch den Azure Data Share-Dienst, wenn der Benutzer Datasets über Azure hinzufügt und Besitzer des Speicherkontos oder Mitglied einer benutzerdefinierten Rolle ist, der die Berechtigung „Microsoft.Authorization/role assignments/write“ zugewiesen ist. 

Alternativ kann der Benutzer der Rolle „Mitwirkender an Storage-Blobdaten“ die verwaltete Identität der Datenfreigaberessource durch einen Azure-Administrator manuell hinzufügen lassen. Wenn diese Rollenzuweisung manuell durch den Administrator erstellt wird, ist es nicht mehr erforderlich, Besitzer des Speicherkontos zu sein oder über eine benutzerdefinierte Rollenzuweisung zu verfügen. Beachten Sie, dass dies Daten betrifft, die für Azure Storage oder Azure Data Lake Gen2 freigegeben werden. Das Empfangen von Daten in Azure Data Lake Gen1 wird nicht unterstützt. 

Um manuell eine Rollenzuweisung für die verwaltete Identität der Datenfreigaberessource zu erstellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zum Speicherkonto.
1. Wählen Sie **Access Control (IAM)** aus.
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie unter *Rolle* die Option *Mitwirkender an Storage-Blobdaten* aus. 
1. Geben Sie unter *Auswählen* den Namen Ihres Azure Data Share-Kontos ein.
1. Klicken Sie auf *Speichern*.

Wenn Sie Daten mithilfe unserer REST-APIs freigeben, müssen Sie diese Rollenzuweisungen manuell erstellen, indem Sie das Data Share-Konto den entsprechenden Rollen hinzufügen. 

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

