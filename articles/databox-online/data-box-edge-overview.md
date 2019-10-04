---
title: Microsoft Azure Data Box Edge – Übersicht | Microsoft-Dokumentation
description: Beschreibt Azure Data Box Edge, eine Speicherlösung, die ein physisches Gerät für die netzwerkbasierte Übertragung in Azure verwendet.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 07/17/2019
ms.author: alkohli
ms.openlocfilehash: 69580f956b603423ef302353953a45ad5d00391e
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305421"
---
# <a name="what-is-azure-data-box-edge"></a>Was ist Azure Data Box Edge? 

Azure Data Box Edge ist ein KI-fähiges Edge-Computing-Gerät mit Netzwerkfunktionen zur Datenübertragung. Dieser Artikel bietet Ihnen einen Überblick über die Lösung Data Box Edge, ihre Vorteile, ihre wichtigsten Funktionen und die Szenarien, in denen Sie dieses Gerät bereitstellen können. 

Data Box Edge ist eine Hardware-as-a-Service-Lösung. Microsoft liefert Ihnen ein von der Cloud verwaltetes Gerät mit einem integrierten FPGA (Field Programmable Gate Array), das beschleunigte KI-Rückschlüsse ermöglicht und alle Funktionen eines Speichergateways bietet. 

## <a name="use-cases"></a>Anwendungsfälle

Dies sind die verschiedenen Szenarien, in denen Data Box Edge für schnelle Machine Learning-Rückschlüsse (ML) und zum Rückschließen von Daten am Edge sowie für die Vorverarbeitung von Daten vor dem Senden an Azure verwendet werden kann.

- **Rückschluss mit Azure Machine Learning**: Mit Data Box Edge können Sie ML-Modelle ausführen, um so schnell Ergebnisse zu erzielen, auf die Sie reagieren können, bevor die Daten in die Cloud übertragen werden. Das gesamte Dataset kann optional übertragen werden, um Ihre ML-Modelle weiter zu trainieren und zu verbessern. Weitere Informationen zur Verwendung der Azure ML-Modelle mit Hardwarebeschleunigung auf dem Data Box Edge-Gerät finden Sie unter [Bereitstellen von Azure ML-Modellen mit Hardwarebeschleunigung auf Data Box Edge](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Vorverarbeiten von Daten**: Transformieren Sie Daten vor dem Senden an Azure, um einen aussagekräftigeren Datensatz zu erstellen. Die Vorverarbeitung ermöglicht Folgendes: 

    - Aggregieren von Daten.
    - Ändern von Daten, um beispielsweise persönliche Daten zu entfernen.
    - Teilmengen von Daten zur Optimierung von Speicher und Bandbreite oder für die weitere Analyse.
    - Analysieren von und Reagieren auf IoT-Ereignisse. 

- **Übertragen von Daten über das Netzwerk an Azure**: Verwenden Sie Data Box Edge, um Daten einfach und schnell in Azure zu übertragen. Dadurch werden weitere Berechnungen und Analysen oder eine Archivierung ermöglicht. 


## <a name="key-capabilities"></a>Wichtige Funktionen

Data Box Edge bietet die folgenden Funktionen:

|Funktion |BESCHREIBUNG  |
|---------|---------|
|Beschleunigte KI-Rückschlüsse| Ermöglicht durch das integrierte FPGA.|
|Computing       |Ermöglicht das Analysieren, Verarbeiten und Filtern von Daten.|
|Hohe Leistung | Hochleistungscompute und -datenübertragungen.|
|Datenzugriff     | Direkter Datenzugriff aus Azure Storage-Blobs und Azure Files über Cloud-APIs zur zusätzlichen Datenverarbeitung in der Cloud. Lokaler Cache auf dem Gerät wird für schnellen Zugriff auf zuletzt verwendete Dateien verwendet.|
|Von der Cloud verwaltet     |Das Gerät und der Dienst werden über das Azure-Portal verwaltet.  |
|Hochladen von Daten im Offlinemodus     | Der Modus „Getrennt“ unterstützt Szenarien für das Hochladen von Daten im Offlinemodus.|
|Unterstützte Protokolle     | Für die Datenerfassung werden die Standardprotokolle SMB und NFS unterstützt. <br> Weitere Informationen zu unterstützten Versionen finden Sie unter [Systemanforderungen für Data Box Edge](data-box-edge-system-requirements.md).|
|Datenaktualisierung     | Möglichkeit, lokale Dateien mit den neuesten Informationen aus der Cloud zu aktualisieren.|
|Verschlüsselung    | BitLocker-Unterstützung zur lokalen Verschlüsselung von Daten und zur sicheren Datenübertragung in die Cloud über *HTTPS*.|
|Bandbreiteneinschränkung| Drosselung, um die Bandbreitennutzung während der Spitzenzeiten einzuschränken.|


## <a name="components"></a>Komponenten

Die Data Box Edge-Lösung umfasst die Data Box Edge-Ressource, ein physisches Data Box Edge-Gerät und eine lokale Webbenutzeroberfläche.

* **Physisches Data Box Edge-Gerät**: Ein von Microsoft bereitgestellter 1HE-Rackserver, der konfiguriert werden kann, um Daten an Azure zu senden. 
    
* **Data Box Edge-Ressource:** Eine Ressource im Azure-Portal, mit der Sie ein Data Box Edge-Gerät auf einer zentralen Webbenutzeroberfläche verwalten können, auf die an verschiedenen geografischen Standorten zugegriffen werden kann. Verwenden Sie die Data Box Edge-Ressource, um Ressourcen zu erstellen und zu verwalten, Geräte und Warnungen anzuzeigen und zu verwalten sowie Freigaben zu verwalten.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Weitere Informationen finden Sie unter [Erstellen einer Bestellung für ein Data Box Edge-Gerät](data-box-edge-deploy-prep.md#create-a-new-resource).

* **Lokale Webbenutzeroberfläche für Data Box**: Auf der lokalen Webbenutzeroberfläche können Sie Diagnosen ausführen, das Data Box Edge-Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Informationen zur Verwendung der webbasierten Benutzeroberfläche finden Sie unter [Verwenden der webbasierten Benutzeroberfläche zum Verwalten Ihrer Data Box](data-box-edge-manage-access-power-connectivity-mode.md).


## <a name="region-availability"></a>Regionale Verfügbarkeit

Das virtuelle Data Box Gateway-Gerät, die Azure-Ressource und das Zielspeicherkonto, an das Sie Daten übertragen, müssen sich nicht alle in der gleichen Region befinden.

- **Ressourcenverfügbarkeit**: Eine Liste aller Regionen, in denen die Data Box Edge-Ressource verfügbar ist, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Data Box Edge kann auch in der Azure Government-Cloud bereitgestellt werden. Weitere Informationen finden Sie unter [What is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) (Was ist Azure Government?).
    
- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar. Die Regionen, in denen die Speicherkonten Data Box 	Edge-Daten speichern, sollten sich in der Nähe des Gerätestandorts befinden, um eine optimale Leistung zu erzielen. Ein weit entfernt vom Gerät befindliches Speicherkonto führt zu langen Wartezeiten und einer langsameren Leistung. 


## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Data Box Edge-Systemanforderungen](data-box-edge-system-requirements.md) an.
- Machen Sie sich mit den [Data Box Edge-Einschränkungen](data-box-edge-limits.md) vertraut.
- Stellen Sie [Azure Data Box Edge](data-box-edge-deploy-prep.md) im Azure-Portal bereit.




