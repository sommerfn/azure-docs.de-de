---
title: Entwerfen von Workflows für Policy-as-Code
description: Erfahren Sie, wie Sie Workflows entwerfen, um Ihre Azure Policy-Definitionen als Code bereitzustellen und Ressourcen automatisch zu überprüfen.
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: e3fcb9996266af7e952538c7c92c665929bb9492
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512087"
---
# <a name="design-policy-as-code-workflows"></a>Entwerfen von Workflows für Policy-as-Code

Je weiter Sie Ihre Cloudgovernance entwickeln, desto mehr werden Sie von der manuellen Verwaltung jeder Richtliniendefinition im Azure-Portal oder über die verschiedenen SDKs zu einem einfacheren und besser wiederholbaren Prozess auf Unternehmensebene wechseln wollen. Im Folgenden finden Sie die beiden vorherrschenden Ansätze für die Verwaltung von umfangreichen Systemen in der Cloud:

- Infrastructure-as-Code: Bei diesem Ansatz werden alle Inhalte, die Ihre Umgebungen definieren – von Resource Manager-Vorlagen über Azure Policy-Definitionen bis hin zu Azure Blueprints – als Quellcode behandelt.
- DevOps: DevOps vereint Personen, Prozesse und Produkte, um hochwertige Continuous Delivery-Prozesse an Endbenutzer zu ermöglichen.

Policy-as-Code ist eine Kombination dieser beiden Ansätze. Im Wesentlichen behalten Sie Ihre Richtliniendefinitionen in der Quellcodeverwaltung und testen und validieren Änderungen, wann immer diese eingeführt werden sollen. Dies sollte jedoch nicht der einzige Aspekt der Einbeziehung von Richtlinien in Infrastructure-as-Code oder DevOps sein.

Der Validierungsschritt sollte auch Teil anderer Continuous Integration- oder Continuous Deployment-Workflows sein. Dies gilt beispielsweise für die Bereitstellung einer Anwendungsumgebung oder einer virtuellen Infrastruktur. Indem die Azure Policy-Validierung bereits frühzeitig im Erstellungs- und Bereitstellungsprozess erfolgt, können Anwendungs- und Betriebsteams eine eventuelle Nichteinhaltung von Richtlinien ermitteln, lange bevor es zu spät ist und die Teams die Anwendung in der Produktion bereitstellen möchten.

## <a name="workflow-overview"></a>Übersicht über Workflow

Der empfohlene allgemeine Workflow für Policy-as-Code sieht in etwa wie das folgende Diagramm aus:

![Übersicht über Policy-as-Code](../media/policy-as-code/policy-as-code-workflow.png)

### <a name="create-and-update-policy-definitions"></a>Erstellen und Aktualisieren von Richtliniendefinitionen

Die Richtliniendefinitionen werden mit JSON erstellt und in der Quellcodeverwaltung gespeichert. Jede Richtlinie verfügt über einen eigenen Satz Dateien – z. B. Parameter, Regeln und Umgebungsparameter –, die im gleichen Ordner gespeichert werden sollten. Die folgende Struktur zeigt eine empfohlene Methode zum Speichern der Richtliniendefinitionen in der Quellcodeverwaltung.

```text
.
|
|- policies/  ________________________ # Root folder for policies
|  |- policy1/  ______________________ # Subfolder for a policy
|     |- policy.json _________________ # Policy definition
|     |- policy.parameters.json ______ # Policy definition of parameters
|     |- policy.rules.json ___________ # Policy rule
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
|  |- policy2/  ______________________ # Subfolder for a policy
|     |- policy.json _________________ # Policy definition
|     |- policy.parameters.json ______ # Policy definition of parameters
|     |- policy.rules.json ___________ # Policy rule
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
```

Wenn eine neue Richtlinie hinzugefügt oder eine vorhandene Richtlinie aktualisiert wird, sollte der Workflow die Richtliniendefinition in Azure automatisch aktualisieren. Das Testen der neuen oder aktualisierten Richtliniendefinition erfolgt in einem späteren Schritt.

### <a name="create-and-update-initiative-definitions"></a>Erstellen und Aktualisieren von Initiativendefinitionen

