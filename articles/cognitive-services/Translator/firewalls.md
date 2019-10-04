---
title: Übersetzung hinter Firewalls – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: Übersetzen Sie mithilfe der Textübersetzungs-API hinter IP-Firewalls.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 3d5c775d24c89d126962b6c4bccb4d5a572801ac
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69906762"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Übersetzung hinter IP-Firewalls mithilfe der Textübersetzungs-API

Die Textübersetzungs-API kann hinter Firewalls entweder mithilfe der Domänennamen- oder der IP-Filterung Übersetzungen durchführen. Die Domänennamenfilterung ist hierbei die bevorzugte Methode. Es wird **nicht empfohlen**, Microsoft Translator hinter einer IP-gefilterten Firewall auszuführen. Die Unterstützung für dieses Setup wird in Zukunft möglicherweise ohne Vorankündigung eingestellt.

## <a name="translator-ip-addresses"></a>IP-Adressen für Translator
Die IP-Adressen für die Microsoft Textübersetzungs-API (api.cognitive.microsofttranslator.com) lauten ab dem 21. August 2019 wie folgt:

* **Asien-Pazifik:** 20.40.125.208, 20.43.88.240, 20.184.58.62, 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99, 51.105.170.64, 52.155.218.251
* **Nordamerika:** 40.90.139.36, 40.90.139.2, 40.119.2.134, 52.224.200.129, 52.249.207.163


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Übersetzung hinter IP-Firewalls in Ihrem Textübersetzungs-API-Aufruf](reference/v3-0-translate.md)
