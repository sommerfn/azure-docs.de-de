---
title: IBM-Workloads in Azure | Microsoft-Dokumentation
description: Verwenden Sie einen Mainframe-Emulator und andere Dienste von Microsoft-Partnern, um Ihre IBM z/OS-Workloads mithilfe von Microsoft Azure neu zu hosten.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 02/22/2019
tags: ''
keywords: ''
ms.openlocfilehash: 13c83c53cdad719d6a4bed4cc1852b85d62082e8
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834582"
---
# <a name="ibm-workloads-on-azure"></a>IBM-Workloads in Azure

Viele auf z/OS basierende IBM-Mainframeworkloads können in Azure ohne Verlust an Funktionalität repliziert werden – so gut, dass die Benutzer die Änderungen am zugrundeliegenden System gar nicht bemerken werden. Das Neuhosten von Anwendungen in Azure gibt Ihnen die benötigten mainframeartigen Funktionen und zusätzlich die Elastizität, Verfügbarkeit und die potentiellen Kosteneinsparungen der Cloud.

Azure unterstützt die Integration mit vorhandenen IBM Mainframe-Umgebungen und erlaubt es Ihnen so, die sinnvollen Anwendungen zu migrieren, bei Bedarf Hybridlösungen zu betreiben und Ihre Migration im Lauf der Zeit durchzuführen. Zwar können Sie vorhandene mainframebasierte Programme vollständig für Azure umschreiben, es ist aber üblicher, sie neu zu hosten. Durch Umschreiben werden Kosten, Komplexität und Zeitaufwand von Migrationsprojekten deutlich gesteigert. Rehosting bietet Ihnen folgende Möglichkeiten:

- Verschieben von Anwendungen auf einen cloudbasierten Emulator.

- Migrieren der Datenbank zu einer cloudbasierten Datenbank.

- Ersetzen von Modulen und Codes mithilfe von Engines zur Codetransformierung.

Darüber hinaus befindet sich IBM-Software, einschließlich WebSphere und MQ, jetzt im Azure Marketplace. Mit einer Lizenz für IBM-Software können Sie von der von Azure bereitgestellten On-Demand-Infrastrukturskalierung profitieren, um schnell einen virtuellen Computer einzurichten.

Ein umfangreiches Partnerökosystem steht zur Verfügung und unterstützt Sie bei der Migration von IBM-Mainframesystemen zu Azure. Die meisten verfolgen einen pragmatischen Ansatz der weitgehenden Wiederverwendung, bevor sie sich auf eine stufenweise Bereitstellung mit Umschreiben oder Ersetzen von Anwendungen einlassen. Weitere Anleitungen und Hilfe von Partnern erhalten Sie im [Azure Mainframe-Migrationscenter](https://azure.microsoft.com/migration/mainframe/).

**Nächste Schritte**

- [Mainframemigration: Mythen und Fakten](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Installieren der IBM ZD&T-Entwicklungs-/Testumgebung in Azure](./install-ibm-z-environment.md)
- [Einrichten einer Application Developers Controlled Distribution (ADCD) in IBM ZD&T v1](./demo.md)
- [IBM DB2 pureScale in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
