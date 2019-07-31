---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/15/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 39b61c78cbd0333183c80234256d6041a109d846
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249917"
---
Eine Azure AD-Ressourcen-ID gibt die Zielgruppe an, für die ein ausgegebenes Token verwendet werden kann, um den Zugriff auf eine Azure-Ressource zu ermöglichen. Im Fall von Azure Storage kann die Ressourcen-ID für ein einzelnes Speicherkonto spezifisch sein, oder sie kann für ein beliebiges Speicherkonto gelten. In der folgenden Tabelle werden die Werte beschrieben, die Sie für die Ressourcen-ID bereitstellen können:

|Ressourcen-ID  |BESCHREIBUNG  |
|---------|---------|
|`https://<account>.blob.core.windows.net` <br /><br /> `https://<account>.queue.core.windows.net`    | Der Dienstendpunkt für ein bestimmtes Speicherkonto. Verwenden Sie diesen Wert, um ein Token für die Autorisierung von Anforderungen nur an dieses bestimmte Azure Storage-Konto und diesen Dienst zu erhalten. Ersetzen Sie den Wert in Klammern durch den Namen Ihres Speicherkontos.      |
|`https://storage.azure.com/`     | Verwenden Sie ihn, um ein Token zum Autorisieren von Anforderungen an ein beliebiges Azure Storage-Konto abzurufen.        |
