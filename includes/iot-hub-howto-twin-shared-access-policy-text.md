---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a843821a7720ddcb17f8a369d012facd7ba81e35
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912372"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

In diesem Artikel erstellen Sie einen Back-End-Dienst, der einem Gerätezwilling die gewünschten Eigenschaften hinzufügt und anschließend die Identitätsregistrierung abfragt, um alle Geräte mit gemeldeten Eigenschaften zu ermitteln, die entsprechend aktualisiert wurden. Ihr Dienst benötigt die Berechtigung **Dienstverbindung**, um die gewünschten Eigenschaften eines Gerätezwillings ändern zu können, sowie die Berechtigung **Lesevorgänge in Registrierung**, um die Identitätsregistrierung abfragen zu können. Da keine SAS-Standardrichtlinie mit nur diesen beiden Berechtigungen zur Verfügung steht, müssen Sie eine erstellen.
