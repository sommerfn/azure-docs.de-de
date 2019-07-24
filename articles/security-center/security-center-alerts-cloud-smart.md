---
title: Korrelation von intelligenten Cloudwarnungen in Azure Security Center (Incidents) | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Fusion durch die Korrelation von intelligenten Cloudwarnungen Sicherheitsincidents in Azure Security Center generiert.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 7ba2cf14d9fac100f44a1ef23997b27ba062bee0
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295861"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Korrelation von intelligenten Cloudwarnungen in Azure Security Center (Incidents)

Security Center analysiert kontinuierlich die Hybrid Cloud-Workloads unter Verwendung von erweiterten Analysen und Threat Intelligence, um Sie auf schädliche Aktivitäten aufmerksam zu machen.

Angesichts eines immer breiteren Bedrohungsspektrums und der zunehmenden Notwendigkeit, selbst kleinste Anzeichen einer Kompromittierung zu erkennen, kann es für Sicherheitsanalysten schwierig sein, die verschiedenen Warnungen zu sichten und einen tatsächlichen Angriff zu erkennen. Security Center unterstützt Analysten bei der Bewältigung der Warnungen sowie bei der zeitnahen Angriffsdiagnose. Zu diesem Zweck werden verschiedene Warnungen und Signale mit geringer Genauigkeit zu Sicherheitsincidents korreliert.

Fusion ist die Technologie und das Analyse-Back-End, die Security Center-Incidents zugrunde liegen, und ermöglicht die Korrelation verschiedener Warnungen und kontextbezogener Signale. Zu diesem Zweck analysiert Fusion ressourcenübergreifend die verschiedenen, für ein Abonnement gemeldeten Signale und ermittelt verbreitete Muster, die Aufschluss über den Status eines Angriffs geben, oder Signale mit gemeinsamen kontextbezogen Informationen, die darauf hindeuten, dass eine einheitliche Gegenmaßnahme ergriffen werden sollte.

Bei Fusion-Analysen werden Sicherheitsdomänenkenntnisse mit künstlicher Intelligenz kombiniert, um Warnungen zu analysieren und neue Angriffsmuster zu erkennen, sobald diese auftreten. 

Security Center nutzt die Angriffsmatrix von Mitre, um einen Zusammenhang zwischen Warnungen und deren vermeintlicher Absicht herzustellen und so zur Formalisierung von Sicherheitsdomänenkenntnissen beizutragen. Auf der Grundlage der Informationen, die für die einzelnen Schritte eines Angriffs gesammelt wurden, kann Security Center außerdem Aktivitäten ausschließen, die lediglich wie Angriffsschritte aussehen, aber gar keine sind.  

Da Angriffe häufig mandantenübergreifend auftreten, kann Security Center KI-Algorithmen für die Analyse von Angriffssequenzen kombinieren, die für die einzelnen Abonnements gemeldet werden, um zu erkennen, ob es sich um verbreitete Warnungsmuster handelt (und nicht nur um Warnungen, die zufällig zusammenhängen).

Bei der Untersuchung eines Incidents benötigen Analysten häufig zusätzlichen Kontext, um die Art der Bedrohung und mögliche Gegenmaßnahmen einschätzen zu können. Selbst wenn also beispielsweise eine Netzwerkanomalie erkannt wurde, ist es schwer, angemessen darauf zu reagieren, wenn die weiteren Vorgänge in Bezug auf das Netzwerk oder die betroffenen Ressourcen nicht klar sind. Ein Sicherheitsincident kann hier mit Artefakten, verwandten Ereignissen und Informationen weiterhelfen. Welche zusätzlichen Informationen für Sicherheitsincidents verfügbar sind, hängt von der Art der erkannten Bedrohung und der Konfiguration Ihrer Umgebung ab. 

![Details eines Sicherheitsincidents](./media/security-center-alerts-cloud-smart/security-incident.png)

Weitere Informationen zu Sicherheitsincidents finden Sie unter [Behandeln von Sicherheitsincidents in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-incident).

