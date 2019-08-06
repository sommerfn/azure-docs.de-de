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
ms.openlocfilehash: f97ab0a0bb91af90daaf60c5c2c85f0ede56a5e7
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403964"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

In diesem Artikel erstellen Sie einen Back-End-Dienst, der einem Gerätezwilling die gewünschten Eigenschaften hinzufügt und anschließend die Identitätsregistrierung abfragt, um alle Geräte mit gemeldeten Eigenschaften zu ermitteln, die entsprechend aktualisiert wurden. Ihr Dienst benötigt die Berechtigung **Dienstverbindung**, um die gewünschten Eigenschaften eines Gerätezwillings ändern zu können, sowie die Berechtigung **Lesevorgänge in Registrierung**, um die Identitätsregistrierung abfragen zu können. Da keine SAS-Standardrichtlinie mit nur diesen beiden Berechtigungen zur Verfügung steht, müssen Sie eine erstellen.
