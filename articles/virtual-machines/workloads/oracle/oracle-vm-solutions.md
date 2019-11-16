---
title: Oracle-Lösungen auf virtuellen Azure-Computern | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu unterstützten Konfigurationen und Einschränkungen von VM-Images für Oracle in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: rogirdh
ms.custom: seodec18
ms.openlocfilehash: 23e638b1d678e6ecf19c23220828185eb0e25a00
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73891453"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Oracle VM-Images und deren Bereitstellung in Microsoft Azure

Dieser Artikel enthält Informationen zu Oracle-Lösungen, die auf VM-Images basieren, die von Oracle im Azure Marketplace veröffentlicht wurden. Wenn Sie an cloudübergreifenden Anwendungslösungen mit Oracle Cloud Infrastructure interessiert sind, finden Sie Informationen unter [Oracle-Anwendungslösungen, die Microsoft Azure in Oracle Cloud Infrastructure integrieren](oracle-oci-overview.md).

Um eine Liste der derzeit verfügbaren Images zu erhalten, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

Seit Mai 2019 sind folgende Images verfügbar:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Diese Images werden als „Bring Your Own License“ betrachtet. Daher werden nur die Kosten für Compute, Speicherung und Netzwerke berechnet, die durch die Ausführung eines virtuellen Computers anfallen.  Es wird vorausgesetzt, dass Sie über eine ordnungsgemäße Lizenz für die Verwendung von Oracle-Software und über einen aktuellen Supportvertrag mit Oracle verfügen. Oracle bietet eine garantierte Lizenzmobilität von einer lokalen Verwendung zu Azure. Informationen zur Lizenzmobilität finden Sie im veröffentlichten Artikel [Oracle und Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

Einzelpersonen können als Basis für ihre Lösungen auch ein benutzerdefiniertes Image verwenden, das sie in Azure von Grund auf neu erstellen, oder sie können ein benutzerdefiniertes Image aus ihren lokalen Umgebungen hochladen.

## <a name="oracle-database-vm-images"></a>Oracle Database VM-Images
Oracle unterstützt die Ausführung der Standard und Enterprise Edition von Oracle DB 12.1 und höher in Azure in VM-Images, die auf Oracle Linux basieren.  Um die optimale Leistung für Produktionsworkloads von Oracle DB in Azure zu erzielen, achten Sie darauf, dass Sie das VM-Image auf eine geeignete Größe festlegen und verwaltete SSD Premium- oder SSD Ultra-Datenträger verwenden. Anweisungen, wie Sie eine Oracle DB mit dem von Oracle veröffentlichten VM-Image schnell in Azure einrichten, erhalten Sie im [Oracle DB-Schnellstart](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Optionen für die Konfiguration angefügter Datenträger

Angefügte Datenträger nutzen den Azure Blob Storage-Dienst. Auf jedem Standarddatenträger können theoretisch bis zu 500 E/A-Vorgänge pro Sekunde (IOPS) ausgeführt werden. Unser Premium-Datenträgerangebot wird in erster Linie für Datenbanknutzlasten mit hoher Leistung verwendet und kann bis zu 5000 IOps pro Datenträger erreichen. Sie können einen einzelnen Datenträger verwenden, wenn dieser Ihre Leistungsanforderungen erfüllt. Sie können die effektive IOPS-Leistung aber auch häufig verbessern, wenn Sie mehrere angefügte Datenträger verwenden, Datenbankdaten auf diesen verteilen und dann Oracle Automatic Storage Management (ASM) verwenden. Spezifische Informationen zu Oracle ASM finden Sie unter [Oracle Automatic Storage Management Overview](https://www.oracle.com/technetwork/database/index-100339.html) (in englischer Sprache). Ein Beispiel für das Installieren und Konfigurieren von Oracle ASM auf einem virtuellen Azure-Linux-Computer finden Sie im Tutorial [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) (Installieren und Konfigurieren von Oracle Automated Storage Management).

### <a name="shared-storage-configuration-options"></a>Konfigurationsoptionen für freigegebenen Speicher

Azure NetApp Files wurde entworfen, um grundlegende Anforderungen für die Ausführung von Hochleistungsworkloads wie Datenbanken in der Cloud zu erfüllen. Der Dienst bietet Folgendes:
- Nativen Azure-Dienst für freigegebenen NFS-Speicher zum Ausführen von Oracle-Workloads entweder mit einem nativen NFS-Client der VM oder mit Oracle dNFS
- Skalierbare Leistungsstufen, die sich an tatsächlichen IOPS-Anforderungen orientieren
- Geringe Wartezeit
- Hochverfügbarkeit, hohe Dauerhaftigkeit und Verwaltbarkeit im jedem Umfang – in der Regel für geschäftskritische Unternehmensworkloads (wie SAP und Oracle)
- Schnelle und effiziente Sicherung und Wiederherstellung, für die höchsten RTO- und RPO-SLAs

Diese Funktionen sind möglich, da Azure NetApp Files auf den vollständig auf Flashspeicher basierenden NetApp® ONTAP®-Systemen aufbaut, die in Azure-Rechenzentren als ein nativer Azure-Dienst ausgeführt werden. Das Ergebnis ist eine perfekte Speichertechnologie für Datenbanken, die genau wie andere Azure Storage-Optionen bereitgestellt und genutzt werden kann. In der [Dokumentation zu Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/) finden Sie weitere Informationen zum Bereitstellen von Azure NetApp Files-NFS-Volumes und zum Zugreifen darauf. Im Dokument [Oracle on Azure deployment best practice guide using Azure NetApp Files](https://www.netapp.com/us/media/tr-4780.pdf) (Oracle auf Azure-Bereitstellung – Leitfaden mit bewährten Methoden unter Verwendung von Azure NetApp Files) finden Sie Empfehlungen für den Betrieb einer Oracle-Datenbank in Azure NetApp Files.


## <a name="licensing-oracle-database--software-on-azure"></a>Lizenzieren von Oracle Database und Software in Azure
Microsoft Azure ist eine autorisierte Cloudumgebung zum Ausführen von Oracle Database. Die Oracle Core Factor-Tabelle ist nicht anwendbar, wenn Sie Oracle-Datenbanken in der Cloud lizenzieren. Stattdessen können Sie, wenn Sie virtuelle Computer mit Hyperthreading-Technologie verwenden, die für Enterprise Edition-Datenbanken aktiviert ist, zwei vCPUs als gleichwertig zu einer Oracle Prozessor-Lizenz zählen, wenn Hyperthreading aktiviert ist (wie im Richtliniendokument angegeben). Die Richtliniendetails finden Sie [hier](http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf).
Oracle-Datenbanken erfordern grundsätzlich mehr Arbeitsspeicher und E/A. Aus diesem Grund werden für diese Workloads [virtuelle Computer mit optimierter Arbeitsspeichergröße](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) empfohlen. Um Ihre Workloads weiter zu optimieren, werden [vCPUs mit eingeschränkte Kernanzahl](https://docs.microsoft.com/azure/virtual-machines/linux/constrained-vcpu) für Oracle DB-Workloads empfohlen, die viel Arbeitsspeicher, Speicher und E/A-Bandbreite, aber keine hohe Anzahl von Kernen erfordern.

Wenn Oracle-Software und -Workloads aus einer lokalen Umgebung zu Microsoft Azure migriert werden, bietet Oracle Lizenzmobilität, wie sie unter [Oracle and Microsoft Azure: Frequently Asked Questions](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html) beschrieben ist.


## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle Real Application Cluster (Oracle RAC)
Oracle RAC ist so konzipiert, dass der Ausfall eines einzelnen Knotens in einer lokalen Clusterkonfiguration mit mehreren Knoten möglichst geringe Auswirkungen hat. RAC beruht auf zwei lokalen Technologien, die in öffentlichen Cloudumgebungen mit Hyperskalierung nicht nativ funktionieren: Netzwerk-Multicast und freigegebene Datenträger. Wenn Ihre Datenbanklösung Oracle RAC in Azure erfordert, benötigen Sie Software von Drittanbietern, um diese Technologien zu aktivieren. Weitere Informationen zu Oracle RAC finden Sie auf der [Seite zu FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Überlegungen zu Hochverfügbarkeit und Notfallwiederherstellung
Wenn Sie Oracle-Datenbanken in Azure verwenden, müssen Sie eine Lösung für Hochverfügbarkeit und Notfallwiederherstellung implementieren, um Ausfallzeiten zu vermeiden. 

Hochverfügbarkeit und Notfallwiederherstellung für Oracle Database Enterprise Edition (ohne Abhängigkeit von Oracle RAC) lassen sich in Azure mithilfe von [Data Guard, Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) oder [Oracle GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate) mit zwei Datenbanken auf zwei getrennten virtuellen Computern erreichen. Beide virtuellen Computer sollten sich in demselben [virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) befinden, um sicherzustellen, dass sie über die private statische IP-Adresse Zugriff aufeinander haben.  Darüber hinaus wird empfohlen, die virtuellen Computer in derselben Verfügbarkeitsgruppe zu platzieren, damit sie von Azure in eigenen Fehlerdomänen und Upgradedomänen angeordnet werden können. Falls Sie Georedundanz verwenden möchten, richten Sie die zwei Datenbanken so ein, dass sie zwischen verschiedenen Regionen repliziert werden und die zwei Instanzen mit einem VPN Gateway verbinden.

Das Tutorial [Implementieren von Oracle DataGuard in Azure](configure-oracle-dataguard.md) führt Sie durch die grundlegenden Schritte der Einrichtung unter Azure.  

Mit Oracle Data Guard kann Hochverfügbarkeit mit einer primären Datenbank auf einem virtuellen Computer, einer sekundären Datenbank (Standbydatenbank) auf einem weiteren virtuellen Computer und unidirektionaler Replikation zwischen diesen Komponenten erzielt werden. Das Ergebnis ist Lesezugriff auf die Kopie der Datenbank. Mit Oracle GoldenGate können Sie bidirektionale Replikation zwischen den beiden Datenbanken konfigurieren. Informationen zum Einrichten einer Hochverfügbarkeitslösung für die Datenbanken mithilfe dieser Tools finden Sie in der Dokumentation zu [Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) und [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) auf der Oracle-Website. Wenn Sie Schreibzugriff auf die Kopie der Datenbank benötigen, können Sie [Oracle Active Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html)verwenden.

Das Tutorial [Implementieren von Oracle GoldenGate in Azure](configure-oracle-golden-gate.md) führt Sie durch die grundlegenden Schritte der Einrichtung unter Azure.

Zusätzlich dazu, dass Sie eine Lösung für Hochverfügbarkeit und Notfallwiederherstellung in Azure besitzen, sollten Sie über eine Sicherungsstrategie zum Wiederherstellen der Datenbank verfügen. Das Tutorial zum [Sichern und Wiederherstellen einer Oracle-Datenbank](oracle-backup-recovery.md) führt Sie durch das grundlegenden Verfahren der Einrichtung einer konsistenten Sicherung.


## <a name="support-for-jd-edwards"></a>Unterstützung für JD Edwards
Laut dem Oracle-Support-Artikel [Dok.-ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) werden JD Edwards EnterpriseOne, Versionen 9.2 und höher, in **jedem öffentlichen Cloudangebot** unterstützt, das ihre spezifischen `Minimum Technical Requirements` (MTR, technische Mindestanforderungen) erfüllt.  Sie müssen benutzerdefinierte Images erstellen, die ihre MTR-Spezifikationen für das Betriebssystem und die Anwendungskompatibilität erfüllen. 


## <a name="oracle-weblogic-server-virtual-machine-images"></a>Images von virtuellen Oracle WebLogic Server-Computern

* **Clustering wird nur in der Enterprise Edition unterstützt.** Sie sind nur für die Verwendung von WebLogic-Clustern lizenziert, wenn Sie die Enterprise Edition von WebLogic Server verwenden. Verwenden Sie Clustering nicht mit der WebLogic Server Standard Edition.
* **UDP-Multicast wird nicht unterstützt.** Azure unterstützt UDP-Unicasting, jedoch weder Multicasting noch Broadcasting. WebLogic Server kann die UDP-Unicast-Funktionen von Azure nutzen. Um optimale Ergebnisse bei der Nutzung von UDP-Unicast zu erzielen, empfiehlt es sich, eine statische Größe des WebLogic-Clusters zu verwenden oder nicht mehr als 10 verwaltete Server aufzunehmen.
* **WebLogic Server erwartet, dass für den T3-Zugriff (z.B. bei Verwendung von Enterprise JavaBeans) öffentliche und private Ports identisch sind.** Beispielszenario: Eine Anwendung auf Dienstebene (EJB) wird in einem WebLogic-Servercluster, der aus mindestens zwei VMs besteht, in einem virtuellen Netzwerk mit dem Namen *SLWLS* ausgeführt. Die Clientebene befindet sich in einem anderen Subnetz in demselben virtuellen Netzwerk, und es wird ein einfaches Java-Programm ausgeführt, das versucht, EJB auf der Dienstebene aufzurufen. Da die Dienstebene einen Lastenausgleich erfordert, muss für die virtuellen Computer im WebLogic Server-Cluster ein öffentlicher Endpunkt mit Lastenausgleich erstellt werden. Wenn der private Port, den Sie angeben, nicht mit dem öffentlichen Port identisch ist (z.B. 7006:7008), tritt ein Fehler wie der folgende auf:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Der Grund dafür ist, dass WebLogic Server für jeden T3-Remotezugriff erwartet, dass der Port für das Load Balancer-Modul und der Port des verwalteten WebLogic-Servers identisch sind. Im obigen Fall greift der Client auf Port 7006 (den Port für den Lastenausgleich) zu, und der verwaltete Server lauscht an Port 7008 (dem privaten Port). Diese Einschränkung gilt nur für den T3-Zugriff und nicht für HTTP.

   Sie können dieses Problem mit einer der folgenden Problemumgehungen vermeiden:

  * Verwenden Sie für Endpunkte mit Lastenausgleich, die für T3-Zugriff vorgesehen sind, die gleiche Nummer für den öffentlichen und privaten Port.
  * Schließen Sie beim Starten von WebLogic Server den folgenden JVM-Parameter ein:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Verwandte Informationen finden Sie im KB-Artikel **860340.1** unter <https://support.oracle.com>.

* **Einschränkungen für dynamisches Clustering und Lastenausgleich**. Angenommen, Sie möchten in WebLogic Server einen dynamischen Cluster verwenden und ihn in Azure über einen einzelnen Endpunkt mit Lastenausgleich verfügbar machen. Dies ist möglich, solange Sie für jeden verwalteten Server eine feste Portnummer (keine dynamisch aus einem Bereich zugewiesene Nummer) verwenden und nicht mehr verwaltete Server starten, als Computer vorhanden sind, die vom Administrator nachverfolgt werden. Das heißt, es gibt nicht mehr als einen verwalteten Server pro virtuellem Computer. Wenn die Konfiguration bewirkt, dass mehr WebLogic-Server gestartet werden, als virtuelle Computer vorhanden sind (d.h., wenn mehrere WebLogic Server-Instanzen denselben virtuellen Computer verwenden), kann nicht mehr als eine dieser WebLogic-Serverinstanzen an eine angegebene Portnummer gebunden werden. Die anderen Instanzen auf diesem virtuellen Computer schlagen fehl.

   Wenn Sie festlegen, dass der Administratorserver automatisch den verwalteten Servern eindeutige Portnummern zuweist, ist kein Lastenausgleich möglich. Der Grund dafür ist, dass diese Konfiguration das Zuordnen eines einzelnen öffentlichen Ports zu mehreren privaten Ports erfordert. Dies wird jedoch von Azure nicht unterstützt.
* **Mehrere Instanzen von WebLogic Server auf einem virtuellen Computer**. Je nach den Anforderungen der Bereitstellung sollten Sie in Betracht ziehen, mehrere Instanzen von WebLogic Server auf demselben virtuellen Computer auszuführen, sofern die Größe des virtuellen Computers ausreicht. Beispielsweise können Sie auf einem virtuellen Computer mittlerer Größe, der zwei Kerne enthält, zwei Instanzen von WebLogic Server ausführen. Wir raten aber immer noch davon ab, nur einen virtuellen Computer zu verwenden, auf dem mehrere Instanzen von WebLogic Server ausgeführt werden, da hierdurch einzelne Fehlerquellen in die Architektur eingeführt werden. Ein besserer Ansatz ist die Verwendung von mindestens zwei virtuellen Computern. Auf jedem virtuellen Computer könnten dann mehrere Instanzen von WebLogic Server ausgeführt werden. Jede Instanz von WebLogic Server kann dennoch Teil desselben Clusters sein. Zurzeit kann Azure jedoch nicht für den Lastenausgleich von Endpunkten verwendet werden, die durch WebLogic Server-Bereitstellungen auf demselben virtuellen Computer verfügbar gemacht werden. Der Grund dafür ist, dass der Azure-Load Balancer die Verteilung der Server mit Lastenausgleich auf eindeutige virtuelle Computer erfordert.

## <a name="oracle-jdk-virtual-machine-images"></a>Images von virtuellen Oracle JDK-Computern
* **Neueste Updates von JDK 6 und 7**. Es wird zwar empfohlen, die neueste öffentliche unterstützte Version von Java (derzeit Java 8) zu verwenden, aber für Azure werden auch JDK 6- und JDK 7-Images verfügbar gemacht. Diese sind für ältere Anwendungen vorgesehen, die noch nicht auf JDK 8 aktualisiert werden können. Updates früherer JDK-Images sind möglicherweise nicht mehr für die allgemeine Öffentlichkeit verfügbar. Aufgrund der Partnerschaft von Microsoft mit Oracle bieten die von Azure bereitgestellten JDK 6- und JDK 7-Images jedoch ein neueres nicht öffentliches Update, das Oracle normalerweise nur einer bestimmten Gruppe unterstützter Oracle-Kunden anbietet. Im Lauf der Zeit werden neuere Version der JDK-Images mit aktualisierten Versionen von JDK 6 und 7 zur Verfügung gestellt.

   Das in den JDK 6- und JDK 7-Images verfügbare JDK und die von ihnen abgeleiteten virtuellen Computer und Images können nur in Azure verwendet werden.
* **64-Bit-JDK.** Die von Azure bereitgestellten Images virtueller Oracle WebLogic Server-Computer und virtueller Oracle JDK-Computer enthalten die 64-Bit-Versionen von Windows Server und dem JDK.

## <a name="next-steps"></a>Nächste Schritte
Sie haben jetzt einen Überblick über aktuelle Oracle-Lösungen, die auf VM-Images basieren, in Microsoft Azure. Der nächste Schritt besteht darin, die erste Oracle-Datenbank in Azure bereitzustellen.

> [!div class="nextstepaction"]
> [Erstellen einer Oracle-Datenbank in Azure](oracle-database-quick-create.md)
