---
title: Bereitstellen eines benutzerdefinierten Pools über ein verwaltetes Image – Azure Batch | Microsoft-Dokumentation
description: Erstellen Sie einen Batch-Pool über eine verwaltete Imageressource, um Serverknoten mit den Softwarekomponenten und Daten für Ihre Anwendung bereitzustellen.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.topic: article
ms.date: 09/16/2019
ms.author: lahugh
ms.openlocfilehash: 82a5f79ca7b7a16cd8f7294ebd1f70816b40ad82
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "73519254"
---
# <a name="use-a-managed-image-to-create-a-pool-of-virtual-machines"></a>Verwenden eines verwalteten Images zum Erstellen eines VM-Pools

Zum Erstellen eines benutzerdefinierten Images für die virtuellen Computer des Batch-Pools können Sie entweder [Shared Image Gallery](batch-sig-images.md) oder eine *verwaltete Imageressource* verwenden.

> [!TIP]
> In den meisten Fällen sollten Sie benutzerdefinierte Images mithilfe von Shared Image Gallery erstellen. Durch die Verwendung von Shared Image Gallery können Sie Pools schneller bereitstellen, eine größere Anzahl von virtuellen Computern skalieren und die Zuverlässigkeit bei der Bereitstellung virtueller Computer verbessern. Weitere Informationen finden Sie unter [Verwenden von Shared Image Gallery zum Erstellen eines benutzerdefinierten Pools](batch-sig-images.md).

## <a name="prerequisites"></a>Voraussetzungen

- **Eine verwaltete Imageressource**: Um einen Pool von virtuellen Computern mithilfe eines benutzerdefinierten Images zu erstellen, müssen Sie eine verwaltete Imageressource in demselben Azure-Abonnement und in derselben Region wie das Batch-Konto besitzen oder erstellen. Das Image sollte auf der Grundlage von Momentaufnahmen des Betriebssystemdatenträgers des virtuellen Computers und optional seiner angefügten Datenträger erstellt werden. Weitere Informationen und Schritte zum Vorbereiten eines verwalteten Images finden Sie im folgenden Abschnitt.
  - Verwenden Sie ein eindeutiges benutzerdefiniertes Image für jeden von Ihnen erstellten Pool.
  - Geben Sie die **Ressourcen-ID** des Images im Format `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage` an, wenn Sie mithilfe der Batch-APIs einen Pool mit dem Image erstellen möchten. Verwenden Sie den **Namen** des Images, wenn Sie das Portal nutzen möchten.  
  - Die verwaltete Imageressource sollte während der Lebensdauer des Pools vorhanden sein, damit der Pool zentral hochskaliert werden kann. Nach dem Löschen des Pools kann sie entfernt werden.

- **Authentifizierung über Azure Active Directory (AAD)** : Die Batch-Client-API muss die AAD-Authentifizierung verwenden. Die Azure Batch-Unterstützung für AAD ist unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) dokumentiert.

## <a name="prepare-a-custom-image"></a>Vorbereiten eines benutzerdefinierten Images

In Azure können Sie ein verwaltetes Image über folgende Komponenten vorbereiten:

- Momentaufnahmen des Betriebssystems und der Datenträger eines virtuellen Azure-Computers
- Einen generalisierten virtuellen Azure-Computer mit verwalteten Datenträgern
- Eine generalisierte lokale VHD, die in die Cloud hochgeladen wurde

Es wird empfohlen, für die zuverlässige Skalierung von Batch-Pools mit einem benutzerdefinierten Image ein verwaltetes Image *nur* mit der ersten Methode zu erstellen: Momentaufnahmen der Datenträger des virtuellen Computers. Informationen zum Vorbereiten eines virtuellen Computers, zum Erstellen einer Momentaufnahme und zum Erstellen eines Images auf der Grundlage der Momentaufnahme finden Sie in den folgenden Schritten.

### <a name="prepare-a-vm"></a>Vorbereiten eines virtuellen Computers

Wenn Sie einen neuen virtuellen Computer für das Image erstellen, können Sie ein von Batch unterstütztes Azure Marketplace-Erstanbieterimage als Basisimage für Ihr verwaltetes Image verwenden. Nur Images von Erstanbietern können als Basisimage verwendet werden. Eine vollständige Liste der von Azure Batch unterstützten Azure Marketplace-Imageverweisen finden Sie im Artikel zum Vorgang zum [Auflisten der Knoten-Agent-SKUs](/java/api/com.microsoft.azure.batch.protocol.accounts.listnodeagentskus).

