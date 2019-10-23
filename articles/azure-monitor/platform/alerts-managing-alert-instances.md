---
title: Verwalten von Warnungsinstanzen in Azure Monitor
description: Verwalten von Warnungsinstanzen in Azure
ms.service: azure-monitor
ms.subservice: alerts
ms.topic: conceptual
author: anantr
ms.author: robb
ms.date: 09/24/2018
ms.openlocfilehash: 955ed75f8253798e6970c02b0abf5794298ae6da
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555638"
---
# <a name="manage-alert-instances-with-unified-alerts"></a>Verwalten von Warnungsinstanzen mit der einheitlichen Oberfläche für Warnungen
Mit der [einheitlichen Oberfläche für Warnungen](https://aka.ms/azure-alerts-overview) in Azure Monitor können Sie alle Ihre verschiedenen Warnungstypen in Azure anzeigen. Dies umfasst die Anzeige mehrerer Abonnements in einem einzelnen Bereich. In diesem Artikel wird gezeigt, wie Sie Ihre Warnungsinstanzen anzeigen und nach bestimmten Warnungsinstanzen zur Fehlerbehebung suchen können.

> [!NOTE]
   >  Sie können nur auf Warnungen zugreifen, die in den letzten 30 Tagen generiert wurden.

## <a name="go-to-the-alerts-page"></a>Navigieren zur Seite „Warnungen“

Sie können auf eine der folgenden Arten zur Seite „Warnungen“ navigieren:

   + Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Monitor** > **Warnungen** aus.  
     ![Screenshot der Seite „Monitor – Warnungen“](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + Verwenden Sie den Kontext einer bestimmten Ressource. Öffnen Sie eine Ressource, wechseln Sie zum Abschnitt **Überwachung**, und wählen Sie **Warnungen** aus. Die Startseite wird vorab nach Warnungen für diese bestimmte Ressource gefiltert.
   
     ![Screenshot von „Überwachung“ – „Warnungen“ für eine Ressource](media/alerts-managing-alert-instances/alert-resource.JPG)
    
   + Verwenden Sie den Kontext einer bestimmten Ressourcengruppe. Öffnen Sie eine Ressourcengruppe, wechseln Sie zum Abschnitt **Überwachung**, und wählen Sie **Warnungen** aus. Die Startseite wird vorab nach Warnungen für diese bestimmte Ressourcengruppe gefiltert.    
   
     ![Screenshot von „Überwachung“ – „Warnungen“ für eine Ressourcengruppe](media/alerts-managing-alert-instances/alert-rg.JPG)

## <a name="find-alert-instances"></a>Suchen von Warnungsinstanzen

Die Seite **Warnungszusammenfassung** gibt Ihnen einen Überblick über Ihre gesamten Warnungsinstanzen in Azure. Sie können die zusammenfassende Übersicht ändern, indem Sie **mehrere Abonnements** (maximal fünf) auswählen oder nach **Ressourcengruppen**, bestimmten **Ressourcen** oder **Zeitbereichen** filtern. Wählen Sie **Warnungen gesamt** oder einen der Schweregrade aus, um zur Listenansicht für Ihre Warnungen zu gelangen.     
   ![Screenshot der Seite „Warnungszusammenfassung“](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
Auf der Seite **Alle Warnungen** werden alle Warnungsinstanzen in Azure aufgelistet. Wenn Sie über eine Warnungsbenachrichtigung zum Portal gelangen, können Sie die verfügbaren Filter verwenden, um die Eingrenzung auf diese bestimmte Warnungsinstanz vorzunehmen.

> [!NOTE]
>  Wenn Sie durch Auswahl eines der Schweregrade auf die Seite gelangt sind, wird die Liste vorab nach diesem Schweregrad gefiltert.

Neben den auf der vorherigen Seite verfügbaren Filtern können Sie auch auf Basis des Überwachungsdiensts (z.B. Plattform für Metriken), der Überwachungsbedingung (ausgelöst oder gelöst), des Schweregrads, des Warnungszustands (neu/bestätigt/geschlossen) oder der ID der intelligenten Gruppe filtern.

   ![Screenshot der Seite „Alle Warnungen“](media/alerts-managing-alert-instances/all-alerts.jpg)

   > [!NOTE]
   >  Wenn Sie durch Auswahl eines der Schweregrade auf die Seite gelangt sind, wird die Liste vorab nach diesem Schweregrad gefiltert.
 
Wenn Sie eine beliebige Warnungsinstanz auswählen, wird die Seite **Warnungsdetails** geöffnet, auf der Sie weitere Informationen zu dieser bestimmten Warnungsinstanz finden.   
   ![Screenshot der Seite „Warnungsdetails“](media/alerts-managing-alert-instances/alert-details.jpg)  

