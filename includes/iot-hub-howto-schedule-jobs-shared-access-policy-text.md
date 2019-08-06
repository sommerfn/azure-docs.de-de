---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 47d46cf4b400e29de8c526f750a10f57d6400220
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402658"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-schedule-jobs.md files-->

In diesem Artikel erstellen Sie einen Back-End-Dienst, der einen Auftrag zum Aufrufen einer direkten Methode auf einem Gerät sowie einen Auftrag zum Aktualisieren des Gerätezwillings plant und den Fortschritt der beiden Aufträge überwacht. Für diese Vorgänge benötigt Ihr Dienst die Berechtigungen **Lesevorgänge in Registrierung** und **Schreibvorgänge in Registrierung**. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **registryReadWrite** erstellt, die diese Berechtigungen erteilt.
