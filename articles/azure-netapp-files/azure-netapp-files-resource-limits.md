---
title: Ressourceneinschränkungen für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt Limits für Azure NetApp Files-Ressourcen und wie eine Erhöhung des Ressourcenlimits angefordert wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: b-juche
ms.openlocfilehash: f7213ddee5d7bdfd41508f5fee66de63cde5b7c4
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71170021"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ressourcenlimits für Azure NetApp Files

Grundlegende Informationen zu Ressourceneinschränkungen für Azure NetApp Files unterstützen Sie bei der Verwaltung Ihrer Volumes.

## <a name="resource-limits"></a>Ressourceneinschränkungen

In der folgenden Tabelle werden die Ressourcengrenzwerte für Azure NetApp Files beschrieben:

|  Resource  |  Standardlimit  |  Über Supportanfrage anpassbar  |
|----------------|---------------------|--------------------------------------|
|  Anzahl der NetApp-Konten pro Azure-Abonnement   |  10    |  Ja   |
|  Anzahl der Kapazitätspools pro NetApp-Konto   |    25     |   Ja   |
|  Anzahl der Volumes pro Kapazitätspool     |    500   |    Ja     |
|  Anzahl von Momentaufnahmen pro Volume       |    255     |    Nein        |
|  Anzahl der an Azure NetApp Files (Microsoft.NetApp/volumes) delegierten Subnetze pro virtuellem Azure-Netzwerk    |   1   |    Nein    |
|  Anzahl der IP-Adressen innerhalb eines VNETs (einschließlich VNETs mit Peering), die auf Azure NetApp Files zugreifen können   |    1000   |    Ja   |
|  Mindestgröße eines einzelnen Kapazitätspools   |  4 TiB     |    Nein  |
|  Maximale Größe eines einzelnen Kapazitätspools    |  500 TiB   |   Nein   |
|  Mindestgröße eines einzelnen Volumes    |    100 GB    |    Nein    |
|  Maximale Größe eines einzelnen Volumes     |    ca. 100 TiB    |    Nein    |
|  Maximale Anzahl von Dateien ([maxfiles](#maxfiles)) pro Volume     |    100 Mio.    |    Ja    |    
|  Maximale Größe einer einzelnen Datei     |    16 TiB    |    Nein    |    

## Maxfiles-Limits <a name="maxfiles"></a> 

Azure NetApp Files-Volumes besitzen ein Limit namens *maxfiles*. Das maxfiles-Limit ist die Anzahl von Dateien, die ein Volume enthalten kann. Das maxfiles-Limit für ein Azure NetApp Files-Volume wird basierend auf der Größe (dem Kontingent) des Volumes indiziert. Das maxfiles-Limit für ein Volume erhöht oder verringert sich mit einer Rate von 20 Millionen Dateien pro TiB bereitgestellter Volumegröße. 

Der Dienst passt das maxfiles-Limit für ein Volume basierend auf seiner bereitgestellten Größe dynamisch an. Beispielsweise hätte ein Volume, das anfänglich mit einer Größe von 1 TiB konfiguriert wurde, ein maxfiles-Limit von 20 Millionen. Nachfolgende Änderungen an der Größe des Volumes führten zu einer automatischen Neuanpassung des maxfiles-Limits basierend auf den folgenden Regeln: 

|    Volumegröße (Kontingent)     |  Automatische Neuanpassung des maxfiles-Limits    |
|----------------------------|-------------------|
|    < 1 TiB                 |    20 Mio.     |
|    >= 1 TiB, aber < 2 TiB    |    40 Mio.     |
|    >= 2 TiB, aber < 3 TiB    |    60 Mio.     |
|    >= 3 TiB, aber < 4 TiB    |    80 Mio.     |
|    >= 4 TiB                |    100 Mio.    |

Für jede Volumegröße können Sie eine [Supportanfrage](#limit_increase) initiieren, um das maxfiles-Limit über 100 Millionen hinaus zu erhöhen.

## Anfordern einer Limiterhöhung <a name="limit_increase"></a> 

Sie können eine Supportanfrage an den Azure-Support stellen, um die anpassbaren Grenzwerte aus der obigen Tabelle zu erhöhen. 

Über den Navigationsbereich im Azure-Portal: 

1. Klicken Sie auf **Hilfe und Support**.
2. Klicken Sie auf **+ Neue Supportanfrage**.
3. Geben Sie die folgenden Informationen auf der Registerkarte „Grundeinstellungen“ an: 
    1. Problemtyp: Wählen Sie **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
    2. Abonnements: Wählen Sie das Abonnement für die Ressource aus, deren Kontingente erhöht werden sollen.
    3. Kontingenttyp: Wählen Sie **Storage: Azure NetApp Files limits** (Speicher: Azure NetApp Files-Grenzwerte) aus.
    4. Klicken Sie auf **Weiter: Lösungen**.
4. Auf der Registerkarte „Details“:
    1. Geben Sie die folgenden Informationen für den entsprechenden Ressourcentyp im Feld „Beschreibung“ an:

        |  Resource  |    Übergeordnete Ressourcen      |    Angeforderte neue Grenzwerte     |    Grund für die Kontingenterhöhung       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Konto |  *Abonnement-ID*   |  *Angeforderte neue maximale Anzahl an **Konten***    |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Pool    |  *Abonnement-ID, Konto-URI*  |  *Angeforderte neue maximale Anzahl an **Pools***   |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Volume  |  *Abonnement-ID, Konto-URI, Pool-URI*   |  *Angeforderte neue maximale Anzahl an **Volumes***     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Maxfiles  |  *Abonnement-ID, Konto-URI, Pool-URI, Volume-URI*   |  *Angeforderter neuer maximaler **maxfiles**-Wert*     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |    

    2. Legen Sie die entsprechende Supportmethode fest, und geben Sie Ihre Kontaktinformationen an.

    3. Klicken Sie auf **Weiter: Überprüfen + erstellen**, um die Anforderung zu erstellen. 


## <a name="next-steps"></a>Nächste Schritte  

- [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Cost model for Azure NetApp Files (Kostenmodell für Azure NetApp Files)](azure-netapp-files-cost-model.md)
