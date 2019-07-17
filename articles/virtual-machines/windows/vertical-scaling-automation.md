---
title: Verwenden von Azure Automation zum vertikalen Skalieren von virtuellen Windows-Computern | Microsoft-Dokumentation
description: So skalieren Sie einen virtuellen Windows-Computer als Reaktion auf die Überwachung von Warnungen mit Azure Automation vertikal
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 581471bb20b0774625dda8a5fa1fdd27d571a5b5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483881"
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Vertikales Skalieren von Windows-VMs mit Azure Automation

Als vertikale Skalierung wird der Vorgang bezeichnet, die Ressourcen eines Computers als Reaktion auf die Workload zu erhöhen oder zu verringern. In Azure kann dies erreicht werden, indem die Größe des virtuellen Computers geändert wird. Dies kann in folgenden Szenarien hilfreich sein:

* Wenn der virtuelle Computer nicht häufig verwendet wird, können Sie ihn auf eine kleinere Größe skalieren, um die monatlichen Kosten zu senken.
* Wenn auf dem virtuellen Computer eine Lastspitze auftritt, kann seine Größe geändert werden, um die Kapazität zu erhöhen.

Nachfolgend werden die Schritte aufgelistet, die hierzu erforderlich sind:

1. Einrichten von Azure Automation für den Zugriff auf Ihre virtuellen Computer
2. Importieren der Azure Automation-Runbooks für die vertikale Skalierung in Ihr Abonnement
3. Hinzufügen eines Webhooks zu Ihrem Runbook
4. Hinzufügen einer Warnung zu Ihrem virtuellen Computer

## <a name="scale-limitations"></a>Einschränkungen bei der Skalierung

Die Größen, auf die der virtuelle Computer skaliert werden kann, hängen einerseits von der Größe des ersten virtuellen Computers sowie von der Verfügbarkeit anderer Größen im Cluster ab, in dem der virtuelle Computer bereitgestellt wurde. In den veröffentlichten Automation-Runbooks, die in diesem Artikel verwendet werden, wird dies berücksichtigt und lediglich eine Skalierung innerhalb der nachstehenden VM-Größenpaare durchgeführt. Dies bedeutet, dass ein virtueller Standard_D1v2-Computer nicht plötzlich auf Standard_G5 hochskaliert oder auf Basic_A0 herunterskaliert werden kann. Ferner wird das Hoch- bzw. Herunterskalieren der Größe von eingeschränkten VMs nicht unterstützt. 

Sie können zwischen den folgenden Größenpaaren skalieren:

