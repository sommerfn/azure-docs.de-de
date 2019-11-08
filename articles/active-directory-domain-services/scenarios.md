---
title: Gängige Bereitstellungsszenarien für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie mehr über einige der gängigen Szenarien und Anwendungsfälle für Azure Active Directory Domain Services, um für einen Mehrwert zu sorgen und Geschäftsanforderungen zu erfüllen.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 489f4a527a5afaf1bab5e2065137a5011d45baa6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474441"
---
# <a name="common-use-cases-and-scenarios-for-azure-active-directory-domain-services"></a>Gängige Anwendungsfälle und Szenarien für Azure Active Directory Domain Services

Azure Active Directory Domain Services (Azure AD DS) stellt verwaltete Domänendienste wie Domänenbeitritt, Gruppenrichtlinien, Lightweight Directory Access Protocol (LDAP) und Kerberos/NTLM-Authentifizierung bereit. Azure AD DS lässt sich in Ihren vorhandenen Azure AD-Mandanten integrieren, sodass Benutzer sich mit ihren vorhandenen Anmeldeinformationen anmelden können. Sie verwenden diese Domänendienste, ohne Domänencontroller in der Cloud bereitstellen, verwalten und patchen zu müssen, was eine reibungslosere Migration lokaler Ressourcen per Lift & Shift zu Azure ermöglicht.

In diesem Artikel werden einige gängige Geschäftsszenarien beschrieben, in denen Azure AD DS einen Mehrwert bietet und die entsprechenden Anforderungen erfüllt.

## <a name="secure-administration-of-azure-virtual-machines"></a>Sichere Verwaltung von virtuellen Azure-Computern

Damit Sie die Möglichkeit haben, einen einzelnen Satz AD-Anmeldeinformationen zu verwenden, können virtuelle Azure-Computer (Virtual Machines, VMs) einer verwalteten Azure AD DS-Domäne hinzugefügt werden. Dieser Ansatz minimiert Probleme bei der Verwaltung von Anmeldeinformationen wie die Verwaltung lokaler Administratorkonten auf den einzelnen VMs oder separater Konten und Kennwörter zwischen Umgebungen.

Einer verwalteten Azure AD DS-Domäne hinzugefügte VMs können auch mithilfe von Gruppenrichtlinien verwaltet und geschützt werden. Erforderliche Sicherheitsbaselines können auf VMs angewendet werden, um die VMs gemäß den Sicherheitsrichtlinien des Unternehmens zu sperren. Beispiel: Sie können mithilfe von Funktionen für die Gruppenrichtlinienverwaltung die Anwendungstypen einschränken, die auf dem virtuellen Computer gestartet werden können.