> [!NOTE]
> Sie können kein Image von Drittanbietern verwenden, das über zusätzliche Lizenz- und Kaufbedingungen zu Ihrem Basisimage verfügt. Informationen zu diesen Marketplace-Images finden Sie in der Anleitung für [Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
)- oder [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
)-VMs.

- Stellen Sie sicher, dass der virtuelle Computer mit einem verwalteten Datenträger erstellt wird. Dies ist die Standardeinstellung für den Speicher, wenn Sie einen virtuellen Computer erstellen.
- Installieren Sie auf dem virtuellen Computer keine Azure-Erweiterungen wie die benutzerdefinierte Skripterweiterung. Wenn das Image eine vorinstallierte Erweiterung enthält, treten in Azure beim Bereitstellen des Batch-Pools unter Umständen Probleme auf.
- Wenn Sie die angefügten Datenträger verwenden, müssen Sie sie innerhalb eines virtuellen Computers einbinden und formatieren, um sie zu verwenden.
- Stellen Sie sicher, dass das von Ihnen bereitgestellte Basisbetriebssystem-Image den standardmäßigen temporären Datenträger verwendet. Der Batch-Knoten-Agent erwartet derzeit den standardmäßigen temporären Datenträger.
- Sobald der virtuelle Computer ausgeführt wird, stellen Sie über RDP (für Windows) oder SSH (für Linux) eine Verbindung mit ihm her. Installieren Sie erforderliche Software, oder kopieren Sie die gewünschten Daten.  

### <a name="create-a-vm-snapshot"></a>Erstellen einer Momentaufnahme eines virtuellen Computers

Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer VHD. Sie können zum Erstellen einer Momentaufnahme des Betriebssystemdatenträgers oder der Datenträger eines virtuellen Computers das Azure-Portal oder Befehlszeilentools verwenden. Schritte und Optionen zum Erstellen einer Momentaufnahme finden Sie in der Anleitung für virtuelle [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md)- oder [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md)-Computer.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Erstellen eines Images auf der Grundlage von Momentaufnahmen

Verwenden Sie zum Erstellen eines verwalteten Images auf der Grundlage einer Momentaufnahme Azure-Befehlszeilentools, etwa den Befehl [az image create](/cli/azure/image). Sie können ein Image erstellen, indem Sie eine Momentaufnahme des Betriebssystemdatenträgers und optional Momentaufnahmen der Datenträger angeben.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Erstellen eines Pools über ein benutzerdefiniertes Image im Portal

Wenn Sie Ihr benutzerdefiniertes Image gespeichert haben und dessen Ressourcen-ID oder Namen kennen, erstellen Sie auf der Grundlage dieses Images einen Batch-Pool. Die folgenden Schritte veranschaulichen, wie Sie mit dem Azure-Portal einen Pool erstellen.

> [!NOTE]
> Wenn Sie den Pool mit einer der Batch-APIs erstellen, stellen Sie sicher, dass die Identität, die Sie für die AAD-Authentifizierung verwenden, über Berechtigungen für die Imageressource verfügt. Weitere Informationen finden Sie unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md).
>
> Die Ressource für das verwaltete Image muss während der gesamten Lebensdauer des Pools vorhanden sein. Wenn die zugrunde liegende Ressource gelöscht wird, kann der Pool nicht skaliert werden.

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto. Das Konto muss sich in dem gleichen Abonnement und der gleichen Region wie die Ressourcengruppe befinden, die das benutzerdefinierte Image enthält.
2. Wählen Sie links im Fenster **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie im Fenster **Pools** den Befehl **Hinzufügen** aus.
4. Wählen Sie im Fenster **Pool hinzufügen** die Option **Benutzerdefiniertes Image (Linux/Windows)** in der Dropdownliste **Imagetyp** aus. Wählen Sie aus der Dropdownliste **Benutzerdefiniertes VM-Image** den Namen des Abbilds (Kurzform der Ressourcen-ID) aus.
5. Wählen Sie **Verleger/Angebot/SKU** für Ihr benutzerdefiniertes Image aus.
6. Geben Sie die übrigen erforderlichen Einstellungen, einschließlich **Knotengröße**, **Ziel für dedizierte Knoten** und **Knoten mit niedriger Priorität**, sowie alle gewünschten optionalen Einstellungen an.

    Für ein benutzerdefiniertes Image im Microsoft Windows Server Datacenter 2016 wird das Fenster **Pool hinzufügen** z. B. so angezeigt:

    ![Hinzufügen eines Pools aus dem benutzerdefinierten Windows-Image](media/batch-custom-images/add-pool-custom-image.png)
  
