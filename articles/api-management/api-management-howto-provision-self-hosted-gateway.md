---
title: Bereitstellen eines selbstgehosteten Gateways in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein selbstgehostetes Gateway in Azure API Management bereitstellen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: ee9594e7aa3b55f7eb6fc7ba41783708555bd15e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510571"
---
# <a name="provision-a-self-hosted-gateway-in-azure-api-management"></a>Bereitstellen eines selbstgehosteten Gateways in Azure API Management

Das Bereitstellen einer Gatewayressource in Ihrer Azure API Management-Instanz ist eine Voraussetzung für die Bereitstellung eines selbstgehosteten Gateways. Dieser Artikel führt Sie schrittweise durch die Bereitstellung einer Gatewayressource in API Management.

> [!NOTE]
> Das Feature „selbstgehostetes Gateway“ ist als Vorschauversion verfügbar. Während der Vorschauphase ist das selbstgehostete Gateway nur in den Tarifen „Developer“ und „Premium“ ohne zusätzliche Kosten verfügbar. Der Tarif „Developer“ ist auf eine Bereitstellung mit einem einzelnen selbstgehosteten Gateway beschränkt.

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="provision-a-self-hosted-gateway"></a>Bereitstellen eines selbstgehosteten Gateways

1. Wählen Sie **Gateways** unter **Einstellungen** aus.
2. Klicken Sie auf **+ Hinzufügen**.
3. Geben Sie den **Namen** und die **Region** des Gateways ein.
> [!TIP]
> **Region** gibt den vorgesehenen Speicherort der Gatewayknoten an, die dieser Gatewayressource zugeordnet werden. Der Wert ist semantisch äquivalent mit einer ähnlichen Eigenschaft, die jeder Azure-Ressource zugeordnet wird, ihm kann aber ein beliebiger Zeichenfolgenwert zugewiesen werden.
4. Geben Sie optional eine **Beschreibung** der Gatewayressource ein.
5. Wählen Sie optional **+** unter **APIs** aus, um dieser Gatewayressource mindestens eine API zuzuordnen.
> [!TIP]
> Sie können eine API einem Gateway auf der Registerkarte **Einstellungen** der API zuordnen bzw. aus ihr entfernen.

> [!IMPORTANT]
> Standardmäßig wird keine der vorhandenen APIs der neuen Gatewayressource zugeordnet. Daher führt der Versuch, sie über das neue Gateway aufzurufen, zu Antworten des Typs `404 Resource Not Found`.
6. Klicken Sie auf **Hinzufügen**.

Die Gatewayressource wurde nun in Ihrer API Management-Instanz bereitgestellt. Sie können mit der Bereitstellung des Gateways fortfahren.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum selbstgehosteten Gateway finden Sie unter [Azure API Management: Übersicht über das selbstgehostete Gateway](self-hosted-gateway-overview.md).
* Weitere Informationen zum [Bereitstellen eines selbstgehosteten Gateways für Kubernetes](api-management-howto-deploy-self-hosted-gateway-to-k8s.md)
* Weitere Informationen zum [Bereitstellen eines selbstgehosteten Gateways für Docker](api-management-howto-deploy-self-hosted-gateway-to-docker.md)
