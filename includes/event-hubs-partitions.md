---
title: include file
description: include file
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b23f9532aa1ca6f7bae914ff8cb9d7566a0fec86
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841452"
---
Event Hubs bietet Nachrichtenstreaming über ein partitioniertes Consumermuster, in dem jeder Consumer nur eine bestimmte Teilmenge oder Partition des Nachrichtenstreams liest. Dieses Muster ermöglicht eine horizontale Skalierung für die Ereignisverarbeitung und bietet andere datenstrombezogene Features, die in Warteschlangen und Themen nicht verfügbar sind.

Eine Partition ist eine geordnete Sequenz von Ereignissen, die in einem Event Hub besteht. Neu eingehende Ereignisse werden am Ende dieser Sequenz hinzugefügt. Eine Partition kann als "Commitprotokoll" betrachtet werden

![Event Hubs](./media/event-hubs-partitions/partition.png)

Event Hubs bewahrt Daten über einen konfigurierten Aufbewahrungszeitraum auf, der für alle Partitionen im Event Hub gilt. Ereignisse laufen nach Zeit ab. Sie können nicht direkt gelöscht werden. Da Partitionen unabhängig sind und ihre eigene Datensequenz enthalten, wachsen sie häufig mit unterschiedlicher Geschwindigkeit.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Die Anzahl der Partitionen wird bei der Erstellung angegeben und muss zwischen zwei und 32 liegen. Die Partitionenanzahl kann nicht geändert werden. Behalten Sie daher beim Festlegen der Partitionenanzahl die langfristige Skalierung im Hinterkopf. Partitionen sind ein Mechanismus zum Organisieren von Daten, der sich auf die erforderliche Downstreamparallelität in verarbeitenden Anwendungen bezieht. Die Anzahl der Partitionen in einem Event Hub steht in direktem Zusammenhang mit der erwarteten Anzahl von gleichzeitigen Lesern. Sie können die Anzahl der Partitionen auf mehr als 32 erhöhen, wenn Sie das Event Hubs-Team kontaktieren.

Auch wenn Partitionen identifizierbar sind und Daten direkt an sie gesendet werden können, wird dies nicht empfohlen. Stattdessen können Sie Konstrukte höherer Ebene verwenden, die im Abschnitt [Ereignisherausgeber](../articles/event-hubs/event-hubs-features.md#event-publishers) erläutert werden. 

Partitionen werden mit einer Sequenz von Ereignisdaten gefüllt, die den Hauptteil des Ereignisses, einen benutzerdefinierten Eigenschaftenbehälter und Metadaten (z.B. Offset in der Partition und Nummer in der Streamsequenz) enthalten.

Es wird empfohlen, Durchsatzeinheiten und Partitionen 1:1 aufeinander abzustimmen, um eine optimale Skalierung zu erreichen. Eine einzelne Partition weist einen garantierten Ein- und Ausgang von bis zu einer Durchsatzeinheit auf. Sie können zwar einen höheren Durchsatz auf einer Partition erzielen, aber die Leistung ist nicht garantiert. Aus diesem Grund wird dringend empfohlen, dass die Anzahl der Partitionen in einem Event Hub größer oder gleich der Anzahl der Durchsatzeinheiten ist.

Angesichts des Gesamtdurchsatzes, den Sie planen zu benötigen, kennen Sie die Anzahl der benötigten Durchsatzeinheiten und die minimale Anzahl der Partitionen, aber wie viele Partitionen sollten Sie verwenden? Wählen Sie die Anzahl der Partitionen basierend auf der zu erreichenden Downstreamparallelität und Ihren zukünftigen Durchsatzanforderungen. Für die Anzahl der Partitionen, die Sie innerhalb eines Event Hubs verwenden, fällt keine Gebühr an.

Weitere Informationen zu Partitionen sowie zur Abwägung von Verfügbarkeit gegen Zuverlässigkeit finden Sie im [Programmierleitfaden für Event Hubs](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) sowie im Artikel [Verfügbarkeit und Konsistenz in Event Hubs](../articles/event-hubs/event-hubs-availability-and-consistency.md).
