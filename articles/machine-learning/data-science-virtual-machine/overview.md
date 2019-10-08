---
title: Worum handelt es sich bei Azure Data Science Virtual Machine?
description: Wichtige Analyseszenarien und -komponenten für Windows und Linux Data Science Virtual Machines.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 02/22/2019
ms.openlocfilehash: 8359a8407e93a2b31466342b82539cef04b0fe01
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802193"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Was ist Azure Data Science Virtual Machine für Linux und Windows?

Data Science Virtual Machine (DSVM) ist ein angepasstes, auf der Azure-Cloudplattform basierendes VM-Image, das speziell für Data Science erstellt wurde. Es hat viele beliebte Data Science-Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. 

Die DSVM-Instanz ist für folgende Betriebssysteme verfügbar:
+ Windows Server 2016
+ Ubuntu 16.04 LTS und CentOS 7.4

> [!NOTE]
> Alle VM-Tools für Deep Learning wurden in Data Science Virtual Machine integriert. 


## <a name="why-choose-the-dsvm"></a>Warum sollten Sie sich für DSVM entscheiden?
Das Ziel von Data Science Virtual Machine besteht darin, für Datenexperten aller Art branchenübergreifend eine reibungslos funktionierende und vorkonfigurierte Data Science-Umgebung bereitzustellen. Sie können also eine DSVM-Instanz bereitstellen, anstatt selbst einen vergleichbaren Arbeitsbereich erstellen zu müssen. Dadurch können Sie Tage oder sogar _Wochen_ bei der Installation, Konfiguration und Paketverwaltung sparen. Nachdem Ihre DSVM zugeordnet wurde, können Sie sofort mit der Arbeit an Ihren Data Science-Projekten beginnen.

## <a name="sample-use-cases"></a>Beispiele für Anwendungsfälle

Im Folgenden werden einige gängige Anwendungsfälle für DSVM-Kunden veranschaulicht.

### <a name="moving-data-science-workloads-to-the-cloud"></a>Verschieben von Data Science-Workloads in die Cloud

DSVM bietet eine Basiskonfiguration für Data Science-Teams, die ihre lokalen Desktops durch einen verwalteten Clouddesktop ersetzen möchten. Damit wird sichergestellt, dass alle Datenexperten im Team über ein einheitliches Setup verfügen, auf das sie ihre Zusammenarbeit stützen und mit dem sie Experimente verifizieren können. Sie trägt ferner zu einer Kostenreduzierung durch eine Verringerung des Aufwands für die Systemverwaltung. Dies wiederum führt zu einer Zeitersparnis bei der Evaluierung, Installation und Verwaltung von Softwarepaketen für erweiterte Analysen.

### <a name="data-science-training-and-education"></a>Data Science-Schulung und -Ausbildung
Ausbilder in Unternehmen und Dozenten, die Data Science-Kurse leiten, stellen in der Regel ein Image eines virtuellen Computers bereit. Durch das Image wird gewährleistet, dass für die Kursteilnehmer eine konsistente Umgebung eingerichtet wird und die Beispiele erwartungsgemäß funktionieren. 

DSVM erstellt eine bedarfsgerechte Umgebung mit einem konsistenten Setup, das den Support erleichtert und Inkompatibilitätsprobleme vermeidet. Wenn diese Umgebungen häufig bereitgestellt werden müssen, insbesondere für kürzere Schulungen, bringt dies erhebliche Vorteile.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Bei Bedarf flexible Kapazität für umfangreiche Projekte
Für Data Science-Hackathons/-Wettbewerbe oder umfangreiche Datenmodelle und Auswertungen wird in der Regel für kurze Zeit eine hochskalierte Hardwarekapazität benötigt. DSVM kann dazu beitragen, die Data Science-Umgebung bei Bedarf schnell auf horizontal skalierten Servern zu replizieren, auf denen Experimente ausgeführt werden können, die leistungsstarke Computingressourcen erfordern.

