---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/22/2019
ms.author: tarcher
ms.openlocfilehash: eb96027351cf244e9cd4404f702544411130db5e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178781"
---
[Azure Service Bus](/azure/service-bus-messaging/service-bus-messaging-overview) ist ein Nachrichtenbroker für die [Unternehmensintegration](https://azure.microsoft.com/product-categories/integration/). Service Bus unterstützt zwei Kommunikationstypen: Warteschlangen und Themen. 

Warteschlangen unterstützen die asynchrone Kommunikation zwischen Anwendungen. Eine App sendet Nachrichten an eine Warteschlange, die die Nachrichten speichert. Die empfangende Anwendung stellt dann eine Verbindung mit der Warteschlange her und liest die Nachrichten.

Themen unterstützen das Veröffentlichen-Abonnieren-Muster, das eine 1:n-Beziehung zwischen dem Absender der Nachricht und den Empfängern der Nachricht ermöglicht.