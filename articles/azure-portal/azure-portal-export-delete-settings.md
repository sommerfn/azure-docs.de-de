---
title: Exportieren oder Löschen von Azure-Portal-Einstellungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Benutzereinstellungen, privaten Dashboards und benutzerdefinierten Einstellungen im Azure-Portal exportieren oder löschen können.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: kfollis
ms.openlocfilehash: fde7ffbaa3ef4d47eea48302a99948932aeb4f00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551666"
---
# <a name="export-or-delete-user-settings"></a>Exportieren oder Löschen von Benutzereinstellungen

Mithilfe der Einstellungen und Funktionen im Azure-Portal können Sie eine benutzerdefinierte Benutzeroberfläche erstellen. Die Informationen zu Ihren benutzerdefinierten Einstellungen werden in Azure gespeichert. Sie können folgende Benutzerdaten exportieren oder löschen:

* Private Dashboards im Azure-Portal
* Benutzereinstellungen wie bevorzugte Abonnements oder Verzeichnisse oder letztes Anmeldeverzeichnis
* Designs und andere benutzerdefinierte Portaleinstellungen

Es empfiehlt sich, Ihre Einstellungen zu exportieren und zu überprüfen, bevor Sie sie löschen. Das Neuerstellen von Dashboards oder das erneute Einrichten von benutzerdefinierten Einstellungen kann zeitaufwendig sein.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Exportieren und Löschen Ihrer Portaleinstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie ganz oben im Portal die Option **Einstellungen** aus.

    ![Screenshot des Zahnradsymbols der Portaleinstellungen](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. Wählen Sie **Alle Einstellungen exportieren** oder **Alle Einstellungen und private Dashboards löschen** aus.

    ![Screenshot der Portaleinstellungen zum Exportieren und Löschen](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      Diese Aktionen sind in der folgenden Tabelle beschrieben.

      | Aktion | BESCHREIBUNG |
      | --- | --- |
      | **Alle Einstellungen exportieren** | Erstellt eine JSON-Datei, die Ihre Benutzereinstellungen enthält, z. B. Farbdesign, Favoriten und private Dashboards.|
      | **Alle Einstellungen und private Dashboards löschen** | Löscht alle Verknüpfungen mit privaten Dashboards und anderen benutzerdefinierten Einstellungen, die Sie im Portal vorgenommen haben. |

> [!NOTE]
> Aufgrund der dynamischen Art der Benutzereinstellungen und des Risikos der Datenbeschädigung können Sie keine Einstellungen aus der JSON-Datei importieren.
>
>


## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Freigeben von Azure-Dashboards](azure-portal-dashboard-share-access.md)
* [Hinzufügen, Entfernen und Sortieren von Favoriten](azure-portal-add-remove-sort-favorites.md)
