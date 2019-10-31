---
title: Informationen zu Dienstnetzen
description: Enthält eine Übersicht über Dienstnetze, ihre Architektur und Funktionen sowie die Kriterien, die Sie bei der Auswahl für die Bereitstellung berücksichtigen sollten.
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 226a4e5cf97be2e23ef13a95b80be07b7fbf5d7a
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529926"
---
# <a name="about-service-meshes"></a>Informationen zu Dienstnetzen

Ein Dienstnetz verfügt für Ihre Workloads über Funktionen für die Bereiche Datenverkehrsverwaltung, Resilienz, Richtlinie, Sicherheit, sichere Identität und Einblick. Ihre Anwendung ist von diesen betriebsbezogenen Funktionen abgekoppelt, und sie werden vom Dienstnetz von der Anwendungsschicht herunter auf die Infrastrukturebene verschoben.

## <a name="scenarios"></a>Szenarien

Hier sind einige Szenarien angegeben, die Sie bei Verwendung eines Dienstnetzes für Ihre Workloads aktivieren können:

- **Verschlüsselung des gesamten Datenverkehrs im Cluster**: Aktivieren Sie gegenseitiges TLS (mTLS) zwischen angegebenen Diensten im Cluster. Dies kann auf den ein- und ausgehenden Datenverkehr im Umkreisnetzwerk erweitert werden. Es wird eine sichere Standardoption bereitgestellt, ohne dass Änderungen am Anwendungscode und der Infrastruktur erforderlich sind.

- **Canary und schrittweise Rollouts**: Geben Sie Bedingungen an, um einen Teil des Datenverkehrs an eine Reihe neuer Dienste im Cluster zu leiten. Nachdem der Test des Canary-Release erfolgreich abgeschlossen wurde, können Sie die bedingte Weiterleitung entfernen und den Prozentsatz des gesamten Datenverkehrs, der an den neuen Dienst gesendet wird, schrittweise erhöhen. Schließlich wird dann der gesamte Datenverkehr an den neuen Dienst geleitet.

- **Verwaltung und Bearbeitung des Datenverkehrs**: Erstellen Sie eine Richtlinie für einen Dienst, mit der für den gesamten Datenverkehr ein Ratenlimit auf eine Version eines Diensts von einem bestimmten Ursprungsort durchgesetzt wird. Sie können auch eine Richtlinie erstellen, mit der eine Wiederholungsstrategie auf Fehlerklassen zwischen angegebenen Diensten angewendet wird. Spiegeln Sie während einer Migration oder zum Debuggen von Problemen Livedatenverkehr auf neuen Versionen von Diensten. Fügen Sie in einer Testumgebung Fehler zwischen Diensten ein, um die Resilienz zu testen.

- **Einblick**: Verschaffen Sie sich einen Einblick in der Verbindung Ihrer Dienste mit dem Datenverkehr, der zwischen den Diensten ausgetauscht wird. Rufen Sie Metriken, Protokolle und Ablaufverfolgungen für den gesamten Datenverkehr im Cluster und für den Ein- und Ausgang ab. Fügen Sie Ihren Anwendungen Funktionen für die verteilte Ablaufverfolgung hinzu.

## <a name="architecture"></a>Architecture

Ein Dienstnetz besteht normalerweise aus einer Steuerungsebene und der Datenebene.

Die **Steuerungsebene** verfügt über einige Komponenten, die die Verwaltung des Dienstnetzes unterstützen. Dies umfasst in der Regel eine Verwaltungsschnittstelle, bei der es sich um eine Benutzeroberfläche oder eine API handeln kann. Meist sind auch Komponenten zum Verwalten der Regel- und Richtliniendefinitionen vorhanden, mit denen definiert wird, wie bestimmte Funktionen vom Dienstnetz implementiert werden sollten. Es gibt auch Komponenten, mit denen Sicherheitsaspekte wie sichere Identität und Zertifikate für gegenseitiges TLS (mTLS) verwaltet werden. Darüber hinaus verfügen Dienstnetze normalerweise auch über eine Metrik- oder Einblickkomponente, mit der Metriken und Telemetriedaten aus den Workloads gesammelt und aggregiert werden.

Die **Datenebene** besteht meist aus einem Proxy, der als „Sidecar“ auf transparente Weise in Ihre Workloads eingefügt wird. Dieser Proxy ist so konfiguriert, dass der gesamte ein- und ausgehende Netzwerkdatenverkehr des Pods, der Ihre Workload enthält, gesteuert wird. Der Proxy kann dann so konfiguriert werden, dass Datenverkehr per mTLS geschützt und eine dynamische Weiterleitung durchgeführt wird und Richtlinien darauf angewendet werden können. Darüber hinaus können Informationen zu Metriken und zur Ablaufverfolgung gesammelt werden. 

![Typische Dienstnetzarchitektur](media/servicemesh/typical-architecture.png)

## <a name="capabilities"></a>Funktionen

Jedes Dienstnetz ist speziell auf die Unterstützung bestimmter Szenarien ausgelegt. Normalerweise ist es aber meist der Fall, dass einige oder alle der folgenden Funktionen implementiert werden.

### <a name="traffic-management"></a>Datenverkehrsverwaltung 

- **Protokoll**: Layer 7 (HTTP, GRPC)
- **Dynamisches Routing**: Bedingtheit, Gewichtung, Spiegelung
- **Resilienz**: Timeouts, Wiederholungen, Trennschalter
- **Richtlinie**: Zugriffssteuerung, Ratenlimits, Kontingente
- **Tests**: Fault Injection

