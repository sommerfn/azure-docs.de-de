---
title: Erstellen einer Azure Kubernetes Service-Clusterressource
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Azure Kubernetes Service-Ressource (AKS) erstellen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 5e442f38a8e81170eeeac2f31f00a7d0eeb08d2b
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377448"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Erstellen einer Azure Kubernetes Service-Clusterressource

1. Wechseln Sie zu [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks), und klicken Sie auf **Erstellen**.

1. Geben Sie folgende Informationen auf der Registerkarte **Grundeinstellungen** an:

    |Einstellung|Wert|
    |--|--|
    |Subscription|Wählen Sie ein entsprechendes Abonnement aus.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus.|
    |Kubernetes-Clustername|Geben Sie einen Namen ein (Kleinbuchstaben).|
    |Region|Wählen Sie einen nahe gelegenen Speicherort aus.|
    |Kubernetes-Version|1.12.8 (Standard).|
    |DNS-Namenspräfix|Wird automatisch erstellt, kann jedoch überschrieben werden.|
    |Knotengröße|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Knotenanzahl|Lassen Sie den Schieberegler auf dem Standardwert.|

1. Lassen Sie die Standardwerte auf der Registerkarte **Skalieren** für **virtuelle Knoten** und **VM-Skalierungsgruppen (Vorschau)** unverändert.
1. Lassen Sie die Standardwerte auf der Registerkarte **Authentifizierung** für **Dienstprinzipal** und **RBAC aktivieren** unverändert.
1. Geben Sie auf der Registerkarte **Netzwerk** die folgende Auswahl ein:

    |Einstellung|Wert|
    |--|--|
    |HTTP-Anwendungsrouting|Nein|
    |Netzwerkkonfiguration|Basic|

1. Stellen Sie auf der Registerkarte **Überwachung** sicher, dass **Containerüberwachung aktivieren** auf **Ja** festgelegt ist, und behalten Sie den Standardwert für **Log Analytics-Arbeitsbereich** bei.
1. Lassen Sie auf der Registerkarte **Tags** die Name/Wert-Paare vorerst leer.
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Wenn die Überprüfung erfolgreich war, wählen Sie **Erstellen** aus.

> [!NOTE]
> Wenn bei der Überprüfung ein Fehler auftritt, ist der Grund dafür möglicherweise ein Fehler beim „Dienstprinzipal“. Navigieren Sie zurück zur Registerkarte **Authentifizierung** und dann zurück zu **Überprüfen und erstellen**, wo die Überprüfung ausgeführt und bestanden werden sollte.