Auch Initiativen verfügen über eine eigene JSON-Datei und zugehörige Dateien, die im gleichen Ordner gespeichert werden sollten. Die Initiativendefinition erfordert eine bereits vorhandene Richtliniendefinition und kann daher erst erstellt oder aktualisiert werden, wenn die Quelle für die Richtlinie zunächst in der Quellcodeverwaltung und dann in Azure aktualisiert wurde. Die folgende Struktur zeigt eine empfohlene Methode zum Speichern der Initiativendefinitionen in der Quellcodeverwaltung.

```text
.
|
|- initiatives/ ______________________ # Root folder for initiatives
|  |- init1/ _________________________ # Subfolder for an initiative
|     |- policyset.json ______________ # Initiative definition
|     |- policyset.definitions.json __ # Initiative list of policies
|     |- policyset.parameters.json ___ # Initiative definition of parameters
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
|  |- init2/ _________________________ # Subfolder for an initiative
|     |- policyset.json ______________ # Initiative definition
|     |- policyset.definitions.json __ # Initiative list of policies
|     |- policyset.parameters.json ___ # Initiative definition of parameters
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
```

Ebenso wie bei Richtliniendefinitionen sollte der Workflow die Initiativendefinition in Azure automatisch aktualisieren, wenn eine neue Initiative hinzugefügt oder eine vorhandene aktualisiert wird. Das Testen der neuen oder aktualisierten Initiativendefinition erfolgt in einem späteren Schritt.

### <a name="test-and-validate-the-updated-definition"></a>Testen und Validieren der aktualisierten Definition

Sobald die Automatisierung Ihre neu erstellten oder aktualisierten Richtlinien- oder Initiativendefinitionen übernommen und das entsprechende Objekt in Azure aktualisiert hat, ist es an der Zeit, die Änderungen zu testen. Entweder die Richtlinie oder die zugehörige Initiative sollte Ressourcen in der Umgebung zugewiesen werden, die am weitesten von der Produktion entfernt ist. Diese Umgebung ist in der Regel die Entwicklungsumgebung: _Dev_.

