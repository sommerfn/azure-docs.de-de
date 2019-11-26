---
title: Einschränkungen für Windows Server-Knotenpools in Azure Kubernetes Service (AKS)
description: Informationen zu den bekannten Einschränkungen beim Ausführen von Windows Server-Knotenpools und Anwendungsworkloads in Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: 2cec24e5ea50360531e247b37369802f3ef4e336
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990294"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Aktuelle Einschränkungen für Windows Server-Knotenpools und Anwendungsworkloads in Azure Kubernetes Service (AKS)

In Azure Kubernetes Service (AKS) können Sie einen Knotenpool erstellen, der Windows Server als Gastbetriebssystem auf den Knoten ausführt. Auf diesen Knoten können native Windows-Containeranwendungen ausgeführt werden, z. B. die in .NET Framework integrierten Anwendungen. Da es große Unterschiede in der Art und Weise gibt, wie Container unter Linux- und Windows-Betriebssystemen unterstützt werden, sind einige allgemeine Kubernetes- und podbezogene Funktionen derzeit nicht für Windows-Knotenpools verfügbar.

In diesem Artikel werden einige Einschränkungen und Betriebssystemkonzepte für Windows Server-Knoten in AKS erläutert. Knotenpools für Windows Server befinden sich derzeit in der Vorschauphase.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen hierzu finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="which-windows-operating-systems-are-supported"></a>Welche Windows-Betriebssysteme werden unterstützt?

AKS verwendet Windows Server 2019 als Host-Betriebssystemversion und unterstützt nur die Prozessisolation. Mit anderen Windows Server-Versionen erstellte Containerimages werden nicht unterstützt. [Versionskompatibilität von Windows-Containern][windows-container-compat]

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Gibt es bei Kubernetes Unterschiede zwischen Windows und Linux?

Die Unterstützung für Windows Server-Knotenpools umfasst einige Einschränkungen, die Teil des Upstreamprojekts „Windows Server in Kubernetes“ sind. Diese Einschränkungen sind nicht spezifisch für AKS. Weitere Informationen zu dieser Upstreamunterstützung für Windows Server in Kubernetes finden Sie im Abschnitt [Supported Functionality and Limitations][upstream-limitations] (Unterstützte Funktionen und Einschränkungen) des Dokuments [Intro to Windows support in Kubernetes][intro-windows] (Einführung in die Unterstützung von Windows in Kubernetes).

Kubernetes ist von jeher auf Linux ausgerichtet. Viele auf der Upstreamwebsite [Kubernetes.io][kubernetes] verwendete Beispiele sind zur Verwendung auf Linux-Knoten vorgesehen. Wenn Sie Bereitstellungen erstellen, bei denen Windows Server-Container verwendet werden, müssen Sie folgende Punkte auf Betriebssystemebene beachten:

- **Identität**: Linux erkennt einen Benutzer anhand einer ganzzahligen Benutzer-ID (UID). Ein Benutzer hat auch einen alphanumerischen Benutzernamen für die Anmeldung, der von Linux in die UID des Benutzers übersetzt wird. Auf ähnliche Weise erkennt Linux eine Benutzergruppe anhand einer ganzzahligen Gruppen-ID (GID) und übersetzt einen Gruppennamen in die entsprechende GID.
    - Unter Windows Server wird eine umfangreichere binäre Sicherheits-ID (SID) verwendet, die in der Windows Security Access Manager-Datenbank (SAM) gespeichert wird. Diese Datenbank wird weder zwischen dem Host und den Containern noch zwischen den einzelnen Containern freigegeben.
- **Dateiberechtigungen:** Unter Windows-Server wird eine Zugriffssteuerungsliste basierend auf SIDs anstelle einer Bitmaske von Berechtigungen sowie UID und GID verwendet.
- **Dateipfade:** Als Konvention wird unter Windows Server „\“ anstelle von „/“ verwendet.
    - Geben Sie in Podspezifikationen, in denen Volumes eingebunden werden, den Pfad für Windows Server-Container korrekt ein. Geben Sie beispielsweise anstelle des Bereitstellungspunkts */mnt/volume* in einem Linux-Container einen Laufwerkbuchstaben und Speicherort ein, z. B. */K/Volume* zum Einbinden als Laufwerk *K:* .

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Welche Typen von Datenträgern werden für Windows unterstützt?

Azure-Datenträger und Azure Files sind die unterstützten Volumetypen, auf die als NTFS-Volumes im Windows Server-Container zugegriffen wird.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>Kann ich nur Windows-basierte Cluster in AKS ausführen?

Die Masterknoten (die Steuerungsebene) in einem AKS-Cluster werden von AKS als Dienst gehostet. Die Daten werden nicht für das Betriebssystem der Knoten verfügbar gemacht, das als Host für die Masterkomponenten dient. Alle AKS-Cluster werden mit einem standardmäßigen ersten Knotenpool erstellt, der auf Linux basiert. Dieser Knotenpool enthält Systemdienste, die für die Funktionsweise des Clusters erforderlich sind. Es wird empfohlen, mindestens zwei Knoten im ersten Knotenpool auszuführen, um die Zuverlässigkeit des Clusters und die Fähigkeit zum Ausführen von Clustervorgängen sicherzustellen. Der erste Linux-basierte Knotenpool kann nur gelöscht werden, wenn der AKS-Cluster selbst gelöscht wird.

