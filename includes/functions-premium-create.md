---
title: include file
description: include file
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 10/25/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 911db285d1ca885142e8a80345926ce76379e3bc
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72958589"
---
1. Öffnen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).

1. Wählen Sie die Schaltfläche **Ressource erstellen** aus.

    ![Erstellen von Ressourcen](./media/functions-create-function-app-portal/function-app-create-resource.png)

1. Wählen Sie **Compute** > **Funktions-App** aus.

    ![Erstellen einer Funktions-App im Azure-Portal](./media/functions-premium-create/function-app-create-start.png)

1. Verwenden Sie die in der Tabelle unter der Abbildung angegebenen Einstellungen für die Funktions-App.

    ![Grundlagen](./media/functions-premium-create/function-app-create-basics.png)

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. |
    | **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)** |  *myResourceGroup* | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird |
    | **Name der Funktions-App** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z` (Groß-/Kleinschreibung nicht beachtet), `0-9` und `-`.  |
    |**Veröffentlichen**| Code | Option zum Veröffentlichen von Codedateien oder eines Docker-Containers. |
    | **Laufzeitstapel** | Bevorzugte Sprache | Wählen Sie eine Runtime aus, die Ihre bevorzugte Programmiersprache für Funktionen unterstützt. Wählen Sie **.NET** für C#- und F#-Funktionen aus. |
    |**Region**| Bevorzugte Region | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |

    Wählen Sie die Schaltfläche **Weiter: Hosting >** aus.

1. Geben Sie die folgenden Hostingeinstellungen ein.

    ![Hosting](./media/functions-premium-create/function-app-premium-create-hosting.png)

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Speicherkonto](../articles/storage/common/storage-quickstart-create-account.md)** |  Global eindeutiger Name |  Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](../articles/azure-functions/functions-scale.md#storage-account-requirements) erfüllen muss. |
    |**Betriebssystem**| Bevorzugtes Betriebssystem | Ein Betriebssystem ist für Sie basierend auf Ihrer Runtimestapelauswahl vorab ausgewählt, aber Sie können die Einstellung ggf. ändern. |
    | **[Plan](../articles/azure-functions/functions-scale.md)** | Premium | Wählen Sie als Plantyp **Premium (Vorschau)** sowie Standardwerte für die Optionen *Windows-Plan* und *SKU und Größe* aus. |

    Wählen Sie die Schaltfläche **Weiter: Überwachung >** aus.

1. Geben Sie die folgenden Überwachungseinstellungen ein.

    ![Überwachung](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Standard | Erstellt eine Application Insights-Ressource mit dem gleichen *App-Namen* in der nächstgelegenen unterstützten Region. Durch Erweitern dieser Einstellung können Sie den **neuen Ressourcennamen** ändern oder einen anderen **Standort** in einer [Azure-Region](https://azure.microsoft.com/global-infrastructure/geographies/) wählen, in der Sie Ihre Daten speichern möchten. |

    Wählen Sie **Überprüfen + erstellen** aus, um die App-Konfigurationsauswahl zu überprüfen.

1. Klicken Sie auf **Erstellen**, um die Funktionen-App bereitzustellen.