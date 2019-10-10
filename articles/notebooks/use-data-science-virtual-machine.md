---
title: Verwenden von Azure Data Science VMs
description: Stellen Sie eine Verbindung zu einer Azure Data Science Virtual Machine (DSVM) her, um die für Azure Notebooks verfügbare Computeleistung auszuweiten.
services: app-service
documentationcenter: ''
author: getroyer
manager: andneil
ms.assetid: 0ccc2529-e17f-4221-b7c7-9496d6a731cc
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/13/2019
ms.author: getroyer
ms.openlocfilehash: 0f06475708adaf1324eedbd8e315fe4d731339be
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71970102"
---
# <a name="use-azure-data-science-virtual-machines"></a>Verwenden von Azure Data Science VMs

Standardmäßig werden Projekte auf der Ebene **Free Compute** ausgeführt, die auf 4 GB Speicher und 1 GB Daten beschränkt ist, um Missbrauch zu verhindern. Sie können diese Einschränkungen umgehen, indem Sie eine anderen virtuellen Computer verwenden, den Sie in einem Azure-Abonnement bereitgestellt haben. Aus diesem Grund ist die beste Wahl eine Azure Data Science Virtual Machine (DSVM), die das Image **Data Science Virtual Machine for Linux (Ubuntu)** verwendet. So eine DSVM ist bereits vorkonfiguriert mit allem, was Sie für Azure Notebooks benötigen, und wird automatisch in der Dropdownliste **Ausführen** in Azure Notebooks angezeigt.

> [!Note]
> Azure Notebooks wird nur auf DSVMs unterstützt, die mit dem Ubuntu Linux-Image erstellt wurden. Es besteht keine Unterstützung unter Windows 2012-, Windows 2016- oder Linux CentOS-Images.

## <a name="create-a-dsvm-instance"></a>Erstellen einer DSVM-Instanz

Um eine neue DSVM-Instanz zu erstellen, befolgen Sie die Anweisungen in [Erstellen einer Ubuntu Data Science VM](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Weitere Informationen, einschließlich der Preisinformationen, finden Sie unter [Virtuelle Data Science-Computer](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Herstellen einer Verbindung mit der DSVM

Sobald Sie die DSVM erstellt haben, wählen Sie im Projektdashboard von Azure Notebooks in der Dropdownliste **Ausführen** die entsprechende DSVM-Instanz aus. In der Dropdownliste werden DSVM-Instanzen angezeigt, wenn die folgenden Bedingungen erfüllt sind:

- Sie sind bei Azure Notebooks mit einem Konto angemeldet, das Azure Active Directory (AAD) verwendet, wie beispielsweise ein Firmenkonto.
- Ihr Konto ist mit einem Azure-Abonnement verbunden.
- Sie verfügen in diesem Abonnement über mindestens eine VM, für die zumindest Lesezugriff besteht, die das Image „Data Science Virtual Machine for Linux (Ubuntu)“ verwendet.

![Data Science Virtual Machine-Instanzen in der Dropdownliste im Projektdashboard](media/project-compute-tier-dsvm.png)

Wenn Sie eine DSVM-Instanz auswählen, werden Sie möglicherweise von Azure Notebooks zur Eingabe der spezifischen Computer-Anmeldeinformationen aufgefordert, die beim Erstellen der VM verwendet wurden.

Wenn eine der Bedingungen nicht zutrifft, können Sie dennoch eine Verbindung zur DSVM herstellen. Wählen Sie in der Dropdownliste die Option **Direct Compute** aus. Sie werden nach einem Namen (für die Anzeige in der Liste), der IP-Adresse der VM mit dem zugehörigen Port (in der Regel 8000, der Standardport, an dem JupyterHub lauscht) und den VM-Anmeldeinformationen gefragt:

![Aufforderung zum Sammeln von Serverinformationen für die Option „Direktes Computing“](media/project-compute-tier-direct.png)

Diese Werte erhalten Sie im Azure-Portal auf der DSVM-Seite.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>Zugriff auf Azure Notebooks-Dateien über die DSVM

Dateisystemzugriff wird für DSVM-Versionen ab 19.06.15 unterstützt. Um die Version zu überprüfen, stellen Sie zuerst über SSH eine Verbindung mit Ihrer DSVM her und führen dann den folgenden Befehl aus: `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"` (Sie müssen die hier angegebene IP-Adresse verwenden). Die Versionsnummer wird in der Ausgabe für „Version“ angezeigt.

Damit die Parität der Dateipfade mit der **Free Compute**-Ebene gewährleistet ist, können Sie auf einer DSVM nur jeweils ein Projekt auf einmal öffnen. Sie müssen das erste Projekt zuerst herunterfahren, bevor Sie ein neues öffnen können.

Wenn ein Projekt auf einer VM ausgeführt wird, werden die Dateien in das Stammverzeichnis des Jupyter-Servers aufgenommen (das Verzeichnis, das in JupyterHub angezeigt wird), wodurch die Standarddateien in Azure Notebooks ersetzt werden. Wenn Sie die VM mithilfe der **Herunterfahren**-Schaltfläche auf der Benutzeroberfläche des Notebooks herunterfahren, stellt Azure Notebooks die Standarddateien wieder her.

![Schaltfläche zum Herunterfahren in Azure Notebooks](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Erstellen neuer DSVM-Benutzer

Wenn mehrere Benutzer eine DSVM gemeinsam nutzen, können Sie vermeiden, dass sie sich gegenseitig blockieren, indem Sie für jeden Notebook-Benutzer einen DSVM-Benutzer erstellen und verwenden:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem virtuellen Computer.
1. Klicken Sie am linken Rand unter **Support und Problembehandlung** auf **Kennwort zurücksetzen**.
1. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie auf **Aktualisieren**. (Vorhandene Benutzernamen sind nicht betroffen.)
1. Wiederholen Sie den vorherigen Schritt für alle weiteren Benutzer.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DSVMs finden Sie unter [Einführung in Azure Data Science Virtual Machine für Linux und Windows](/azure/machine-learning/data-science-virtual-machine/overview).