## <a name="what-network-plug-ins-are-supported"></a>Welche Netzwerk-Plug-Ins werden unterstützt?

AKS-Cluster mit Windows-Knotenpools müssen das (erweiterte) Azure CNI-Netzwerkmodell verwenden. Kubenet Basic-Netzwerke werden nicht unterstützt. Weitere Informationen zu den Unterschieden der Netzwerkmodelle finden Sie unter [Netzwerkkonzepte für Anwendungen in AKS][azure-network-models]. Das Azure CNI-Netzwerkmodell erfordert zusätzliche Planung und Überlegungen in Bezug auf die Verwaltung von IP-Adressen. Weitere Informationen zum Planen und Implementieren von Azure CNI finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in AKS][configure-azure-cni].

## <a name="can-i-change-the-min--of-pods-per-node"></a>Kann ich die Mindestanzahl von Pods pro Knoten ändern?

Derzeit müssen zwingend mindestens 30 Pods festgelegt werden, um die Zuverlässigkeit Ihrer Cluster sicherzustellen.

## <a name="how-do-patch-my-windows-nodes"></a>Wie kann ich meine Windows-Knoten patchen?

Windows Server-Knoten in AKS müssen *aktualisiert* werden, um die neuesten Patches und Updates zu erhalten. Windows-Updates sind auf Knoten in AKS nicht aktiviert. AKS gibt neue Knotenpoolimages frei, sobald Patches verfügbar sind. Die Kunden sind dafür verantwortlich, Knotenpools zu aktualisieren, um in Bezug auf Patches und Hotfixes auf dem neuesten Stand zu bleiben. Dies gilt auch für die verwendete Kubernetes-Version. In den AKS-Versionshinweisen wird angegeben, wann neue Versionen verfügbar sind. Weitere Informationen zum Upgrade eines Windows Server-Knotenpools finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].

> [!NOTE]
> Das aktualisierte Windows Server-Image wird nur verwendet, wenn vor der Aktualisierung des Knotenpools ein Clusterupgrade (ein Upgrade der Steuerungsebene) durchgeführt wird.
>

## <a name="how-many-node-pools-can-i-create"></a>Wie viele Knotenpools kann ich erstellen?

Der AKS-Cluster kann maximal acht (8) Knotenpools umfassen. In diesen Knotenpools können maximal 400 Knoten enthalten sein. Weitere Informationen finden Sie unter [Einschränkungen von Knotenpools][nodepool-limitations].

## <a name="what-can-i-name-my-windows-node-pools"></a>Wie kann ich meine Windows-Knotenpools benennen?

Der Name darf maximal sechs (6) Zeichen umfassen. Dies ist eine aktuelle AKS-Beschränkung.

## <a name="are-all-features-supported-with-windows-nodes"></a>Werden alle Features für Windows-Knoten unterstützt?

Netzwerkrichtlinien und kubenet werden für Windows-Knoten derzeit nicht unterstützt. 

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Kann ich Eingangscontroller auf Windows-Knoten ausführen?

Ja. Ein Eingangscontroller, der Windows Server-Container unterstützt, kann auf Windows-Knoten in AKS ausgeführt werden.

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Kann ich Azure Dev Spaces mit Windows-Knoten verwenden?

Azure Dev Spaces ist derzeit nur für Linux-basierte Knotenpools verfügbar.

## <a name="can-my-windows-server-containers-use-gmsa"></a>Können meine Windows Server-Container gMSA verwenden?

Die Unterstützung für gruppenverwaltete Dienstkonten (Group Managed Service Accounts, gMSA) ist derzeit in AKS nicht verfügbar.

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Kann ich Azure Monitor für Container mit Windows-Knoten und -Containern verwenden?

Ja, das können Sie, aber Azure Monitor erfasst keine Protokolle (stdout) von Windows-Containern. Sie können weiterhin an den Livestream von stdout-Protokollen aus einem Windows-Container anfügen.

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>Wie sieht es aus, wenn ich ein Feature benötige, das nicht unterstützt wird?

Wir arbeiten daran, alle von Ihnen benötigten Features für Windows in AKS zu integrieren. Wenn Sie jedoch auf Lücken stoßen, bietet das Open-Source-Upstreamprojekt [aks-engine][aks-engine] eine einfache und vollständig anpassbare Methode zum Ausführen von Kubernetes in Azure, einschließlich der Windows-Unterstützung. Sehen Sie sich unbedingt unsere [AKS-Roadmap][aks-roadmap] mit den kommenden Features an.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie zum Einstieg in Windows Server-Container in AKS [einen Knotenpool, auf dem Windows Server in AKS ausgeführt wird][windows-node-cli].

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[preview-support]: support-policies.md#preview-features-or-feature-flags
[windows-container-compat]: https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility#windows-server-2019-host-os-compatibility
