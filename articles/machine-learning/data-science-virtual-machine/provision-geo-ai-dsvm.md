---
title: 'Schnellstart: Erstellen einer DSVM-Instanz für geografische KI'
description: Konfigurieren und erstellen Sie für räumliche Analysen und maschinelles Lernen eine DSVM-Instanz für geografische KI.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: 6b32d1f76091d7bbb8f870402020d0ac247d425b
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71170724"
---
# <a name="quickstart-set-up-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Schnellstart: Einrichten eines virtuellen Computers für geografische KI in Azure 

Geo AI Data Science Virtual Machine (Geo-DSVM) ist eine Erweiterung des beliebten Konzepts von [Azure Data Science Virtual Machine](https://aka.ms/dsvm), die speziell zur Kombination von KI und geografischen Analysen konfiguriert ist. Die geografischen Analysen auf dem virtuellen Computer werden durch [ArcGIS Pro](https://www.arcgis.com/features/index.html) ermöglicht. Mit Data Science Virtual Machine (DSVM) können Machine Learning- und sogar Deep Learning-Modelle schnell trainiert werden. Für die Entwicklung dieser Modelle werden durch geografische Informationen angereicherte Daten verwendet. Geo-DSVM wird nur unter Windows 2016 DSVM unterstützt. 

Geo-DSVM bietet folgende KI-Tools:

- GPU-Editionen beliebter Deep Learning-Frameworks wie Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2 und Chainer
- Tools zum Abrufen und Vorverarbeiten von Bild- und Textdaten
- Tools für Entwicklungsaktivitäten wie Microsoft Machine Learning Server Developer Edition, Anaconda Python, Jupyter Notebooks für Python und R, IDEs für Python und R sowie SQL-Datenbanken
- ArcGIS Pro-Desktopsoftware von ESRI in Verbindung mit Python- und R-Schnittstellen, die die räumlichen Daten aus Ihren KI-Anwendungen verarbeiten können
 

## <a name="create-your-geo-ai-data-science-vm"></a>Erstellen einer Data Science-VM für geografische KI

Gehen Sie wie folgt vor, um eine Data Science VM-Instanz für geografische KI zu erstellen:

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows)zur Auflistung der virtuellen Computer.
1. Wählen Sie am unteren Rand die Option **Erstellen** aus, um einen Assistenten zu generieren:

   ![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)

1. In jedem der vier Assistentenschritte sind Eingaben erforderlich. Ausführliche Informationen zu diesen Eingaben finden Sie im nächsten Abschnitt.

### <a name="wizard-details"></a>Details zum Assistenten ###

**Grundlagen**:

- **Name**: Der Name des Data Science-Servers, den Sie erstellen.
    
- **Benutzername**: Die Anmelde-ID des Administratorkontos.
    
- **Kennwort**: Das Kennwort des Administratorkontos.
    
- **Abonnement**: Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird.
    
- **Ressourcengruppe**: Sie können eine neue Ressourcengruppe erstellen oder eine **leere** in Ihrem Abonnement vorhandene Azure-Ressourcengruppe verwenden.
    
- **Standort**: Wählen Sie das Datencenter aus, das am besten geeignet ist. In der Regel handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist, oder um das Rechenzentrum, das Ihrem physischen Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen. Wenn Sie Deep Learning-Vorgänge mit GPUs durchführen möchten, müssen Sie eine der Azure-Regionen mit GPU-VM-Instanzen der NC-Serie auswählen. Derzeit sind das folgende Standorte: **USA, Osten, USA, Norden-Mitte, USA, Süden-Mitte, USA, Westen 2, Europa, Norden, Europa, Westen**. Die aktuelle Liste finden Sie auf der Seite [Azure-Produkte nach Region](https://azure.microsoft.com/regions/services/). Suchen Sie hier unter **Compute** nach **NC-Serie**. 
    
    
**Einstellungen**: Wenn Sie vorhaben, Deep Learning auf GPUs Ihrer Geo-DSVM-Instanz auszuführen, wählen Sie eine Größe für GPU-VMs der NC-Serie aus. Andernfalls können Sie eine der CPU-basierten Instanzen auswählen. Erstellen Sie ein Speicherkonto für Ihre VM. 
       
**Zusammenfassung**: Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind.
    
**Kaufen**: Klicken Sie auf **Kaufen**, um den Bereitstellungsprozess zu starten. Es wird ein Link zu den allgemeinen Geschäftsbedingungen bereitgestellt. Für die VM fallen abgesehen von den Computekosten für die Servergröße, die Sie im Schritt **Größe** ausgewählt haben, keine weiteren Kosten an. 
 
 >[!NOTE]
 > Die Bereitstellung dauert etwa 20 bis 30 Minuten. Der Status der Bereitstellung wird im Azure-Portal angezeigt.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Zugreifen auf die Data Science-VM für geografische KI

 Nachdem Ihre VM erstellt wurde, können Sie die darauf installierten und vorkonfigurierten Tools verwenden. Für viele der Tools stehen Desktopsymbole und Kacheln im Startmenü zur Verfügung. Sie können per Remotedesktop unter Verwendung der Anmeldeinformationen des Administratorkontos, das Sie im Abschnitt **Grundlagen** konfiguriert haben, auf die VM zugreifen.

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>Verwenden der auf dem virtuellen Computer installierten Software ArcGIS Pro

In Geo-DSVM ist der ArcGIS Pro-Desktop bereits vorinstalliert, und die Umgebung ist für die Verwendung aller Tools in DSVM vorkonfiguriert. Wenn Sie ArcGIS starten, werden Sie zur Eingabe von Anmeldeinformationen für Ihr ArcGIS-Konto aufgefordert. Wenn Sie bereits über ein ArcGIS-Konto verfügen und Lizenzen für die Software besitzen, können Sie Ihre vorhandenen Anmeldeinformationen verwenden.  

![ArcGIS-Anmeldung](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Andernfalls können Sie sich für ein neues ArcGIS-Konto und eine Lizenz registrieren oder eine [kostenlose Testversion](https://www.arcgis.com/features/free-trial.html) erhalten. 

![Kostenlose Testversion von ArcGIS](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Nachdem Sie sich für ein ArcGIS-Standardkonto oder für eine kostenlose Testversion registriert haben, können Sie ArcGIS Pro für Ihr Konto autorisieren. Eine entsprechende Anleitung finden Sie unter [Getting Started with ArcGIS Pro](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf) (Erste Schritte mit ArcGIS Pro).

Nachdem Sie sich mit Ihrem ArcGIS-Konto beim ArcGIS Pro-Desktop angemeldet haben, können Sie die auf der VM installierten und konfigurierten Data Science-Tools für Ihre geografischen Analysen sowie für Ihre Projekte für maschinelles Lernen verwenden.

## <a name="next-steps"></a>Nächste Schritte

In der folgenden Ressource finden Sie Informationen zu den ersten Schritten mit einer Data Science VM-Instanz für geografische KI:

* [Verwenden der Data Science-VM für geografische KI](use-geo-ai-dsvm.md)
