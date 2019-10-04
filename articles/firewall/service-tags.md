---
title: Übersicht über Azure Firewall-Diensttags
description: Dieser Artikel bietet eine Übersicht über die Azure Firewall-Diensttags.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 6/27/2019
ms.author: victorh
ms.openlocfilehash: d0ac36e415c056dffc9c75d00968ff74c2156e63
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450171"
---
# <a name="azure-firewall-service-tags"></a>Azure Firewall-Diensttags

Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern. Sie können kein eigenes Diensttag erstellen und auch nicht angeben, welche IP-Adressen in einem Tag enthalten sind. Microsoft verwaltet die Adresspräfixe, die mit dem Diensttag abgedeckt werden, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Azure Firewall-Diensttags können im Zielfeld für Netzwerkregeln verwendet werden. Sie können sie anstelle bestimmter IP-Adressen verwenden.

## <a name="supported-service-tags"></a>Unterstützte Diensttags

Eine Liste mit Diensttags, die für Netzwerkregeln der Azure-Firewall verfügbar sind, finden Sie unter [Diensttags](../virtual-network/security-overview.md#service-tags).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Firewall-Regeln finden Sie unter [Logik für die Azure Firewall-Regelverarbeitung](rule-processing.md).