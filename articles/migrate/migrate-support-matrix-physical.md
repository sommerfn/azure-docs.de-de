---
title: Azure Migrate-Unterstützungsmatrix für die Bewertung und Migration physischer Server
description: Fasst die Einstellungen und Einschränkungen für die Bewertung und Migration physischer Server mit dem Azure Migrate-Dienst zusammen.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 902e89305aa24e8cbb6d2dd84d6a61c710d11637
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715369"
---
# <a name="support-matrix-for-physical-server-assessment-and-migration"></a>Unterstützungsmatrix für die Bewertung und Migration physischer Server

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Übersicht über die Unterstützungseinstellungen und -einschränkungen für die Bewertung und Migration von lokalen physischen Servern.

> [!NOTE]
> Werden hier Features erwähnt, die Sie noch nicht im Azure Migrate-Portal sehen, bitten wir Sie um etwas Geduld. Sie werden voraussichtlich im Laufe der nächsten Woche verfügbar.


## <a name="physical-server-scenarios"></a>Physischer Server – Szenarien

Die Tabelle enthält eine Übersicht über die unterstützten Szenarien für physische Server.

**Bereitstellung** | **Details***
--- | ---
**Zugriff auf lokale physische Server** | [Einrichten](tutorial-prepare-physical.md) Ihrer ersten Bewertung
**Migrieren physischer Server zu Azure** | [Testen ](tutorial-migrate-physical-virtual-machines.md) der Migration zu Azure


## <a name="azure-migrate-projects"></a>Azure Migrate-Projekte

**Unterstützung** | **Details**
--- | ---
**Azure-Berechtigungen** | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
**Physische Server** | Sie können bis zu 250 physische Server in einem einzelnen Projekt bewerten. Ein Azure-Abonnement kann mehrere Projekte enthalten. Ein Projekt kann im Rahmen der Bewertungseinschränkungen physische Server, VMware-VMs und Hyper-V-VMs umfassen.
**Geografie** | Sie können Azure Migrate-Projekte in verschiedenen geografischen Regionen erstellen. Obwohl Sie Projekte nur in spezifischen geografischen Regionen erstellen können, haben Sie die Möglichkeit, Computer für andere Zielstandorte zu bewerten und dorthin zu migrieren. Die Projektgeografie wird nur zum Speichern der ermittelten Metadaten verwendet.

  **Geografie** | **Speicherort der Metadaten**
  --- | ---
  Azure Government | US Government, Virginia
  Asien-Pazifik | „Asien, Osten“ und „Asien, Südosten“
  Australien | „Australien, Osten“ oder „Australien, Südosten“
  Brasilien | Brasilien Süd
  Kanada | „Kanada, Mitte“ oder „Kanada, Osten“
  Europa | „Europa, Norden“ oder „Europa, Westen“
  Frankreich | Frankreich, Mitte
  Indien | „Indien, Mitte“ oder „Indien, Süden“
  Japan |  „Japan, Osten“ oder „Japan, Westen“
  Korea | „Südkorea, Mitte“ oder „Südkorea, Süden“
  Vereinigtes Königreich | „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“
  USA | „USA, Mitte“ oder „USA, Westen 2“


 > [!NOTE]
 > Der Support für Azure Government ist derzeit nur für die [ältere Version](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar.


## <a name="assessment-physical-server-requirements"></a>Anforderungen an physische Server für die Bewertung

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Physische Serverbereitstellung**       | Der physische Server kann eigenständig sein oder in einem Cluster bereitgestellt werden. |
| **Berechtigungen**           | **Windows:** Richten Sie auf allen Windows-Servern, die Sie in die Ermittlung einschließen möchten, ein lokales Benutzerkonto ein. Das Benutzerkonto muss diesen Gruppen hinzugefügt werden: Remotedesktopbenutzer, Leistungsüberwachungsbenutzer und Leistungsprotokollbenutzer. <br/> **Linux:** Sie benötigen ein root-Konto auf den Linux-Servern, die Sie ermitteln möchten. |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)-Betriebssysteme, die von Azure unterstützt werden |


## <a name="assessment-appliance-requirements"></a>Anforderungen an die Appliances für die Bewertung

Für die Bewertung führt Azure Migrate eine einfache Appliance aus, um physische Server zu ermitteln und Servermetadaten sowie Leistungsdaten an Azure Migrate zu senden. Die Appliance kann auf einem physischen Server oder virtuellen Computer ausgeführt werden. Sie wird mithilfe eines PowerShell-Skripts eingerichtet, das Sie aus dem Azure-Portal herunterladen. Die folgende Tabelle fasst die Anforderungen an die Appliance zusammen.

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Bereitstellung einer Appliance**   |  Sie stellen die Appliance auf einem physischen Server oder virtuellen Computer bereit.<br/>  Auf dem Hostcomputer muss Windows Server 2012 R2 oder höher ausgeführt werden.<br/> Der Host benötigt ausreichenden Speicherplatz, um 16 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einen externen Switch für die Appliance-VM zuzuweisen.<br/> Die Appliance benötigt eine statische oder dynamische IP-Adresse und einen Internetzugang.
| **Azure Migrate-Projekt**  |  Eine Appliance kann einem einzelnen Projekt zugeordnet werden.<br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> Sie können bis zu 35.000 Computer in einem Projekt bewerten.
| **Ermittlung**              | Eine einzelne Appliance kann bis zu 200 Server ermitteln.
| **Bewertungsgruppe**       | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.
| **Bewertung**             | Sie können bis zu 35.000 virtuelle Computer in einer einzelnen Bewertung bewerten.


## <a name="assessment-appliance-url-access"></a>URL-Zugriff auf die Bewertungsappliance

Zur Bewertung von VMs muss die Azure Migrate-Appliance mit dem Internet verbunden sein.

- Bei der Bereitstellung der Appliance führt Azure Migrate eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch.
- Wenn Sie einen URL-basierten Proxy verwenden, lassen Sie den Zugriff auf die URLs in der Tabelle zu, und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die beim Durchsuchen der URLs empfangen werden.
- Wenn Sie über einen abfangenden Proxy verfügen, müssen Sie möglicherweise das Serverzertifikat vom Proxyserver in die Appliance importieren.


**URL** | **Details**  
--- | ---
*.portal.azure.com | Navigation zum Azure-Portal
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com  | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Erstellung von Azure Active Directory-Anwendungen für die Kommunikation von der Appliance zum Dienst
management.azure.com | Erstellung von Azure Active Directory-Anwendungen für die Kommunikation von der Appliance zum Dienst
dc.services.visualstudio.com | Protokollierung und Überwachung
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault bei der Kommunikation zwischen Appliance und Dienst.
aka.ms/* | Zulassen des Zugriffs auf aka-Links
https://download.microsoft.com/download/* | Lässt Downloads über die Microsoft-Downloadwebsite zu.



## <a name="assessment-port-requirements"></a>Portanforderungen für die Bewertung

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/> Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Physische Server** | **Windows:** Eingehende Verbindungen auf den Ports 443 und 5989 zum Abrufen von Konfigurations- und Leistungsmetadaten von Windows-Servern. <br/> **Linux:**  Eingehende Verbindungen auf dem Port 22 (UDP) zum Abrufen von Konfigurations- und Leistungsmetadaten von Linux-Servern. |


## <a name="next-steps"></a>Nächste Schritte

Führen Sie die [Vorbereitung der Bewertung physischer Server](tutorial-prepare-physical.md) für die Bewertung und Migration physischer Server durch.
