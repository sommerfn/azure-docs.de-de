---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.custom: include file
ms.openlocfilehash: d25f6f0d787b7343b93025ff2dd2bd5bf4f0f6d8
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71948936"
---
Rollenbasierte Zugriffssteuerung ist eine vererbungsbasierte Sicherheitsstrategie zum Verwalten von Zugriffen, Berechtigungen und Rollen. Untergeordnete Rollen erben Berechtigungen von übergeordneten Rollen. Berechtigungen können auch zugewiesen werden, ohne dass sie von einer übergeordneten Rolle geerbt wurden. Sie können auch zugewiesen werden, um eine Rolle nach Bedarf anzupassen.

Beispielsweise benötigt ein Bereichsadministrator möglicherweise globalen Zugriff zum Ausführen aller Vorgänge für einen angegebenen Bereich. Der Zugriff schließt alle Knoten unterhalb oder innerhalb des Bereichs ein. Ein Geräte-Installationsprogramm benötigt möglicherweise nur Berechtigungen zum *Lesen* und *Aktualisieren* für Geräte und Sensoren.

In jedem Fall werden Rollen *genau die Zugriffsberechtigungen* erteilt, die sie zum Erfüllen ihrer Aufgaben benötigen und die dem Prinzip der geringsten Rechte entsprechen. Nach diesem Prinzip gilt für eine Identität *nur Folgendes*:

* Der Umfang an Zugriff, den sie zum Ausführen ihrer Aufgabe benötigt.
* Eine Rolle, die für das Ausführen ihrer Aufgabe geeignet und darauf beschränkt ist.

>[!IMPORTANT]
> Befolgen Sie immer das Prinzip der geringsten Rechte.

Zwei weitere wichtige Aspekte einer rollenbasierten Zugriffssteuerung, die zu beachten sind:

> [!div class="checklist"]
> * Überprüfen Sie in regelmäßigen Abständen Rollenzuweisungen, um sicherzustellen, dass jede Rolle die richtigen Berechtigungen hat.
> * Bereinigen Sie Rollen und Zuweisungen, sobald Rollen oder Zuweisungen von Personen geändert wurden.