![Optimierte Verwaltung von virtuellen Azure-Computern](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Im Folgenden wird ein gängiges Beispielszenario beschrieben. Wenn Server und andere Infrastrukturkomponenten das Ende ihrer Lebensdauer erreichen, möchte Contoso die aktuell lokal gehosteten Anwendungen in die Cloud verschieben. Der aktuelle IT-Standard des Unternehmens gibt vor, dass Server, auf denen Unternehmensanwendungen gehostet werden, einer Domäne angehören und mithilfe von Gruppenrichtlinien verwaltet werden müssen. Der IT-Administrator von Contoso zieht es vor, für in Azure bereitgestellte VMs den Beitritt zu einer Domäne durchzuführen, um die Verwaltung zu vereinfachen, da sich Benutzer mit ihren Unternehmensanmeldeinformationen anmelden können. Wenn die VMs in die Domäne eingebunden sind, können sie mithilfe von Gruppenrichtlinienobjekten (Group Policy Objects, GPOs) auch so konfiguriert werden, dass sie die erforderlichen Sicherheitsbaselines erfüllen. Contoso würde es vorziehen, die eigenen Domänencontroller in Azure nicht bereitstellen, überwachen und verwalten zu müssen.

Für diesen Anwendungsfall ist Azure AD DS hervorragend geeignet. Mit einer verwalteten Azure AD DS-Domäne können Sie VMs in eine Domäne einbinden, einen einzelnen Satz Anmeldeinformationen verwenden und Gruppenrichtlinien anwenden. Bei einer verwalteten Domäne müssen Sie die Domänencontroller nicht selbst konfigurieren und verwalten.

### <a name="deployment-notes"></a>Hinweise zur Bereitstellung

Beachten Sie die folgenden Bereitstellungsaspekte für diesen Beispielanwendungsfall:

* Verwaltete Azure AD DS-Domänen verwenden standardmäßig eine einzige flache Organisationseinheitsstruktur (Organizational Unit, OU). Alle in die Domäne eingebundenen VMs befinden sich in einer einzigen Organisationseinheit. Sie können aber auch benutzerdefinierte Organisationseinheiten erstellen.
* Azure AD DS verwendet jeweils ein integriertes Gruppenrichtlinienobjekt (Group Policy Object, GPO) für Benutzer- und Computercontainer. Um weitere Steuerungsmöglichkeiten zu haben, können Sie benutzerdefinierte Gruppenrichtlinienobjekte erstellen und diese benutzerdefinierten Organisationseinheiten zuweisen.
* Azure AD DS unterstützt das grundlegende AD-Computerobjektschema. Das Schema des Computerobjekts kann nicht erweitert werden.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-bind-authentication"></a>Migrieren lokaler Anwendungen per Lift & Shift, die eine LDAP-Bindungsauthentifizierung verwenden

In einem Beispielszenario verfügt Contoso über eine lokale Anwendung, die vor vielen Jahren von einem unabhängigen Softwarehersteller (Independent Software Vendor, ISV) gekauft wurde. Die Anwendung wurde vom ISV in den Wartungsmodus versetzt, und das Anfordern von Änderungen an der Anwendung ist äußerst kostspielig. Diese Anwendung verfügt über ein webbasiertes Front-End, mit dem Benutzeranmeldeinformationen mithilfe eines Webformulars gesammelt werden, und anschließend werden Benutzer authentifiziert, indem eine LDAP-Bindung an die lokale AD DS-Umgebung ausgeführt wird.

![LDAP-Bindung](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso möchte diese Anwendung zu Azure migrieren. Die Anwendung sollte unverändert weiterhin funktionieren, ohne dass Änderungen erforderlich sind. Außerdem sollen sich Benutzer mit ihren vorhandenen Unternehmensanmeldeinformationen und ohne zusätzlichen Schulungsaufwand authentifizieren können. Es sollte für Endbenutzer transparent sein, wo die Anwendung ausgeführt wird.

In diesem Szenario ermöglicht Azure AD DS Anwendungen das Ausführen von LDAP-Bindungen als Teil des Authentifizierungsvorgangs. Ältere lokale Anwendungen können per Lift & Shift zu Azure migriert werden und Benutzer weiterhin nahtlos authentifizieren, ohne dass Änderungen an der Konfiguration oder der Benutzeroberfläche vorgenommen werden müssen.

### <a name="deployment-notes"></a>Hinweise zur Bereitstellung

Beachten Sie die folgenden Bereitstellungsaspekte für diesen Beispielanwendungsfall:

* Stellen Sie sicher, dass es für die Anwendung nicht erforderlich ist, das Verzeichnis zu ändern oder in das Verzeichnis zu schreiben. Der LDAP-Schreibzugriff auf eine verwaltete Azure AD DS-Domäne wird nicht unterstützt.
* Sie können Kennwörter nicht direkt für eine verwaltete Azure AD DS-Domäne ändern. Endbenutzer können ihr Kennwort entweder mithilfe des Self-Service-Verfahrens zum Ändern des Kennworts von Azure AD oder basierend auf dem lokalen Verzeichnis ändern. Diese Änderungen werden dann automatisch synchronisiert und sind in der verwalteten Azure AD DS-Domäne verfügbar.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-read-to-access-the-directory"></a>Migrieren lokaler Anwendungen per Lift & Shift, die LDAP-Lesevorgänge zum Zugreifen auf das Verzeichnis verwenden

Wie im vorherigen Beispielszenario gehen wir davon aus, dass Contoso über eine lokale Branchenanwendung verfügt, die vor fast zehn Jahren entwickelt wurde. Diese Anwendung ist verzeichnisfähig und wurde für die Verwendung von LDAP zum Lesen von Informationen/Attributen der Benutzer aus AD DS konzipiert. Die Anwendung ändert keine Attribute und schreibt auch nicht in das Verzeichnis.

Contoso möchte diese Anwendung zu Azure migrieren und die ältere lokale Hardware außer Betrieb nehmen, auf der diese Anwendung aktuell gehostet wird. Die Anwendung kann nicht zur Verwendung moderner Verzeichnis-APIs (wie der REST-basierten Azure AD Graph-API) umgeschrieben werden. Eine Option zum Migrieren per Lift & Shift ist erwünscht, bei der die Anwendung zur Ausführung in der Cloud migriert werden kann, ohne dass Code geändert oder die Anwendung umgeschrieben wird.

Zur Unterstützung dieses Szenarios ermöglicht Azure AD DS Anwendungen die Ausführung von LDAP-Lesevorgängen für die verwaltete Domäne, um die erforderlichen Attributinformationen zu erhalten. Die Anwendung muss nicht umgeschrieben werden, sodass Benutzer durch eine Migration per Lift & Shift zu Azure die App weiterhin verwenden können, ohne zu merken, dass sich der Ort der Ausführung ändert.

### <a name="deployment-notes"></a>Hinweise zur Bereitstellung

Beachten Sie die folgenden Bereitstellungsaspekte für diesen Beispielanwendungsfall:

* Stellen Sie sicher, dass es für die Anwendung nicht erforderlich ist, das Verzeichnis zu ändern oder in das Verzeichnis zu schreiben. Der LDAP-Schreibzugriff auf eine verwaltete Azure AD DS-Domäne wird nicht unterstützt.
* Stellen Sie sicher, dass die Anwendung kein benutzerdefiniertes/erweitertes Active Directory-Schema benötigt. Schemaerweiterungen werden in Azure AD DS nicht unterstützt.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure"></a>Migrieren eines lokalen Diensts oder einer Daemonanwendung zu Azure

Einige Anwendungen umfassen mehrere Ebenen, wobei eine der Ebenen authentifizierte Aufrufe einer Back-End-Ebene (z. B. einer Datenbank) ausführen muss. In diesen Szenarien werden häufig AD-Dienstkonten verwendet. Wenn Sie Anwendungen per Lift & Shift zu Azure migrieren, können Sie mit Azure AD DS Dienstkonten weiterhin auf die gleiche Weise verwenden. Sie können das gleiche Dienstkonto verwenden, das aus Ihrem lokalen Verzeichnis mit Azure AD synchronisiert wird, oder eine benutzerdefinierte Organisationseinheit erstellen und dann ein separates Dienstkonto in dieser Organisationseinheit erstellen. Bei beiden Ansätzen funktionieren Anwendungen weiterhin auf die gleiche Weise, um authentifizierte Aufrufe anderer Ebenen und Dienste auszuführen.

![Dienstkonto mit WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

In diesem Beispielszenario verfügt Contoso über eine benutzerdefinierte Softwaretresoranwendung, die ein Web-Front-End, eine SQL Server-Instanz und einen Back-End-FTP-Server umfasst. Die integrierte Windows-Authentifizierung mithilfe von Dienstkonten authentifiziert das Web-Front-End beim FTP-Server. Das Web-Front-End wird zur Ausführung als Dienstkonto eingerichtet. Der Back-End-Server wird zum Autorisieren des Zugriffs vom Dienstkonto für das Web-Front-End konfiguriert. Contoso möchte nicht die eigenen Domänencontroller-VMs in der Cloud bereitstellen und verwalten, um diese Anwendung in Azure zu verschieben.

In diesem Szenario können die Server, auf denen das Web-Front-End, die SQL Server-Instanz und der FTP-Server gehostet werden, zu Azure-VMs migriert und in eine verwaltete Azure AD DS-Domäne eingebunden werden. Die VMs können dann das gleiche Dienstkonto im lokalen Verzeichnis für die Authentifizierungszwecke der App verwenden, das über Azure AD mithilfe von Azure AD Connect synchronisiert wird.

### <a name="deployment-notes"></a>Hinweise zur Bereitstellung

Beachten Sie die folgenden Bereitstellungsaspekte für diesen Beispielanwendungsfall:

* Stellen Sie sicher, dass die Anwendungen für die Authentifizierung einen Benutzernamen und ein Kennwort verwenden. Die zertifikat- oder smartcardbasierte Authentifizierung wird von Azure AD DS nicht unterstützt.
* Sie können Kennwörter nicht direkt für eine verwaltete Azure AD DS-Domäne ändern. Endbenutzer können ihr Kennwort entweder mithilfe des Self-Service-Verfahrens zum Ändern des Kennworts von Azure AD oder basierend auf dem lokalen Verzeichnis ändern. Diese Änderungen werden dann automatisch synchronisiert und sind in der verwalteten Azure AD DS-Domäne verfügbar.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Bereitstellungen von Windows Server-Remotedesktopdiensten in Azure

Mit Azure AD DS können Sie verwaltete Domänendienste für in Azure bereitgestellte Remotedesktopserver bereitstellen. Weitere Informationen zu diesem Bereitstellungsszenario finden Sie unter [Integrieren von Azure AD Domain Services in Ihre RDS-Bereitstellung][windows-rds].

## <a name="domain-joined-hdinsight-clusters-preview"></a>In die Domäne eingebundene HDInsight-Cluster (Vorschau)

Sie können einen in eine verwaltete Azure AD DS-Domäne eingebundenen Azure HDInsight-Cluster einrichten, für den Apache Ranger aktiviert ist. Sie können Hive-Richtlinien über Apache Ranger erstellen und anwenden und zulassen, dass Benutzer (z. B. Datenanalysten) mithilfe von ODBC-basierten Tools wie Excel oder Tableau eine Verbindung mit Hive herstellen. Wir arbeiten weiterhin daran, andere Workloads wie HBase, Spark und Storm zu HDInsight-Instanzen hinzuzufügen, die in eine Domäne eingebunden sind.

Weitere Informationen zu diesem Bereitstellungsszenario finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern][hdinsight].

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][tutorial-create-instance].

<!-- INTERNAL LINKS -->
[hdinsight]: ../hdinsight/domain-joined/apache-domain-joined-configure.md
[tutorial-create-instance]: tutorial-create-instance.md

<!-- EXTERNAL LINKS -->
[windows-rds]: /windows-server/remote/remote-desktop-services/rds-azure-adds
