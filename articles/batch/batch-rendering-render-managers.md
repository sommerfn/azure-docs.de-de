---
title: 'Unterstützung von Render-Managern: Azure Batch'
description: Verwenden von Azure für das Rendering mit Integration eines Azure Batch-Render-Managers
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: eb3ce47e5ffed697392065a1faacbbfaec19f4d1
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983668"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Verwenden von Azure Batch mit Renderfarm-Managern

Wenn Sie eine vorhandene lokale Renderfarm nutzen, ist die Wahrscheinlichkeit hoch, dass die Kapazität und die Renderaufträge der Renderfarm mit einem Render-Manager gesteuert werden.

In Azure werden entweder die integrierte Unterstützung oder Add-Ons für beliebte Render-Manager bereitgestellt. Sie können dann Azure-VMs hinzufügen und entfernen, z.B. auch VMs mit nutzungsbasierter Anwendungslizenzierung und VMs mit niedriger Priorität.

Die folgenden Render-Manager werden unterstützt:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](https://www.royalrender.de/)
* [Thinkbox Deadline](https://deadline.thinkboxsoftware.com/)

## <a name="azure-render-hub"></a>Azure Render Hub

Azure Render Hub vereinfacht die Erstellung und Verwaltung von Azure-Renderfarmen.  Render Hub bietet native Unterstützung für PipelineFx Qube und Deadline 10.  Weitere Informationen und detaillierte Anweisungen finden Sie im [GitHub-Repository](https://github.com/Azure/azure-render-hub).

## <a name="using-azure-with-pipelinefx-qube"></a>Verwenden von Azure mit PipelineFX Qube

Azure Render Hub unterstützt beliebte Render-Manager einschließlich Deadline.  Anweisungen zum Bereitstellen und Verwenden von Render Hub finden Sie im [GitHub-Repository](https://github.com/Azure/azure-render-hub).

Skripts und Anleitungen zum Aktivieren von Azure Batch-Pool-VMs als Qube-Worker sind im [GitHub-Repository](https://github.com/Azure/azure-qube) ebenfalls verfügbar.

## <a name="using-azure-with-royal-render"></a>Verwenden von Azure mit Royal Render

Royal Render verfügt standardmäßig über eine Azure- und Azure Batch-Integration, sodass Sie eine Renderfarm mit Azure-basierten VMs erweitern können. Eine Zusammenfassung hierzu finden Sie in den [Hilfedateien](https://www.royalrender.de/help8/index.html?Cloudrendering.html).

Ein Beispiel für einen Royal Render-Kunden, der die Azure-Integration nutzt, finden Sie unter dem [Jellyfish Pictures-Kundenbericht](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Verwenden von Azure mit Thinkbox Deadline

Azure Render Hub unterstützt beliebte Render-Manager einschließlich Deadline.  Anweisungen zum Bereitstellen und Verwenden von Render Hub finden Sie im [GitHub-Repository](https://github.com/Azure/azure-render-hub).

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die Azure Batch-Integration für Ihren Render-Manager, indem Sie das entsprechende Plug-In und die Anleitung auf GitHub verwenden, falls zutreffend.
