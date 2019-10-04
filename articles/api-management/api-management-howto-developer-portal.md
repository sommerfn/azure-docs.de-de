---
title: Zugriff auf und Anpassen des neuen Entwicklerportals – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das neue Entwicklerportal in API Management verwenden.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: da75ca43a2576e3214d4b67f9eb61c7bad3bd5cc
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70073521"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Zugriff auf und Anpassen des neuen Entwicklerportals – Azure API Management

Dieser Artikel veranschaulicht, wie Sie auf das neue Azure API Management-Entwicklerportal zugreifen. Er führt Sie durch die Visual Editor-Erfahrung – Hinzufügen und Bearbeiten von Inhalten – sowie durch das Anpassen des Aussehens der Website.

![Neues Entwicklerportal für API Management](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a> Voraussetzungen

- Bearbeiten Sie den folgenden Schnellstart: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Importieren und veröffentlichen Sie eine Azure API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> Das neue Entwicklerportal ist zurzeit als Vorschauversion verfügbar.

## <a name="managed-vs-self-hosted"></a> Verwaltete und selbstgehostete Versionen

Sie können Ihr Entwicklerportal auf zwei Arten erstellen:

- **Verwaltete Version**: Indem Sie das in Ihre API Management-Instanz integrierte Portal bearbeiten und anpassen, auf das Sie über die URL `<your-api-management-instance-name>.developer.azure-api.net` zugreifen können.
- **Selbst gehostete Version**: Indem Sie Ihr Portal außerhalb einer API Management-Instanz bereitstellen und selbst hosten. Dieser Ansatz ermöglicht es Ihnen, die Codebasis des Portals zu bearbeiten und die bereitgestellten Kernfunktionen zu erweitern. Details und Anweisungen finden Sie im [GitHub-Repository mit dem Quellcode des Portals][1].

## <a name="managed-access"></a> Zugreifen auf die verwaltete Version des Portals

Führen Sie die folgenden Schritte aus, um auf die verwaltete Version des Portals zuzugreifen.

1. Wechseln Sie im Azure-Portal zu Ihrer API Management-Dienstinstanz.
1. Klicken Sie oben in der Navigationsleiste auf die Schaltfläche **Neues Entwicklerportal (Vorschau)** . Eine neue Browserregisterkarte mit einer administrativen Version des Portals wird geöffnet. Wenn Sie zum ersten Mal auf das Portal zugreifen, wird der Standardinhalt automatisch bereitgestellt.

## <a name="managed-tutorial"></a> Bearbeiten und Anpassen der verwalteten Version des Portals

In dem folgenden Video demonstrieren wir, wie Sie den Inhalt des Portals bearbeiten, das Aussehen der Website anpassen und die Änderungen veröffentlichen.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="faq"></a> Häufig gestellte Fragen

In diesem Abschnitt beantworten wir allgemeine häufig gestellte Fragen zum neuen Entwicklerportal. Fragen, die sich speziell auf die selbstgehostete Version beziehen, finden Sie im [GitHub-Repository im Abschnitt „Wiki“](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="how-can-i-migrate-content-from-the-old-developer-portal-to-the-new-one"></a>Wie kann ich Inhalte aus dem alten Entwicklerportal zu dem neuen Portal migrieren?

Das ist nicht möglich. Die Portale sind nicht miteinander kompatibel.

### <a name="when-will-the-portal-become-generally-available"></a>Wann wird das Portal allgemein verfügbar sein?

Das Portal befindet sich derzeit in der Vorschauphase und wird bis Ende des Kalenderjahres (2019) allgemein verfügbar sein. Die Vorschauversion sollte nicht zu Produktionszwecken verwendet werden.

### <a name="will-the-old-portal-be-deprecated"></a>Wird das alte Portal eingestellt?

Ja, es wird eingestellt, wenn das neue Portal allgemein verfügbar wird. Wenn Sie Bedenken haben, posten Sie diese in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/121).

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>Verfügt das neue Portal über alle Features des alten Portals?

Das Ziel der allgemeinen Verfügbarkeit besteht darin, eine szenariobasierte Featureparität mit dem alten Portal zu bieten. Bis dahin wurden in der Vorschauversion möglicherweise noch keine ausgewählten Features implementiert.

Ausnahmen sind die *Anwendungen* und *Issues* aus dem alten Portal, die im neuen Portal nicht verfügbar sein werden. Wenn Sie *Issues* im alten Portal verwenden und diese im neuen Portal benötigen, posten Sie einen Kommentar in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/122).

### <a name="ive-found-bugs-andor-id-like-to-request-a-feature"></a>Ich habe Fehler gefunden und/oder möchte eine Funktion anfordern.

Prima. Sie können uns Feedback geben, eine Funktionsanforderung übermitteln oder einen Fehlerbericht im [Abschnitt „Probleme“ des GitHub-Repositorys](https://github.com/Azure/api-management-developer-portal/issues) einreichen. Wenn Sie einmal dort sind, würden wir uns über Ihr Feedback zu den Problemen freuen, die mit der Bezeichnung `community` markiert sind.

### <a name="i-want-to-move-the-content-of-the-new-portal-between-environments-how-can-i-do-that-and-do-i-need-to-go-with-the-self-hosted-version"></a>Ich möchte den Inhalt des neuen Portals zwischen Umgebungen verschieben. Wie kann ich das machen, und muss ich dazu die selbstgehostete Version verwenden?

Dies ist sowohl in der verwalteten als auch in der selbstgehosteten Portalversion möglich. Das neue Entwicklerportal unterstützt das Extrahieren von Inhalten über die Verwaltungs-API Ihres API Management-Diensts. Die APIs sind im [GitHub-Repository im Abschnitt „Wiki“](https://github.com/Azure/api-management-developer-portal/wiki/) dokumentiert. Wir haben auch [ein Skript](https://github.com/Azure/api-management-developer-portal/blob/master/scripts/migrate.bat) geschrieben, das Ihnen beim Einstieg helfen könnte.

Wir arbeiten noch daran, diesen Prozess auf das API Management DevOps Resource Kit auszurichten.

### <a name="how-can-i-select-a-layout-when-creating-a-new-page"></a>Wie kann ich ein *Layout* beim Erstellen einer neuen *Seite* auswählen?

Ein *Layout* wird auf eine Seite angewendet, indem die zugehörige URL-Vorlage mit der URL *der Seite* abgeglichen wird. Beispielsweise wird das *Layout* mit einer URL-Vorlage vom Typ `/wiki/*` auf jede *Seite* mit dem Segment `/wiki/` angewendet: `/wiki/getting-started`, `/wiki/styles` usw.

### <a name="why-doesnt-the-interactive-developer-console-work"></a>Warum funktioniert die interaktive Entwicklerkonsole nicht?

Das liegt wahrscheinlich an CORS. Die interaktive Konsole sendet eine clientseitige API-Anforderung über den Browser. Sie können das CORS-Problem beheben, indem Sie [eine CORS-Richtlinie](https://docs.microsoft.com/azure/api-management/api-management-cross-domain-policies#CORS) für Ihre API(s) hinzufügen. Sie können entweder alle Parameter manuell angeben (z. B. „origin“ als https://contoso.com) oder einen Platzhalterwert `*` verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum neuen Entwicklerportal:

- [GitHub-Repository mit dem Quellcode][1]
- [Anweisungen zum Selbsthosten des Portals][2]
- [Öffentliche Roadmap des Projekts][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects