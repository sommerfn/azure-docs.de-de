---
title: Herstellen einer Verbindung mit einem virtuellen Azure SQL-Computer für die Suchindizierung
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie verschlüsselte Verbindungen, und konfigurieren Sie die Firewall für Verbindungen mit SQL Server auf einem virtuellen Azure-Computer (VM) über einen Indexer in der kognitiven Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 57bea41d95b8859af55be777b17189861a445a12
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792146"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-server-on-an-azure-vm"></a>Konfigurieren einer Verbindung eines Indexers der kognitiven Azure-Suche mit SQL Server auf einer Azure-VM

Wie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq) erläutert, wird das Erstellen von Indexern für **SQL Server auf Azure-VMs** (oder kurz **SQL Azure-VMs**) in der kognitiven Azure-Suche unterstützt. Zunächst müssen aber einige sicherheitsbezogene Voraussetzungen erfüllt werden. 

Verbindungen zwischen der kognitiven Azure-Suche und SQL Server auf einer VM erfolgen über eine öffentliche Internetverbindung. Alle Sicherheitsmaßnahmen, die Sie normalerweise für diese Verbindungen befolgen würden, gelten auch hier:

+ Erhalten Sie ein Zertifikat von einem [Zertifizierungsstellenanbieter](https://en.wikipedia.org/wiki/Certificate_authority#Providers) für den voll qualifizierten Domänennamen der SQL Server-Instanz auf der Azure-VM.
+ Installieren Sie das Zertifikat auf der VM, und aktivieren und konfigurieren Sie dann verschlüsselte Verbindungen auf der VM mithilfe der Anweisungen in diesem Artikel.

## <a name="enable-encrypted-connections"></a>Aktivieren von verschlüsselten Verbindungen
Bei der kognitiven Azure-Suche ist für alle Indexeranforderungen über eine öffentliche Internetverbindung ein verschlüsselter Kanal erforderlich. In diesem Abschnitt sind die entsprechenden Schritte angegeben.

1. Überprüfen Sie die Eigenschaften des Zertifikats, um sicherzustellen, dass der Antragstellername der vollqualifizierte Domänenname (FQDN) der Azure-VM ist. Sie können ein Tool wie CertUtils oder das Zertifikat-Snap-In zum Anzeigen der Eigenschaften verwenden. Sie finden den vollqualifizierten Domänennamen im **Azure-Portal** auf dem Blatt des VM-Diensts unter „Zusammenfassung“ im Feld [Öffentliche IP-Adresse/DNS-Namensbezeichnung](https://portal.azure.com/).
   
   * Für VMs, die mit der neueren **Resource Manager**-Vorlage erstellt wurden, hat der FQDN das Format `<your-VM-name>.<region>.cloudapp.azure.com`.
   * Für ältere virtuelle Computer, die als **klassische** VM erstellt wurden, hat der FQDN das Format `<your-cloud-service-name.cloudapp.net>`.

2. Konfigurieren Sie SQL Server für die Verwendung des Zertifikats mit dem Registrierungs-Editor (regedit). 
   
    SQL Server-Konfigurations-Manager wird zwar häufig für diese Aufgabe verwendet, aber für dieses Szenario ist dies nicht möglich. Das importierte Zertifikat kann nicht gefunden werden, da der FQDN der VM unter Azure nicht mit dem von der VM ermittelten FQDN übereinstimmt (Domäne wird entweder als lokaler Computer oder als Netzwerkdomäne identifiziert, für die der Beitritt durchgeführt wurde). Verwenden Sie „regedit“, um das Zertifikat anzugeben, wenn die Namen nicht übereinstimmen.
   
   * Navigieren Sie in „regedit“ zu diesem Registrierungsschlüssel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     Der Teil `[MSSQL13.MSSQLSERVER]` variiert je nach Version und Instanzname. 
   * Legen Sie den Wert des Schlüssels **Certificate** (Zertifikat) auf den **Thumbprint** (Fingerabdruck) des SSL-Zertifikats fest, das Sie in die VM importiert haben.
     
     Es gibt mehrere Möglichkeiten zur Beschaffung des Fingerabdrucks, von denen einige besser als andere geeignet sind. Wenn Sie ihn aus dem **Zertifikate**-Snap-In in MMC kopieren, ist unter Umständen ein unsichtbares vorangestelltes Zeichen enthalten. Dies führt zu einem Fehler, wenn Sie versuchen, die Verbindung herzustellen. [Informationen hierzu finden Sie in diesem Supportartikel](https://support.microsoft.com/kb/2023869/). Für dieses Problem gibt es mehrere Lösungen. Die einfachste Methode ist die Verwendung der Rückschritttaste und das erneute Eingeben des ersten Zeichens des Fingerabdrucks, um das vorangestellte Zeichen im Schlüsselwertfeld des Registrierungs-Editors zu entfernen. Alternativ dazu können Sie auch ein anderes Tool zum Kopieren des Fingerabdrucks verwenden.

3. Gewähren Sie Berechtigungen für das Dienstkonto. 
   
    Stellen Sie sicher, dass dem SQL Server-Dienstkonto die entsprechende Berechtigung für den privaten Schlüssel des SSL-Zertifikats gewährt wird. Wenn Sie diesen Schritt übersehen, startet SQL Server nicht. Sie können das **Zertifikate**-Snap-In oder **CertUtils** für diese Aufgabe verwenden.
    
4. Starten Sie den SQL Server-Dienst neu.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfigurieren der SQL Server-Konnektivität in der VM
Nachdem Sie die für die kognitive Azure-Suche erforderliche verschlüsselte Verbindung eingerichtet haben, müssen noch weitere Konfigurationsschritte für SQL Server auf Azure-VMs ausgeführt werden. Falls dies noch nicht erfolgt ist, ist der nächste Schritt das Abschließen der Konfiguration mit einem dieser Artikel:

* Informationen für eine **Resource Manager** -VM finden unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* Informationen für eine **klassische** VM finden unter [Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer in Azure (Klassische Bereitstellung)](../virtual-machines/windows/classic/sql-connect.md).

Sehen Sie sich in den Artikeln vor allem jeweils den Abschnitt zum „Verbinden über das Internet“ an.

## <a name="configure-the-network-security-group-nsg"></a>Konfigurieren der Netzwerksicherheitsgruppe (NSG)
Es ist nicht ungewöhnlich, die NSG und den entsprechenden Azure-Endpunkt oder die Zugriffssteuerungsliste (Access Control List, ACL) zu konfigurieren, um Ihre Azure-VM für andere Parteien zugänglich zu machen. Wahrscheinlich haben Sie diese Konfiguration bereits durchgeführt, damit Ihre eigene Anwendungslogik die Verbindung mit Ihrer SQL Azure-VM herstellen kann. Für eine Verbindung zwischen der kognitiven Azure-Suche und Ihrer SQL Azure-VM ist es nicht anders. 

Die folgenden Links führen zu Anleitungen zur NSG-Konfiguration für VM-Bereitstellungen. Verwenden Sie diese Anleitungen, um für einen Endpunkt der kognitiven Azure-Suche basierend auf der IP-Adresse eine Zugriffssteuerungsliste einzurichten.

> [!NOTE]
> Ausführlichere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe?](../virtual-network/security-overview.md)
> 
> 

* Informationen für eine **Resource Manager** -VM finden unter [Verwalten von NSGs mithilfe des Azure-Portals](../virtual-network/tutorial-filter-network-traffic.md). 
* Informationen für eine **klassische** VM finden Sie unter [Erstellen von NSGs (klassisch) in PowerShell](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

Die IP-Adressierung kann mit einigen Schwierigkeiten verbunden sein, die leicht bewältigt werden können, wenn Sie mit den Problemen und den entsprechenden Lösungen bereits vertraut sind. Die restlichen Abschnitte enthalten Empfehlungen für die Behandlung von Problemen in Bezug auf IP-Adressen in der ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Einschränken des Zugriffs auf die Suchdienst-IP-Adresse
Wir empfehlen dringend, den Zugriff auf die IP-Adresse Ihres Suchdiensts in der ACL einzuschränken, anstatt Ihre SQL Azure-VMs für alle Verbindungsanfragen generell zu öffnen. Sie können die IP-Adresse auf einfache Weise herausfinden, indem Sie den vollqualifizierten Domänennamen (z.B. `<your-search-service-name>.search.windows.net`) Ihres Suchdiensts pingen.

#### <a name="managing-ip-address-fluctuations"></a>Verwalten der Fluktuation von IP-Adressen
Wenn Ihr Suchdienst nur über eine Sucheinheit verfügt (also ein Replikat und eine Partition), ändert sich die IP-Adresse bei routinemäßigen Neustarts des Diensts, sodass eine vorhandene ACL mit der IP-Adresse Ihres Suchdiensts ungültig wird.

Eine Möglichkeit zur Vermeidung des nachfolgenden Verbindungsfehlers ist die Verwendung von mehreren Replikaten und mehreren Partitionen in der kognitiven Azure-Suche. Die Kosten erhöhen sich, aber gleichzeitig wird das Problem mit den IP-Adressen gelöst. In der kognitiven Azure-Suche ändern sich die IP-Adressen nicht, wenn Sie über mehrere Sucheinheiten verfügen.

Ein zweiter Ansatz besteht darin, den Verbindungsfehler zuzulassen und die ACLs in der NSG dann neu zu konfigurieren. IP-Adressen ändern sich normalerweise jeweils nach einigen Wochen. Für Kunden, die in unregelmäßigen Abständen eine kontrollierte Indizierung durchführen, ist dieser Ansatz ggf. gut geeignet.

Der dritte mögliche (wenn auch nicht besonders sichere) Ansatz besteht darin, den IP-Adressbereich der Azure-Region anzugeben, in der Ihr Suchdienst bereitgestellt wurde. Die Liste mit den IP-Adressbereichen, aus denen Azure-Ressourcen öffentliche IP-Adressen zugewiesen werden, finden Sie unter [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-cognitive-search-portal-ip-addresses"></a>Einbeziehen der IP-Adressen des Portals für die kognitive Azure-Suche
Wenn Sie zum Erstellen eines Indexers das Azure-Portal verwenden, benötigt die Logik des Portals für die kognitive Azure-Suche auch während der Erstellung Zugriff auf die SQL Azure-VM. Die IP-Adressen des Portals für die kognitive Azure-Suche ermitteln Sie durch Pingen von `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Nächste Schritte
Da die Konfiguration nun abgeschlossen ist, können Sie jetzt eine SQL Server-Instanz auf der Azure-VM als Datenquelle für einen Indexer der kognitiven Azure angeben. Weitere Informationen finden Sie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).

