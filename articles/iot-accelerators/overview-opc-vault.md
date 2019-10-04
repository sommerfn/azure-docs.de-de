---
title: Was ist OPC Vault? – Azure | Microsoft-Dokumentation
description: Übersicht über OPC Vault
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 44315790116545dd888aed533731bbf01abe801d
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997309"
---
# <a name="what-is-opc-vault"></a>Was ist OPC Vault?

OPC Vault ist ein Microservice, mit dem der Zertifikatlebenszyklus für OPC UA-Server- und -Clientanwendungen in der Cloud konfiguriert, registriert und verwaltet werden kann. In diesem Artikel werden einfache Anwendungsfälle für OPC Vault beschrieben.

## <a name="certificate-management"></a>Zertifikatverwaltung

Es kann beispielsweise sein, dass ein Fertigungsunternehmen seinen OPC UA-Servercomputer mit einer neu entwickelten Clientanwendung verbinden muss. Wenn das Fertigungsunternehmen zum ersten Mal auf den Servercomputer zugreift, wird in der OPC UA-Serveranwendung sofort eine Fehlermeldung angezeigt, um darauf hinzuweisen, dass die Clientanwendung nicht sicher ist. Dieser Mechanismus ist in den OPC UA-Servercomputer integriert, um den unberechtigten Zugriff auf die Anwendung und somit gefährliches Hacking in der Fertigung zu verhindern.

## <a name="application-security-management"></a>Verwaltung der Anwendungssicherheit
Ein Sicherheitsexperte nutzt den OPC Vault-Microservice zum einfachen Aktivieren des OPC UA-Servers für die Kommunikation mit einer beliebigen Clientanwendung, da OPC Vault über alle Funktionen für die Zertifikatregistrierung, Speicherung und Lebenszyklusverwaltung verfügt. Nachdem der OPC UA-Server nun auf sichere Weise verbunden wurde, kann er mit der neu entwickelten Clientanwendung kommunizieren.

## <a name="the-complete-opc-vault-architecture"></a>Vollständige OPC Vault-Architektur
Im folgenden Diagramm ist die gesamte OPC Vault-Architektur dargestellt.

![OPC Vault-Architektur](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier über OPC Vault und entsprechende Verwendungsmöglichkeiten informiert haben, können Sie mit dem nächsten Thema fortfahren:

> [!div class="nextstepaction"]
> [OPC Vault-Architektur](overview-opc-vault-architecture.md)
