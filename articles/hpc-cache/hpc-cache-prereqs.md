---
title: Azure HPC Cache-Voraussetzungen
description: Voraussetzungen für die Verwendung von Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: ca7a12f45f8d907ee65df85e349883e4c14af47a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582151"
---
# <a name="prerequisites-for-azure-hpc-cache"></a>Voraussetzungen für Azure HPC Cache

Stellen Sie sicher, dass Ihre Umgebung diese Anforderungen erfüllt, bevor Sie das Azure-Portal zum Erstellen eines neuen Azure HPC Cache verwenden.

## <a name="azure-subscription"></a>Azure-Abonnement

Ein kostenpflichtiges Abonnement wird empfohlen.

> [!NOTE]
> In den ersten Monaten muss das Azure HPC Cache-Team Ihr Abonnement der Zugriffsliste hinzufügen, damit es zum Erstellen einer Cache-Instanz verwendet werden kann. Mithilfe dieser Vorgehensweise wird sichergestellt, dass jeder Kunde gute Reaktionszeiten von seinen Caches erhält. Füllen Sie [dieses Formular](https://aka.ms/onboard-hpc-cache) aus, um Zugriff anzufordern.

## <a name="network-infrastructure"></a>Netzwerkinfrastruktur

Bevor Sie den Cache verwenden können, sollten Sie zwei netzwerkbezogene Voraussetzungen einrichten:

* Ein dediziertes Subnetz für die Azure HPC Cache-Instanz
* DNS-Unterstützung, sodass der Cache auf Speicher und andere Ressourcen zugreifen kann

### <a name="cache-subnet"></a>Cache-Subnetz

Der Azure HPC Cache benötigt ein dediziertes Subnetz mit diesen Eigenschaften:

* Im Subnetz müssen mindestens 64 IP-Adressen verfügbar sein.
* Das Subnetz darf keine weiteren VMs hosten, nicht einmal für verwandte Dienste wie Clientcomputer.
* Wenn Sie mehrere Azure HPC Cache-Instanzen verwenden, benötigt jede ein eigenes Subnetz.

Die bewährte Methode besteht im Erstellen eines neuen Subnetzes für die einzelnen Caches. Sie können im Rahmen der Cache-Erstellung ein neues virtuelles Netzwerk und Subnetz erstellen.

### <a name="dns-access"></a>DNS-Zugriff

Der Cache benötigt DNS für den Zugriff auf Ressourcen außerhalb seines virtuellen Netzwerks. Je nachdem, welche Ressourcen Sie verwenden, müssen Sie möglicherweise einen angepassten DNS-Server einrichten und die Weiterleitung zwischen diesem Server und Azure DNS-Servern konfigurieren:

* Um auf Azure Blob Storage-Endpunkte und andere interne Ressourcen zuzugreifen, benötigen Sie den Azure-basierten DNS-Server.
* Um auf lokalen Speicher zuzugreifen, müssen Sie einen benutzerdefinierten DNS-Server konfigurieren, der Ihre Speicherhostnamen auflösen kann.

Wenn Sie nur Zugriff auf Blobspeicher benötigen, können Sie den von Azure bereitgestellten DNS-Standardserver für Ihren Cache verwenden. Wenn Sie hingegen Zugriff auf weitere Ressourcen benötigen, sollten Sie einen benutzerdefinierten DNS-Server erstellen und für die Weiterleitung aller Azure-spezifischen Auflösungsanforderungen an den Azure DNS-Server konfigurieren. (Ein einfacher DNS-Server kann außerdem für den Lastenausgleich für Clientverbindungen zwischen allen verfügbaren Cache-Einbindungspunkten verwendet werden.)

Weitere Informationen zu virtuellen Azure-Netzwerken und DNS-Serverkonfigurationen finden Sie unter [Namensauflösung für Ressourcen in virtuellen Azure-Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

## <a name="permissions"></a>Berechtigungen

Überprüfen Sie diese Voraussetzungen im Hinblick auf Berechtigungen, bevor Sie mit dem Erstellen des Caches beginnen.

* Die Cache-Instanz muss in der Lage sein, virtuelle Netzwerkschnittstellen (NICs) zu erstellen. Der Benutzer, der den Cache erstellt, muss über ausreichende Berechtigungen für das Abonnement verfügen, um NICs zu erstellen.

* Bei der Verwendung von Blobspeicher benötigt Azure HPC Cache die Autorisierung zum Zugriff auf Ihr Speicherkonto. Sie können rollenbasierte Zugriffssteuerung (RBAC) verwenden, um dem Cache Zugriff auf Ihren Blobspeicher zu erteilen. Zwei Rollen sind erforderlich: „Mitwirkender von Speicherkonto“ und „Mitwirkender an Storage-Blobdaten“. Befolgen Sie die Anweisungen unter [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account), um die Rollen hinzuzufügen.

## <a name="storage-infrastructure"></a>Speicherinfrastruktur

Der Cache unterstützt Azure-Blobcontainer oder NFS-Hardwarespeicherexporte. Fügen Sie nach dem Erstellen des Caches Speicherziele hinzu.

Für jeden Speichertyp gelten bestimmte Voraussetzungen.

### <a name="nfs-storage-requirements"></a>NFS-Speicheranforderungen

Beim Verwenden von lokalem Hardwarespeicher benötigt der Cache Netzwerkzugriff aus seinem Subnetz auf das Rechenzentrum mit hoher Bandbreite. [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) oder ein ähnliches Zugriffsverfahren wird empfohlen.

Der NFS-Back-End-Speicher muss eine kompatible Hardware-/Softwareplattform aufweisen. Ausführliche Informationen erhalten Sie vom Azure HPC Cache-Team.

### <a name="blob-storage-requirements"></a>Blobspeicheranforderungen

Wenn Sie Azure-Blobspeicher in Kombination mit Ihrem Cache verwenden möchten, benötigen Sie ein kompatibles Speicherkonto und entweder einen leeren Blobcontainer oder einen Container, der mit Daten im Format von Azure HPC Cache aufgefüllt ist, wie unter [Verschieben von Daten in Azure Blob Storage](hpc-cache-ingest.md) beschrieben.

Erstellen Sie das Konto und den Container, bevor Sie versuchen, ihn als Speicherziel hinzufügen.

Verwenden Sie diese Einstellungen, um ein kompatibles Speicherkonto zu erstellen:

* Leistung: **Standard**
* Kontoart: **StorageV2 (universell v2)**
* Replikation: **Lokal redundanter Speicher (LRS)**
* Zugriffsebene (Standard): **Heiße Ebene**

Es wird empfohlen, ein Speicherkonto am Standort Ihres Caches zu verwenden.
<!-- clarify location - same region or same resource group or same virtual network? -->

Außerdem müssen Sie der Cache-Anwendung Zugriff auf Ihr Azure-Speicherkonto erteilt haben. Folgen Sie der Beschreibung unter [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account), um dem Cache die Zugriffsrollen „Mitwirkender von Speicherkonto“ und „Mitwirkender an Storage-Blobdaten“ zu erteilen. Wenn Sie nicht der Besitzer des Speicherkontos sind, lassen Sie den Besitzer diesen Schritt ausführen.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md) im Azure-Portal