### <a name="security"></a>Sicherheit

- **Verschlüsselung**: mTLS, Zertifikatverwaltung, externe Zertifizierungsstelle
- **Sichere Identität**: SPIFFE oder Ähnliches
- **Authentifizierung**: Authentifizierung, Autorisierung

### <a name="observability"></a>Einblick

- **Metriken**: „Golden Metrics“, Prometheus, Grafana
- **Ablaufverfolgung**: Workloadübergreifende Ablaufverfolgungen
- **Datenverkehr**: Cluster, Ein-/Ausgang

### <a name="mesh"></a>Netz

- **Unterstützte Computevorgänge**: Kubernetes, virtuelle Computer
- **Multicluster**: Gateways, Verbund

## <a name="selection-criteria"></a>Auswahlkriterien

Stellen Sie vor dem Auswählen eines Dienstnetzes sicher, dass Sie ausreichend mit Ihren Anforderungen und den Gründen für die Installation eines Dienstnetzes vertraut sind. Versuchen Sie, die folgenden Fragen zu stellen.

- **Ist ein Eingangscontroller für meine Zwecke ausreichend?** - In einigen Fällen reicht es aus, eine Funktion wie A/B-Tests oder die Datenverkehrsaufteilung für eingehenden Datenverkehr zu verwenden, um das gewünschte Szenario zu unterstützen. Erhöhen Sie die Komplexität Ihrer Anwendung nicht, wenn sich daraus keine Vorteile ergeben.

- **Können meine Workloads und meine Umgebung den zusätzlichen Aufwand verarbeiten?** - Für alle herkömmlichen Komponenten, die zur Unterstützung des Dienstnetzes benötigt werden, sind zusätzliche Ressourcen in Bezug auf CPU und Arbeitsspeicher erforderlich. Darüber hinaus führen alle Proxys und die zugehörigen Richtlinienüberprüfungen für Ihren Datenverkehr zu mehr Latenz. Falls Sie über Workloads verfügen, die sehr empfindlich auf Latenz reagieren, oder falls die zusätzlichen Ressourcen für die Dienstnetzkomponenten nicht bereitgestellt werden können, sollten Sie ggf. eine andere Lösung erwägen.

- **Wird die Komplexität unnötig erhöht?** - Falls der Grund für die Installation eines Dienstnetzes die Erzielung einer Funktion ist, die nicht unbedingt von entscheidender Bedeutung für das Geschäft bzw. den Betrieb ist, sollten Sie abwägen, ob sich der Mehraufwand für Installation, Wartung und Konfiguration lohnt.

- **Kann für die Einführung ein inkrementeller Ansatz verwendet werden?** - Einige Dienstnetze, mit denen viele Funktionen bereitgestellt werden, können auch mit einem inkrementellen Ansatz eingeführt werden. Installieren Sie nur die Komponenten, die Sie benötigen, um den Erfolg sicherzustellen. Wenn Sie später dann Erfahrung gesammelt haben und zusätzliche Funktionen erforderlich sind, können Sie diese separat erkunden. Widerstehen Sie der Versuchung, *alles* gleich am Anfang zu installieren.

Falls Sie nach sorgfältiger Überlegung die Entscheidung treffen, dass Sie zum Bereitstellen der erforderlichen Funktionen ein Dienstnetz benötigen, lautet die nächste Frage: *Welches Dienstnetz soll verwendet werden?*

Sehen Sie sich die folgenden Bereiche an, um zu ermitteln, welche am ehesten auf Ihre Anforderungen zutreffen. Auf diese Weise können Sie die beste Wahl für Ihre Umgebung und die Workloads treffen. Der Abschnitt [Nächste Schritte](#next-steps) enthält Links zu ausführlicheren Informationen zu bestimmten Dienstnetzen und deren Eignung für diese Bereiche.

- **Technik**: Datenverkehrsverwaltung, Richtlinie, Sicherheit, Einblick

- **Business**: Commercial Support, Foundation (CNCF), OSS-Lizenz, Governance

- **Betrieb**: Installation/Upgrades, Ressourcenanforderungen, Leistungsanforderungen, Integrationen (Metriken, Telemetriedaten, Dashboards, Tools, SMI), gemischte Workloads (Linux- und Windows-Knotenpools), Compute (Kubernetes, virtuelle Computer), Multicluster

- **Sicherheit**: Authentifizierung, Identität, Zertifikatsverwaltung und Rotation, Plug-fähige externe Zertifizierungsstelle


## <a name="next-steps"></a>Nächste Schritte

Die folgende Dokumentation enthält weitere Informationen zu Dienstnetzen, die Sie in Azure Kubernetes Service (AKS) ausprobieren können:

> [!div class="nextstepaction"]
> [Weitere Informationen zu Istio...][istio-about]

> [!div class="nextstepaction"]
> [Weitere Informationen zu Linkerd...][linkerd-about]

> [!div class="nextstepaction"]
> [Weitere Informationen zu Consul...][consul-about]

Sie können auch Service Mesh Interface (SMI), eine Standardschnittstelle für Dienstnetze in Kubernetes, erkunden:

- [Service Mesh Interface (SMI)][smi]


<!-- LINKS - external -->
[smi]: https://smi-spec.io/

<!-- LINKS - internal -->
[istio-about]: ./servicemesh-istio-about.md
[linkerd-about]: ./servicemesh-linkerd-about.md
[consul-about]: ./servicemesh-consul-about.md