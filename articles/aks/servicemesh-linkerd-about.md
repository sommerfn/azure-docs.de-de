---
title: Übersicht über Linkerd
description: Hier finden Sie eine Übersicht über Linkerd.
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 9d0a65f2cf557120ec5a551494e697c876ad5c59
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529774"
---
# <a name="linkerd"></a>Linkerd

## <a name="overview"></a>Übersicht

[Linkerd][linkerd] ist ein benutzerfreundliches und leichtes Dienstmesh.

## <a name="architecture"></a>Architecture

Linkerd stellt eine Datenebene bereit, die aus ultraleichten auf [Linkerd][linkerd-proxy] spezialisierten Proxysidecars besteht. Diese intelligenten Proxys steuern den gesamten ein- und ausgehenden Netzwerkdatenverkehr Ihrer Mesh-Apps und -Workloads. Die Proxys machen Metriken auch über [Prometheus][prometheus]-Metrikendpunkte verfügbar.

Die Steuerungsebene verwaltet die Konfiguration und die aggregierte Telemetrie über die folgenden [Komponenten][linkerd-architecture]:

- **Controller** – Stellt eine API zur Verfügung, die die Linkerd-CLI und das Dashboard steuert. Stellt eine Konfiguration für Proxys bereit.

- **Tap** – Richtet Überwachungen in Echtzeit zu Anforderungen und Antworten ein.

- **Identity** – Stellt Identitäts- und Sicherheitsfunktionen bereit, die mTLS zwischen Diensten ermöglichen.

- **Web** – Stellt das Linkerd-Dashboard bereit.


Das folgende Architekturdiagramm veranschaulicht, wie die verschiedenen Komponenten auf der Daten- und Steuerungsebene interagieren.


![Übersicht über die Linkerd-Komponenten und die Linkerd-Architektur](media/servicemesh/linkerd/about-architecture.png)


## <a name="selection-criteria"></a>Auswahlkriterien

Bei der Auswertung von Linkerd für Ihre Workloads ist es wichtig, die folgenden Bereiche zu verstehen und zu berücksichtigen:

- [Entwurfsprinzipien](#design-principles)
- [Capabilities](#capabilities)
- [Szenarios](#scenarios)


### <a name="design-principles"></a>Entwurfsprinzipien

Die folgenden Entwurfsprinzipien [gelten][design-principles] für das Linkerd-Projekt:

- **Einfachheit** – Es muss benutzerfreundlich und gut verständlich sein.

- **Minimieren von Ressourcenanforderungen** – Beschränken Sie die Kosten für Leistung und Ressourcen auf ein Minimum.

- **Gute Funktionsweise** – Sorgen Sie dafür, dass vorhandene Anwendungen nicht beeinträchtigt werden und dass keine komplexe Konfiguration erforderlich ist.


### <a name="capabilities"></a>Funktionen

Linkerd bietet folgende Funktionen:

- **Mesh** – integrierte Debugoption

- **Datenverkehrsverwaltung** – Aufteilung, Timeouts, Wiederholungen, Eingang

- **Sicherheit** – Verschlüsselung (mTLS), Zertifikate werden alle 24 Stunden automatisch rotiert

- **Einblick** – „Golden Metrics“, „Tap“, Ablaufverfolgung, Dienstprofile und Metriken pro Route, Webdashboard mit Topologiediagrammen, Prometheus, Grafana


### <a name="scenarios"></a>Szenarien

Linkerd wird für folgende Szenarien empfohlen:

- Einfache Verwendung mit nur den wesentlichen Funktionsanforderungen

- Geringe Wartezeit, geringer Aufwand mit Schwerpunkt auf Einblicken und einfacher Datenverkehrsverwaltung


## <a name="next-steps"></a>Nächste Schritte

In der folgenden Dokumentation erfahren Sie, wie Sie Linkerd in Azure Kubernetes Service (AKS) installieren:

> [!div class="nextstepaction"]
> [Installieren von Linkerd in Azure Kubernetes Service (AKS)][linkerd-install]

Weitere Informationen zu Linkerd-Features und zur Linkerd-Architektur finden Sie hier:

- [Linkerd-Features][linkerd-features]
- [Linkerd-Architektur][linkerd-architecture]

<!-- LINKS - external -->
[linkerd]: https://linkerd.io/2/overview/
[linkerd-architecture]: https://linkerd.io/2/reference/architecture/
[linkerd-features]: https://linkerd.io/2/features/
[design-principles]: https://linkerd.io/2/design-principles/
[linkerd-proxy]: https://github.com/linkerd/linkerd2-proxy

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[linkerd-install]: ./servicemesh-linkerd-install.md