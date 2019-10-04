---
title: Was ist Azure Service Health?
description: Personalisierte Informationen dazu, wie Ihre Azure-Apps von aktuellen und zukünftigen Problemen und Wartungen der Azure-Dienste betroffen sind.
author: stephbaron
ms.author: stbaron
services: service-health
ms.service: service-health
ms.topic: article
ms.date: 05/10/2019
ms.openlocfilehash: 058a171766680f09eaf4de14ddd25235020b1ba4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081811"
---
# <a name="what-is-azure-service-health"></a>Was ist Azure Service Health?

Azure stellt eine Reihe von Benutzeroberflächen bereit, in denen Sie ständig über den Zustand Ihrer Cloudressourcen informiert werden. Diese Informationen beinhalten aktuelle und zukünftige Probleme, etwa Ereignisse, die sich auf die Dienstverfügbarkeit auswirken, geplante Wartung und andere Änderungen, die die Verfügbarkeit Ihres Systems beeinflussen können.

Azure Service Health ist eine Kombination aus drei getrennten kleineren Diensten.

[Azure-Status](azure-status-overview.md) informiert Sie über Dienstausfälle in Azure auf der **[Seite „Azure-Status“](https://status.azure.com)** . Die Seite ist eine globale Ansicht der Integrität von allen Azure-Diensten in allen Azure-Regionen. Die Statusseite ist eine gute Referenz für Vorfälle mit umfangreichen Auswirkungen. Aktuellen Azure-Benutzer wird jedoch dringend empfohlen, Azure Service Health zu nutzen, um über Azure-Vorfälle und -Wartungen auf dem Laufenden zu bleiben.

[Azure Service Health](service-health-overview.md) bietet eine personalisierte Darstellung der Integrität der von Ihnen verwendeten Azure-Dienste und -Regionen. In Azure Service Health finden Sie beispielsweise Informationen zu Ausfällen und geplanten Wartungen sowie andere Integritätsempfehlungen, da die authentifizierte Azure Service Health-Umgebung weiß, welche Dienste und Ressourcen Sie derzeit verwenden. Um Service Health optimal nutzen zu können, wird die Einrichtung von Service Health-Warnungen empfohlen, die Sie über Ihren bevorzugten Kommunikationskanal informieren, wenn Dienstprobleme, geplante Wartungen oder andere Änderungen möglicherweise die von Ihnen verwendeten Azure-Dienste und -Regionen beeinträchtigen.

[Azure Resource Health](resource-health-overview.md) enthält Informationen zur Integrität von einzelnen Cloudressourcen, etwa eine bestimmte Instanz eines virtuellen Computers. Mithilfe von Azure Monitor können Sie darüber hinaus Warnungen konfigurieren, die Sie über Änderungen der Verfügbarkeit Ihrer Cloudressourcen informieren. Dank Benachrichtigungen von Azure Resource Health und Azure Monitor sind Sie jederzeit bestens über die Verfügbarkeit Ihrer Ressourcen informiert und können schnell erkennen, ob ein Problem auf einen Fehler auf Ihrer Seite oder auf ein Ereignis der Azure-Plattform zurückzuführen ist.

Zusammen bieten Ihnen diese Oberflächen einen umfassenden Einblick in die Integrität von Azure mit der Granularität, die für Sie relevant ist.

**Sehen Sie sich eine Übersicht über die Seite „Azure-Status“ sowie über Azure Service Health und Azure Resource Health an.**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OgX6]
