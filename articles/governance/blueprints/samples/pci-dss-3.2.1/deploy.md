---
title: Beispiel - PCI-DSS v3.2.1 Blueprint - Bereitstellungsschritte
description: Stellen Sie die Schritte des Blueprint-Musters des Payment Card Industry Data Security Standard v3.2.1 bereit.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 237444cde6e9ccd709f61d13669da03094e5be10
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019420"
---
# <a name="deploy-the-pci-dss-v321-blueprint-sample"></a>Bereitstellen des PCI-DSS v3.2.1 Blueprint-Musters

Um das Blaupausenmuster von Azure Blueprints PCI-DSS v3.2.1 bereitzustellen, müssen die folgenden Schritte durchgeführt werden:

> [!div class="checklist"]
> - Erstellen einer neuen Blaupause mithilfe des Beispiels
> - Markieren Ihrer Kopie des Beispiels als **Veröffentlicht**
> - Zuweisen Ihrer Kopie der Blaupause zu einem vorhandenen Abonnement

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="create-blueprint-from-sample"></a>Erstellen einer Blaupause mithilfe des Beispiels

Implementieren Sie zuerst das Blaupausenbeispiel, indem Sie mithilfe des Beispiels eine neue Blaupause in Ihrer Umgebung erstellen.

1. Wählen Sie **Alle Dienste** aus, suchen Sie im linken Bereich nach der Option **Richtlinie**, und wählen Sie sie aus. Wählen Sie auf der Seite **Richtlinie** die Option **Blaupausen** aus.

1. Klicken Sie links auf der Seite **Erste Schritte** unter _Blaupause erstellen_ auf die Schaltfläche **Erstellen**.

1. Finden Sie das **PCI-DSS v3.2.1** Blueprint-Muster unter _Andere Muster_ und wählen Sie **Dieses Muster benutzen**.

1. Geben Sie die _Grundlagen_ des Blaupausenbeispiels ein:

   - **Name der Blaupause**: Geben Sie einen Namen für Ihre Kopie des PCI-DSS v3.2.1 Blueprint-Musters an.
   - **Definitionsspeicherort**: Klicken Sie auf die Schaltfläche mit den Auslassungspunkten, und wählen Sie die Verwaltungsgruppe aus, in der Sie Ihre Kopie des Beispiels speichern möchten.

1. Wählen Sie oben auf der Seite die Registerkarte _Artefakte_ oder unten auf der Seite die Option **Weiter: Artefakte** aus.

1. Überprüfen Sie die Liste der Artefakte, aus denen das Blaupausenbeispiel besteht. Viele der Artefakte enthalten Parameter, die Sie später definieren. Wählen Sie **Entwurf speichern** aus, nachdem Sie das Blaupausenbeispiel überprüft haben.

## <a name="publish-the-sample-copy"></a>Veröffentlichen der Kopie des Beispiels

Ihre Kopie des Blaupausenbeispiels wurde jetzt in Ihrer Umgebung erstellt. Sie wird im Modus **Entwurf** erstellt und muss **veröffentlicht** werden, bevor sie zugewiesen und bereitgestellt werden kann. Die Kopie des Blueprint-Musters kann an Ihre Umgebung und Ihre Bedürfnisse angepasst werden, aber diese Änderung kann sie vom PCI-DSS v3.2.1-Standard entfernen.

1. Wählen Sie **Alle Dienste** aus, suchen Sie im linken Bereich nach der Option **Richtlinie**, und wählen Sie sie aus. Wählen Sie auf der Seite **Richtlinie** die Option **Blaupausen** aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite die Option **Blaupause veröffentlichen** aus. Geben Sie auf der neuen Seite rechts für Ihre Kopie des Blaupausenbeispiels eine **Version** an. Diese Eigenschaft ist hilfreich, wenn Sie später Änderungen vornehmen. Geben Sie **Änderungsnotizen** wie „Erste Version aus dem PCI-DSS v3.2.1 Blueprint-Muster veröffentlicht“ an. Wählen Sie dann unten auf der Seite **Veröffentlichen** aus.

## <a name="assign-the-sample-copy"></a>Zuweisen der Kopie des Beispiels

Nachdem die Kopie des Blaupausenbeispiels erfolgreich **veröffentlicht** wurde, kann sie einem Abonnement innerhalb der Verwaltungsgruppe, in der sie gespeichert wurde, zugewiesen werden. In diesem Schritt werden Parameter angegeben, damit jede Bereitstellung der Kopie des Blaupausenbeispiels eindeutig ist.

1. Wählen Sie **Alle Dienste** aus, suchen Sie im linken Bereich nach der Option **Richtlinie**, und wählen Sie sie aus. Wählen Sie auf der Seite **Richtlinie** die Option **Blaupausen** aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite mit der Blaupausendefinition die Option **Blaupause zuweisen** aus.

