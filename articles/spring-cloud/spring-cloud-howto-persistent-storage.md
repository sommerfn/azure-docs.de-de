---
title: Verwenden von beständigem Speicher in Azure Spring Cloud | Microsoft-Dokumentation
description: Verwenden von beständigem Speicher in Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: jeconnoc
ms.openlocfilehash: 8c57698471d1363438c10e5806f9ed6f1da5333f
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038449"
---
# <a name="how-to-use-persistent-storage-in-azure-spring-cloud"></a>Verwenden von beständigem Speicher in Azure Spring Cloud

Azure Spring Cloud bietet zwei Arten von Speicher für Ihre Anwendung: beständigen und temporären Speicher.  Azure Spring Cloud aktiviert standardmäßig temporären Speicher für jede Anwendungsinstanz. Der temporäre Speicher ist auf 5 GB beschränkt, und der standardmäßige Einbindungspfad lautet `/tmp`.

> [!WARNING]
> Durch das Neustarten einer Anwendungsinstanz wird der zugehörige temporäre Speicher dauerhaft gelöscht.

Der beständige Speicher ist ein von Azure verwalteter Dateifreigabecontainer, der pro Anwendung zugeordnet wird. Im beständigen Speicher gespeicherte Daten werden für alle Instanzen der Anwendung freigegeben. Eine Azure Spring Cloud-Dienstinstanz kann maximal zehn Anwendungen mit aktiviertem beständigen Speicher enthalten, und jeder Anwendung werden 50 GB an beständigem Speicher zugeordnet. Der Standardeinbindungspfad für beständigen Speicher lautet `/persistent`.

## <a name="enable-persistent-storage-using-the-azure-portal"></a>Aktivieren des beständigen Speichers mithilfe des Azure-Portals

1. Navigieren Sie zur Seite Ihres Azure Spring Cloud-Diensts, und wählen Sie **Anwendungsdashboard** und anschließend die Anwendung aus, für die beständiger Speicher erforderlich ist.
1. Suchen Sie auf der Registerkarte **Übersicht** das Attribut **Persistent Storage** (Beständiger Speicher), und wählen Sie **Deaktiviert** aus.
1. Klicken Sie auf **Aktivieren**, um beständigen Speicher zu aktivieren, und wählen Sie die Schaltfläche **OK** aus, um die Änderung zu übernehmen.

Wurde der beständige Speicher aktiviert, werden seine Größe und sein Pfad auf der Seite **Übersicht** im Attribut **Persistent Storage** (Beständiger Speicher) angezeigt.

> [!WARNING]
> Durch die *Deaktivierung* des beständigen Speichers wird die Zuordnung des Speichers für diese Anwendung aufgehoben.  Alle Daten in diesem Speicherkonto gehen verloren. 

## <a name="enable-persistent-storage-using-the-azure-cli"></a>Aktivieren des beständigen Speichers mithilfe der Azure CLI

Erstellen Sie eine App mit aktiviertem persistenten Datenträger:
 
```azurecli
az spring-cloud app create -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Aktivieren Sie beständigen Speicher in einer vorhandenen App:

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Deaktivieren Sie beständigen Speicher in einer vorhandenen App:

> [!WARNING]
> Durch die Deaktivierung des beständigen Speichers wird die Zuordnung des Speichers für diese Anwendung aufgehoben, sodass alle dort gespeicherten Daten dauerhaft verloren gehen. 

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage false
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Anwendungs- und Dienstkontingente](spring-cloud-quotas.md) oder das [manuelle Skalieren Ihrer Anwendung](spring-cloud-tutorial-scale-manual.md).