---
title: Überblick über das Azure API Management-Entwicklerportal – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Entwicklerportal in API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 1311328dde6fc70202ce3c6271b33f79d52102cc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472226"
---
# <a name="azure-api-management-developer-portal-overview"></a>Azure API Management-Entwicklerportal: Übersicht

Das Entwicklerportal ist eine automatisch generierte, vollständig anpassbare Website mit der Dokumentation Ihrer APIs. Dort können API-Consumer ihre APIs ermitteln, erfahren, wie sie verwendet werden, Zugriff anfordern und die APIs ausprobieren.

In diesem Artikel werden die Unterschiede zwischen selbstgehosteten und verwalteten Versionen des Entwicklerportals in API Management beschrieben. Außerdem wird die Architektur erläutert, und es werden Antworten auf häufig gestellte Fragen bereitgestellt.

> [!WARNING]
> Zurzeit erfolgt das Rollout des neuen Entwicklerportals in die API Management-Dienste.
> Wenn Ihr Dienst neu erstellt wurde oder es sich um einen Dienst der Entwicklerebene handelt, sollten Sie bereits über die neueste Version verfügen. Andernfalls können Probleme auftreten (z.B. mit der Veröffentlichungsfunktion). Das Featurerollout wird am Montag, den 11. November 2019 abgeschlossen.
>
> Erfahren Sie, wie Sie von der Vorschauversion zur allgemein verfügbaren Version des neuen Entwicklerportals [migrieren](#preview-to-ga) können.

![Entwicklerportal für API Management](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="managed-vs-self-hosted"></a> Verwaltete und selbstgehostete Versionen

Sie können Ihr Entwicklerportal auf zwei Arten erstellen:

- **Verwaltete Version**: Indem Sie das in Ihre API Management-Instanz integrierte Portal bearbeiten und anpassen, auf das Sie über die URL `<your-api-management-instance-name>.developer.azure-api.net` zugreifen können. Lesen Sie [diesen Dokumentationsartikel](api-management-howto-developer-portal-customize.md), um zu erfahren, wie Sie auf das verwaltete Portal zugreifen und es anpassen können.
- **Selbst gehostete Version**: Indem Sie Ihr Portal außerhalb einer API Management-Instanz bereitstellen und selbst hosten. Dieser Ansatz ermöglicht es Ihnen, die Codebasis des Portals zu bearbeiten und die bereitgestellten Kernfunktionen zu erweitern. Sie müssen auch ein Upgrade des Portals auf die neueste Version ausführen. Details und Anweisungen finden Sie im [GitHub-Repository mit dem Quellcode des Portals][1]. Das [Tutorial für die verwaltete Version](api-management-howto-developer-portal-customize.md) führt Sie schrittweise durch den Verwaltungsbereich des Portals, der auch in der selbstgehosteten Version enthalten ist.

## <a name="portal-architectural-concepts"></a>Konzepte der Portarchitektur

Die Portalkomponenten können logisch in zwei Kategorien unterteilt werden: *Code* und *Inhalt*.

*Code* wird im [GitHub-Repository][1] verwaltet und umfasst Folgendes:

- Widgets, die visuelle Elemente darstellen und HTML, JavaScript, Formatierungsfunktionen, Einstellungen und Inhaltszuordnung kombinieren. Beispiele hierfür sind ein Bild, ein Textabsatz, ein Formular, eine Liste mit APIs usw.
- Formatierungsdefinitionen, die angeben, wie Widgets formatiert werden können.
- Die Engine, die statische Webseiten aus Portalinhalten generiert und in JavaScript geschrieben ist.
- Einen visuellen Editor, der das Anpassen und Erstellen im Browser ermöglicht.

*Inhalt* wird in zwei Unterkategorien unterteilt: *Portalinhalt* und *API Management-Inhalt*.

*Portalinhalt* ist für das Portal spezifisch und umfasst Folgendes:

- Seiten: etwa Landing Page, API-Tutorials, Blogbeiträge
- Medien: Bilder, Animationen und andere dateibasierte Inhalte
- Layouts: Vorlagen, die mit einer URL abgeglichen werden und definieren, wie Seiten angezeigt werden
- Formatvorlagen: Werte für Formatierungsdefinitionen, z.B. Schriftarten, Farben, Rahmen
- Einstellungen: Konfiguration, z.B. Favicon, Websitemetadaten

*Portalinhalte* (mit Ausnahme von Medien) werden als JSON-Dokumente ausgedrückt.

*API Management-Inhalte* umfassen Entitäten wie APIs, Vorgänge, Produkte und Abonnements.

Das Portal basiert auf einem angepassten Fork des [Paperbits-Frameworks](https://paperbits.io/). Die ursprüngliche Paperbits-Funktionalität wurde erweitert, um für API Management spezifische Widgets (z.B. eine Liste von APIs, eine Liste von Produkten) und einen Connector zum API Management-Dienst zum Speichern und Abrufen von Inhalten bereitzustellen.

## <a name="faq"></a> Häufig gestellte Fragen

In diesem Abschnitt beantworten wir häufig gestellte Fragen zum neuen Entwicklerportal, die allgemeiner Natur sind. Fragen, die sich speziell auf die selbstgehostete Version beziehen, finden Sie im [GitHub-Repository im Abschnitt „Wiki“](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="a-idpreview-to-ga-how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"/> Wie kann ich von der Vorschauversion des Portals migrieren?

Mit der Vorschauversion des Entwicklerportals haben Sie den Vorschauinhalt in Ihrem API Management-Dienst bereitgestellt. Der Standardinhalt wurde in der allgemein verfügbaren Version zur Verbesserung der Benutzerfreundlichkeit erheblich geändert. Außerdem sind neue Widgets enthalten.

Wenn Sie die verwaltete Version verwenden, setzen Sie den Inhalt des Portals zurück, indem Sie im Abschnitt **Vorgänge** des Menüs auf **Inhalt zurücksetzen** klicken. Wenn Sie diesen Vorgang bestätigen, wird der gesamte Inhalt des Portals entfernt, und die neuen Standardinhalte werden bereitgestellt. Die Engine des Portals wurde automatisch in Ihrem API Management-Dienst aktualisiert.

![Zurücksetzen des Portalinhalts](media/api-management-howto-developer-portal/reset-content.png)

Wenn Sie die selbstgehostete Version verwenden, verwenden Sie die Dateien `scripts/cleanup.bat` und `scripts/generate.bat` aus dem GitHub-Repository, um vorhandene Inhalte zu entfernen und neue Inhalte bereitzustellen. Stellen Sie sicher, dass Sie den Code Ihres Portals im Vorfeld im GitHub-Repository auf die neueste Version aktualisieren.

Wenn Sie den Inhalt des Portals nicht zurücksetzen möchten, ziehen Sie ggf. in Erwägung, neu verfügbare Widgets auf Ihren Seiten zu verwenden. Vorhandene Widgets wurden automatisch auf die neuesten Versionen aktualisiert.

Wenn Ihr Portal nach der Ankündigung der allgemeinen Verfügbarkeit bereitgestellt wurde, sollte es bereits über die neuen Standardinhalte verfügen. Von Ihrer Seite ist keine Aktion erforderlich.

### <a name="how-can-i-migrate-from-the-old-developer-portal-to-the-new-developer-portal"></a>Wie kann ich aus dem alten Entwicklerportal zum neuen Portal migrieren?

Die Portale sind nicht kompatibel, und Sie müssen den Inhalt manuell migrieren.

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>Verfügt das neue Portal über alle Features des alten Portals?

Das neue Entwicklerportal unterstützt keine *Anwendungen* und *Issues*. Wenn Sie *Issues* im alten Portal verwendet haben und diese im neuen Portal benötigen, posten Sie einen Kommentar in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/122).

### <a name="has-the-old-portal-been-deprecated"></a>Wurde das alte Portal eingestellt?

Die alten Entwickler- und Herausgeberportale sind nun *Legacyfeatures*. Sie erhalten nur noch Sicherheitsupdates. Neue Features werden nur im neuen Entwicklerportal implementiert.

Die Einstellung veralteter Portale wird separat angekündigt. Wenn Sie Fragen, Bedenken oder Kommentare haben, posten Sie diese in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/121).

### <a name="how-can-i-automate-portal-deployments"></a>Wie kann ich Portalbereitstellungen automatisieren?

Sie können über die REST-API programmgesteuert auf die Inhalte des Entwicklerportals zugreifen und diese verwalten, und zwar unabhängig davon, ob Sie eine verwaltete oder eine selbstgehostete Version verwenden.

Die APIs sind im [Wiki-Abschnitt des GitHub-Repositorys][2] dokumentiert. Diese Dokumentation kann auch für die Automatisierung von Migrationen von Portalinhalten zwischen Umgebungen verwendet werden (z.B. aus einer Testumgebung in die Produktionsumgebung). Weitere Informationen zu diesem Vorgang finden Sie [in diesem Dokumentationsartikel](https://aka.ms/apimdocs/migrateportal) auf GitHub.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>Unterstützt das Portal Azure Resource Manager-Vorlagen und/oder ist es mit dem API Management DevOps Resource Kit kompatibel?

Nein.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>Muss ich zusätzliche VNET-Konnektivität für die Abhängigkeiten des verwalteten Portals aktivieren?

Nein.

### <a name="im-getting-a-cors-error-when-using-the-interactive-console-what-should-i-do"></a>Beim Verwenden der interaktiven Konsole erhalte ich einen CORS-Fehler. Wie sollte ich vorgehen?

Die interaktive Konsole sendet eine clientseitige API-Anforderung über den Browser. Sie können das CORS-Problem beheben, indem Sie [eine CORS-Richtlinie](https://docs.microsoft.com/azure/api-management/api-management-cross-domain-policies#CORS) für Ihre API(s) hinzufügen. Sie können entweder alle Parameter manuell angeben (z. B. „origin“ als https://contoso.com) oder einen Platzhalterwert `*` verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum neuen Entwicklerportal:

- [Zugreifen auf das neue Entwicklerportal und Verwalten des Portals](api-management-howto-developer-portal-customize.md)
- [Einrichten einer selbstgehosteten Version des Portals][2]

Weitere Ressourcen:

- [GitHub-Repository mit dem Quellcode][1]
- [Öffentliche Roadmap des Projekts][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects