---
title: Bewährte Methoden zum Verwenden von Key Vault – Azure Key Vault | Microsoft-Dokumentation
description: In diesem Dokument werden einige der bewährten Methoden zum Verwenden von Key Vault erläutert.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-key-vault
ms.service: key-vault
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 654a9bb772c8a7426a335c98dfeca69515b9ce67
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70881621"
---
# <a name="best-practices-to-use-key-vault"></a>Bewährte Methoden zum Verwenden von Key Vault

## <a name="control-access-to-your-vault"></a>Steuern des Zugriffs auf Ihren Schlüsseltresor

Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). Da es sich hierbei um vertrauliche und geschäftskritische Daten handelt, müssen Sie den Zugriff auf Schlüsseltresore schützen, sodass nur autorisierte Anwendungen und Benutzer zugelassen sind. In diesem [Artikel](key-vault-secure-your-key-vault.md) finden Sie eine Übersicht des Modells für den Key Vault-Zugriff. Er erläutert Authentifizierung und Autorisierung und beschreibt, wie Sie den Zugriff auf Ihre Schlüsseltresore schützen.

Vorschläge für die Zugriffssteuerung auf Ihren Schlüsseltresor sehen wie folgt aus:
1. Sperren Sie den Zugriff auf Ihr Abonnement, Ihre Ressourcengruppe und Ihre Schlüsseltresore (RBAC).
2. Erstellen Sie Zugriffsrichtlinien für jeden Schlüsseltresor.
3. Verwenden Sie den Prinzipal mit den geringsten Zugriffsrechten, um den Zugriff zu gewähren.
4. Aktivieren Sie die Firewall und [VNET-Dienstendpunkte](key-vault-overview-vnet-service-endpoints.md).

## <a name="use-separate-key-vault"></a>Verwenden Sie separate Key Vaults.

Es wird empfohlen, einen Schlüsseltresor pro Anwendung und Umgebung (Entwicklung, Präproduktion und Produktion) zu verwenden. Dadurch können Sie die Geheimnisse nicht umgebungsübergreifend freigegeben und es verringert auch die Bedrohung im Falle einer Sicherheitsverletzung.

## <a name="backup"></a>Backup

Stellen Sie sicher, dass Sie regelmäßig Sicherungskopien Ihres [Schlüsseltresors](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) beim Aktualisieren/Löschen/Erstellen von Objekten in einem Schlüsseltresor erstellen.

## <a name="turn-on-logging"></a>Aktivieren der Protokollierung

[Aktivieren Sie die Protokollierung](key-vault-logging.md) für Ihren Schlüsseltresor. Richten sie auch Warnungen ein.

## <a name="turn-on-recovery-options"></a>Aktivieren von Wiederherstellungsoptionen

1. Aktivieren Sie [Vorläufiges Löschen](key-vault-ovw-soft-delete.md).
2. Aktivieren Sie den Bereinigungsschutz, wenn Sie sich auch nach dem Aktivieren des vorläufigen Löschens vor dem erzwungenen Löschen des Geheimnis/Schlüsseltresors schützen möchten.
