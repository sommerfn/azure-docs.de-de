---
title: 'Überprüfen von XML für die B2B-Unternehmensintegration: Azure Logic Apps'
description: Überprüfen von XML mithilfe von Schemas in Azure Logic Apps mit Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: 7813dcb375ff4a123b1314f8f9db453b1f0b187e
ms.sourcegitcommit: d37991ce965b3ee3c4c7f685871f8bae5b56adfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680231"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Überprüfen von XML für die B2B-Unternehmensintegration in Azure Logic Apps mit Enterprise Integration Pack

In B2B-Szenarien müssen sich die Parteien einer Vereinbarung häufig vergewissern, dass die untereinander ausgetauschten Nachrichten gültig sind, bevor eine Datenverarbeitung gestartet werden kann. Dokumente können mithilfe der XML-Überprüfungsaktion aus dem Enterprise Integration Pack anhand eines vordefinierten Schemas überprüft werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Sollten Sie noch kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine leere oder bereits vorhandene Logik-App, in der Sie die XML-Überprüfungsaktion verwenden möchten. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ein Ihrem Azure-Abonnement zugeordnetes [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) wird mit der Logik-App verknüpft, in der Sie die XML-Überprüfungsaktion verwenden möchten, und enthält das Schema, das Sie für die Überprüfung von XML-Inhalten verwenden möchten. Sowohl Ihre Logik-App als auch Ihr Integrationskonto müssen an demselben Standort oder in derselben Azure-Region vorhanden sein.

## <a name="add-xml-validation-action"></a>Hinzufügen der XML-Überprüfungsaktion

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wenn Sie über eine leere Logik-App verfügen, geben Sie im Suchfeld des Logik-App-Designers `HTTP request` als Filter ein, und wählen Sie den Trigger **Beim Empfang einer HTTP-Anforderung** aus. Fahren Sie andernfalls mit dem nächsten Schritt fort.

1. Wählen Sie im letzten Schritt des Workflows die Option **Neuer Schritt** aus.

   Wenn Sie eine Aktion zwischen vorhandenen Schritten hinzufügen möchten, zeigen Sie auf den Pfeil, der die beiden Schritte verbindet. Daraufhin wird ein Pluszeichen ( **+** ) angezeigt. Wählen Sie dieses Pluszeichen und anschließend die Option **Aktion hinzufügen** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `xml validation` als Filter ein. Wählen Sie in der Liste mit den Aktionen die Aktion **XML-Überprüfung** aus.

   ![Suchen und Auswählen der Aktion „XML-Überprüfung“](./media/logic-apps-enterprise-integration-xml-validation/select-xml-validation-action.png)

1. Klicken Sie zum Angeben des zu überprüfenden XML-Inhalts auf das Feld **Inhalt**, um die Liste mit den dynamischen Inhalten anzuzeigen.

   ![Öffnen der Liste mit den dynamischen Inhalten](./media/logic-apps-enterprise-integration-xml-validation/open-dynamic-content-list.png)

   Die Liste mit den dynamischen Inhalten enthält Eigenschaftstoken, die die Ausgaben der vorherigen Schritte im Workflow darstellen. Sollte eine erwartete Eigenschaft nicht in der Liste enthalten sein, überprüfen Sie in der Trigger- oder Aktionsüberschrift, ob Sie **Mehr anzeigen** auswählen können.

1. Wählen Sie in der Liste mit den dynamischen Inhalten die Eigenschaft mit dem zu überprüfenden Inhalt aus.

   In diesem Beispiel wird die Triggerausgabe **Text** ausgewählt.

   ![Auswählen des zu überprüfenden Inhalts](./media/logic-apps-enterprise-integration-xml-validation/select-content-to-validate.png)

1. Öffnen Sie zum Angeben des Schemas, das Sie für die Überprüfung verwenden möchten, die Liste **Schemaname**, und wählen Sie das Überprüfungsschema aus, das Sie Ihrem verknüpften Integrationskonto hinzugefügt haben.

   ![Auswählen des Schemas für die Überprüfung](./media/logic-apps-enterprise-integration-xml-validation/select-validation-schema.png)

1. Speichern Sie Ihre Logik-App.

   Damit ist die Einrichtung Ihrer Überprüfung abgeschlossen. In einer echten App empfiehlt es sich ggf., die überprüften Daten in einer branchenspezifischen App wie Salesforce zu speichern. Um die überprüfte Ausgabe an Salesforce zu senden, fügen Sie eine Aktion hinzu.

1. Sie können Ihre Überprüfungsaktion testen, indem Sie eine Anforderung senden, die den Workflow Ihrer Logik-App auslöst.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)