Um festzustellen, ob ein vorhandener Pool auf einem benutzerdefinierten Image basiert, überprüfen Sie die **Betriebssystem**-Eigenschaft im Abschnitt mit der Ressourcenzusammenfassung im Fenster **Pool**. Wenn der Pool aus einem benutzerdefinierten Image erstellt wurde, wird er auf **Benutzerdefiniertes VM-Image** festgelegt.

Alle benutzerdefinierten Images, die einem Pool zugeordnet sind, werden im Fenster **Eigenschaften** des Pools angezeigt.

## <a name="considerations-for-large-pools"></a>Überlegungen zu großen Pools

Wenn Sie mithilfe eines benutzerdefinierten Images einen Pool mit mehreren hundert virtuellen Computern erstellen möchten, folgen Sie den obigen Anweisungen, um ein Image zu verwenden, das auf der Grundlage einer Momentaufnahme eines virtuellen Computers erstellt wurde.

Beachten Sie außerdem die folgenden Überlegungen:

- **Größenbeschränkungen:** In Batch wird die Poolgröße auf 2.500 dedizierte Computeknoten oder 1.000 Knoten mit niedriger Priorität beschränkt, wenn Sie ein benutzerdefiniertes Image verwenden.

  Wenn Sie zum Erstellen mehrerer Pools das gleiche Image (oder mehrere Images, die auf der gleichen zugrunde liegenden Momentaufnahme basieren) verwenden, darf die Gesamtanzahl von Computeknoten in den Pools den oben genannten Grenzwert nicht überschreiten. Ein Image oder die zugrunde liegende Momentaufnahme sollte maximal für einen Pool verwendet werden.

  Die Grenzwerte können niedriger sein, wenn Sie den Pool mit [NAT-Eingangspools](pool-endpoint-configuration.md) konfigurieren.

- **Timeout bei Größenänderung:** Wenn Ihr Pool eine feste Anzahl von Knoten enthält (also nicht automatisch skaliert wird), erhöhen Sie die Eigenschaft „resizeTimeout“ des Pools (beispielsweise auf einen Wert zwischen 20 und 30 Minuten). Erreicht der Pool innerhalb des Timeoutzeitraums nicht die Zielgröße, führen Sie einen weiteren [Vorgang zur Größenänderung](/rest/api/batchservice/pool/resize) aus.

  Wenn Sie einen Pool mit mehr als 300 Computeknoten planen, müssen Sie die Größe des Pools unter Umständen mehrmals ändern, um die Zielgröße zu erreichen.
  
Durch Verwenden von [Shared Image Gallery](batch-sig-images.md) können Sie größere Pools mit Ihren benutzerdefinierten Images zusammen mit weiteren freigegebenen Imagereplikaten erstellen. Bei Verwendung von freigegebenen Images verkürzt sich die Zeit, die der Pool benötigt, um den stabilen Status zu erreichen, um bis zu 25 %, und die Leerlaufzeit des virtuellen Computers verkürzt sich um bis zu 30 %.

## <a name="considerations-for-using-packer"></a>Überlegungen zur Verwendung von Packer

Das direkte Erstellen einer verwalteten Imageressource mit Packer kann nur mit dem Benutzerabonnementmodus „Batch-Konten“ erfolgen. Für Konten im Modus „Batch-Dienst“ müssen Sie zuerst eine VHD erstellen und dann die VHD in eine verwaltete Imageressource importieren. Abhängig von Ihrem Poolzuordnungsmodus (Benutzerabonnement oder Batch-Dienst) können Ihre Schritte zum Erstellen einer verwalteten Imageressource variieren.

Stellen Sie sicher, dass die zur Erstellung des verwalteten Images verwendete Ressource für die Lebensdauer eines Pools mit Verweis auf das benutzerdefinierte Image vorhanden ist. Andernfalls kann es zu Fehlern bei der Poolzuordnung und/oder Skalierung kommen.

Wenn das Image oder die zugrunde liegende Ressource entfernt wird, erhalten Sie möglicherweise einen ähnlichen Fehler wie: `There was an error encountered while performing the last resize on the pool. Please try resizing the pool again. Code: AllocationFailed`. Stellen Sie im Fall dieser Fehlermeldung sicher, dass die zugrunde liegende Ressource nicht entfernt wurde.

Weitere Informationen zur Verwendung von Packer zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen eines Linux-Images mit Packer](../virtual-machines/linux/build-image-with-packer.md) oder [Erstellen eines Windows Images mit Packer](../virtual-machines/windows/build-image-with-packer.md).

## <a name="next-steps"></a>Nächste Schritte

Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
