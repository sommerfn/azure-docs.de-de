---
title: 'Azure VMware-Lösung von CloudSimple: Zuordnen öffentlicher IP-Adressen'
description: Beschreibt, wie öffentliche IP-Adressen für virtuelle Computer in der privaten Cloudumgebung zugeordnet werden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 12a6af73dc0abc9598184f6c83e9d7652973a99e
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543969"
---
# <a name="allocate-public-ip-addresses-for-private-cloud-environment"></a>Zuordnen von öffentlichen IP-Adressen für die private Cloudumgebung

Öffnen Sie die Registerkarte „Public IPs“ (Öffentliche IP-Adressen) auf der Seite „Network“ (Netzwerk), um öffentliche IP-Adressen für virtuelle Computer in Ihrer privaten Cloudumgebung zuzuordnen.

1. [Greifen Sie auf das CloudSimple Portal zu](monitor-activity.md), und wählen Sie **Network** (Netzwerk) im seitlichen Menü aus.
2. Wählen Sie **Public IPs** (Öffentliche IP-Adressen) aus.
3. Klicken Sie auf **New Public IP** (Neue öffentliche IP-Adresse).

    ![Seite „Öffentliche IP-Adressen“](media/public-ips-page.png)

4. Geben Sie einen Namen ein, um den IP-Adresseintrag zu identifizieren.
5. Behalten Sie den Standardort bei.
6. Verwenden Sie den Schieberegler, um das Leerlauftimeout bei Bedarf zu ändern.
7. Geben Sie die lokale IP-Adresse ein, für die Sie eine öffentliche IP-Adresse zuweisen möchten.
8. Geben Sie einen zugeordneten DNS-Namen ein.
9. Klicken Sie auf **Submit**.

![Zuordnen von öffentlichen IP-Adressen](media/network-public-ip-allocate.png)

Jetzt beginnt die Aufgabe des Zuordnens der öffentlichen IP-Adresse. Sie können den Status der Aufgabe auf der Seite **Aktivität > Aufgaben** überprüfen. Nach Abschluss der Zuordnung wird der neue Eintrag auf der Seite „Öffentliche IP-Adressen“ angezeigt.
