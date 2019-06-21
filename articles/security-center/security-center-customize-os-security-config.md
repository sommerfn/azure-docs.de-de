---
title: Anpassen der Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center (Vorschau) | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie Security Center-Bewertungen anpassen.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: d9dc9f3a4ab964b66e3a2cb03f4aad442c5665e9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573607"
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>Anpassen der Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center (Vorschau)

Diese exemplarische Vorgehensweise veranschaulicht, wie Sie Bewertungen für Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center anpassen.

> [!NOTE]
> Die Möglichkeit zum Anpassen von Sicherheitskonfigurationen von Betriebssystemen wird am 31. Juli 2019 eingestellt. Weitere Informationen und alternative Dienste finden Sie unter [Auslaufen von Security Center-Funktionen (Juli 2019)](security-center-features-retirement-july2019.md#menu_securityconfigurations).

## <a name="what-are-os-security-configurations"></a>Was sind Sicherheitskonfigurationen von Betriebssystemen?

Azure Security Center überwacht Sicherheitskonfigurationen durch Anwenden von [über 150 empfohlenen Regeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) für die Härtung des Betriebssystems, einschließlich Regeln für Firewalls, Überwachung, Kennwortrichtlinien und mehr. Wenn auf einem Computer eine angreifbare Konfiguration festgestellt wird, generiert Security Center eine Sicherheitsempfehlung.

Durch Anpassen der Regeln können Organisationen steuern, welche Konfigurationsoptionen sich für ihre Umgebung besser eignen. Sie können eine benutzerdefinierte Bewertungsrichtlinie einrichten und diese auf alle passenden Computer im Abonnement anwenden.

> [!NOTE]
> - Derzeit steht die Anpassung der Sicherheitskonfiguration von Betriebssystemen nur für die Windows Server-Versionen 2008, 2008 R2, 2012, 2012 R2 und 2016 zur Verfügung.
> - Die Konfiguration gilt für alle VMs und Computer, die mit allen Arbeitsbereichen im ausgewählten Abonnement verbunden sind.
> - Die Anpassung der Sicherheitskonfiguration von Betriebssystemen ist nur im Security Center-Tarif „Standard“ verfügbar.
>
>

Sie können die Regeln der Sicherheitskonfiguration von Betriebssystemen anpassen, indem Sie eine bestimmte Regel aktivieren und deaktivieren, die gewünschte Einstellung für eine bestehende Regel ändern oder basierend auf den unterstützten Regeltypen (Registrierung, Überwachungsrichtlinie und Sicherheitsrichtlinie) eine neue Regel hinzufügen. Derzeit muss die gewünschte Einstellung ein genauer Wert sein.

Neue Regeln müssen das gleiche Format und die gleiche Struktur aufweisen wie andere vorhandene Regeln desselben Typs.

> [!NOTE]
> Um Sicherheitskonfigurationen von Betriebssystemen anpassen zu können, muss Ihnen die Rolle *Abonnementbesitzer*, *Mitwirkender am Abonnement* oder *Sicherheitsadministrator* zugewiesen sein.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Anpassen der standardmäßigen Sicherheitskonfiguration von Betriebssystemen

Um die standardmäßige Sicherheitskonfiguration von Betriebssystemen in Security Center anzupassen, gehen Sie folgendermaßen vor:

1.  Öffnen Sie das Dashboard **Security Center**.

2.  Wählen Sie im linken Bereich **Sicherheitsrichtlinie**.      

    ![Sicherheitsrichtlinie – Liste](media/security-center-customize-os-security-config/manual-provision.png)

3.  Klicken Sie in der Zeile des Abonnements, das Sie anpassen möchten, auf **Einstellungen bearbeiten**.

4. Wählen Sie **Sicherheitskonfigurationen bearbeiten** aus.  

    ![Das Fenster „Sicherheitskonfigurationen bearbeiten“](media/security-center-customize-os-security-config/blade.png)

5. Befolgen Sie die Schritte zum Herunterladen, Bearbeiten und Hochladen der geänderten Datei.

   > [!NOTE]
   > Standardmäßig hat die Konfigurationsdatei, die Sie herunterladen, das *JSON*-Format. Anleitungen zum Ändern dieser Datei finden Sie unter [Anpassen der Konfigurationsdatei](#customize-the-configuration-file).
   >

6. Um die Änderung zu übernehmen, wählen Sie **Speichern** aus. Andernfalls wird die Richtlinie nicht gespeichert.

    ![Die Schaltfläche „Speichern“](media/security-center-customize-os-security-config/save-successfully.png)

   Nachdem Sie die Datei erfolgreich gespeichert haben, wird die Konfiguration auf alle VMs und Computer angewandt, die mit den Arbeitsbereichen im Abonnement verbunden sind. Der Vorgang nimmt üblicherweise nur wenige Minuten in Anspruch, kann aber je nach Größe der Infrastruktur auch länger dauern.

Sie können die aktuelle Richtlinienkonfiguration jederzeit auf den Standardzustand zurücksetzen. Wählen Sie zu diesem Zweck im Fenster **Regeln für Betriebssystem-Sicherheitskonfiguration bearbeiten** die Option **Zurücksetzen** aus. Bestätigen Sie den Vorgang, indem Sie im Popupfenster zur Bestätigung **Ja** auswählen.

![Bestätigungsfenster für die Zurücksetzung](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Anpassen der Konfigurationsdatei

Die Anpassungsdatei enthält einen Regelsatz für jede unterstützte Betriebssystemversion. Jeder Regelsatz weist einen eigenen Namen und eine eindeutige ID auf, wie das folgende Beispiel zeigt:

![Name und ID des Regelsatzes](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Die Beispieldatei wurde in Visual Studio bearbeitet, aber Sie können auch Editor verwenden, wenn das JSON-Viewer-Plug-In installiert ist.
>
>

Wenn Sie die Anpassungsdatei bearbeiten, können Sie eine Regel oder alle Regeln ändern. Jeder Regelsatz enthält den Abschnitt *rules*, der in drei Kategorien unterteilt ist: Registrierung, Überwachungsrichtlinie und Sicherheitsrichtlinie, wie hier zu sehen:

![Drei Regelsatzkategorien](media/security-center-customize-os-security-config/rules-section.png)

Jede Kategorie hat einen eigenen Satz von Attributen. Sie können die folgenden Attribute ändern:

- **expectedValue**: Der Felddatentyp dieses Attributs muss mit den unterstützten Werten jedes *Regeltyps* übereinstimmen, beispielsweise:

  - **baselineRegistryRules**: Der Wert muss dem in dieser Regel definierten Element [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) entsprechen.

  - **baselineAuditPolicyRules**: Verwenden Sie einen der folgenden Zeichenfolgenwerte:

    - *Erfolgreich und fehlerhaft*

    - *Erfolgreich*

  - **baselineSecurityPolicyRules**: Verwenden Sie einen der folgenden Zeichenfolgenwerte:

    - *Keiner*

    - Liste der zulässigen Benutzergruppen. Beispiele: *Administratoren*, *Sicherungsoperatoren*

-   **state**: Diese Zeichenfolge kann die Option *Disabled* (Deaktiviert) oder *Enabled* (Aktiviert) enthalten. Bei dieser Version wird für die Zeichenfolge Groß-/Kleinschreibung beachtet.

Dies sind die einzigen Felder, die konfiguriert werden können. Wenn Sie gegen das Dateiformat oder die Dateigröße verstoßen, können Sie die Änderung nicht speichern. Sie werden in einer Fehlermeldung darüber informiert, dass Sie eine gültige JSON-Konfigurationsdatei hochladen müssen.

Eine Liste weiterer möglicher Fehler finden Sie unter [Fehlercodes](#error-codes).

Die folgenden drei Abschnitte enthalten Beispiele für die oben genannten Regeln. Die Attribute *expectedValue* und *state* können geändert werden.

**baselineRegistryRules**
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```json
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```json
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Einige Regeln sind für die verschiedenen Betriebssystemtypen dupliziert. Duplizierte Regeln weisen das gleiche *originalId*-Attribut auf.

## <a name="create-custom-rules"></a>Benutzerdefinierte Regeln erstellen

Sie können auch neue Regeln erstellen. Bevor Sie eine neue Regel erstellen, beachten Sie die folgenden Einschränkungen:

-   Schemaversion, *baselineId* und *baselineName* können nicht geändert werden.

-   Der Regelsatz kann nicht entfernt werden.

-   Der Regelsatz kann nicht hinzugefügt werden.

-   Die maximale zulässige Anzahl von Regeln (einschließlich Standardregeln) ist 1000.

Neue benutzerdefinierte Regeln werden mit einer neuen benutzerdefinierten Quelle markiert (! = "Microsoft"). Das Feld *ruleId* kann NULL oder leer sein. Wenn es leer ist, wird es von Microsoft generiert. Wenn es nicht leer ist, muss es eine gültige GUID enthalten, die für alle (standardmäßigen und benutzerdefinierten) Regeln eindeutig ist. Lesen Sie die folgenden Einschränkungen für die wichtigsten Felder:

-   **originalId**: Kann NULL oder leer sein. Wenn *originalId* nicht leer ist, muss dies eine gültige GUID sein.

-   **cceId**: Kann NULL oder leer sein. Wenn *cceId* ist nicht leer ist, muss der Wert eindeutig sein.

-   **ruleType**: „Registry“, „AuditPolicy“ oder „SecurityPolicy“ (wählen Sie einen Wert aus).

-   **severity**: „Unbekannt“, „Kritisch“, „Warnung“ oder „Information“ (wählen Sie einen Wert aus).

-   **analyzeOperation**: Muss *Equals* (Gleich) lauten.

-   **auditPolicyId**: Muss eine gültige GUID sein.

-   **regValueType**: „Int“, „Long“, „String“ oder „MultipleString“ (wählen Sie einen Wert aus).

> [!NOTE]
> „Hive“ muss *LocalMachine* sein.
>
>

Beispiel einer neuen benutzerdefinierten Regel:

**Registrierung**:
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule", "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Sicherheitsrichtlinie**:
```json
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Überwachungsrichtlinie**:
```json
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Fehler beim Hochladen von Dateien

Wenn die übermittelte Konfigurationsdatei aufgrund von Fehlern bei Werten oder Formatierungen ungültig ist, wird ein Fehler angezeigt, etwa **Fehler bei Aktion zum Speichern**. Sie können einen detaillierten Fehlerbericht im CSV-Format herunterladen, um Fehler zu korrigieren, bevor Sie die korrigierte Konfigurationsdatei erneut übermitteln.

Beispiel einer Fehlerdatei:

![Beispiel für Fehlerdatei](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Fehlercodes

Alle potenziellen Fehler sind in der folgenden Tabelle aufgeführt:

| **Fehler**                                | **Beschreibung**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | Die Eigenschaft *schemaVersion* war ungültig oder leer. Der Wert muss auf *{0}* festgelegt werden.                                                         |
| BaselineInvalidStringError               | Die Eigenschaft *{0}* darf nicht *\\n* enthalten.                                                                                                         |
| BaselineNullRuleError                    | Die Liste der Regeln für die Baselinekonfiguration enthält eine Regel mit dem Wert *NULL*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | Die CCE-ID *{0}* ist nicht eindeutig.                                                                                                                  |
| BaselineRuleEmptyProperty                | Die Eigenschaft *{0}* fehlt oder ist ungültig.                                                                                                       |
| BaselineRuleIdNotInDefault               | Die Regel weist die Quelleigenschaft *Microsoft* auf, wurde aber im Standardregelsatz von Microsoft nicht gefunden.                                                   |
| BaselineRuleIdNotUniqueError             | Der Regel-Bezeichner ist nicht eindeutig.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Die Eigenschaft *{0}* wurde als ungültig ermittelt. Der Wert ist keine gültige GUID.                                                                             |
| BaselineRuleInvalidHive                  | „Hive“ muss „LocalMachine“ sein.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Der Regelname ist nicht eindeutig.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Die Regel wurde in der neuen Konfiguration nicht gefunden. Regel kann nicht gelöscht werden.                                                                     |
| BaselineRuleNotFoundError                | Die Regel wurde im standardmäßigen Baselineregelsatz nicht gefunden.                                                                                        |
| BaselineRuleNotInPlace                   | Die Regel entspricht einer Standardregel des Typs {0} und ist in der Liste {1} aufgeführt.                                                                       |
| BaselineRulePropertyTooLong              | Die Eigenschaft *{0}* ist zu lang. Maximal zulässige Länge: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Der erwartete Wert *{0}* stimmt nicht mit dem definierten Registrierungswerttyp überein.                                                              |
| BaselineRulesetAdded                     | Der Regelsatz mit dem Bezeichner *{0}* wurde in der Standardkonfiguration nicht gefunden. Der Regelsatz kann nicht hinzugefügt werden.                                               |
| BaselineRulesetIdMustBeUnique            | Der angegebene Baselineregelsatz *{0}* muss eindeutig sein.                                                                                           |
| BaselineRulesetNotFound                  | Der Regelsatz mit dem Bezeichner *{0}* und dem Namen *{1}* wurde in der angegebenen Konfiguration nicht gefunden. Der Regelsatz kann nicht gelöscht werden.                                |
| BaselineRuleSourceNotMatch               | Die Regel mit dem Bezeichner *{0}* ist bereits definiert.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Der Standardregeltyp lautet *{0}* .                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Der tatsächliche Typ der Regel ist *{0}* , aber die *ruleType*-Eigenschaft ist *{1}* .                                                                          |
| BaselineRuleUnpermittedChangesError      | Nur die Eigenschaften *expectedValue* und *state* dürfen geändert werden.                                                                       |
| BaselineTooManyRules                     | Die maximal zulässige Anzahl angepasster Regeln ist {0}. Die angegebene Konfiguration enthält {1} Regeln, {2} Standardregeln und {3} angepasste Regeln. |
| ErrorNoConfigurationStatus               | Es wurde kein Konfigurationsstatus gefunden. Geben Sie den gewünschten Konfigurationsstatus an: *Standard* oder *Benutzerdefiniert*.                                    |
| ErrorNonEmptyRulesetOnDefault            | Der Konfigurationsstatus ist auf den Standardwert festgelegt. Die Liste *BaselineRulesets* muss NULL oder leer sein.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Der angegebene Konfigurationsstatus ist *Benutzerdefiniert*, aber die Eigenschaft *baselineRulesets* ist NULL oder leer.                                             |
| ErrorParsingBaselineConfig               | Die angegebene Konfiguration ist ungültig. Mindestens einer der definierten Werte weist einen NULL-Wert oder einen ungültigen Typ auf.                                  |
| ErrorParsingIsDefaultProperty            | Der angegebene *configurationStatus*-Wert *{0}* ist ungültig. Der Wert kann nur *Standard* oder *Benutzerdefiniert* lauten.                                         |
| InCompatibleViewVersion                  | Die Ansichtsversion *{0}* wird für diesen Arbeitsbereichstyp *nicht* unterstützt.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Die angegebene Baselinekonfiguration wies mindestens einen Typvalidierungsfehler auf.                                                          |
| ViewConversionError                      | Die Ansicht liegt in einer älteren Version vor als vom Arbeitsbereich unterstützt. Fehler bei der Ansichtskonvertierung: {0}.                                                                 |

Wenn Sie nicht über ausreichende Berechtigungen verfügen, erhalten Sie möglicherweise einen allgemeinen Fehler, wie hier gezeigt:

![Meldung zu Fehler bei Speicheraktion](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Sie Bewertungen für Sicherheitskonfigurationen von Betriebssystemen in Azure Security Center anpassen. Weitere Informationen zu den Konfigurationsregeln und ihrer Korrektur finden Sie unter:

- [Security Center Common Configuration Identifiers and Baseline Rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) (Security Center: Allgemeine Konfigurationsbezeichner und Baselineregeln).
- Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen. Weitere Informationen finden Sie unter [CCE](https://nvd.nist.gov/config/cce/index).
- Unter [Korrigieren von Sicherheitskonfigurationen](security-center-remediate-os-vulnerabilities.md) erfahren Sie, wie Sie Sicherheitsrisiken beheben können, wenn Ihre Betriebssystemkonfiguration nicht den empfohlenen Sicherheitskonfigurationsregeln entspricht.
