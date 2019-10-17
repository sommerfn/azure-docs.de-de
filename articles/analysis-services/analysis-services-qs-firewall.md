---
title: Schnellstart – Konfigurieren einer Firewall für einen Analysis Services-Server in Azure | Microsoft-Dokumentation
description: Informationen zum Konfigurieren einer Firewall für eine Analysis Services-Serverinstanz in Azure
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/02/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c3c8de80f90fe203029b38171502e3d4a9b9dd46
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298534"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Schnellstart: Konfigurieren der Serverfirewall – Portal

In diesem Schnellstart wird beschrieben, wie Sie eine Firewall für Ihren Azure Analysis Services-Server konfigurieren. Das Aktivieren einer Firewall und das Konfigurieren von IP-Adressbereichen nur für jene Computer, die auf Ihren Server zugreifen, sind ein wichtiger Bestandteil der Sicherung Ihres Servers und Ihrer Daten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Analysis Services-Server in Ihrem Abonnement. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Analysis Services-Servers im Azure-Portal](analysis-services-create-server.md) oder unter [Schnellstart: Erstellen eines Servers mit PowerShell](analysis-services-create-powershell.md).
- Mindestens ein IP-Adressbereich für Clientcomputer (falls erforderlich).
- Beachten Sie, dass das Importszenario von Power BI Premium derzeit nicht unterstützt wird.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an. 

[Anmelden beim Portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Konfigurieren einer Firewall

1. Klicken Sie auf Ihren Server, um die Seite „Übersicht“ zu öffnen. 
2. Klicken Sie unter **EINSTELLUNGEN** > **Firewall** > **Firewall aktivieren** auf **Ein**.
3. Klicken Sie in **Zugriff über Power BI zulassen** auf **Ein**, um DirectQuery-Zugriff über Power BI zuzulassen.  
4. (Optional) Geben Sie einen oder mehrere IP-Adressbereiche an. Geben Sie einen Namen sowie eine Start- und End-IP-Adresse für jeden Bereich ein. Der Name einer Firewallregel ist auf 128 Zeichen begrenzt und darf nur aus Großbuchstaben, Kleinbuchstaben, Zahlen, Unterstriche und Bindestriche bestehen. Leer- und andere Sonderzeichen sind nicht zulässig.
5. Klicken Sie auf **Speichern**.

     ![Firewalleinstellungen](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie IP-Adressbereiche, oder deaktivieren Sie die Firewall, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie erfahren, wie eine Firewall für Ihren Server konfiguriert wird. Nachdem Sie nun über einen Server verfügen und ihn mit einer Firewall gesichert haben, können Sie ihm ein einfaches Beispieldatenmodell aus dem Portal hinzufügen. Ein Beispielmodell ist hilfreich, um sich mit dem Konfigurieren von Modelldatenbankrollen und dem Testen von Clientverbindungen vertraut zu machen. Fahren Sie mit dem Tutorial zum Hinzufügen eines Beispielmodells fort, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Tutorial: Hinzufügen eines Beispielmodells zu Ihrem Server](analysis-services-create-sample-model.md)
