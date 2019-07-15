---
title: Welche Dienstabhängigkeiten bestehen bei bedingtem Zugriff mit Azure Active Directory? | Microsoft-Dokumentation
description: Erfahren Sie, wie Bedingungen beim bedingten Zugriff mit Azure Active Directory verwendet werden, um eine Richtlinie auszulösen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aca2e4ea5e107358ff72e83562057830ece2cc
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509351"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Welche Dienstabhängigkeiten bestehen bei bedingtem Zugriff mit Azure Active Directory? 

Mit Richtlinien mit bedingtem Zugriff können Sie Zugriffsanforderungen für Websites und Dienste angeben. Ihre Zugriffsanforderungen können beispielsweise die Multi-Factor Authentication (MFA) oder [verwaltete Geräte](require-managed-devices.md) umfassen. 

Wenn Sie direkt auf eine Site oder einen Dienst zugreifen, kann die Auswirkung einer zugehörigen Richtlinie normalerweise leicht bewertet werden. Falls Sie beispielsweise über eine Richtlinie verfügen, für die eine MFA-Konfiguration für SharePoint Online benötigt wird, wird MFA für jede Anmeldung am SharePoint-Webportal erzwungen. Es ist aber nicht immer einfach, die Auswirkung einer Richtlinie zu bewerten, weil Cloud-Apps mit Abhängigkeiten von anderen Cloud-Apps vorhanden sind. Beispielsweise können Microsoft-Teams den Zugriff auf Ressourcen in SharePoint Online ermöglichen. Wenn Sie in Ihrem aktuellen Szenario also auf Microsoft Teams zugreifen, unterliegen Sie auch der SharePoint-MFA-Richtlinie.   

## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien 

Wenn Sie eine Dienstabhängigkeit konfiguriert haben, kann die Richtlinie per früh oder spät gebundener Erzwingung angewendet werden. 

- **Früh gebundene Richtlinienerzwingung** bedeutet, dass ein Benutzer die abhängige Dienstrichtlinie erfüllen muss, bevor er auf die aufrufende App zugreift. Ein Benutzer muss beispielsweise die SharePoint-Richtlinie erfüllen, bevor er sich an MS Teams anmeldet. 
- Die **spät gebundene Richtlinienerzwingung** erfolgt, nachdem sich der Benutzer an der aufrufenden App angemeldet hat. Die Erzwingung wird zurückgestellt, wenn App-Anforderungen (Token für den Downstreamdienst) aufgerufen werden. Beispiele hierfür sind der Zugriff auf Planner durch MS Teams und auf SharePoint durch Office.com. 

In der Abbildung unten sind MS Teams-Dienstabhängigkeiten dargestellt. Durchgehende Pfeile weisen auf eine früh gebundene Erzwingung hin, während der gestrichelte Pfeil für Planner für eine spät gebundene Erzwingung steht. 

![MS Teams-Dienstabhängigkeiten](./media/service-dependencies/01.png)

Die bewährte Methode besteht darin, für alle zugehörigen Apps und Dienste nach Möglichkeit immer gemeinsame Richtlinien festzulegen. Ein einheitlicher Sicherheitsstatus sorgt für die bestmögliche Benutzererfahrung. Wenn Sie beispielsweise eine gemeinsame Richtlinie für Exchange Online, SharePoint Online, Microsoft Teams und Skype für Unternehmen festlegen, werden unerwartete Eingabeaufforderungen, die sich aus der Anwendung unterschiedlicher Richtlinien auf nachgelagerte Dienste ergeben können, deutlich reduziert. 

In der Tabelle unten sind die zusätzlichen Dienstabhängigkeiten aufgeführt, die von den Client-Apps erfüllt werden müssen.  

| Client-Apps         | Downstreamdienst                          | Erzwingung |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |
| Microsoft Classroom | Exchange                                    | Früh gebunden |
|                     | SharePoint                                  | Früh gebunden  |
| Microsoft Teams     | Exchange                                    | Früh gebunden |
|                     | MS Planner                                  | Spät gebunden  |
|                     | SharePoint                                  | Früh gebunden |
|                     | Skype for Business Online                   | Früh gebunden |
| Office-Portal       | Exchange                                    | Spät gebunden  |
|                     | SharePoint                                  | Spät gebunden  |
| Outlook-Gruppen      | Exchange                                    | Früh gebunden |
|                     | SharePoint                                  | Früh gebunden |
| PowerApps           | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |
|                     | Microsoft Azure Active Directory              | Früh gebunden |
| Project             | Dynamics CRM                                | Früh gebunden |
| Skype for Business  | Exchange                                    | Früh gebunden |
| Visual Studio       | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Implementierung von bedingtem Zugriff in Ihrer Umgebung finden Sie unter[ Planen der Bereitstellung von bedingtem Zugriff in Azure Active Directory](plan-conditional-access.md).
