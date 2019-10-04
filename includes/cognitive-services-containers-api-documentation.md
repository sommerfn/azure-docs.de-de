---
author: IEvangelist
ms.author: dapine
ms.date: 08/22/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 081155c8984f39cc9cc8e905eb108c07dee98d2e
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034426"
---
## <a name="validate-that-a-container-is-running"></a>Überprüfen auf aktive Container 

Es gibt mehrere Möglichkeiten zu überprüfen, ob ein Container aktiv ist. Suchen Sie die *externe IP*-Adresse und den verfügbar gemachten Port des betreffenden Containers, und öffnen Sie Ihren bevorzugten Webbrowser. Verwenden Sie die verschiedenen URLs der untenstehenden Anforderungen, um zu überprüfen, ob der Container ausgeführt wird. Die unten aufgeführten Beispiel-URLs für Anforderungen sind `http://localhost:5000`, aber Ihr spezifischer Container kann variieren. Denken Sie daran, dass Sie sich auf die *externe IP-Adresse* Ihres Containers und den verfügbar gemachten Port verlassen müssen.

| Anfrage-URL | Zweck |
|--|--|
| `http://localhost:5000/` | Der Container stellt eine Homepage bereit. |
| `http://localhost:5000/status` | Anforderung mit HTTP GET, um zu überprüfen, ob der Container aktiv ist, ohne eine Endpunktabfrage zu verursachen. Diese Anforderung kann für [Live- und Bereitschaftstests](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) von Kubernetes verwendet werden. |
| `http://localhost:5000/swagger` | Der Container stellt eine umfassende Dokumentation für die Endpunkte sowie die Funktion **Jetzt ausprobieren** bereit. Diese Funktion ermöglicht Ihnen die Eingabe Ihrer Einstellungen in einem webbasierten HTML-Formular, sodass Sie die Abfrage ausführen können, ohne Code schreiben zu müssen. Nach der Rückgabe der Abfrage wird ein cURL-Beispielbefehl bereitgestellt, der das erforderliche Format für HTTP-Header und -Text veranschaulicht. |

![Homepage des Containers](./media/cognitive-services-containers-api-documentation/container-webpage.png)