### <a name="custom-compute-power-for-azure-notebooks"></a>Benutzerdefinierte Computeleistung für Azure Notebooks
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) ist ein kostenloser gehosteter Dienst zum Entwickeln, Ausführen und Freigeben von Jupyter Notebooks in der Cloud ohne Installation. Die kostenlose Dienstebene ist auf 4 GB Arbeitsspeicher und 1 GB Daten beschränkt. 

Um sämtliche Einschränkungen aufzuheben, können Sie an eine DSVM-Instanz oder an einen beliebigen anderen virtuellen Computer auf einem Jupyter-Server ein Notebooks-Projekt anfügen. Wenn Sie sich mit einem Konto über Azure Active Directory (z. B. einem Unternehmenskonto) bei Azure Notebooks anmelden, zeigt Notebooks automatisch DSVM-Instanzen in allen Abonnements an, die mit diesem Konto verknüpft sind. Sie können [eine DSVM-Instanz an Azure Notebooks anfügen](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier), um die verfügbare Computeleistung zu erhöhen.

### <a name="short-term-experimentation-and-evaluation"></a>Kurzfristige Experimente und Auswertungen
Mit DSVM können Sie neue Data Science-[Tools](./tools-included.md) testen oder kennenlernen, insbesondere indem Sie einige unserer veröffentlichten [Beispiele und exemplarischen Vorgehensweisen](./dsvm-samples-and-walkthroughs.md) durcharbeiten.


### <a name="deep-learning-with-gpus"></a>Deep Learning mit GPUs
In DSVM können Ihre Trainingsmodelle Deep Learning-Algorithmen auf Hardware verwenden, die auf Grafikprozessoren (Graphics Processing Units, GPUs) basiert. Dank der VM-Skalierungsfunktionen der Azure-Plattform unterstützt Sie DSVM bei der bedarfsgerechten Nutzung von GPU-basierter Hardware in der Cloud. Sie können zu einem GPU-basierten virtuellen Computer wechseln, wenn Sie große Modelle trainieren oder schnelle Berechnungen benötigen, und dabei den gleichen Betriebssystem-Datenträger beibehalten. Für DSVM können Sie beliebige GPU-fähige VM-SKUs der N-Serie auswählen. Beachten Sie, dass kostenlose Azure-Konten keine GPU-fähigen VM-SKUs unterstützen.

Die Windows Server 2016-Edition von DSVM enthält vorinstallierte GPU-Treiber und -Frameworks sowie GPU-Versionen der Deep Learning-Frameworks. Unter der Linux-Edition ist GPU-basiertes Deep Learning sowohl auf der DSVM-Instanz mit CentOS als auch auf der DSVM-Instanz mit Ubuntu aktiviert. 

Sie können die Ubuntu-, CentOS- oder Windows 2016-Edition von DSVM auch auf einem virtuellen Azure-Computer bereitstellen, der nicht auf GPUs basiert. In diesem Fall werden aber alle Deep Learning-Frameworks auf den CPU-Modus zurückgesetzt.
 
[Informieren Sie sich ausführlicher über die verfügbaren Deep Learning- und KI-Frameworks.](dsvm-deep-learning-ai-frameworks.md)

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>Was ist in DSVM enthalten?

Eine vollständige Liste der Tools für Windows- und Linux-DSVM-Instanzen finden Sie [hier](tools-included.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in diesen Artikeln:

+ Windows:
  + [Bereitstellen einer Data Science Virtual Machine für Windows in Azure](provision-vm.md)
  + [Zehn Dinge, die Sie mit der Windows Data Science Virtual Machine machen können](vm-do-ten-things.md)

+ Linux:
  + [Bereitstellen der Data Science Virtual Machine für Linux (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Bereitstellen einer Linux CentOS Data Science-VM in Azure](linux-dsvm-intro.md)
  + [Data Science mit einer Linux Data Science Virtual Machine in Azure](linux-dsvm-walkthrough.md)
