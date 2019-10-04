---
title: Beispiele für Richtlinienvorlagen | Microsoft-Dokumentation
description: Beispiele für Azure-Richtlinienvorlagen für virtuelle Netzwerke.
services: virtual-network
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 8228da778e6cf6a874868ef0206092913908352d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694287"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Beispiele für Azure-Richtlinienvorlagen für virtuelle Netzwerke

In der folgenden Tabelle sind Links zu [Azure Policy](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Beispielen enthalten. Diese Beispiele befinden sich im [Azure Policy-Beispielrepository](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Netzwerk**||
| [Netzwerksicherheitsgruppe X auf jeder Netzwerkschnittstelle](../governance/policy/samples/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jede virtuelle Netzwerkschnittstelle verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Netzwerksicherheitsgruppe X auf jedem Subnetz](../governance/policy/samples/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jedes virtuelle Subnetz verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Keine Routingtabelle](../governance/policy/samples/no-user-defined-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Verhindert, dass virtuelle Netzwerke mit einer Routingtabelle bereitgestellt werden. |
| [Verwenden eines zulässigen Subnetzes für VM-Netzwerkschnittstellen](../governance/policy/samples/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass Netzwerkschnittstellen ein zulässiges Subnetz verwenden. Sie geben die ID des zulässigen Subnetzes an. |
| [Verwenden eines zulässigen virtuellen Netzwerks für VM-Netzwerkschnittstellen](../governance/policy/samples/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass Netzwerkschnittstellen ein zulässiges virtuelles Netzwerk verwenden. Sie geben die ID des zulässigen virtuellen Netzwerks an. |
|**Überwachung**||
| [Überwachen der Diagnoseeinstellung](../governance/policy/samples/audit-diagnostic-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Überwacht, wenn Diagnoseeinstellungen für angegebene Ressourcentypen nicht aktiviert sind Sie geben ein Array von Ressourcentypen an, um zu überprüfen, ob die Diagnoseeinstellungen aktiviert sind. |
|**Namens- und Textkonventionen**||
| [Zulassen mehrerer Namensmuster](../governance/policy/samples/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Lassen Sie die Verwendung von einem der vielen Namensmuster für Ressourcen zu. |
| [Ähnliches Muster erfordern](../governance/policy/samples/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass Ressourcennamen eine *ähnliche* Bedingung für ein Muster erfüllen. |
| [Übereinstimmungsmuster erfordern](../governance/policy/samples/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass Ressourcennamen einem angegebenen Benennungsmuster entsprechen. |
| [Tagübereinstimmungsmuster erfordern](../governance/policy/samples/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass ein Tagwert einem Textmuster entspricht. |
|**Tags**||
| [Richtlinieninitiative zur Abrechnung von Tags](../governance/policy/samples/billing-tags-policy-initiative.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt angegebene Tagwerte für die Kostenstelle und den Produktnamen. Verwendet integrierte Richtlinien, um erforderliche Tags anzuwenden und zu erzwingen. Sie geben die erforderlichen Werte für die Tags an.  |
| [Erzwingen eines Tags und dessen Werts für Ressourcengruppen](../governance/policy/samples/enforce-tag-on-resource-groups.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt einen Tag und einen Wert für eine Ressourcengruppe. Sie geben den erforderlichen Tagnamen und -wert an.  |
| [Erzwingen eines Tags und dessen zugehörigen Werts](../governance/policy/samples/enforce-tag-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Erfordert einen angegebenen Tagnamen und-wert. Sie geben den zu erzwingenden Tagnamen und -wert an.  |
| [Tag und Standardwert anwenden](../governance/policy/samples/apply-tag-default-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Fügt einen angegebenen Tagnamen und -wert an, wenn dieses Tag nicht bereitgestellt wird. Sie geben den anzuwendenden Tagnamen und -wert an.  |
|**Allgemein**||
| [Zulässige Speicherorte](../governance/policy/samples/allowed-locations.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Erfordert, dass alle Ressourcen für die zulässigen Speicherorte bereitgestellt werden. Sie geben ein Array von zulässigen Speicherorten an.  |
| [Zulässige Ressourcentypen](../governance/policy/samples/allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellt sicher, dass nur genehmigte Ressourcentypen bereitgestellt werden. Sie geben ein Array von Ressourcentypen an, die erlaubt sind.  |
| [Nicht zulässige Ressourcentypen](../governance/policy/samples/not-allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Untersagt die Bereitstellung der angegebenen Ressourcentypen. Sie geben ein Array der zu blockierenden Ressourcentypen an.  |