---
title: Was ist der Azure Private Link-Dienst?
description: Erfahren Sie mehr über den Azure Private Link-Dienst.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: cad8e3e4f32a8773fe914362b637d39765a23c21
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672517"
---
# <a name="what-is-azure-private-link-service"></a>Was ist der Azure Private Link-Dienst?

Der Azure Private Link-Dienst ist der Verweis auf Ihren eigenen Dienst, der von Azure Private Link unterstützt wird. Ihr Dienst, der hinter [Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md) ausgeführt wird, kann für den Zugriff auf Private Link aktiviert werden, sodass die Nutzer Ihres Dienstes privat über ihre eigenen virtuellen Netzwerke darauf zugreifen können. Ihre Kunden können einen privaten Endpunkt in ihrem VNet erstellen und diesem Dienst zuordnen. Dieser Artikel erläutert Konzepte, die sich auf die Seite des Dienstanbieters beziehen. 

## <a name="workflow"></a>Workflow

![Private Link-Dienstworkflow](media/private-link-service-overview/private-link-service-workflow.png)

### <a name="create-your-private-link-service"></a>Erstellen eines Private Link-Diensts

- Konfigurieren Sie Ihre Anwendung so, dass sie hinter einem Load Balancer Standard in Ihrem virtuellen Netzwerk ausgeführt wird. Wenn Sie Ihre Anwendung bereits hinter einem Load Balancer Standard konfiguriert haben, können Sie diesen Schritt überspringen.   
- Erstellen Sie einen Private Link-Dienst, der auf den obigen Lastausgleich verweist. Wählen Sie im Auswahlprozess des Lastausgleichs die Front-End-IP-Konfiguration aus, für die Sie den Datenverkehr empfangen möchten. Wählen Sie ein Subnetz für NAT-IP-Adressen für den Private Link-Dienst aus. Es wird empfohlen, dass im Subnetz mindestens acht NAT-IP-Adressen zur Verfügung stehen. Der gesamte Consumerdatenverkehr scheint von diesem Pool privater IP-Adressen an den Dienstanbieter zu stammen. Wählen Sie die entsprechenden Eigenschaften/Einstellungen für den Private Link-Dienst aus.    

    > [!NOTE]
    > Der Azure Private Link-Dienst wird nur von Load Balancer Standard unterstützt. 
    
### <a name="share-your-service"></a>Freigeben Ihres Diensts

Nachdem Sie einen Private Link-Dienst erstellt haben, generiert Azure einen weltweit eindeutigen Moniker namens „Alias“, der auf dem Namen basiert, den Sie für Ihren Dienst angeben. Sie können entweder den Alias oder den Ressourcen-URI Ihres Diensts offline für Ihre Kunden freigeben. Consumer können eine Private Link-Verbindung mit dem Alias oder Ressourcen-URI starten.
 
### <a name="manage-your-connection-requests"></a>Verwalten Ihrer Verbindungsanforderungen

Nachdem ein Consumer eine Verbindung hergestellt hat, kann der Dienstanbieter die Verbindungsanforderung annehmen oder ablehnen. Alle Verbindungsanforderungen werden unter der Eigenschaft **privateendpointconnections** des Private Link-Diensts aufgeführt.
 
### <a name="delete-your-service"></a>Löschen Ihres Diensts

Wenn der Private Link-Dienst nicht mehr verwendet wird, können Sie ihn löschen. Bevor Sie den Dienst jedoch löschen, stellen Sie sicher, dass ihm keine privaten Endpunktverbindungen zugeordnet sind. Sie können alle Verbindungen ablehnen und den Dienst löschen.

## <a name="properties"></a>Properties

Ein Private Link-Dienst legt die folgenden Eigenschaften fest: 

