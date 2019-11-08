---
title: Einbinden einer Azure HPC Cache-Instanz
description: Herstellen einer Verbindung von Clients mit einem Azure HPC Cache-Dienst
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: d906ed9a1a55e936c6374806a9037085c47e3b01
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582213"
---
# <a name="mount-the-azure-hpc-cache"></a>Einbinden einer Azure HPC Cache-Instanz

Nachdem der Cache erstellt wurde, können NFS-Clients mit einem einfachen Einbindebefehl darauf zugreifen.

Der Einbindungsbefehl besteht aus zwei Elementen:

* Eine der Einbindungsadressen des Cache (aufgelistet auf der Übersichtsseite des Cache).
* Der Pfad des virtuellen Namespace, den Sie beim Erstellen des Speicherziels festgelegt haben.

![Screenshot der Übersichtsseite einer Azure HPC Cache-Instanz mit einem Hervorhebungskasten um die Einbindungsadressliste rechts unten](media/hpc-cache-mount-addresses.png)

> [!NOTE] 
> Die Cache-Einbindungsadressen entsprechen Netzwerkschnittstellen innerhalb des Cache-Subnetzes. Diese NICs werden in einer Ressourcengruppe mit Namen angezeigt, die auf `-cluster-nic-` und einer Zahl enden. Sie dürfen diese Schnittstellen nicht ändern oder löschen, sonst ist der Cache nicht mehr verfügbar.

Die virtuellen Namespacepfade werden auf der Seite **Speicherziele** angezeigt. Klicken Sie auf den Namen eines einzelnen Speicherziels, um dessen Details anzuzeigen, einschließlich zugeordneter aggregierter Namespacepfade.

![Screenshot des Speicherzielfelds des Caches mit einem Hervorhebungskasten, der einen Eintrag in der Spalte „Pfad“ der Tabelle umgibt](media/hpc-cache-view-namespace-paths.png)

## <a name="mount-command-syntax"></a>Syntax des Einbindungsbefehls

Verwenden Sie einen Einbindungsbefehl wie diesen:

> sudo mount *cache_mount_address*:/*Namespacepfad* *lokaler_Pfad* {*Optionen*}

Beispiel:

```
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -orw,tcp,mountproto=tcp,vers3,hard
root@test-client:/tmp# 
```

Nachdem dieser Befehl erfolgreich ausgeführt wurde, sollte der Inhalt des Speicherexports im ``hpccache``-Verzeichnis auf dem Client sichtbar sein.

> [!NOTE] 
> Ihre Clients müssen in der Lage sein, auf das virtuelle Netzwerk und das Subnetz zuzugreifen, in dem sich Ihr Cache befindet. Erstellen Sie beispielsweise Client-VMs innerhalb des gleichen virtuellen Netzwerks, oder verwenden Sie einen Endpunkt, ein Gateway oder eine andere Lösung im virtuellen Netzwerk für den Zugriff von außen. Beachten Sie, dass innerhalb des Cache-Subnetzes nichts anderes gehostet werden kann.

### <a name="mount-command-options"></a>Einbindungsbefehlsoptionen

Um eine problemlose Clienteinbindung sicherzustellen, übergeben Sie diese Einstellungen und Argumente in Ihrem Einbindungsbefehl: 

``mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Empfohlene Einstellungen für den mount-Befehl | |
--- | --- 
``hard`` | Softwareseitige Einbindungen für Azure HPC Cache sind Anwendungsfehlern und möglichen Datenverlusten zugeordnet. 
``proto=netid`` | Diese Option unterstützt eine angemessene Behandlung von NFS-Netzwerkfehlern.
``mountproto=netid`` | Diese Option unterstützt die angemessene Behandlung von Netzwerkfehlern für Einbindungsvorgänge.
``retry=n`` | Legen Sie ``retry=30`` fest, um vorübergehende Einbindungsfehler zu vermeiden. (Bei Einbindungen im Vordergrund wird ein anderer Wert empfohlen.)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verschieben von Daten in die Speicherziele des Caches finden Sie unter [Auffüllen des neuen Azure-Blobspeichers](hpc-cache-ingest.md).
