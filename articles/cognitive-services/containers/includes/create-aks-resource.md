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
ms.openlocfilehash: ab7ce8b4a538e6a286a00285069a22878c5d88d9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877443"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Erstellen einer Azure Kubernetes Service-Clusterressource

1. Rufen Sie [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks) auf, und klicken Sie auf **Erstellen**.

1. Geben Sie folgende Informationen auf der Registerkarte **Grundeinstellungen** an:

    |Einstellung|Wert|
    |--|--|
    |Subscription|Wählen Sie ein entsprechendes Abonnement aus.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus|
    |Kubernetes-Clustername|Geben Sie einen Namen ein (Kleinbuchstaben)|
    |Region|Wählen Sie einen nahe gelegenen Speicherort aus|
    |Kubernetes-Version|1.12.8 (Standard)|
    |DNS-Namenspräfix|Wird automatisch erstellt, kann jedoch überschrieben werden|
    |Knotengröße|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Knotenanzahl|Lassen Sie den Schieberegler auf dem Standardwert|

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
> Wenn bei der Validierung ein Fehler auftritt, ist dies möglicherweise auf einen Fehler beim „Dienstprinzipal“ zurückzuführen. Navigieren Sie zurück zur Registerkarte **Authentifizierung** und dann zurück zu **Überprüfen und erstellen**, wo die Validierung ausgeführt und bestanden werden sollte.
