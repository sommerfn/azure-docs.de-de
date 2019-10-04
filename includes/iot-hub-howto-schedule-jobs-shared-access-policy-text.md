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
ms.openlocfilehash: e493d1c4f5851ee510ea83e706afce5fbb6f487e
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70049047"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-schedule-jobs.md files-->

In diesem Artikel erstellen Sie einen Back-End-Dienst, der einen Auftrag zum Aufrufen einer direkten Methode auf einem Gerät sowie einen Auftrag zum Aktualisieren des Gerätezwillings plant und den Fortschritt der beiden Aufträge überwacht. Für diese Vorgänge benötigt Ihr Dienst die Berechtigungen **Lesevorgänge in Registrierung** und **Schreibvorgänge in Registrierung**. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **registryReadWrite** erstellt, die diese Berechtigungen erteilt.
