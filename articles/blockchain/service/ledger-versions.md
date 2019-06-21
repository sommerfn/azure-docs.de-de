---
title: Von Azure Blockchain unterstützte Ledgerversionen, Patchen und Upgraden
description: Übersicht der unterstützten Ledgerversionen in Azure Blockchain, einschließlich Richtlinien bezüglich Systempatches und Upgrades, die vom System bzw. von Benutzern verwaltet werden.
services: azure-blockchain
keywords: Blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 53f65ec91a1e0f1e5a6322f0125bf83cd3e400b2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399101"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Von Azure Blockchain unterstützte Ledgerversionen

Azure Blockchain verwendet den auf Ethereum basierenden Ledger [Quorum](https://www.goquorum.com/developers), der dafür konzipiert ist, private Transaktionen innerhalb einer Gruppe bekannter Teilnehmer zu verarbeiten. Diese werden in Azure Blockchain als Konsortium bezeichnet.

Momentan unterstützt Azure Blockchain [Quorum Version 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) und den [Transaktions-Manager Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades

Für die Versionsverwaltung in Quorum werden eine Hauptversion, eine Nebenversion und Patchreleases verwendet. Wenn die Quorum-Version beispielsweise 2.0.1 lautet, wird der Releasetyp wie folgt kategorisiert:

|Hauptversion | Nebenversion  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service führt die Aktualisierung der Patchreleases von Quorum auf vorhandene ausgeführte Mitglieder innerhalb von 30 Tagen nach der Bereitstellung durch Quorum automatisch durch.

## <a name="availability-of-new-ledger-versions"></a>Verfügbarkeit neuer Ledgerversionen

Azure Blockchain Service macht die aktuellste Haupt- und Nebenversion des Quorum-Ledgers innerhalb von 60 Tagen nach dessen Veröffentlichung durch den Quorum-Hersteller verfügbar. Wenn Sie ein neues Mitglied und Konsortium bereitstellen, stehen maximal vier Nebenversionen für Konsortien zur Auswahl. Upgrades von einer Haupt- auf eine Nebenversion werden derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Einschränkungen des Azure Blockchain-Diensts](limits.md)
