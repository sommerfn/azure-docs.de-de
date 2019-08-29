---
title: Classroom-Labs in Azure Lab Services – Häufig gestellte Fragen | Microsoft-Dokumentation
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Classroom-Labs in Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: 411037dd97350d877aff4e2d094c3408f168f9fd
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648572"
---
# <a name="classroom-labs-in-azure-lab-services---frequently-asked-questions-faq"></a>Classroom-Labs in Azure Lab Services: Häufig gestellte Fragen (FAQ)
Hier erhalten Sie Antworten auf einige der am häufigsten gestellten Fragen zu Classroom-Labs in Azure Lab Services. 

## <a name="quotas"></a>Kontingente

### <a name="is-the-quota-per-user-or-per-week-or-per-entire-duration-of-the-lab"></a>Gilt das Kontingent pro Benutzer oder pro Woche oder für die gesamte Dauer des Labs? 
Das Kontingent, das Sie für ein Lab festlegen, gilt für jeden Kursteilnehmer für die gesamte Dauer des Labs. Und die [geplante Ausführungszeit von virtuellen Computern](how-to-create-schedules.md) wird nicht mit dem Kontingent eines Benutzers verrechnet. Das Kontingent dient für die Zeiten außerhalb der Stunden im Zeitplan, die ein Kursteilnehmer mit VMs verbringt.  Weitere Informationen zu Kontingenten finden Sie unter [Festlegen von Kontingenten für Benutzer](how-to-configure-student-usage.md#set-quotas-for-users).

## <a name="schedules"></a>Zeitpläne

### <a name="do-all-vms-in-the-lab-start-automatically-when-a-schedule-is-set"></a>Werden alle VMs im Lab automatisch gestartet, wenn ein Zeitplan festgelegt wird? 
Nein. Nicht alle VMs. Nur die VMs, die Benutzern nach einem Zeitplan zugewiesen sind. Die VMs, die keinem Benutzer zugewiesen sind, werden nicht automatisch gestartet. Dies ist beabsichtigt. 

## <a name="lab-accounts"></a>Labkonten

### <a name="why-am-i-not-able-to-create-a-lab-because-of-unavailability-of-the-address-range"></a>Warum kann ich wegen der Nichtverfügbarkeit des Adressbereichs kein Lab erstellen? 
Classroom-Labs können Lab-VMs in einem IP-Adressbereich erstellen, den Sie beim Erstellen Ihres Labkontos im Azure-Portal angeben. Wenn ein Adressbereich angegeben ist, werden jedem Lab, das danach erstellt wird, 512 IP-Adressen für Lab-VMs zugewiesen. Der Adressbereich für das Labkonto muss groß genug sein, um alle Labs aufnehmen zu können, die Sie unter dem Labkonto erstellen möchten. 

Wenn Sie beispielsweise den Block „/19 - 10.0.0.0/19“ verwenden, umfasst dieser Adressbereich 8192 IP-Adressen und 16 Labs (8192:512 = 16 Labs). In diesem Fall schlägt die Erstellung des 17. Labs fehl.

## <a name="blog-post"></a>Blogbeitrag
Abonnieren Sie den [Azure Lab Services-Blog](https://azure.microsoft.com/blog/tag/azure-lab-services/).

## <a name="update-notifications"></a>Updatebenachrichtigungen
Abonnieren Sie [Lab Services-Updates](https://azure.microsoft.com/updates/?product=lab-services), um über neue Features in Labs Services auf dem Laufenden zu bleiben.

## <a name="general"></a>Allgemein
### <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, damit wir Ihnen helfen können, eine Antwort zu finden.

- Veröffentlichen Sie eine Frage am Ende dieses Artikels zu häufig gestellten Fragen. 
- Um eine größere Zielgruppe zu erreichen, veröffentlichen Sie eine Frage im [Stack Overflow-Forum zu Azure Lab Services](https://stackoverflow.com/questions/tagged/azure-lab-services). 
- Um Funktionsanforderungen einzureichen, übermitteln Sie Ihre Anfragen und Ideen an die [UserVoice-Plattform für Azure Lab Services](https://feedback.azure.com/forums/320373-lab-services?category_id=352774).

