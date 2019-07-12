---
title: 'Einrichten von Dienstwarnungen für Windows Virtual Desktop: Azure'
description: Informationen zum Einrichten von Azure Service Health für den Empfang von Dienstbenachrichtigungen für Windows Virtual Desktop.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: v-chjenk
ms.openlocfilehash: cae75f16da2cad453c74b7e6e9fb62dd789fe5c7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081140"
---
# <a name="tutorial-set-up-service-alerts"></a>Tutorial: Einrichten von Dienstwarnungen

Mit Azure Service Health können Sie Dienstprobleme und Integritätsempfehlungen für Windows Virtual Desktop überwachen. Azure Service Health kann Sie mithilfe verschiedener Warnungstypen (z.B. E-Mail oder SMS) benachrichtigen, die Auswirkungen eines Problems verdeutlichen und Sie über die Lösung eines Problems auf dem Laufenden halten. Azure Service Health kann Sie auch bei der Verkürzung von Ausfallzeiten und Vorbereitung auf eine geplante Wartung und Änderungen unterstützen, die sich u.U. auf die Verfügbarkeit Ihrer Ressourcen auswirken.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen und Konfigurieren von Dienstwarnungen

Weitere Informationen zu Azure Service Health finden Sie in der [Azure Service Health-Dokumentation](https://docs.microsoft.com/azure/service-health/).

## <a name="prerequisites"></a>Voraussetzungen

- [Tutorial: Erstellen eines Mandanten in Windows Virtual Desktop (Vorschauversion)](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory)
- [Tutorial: Erstellen von Dienstprinzipalen und Rollenzuweisungen mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell)
- [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)

## <a name="create-service-alerts"></a>Erstellen von Dienstwarnungen

In diesem Abschnitt erfahren Sie, wie Sie Azure Service Health konfigurieren und Benachrichtigungen einrichten, auf die Sie im Azure-Portal zugreifen können. Sie können verschiedene Arten von Warnungen einrichten und diese so planen, dass Sie rechtzeitig benachrichtigt werden.

### <a name="recommended-service-alerts"></a>Empfohlene Dienstwarnungen

Wir empfehlen die Erstellung von Dienstwarnungen für die folgenden Integritätsereignistypen:

- **Dienstproblem:** Sie können Benachrichtigungen zu wichtigen Problemen erhalten, die sich auf die Verbindung Ihrer Benutzer mit dem Dienst oder auf die Verwaltung Ihres Windows Virtual Desktop-Mandanten auswirken.
- **Integritätsempfehlung:** Sie können Benachrichtigungen erhalten, die Ihre Aufmerksamkeit erfordern. Es folgen Beispiele für diese Art von Benachrichtigung:
    - Virtuelle Computer (VMs) mit offenem Port 3389 nicht sicher konfiguriert
    - Unterstützung von Funktionalität eingestellt

### <a name="configure-service-alerts"></a>Konfigurieren von Dienstwarnungen

So konfigurieren Sie Dienstwarnungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Service Health** aus.
3. Befolgen Sie die Anweisungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal), um Ihre Warnungen und Benachrichtigungen einzurichten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie mit Azure Service Health Dienstprobleme und Integritätsempfehlungen für Windows Virtual Desktop überwachen. Fahren Sie mit den Anleitungsartikeln zum Herstellen der Verbindung mit Windows Virtual Desktop fort, um mehr zur Anmeldung bei Windows Virtual Desktop zu erfahren.

> [!div class="nextstepaction"]
> [Connect to the Remote Desktop client on Windows 7 and Windows 10](./connect-windows-7-and-10.md) (Herstellen einer Verbindung mit dem Remotedesktopclient unter Windows 7 und Windows 10)
