---
title: include file
description: include file
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ab9b3c33aae6005510c34b207c7f87714149e5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608188"
---
### <a name="preview"></a>Wie kann ich die Public Preview nutzen?

Sie müssen ein Onboarding durchführen, um an der Public Preview teilnehmen zu können. Anhand der Schritte in [diesem Artikel](../articles/bastion/bastion-create-host-portal.md) können Sie eine neue Azure Bastion-Ressource erstellen. Derzeit müssen Sie [die Vorschauversion des Azure-Portals](https://aka.ms/BastionHost) statt des normalen Azure-Portals verwenden, wenn Sie diesen Dienst nutzen möchten.

### <a name="regions"></a>Welche Regionen sind während der Vorschauphase verfügbar?

Sie können die Bastion-Ressource in den folgenden Vorschauregionen über den [Link zur Vorschauversion des Azure-Portals](https://aka.ms/BastionHost) bereitstellen und nutzen.

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>Ich finde die Bastion-Ressource im Azure-Portal nicht. Wie sollte ich vorgehen?

Stellen Sie sicher, dass Sie den [Link zur Vorschauversion des Azure-Portals](https://aka.ms/BastionHost) und nicht das normale Azure-Portal nutzen.

### <a name="publicip"></a>Benötige ich auf meinem virtuellen Computer eine öffentliche IP-Adresse?

Sie benötigen auf dem virtuellen Azure-Computer, über den Sie eine Verbindung mit dem Azure Bastion-Dienst herstellen, KEINE öffentliche IP-Adresse. Der Bastion-Dienst öffnet die RDP-/SSH-Sitzung oder -Verbindung mit Ihrem virtuellen Computer über die private IP des virtuellen Computers in Ihrem virtuellen Netzwerk.

### <a name="rdpssh"></a>Benötige ich einen RDP- oder SSH-Client?

Sie benötigen keinen RDP- oder SSH-Client, um auf das RDP/SSH Ihres virtuellen Computers über das Azure-Portal zuzugreifen. Über den [Link zur Vorschauversion des Azure-Portals](https://aka.ms/BastionHost) können Sie auf den Vorschau-Flight des Portals zugreifen. Darüber erhalten Sie direkt über den Browser RDP-/SSH-Zugriff auf Ihren virtuellen Computer.

### <a name="agent"></a>Muss auf dem virtuellen Azure-Computer ein Agent ausgeführt werden?

Sie müssen keinen Agent oder eine andere Software in Ihrem Browser oder auf Ihrem virtuellen Azure-Computer installieren. Für den Bastion-Dienst ist kein Agent und keine zusätzliche Software für RDP/SSH erforderlich.

### <a name="browsers"></a>Welche Browser werden unterstützt?

Verwenden Sie während der Public Preview unter Windows Microsoft Edge oder Google Chrome. Verwenden Sie unter macOS Google Chrome. Microsoft Edge Chromium wird auch für Windows und Mac unterstützt.

### <a name="roles"></a>Sind für den Zugriff auf einen virtuellen Computer Rollen erforderlich?

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="previewbill"></a>Preise: Fallen für die Teilnahme an der Vorschau Gebühren an?

Während der Public Preview werden nur teilweise Gebühren in Rechnung gestellt. Für die Bereitstellung gilt allerdings auch keine SLA. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://aka.ms/BastionHostPricing).

### <a name="previewbill"></a>Warum erhalte ich vor Beginn der Bastion-Sitzung die Fehlermeldung „Ihre Sitzung ist abgelaufen.“?

Eine Sitzung darf nur über das Azure-Portal initiiert werden. Melden Sie sich beim Azure-Portal an, und starten Sie Ihre Sitzung erneut. Dieser Fehler ist zu erwarten, wenn Sie direkt von einer anderen Browsersitzung oder -registerkarte aus zu der URL navigieren. Er trägt zum Schutz Ihrer Sitzung bei und sorgt dafür, dass nur über das Azure-Portal auf die Sitzung zugegriffen werden kann.