Die Zuweisung sollte den [enforcementMode](./assignment-structure.md#enforcement-mode) _deaktiviert_ verwenden, damit Ressourcenerstellung und -aktualisierung nicht blockiert, aber vorhandene Ressourcen weiterhin auf Einhaltung der aktualisierten Richtliniendefinition überwacht werden. Auch im enforcementMode wird empfohlen, entweder eine Ressourcengruppe oder ein Abonnement, das speziell zur Validierung von Richtlinien dient, als Zuweisungsbereich zu verwenden.

> [!NOTE]
> Dieser Modus ist zwar hilfreich, darf aber gründliche Tests einer Richtliniendefinition unter verschiedenen Bedingungen nicht ersetzen. Die Richtliniendefinition sollte mit den REST-API-Aufrufen `PUT` und `PATCH`, konformen und nicht konformen Ressourcen sowie für Sonderfälle wie fehlenden Ressourceneigenschaften getestet werden.

Verwenden Sie nach dem Bereitstellen der Zuweisung das Policy SDK zum [Abrufen von Compliancedaten](../how-to/get-compliance-data.md) für die neue Zuweisung. Die zum Testen der Richtlinien und Zuweisungen verwendete Umgebung sollte sowohl konforme als auch nicht konforme Ressourcen enthalten. Ebenso wie bei einem sorgfältigen Komponententest für Code sollten Sie überprüfen, ob die Ressourcen den Erwartungen entsprechen und dass Sie keine falsch positiven oder falsch negativen Ergebnisse erhalten. Wenn Sie nur auf Ergebnisse testen, die Sie erwarten, kann die Richtlinie unerwartete und nicht identifizierte Auswirkungen nach sich ziehen. Weitere Informationen finden Sie unter [Auswerten der Auswirkung einer neuen Azure-Richtlinie](./evaluate-impact.md).

### <a name="enable-remediation-tasks"></a>Ermöglichen von Korrekturtasks

Wenn die Validierung der Zuweisung den Erwartungen entspricht, besteht der nächste Schritt darin, mögliche Korrekturmaßnahmen zu überprüfen.
Richtlinien, die entweder [deployIfNotExists](./effects.md#deployifnotexists) oder [modify](./effects.md#modify) verwenden, können in einen Korrekturtask umgewandelt werden und nicht konforme Ressourcen korrigieren.

Hierfür müssen Sie als Erstes der Richtlinienzuweisung die Rollenzuweisung erteilen, die in der Richtliniendefinition definiert ist. Diese Rollenzuweisung gewährt der Identität, die über die Richtlinienzuweisung verwaltet wird, ausreichende Rechte, um die erforderlichen Änderungen vorzunehmen, damit die Ressource konform wird.

Sobald die Richtlinienzuweisung über die entsprechenden Rechte verfügt, verwenden Sie das Policy SDK, um den Korrekturtask für einen Satz von Ressourcen auszulösen, die bekanntermaßen nicht konform sind. Für diese Korrekturtasks sollten drei Tests ausgeführt werden, bevor das Verfahren fortgesetzt wird:

- Überprüfen, ob der Korrekturtask erfolgreich abgeschlossen wurde
- Ausführen einer Richtlinienauswertung, um herauszufinden, ob die Richtlinienkonformitätsergebnisse erwartungsgemäß aktualisiert wurden
- Ausführen eines Komponententests in der Umgebung direkt für die Ressourcen, um zu überprüfen, ob die Eigenschaften geändert wurden

Indem Sie sowohl die aktualisierten Ergebnisse der Richtlinienauswertung als auch die Umgebung direkt testen, erhalten Sie die Bestätigung, dass die Korrekturtasks alle Aspekte erwartungsgemäß geändert haben und dass die Konformitätsänderung in der Richtliniendefinition reflektiert wird.

### <a name="update-to-enforced-assignments"></a>Aktualisieren von erzwungenen Zuweisungen

Wenn alle Validierungsvorgänge abgeschlossen sind, aktualisieren Sie die Zuweisung so, dass sich der **enforcementMode** in _aktiviert_ ändert. Diese Änderung sollte zunächst in derselben, weit von der Produktionsumgebung entfernten Umgebung durchgeführt werden. Sobald die erwartungsgemäße Funktionsweise dieser Umgebung überprüft wurde, kann die Änderung auf die nächste Umgebung und dann weiter ausgedehnt werden, bis die Richtlinie in Produktionsressourcen bereitgestellt ist.

## <a name="process-integrated-evaluations"></a>Verarbeiten von integrierten Auswertungen

Im allgemeinen Workflow für Policy-as-Code werden Richtlinien und Initiativen in großem Umfang für eine Umgebung entwickelt und bereitgestellt. Die Richtlinienauswertung sollte jedoch Teil des Bereitstellungsprozesses jedes Workflows sein, mit dem Ressourcen in Azure erstellt oder bereitgestellt werden. Hierzu gehören beispielsweise das Bereitstellen von Anwendung oder das Ausführen von Resource Manager-Vorlagen zum Erstellen einer Infrastruktur.

In diesen Fällen sollte nach der Bereitstellung der Anwendung oder Infrastruktur in einem Testabonnement oder einer Testressourcengruppe eine Richtlinienauswertung für diesen Bereitstellungsumfang erfolgen, um die erfolgreiche Validierung aller vorhandenen Richtlinien und Initiativen zu überprüfen. Auch wenn Richtlinien und Initiativen in einer solchen Umgebung möglicherweise mit dem **enforcementMode** _deaktiviert_ konfiguriert sind, ist es nützlich, bereits frühzeitig zu erfahren, ob eine Anwendungs- oder Infrastrukturbereitstellung gegen Richtlinien verstoßen könnte. Diese Richtlinienauswertung sollte daher als Schritt in solche Workflows eingebunden werden und Bereitstellungen, die nicht konforme Ressourcen erstellen, als fehlerhaft auswerten.

## <a name="review"></a>Überprüfung

Dieser Artikel beschreibt den allgemeinen Workflow für Policy-as-Code und erläutert, in welchen Fällen eine Richtlinienauswertung Teil anderer Bereitstellungsworkflows sein sollte. Dieser Workflow kann in jeder Umgebung eingesetzt werden, die skriptbasierte Schritte und Automatisierung basierend auf Triggern unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Struktur von Richtliniendefinitionen](./definition-structure.md).
- Erfahren Sie mehr über die [Struktur von Richtlinienzuweisungen](./assignment-structure.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).