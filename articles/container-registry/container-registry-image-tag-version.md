---
title: Taggen und Versionsverwaltung von Images in der Azure Container Registry
description: Bewährte Methoden für das Taggen und die Versionsverwaltung von Docker-Containerimages
services: container-registry
author: stevelasker
ms.service: container-registry
ms.topic: article
ms.date: 07/10/2019
ms.author: stevelas
ms.openlocfilehash: ea7c0831f4ecc345cbcd8a9b8eb6d6566e8c5023
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297762"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Empfehlungen für das Taggen und die Versionsverwaltung von Containerimages

Wenn Sie die Bereitstellung von Containerimages in eine Containerregistrierung pushen und die Images dann bereitstellen, benötigen Sie eine Strategie für das Taggen und die Versionsverwaltung der Images. In diesem Artikel werden zwei verschiedene Verfahren erläutert und beschrieben, welches Verfahren sich an verschiedenen Punkten des Lebenszyklus des Containers am besten eignet:

* **Stabile Tags**: Diese Tags können Sie wiederverwenden, um z.B. eine Haupt- und eine Nebenversion wie *mycontainerimage:1.0* zu bezeichnen.
* **Eindeutige Tags**: Sie verwenden für jedes Image, das Sie in eine Registrierung pushen, ein anderes Tag, z.B. *mycontainerimage:abc123*.

## <a name="stable-tags"></a>Stabile Tags

**Empfehlung**: Verwenden Sie stabile Tags, um **Basisimages** für Ihre Containerbuilds zu verwalten. Vermeiden Sie Bereitstellungen mit stabilen Tags, da diese Tags weiterhin Updates erhalten und dadurch Inkonsistenzen in Produktionsumgebungen verursachen können.

*Stabile Tags* bedeuten, dass ein Entwickler oder ein Buildsystem immer wieder ein Pull für ein bestimmtes Tag ausführen kann, das weiter Updates erhält. „Stabil“ bedeutet nicht, dass die Inhalte unveränderlich sind. Es bedeutet, dass das Image hinsichtlich der Absicht dieser Version stabil sein muss. Damit das Image weiter stabil bleibt, werden möglicherweise Sicherheitspatches oder Frameworkupdates für das Image ausgeführt.

### <a name="example"></a>Beispiel

Ein Frameworkteam liefert Version 1.0 aus. Das Team weiß, dass in Zukunft Updates und Nebenversionen ausgeliefert werden. Um stabile Tags für eine bestimmte Haupt- und Nebenversion zu unterstützen, richtet das Team zwei Sätze stabiler Tags ein.

* `:1`: ein stabiles Tag für die Hauptversion. `1` repräsentiert die „neueste“ Version: 1.*.
* `:1.0`: ein stabiles Tag für Version 1.0, mit dem ein Entwickler Code an Updates von 1.0 binden kann, für den kein Rollforward zu Version 1.1 ausgeführt wird, wenn diese veröffentlicht wird.

Das Team verwendet auch das `:latest`-Tag, das auf das neueste stabile Tag zeigt, unabhängig von der aktuellen Hauptversion.

Wenn Updates für das Basisimage oder irgendeine Art Wartungsupdate des Frameworks verfügbar sind, werden Images mit stabilen Tags auf den neuesten Hash aktualisiert, der das neueste stabile Release dieser Version repräsentiert.

In diesem Fall werden sowohl das Tag für die Hauptversion als auch das Tag für die Nebenversion weiterhin gewartet. Für ein Szenario mit Basisimages bedeutet dies, dass der Imagebesitzer gewartete Images bereitstellen kann.

## <a name="unique-tags"></a>Eindeutige Tags

**Empfehlung**: Verwenden Sie eindeutige Tags für **Bereitstellungen**, insbesondere in einer Umgebung, die sich über mehrere Knoten erstrecken kann. Ihr Ziel ist wahrscheinlich eine gut geplante Bereitstellung konsistenter Komponentenversionen. Wenn Ihr Container neu gestartet wird oder ein Orchestrator horizontal auf weitere Instanzen skaliert, rufen Ihre Hosts nicht versehentlich eine neuere Version ab, die inkonsistent mit den anderen Knoten wäre.

Eindeutige Tags bedeuten ganz einfach, dass jedes Image, das in eine Registrierung gepusht wird, über ein eindeutiges Tag verfügt. Tags werden nicht wiederverwendet. Es gibt verschiedene Muster, die Sie beim Generieren von eindeutigen Tags verwenden können, z.B. die folgenden:

* **Datums-/Uhrzeitstempel**: Dieser Ansatz wird recht häufig verwendet, weil Sie eindeutig erkennen können, wann das Image erstellt wurde. Aber wie erreichen Sie eine Korrelation mit Ihrem Buildsystem? Müssen Sie den Build suchen, der zur gleichen Zeit abgeschlossen wurde? In welcher Zeitzone befinden Sie sich? Sind all Ihre Buildsysteme auf UTC kalibriert?
* **Git-Commit**: Dieser Ansatz funktioniert, solange Sie keine Updates für Basisimages unterstützen. Wenn ein Basisimage aktualisiert wird, verwendet Ihr Buildsystem dennoch den gleichen Git-Commit wie beim vorherigen Build. Das Basisimage weist jedoch neue Inhalte auf. Allgemein gesagt, stellt ein Git-Commit ein *semistabiles* Tag bereit.
* **Manifesthash**: Jedes Containerimage, das in eine Containerregistrierung gepusht wird, ist mit einem Manifest verknüpft, das durch einen eindeutigen SHA-256-Hash identifiziert wird. Ein solcher Hash ist zwar eindeutig, aber lang, schwer zu lesen und nicht mit Ihrer Buildumgebung korreliert.
* **Build-ID**: Diese Option ist möglicherweise die beste, weil sie wahrscheinlich inkrementell ist und Ihnen die Korrelation mit einem bestimmten Build ermöglicht, um alle Artefakte und Protokolle zu finden. Ebenso wie ein Manifesthash ist eine solche ID allerdings möglicherweise für Menschen schwer zu lesen.

  Wenn Ihre Organisation über mehrere Buildsysteme verfügt, besteht eine Variante dieser Option darin, dem Tag den Namen des Buildsystems voranzustellen: `<build-system>-<build-id>`. So lassen sich beispielsweise Builds aus dem Jenkins-Buildsystem des API-Teams vom Azure Pipelines-Buildsystem des Webteams unterscheiden.

## <a name="next-steps"></a>Nächste Schritte

Eine ausführlichere Erläuterung der Konzepte in diesem Artikel finden Sie im Blogbeitrag [Docker Tagging: Best practices for tagging and versioning docker images](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/) (Bewährte Methoden für das Taggen und die Versionsverwaltung von Docker-Images).

Informationen dazu, wie Sie die Leistung und den kosteneffektiven Einsatz Ihrer Azure Container Registry maximieren, finden Sie unter [Bewährte Methoden für Azure Container Registry](container-registry-best-practices.md).

<!-- IMAGES -->


<!-- LINKS - Internal -->

