---
title: Bereitstellen der OPC Twin-Cloudabhängigkeiten in Azure | Microsoft-Dokumentation
description: Informationen zum Bereitstellen von Azure OPC Twin-Abhängigkeiten.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: cb07899b51280cff8613d637640c0da23debbc8e
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69016514"
---
# <a name="deploying-dependencies-for-local-development"></a>Bereitstellen von Abhängigkeiten für die lokale Entwicklung

In diesem Artikel wird erläutert, wie Sie ausschließlich die für die lokale Entwicklung und das lokale Debuggen erforderlichen Azure-Plattformdienste bereitstellen.   Am Ende haben Sie eine Ressourcengruppe mit allen für die lokale Entwicklung und das lokale Debuggen erforderlichen Elementen erstellt.

## <a name="deploy-azure-platform-services"></a>Bereitstellen von Azure-Plattformdiensten

1. Stellen Sie sicher, dass die PowerShell- und [AzureRM PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)-Erweiterungen installiert sind.  Öffnen Sie eine Eingabeaufforderung oder ein Terminal, und führen Sie Folgendes aus:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Befolgen Sie die Anweisungen, um der Ressourcengruppe für die Bereitstellung einen Namen zuzuweisen.  Mit dem Skript werden ausschließlich die Abhängigkeiten für diese Ressourcengruppe in Ihrem Azure-Abonnement, jedoch nicht die Microservices zugewiesen.  Außerdem wird mit dem Skript eine Anwendung in Azure Active Directory registriert.  Diese ist zur Unterstützung der OAUTH-basierten Authentifizierung erforderlich.  Die Bereitstellung kann einige Minuten dauern.

3. Nach Abschluss des Skripts können Sie die ENV-Datei speichern.  Die ENV-Umgebungsdatei ist die Konfigurationsdatei für alle Dienste und Tools, die Sie auf dem Entwicklungscomputer ausführen möchten.  

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung

### <a name="resource-group-name"></a>Ressourcengruppenname

Verwenden Sie einen kurzen und einfachen Namen für die Ressourcengruppe.  Der Name wird auch zum Benennen von Ressourcen verwendet, daher muss er den Namenskonventionen für Ressourcen entsprechen.  

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory-Registrierung (AAD)

Über das Bereitstellungsskript wird versucht, die AAD-Anwendungen in Azure Active Directory zu registrieren.  Je nach Ihren Rechten für den ausgewählten AAD-Mandanten können dabei Fehler auftreten.   Drei Optionen stehen zur Verfügung:

1. Wenn Sie in einer Liste mit AAD-Mandanten einen Mandanten ausgewählt haben, starten Sie das Skript neu, und wählen Sie einen anderen Mandanten aus.
2. Alternativ können Sie einen privaten AAD-Mandanten bereitstellen, das Skript neu starten und diesen Mandanten verwenden.
3. Fahren Sie ohne Authentifizierung fort.  Da Sie Ihre Microservices lokal ausführen, ist dies möglich. Produktionsumgebungen werden jedoch nicht simuliert.  

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie OPC Twin-Dienste erfolgreich in einem vorhandenen Projekt bereitgestellt haben, empfiehlt sich nun folgender Schritt:

> [!div class="nextstepaction"]
> [Bereitstellen von OPC Twin-Modulen](howto-opc-twin-deploy-modules.md)
