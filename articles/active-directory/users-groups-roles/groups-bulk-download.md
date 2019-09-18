---
title: Herunterladen einer Liste von Gruppen im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Laden Sie Gruppeneigenschaften in einem Massenvorgang im Azure Admin Center in Azure Active Directory herunter.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: b1d0f94e9cf9b91bc365586dafc147dc73b17876
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914754"
---
# <a name="bulk-download-a-list-of-groups-preview-in-azure-active-directory"></a>Herunterladen einer Liste von Gruppen in einem Massenvorgang (Vorschau) in Azure Active Directory

Im Azure Active Directory-Portal (Azure AD) können Sie die Liste aller Gruppen in Ihrer Organisation per Massenvorgang in eine CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) herunterladen.

> [!NOTE]
> Azure AD-Massenvorgänge sind eine öffentliche Previewfunktion von Azure AD und bei jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Nutzungsbedingungen für Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-download-service-limits"></a>Grenzwerte für den Massendownloaddienst

Jede Massenaktivität zum Herunterladen einer Gruppenliste kann bis zu einer Stunde dauern. Auf diese Weise können Sie eine Liste von mindestens 300.000 Gruppen herunterladen.

## <a name="to-download-a-list-of-groups"></a>So laden Sie eine Liste von Gruppen herunter

1. Melden Sie sich mit dem Konto eines Administrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie in Azure AD **Gruppen** > **Gruppen herunterladen** aus.
1. Wählen Sie auf der Seite **Gruppen herunterladen** die Option **Start** aus, um eine CSV-Datei mit der Auflistung Ihrer Gruppen herunterzuladen.

   ![Der Befehl „Gruppen herunterladen“ befindet sich auf der Seite „Alle Gruppen“.](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Überprüfen des Downloadstatus

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Die Seite „Ergebnisse von Massenvorgängen“ zeigt Ihnen den Status von Massenanforderungen.](./media/groups-bulk-download/bulk-center.png)

## <a name="next-steps"></a>Nächste Schritte

- [Massenentfernung von Gruppenmitgliedern](groups-bulk-remove-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
