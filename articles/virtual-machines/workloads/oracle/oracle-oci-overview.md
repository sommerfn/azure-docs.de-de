---
title: Integrieren von Microsoft Azure mit Oracle Cloud Infrastructure | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu Lösungen, die in Microsoft Azure ausgeführte Oracle-Apps mit Datenbanken in Oracle Cloud Infrastructure (OCI) integrieren.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: 0f35f22d6bd32d75d5c3b07d8a20b4bb7714e247
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71799562"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Oracle-Anwendungslösungen mit Integration von Microsoft Azure und Oracle Cloud Infrastructure (Vorschauversion)

Microsoft und Oracle haben sich zusammengetan, um cloudübergreifende Konnektivität mit geringer Wartezeit und hohem Durchsatz bereitzustellen, sodass Sie von den Vorteilen beider Clouds profitieren können. 

Dank dieser cloudübergreifenden Konnektivität können Sie eine mehrschichtige Anwendung partitionieren, um Ihre Datenbankschicht in Oracle Cloud Infrastructure (OCI) und die Anwendung sowie andere Schichten in Microsoft Azure auszuführen. Dies ist vergleichbar mit der Ausführung des gesamten Lösungsstapels in einer einzelnen Cloud. 

> [!IMPORTANT]
> Das cloudübergreifende Feature befindet sich derzeit in der Vorschauphase und unterliegt [Einschränkungen](#preview-limitations). Damit Verbindungen mit geringer Latenz zwischen Azure und OCI eingerichtet werden können, muss diese Funktionalität für Ihr Azure-Abonnement zunächst aktiviert werden. Sie müssen sich für die Vorschauversion registrieren, indem Sie dieses kurze [Umfrageformular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyzVVsi364tClw522rL9tkpUMVFGVVFWRlhMNUlRQTVWSTEzT0dXMlRUTyQlQCN0PWcu) ausfüllen. Sie erhalten eine E-Mail zurück, nachdem Ihr Abonnement registriert wurde. Sie können die Funktion erst verwenden, wenn Sie eine Bestätigungs-E-Mail erhalten haben. Sie können sich auch an Ihren Ansprechpartner bei Microsoft wenden, um für diese Vorschauversion aktiviert zu werden. Der Zugriff auf die Vorschaufunktion ist vorbehaltlich der Verfügbarkeit und wird von Microsoft nach eigenem Ermessen beschränkt. Die Beantwortung der Umfrage garantiert keinen Zugriff. Diese Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

Wenn Sie Oracle-Lösungen vollständig in der Azure-Infrastruktur bereitstellen möchten, lesen Sie [Oracle VM images and their deployment on Microsoft Azure](oracle-vm-solutions.md) (Oracle-VM-Images und deren Bereitstellung in Microsoft Azure).

## <a name="scenario-overview"></a>Übersicht über das Szenario

Mit cloudübergreifender Konnektivität können Sie die branchenführenden Anwendungen von Oracle sowie Ihre eigenen benutzerdefinierten Anwendungen auf virtuellen Azure-Computern ausführen und gleichzeitig von den Vorteilen gehosteter Datenbankdienste in OCI profitieren. 

Zu den Anwendungen, die Sie in einer cloudübergreifenden Konfiguration ausführen können, zählen unter anderem:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Oracle Retail-Anwendungen
* Oracle Hyperion Financial Management

Das folgende Diagramm bietet einen allgemeinen Überblick über die verbundene Lösung. Der Einfachheit halber enthält das Diagramm nur eine Logik- und eine Datenschicht. Ihre Lösung kann je nach Anwendungsarchitektur auch weitere Schichten enthalten – etwa eine Webschicht in Azure. Weitere Informationen finden Sie in den folgenden Abschnitten.

![Übersicht über die Azure-OCI-Lösung](media/oracle-oci-overview/crosscloud.png)

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Die Vorschauversion der cloudübergreifenden Konnektivität ist auf die Azure-Regionen „USA, Osten“ (eastus), „Vereinigtes Königreich, Süden“ (uksouth) und die OCI-Regionen „Ashburn“ (USA, Osten) und London (Vereinigtes Königreich, Süden) beschränkt. Verwenden Sie für die Region „Vereinigtes Königreich, Süden“ die Verfügbarkeitsdomäne 1 (AD 1) in OCI, wenn Sie die Konnektivität für geringere Latenzen bereitstellen.

## <a name="networking"></a>Netzwerk

Unternehmenskunden entscheiden sich aus verschiedenen geschäftlichen und betrieblichen Gründen oftmals für eine Diversifizierung und stellen Workloads über mehrere Clouds bereit. Zur Diversifizierung verbinden Kunden Cloudnetzwerke über das Internet, per IPSec-VPN oder mithilfe der Direktverbindungslösung des Cloudanbieters über Ihr lokales Netzwerk. Die Verbindungsherstellung zwischen Cloudnetzwerken kann einen erheblichen Zeit- und Kostenaufwand bedeuten und hohe Investitionen für Entwurf, Beschaffung, Installation, Tests und Betrieb erfordern. 

Aus diesem Grund haben Oracle und Microsoft eine integrierte Multi-Cloud-Umgebung entwickelt. Zur Schaffung eines cloudübergreifenden Netzwerks wird eine Verbindung zwischen [ExpressRoute](../../../expressroute/expressroute-introduction.md) in Microsoft Azure und [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) in OCI hergestellt. Diese Konnektivität ist möglich, wenn sich ein Azure ExpressRoute-Peeringstandort in der Nähe oder am gleichen Peeringstandort befindet wie OCI FastConnect. Dies ermöglicht eine sichere und schnelle Verbindung zwischen den beiden Clouds ohne zusätzlichen Dienstanbieter.

Mit ExpressRoute und FastConnect können Kunden mittels Peering ein virtuelles Netzwerk in Azure mit einem virtuellen Cloudnetzwerk in OCI kombinieren – vorausgesetzt, der private IP-Adressbereich überschneidet sich nicht. Dank des Peerings der beiden Netzwerke kann eine Ressource im virtuellen Netzwerk mit einer Ressource im virtuellen Cloudnetzwerk von OCI kommunizieren, als befänden sich beide im gleichen Netzwerk.

## <a name="network-security"></a>Netzwerksicherheit

Netzwerksicherheit ist ein wichtiger Bestandteil jeder Unternehmensanwendung und ein zentraler Aspekt dieser Multi-Cloud-Lösung. Sämtlicher Datenverkehr über ExpressRoute und FastConnect wird über ein privates Netzwerk abgewickelt. Diese Konfiguration ermöglicht die sichere Kommunikation zwischen einem virtuellen Azure-Netzwerk und einem virtuellen Cloudnetzwerk von Oracle. Sie müssen keine öffentliche IP-Adresse für einen virtuellen Computer in Azure angeben. Und Sie benötigen auch kein Internetgateway in OCI. Die gesamte Kommunikation erfolgt über die private IP-Adresse der Computer.

Darüber hinaus können Sie [Sicherheitslisten](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) für Ihr virtuelles Cloudnetzwerk in OCI sowie Sicherheitsregeln einrichten, die mit [Netzwerksicherheitsgruppen](../../../virtual-network/security-overview.md) in Azure verknüpft sind. Diese Regeln dienen zum Steuern des Datenverkehrsfluss zwischen Computern in den virtuellen Netzwerken. Netzwerksicherheitsregeln können auf Computerebene, auf Subnetzebene sowie auf der Ebene des virtuellen Netzwerks hinzugefügt werden.
 
## <a name="identity"></a>Identity

Identität ist einer der Grundpfeiler der Partnerschaft zwischen Microsoft und Oracle. Für die Integration von [Oracle Identity Cloud Service](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) (IDCS) und [Azure Active Directory](../../../active-directory/index.yml) (Azure AD) wurde ein erheblicher Aufwand betrieben. Azure AD ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft. Er unterstützt Ihre Benutzer bei der Anmeldung sowie beim Zugriff auf verschiedene Ressourcen. Darüber hinaus können Sie mithilfe von Azure AD Ihre Benutzer und deren Berechtigungen verwalten.

Diese Integration ermöglicht aktuell die Verwaltung an einem zentralen Ort (sprich: in Azure Active Directory). Azure AD synchronisiert sämtliche Änderungen im Verzeichnis mit dem entsprechenden Oracle-Verzeichnis und wird für einmaliges Anmelden bei cloudübergreifenden Oracle-Lösungen verwendet.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit einem [cloudübergreifenden Netzwerk](configure-azure-oci-networking.md) zwischen Azure und OCI. 

Weitere Informationen und Whitepaper zu OCI finden Sie in der [Dokumentation zu Oracle Cloud Infrastructure](https://docs.cloud.oracle.com/iaas/Content/home.htm).
