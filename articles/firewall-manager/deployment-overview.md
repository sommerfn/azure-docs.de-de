---
title: Übersicht über die Bereitstellung von Azure Firewall Manager (Vorschau)
description: Informieren Sie sich über die allgemeinen Schritte zur Bereitstellung von Azure Firewall Manager (Vorschau).
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: df87e652d2969d4ae12e97a2b455648cf39382c3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488256"
---
# <a name="azure-firewall-manager-preview-deployment-overview"></a>Übersicht über die Bereitstellung von Azure Firewall Manager (Vorschau)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Es gibt mehrere Möglichkeiten, die Vorschauversion von Azure Firewall Manager bereitzustellen, jedoch sollten Sie den folgenden allgemeinen Prozess nutzen.

## <a name="prerequisites"></a>Voraussetzungen

> [!IMPORTANT]
> Azure Firewall Manager (Vorschauversion) muss explizit mithilfe des PowerShell-Befehls `Register-AzProviderFeature` aktiviert werden.
>Führen Sie an einer PowerShell-Eingabeaufforderung die folgenden Befehle aus:
>
>```azure-powershell
>connect-azaccount
>Register-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network
>```
>Es dauert bis zu 30 Minuten, bis die Featureregistrierung abgeschlossen ist. Führen Sie den folgenden Befehl aus, um den Registrierungsstatus zu überprüfen:
>
>`Get-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network`



## <a name="general-deployment-process"></a>Allgemeiner Bereitstellungsprozess

1. Erstellen Ihrer Hub-and-Spoke-Architektur

   - Erstellen Sie mit Azure Firewall Manager einen geschützten virtuellen Hub, und fügen Sie virtuelle Netzwerkverbindungen hinzu.<br>*or*<br>
   - Erstellen Sie einen virtuellen WAN-Hub, und fügen Sie virtuelle Netzwerkverbindungen hinzu.
2. Auswählen von Sicherheitsanbietern

   - Erfolgt beim Erstellen eines geschützten virtuellen Hubs.<br>*or*<br>
   - Konvertieren Sie einen vorhandenen virtuellen WAN-Hub in einen geschützten virtuellen Hub.
3. Erstellen einer Firewallrichtlinie und deren Zuordnung zu Ihrem Hub

   - Gilt nur, wenn Azure Firewall verwendet wird.
   - SECaaS-Richtlinien (Security-as-a-Service, Sicherheit als Dienst) von Drittanbietern werden über die Verwaltungsbenutzeroberfläche von Partnern konfiguriert.
4. Konfigurieren von Routeneinstellungen zum Weiterleiten von Datenverkehr an Ihren geschützten Hub

   - Leiten Sie mithilfe der Seite zur Routeneinstellung für geschützte virtuelle Hubs den Datenverkehr zur Filterung und Protokollierung ohne benutzerdefinierte Routen (UDR) auf virtuellen Spoke-Netzwerken einfach an Ihren geschützten Hub weiter.

> [!NOTE]
> - Sie können nicht mehr als einen Hub pro virtuellem WAN verwenden. Aber Sie können mehrere virtuelle WANs in der Region hinzufügen, um dies zu erreichen.
> - Überlappende IP-Adressräume für Hubs sind in einem VWAN nicht möglich.
> - Ihre Hub-VNET-Verbindungen müssen sich in der gleichen Region wie der Hub befinden.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager (Vorschauversion) unter Verwendung des Azure-Portals](secure-cloud-network.md)