|Eigenschaft |Erklärung  |
|---------|---------|
|Bereitstellungsstatus (provisioningState)  |Eine schreibgeschützte Eigenschaft, die den aktuellen Bereitstellungsstatus für den Private Link-Dienst auflistet. Geeignete Bereitstellungsstatus sind: „Wird gelöscht; Fehler; Erfolgreich; Wird aktualisiert“. Wenn der Bereitstellungsstatus „Erfolgreich“ lautet, haben Sie Ihren Private Link-Dienst erfolgreich bereitgestellt.        |
|Alias (alias)     | Alias ist eine weltweit eindeutige schreibgeschützte Zeichenfolge für Ihren Dienst. Sie hilft Ihnen, die Kundendaten für Ihren Dienst zu maskieren und erstellt gleichzeitig einen einfach zu teilenden Namen für Ihren Dienst. Wenn Sie einen Private Link-Dienst erstellen, generiert Azure den Alias für Ihren Dienst, den Sie mit Ihren Kunden teilen können. Ihre Kunden können unter diesem Alias eine Verbindung zu Ihrem Dienst anfordern.          |
|Sichtbarkeit (visibility)     | Sichtbarkeit ist die Eigenschaft, die die Einstellungen für die Offenlegung für Ihren Private Link-Dienst steuert. Dienstanbieter können wählen, ob sie das Risiko für ihren Dienst durch Abonnements mit rollenbasierten Zugriffssteuerungsberechtigungen (RBAC), einem eingeschränkten Satz von Abonnements oder allen Azure-Abonnements begrenzen möchten.          |
|Automatische Genehmigung (autoApproval)    |   Die automatische Genehmigung steuert den automatisierten Zugriff auf den Private Link-Dienst. Die in der automatischen Genehmigungsliste angegebenen Abonnements werden automatisch genehmigt, wenn eine Verbindung von privaten Endpunkten in diesen Abonnements angefordert wird.          |
|Front-End-IP-Konfiguration des Lastenausgleichs (loadBalancerFrontendIpConfigurations)    |    Der Private Link-Dienst ist an die Front-End-IP-Adresse eines Load Balancer Standard gebunden. Der gesamte für den Dienst bestimmte Datenverkehr erreicht das Front-End des Load Balancer Standard. Sie können Load Balancer Standard-Regeln konfigurieren, um diesen Datenverkehr an geeignete Back-End-Pools weiterzuleiten, in denen Ihre Anwendungen ausgeführt werden. Front-End-IP-Konfigurationen für den Lastenausgleich unterscheiden sich von NAT-IP-Konfigurationen.      |
|NAT-IP-Konfiguration (ipConfigurations)    |    Diese Eigenschaft bezieht sich auf die NAT-IP-Konfiguration (Network Address Translation) für den Private Link-Dienst. Die NAT-IP-Adresse kann aus einem beliebigen Subnetz im virtuellen Netzwerk eines Dienstanbieters ausgewählt werden. Der Private Link-Dienst führt die zielseitige NAT-Erstellung für den Private Link-Datenverkehr durch. Dadurch wird sichergestellt, dass es keinen IP-Konflikt zwischen Quell- (Consumerseite) und Zieladressraum (Dienstanbieter) gibt. Auf der Zielseite (Dienstanbieterseite) wird die NAT-IP-Adresse als Quell-IP-Adresse für alle von Ihrem Dienst empfangenen Pakete und die Ziel-IP-Adresse für alle von Ihrem Dienst gesendeten Pakete angezeigt.       |
|Private Endpunktverbindungen (privateEndpointConnections)     |  Diese Eigenschaft listet die privaten Endpunkte auf, die sich mit dem Private Link-Dienst verbinden. Mehrere private Endpunkte können mit demselben Private Link-Dienst verbunden werden, und der Dienstanbieter kann den Status für einzelne private Endpunkte steuern.        |
|||


### <a name="details"></a>Details

- Auf den Private Link-Dienst kann von genehmigten privaten Endpunkten in der gleichen Region aus zugegriffen werden. Der private Endpunkt kann über dasselbe virtuelle Netzwerk, regionale VNets mit Peering, globale VNets mit Peering und lokal über private VPN- oder ExpressRoute-Verbindungen erreicht werden. 
 
- Beim Erstellen eines Private Link-Diensts wird für die Lebensdauer der Ressource eine Netzwerkschnittstelle erstellt. Diese Schnittstelle kann vom Kunden nicht verwaltet werden.
 
- Der Private Link-Dienst muss in derselben Region wie das virtuelle Netzwerk und der Load Balancer Standard bereitgestellt werden.  
 
- Auf einen einzelnen Private Link-Dienst kann von mehreren privaten Endpunkten zugegriffen werden, die zu verschiedenen VNets, Abonnements und/oder Active Directory-Mandanten gehören. Die Verbindung wird über einen Verbindungsworkflow hergestellt. 
 
- Mehrere Private Link-Dienste können auf demselben Load Balancer Standard mit unterschiedlichen Front-End-IP-Konfigurationen erstellt werden. Die Anzahl der Private Link-Dienste, die Sie pro Load Balancer Standard und pro Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel zu  [Azure-Grenzwerten](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).
 
- Der Private Link-Dienst kann mit mehr als einer NAT-IP-Konfiguration verknüpft sein. Die Auswahl von mehreren NAT-IP-Konfigurationen kann Dienstanbietern bei der Skalierung helfen. Heute können Dienstanbieter bis zu acht NAT-IP-Adressen pro Private Link-Dienst zuweisen. Mit jeder NAT-IP-Adresse können Sie mehr Ports für Ihre TCP-Verbindungen zuweisen und auf diese Weise horizontal hochskalieren. Nachdem Sie mehrere NAT-IP-Adressen zu einem Private Link-Dienst hinzugefügt haben, können Sie die NAT-IP-Adressen nicht mehr löschen. Dadurch wird sichergestellt, dass beim Löschen der NAT-IP-Adressen die aktiven Verbindungen nicht beeinträchtigt werden.


