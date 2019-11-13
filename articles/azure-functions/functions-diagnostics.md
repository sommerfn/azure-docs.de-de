---
title: Übersicht über die Azure Functions-Diagnose
description: Erfahren Sie, wie Sie Probleme mit Ihrer Funktions-App mit der Azure Functions-Diagnose beheben können.
keywords: Funktionen, Diagnosen, Support, serverlos, Problembehandlung, Selbsthilfe
author: yunjchoi
manager: gwallace
ms.service: azure-functions
ms.topic: article
ms.date: 11/01/2019
ms.author: yunjchoi
ms.custom: na
ms.openlocfilehash: 96bfc133ef390f34712151733277ae6f9e9a8d70
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73722412"
---
# <a name="azure-functions-diagnostics-overview"></a>Übersicht über die Azure Functions-Diagnose

Wenn Sie eine Funktions-App ausführen, möchten Sie auf alle auftretenden Probleme vorbereitet sein, von 4xx-Fehlern bis hin zu Triggerfehlern. Die Azure Functions-Diagnose ist eine intelligente und interaktive Komponente, mit der Sie Probleme bei Ihrer Funktions-App beheben können, ohne dass eine Konfiguration erforderlich ist oder zusätzliche Kosten anfallen. Wenn bei Ihrer Funktions-App Probleme auftreten, zeigt die Azure Functions-Diagnose den Fehler auf, damit Sie die richtigen Informationen finden, um das Problem einfacher und schneller behandeln und lösen zu können. Dieser Artikel zeigt Ihnen die Grundlagen, wie Sie die Azure Functions-Diagnose verwenden können, um häufige Probleme mit Funktions-Apps schneller zu diagnostizieren und zu lösen.

## <a name="start-azure-functions-diagnostics"></a>Starten der Azure Functions-Diagnose

So greifen Sie auf die Azure Functions-Diagnose zu:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App.
2. Wählen Sie die Registerkarte **Plattformfeatures** aus.
3. Wählen Sie **Diagnose und Problembehandlung** unter **Resource Management-** aus. Die Azure Functions-Diagnose wird geöffnet.
4. Wählen Sie mithilfe der Schlüsselwörter auf der Kachel „Startseite“ eine Kategorie aus, die das Problem ihrer Funktions-App am besten beschreibt. Sie können auch ein Schlüsselwort in die Suchleiste eingeben, das Ihr Problem am besten beschreibt. Beispielsweise können Sie `execution` eingeben, um eine Liste der Diagnoseberichte anzuzeigen, die sich auf die Ausführung ihrer Funktions-App beziehen, und diese direkt über die Startseite öffnen.

![Startseite](./media/functions-diagnostics/homepage.png)

## <a name="use-the-interactive-interface"></a>Verwenden der interaktiven Benutzeroberfläche

Nachdem Sie auf der Startseite eine Kategorie ausgewählt haben, die am ehesten auf das bei der Funktions-App aufgetretene Problem zutrifft, können Sie über die interaktive Benutzeroberfläche Genie der Azure Functions-Diagnose durch die Diagnose und Problembehebung Ihrer App geleitet werden. Sie können die in Genie angegebenen Kachelverknüpfungen verwenden, um den vollständigen Diagnosebericht der jeweiligen Problemkategorie anzuzeigen. Über die Kachelverknüpfungen haben Sie direkt die Möglichkeit, auf die Diagnosemetriken zuzugreifen.

![Genie](./media/functions-diagnostics/genie.png)

Nachdem Sie eine Kachel ausgewählt haben, wird eine Liste mit Themen angezeigt, die sich auf das in der jeweiligen Kachel beschriebene Problem beziehen. Diese Themen enthalten Auszüge mit relevanten Informationen aus dem vollständigen Bericht. Sie können jedes einzelne dieser Themen auswählen, um die Probleme weiter zu untersuchen. Zudem können Sie **Vollständigen Bericht anzeigen** auswählen, um alle Themen auf einer Seite zu untersuchen.

![Vorschau des Diagnoseberichts](./media/functions-diagnostics/preview-of-diagnostic-report.png)

## <a name="view-a-diagnostic-report"></a>Anzeigen eines Diagnoseberichts

Nachdem Sie ein Thema ausgewählt haben, wird ein spezifischer Diagnosebericht für Ihre Funktions-App angezeigt. In Diagnoseberichten werden Statussymbole verwendet, um anzugeben, ob bestimmte Probleme mit der App vorliegen. Sie sehen eine detaillierte Beschreibung des Problems, empfohlene Aktionen, verwandte Metriken und hilfreiche Dokumente. Angepasste Diagnoseberichte werden aus einer Reihe von Überprüfungen generiert, die für Ihre Funktions-App ausgeführt werden. Diagnoseberichte können ein nützliches Tool zum Ermitteln von Problemen in ihrer Funktions-App und zur Behebung des Problems sein.

## <a name="find-the-problem-code"></a>Ermitteln des Problemcodes 

Für skriptbasierte Funktionen können Sie **Function Execution** (Funktionsausführung) und **Errors under Function App Down or Reporting Errors** („Fehler“ unter „Funktions-App ausgefallen“ oder „Fehlermeldungen“) verwenden, um die Codezeile einzugrenzen, die Ausnahmen oder Fehler verursacht. Diese Funktion kann ein nützliches Tool sein, um die Grundursache zu ermitteln und Probleme aus einer bestimmten Codezeile zu beheben. Diese Option ist für vorkompilierte C#- und Java-Funktionen nicht verfügbar.

![Diagnosebericht zu Funktionsausführungsfehlern](./media/functions-diagnostics/diagnostic-report-on-function-execution-errors.png)

![Funktionsausnahme](./media/functions-diagnostics/function-exception.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [UserVoice](https://feedback.azure.com/forums/355860-azure-functions) können Sie Fragen stellen oder Feedback zur Azure Functions-Diagnose bereitstellen. Bitte schließen Sie `[Diag]` in den Titel Ihres Feedbacks ein.

> [!div class="nextstepaction"]
> [Überwachen Ihrer Funktions-Apps](functions-monitoring.md)
