---
title: include file
description: include file
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 103ad020b8d9f50ffe690f502b7cac08dab9237a
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640448"
---
* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.)

* **Windows**

    * [Python 2.x oder 3.x.](https://www.python.org/downloads/) Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. [*pip*, das Python-Paketverwaltungssystem, installieren oder upgraden](https://pip.pypa.io/en/stable/installing/).

    * Vergewissern Sie sich bei Verwendung des Windows-Betriebssystems, dass Sie über die korrekte Version von [Visual C++ Redistributable Package](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) verfügen, um die Verwendung nativer DLLs über Python zu ermöglichen. Es empfiehlt sich, die neueste Version zu verwenden.

    * Installieren Sie bei Bedarf das Paket [azure-iothub-device-client](https://pypi.org/project/azure-iothub-device-client/) mithilfe des Befehls `pip install azure-iothub-device-client`.

    * Installieren Sie bei Bedarf das Paket [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) mithilfe des Befehls `pip install azure-iothub-service-client`.

* **Mac OS**

    Für Mac OS benötigen Sie Python 3.7.0 (oder 2.7) sowie libboost-1.67 und cURL 7.61.1 (jeweils installiert über Homebrew). In allen anderen Distributionen/Betriebssystemen sind wahrscheinlich andere Versionen von Boost sowie Abhängigkeiten eingebettet, die nicht funktionieren und zur Laufzeit einen Importfehler verursachen.

    Die *pip*-Pakete für `azure-iothub-service-client` und `azure-iothub-device-client` sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).

> [!NOTE]
> Uns liegen mehrere Berichte über Fehler beim Importieren von „iothub_client“ in einem Beispiel vor. Weitere Informationen zum Umgang Problemen vom Typ **ImportError** finden Sie unter [Dealing with ImportError issues](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues) (Behandeln von ImportError-Problemen).