* [A-Serie](#a-series)
* [B-Serie](#b-series)
* [D-Serie](#d-series)
* [E-Serie](#e-series)
* [F-Serie](#f-series)
* [G-Serie](#g-series)
* [H-Serie](#h-series)
* [L-Serie](#l-series)
* [M-Serie](#m-series)
* [N-Serie](#n-series)

### <a name="a-series"></a>A-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Basic_A0 | Basic_A1 |
| Basic_A1 | Basic_A2 |
| Basic_A2 | Basic_A3 |
| Basic_A3 | Basic_A4 |
| Standard_A0 | Standard_A1 |
| Standard_A1 | Standard_A2 |
| Standard_A2 | Standard_A3 |
| Standard_A3 | Standard_A4 |
| Standard_A5 | Standard_A6 |
| Standard_A6 | Standard_A7 |
| Standard_A8 | Standard_A9 |
| Standard_A10 | Standard_A11 |
| Standard_A1_v2 | Standard_A2_v2 |
| Standard_A2_v2 | Standard_A4_v2 |
| Standard_A4_v2 | Standard_A8_v2 |
| Standard_A2m_v2 | Standard_A4m_v2 |
| Standard_A4m_v2 | Standard_A8m_v2 |

### <a name="b-series"></a>B-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_B1s | Standard_B2s |
| Standard_B1ms | Standard_B2ms |
| Standard_B2ms | Standard_B4ms |
| Standard_B4ms | Standard_B8ms |

### <a name="d-series"></a>D-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_D1 | Standard_D2 |
| Standard_D2 | Standard_D3 |
| Standard_D3 | Standard_D4 |
| Standard_D11 | Standard_D12 |
| Standard_D12 | Standard_D13 |
| Standard_D13 | Standard_D14 |
| Standard_DS1 | Standard_DS2 |
| Standard_DS2 | Standard_DS3 |
| Standard_DS3 | Standard_DS4 |
| Standard_DS11 | Standard_DS12 |
| Standard_DS12 | Standard_DS13 |
| Standard_DS13 | Standard_DS14 |
| Standard_D1_v2 | Standard_D2_v2 |
| Standard_D2_v2 | Standard_D3_v2 |
| Standard_D3_v2 | Standard_D4_v2 |
| Standard_D4_v2 | Standard_D5_v2 |
| Standard_D11_v2 | Standard_D12_v2 |
| Standard_D12_v2 | Standard_D13_v2 |
| Standard_D13_v2 | Standard_D14_v2 |
| Standard_DS1_v2 | Standard_DS2_v2 |
| Standard_DS2_v2 | Standard_DS3_v2 |
| Standard_DS3_v2 | Standard_DS4_v2 |
| Standard_DS4_v2 | Standard_DS5_v2 |
| Standard_DS11_v2 | Standard_DS12_v2 |
| Standard_DS12_v2 | Standard_DS13_v2 |
| Standard_DS13_v2 | Standard_DS14_v2 |
| Standard_D2_v3 | Standard_D4_v3 |
| Standard_D4_v3 | Standard_D8_v3 |
| Standard_D8_v3 | Standard_D16_v3 |
| Standard_D16_v3 | Standard_D32_v3 |
| Standard_D32_v3 | Standard_D64_v3 |
| Standard_D2s_v3 | Standard_D4s_v3 |
| Standard_D4s_v3 | Standard_D8s_v3 |
| Standard_D8s_v3 | Standard_D16s_v3 |
| Standard_D16s_v3 | Standard_D32s_v3 |
| Standard_D32s_v3 | Standard_D64s_v3 |
| Standard_DC2s | Standard_DC4s |

### <a name="e-series"></a>E-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_E2_v3 | Standard_E4_v3 |
| Standard_E4_v3 | Standard_E8_v3 |
| Standard_E8_v3 | Standard_E16_v3 |
| Standard_E16_v3 | Standard_E20_v3 |
| Standard_E20_v3 | Standard_E32_v3 |
| Standard_E32_v3 | Standard_E64_v3 |
| Standard_E2s_v3 | Standard_E4s_v3 |
| Standard_E4s_v3 | Standard_E8s_v3 |
| Standard_E8s_v3 | Standard_E16s_v3 |
| Standard_E16s_v3 | Standard_E20s_v3 |
| Standard_E20s_v3 | Standard_E32s_v3 |
| Standard_E32s_v3 | Standard_E64s_v3 |

### <a name="f-series"></a>F-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_F1 | Standard_F2 |
| Standard_F2 | Standard_F4 |
| Standard_F4 | Standard_F8 |
| Standard_F8 | Standard_F16 |
| Standard_F1s | Standard_F2s |
| Standard_F2s | Standard_F4s |
| Standard_F4s | Standard_F8s |
| Standard_F8s | Standard_F16s |
| Standard_F2s_v2 | Standard_F4s_v2 |
| Standard_F4s_v2 | Standard_F8s_v2 |
| Standard_F8s_v2 | Standard_F16s_v2 |
| Standard_F16s_v2 | Standard_F32s_v2 |
| Standard_F32s_v2 | Standard_F64s_v2 |
| Standard_F64s_v2 | Standard_F7s_v2 |

### <a name="g-series"></a>G-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_G1 | Standard_G2 |
| Standard_G2 | Standard_G3 |
| Standard_G3 | Standard_G4 |
| Standard_G4 | Standard_G5 |
| Standard_GS1 | Standard_GS2 |
| Standard_GS2 | Standard_GS3 |
| Standard_GS3 | Standard_GS4 |
| Standard_GS4 | Standard_GS5 |

### <a name="h-series"></a>H-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_H8 | Standard_H16 |
| Standard_H8m | Standard_H16m |

### <a name="l-series"></a>L-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_L4s | Standard_L8s |
| Standard_L8s | Standard_L16s |
| Standard_L16s | Standard_L32s |
| Standard_L8s_v2 | Standard_L16s_v2 |
| Standard_L16s_v2 | Standard_L32s_v2 |
| Standard_L32s_v2 | Standard_L64s_v2 |
| Standard_L64s_v2 | Standard_L80s_v2 |

### <a name="m-series"></a>M-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_M8ms | Standard_M16ms |
| Standard_M16ms | Standard_M32ms |
| Standard_M32ms | Standard_M64ms |
| Standard_M64ms | Standard_M128ms |
| Standard_M32ls | Standard_M64ls |
| Standard_M64s | Standard_M128s |
| Standard_M64 | Standard_M128 |
| Standard_M64m | Standard_M128m |

### <a name="n-series"></a>N-Serie

| Ursprüngliche Größe | Hochskalierte Größe | 
| --- | --- |
| Standard_NC6 | Standard_NC12 |
| Standard_NC12 | Standard_NC24 |
| Standard_NC6s_v2 | Standard_NC12s_v2 |
| Standard_NC12s_v2 | Standard_NC24s_v2 |
| Standard_NC6s_v3 | Standard_NC12s_v3 |
| Standard_NC12s_v3 | Standard_NC24s_v3 |
| Standard_ND6 | Standard_ND12 |
| Standard_ND12 | Standard_ND24 |
| Standard_NV6 | Standard_NV12 |
| Standard_NV12 | Standard_NV24 |
| Standard_NV6s_v2 | Standard_NV12s_v2 |
| Standard_NV12s_v2 | Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Einrichten von Azure Automation für den Zugriff auf Ihre virtuellen Computer
Als Erstes müssen Sie ein Azure Automation-Konto erstellen, in dem die Runbooks gehostet werden, mit denen ein virtueller Computer skaliert wird. Vor Kurzem wurde für den Automation-Dienst die Funktion des ausführenden Kontos eingeführt, wodurch die Einrichtung des Dienstprinzipals für die automatische Ausführung der Runbooks im Auftrag des Benutzers stark vereinfacht wird. Weitere Informationen darüber finden Sie im unten stehenden Artikel:

* [Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importieren der Azure Automation-Runbooks für die vertikale Skalierung in Ihr Abonnement
Die Runbooks, die zur vertikalen Skalierung Ihres virtuellen Computers benötigt werden, sind bereits im Azure Automation-Runbookkatalog veröffentlicht. Sie müssen diese in Ihr Abonnement importieren. Informationen dazu, wie Sie Runbooks importieren, finden Sie im folgenden Artikel:

* [Runbook und Modulkataloge für Azure Automation](../../automation/automation-runbook-gallery.md)

Die nachfolgende Abbildung zeigt, welche Runbooks importiert werden müssen:

![Importieren von Runbooks](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Hinzufügen eines Webhooks zu Ihrem Runbook
Nachdem Sie die Runbooks importiert haben, müssen Sie einen Webhook zum Runbook hinzufügen, damit es über eine Warnung von einem virtuellen Computer ausgelöst werden kann. Ausführliche Informationen zum Erstellen eines Webhooks für Ihr Runbook finden Sie hier:

* [Azure Automation-Webhooks](../../automation/automation-webhooks.md)

Stellen Sie sicher, dass Sie den Webhook kopieren, bevor Sie das Dialogfeld für Webhooks schließen, da sie den Webhook im nächsten Abschnitt benötigen.

## <a name="add-an-alert-to-your-virtual-machine"></a>Hinzufügen einer Warnung zu Ihrem virtuellen Computer
1. Wählen Sie die VM-Einstellungen aus.
2. Wählen Sie „Warnungsregeln“ aus.
3. Wählen Sie „Warnung hinzufügen“ aus.
4. Wählen Sie die Metrik aus, bei der die Warnung ausgelöst wird.
5. Wählen Sie die Bedingung aus, bei deren Erfüllung die Warnung ausgelöst wird.
6. Wählen Sie einen Schwellenwert aus, bei dem die Bedingung aus Schritt 5 erfüllt sein soll.
7. Wählen Sie den Zeitraum aus, in dem der Überwachungsdienst auf die Bedingung und die Schwellenwerte aus den Schritten 5 und 6 prüft.
8. Fügen Sie den Webhook ein, den Sie im vorherigen Abschnitt kopiert haben.

![Hinzufügen einer Warnung zu VM 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Hinzufügen einer Warnung zu VM 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

