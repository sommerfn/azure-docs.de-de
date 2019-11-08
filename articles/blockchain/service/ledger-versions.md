---
title: Azure Blockchain-Ledgerversionen, Patches und Upgrades
description: Übersicht der unterstützten Ledgerversionen in Azure Blockchain, einschließlich Richtlinien bezüglich Systempatches und Upgrades, die vom System bzw. von Benutzern verwaltet werden.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 16d0f0876e7d75dfd0266468ddc3005b86fc632f
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73579799"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Von Azure Blockchain unterstützte Ledgerversionen

Azure Blockchain verwendet den auf Ethereum basierenden Ledger [Quorum](https://www.goquorum.com/developers), der dafür konzipiert ist, private Transaktionen innerhalb einer Gruppe bekannter Teilnehmer zu verarbeiten. Diese werden in Azure Blockchain als Konsortium bezeichnet.

Momentan unterstützt Azure Blockchain [Quorum Version 2.2.3](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.3) und den [Transaktions-Manager Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades

Für die Versionsverwaltung in Quorum werden eine Hauptversion, eine Nebenversion und Patchreleases verwendet. Wenn die Quorum-Version beispielsweise 2.0.1 lautet, wird der Releasetyp wie folgt kategorisiert:

|Hauptversion | Nebenversion  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service führt die Aktualisierung der Patchreleases von Quorum auf vorhandene ausgeführte Mitglieder innerhalb von 30 Tagen nach der Bereitstellung durch Quorum automatisch durch.

## <a name="availability-of-new-ledger-versions"></a>Verfügbarkeit neuer Ledgerversionen

Azure Blockchain Service macht die aktuellste Haupt- und Nebenversion des Quorum-Ledgers innerhalb von 60 Tagen nach dessen Veröffentlichung durch den Quorum-Hersteller verfügbar. Wenn Sie ein neues Mitglied und Konsortium bereitstellen, stehen maximal vier Nebenversionen für Konsortien zur Auswahl. Upgrades von einer Haupt- auf eine Nebenversion werden derzeit nicht unterstützt. Wenn Sie also beispielsweise die Version 2.x verwenden, wird derzeit kein Upgrade auf die Version 3.x unterstützt. Analog dazu wird bei Verwendung der Version 2.2 derzeit kein Upgrade auf die Version 2.3 unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Einschränkungen des Azure Blockchain-Diensts](limits.md)
