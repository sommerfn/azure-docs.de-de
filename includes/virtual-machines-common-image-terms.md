---
title: include file
description: include file
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174953"
---
## <a name="terminology"></a>Begriff

Ein Marketplace-Image in Azure hat die folgenden Attribute:

* **Herausgeber**: Die Organisation, die das Image erstellt hat. Beispiele: Canonical, MicrosoftWindowsServer
* **Angebot**: Name einer Gruppe verwandter Images, die von einem Herausgeber erstellt wurden. Beispiele: UbuntuServer, WindowsServer
* **SKU**: Eine Instanz eines Angebots, z.B. eine Hauptversion einer Distribution. Beispiele: 18.04-LTS, 2019-Datacenter
* **Version**: Die Versionsnummer einer Image-SKU. 

Um ein Marketplace-Image zu identifizieren, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen, geben Sie diese Werte einzeln als Parameter an. Einige Tools akzeptieren einen Image-*URN*. Der URN kombiniert diese Werte und trennt sie durch einen Doppelpunkt (:): *Herausgeber*:*Angebot*:*SKU*:*Version*. In einem URN können Sie die Versionsnummer durch „latest“ ersetzen, um die neueste Version des Images auszuwählen. 

Wenn der Imageherausgeber zusätzliche Lizenz- und Kaufbedingungen angibt, müssen Sie diese Bedingungen akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Sie müssen auch die Parameter des *Kaufplans* angeben, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen. Siehe [Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen](#deploy-an-image-with-marketplace-terms).
