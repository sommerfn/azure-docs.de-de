---
title: Azure VMware-Lösung von CloudSimple – CloudSimple-Knotenkontingent
description: Beschreibt die Kontingentgrenzen für CloudSimple-Knoten und das Anfordern einer Kontingenterhöhung
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e765d7c87f2f941a5e2d558b71c4e5a71d2df9b1
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816676"
---
# <a name="cloudsimple-node-quota-limits"></a>CloudSimple-Knotenkontingentgrenzen

Die für eine Bereitstellung verfügbare Standardmenge beträgt vier Knoten, wenn Ihr Abonnement für den CloudSimple-Dienst aktiviert ist.  Sie können jeden [Knotentypen](cloudsimple-node.md) aus dem Azure-Portal bereitstellen.  Mindestens drei Knoten derselben SKU sind erforderlich, um eine Private Cloud zu erstellen.  Wenn Sie die Knoten bereitgestellt haben, wird unter Umständen ein Fehler angezeigt, wenn Sie versuchen, weitere Knoten zu bereitzustellen.

## <a name="quota-increase"></a>Kontingenterhöhung

Sie können das Knotenkontingent erhöhen, indem Sie eine Supportanfrage übermitteln. Das für Dienstvorgänge zuständige Team wird die Anforderung auswerten und gemeinsam mit Ihnen das Knotenkontingent erhöhen.  Wählen Sie die folgenden Optionen aus, wenn Sie ein neues Ticket öffnen:

* Problemtyp: **Technisch**
* Abonnement: **Ihre Abonnement-ID**
* Diensttyp: **VMware-Lösung von CloudSimple**
* Problemtyp: **Kontingent dedizierter Knoten**
* Problemuntertyp: **Erhöhen des Kontingents der dedizierten Knoten**
* Antragsteller: **Kontingenterhöhung**

Geben Sie in den Details des Supporttickets die erforderliche Anzahl von Knoten und Knoten-SKUs ein.

Sie können auch unter [azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) Ihren Microsoft-Kontobeauftragten kontaktieren, um das Knotenkontingent Ihres Abonnements zu erhöhen.  Dabei müssen Sie folgende Angaben machen:

* Abonnement-ID
* Knoten-SKU
* Anzahl der zusätzlichen Knoten, für die Sie eine Kontingenterhöhung anfordern

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen von Knoten](create-nodes.md)
* [Übersicht über CloudSimple-Knoten](cloudsimple-node.md)