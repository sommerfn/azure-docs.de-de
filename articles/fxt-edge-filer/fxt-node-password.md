---
title: Initialisieren der Hardware – Microsoft Azure FXT Edge Filer
description: Hier erfahren Sie, wie Sie ein anfängliches Kennwort für Azure FXT Edge Filer-Knoten festlegen.
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: rohogue
ms.openlocfilehash: 080aa05af77b996bc0eb71287a3dfef25c24629a
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72256016"
---
# <a name="tutorial-set-hardware-passwords"></a>Tutorial: Festlegen von Hardwarekennwörtern

Wenn Sie einen Azure FXT Edge Filer-Knoten zum ersten Mal einschalten, müssen Sie ein Stammkennwort festlegen. Die Hardwareknoten werden nicht mit einem Standardkennwort ausgeliefert. 

Die Netzwerkanschlüsse sind deaktiviert, bis das Kennwort festgelegt wurde und der Root-Benutzer sich anmeldet.

Führen Sie diesen Schritt nach der Installation und Verkabelung des Knotens, aber noch vor der Clustererstellung aus. 

In diesem Tutorial erfahren Sie, wie Sie eine Verbindung mit dem Hardwareknoten herstellen und das Kennwort festlegen. 

In diesem Lernprogramm lernen Sie Folgendes: 

> [!div class="checklist"]
> * Anschließen einer Tastatur und eines Monitors an den Knoten und Einschalten des Knotens
> * Festlegen von Kennwörtern für den iDRAC-Anschluss und den Root-Benutzer dieses Knotens
> * Anmelden als Root-Benutzer 

Wiederholen Sie diese Schritte für jeden Knoten, den Sie in Ihrem Cluster verwenden möchten. 

Planen Sie für dieses Tutorial ca. 15 Minuten ein. 

## <a name="prerequisites"></a>Voraussetzungen

Zur Vorbereitung dieses Tutorials müssen folgende Schritte ausgeführt werden: 

* [Installieren](fxt-install.md) Sie die einzelnen Azure FXT Edge Filer-Knoten in einem Rack, und schließen Sie die Netz- und Netzwerkkabel an, wie in [diesem Tutorial](fxt-network-power.md) beschrieben. 
* Halten Sie eine USB-Tastatur und einen Monitor mit VGA-Anschluss bereit, die Sie an die Hardwareknoten anschließen können. (Der serielle Anschluss des Knotens ist inaktiv, solange kein Kennwort festgelegt wurde.)

## <a name="connect-a-keyboard-and-monitor-to-the-node"></a>Anschließen einer Tastatur und eines Monitors an den Knoten

Verbinden Sie einen Monitor und eine Tastatur physisch mit dem Azure FXT Edge Filer-Knoten. 

* Schließen Sie den Monitor an den VGA-Anschluss an.
* Schließen Sie die Tastatur an einen der USB-Anschlüsse an. 

Das folgende Referenzdiagramm zeigt die Anschlüsse auf der Gehäuserückseite. 

> [!NOTE]
> Der serielle Anschluss ist inaktiv, solange kein Kennwort festgelegt wurde. 

![Diagramm: Rückseite von Azure FXT Edge Filer mit Beschriftungen für seriellen Anschluss, VGA-Anschluss und USB-Anschlüsse](media/fxt-back-serial-vga-usb.png)

Wenn Sie mehrere Knoten mit den gleichen Peripheriegeräten verbinden möchten, können Sie einen KVM-Switch verwenden. 

Schalten Sie den Knoten ein, indem Sie den Netzschalter auf der Vorderseite drücken. 

![Diagramm: Vorderseite von Azure FXT Edge Filer. Der runde Netzschalter befindet sich rechts oben (siehe Beschriftung).](media/fxt-front-annotated.png)

## <a name="set-initial-passwords"></a>Festlegen anfänglicher Kennwörter 

Der Azure FXT Edge Filer-Knoten gibt beim Start verschiedene Meldungen auf dem Monitor aus. Nach einigen Augenblicken wird ein Bildschirm für die Ersteinrichtung angezeigt, der in etwa wie folgt aussieht:

```
------------------------------------------------------
        Microsoft FXT node initial setup
------------------------------------------------------
Password Setup
---------------
Enter a password to set iDRAC and temporary root password.
Minimum password length is 8.
Enter new password:
```

Das Kennwort, das Sie eingeben, wird für zwei Dinge verwendet: 

* Als temporäres Stammkennwort für diesen Azure FXT Edge Filer-Knoten. 

  Dieses Kennwort ändert sich, wenn Sie einen Cluster mit diesem Knoten erstellen oder den Knoten dem Cluster hinzufügen. Das Clusterverwaltungskennwort (das dem Benutzer ``admin`` zugeordnet ist) ist auch das Stammkennwort für alle Knoten in einem Cluster.

* Als langfristiges Kennwort für den iDRAC/IPMI-Hardwareverwaltungsanschluss.

  Merken Sie sich das Kennwort gut, damit Sie sich bei Bedarf zur Behandlung von Hardwareproblemen über IPMI anmelden können.

Geben Sie das Kennwort ein, und bestätigen Sie es: 

```
Enter new password:**********
Re-enter password:**********
Loading AvereOS......
```

Nach Eingabe des Kennworts wird der Startvorgang des Systems fortgesetzt. Wenn der Startvorgang abgeschlossen ist, wird eine Anmeldeaufforderung (``login:``) angezeigt. 

## <a name="sign-in-as-root"></a>Anmelden als Root-Benutzer

Melden Sie sich als ``root`` an, und verwenden Sie dabei das soeben festgelegte Kennwort. 

```
login: root
Password:**********
```

Nachdem Sie sich als Root-Benutzer angemeldet haben, werden die Netzwerkanschlüsse aktiviert, und es wird eine Verbindung mit dem DHCP-Server hergestellt, um IP-Adressen zu beziehen. 

## <a name="next-steps"></a>Nächste Schritte

Der Knoten kann nun in einen Cluster eingebunden werden. Sie können ihn zur Erstellung des Azure FXT Edge Filer-Clusters verwenden oder ihn [einem vorhandenen Cluster hinzufügen](fxt-add-nodes.md). 

> [!div class="nextstepaction"]
> [Erstellen eines Clusters](fxt-cluster-create.md)
