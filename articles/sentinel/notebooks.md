---
title: Ermittlungsfunktionen mit Notebooks in Azure Sentinel | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Notebooks mit den Ermittlungsfunktionen von Azure Sentinel verwendet werden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: rkarlin
ms.openlocfilehash: ba22cc3db0ca50a292ddef4d0d646f8578c15cd4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489132"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Aufspüren von Sicherheitsrisiken mit Jupyter Notebooks

Grundlage für Azure Sentinel ist der Datenspeicher; dieser zeichnet sich durch Hochleistungsabfragen, ein dynamisches Schema und die Skalierung auf große Datenvolumen aus. Das Azure-Portal und alle Azure Sentinel-Tools greifen über eine gemeinsame API auf diesen Datenspeicher zu. Die gleiche API ist auch für externe Tools wie [Jupyter Notebooks](https://jupyter.org/) und Python verfügbar. Viele gängige Aufgaben können im Portal durchgeführt werden, Jupyter erweitert jedoch den Rahmen der Verarbeitung dieser Daten. Jupyter bietet sowohl umfassende Programmierbarkeit als auch eine große Sammlung von Bibliotheken für Machine Learning, Visualisierung und Datenanalyse. Dies macht Jupyter zu einem überzeugenden Tool für Sicherheitsuntersuchungen und Aufspüren von Sicherheitsrisiken.

![Beispielnotebook](./media/notebooks/sentinel-notebooks-map.png)

Wir haben die Jupyter-Benutzeroberfläche in das Azure-Portal integriert, sodass Sie Notebooks auf einfache Weise erstellen und mit diesen Notebooks Daten analysieren können. Mit der *Kqlmagic*-Bibliothek als verbindende Komponente können Sie Abfragen von Azure Sentinel annehmen und diese direkt in einem Notebook ausführen. Für Abfragen wird die Abfragesprache [Kusto](https://kusto.azurewebsites.net/docs/query/index.html) verwendet. Verschiedene von Microsoft-Sicherheitsanalysten entwickelte Notebooks werden mit Azure Sentinel verpackt. Einige diese Notebooks wurden auf ein bestimmtes Szenario ausgelegt, und sie sind ohne weitere Änderungen sofort verwendungsfähig. Andere sollen beispielhaft Techniken und Funktionen veranschaulichen, die Sie kopieren oder für die Verwendung in Ihren eigenen Notebooks anpassen können. Andere Notebooks wiederum können auch aus der Azure Sentinel-Community GitHub importiert werden.

Diese integrierte Jupyter-Benutzererfahrung verwendet [Azure Notebooks](https://notebooks.azure.com/) zum Speichern, Freigeben und Ausführen von Notebooks. Sie können diese Notebooks auch lokal (wenn Sie über eine Python-Umgebung verfügen und Jupyter auf Ihrem Computer installiert ist) oder in anderen JupyterHub-Umgebungen wie Azure Databricks ausführen.

Notebooks verfügen über zwei Komponenten:

- Die browserbasierte Benutzeroberfläche, in der Sie Abfragen und Code eingeben und ausführen und in der die Ergebnisse der Ausführung angezeigt werden.
- Einen *Kernel*, der für das Analysieren und Ausführen des Codes selbst zuständig ist. 

In Azure Notebooks wird dieser Kernel standardmäßig unter Azure *Free Cloud Compute and Storage* ausgeführt. Wenn Ihre Notebooks komplexe Machine Learning-Modelle oder Visualisierungen einschließen, empfiehlt es sich, leistungsfähigere, dedizierte Computeressourcen wie [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM) zu verwenden. Die Vertraulichkeit der Notebooks in Ihrem Konto bleibt gewahrt, sofern Sie sie nicht freigeben.

Die Azure Sentinel-Notebooks nutzen viele beliebte Python-Bibliotheken wie pandas, matplotlib, bokeh und andere. Zahlreiche weitere Python-Pakete stehen zur Auswahl, die u.a. die folgenden Bereiche abdecken:

- Visualisierungen und Grafiken
- Datenverarbeitung und -analyse
- Statistiken und numerisches Computing
- Machine Learning und Deep Learning

Im Paket [msticpy](https://github.com/Microsoft/msticpy/) haben wir zudem einige Open Source-Jupyter-Sicherheitstools veröffentlicht. Dieses Paket wird in vielen der enthaltenen Notebooks verwendet. Msticpy-Tools sollen Sie insbesondere beim Erstellen von Notebooks zum Aufspüren und Untersuchen von Sicherheitsrisiken unterstützen, und wir arbeiten aktiv an neuen Features und Verbesserungen.

Derzeit sind u. a. folgende Notebooks enthalten:

- **Guided investigation - Process Alerts** (Geführte Untersuchung – Warnungen verarbeiten): Hiermit können Sie Warnungen durch Analysieren der Aktivität auf dem oder den betroffenen Host(s) schnell selektieren.
- **Guided hunting - Windows host explorer** (Geführte Suche – Windows-Host-Explorer): Hiermit können Sie Kontoaktivitäten, Prozessausführungen, Netzwerkaktivitäten und andere Ereignisse auf einem Host untersuchen.
- **Guided hunting - Office365-Exploring** (Geführte Suche – Office365-Untersuchung): Hiermit können Sie nach verdächtigen Aktivitäten in Office 365 in mehreren Office 365-Datasets suchen.

Im [GitHub-Repository der Azure Sentinel-Community](https://github.com/Azure/Azure-Sentinel) finden sich alle künftig von Microsoft erstellten oder von der Community beigesteuerten Azure Sentinel-Notebooks.

Sie benötigen ein Azure Notebooks-Konto, um die Notebooks verwenden zu können. Weitere Informationen finden Sie unter [Quickstart: Anmelden und Festlegen einer Benutzer-ID](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks) aus der Azure Notebooks-Dokumentation. Um dieses Konto zu erstellen, können Sie die Option **Für Azure Notebooks registrieren** in der Befehlsleiste von **Azure Sentinel – Notebooks** verwenden:

> [!div class="mx-imgBorder"]
>![Option „Für Azure Notebooks registrieren“](./media/notebooks/sentinel-azure-sign-up-azure-notebooks.png)

## <a name="view-available-notebooks-from-azure-sentinel"></a>Anzeigen der verfügbaren Notebooks von Azure Sentinel
 
1. Navigieren Sie vom Azure-Portal aus zu **Azure Sentinel** > **Bedrohungsverwaltung** > **Notebooks**, wo die von Azure Sentinel bereitgestellten Notebooks angezeigt werden. 

2. Wählen Sie einzelne Notebooks aus, um deren Beschreibungen, erforderliche Datentypen und Datenquellen zu lesen. Beispiel:
    
    > [!div class="mx-imgBorder"]
    > ![Notebook starten](./media/notebooks/sentinel-azure-notebooks-nolaunch.png)

3. Wählen Sie **Notebook starten** aus, um die Notebooks im [Azure Sentinel Community GitHub-Repository](https://github.com/Azure/Azure-Sentinel) zu durchsuchen.

Derzeit ist es nicht möglich, ein Notebook direkt von Azure Sentinel aus zu starten. Gehen Sie stattdessen wie folgt vor, um die Notebooks auf GitHub in ein Azure Notebooks-Projekt zu klonen.

## <a name="clone-azure-sentinel-notebooks-to-a-new-azure-notebooks-project"></a>Klonen von Azure Sentinel-Notebooks in ein neues Azure Notebooks-Projekt

Diese Vorgehensweise erstellt für Sie ein Azure Notebooks-Projekt, das die Azure Sentinel-Notebooks enthält. Sie können die Notebooks dann unverändert ausführen oder Änderungen an ihnen vornehmen und sie dann ausführen.

1. Navigieren Sie vom Azure-Portal aus zu **Azure Sentinel** > **Bedrohungsverwaltung** > **Notebooks**, und wählen Sie dann **Notebooks klonen** über die Befehlsleiste aus:
  
    > [!div class="mx-imgBorder"]
    >![Option „Notebooks klonen“](./media/notebooks/sentinel-azure-clone-notebooks.png)

2. Wenn das folgende Dialogfeld angezeigt wird, wählen Sie **Importieren** aus, um das GitHub-Repository in Ihr Azure Notebook-Projekt zu klonen. Wenn Sie über kein vorhandenes Azure Notebooks-Konto verfügen, werden Sie aufgefordert, eines zu erstellen und sich anzumelden.

   ![Notebook importieren](./media/notebooks/sentinel-notebooks-clone.png)

3. Aktivieren Sie im Dialogfeld **GitHub-Repository hochladen** nicht **Rekursiv klonen** aus, da sich diese Option auf verknüpfte GitHub-Repositorys bezieht. Verwenden Sie für den Projektnamen den Standardnamen, oder geben Sie einen neuen Namen ein. Klicken Sie dann auf **Importieren**, um den GitHub-Inhalt zu klonen. Dieser Vorgang kann einige Minuten dauern.

   ![Notebook importieren](./media/notebooks/sentinel-create-project.png)

4. Öffnen Sie das soeben erstellte Projekt, und öffnen Sie dann den Ordner **Notebooks**, um die Notebooks anzuzeigen. Beispiel:

   ![Repository importieren](./media/notebooks/sentinel-open-notebook1.png)

Sie können die Notebooks dann von Azure Notebooks aus ausführen. Um von Azure Sentinel zu diesen Notebooks zurückzukehren, wählen Sie **Zu Ihren Notebooks wechseln** über die Befehlsleiste in **Azure Sentinel – Notebooks** aus:

> [!div class="mx-imgBorder"]
>![Option „Zu Ihren Notebooks wechseln“](./media/notebooks/sentinel-azure-to-go-notebooks.png)


## <a name="using-notebooks-to-hunt"></a>Verwenden von Notebooks für die Suche

In jedem Notebook werden Sie durch die einzelnen Schritte zum Durchführen einer Suche oder Untersuchung geführt. Bibliotheken und andere benötigte Abhängigkeiten des Notebooks können über das Notebook selbst oder über eine einfache Konfigurationsprozedur installiert werden. Die Konfiguration, die Ihr Notebook-Projekt mir Ihrem Azure Sentinel-Abonnement verknüpft, wurde automatisch in den vorherigen Schritten festgelegt.

1. Wenn Sie sich noch nicht in Azure Notebooks befinden, können Sie die Option **Zu Ihren Notebooks wechseln** über die Befehlsleiste in **Azure Sentinel – Notebooks** verwenden:
    
    > [!div class="mx-imgBorder"]
    >![Option „Zu Ihren Notebooks wechseln“](./media/notebooks/sentinel-azure-to-go-notebooks.png)
    
    Wählen Sie in Azure Notebooks **Meine Projekte**, dann das Projekt, das die Azure Sentinel-Notebooks enthält, und schließlich den Ordner **Notebooks** aus.
    
2. Bevor Sie ein Notebook öffnen, beachten Sie, dass standardmäßig „Free Compute“ für die Ausführung der Notebooks ausgewählt ist:
    
   ![Notebook auswählen](./media/notebooks/sentinel-open-notebook2.png)
    
    Wenn Sie eine DSVM (Data Science Virtual Machine) für die Verwendung konfiguriert haben, wie in der Einführung beschrieben, wählen Sie die DSVM aus und authentifizieren Sie sich, bevor Sie das erste Notebook öffnen. 

3. Wählen Sie ein Notebook aus, um es zu öffnen.
    
    Wenn Sie ein Notebook zum ersten Mal öffnen, werden Sie möglicherweise aufgefordert, eine Kernelversion auszuwählen. Wenn Sie nicht aufgefordert werden, können Sie die Kernelversion über **Kernel** >  **Kernel ändern** auswählen. Anschließend können Sie eine Version auswählen (mindestens 3.6). Die ausgewählte Kernelversion wird oben rechts im Notebook-Fenster angezeigt:
    
   ![Notebook auswählen](./media/notebooks/sentinel-select-kernel.png)

4. Bevor Sie Änderungen an einem heruntergeladenen Notebook vornehmen, ist es ratsam, eine Kopie des ursprünglichen Notebooks zu erstellen und mit der Kopie zu arbeiten. Wählen Sie dazu **Datei** > **Kopie erstellen** aus. Durch das Arbeiten mit Kopien können Sie problemlos auf künftige Versionen von Notebooks aktualisieren, ohne dass Ihre Daten überschrieben werden.
    
    Sie sind jetzt bereit, das ausgewählte Notebook auszuführen oder zu bearbeiten.

Empfehlungen:

- Eine kurze Einführung zum Abfragen von Daten in Azure Sentinel, finden Sie im Notebook [GetStarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) im Hauptordner **Notebooks**. 

- Weitere Beispiel-Notebooks finden sich im Unterordner **Sample-Notebooks**. Diese Beispiel-Notebooks wurden mit Daten gespeichert, sodass die gewünschte Ausgabe leichter überprüft werden kann. Wir empfehlen für die Anzeige dieser Notebooks den [nbviewer](https://nbviewer.jupyter.org/). 

- Der Ordner **HowTos** enthält Notebooks, die beispielsweise Folgendes beschreiben: Festlegen der Python-Standardversion, Konfigurieren einer DSVM, Erstellen von Azure Sentinel-Lesezeichen in einem Notebook sowie andere Themen.

Die bereitgestellten Notebooks fungieren einerseits als hilfreiche Tools, Sie dienen andererseits aber auch zur Veranschaulichung mit Codebeispielen, die Sie in der Umgebung Ihrer eigenen Notebooks verwenden können.

Wir freuen uns über Feedback, egal ob es sich um Vorschläge, Empfehlungen für Funktionen, eigene Beiträge von Notebooks, Fehlerberichte oder Verbesserungen und Ergänzungen zur vorhandenen Notebooks handelt. Besuchen Sie die [Azure Sentinel-Community GitHub](https://github.com/Azure/Azure-Sentinel), um ein Problem zu melden oder eine Verzweigung anzulegen oder um einen Beitrag hochzuladen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden die ersten Schritte beim Verwenden von Jupyter-Notebooks in Azure Sentinel erläutert. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- [Proactively hunt for threats (Proaktive Ermittlung von Bedrohungen)](hunting.md)
- [Use bookmarks to save interesting information while hunting (Verwenden von Lesezeichen zum Speichern interessanter Informationen bei der Ermittlung)](bookmarks.md)