1. Geben Sie die Parameterwerte für die Blaupausenzuweisung an:

   - Grundlagen

     - **Abonnements**: Wählen Sie mindestens eines der Abonnements in der Verwaltungsgruppe aus, in der Sie die Kopie des Blaupausenbeispiels gespeichert haben. Wenn Sie mehrere Abonnements auswählen, wird für jedes Abonnement eine Zuweisung mit den eingegebenen Parametern erstellt.
     - **Zuweisungsname:** Der Name wird basierend auf dem Namen der Blaupause vorab ausgefüllt.
       Sie können ihn nach Bedarf ändern oder unverändert übernehmen.
     - **Standort**: Wählen Sie eine Region aus, in der die verwaltete Identität erstellt werden soll. Azure Blueprint verwendet diese verwaltete Identität zum Bereitstellen aller Artefakte in der zugewiesenen Blaupause. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Version der Blaupausendefinition:** Wählen Sie eine **veröffentlichte** Version Ihrer Kopie des Blaupausenbeispiels aus.

   - Zuweisung sperren

     Wählen Sie die Blaupausensperreinstellung für die Umgebung aus. Weitere Informationen finden Sie unter [Grundlegendes zur Ressourcensperre in Azure Blueprint](../../concepts/resource-locking.md).

   - Verwaltete Identität

     Behalten Sie die Standardeinstellung _Vom System zugewiesen_ für die verwaltete Identität bei.

   - Artefaktparameter

     Die in diesem Abschnitt definierten Parameter gelten für das Artefakt, unter dem sie definiert werden. Diese Parameter sind [dynamische Parameter](../../concepts/parameters.md#dynamic-parameters), da sie während der Zuweisung der Blaupause definiert werden. Eine vollständige Liste der Artefaktparameter und die zugehörigen Beschreibungen finden Sie in der [Tabelle der Artefaktparameter](#artifact-parameters-table).

1. Nachdem Sie alle Parameter eingegeben haben, wählen Sie unten auf der Seite die Option **Zuweisen** aus. Die Blaupausenzuweisung wird erstellt, und die Artefaktbereitstellung wird gestartet. Die Bereitstellung dauert ungefähr eine Stunde. Um den Status der Bereitstellung zu überprüfen, öffnen Sie die Blaupausenzuweisung.

> [!WARNING]
> Der Azure Blueprints-Dienst und die integrierten Blaupausenbeispiele sind **kostenlos**. Azure-Ressourcen werden [nach Produkt abgerechnet](https://azure.microsoft.com/pricing/). Verwenden Sie den [Preisrechner](https://azure.microsoft.com/pricing/calculator/) zum Schätzen der Kosten für die Ausführung der Ressourcen, die mit diesem Blaupausenbeispiel bereitgestellt werden.

## <a name="artifact-parameters-table"></a>Tabelle der Artefaktparameter

Die folgende Tabelle enthält eine Aufstellung der Parameter des Blaupausenartefakts:

|Artefaktname|Artefakttyp|Parametername|BESCHREIBUNG|
|-|-|-|-|
|\[Vorschau\]: Audit PCI v3.2.1:2018 steuert und stellt spezifische VM-Erweiterungen zur Unterstützung der Überwachungsanforderungen bereit.|Richtlinienzuweisung|Liste der Ressourcentypen | Überwachen der Diagnoseeinstellung für ausgewählte Ressourcentypen. Standardmäßig sind alle Ressourcen ausgewählt| 
|Allowed locations (Zulässige Speicherorte)|Richtlinienzuweisung|Die Liste der zulässigen Speicherorte|Liste der Standorte im Rechenzentrum, in denen jede Ressource bereitgestellt werden kann. Diese Liste kann an die gewünschten Azure-Standorte weltweit angepasst werden. Wählen Sie Standorte, die Sie zulassen möchten.| 
|Zulässige Speicherorte für Ressourcengruppen|Richtlinienzuweisung |Zulässiger Standort |Mit dieser Richtlinie können Sie die Standorte einschränken, an denen Ihr Unternehmen Ressourcengruppen erstellen kann. Wird zur Erzwingung Ihrer Geokonformitätsanforderungen verwendet.| 
|Bereitstellen von Auditing auf SQL-Servern|Richtlinienzuweisung|Aufbewahrung|Datenaufbewahrung in Anzahl der Tage. Standardmäßig ist der Wert ist 180, aber PCI erfordert 365.| 
|Bereitstellen von Auditing auf SQL-Servern|Richtlinienzuweisung|Name der Ressourcengruppe für das Speicherkonto|Auditing schreibt Datenbankereignisse in ein Audit-Protokoll in Ihrem Azure Storage-Konto (ein Speicherkonto wird in jeder Region erstellt, in der ein SQL-Server erstellt wird, der von allen Servern in dieser Region gemeinsam genutzt wird).| 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun die Schritte zur Bereitstellung des PCI-DSS v3.2.1 Blueprint-Musters angesehen haben, lesen Sie die folgenden Artikel, um mehr über die Übersichts- und Kontrollzuordnung zu erfahren:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 Blueprint – Übersicht ](./index.md)
> [PCI-DSS v3.2.1 Blueprint – Control Mapping](./control-mapping.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).
