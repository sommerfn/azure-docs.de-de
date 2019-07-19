---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Azure Kubernetes-Ressource (AKS) erstellen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1d68c08f6dfca74c38973af1686d614f3f10cc28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455093"
---
## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>Erstellen einer Azure Kubernetes Service-Clusterressource (AKS)

1. Wechseln Sie zu [Erstellen](https://ms.portal.azure.com/#create/microsoft.aks) für Kubernetes-Dienste.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** die folgenden Informationen ein:

    |Einstellung|Wert|
    |--|--|
    |Abonnement|Wählen Sie ein passendes Abonnement aus|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|
    |Kubernetes-Clustername|Gewünschter Name (Kleinbuchstaben)|
    |Region|Wählen Sie einen nahe gelegenen Speicherort aus|
    |Kubernetes-Version|1.12.8 (Standard)|
    |DNS-Namenspräfix|Wird automatisch erstellt, kann jedoch optional außer Kraft gesetzt werden|
    |Knotengröße|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Knotenanzahl|Lassen Sie den Schieberegler auf dem Standardwert|

1. Lassen Sie auf der Registerkarte **Skalieren** die Standardwerte für *virtuelle Knoten* und *VM-Skalierungsgruppen* unverändert.
1. Lassen Sie auf der Registerkarte **Authentifizierung** die Standardwerte für *Dienstprinzipal* und *RBAC aktivieren* unverändert.
1. Geben Sie auf der Registerkarte **Netzwerk** die folgende Auswahl ein:

    |Einstellung|Wert|
    |--|--|
    |HTTP-Anwendungsrouting|Nein|
    |Netzwerkkonfiguration|Basic|

1. Stellen Sie auf der Registerkarte **Überwachung** sicher, dass *Containerüberwachung aktivieren* auf **Ja** gesetzt ist, und behalten Sie den *Log Analytics-Arbeitsbereich* als Standardwert bei.
1. Lassen Sie auf der Registerkarte **Tags** die Name/Wert-Paare vorerst leer.
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Wenn die Überprüfung erfolgreich war, klicken Sie auf **Erstellen**.

> [!NOTE]
> Wenn bei der Validierung ein Fehler auftritt, ist dies möglicherweise auf einen *Dienstprinzipal*-Fehler zurückzuführen. Navigieren Sie zurück zur Registerkarte *Authentifizierung* und dann zurück zu *Überprüfen und erstellen*, wo die Validierung ausgeführt werden sollte, und übergeben Sie die Werte.
