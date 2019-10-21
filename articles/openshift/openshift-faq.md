---
title: Häufig gestellte Fragen zu Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Enthält Antworten auf häufige gestellte Fragen zu Microsoft Azure Red Hat OpenShift.
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 8f7349310f72c8cccc7b1906239ece3038dd7861
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249217"
---
# <a name="azure-red-hat-openshift-faq"></a>Häufig gestellte Fragen zu Azure Red Hat OpenShift

Dieser Artikel beantwortet häufig gestellte Fragen zu Microsoft Azure Red Hat OpenShift.

## <a name="how-do-i-get-started"></a>Wie fange ich an?

Bevor Sie Azure Red Hat OpenShift verwenden können, müssen Sie mindestens 4 reservierte Azure Red Hat OpenShift-Anwendungsknoten gekauft haben.

Wenn Sie Azure-Kunde sind, [erwerben Sie reservierte Azure Red Hat OpenShift-Instanzen](https://aka.ms/openshift/buy) über das Azure-Portal. Nach dem Kauf wird Ihr Abonnement innerhalb von 24 Stunden aktiviert. Anschließend können Sie Cluster bereitstellen.

Wenn Sie kein Azure-Kunde sind, [wenden Sie sich an das Vertriebsteam](https://aka.ms/openshift/contact-sales), und füllen Sie das Formular am Ende der Seite aus, um den Prozess zu starten.

Weitere Informationen finden Sie auf der [Azure Red Hat OpenShift-Preisseite](https://aka.ms/openshift/pricing).

## <a name="which-azure-regions-are-supported"></a>Welche Azure-Regionen werden unterstützt?

Eine Liste der weltweiten Regionen, in denen Azure Red Hat OpenShift unterstützt wird, finden Sie unter [Unterstützte Ressourcen](supported-resources.md#azure-regions).

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Kann ich einen Cluster in einem vorhandenen virtuellen Netzwerk bereitstellen?

Nein. Aber Sie können einen Azure Red Hat OpenShift-Cluster über Peering mit einem vorhandenen VNET verbinden. Weitere Informationen finden Sie unter [Verbinden des virtuellen Netzwerks eines Clusters mit einem bestehenden virtuellen Netzwerk ](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network).

## <a name="what-cluster-operations-are-available"></a>Welche Clustervorgänge sind verfügbar?

Sie können nur die Anzahl von Computeknoten zentral hoch- oder herunterskalieren. Keine weiteren Änderungen sind an der `Microsoft.ContainerService/openShiftManagedClusters`-Ressource nach der Erstellung zulässig. Die maximale Anzahl von Computeknoten ist auf 20 beschränkt.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Welche Größen von virtuellen Computern kann ich verwenden?

Eine Liste der VM-Größen, die Sie mit einem Azure Red Hat OpenShift-Cluster verwenden können, finden Sie unter [VM-Größen für Azure Red Hat OpenShift](supported-resources.md#virtual-machine-sizes).

## <a name="is-data-on-my-cluster-encrypted"></a>Werden Daten in meinem Cluster verschlüsselt?

Standardmäßig erfolgt die Verschlüsselung ruhender Daten. Die Azure Storage-Plattform verschlüsselt Ihre Daten automatisch vor dem dauerhaften Ablegen und entschlüsselt sie vor dem Abrufen. Details finden Sie unter [Azure-Speicherdienstverschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="can-i-use-prometheusgrafana-to-monitor-my-applications"></a>Kann ich Prometheus/Grafana zum Überwachen meiner Anwendungen verwenden?

Ja, Sie können Prometheus in Ihrem Namespace bereitstellen und Anwendungen in Ihrem Namespace überwachen.

## <a name="can-i-use-prometheusgrafana-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>Kann ich Prometheus/Grafana zum Überwachen von Metriken zur Clusterintegrität und -kapazität verwenden?

Nein, zurzeit nicht.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Ist die Docker-Registrierung extern verfügbar, damit ich Tools wie Jenkins verwenden kann?

Die Docker-Registrierung steht unter `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` zur Verfügung. Es wird aber keine Garantie für starke Dauerhaftigkeit des Speichers übernommen. Sie können auch die [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) verwenden.

## <a name="is-cross-namespace-networking-supported"></a>Wird namespaceübergreifender Netzwerkdatenverkehr unterstützt?

Kunden- und einzelne Projektadministratoren können namespaceübergreifenden Netzwerkdatenverkehr pro Projekt mithilfe von `NetworkPolicy`-Objekten anpassen (einschließlich seiner Untersagung).

## <a name="can-an-admin-manage-users-and-quotas"></a>Kann ein Administrator Benutzer und Kontingente verwalten?

Ja. Ein Azure Red Hat OpenShift-Administrator kann zusätzlich zum Zugriff auf alle von Benutzern erstellte Projekte Benutzer und Kontingente verwalten.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Kann ich einen Cluster auf nur bestimmte Azure AD-Benutzer beschränken?

Ja. Sie können einschränken, welche Azure AD-Benutzer sich bei einem Cluster anmelden können, indem Sie die Azure AD-Anwendung konfigurieren. Detailinformationen finden Sie unter [Gewusst wie: Einschränken Ihrer App auf eine Gruppe von Benutzern](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users).

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Kann ein Cluster Computeknoten über mehrere Azure-Regionen hinweg besitzen?

Nein. Alle Knoten in einem Azure Red Hat OpenShift-Cluster müssen aus derselben Azure-Region stammen.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Werden Master- und Infrastrukturknoten abstrahiert, weil sie zu Azure Kubernetes Service (AKS) gehören?

Nein. Alle Ressourcen, einschließlich des Clustermasters, werden in Ihrem Kundenabonnement ausgeführt. Diese Typen von Ressourcen werden in einer „Nur-Lesen“-Ressourcengruppe platziert.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>Wird Open Service Broker für Azure (OSBA) unterstützt?

Ja. Sie können OSBA mit Azure Red Hat OpenShift verwenden. Weitere Informationen finden Sie unter [Open Service Broker für Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template).

## <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Ich versuche, per Peering eine Verbindung mit einem virtuellen Netzwerk in einem anderen Abonnement herzustellen, erhalte jedoch den Fehler `Failed to get vnet CIDR`.

Stellen Sie in dem Abonnement mit dem virtuellen Netzwerk sicher, dass der `Microsoft.ContainerService`-Anbieter mit `az provider register -n Microsoft.ContainerService --wait` registriert wird. 

## <a name="what-is-the-azure-red-hat-openshift-aro-maintenance-process"></a>Was ist der Azure Red Hat OpenShift-Wartungsprozess?

Es gibt drei Arten von Wartung für Azure Red Hat OpenShift (ARO): Upgrades, Sicherung und Wiederherstellung von etcd-Daten sowie vom Cloudanbieter ausgelöste Wartung.

+ Upgrades umfassen Softwareupgrades und CVEs. CVE-Bereinigung erfolgt beim Startvorgang durch Ausführen von `yum update` und führt zu einer sofortigen Risikominderung.  Parallel dazu wird ein neuer Imagebuild für zukünftige Clustererstellungen erstellt.

+ Die Sicherung und Verwaltung von etcd-Daten ist ein automatisierter Prozess, bei dem, je nach Aktion, möglicherweise Clusterausfallzeiten erforderlich sind. Wird die etcd-Datenbank aus einer Sicherung wiederhergestellt, gibt es eine Ausfallzeit. etcd wird stündlich gesichert, und es werden die Sicherungen aus den letzten 6 Stunden beibehalten.

+ Eine von einem Cloudanbieter ausgelöste Wartung umfasst Netzwerk-, Speicher- und regionalen Ausfälle. Die Wartung ist abhängig vom Cloudanbieter und basiert auf vom Anbieter bereitgestellten Updates.

## <a name="what-is-the-general-upgrade-process"></a>Wie sieht der allgemeine Upgradeprozess aus?

Das Ausführen eines Upgrades sollte ein sicher auszuführender Prozess sein und sollte die Clusterdienste nicht stören. Der SRE (Site Reliability Engineer) kann den Upgradeprozess auslösen, wenn neue Versionen verfügbar oder CVEs ausstehend sind.

Verfügbare Updates werden in einer Stage-Umgebung getestet und dann auf Produktionscluster angewendet. Wenn Updates angewendet werden, wird temporär ein neuer Knoten hinzugefügt, und die Knoten werden einzeln nacheinander aktualisiert, sodass die Replikatanzahl für Pods erhalten bleibt. Ein Vorgehen gemäß den bewährten Methoden bietet Gewähr für minimale oder sogar keine Ausfallzeit.

Je nach Schweregrad des ausstehenden Upgrades oder Updates kann sich der Prozess darin unterscheiden, dass die Updates möglicherweise schnell angewendet werden, um die Gefährdung des Diensts durch ein CVE-Vorkommen zu verringern. Ein neues Image wird asynchron erstellt, getestet und als Clusterupgrade ausgerollt. Abgesehen hiervon gibt es keinen Unterschied zwischen einer Notfall- und einer geplanten Wartung. Geplante Wartung wird nicht zusammen mit dem Kunden vorgeplant.

Benachrichtigungen können über ICM und per E-Mail gesendet werden, wenn Kommunikation mit dem Kunden erforderlich ist.

## <a name="what-about-emergency-vs-planned-maintenance-windows"></a>Wird zwischen Notfall- und geplanten Wartungsfenstern unterschieden?

Wir unterscheiden nicht zwischen diesen beiden Wartungstypen. Unsere Teams sind 24/7/365 verfügbar und nutzen keine herkömmlichen geplanten Wartungsfenster „außerhalb der üblichen Betriebszeiten“.

## <a name="how-will-host-operating-system-and-openshift-software-be-updated"></a>Wie werden Hostbetriebssystem- und OpenShift-Software aktualisiert?

Die Hostbetriebssystem- und die OpenShift-Software werden durch unseren allgemeinen Upgrade-und Imageerstellungsprozess aktualisiert.

## <a name="whats-the-process-to-reboot-the-updated-node"></a>Wie sieht der Prozess für einen Neustart des aktualisierten Knotens aus?

Dieser Prozess sollte als Teil eines Upgrades ausgeführt werden.

## <a name="is-data-stored-in-etcd-encrypted-on-aro"></a>Werden Daten, die in etcd gespeichert sind, in ARO verschlüsselt?

Sie werden auf etcd-Ebene nicht verschlüsselt. Die Option zum Aktivieren der Verschlüsselung wird derzeit nicht unterstützt. OpenShift unterstützt diese Funktionalität, aber es sind technische Anstrengungen erforderlich, um sie geplant zu implementieren. Die Daten werden auf Datenträgerebene verschlüsselt. Weitere Informationen hierzu finden Sie unter [Encrypting Data at Datastore Layer](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html) (Verschlüsseln von Daten auf Datenspeicherebene).

## <a name="can-logs-of-underlying-vms-be-streamed-out-to-a-customer-log-analysis-system"></a>Können Protokolle von zugrunde liegenden virtuellen Computern an ein Protokollanalysesystem eines Kunden gestreamt werden?

Syslog, Docker-Protokolle, Journal und dmesg werden vom verwalteten Dienst verarbeitet und nicht für Kunden verfügbar gemacht.

## <a name="how-can-a-customer-get-access-to-metrics-like-cpumemory-at-the-node-level-to-take-action-to-scale-debug-issues-etc-i-cannot-seem-to-run-kubectl-top-on-an-aro-cluster"></a>Wie kann ein Kunde Zugriff auf Metriken wie CPU/Arbeitsspeicher auf Knotenebene erhalten, um Aktionen wie Skalieren, Debuggen von Problemen usw. auszuführen? Offensichtlich kann `kubectl top` nicht in einem ARO-Cluster ausgeführt werden.

`kubectl top` ist in Red Hat OpenShift nicht verfügbar. Für diesen Befehl ist eine unterstützende Metrikenquelle erforderlich, entweder Heapster (veraltet) oder „metrics-server“ („incubator“ oder „alpha“), von denen keine im OpenShift-Überwachungsstapel enthalten ist.

## <a name="what-is-the-default-pod-scheduler-configuration-for-aro"></a>Wie sieht die Standardkonfiguration für den Pod-Planer für ARO aus?

ARO verwendet den Standardplaner, der mit OpenShift bereitgestellt wird. Es gibt einige zusätzliche Mechanismen, die in ARO nicht unterstützt werden. Ausführlichere Informationen hierzu finden Sie in der [Default Scheduling-Dokumentation](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/scheduler.html#generic-scheduler) und in der [master scheduler-Dokumentation](https://github.com/openshift/openshift-azure/blob/master/pkg/startup/v6/data/master/etc/origin/master/scheduler.json).

Erweiterte/benutzerdefinierte Zeitplanung wird derzeit nicht unterstützt. Weitere Informationen hierzu finden Sie in der [Scheduling-Dokumentation](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/index.html).

## <a name="if-we-scale-up-the-deployment-how-do-azure-fault-domains-map-into-pod-placement-to-ensure-all-pods-for-a-service-do-not-get-knocked-out-by-a-failure-in-a-single-fault-domain"></a>Wie werden, wenn die Bereitstellung zentral hochskaliert wird, Azure-Fehlerdomänen in der Pod-Platzierung zugeordnet, damit sichergestellt ist, dass nicht alle Pods für einen Dienst durch einen Fehler in einer einzelnen Fehlerdomäne lahmgelegt werden?

Es gibt standardmäßig fünf Fehlerdomänen, wenn VM-Skalierungsgruppen in Azure verwendet werden. Jede Instanz eines virtuellen Computers in einer Skalierungsgruppe wird in einer dieser Fehlerdomänen platziert. Dadurch ist sichergestellt, dass Anwendungen, die auf den Computeknoten in einem Cluster bereitgestellt werden, in separaten Fehlerdomänen platziert werden.

Ausführlichere Informationen hierzu finden Sie unter [Auswählen der richtigen Anzahl von Fehlerdomänen für VM-Skalierungsgruppen](https://docs.microsoft.com//azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains).

## <a name="is-there-a-way-to-manage-pod-placement"></a>Gibt es eine Möglichkeit, die Pod-Platzierung zu verwalten?

Ab dem bevorstehenden customer-admin-Update werden Kunden die Möglichkeit haben, Knoten zu erhalten und Bezeichnungen anzuzeigen.  Dies bietet eine Möglichkeit, einen beliebigen virtuellen Computer in der Skalierungsgruppe als Ziel festzulegen.

Die Verwendung bestimmter Bezeichnungen muss sehr vorsichtig erfolgen:

- Der Hostname darf nicht verwendet werden. Der Hostname wird häufig mit Upgrades und Updates ausgetauscht und wird garantiert geändert.

- Wenn der Kunde bestimmte Bezeichnungen oder eine Bereitstellungsstrategie benötigt, ließe sich dies erreichen, würde aber technische Anstrengungen voraussetzen und wird derzeit nicht unterstützt.

## <a name="what-is-the-maximum-number-of-pods-in-an-aro-cluster-what-is-the-maximum-number-of-pods-per-node-in-aro"></a>Wie groß ist die maximale Anzahl von Pods in einem ARO-Cluster?  Wie groß ist die maximale Anzahl von Pods pro Knoten in ARO?

Ausführlichere Informationen hierzu finden Sie unter [OpenShift Container Platform Cluster Limits](https://docs.openshift.com/container-platform/3.11/scaling_performance/cluster_limits.html#scaling-performance-current-cluster-limits). Red Hat OpenShift 3.11 hat ein Limit von 250 Pods/Knoten, während [ARO ein Limit von 20 Computeknoten hat](https://docs.microsoft.com/azure/openshift/openshift-faq#what-cluster-operations-are-available). Somit ist die maximale Anzahl von Pods, die in einem ARO-Cluster unterstützt werden, auf 250 * 20 = 5000 begrenzt.

## <a name="can-we-specify-ip-ranges-for-deployment-on-the-private-vnet-avoiding-clashes-with-other-corporate-vnets-once-peered"></a>Können IP-Adressbereiche für die Bereitstellung im privaten VNET angegeben werden, um Konflikte mit anderen Unternehmens-VNETs zu vermeiden, nachdem Peering erfolgt ist?

Azure Red Hat OpenShift unterstützt VNET-Peering und ermöglicht es dem Kunden, ein VNET, mit dem Peering erfolgt, und einen VNET-CIDR bereitzustellen, in dem das OpenShift-Netzwerk funktioniert.

Das von ARO erstellte VNET wird geschützt und akzeptiert keine Konfigurationsänderungen. Das VNET, für das Peering erfolgt ist, wird vom Kunden gesteuert und befindet sich in dessen Abonnement.

## <a name="does-the-cluster-reside-in-a-customer-subscription"></a>Befindet sich der Cluster in einem Kundenabonnement? 

Die verwaltete Azure-Anwendung befindet sich in einer gesperrten Ressourcengruppe im Kundenabonnement. Der Kunde kann Objekte in dieser Ressourcengruppe anzeigen, aber nicht ändern.

## <a name="is-the-sdn-module-configurable"></a>Ist das SDN-Modul konfigurierbar?

SDN ist „openshift-ovs-networkpolicy“ und kann nicht konfiguriert werden.

## <a name="which-unix-rights-in-iaas-are-available-for-mastersinfraapp-nodes"></a>Welche UNIX-Rechte (in IaaS) sind für Master/Infra/App-Knoten verfügbar?

Für dieses Angebot nicht zutreffend. Knotenzugriff ist unzulässig.

## <a name="which-ocp-rights-do-we-have-cluster-admin-project-admin"></a>Welche OCP-Rechte haben wir? Clusteradministrator? Projektadministrator?

Weitere Informationen finden Sie in der Azure Red Hat OpenShift-[Übersicht für Clusteradministration](https://docs.openshift.com/aro/admin_guide/index.html).

## <a name="which-kind-of-federation-with-ldap"></a>Welche Art von Verbund liegt mit LDAP vor?

Dies wird über Azure AD-Integration erreicht. 

## <a name="is-there-any-element-in-aro-shared-with-other-customers-or-is-everything-independent"></a>Gibt es irgendein Element in ARO, das für andere Kunden freigegeben ist? Oder ist alles unabhängig?

Jeder Azure Red Hat OpenShift-Cluster ist einem bestimmten Kunden zugeordnet und befindet sich im Abonnement des Kunden. 

## <a name="can-we-choose-any-persistent-storage-solution-like-ocs"></a>Können wir eine permanente Speicherlösung auswählen, z. B. OCS? 

Es sind zwei Speicherklassen zur Auswahl verfügbar: Azure-Datenträger (Azure Disk) und Azure-Datei (Azure File).

## <a name="how-is-a-cluster-updated-including-majors-and-minors-due-to-vulnerabilities"></a>Wie wird ein Cluster aktualisiert (einschließlich Hauptversionen und Nebenversionen aufgrund von Sicherheitsrisiken)?

Informationen hierzu finden Sie unter [Wie sieht der allgemeine Upgradeprozess aus?](https://docs.microsoft.com/azure/openshift/openshift-faq#what-is-the-general-upgrade-process)

## <a name="what-azure-load-balancer-is-used-by-aro-is-it-standard-or-basic-and-is-it-configurable"></a>Welche Azure Load Balancer-Variante wird von ARO verwendet?  Ist es die Standard- oder Basic-Variante, und ist diese konfigurierbar?

ARO verwendet Standard-Variante von Azure Load Balancer, und diese ist nicht konfigurierbar.

## <a name="can-aro-use-netapp-based-storage"></a>Kann ARO NetApp-basierten Speicher verwenden?

Zurzeit werden nur die Speicherklassen „Azure-Datenträger“ und „Azure-Datei“ unterstützt. 


