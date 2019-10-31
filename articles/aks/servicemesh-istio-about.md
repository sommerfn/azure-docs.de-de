---
title: Übersicht über Istio
description: Verschaffen Sie sich einen Überblick über Istio.
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 3db2a3d75df08e7c668b1983ad9b4493446475df
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529866"
---
# <a name="istio"></a>Istio

## <a name="overview"></a>Übersicht

[Istio][istio] ist ein anpassbares und erweiterbares Dienstmesh mit vollem Funktionsumfang.

## <a name="architecture"></a>Architecture

Istio stellt eine Datenebene bereit, die aus [Envoy][envoy-proxy]-basierten Sidecars besteht. Diese intelligenten Proxys steuern den gesamten ein- und ausgehenden Netzwerkdatenverkehr Ihrer Mesh-Apps und -Workloads.

Die Steuerungsebene verwaltet die Konfiguration, die Richtlinie und die Telemetrie über die folgenden [Komponenten][what-is-istio]:

- **Mixer** – Erzwingt Zugriffssteuerung- und Nutzungsrichtlinien. Erfasst Telemetriedaten von den Proxys, die mithilfe von Push in [Prometheus][prometheus] übertragen werden.

- **Pilot** – Stellt Richtlinien/Konfiguration zur Dienstermittlung und Datenverkehrsverwaltung für die Proxys bereit.

- **Citadel** – Stellt Identitäts- und Sicherheitsfunktionen bereit, die mTLS zwischen Diensten ermöglichen.

- **Galley** – Abstrahiert die Konfiguration und stellt sie für Komponenten bereit.

Das folgende Architekturdiagramm veranschaulicht, wie die verschiedenen Komponenten auf der Daten- und Steuerungsebene interagieren.


![Übersicht über Istio-Komponenten und die Istio-Architektur](media/servicemesh/istio/about-architecture.png)


## <a name="selection-criteria"></a>Auswahlkriterien

Beim Auswerten von Istio für Ihre Workloads ist es wichtig, die folgenden Bereiche zu verstehen und zu berücksichtigen:

- [Designziele](#design-goals)
- [Capabilities](#capabilities)
- [Szenarios](#scenarios)


### <a name="design-goals"></a>Designziele

Die folgenden Designziele [gelten][design-goals] für das Istio-Projekt:

- **Maximieren der Transparenz** – Ermöglichen Sie die Einführung mit minimalem Arbeitsaufwand, um einen echten Nutzen des Systems zu erzielen.

- **Erweiterbarkeit** – Eine Vergrößerung und Anpassung an sich verändernde Anforderungen muss möglich sein.

- **Portabilität** – Ermöglichen Sie eine problemlose Ausführung in verschiedenen Arten von Umgebungen, wie etwa in der Cloud oder lokal.

- **Einheitlichkeit der Richtlinien** – Die Richtlinien müssen über verschiedene Ressourcen hinweg konsistent sein.


### <a name="capabilities"></a>Funktionen

Istio bietet die folgenden Funktionen:

- **Mesh** – Gateways (Multicluster), virtuelle Computer (Mesherweiterung)

- **Datenverkehrsverwaltung** – Routing, Aufteilung, Timeouts, Schutzschalter, Wiederholungen, Eingang, Ausgang

- **Richtlinien** – Zugriffssteuerung, Ratenbegrenzung, Kontingent, benutzerdefinierte Richtlinienadapter

- **Sicherheit** – Authentifizierung (jwt), Autorisierung, Verschlüsselung (mTLS), externe Zertifizierungsstelle (HashiCorp Vault)

- **Einblick** – „Golden Metrics“, Spiegelung, Ablaufverfolgung, benutzerdefinierte Adapter, Prometheus, Grafana

### <a name="scenarios"></a>Szenarien

Istio wird für folgende Szenarien empfohlen:

- Anfordern von Erweiterbarkeit und umfangreichen Funktionen

- Mesherweiterung zum Einbeziehen VM-basierter Workloads

- Multicluster-Dienstmesh

## <a name="next-steps"></a>Nächste Schritte

In der folgenden Dokumentation wird beschrieben, wie Sie Istio in Azure Kubernetes Service (AKS) installieren können:

> [!div class="nextstepaction"]
> [Installieren von Istio in Azure Kubernetes Service (AKS)][istio-install]

Sie können auch weitere Istio-Konzepte und zusätzliche Bereitstellungsmodelle kennenlernen:

- [Istio-Konzepte][what-is-istio]
- [Istio-Bereitstellungsmodelle][deployment-models]

<!-- LINKS - external -->
[istio]: https://istio.io
[what-is-istio]: https://istio.io/docs/concepts/what-is-istio/
[design-goals]: https://istio.io/docs/concepts/what-is-istio/#design-goals
[deployment-models]: https://istio.io/docs/concepts/deployment-models/

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[istio-install]: ./servicemesh-istio-install.md
