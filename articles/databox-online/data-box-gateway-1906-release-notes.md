---
title: Versionshinweise zu Azure Data Box Gateway und Azure Data Box Edge 1906 | Microsoft-Dokumentation
description: In diesem Artikel werden schwerwiegende offene Probleme und Lösungen für das Release 1906 von Azure Data Box Gateway und Azure Data Box Edge beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 09/18/2019
ms.author: alkohli
ms.openlocfilehash: 69c905176a5beb1c7d442cd5f1a69161cd520c5a
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71099447"
---
# <a name="azure-data-box-edge-and-azure-data-box-gateway-1906-release-notes"></a>Versionshinweise zu Azure Data Box Edge und Azure Data Box Gateway 1906

In den folgenden Versionshinweisen werden die schwerwiegenden offenen und die gelösten Probleme für das Release 1906 von Azure Data Box Edge und Azure Data Box Gateway beschrieben.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres Data Box Edge/Data Box Gateway die Informationen in den Anmerkungen zu dieser Version sorgfältig durch.

Dieses Release entspricht den folgenden Softwareversionen:

- **Data Box Gateway 1906 (1.6.978.743)**
- **Data Box Edge 1906 (1.6.978.743)**

> [!NOTE]
> Update 1906 kann nur auf Data Box Edge-Geräte angewandt werden, auf denen die allgemein verfügbare Version oder Version 1905 der Software ausgeführt wird.

## <a name="whats-new"></a>Neuigkeiten

- **Fehlerbehebung im Workflow zur Verwaltung des Wiederherstellungsschlüssels**: In der früheren Version gab es einen Fehler, durch den der Wiederherstellungsschlüssel nicht angewendet wurde. Dieser Fehler wurde in dieser Version behoben. Es wird dringend empfohlen, dieses Update anzuwenden, da mit dem Wiederherstellungsschlüssel die Daten auf dem Gerät wiederhergestellt werden können, falls das Gerät nicht gestartet wird. Weitere Informationen finden Sie unter der Vorgehensweise zum [Speichern des Wiederherstellungsschlüssels beim Bereitstellen von Data Box Edge oder Data Box Gateway](data-box-edge-deploy-connect-setup-activate.md#set-up-and-activate-the-physical-device).
- **Verbesserungen bei der FPGA-Protokollierung (Field Programmable Gate Array):** Ab Version 1905 wurden Verbesserungen bei der Protokollierung und den Warnungen im Zusammenhang mit FPGA vorgenommen. Dies ist weiterhin ein erforderliches Update für Data Box Edge, wenn Sie die Edgecomputingfunktion mit dem FPGA verwenden. Weitere Informationen finden Sie unter [Transformieren von Daten mit Edgecomputing unter Data Box Edge](data-box-edge-deploy-configure-compute-advanced.md).

## <a name="known-issues-in-ga-release"></a>Bekannte Probleme im Release zur allgemeinen Verfügbarkeit

Für dieses Release werden keine neuen Probleme aufgeführt. Alle in den Versionshinweisen enthaltenen Probleme wurden aus früheren Releases übernommen. Um eine Liste der bekannten Probleme anzuzeigen, wechseln Sie zu [Bekannte Probleme in der Version für die allgemeine Verfügbarkeit](data-box-gateway-release-notes.md#known-issues-in-ga-release).


## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
- [Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md)