## <a name="alias"></a>Alias

**Alias** ist ein weltweit eindeutiger Name für Ihren Dienst. Sie hilft Ihnen, die Kundendaten für Ihren Dienst zu maskieren und erstellt gleichzeitig einen einfach zu teilenden Namen für Ihren Dienst. Wenn Sie einen Private Link-Dienst erstellen, generiert Azure einen Alias für Ihren Dienst, den Sie mit Ihren Kunden teilen können. Ihre Kunden können unter diesem Alias eine Verbindung zu Ihrem Dienst anfordern.

Der Alias besteht aus drei Teilen: *Präfix*.*GUID*.*Suffix*

- Das Präfix ist der Dienstname. Sie können ein eigenes Präfix auswählen. Nachdem der „Alias“ erstellt wurde, können Sie ihn nicht mehr ändern, also wählen Sie Ihr Präfix entsprechend aus.  
- Die GUID wird von der Plattform bereitgestellt. Dies trägt dazu bei, den Namen weltweit eindeutig zu gestalten. 
- Das Suffix wird von Azure angefügt: *region*.azure.privatelinkservice 

Vollständiger Alias:  *Präfix*. {GUID}.*region*.azure.privatelinkservice  

## <a name="control-service-exposure"></a>Steuern der Dienstoffenlegung

Der Private Link-Dienst bietet Ihnen Optionen, um die Sichtbarkeit Ihres Diensts über die Einstellung „Sichtbarkeit“ zu steuern. Sie können den Dienst für den Gebrauch durch verschiedene Ihrer VNets als „privat“ definieren (nur RBAC-Berechtigungen), die Offenlegung auf eine begrenzte Anzahl von vertrauenswürdigen Abonnements beschränken oder ihn als „öffentlich“ festlegen, sodass alle Azure-Abonnements Verbindungen über den Private Link-Dienst anfordern können. Ihre Sichtbarkeitseinstellungen bestimmen, ob ein Consumer eine Verbindung zu Ihrem Dienst herstellen kann. 

## <a name="control-service-access"></a>Steuern des Dienstzugriffs

Consumer, für die Ihr Private Link-Dienst offengelegt ist (über die Sichtbarkeitseinstellung gesteuert), können in ihren VNets einen privaten Endpunkt erstellen und eine Verbindung zu Ihrem Private Link-Dienst anfordern. Die private Endpunktverbindung wird im Zustand „Ausstehend“ für das Private Link-Dienstobjekt erstellt. Der Dienstanbieter ist für die Bearbeitung der Verbindungsanforderung zuständig. Sie können die Verbindung entweder genehmigen, ablehnen oder löschen. Nur genehmigte Verbindungen können Datenverkehr an den Private Link-Dienst senden.

Die Genehmigung der Verbindungen kann automatisiert werden, indem Sie die Eigenschaft „auto-approval“ (automatische Genehmigung) im Private Link-Dienst verwenden. Die automatische Genehmigung ist eine Möglichkeit für Dienstanbieter, eine Reihe von Abonnements für den automatisierten Zugriff auf ihren Dienst vorab zu genehmigen. Kunden müssen ihre Abonnements offline freigeben, damit Dienstanbieter sie in die Liste der automatischen Genehmigungen aufnehmen können. Die automatische Genehmigung ist eine Teilmenge des Sichtbarkeitsarrays. Die Sichtbarkeit steuert die Einstellungen für die Offenlegung, während die automatische Genehmigung die Genehmigungseinstellungen für Ihren Dienst steuert. Wenn ein Kunde eine Verbindung von einem Abonnement in der automatischen Genehmigungsliste anfordert, wird die Verbindung automatisch genehmigt und hergestellt. Dienstanbieter müssen die Anforderung nicht mehr manuell genehmigen. Wenn ein Kunde hingegen eine Verbindung von einem Abonnement im Sichtbarkeitsarray und nicht im automatischen Genehmigungsarray anfordert, erreicht die Anforderung den Dienstanbieter, aber der Dienstanbieter muss die Verbindungen manuell genehmigen.

## <a name="limitations"></a>Einschränkungen

Nachfolgend sind die bekannten Einschränkungen bei der Nutzung des Private Link-Diensts aufgeführt:
- Wird nur von Load Balancer Standard unterstützt 
- Unterstützt nur IPv4-Datenverkehr
- Unterstützt nur TCP-Datenverkehr
- Nur über private Endpunkte in derselben Region erreichbar
- Erstellen und Verwalten von Erfahrungen aus dem Azure-Portal wird nicht unterstützt
- Verbindungsinformationen von Clients, die das Proxyprotokoll verwenden, sind für den Dienstanbieter nicht verfügbar

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen eines Private Link-Diensts mit Azure PowerShell](create-private-link-service-powershell.md)
- [Erstellen eines Private Link-Diensts mithilfe der Azure CLI](create-private-link-service-cli.md)
