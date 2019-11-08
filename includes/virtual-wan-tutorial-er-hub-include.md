---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6f910dbe91ed8e1cb65eefa6dfc48c72a689bf25
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491538"
---
1. Suchen Sie das virtuelle WAN, das Sie erstellt haben. Wählen Sie auf der Seite „Virtual WAN“ im Abschnitt **Konnektivität** die Option **Hubs** aus.
2. Wählen Sie auf der Seite „Hubs“ **+ Neuer Hub** aus, um die Seite **Virtuellen Hub erstellen** zu öffnen.
3. Füllen Sie auf der Seite **Virtuellen Hub erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   ![Grundlagen](./media/virtual-wan-tutorial-er-hub-include/hub1.png "Grundlagen")

    **Projektdetails**

   * Region (zuvor als „Standort“ bezeichnet)
   * NAME
   * Privater Adressraum des Hubs. Der minimale Adressraum ist „/24“ zum Erstellen eines Hubs. Dies beinhaltet, dass ein Bereich von „/25“ bis „/32“ bei der Erstellung einen Fehler erzeugt.
4. Wählen Sie die Registerkarte **ExpressRoute** aus.

5. Füllen Sie auf der Registerkarte **ExpressRoute** die folgenden Felder aus:

   ![ExpressRoute](./media/virtual-wan-tutorial-er-hub-include/hub2.png "ExpressRoute")

   * Wählen Sie **Ja** aus, um ein **ExpressRoute**-Gateway zu erstellen.
   * Wählen Sie den Wert **Gatewayskalierungseinheiten** in der Dropdownliste aus.
6. Wählen Sie zum Überprüfen **Überprüfen + erstellen** aus.
7. Wählen Sie **Erstellen** aus, um den Hub zu erstellen. Klicken Sie nach 30 Minuten auf **Aktualisieren**, um den Hub auf der Seite **Hubs** anzuzeigen. Wählen Sie **Zu Ressource wechseln** aus, um zur Ressource zu navigieren.