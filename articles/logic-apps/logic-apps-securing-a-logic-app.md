---
title: Schützen des Zugriffs und der Daten – Azure Logic Apps
description: Schützen Sie Parametereingaben, HTTP-Anforderungstrigger, den Ausführungsverlauf, Logik-App-Vorgänge und Verbindungen mit anderen Diensten in Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: c7b6abb9874c8b6bb2469595b9ce53541656834f
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430130"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Schützen des Zugriffs und der Daten in Azure Logic Apps

Um den Zugriff zu steuern und Daten in Azure Logic Apps zu schützen, können Sie die Sicherheit in diesen Bereichen einrichten:

* [Zugriff auf HTTP-Anforderungstrigger](#secure-triggers)
* [Zugriff auf Logik-App-Vorgänge](#secure-operations)
* [Zugriff auf Eingaben und Ausgaben von Ausführungsverläufen](#secure-run-history)
* [Zugriff auf Parametereingaben](#secure-action-parameters)
* [Zugriff auf Dienste und Systeme, die von Logik-Apps aus aufgerufen werden](#secure-requests)

<a name="secure-triggers"></a>

## <a name="access-to-http-request-triggers"></a>Zugriff auf HTTP-Anforderungstrigger

Wenn Ihre Logik-App einen auf HTTP-Anforderungen basierenden Trigger verwendet, z. B. den Trigger [Anforderung](../connectors/connectors-native-reqres.md) oder [Webhook](../connectors/connectors-native-webhook.md), können Sie den Zugriff so einschränken, dass nur autorisierte Clients die Logik-App starten können. Alle von einer Logik-App empfangenen Anforderungen werden mit dem Secure Sockets Layer-Protokoll (SSL) verschlüsselt und gesichert. 

Zum Absichern des Zugriffs auf diesen Triggertyp gibt es folgende Möglichkeiten:

* [Generieren von Shared Access Signatures](#sas)
* [Beschränken eingehender IP-Adressen](#restrict-incoming-ip-addresses)
* [Hinzufügen von Azure Active Directory, OAuth oder anderen Sicherheitsfeatures](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Generieren von Shared Access Signatures (SAS)

Jeder Anforderungsendpunkt für eine Logik-App weist in der URL des Endpunkts eine [Shared Access Signature (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) im folgenden Format auf:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Jede URL enthält die Abfrageparameter `sp`, `sv` und `sig`, wie in dieser Tabelle beschrieben:

| Query parameter (Abfrageparameter) | BESCHREIBUNG |
|-----------------|-------------|
| `sp` | Gibt die Berechtigungen für die zulässigen HTTP-Methoden an. |
| `sv` | Gibt die SAS-Version an, die zum Generieren der Signatur verwendet werden soll. |
| `sig` | Gibt die Signatur an, die für die Authentifizierung des Zugriffs auf den Trigger verwendet werden soll. Diese Signatur wird mit dem SHA256-Algorithmus mit einem geheimen Zugriffsschlüssel für alle URL-Pfade und -Eigenschaften generiert. Dieser Schlüssel wird nie verfügbar gemacht oder veröffentlicht. Er bleibt verschlüsselt und wird mit der Logik-App gespeichert. Die Logik-App autorisiert nur Trigger, die eine gültige, mit dem geheimen Schlüssel erstellte Signatur enthalten. |
|||

Weitere Informationen zum Absichern des Zugriffs mit einer Shared Access Signature finden Sie in den folgenden Abschnitten in diesem Thema:

* [Erneutes Generieren von Zugriffsschlüsseln](#access-keys)
* [Erstellen von ablaufenden Rückruf-URLs](#expiring-urls)
* [Erstellen von URLs mit Primär- oder Sekundärschlüssel](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

Über die Azure-REST-API oder das Azure-Portal können Sie jederzeit einen neuen sicheren Zugriffsschlüssel generieren. Alle zuvor generierten URLs, die den alten Schlüssel verwenden, werden ungültig und sind nicht mehr berechtigt, die Logik-App auszulösen. Die URLs, die Sie nach der erneuten Generierung abrufen, werden mit dem neuen Zugriffsschlüssel signiert.

1. Öffnen Sie im Azure-Portal die Logik-App mit dem Schlüssel, den Sie erneut generieren möchten.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Zugriffsschlüssel** aus.

1. Wählen Sie den Schlüssel aus, den Sie erneut generieren möchten, und schließen Sie den Vorgang ab.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Erstellen ablaufender Rückruf-URLs

Wenn Sie die Endpunkt-URL eines auf HTTP-Anforderungen basierenden Triggers für andere Parteien freigeben, können Sie Rückruf-URLs mit bestimmten Schlüsseln und Ablaufdaten generieren. So können Sie nahtlos rollierende Schlüssel bereitstellen oder den Zugriff für das Auslösen Ihrer Logik-App auf einen bestimmten Zeitraum einschränken. Über die [Logic Apps-REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) können Sie ein Ablaufdatum für eine URL angeben. Beispiel:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Verwenden Sie im Text die `NotAfter`-Eigenschaft mit einer JSON-Datumszeichenfolge. Diese Eigenschaft gibt eine Rückruf-URL zurück, die nur bis zum Datum und der Uhrzeit in `NotAfter` gültig ist.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Erstellen von URLs mit einem primären oder sekundären geheimen Schlüssel

Beim Generieren oder Auflisten von Rückruf-URLs für auf HTTP-Anforderungen basierende Trigger können Sie den Schlüssel zum Signieren der URL angeben. Um eine URL zu generieren, die von einem bestimmten Schlüssel signiert wird, verwenden Sie die [Logic Apps-REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers). Beispiel:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Schließen Sie in den Textkörper die `KeyType`-Eigenschaft als `Primary` oder als `Secondary` ein. Diese Eigenschaft gibt eine URL zurück, die mit dem angegebenen sicheren Schlüssel signiert ist.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Beschränken eingehender IP-Adressen

Zusätzlich zur Shared Access Signature empfiehlt es sich, die Clients einzuschränken, die Ihre Logik-App aufrufen können. Beispiel: Wenn Sie den Anforderungsendpunkt mit Azure API Management verwalten, können Sie für die Logik-App festlegen, dass sie Anforderungen nur von der IP-Adresse der API Management-Instanz annimmt.

#### <a name="restrict-incoming-ip-ranges-in-azure-portal"></a>Einschränken eingehender IP-Adressbereiche im Azure-Portal

1. Öffnen Sie die Logik-App im Azure-Portal im Logik-App-Designer.

1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Workfloweinstellungen** aus.

1. Wählen Sie unter **Konfiguration der Zugriffssteuerung** > **Zulässige eingehende IP-Adressen** die Option **Spezifische IP-Bereiche** aus.

1. Geben Sie unter **IP-Bereiche für Trigger** die IP-Adressbereiche an, die der Trigger akzeptiert.

   Gültige IP-Adressbereiche verwenden die Formate *x.x.x.x/x* oder *x.x.x.x-x.x.x.x*.

Wenn Sie Ihre Logik-App nur als geschachtelte Logik-App auslösen möchten, wählen Sie in der Liste **Zulässige eingehende IP-Adressen** die Option **Nur andere Logik-Apps** aus. Diese Option schreibt ein leeres Array in Ihre Logik-App-Ressource. Auf diese Weise können nur Aufrufe über den Logic Apps-Dienst (übergeordnete Logik-Apps) die geschachtelte Logik-App auslösen.

> [!NOTE]
> Unabhängig von der IP-Adresse können Sie eine Logik-App, die einen auf HTTP-Anforderungen basierenden Trigger aufweist, weiterhin mithilfe von `/triggers/<trigger-name>/run` über die Azure-REST-API oder über API Management ausführen. In diesem Szenario ist jedoch weiterhin eine Authentifizierung über die Azure-REST-API erforderlich. Alle Ereignisse werden im Azure-Überwachungsprotokoll angezeigt. Achten Sie darauf, die Richtlinien für die Zugriffssteuerung entsprechend festzulegen.

#### <a name="restrict-incoming-ip-ranges-in-azure-resource-manager-template"></a>Einschränken eingehender IP-Adressbereich in der Azure Resource Manager-Vorlage

Wenn Sie die Bereitstellung von Logik-Apps mithilfe einer [Azure Resource Manager-Vorlage](../logic-apps/logic-apps-create-deploy-template.md) automatisieren, können Sie die IP-Adressbereiche angeben, indem Sie beispielsweise den Abschnitt `accessControl` mit dem Abschnitt `triggers` in der Ressourcendefinition Ihrer Logik-App verwenden:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Hinzufügen von Azure Active Directory, OAuth oder anderen Sicherheitsfeatures

Um Ihrer Logik-App weitere Autorisierungsprotokolle hinzuzufügen, können Sie [Azure API Management](../api-management/api-management-key-concepts.md) verwenden. Dieser Dienst bietet Ihnen die Möglichkeit, Ihre Logik-App als API verfügbar zu machen. Er bietet außerdem umfassende Überwachung, Sicherheit, Richtlinien und Dokumentation für alle Endpunkte. API Management kann einen öffentlichen oder privaten Endpunkt für die Logik-App verfügbar machen. Dadurch können Azure Active Directory, OAuth, Zertifikate oder andere Sicherheitsstandards genutzt werden. Wenn API Management eine Anforderung empfängt, sendet der Dienst die Anforderung an Ihre Logik-App und setzt dazu alle erforderlichen Transformationen oder Einschränkungen um. Damit nur API Management Ihre Logik-App auslösen kann, können Sie die Einstellungen für den eingehenden IP-Bereich Ihrer Logik-App anpassen.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Zugriff auf Logik-App-Vorgänge

Sie können nur bestimmten Benutzern oder Gruppen erlauben, bestimmte Operationen auszuführen, wie z. B. das Verwalten, Bearbeiten und Anzeigen von Logik-Apps. Um die jeweiligen Berechtigungen zu steuern, verwenden Sie die [rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/role-assignments-portal.md) (Role-Based Access Control, RBAC), um den Mitgliedern in Ihrem Azure-Abonnement benutzerdefinierte oder integrierte Rollen zuzuweisen:

* [Logik-App-Mitwirkender:](../role-based-access-control/built-in-roles.md#logic-app-contributor) Ermöglicht Ihnen die Verwaltung von Logik-Apps, aber nicht die Änderung des App-Zugriffs.

* [Logik-App-Operator:](../role-based-access-control/built-in-roles.md#logic-app-operator) Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps, die Sie aber nicht bearbeiten oder aktualisieren können.

Um zu verhindern, dass andere Personen Ihre Logik-App ändern oder löschen, können Sie die [Azure-Ressourcensperre](../azure-resource-manager/resource-group-lock-resources.md) verwenden. Diese verhindert, dass andere Personen Produktionsressourcen ändern oder löschen.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Zugriff auf Ausführungsverlaufsdaten

Während der Ausführung einer Logik-App werden alle Daten [bei der Übertragung](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) mithilfe von Transport Layer Security (TLS) sowie [im Ruhezustand](../security/fundamentals/encryption-atrest.md) verschlüsselt. Wenn Ihre Logik-App die Ausführung beendet hat, können Sie den Verlauf für diese Ausführung anzeigen, einschließlich der ausgeführten Schritte sowie Status, Dauer, Eingaben und Ausgaben für die einzelnen Aktionen. Diese umfangreichen Informationen geben einen Einblick in die Funktionsweise Ihrer Logik-App und zeigen, wo Sie bei der Problembehandlung ansetzen können.

Wenn Sie auf den Ausführungsverlauf einer Logik-App zugreifen, authentifiziert Logic Apps den Zugriff und stellt Links zu den Eingaben und Ausgaben aus den Anforderungen und Antworten in der Ausführung Ihrer Logik-App bereit. Bei Aktionen, die Kennwörter, Geheimnisse, Schlüssel oder andere sensible Informationen verarbeiten, sollten Sie jedoch verhindern, dass andere Personen diese Daten einsehen und darauf zugreifen. Wenn Ihre Logik-App beispielsweise ein Geheimnis aus [Azure Key Vault](../key-vault/key-vault-overview.md) erhält, das bei der Authentifizierung einer HTTP-Aktion verwendet werden soll, sollten Sie dieses Geheimnis ausblenden.

Um den Zugriff auf die Ein- und Ausgaben im Ausführungsverlauf Ihrer Logik-App zu steuern, stehen Ihnen folgende Optionen zur Verfügung:

* [Beschränken des Zugriffs nach IP-Adressbereich](#restrict-ip).

  Mit dieser Option können Sie den Zugriff auf den Ausführungsverlauf basierend auf den Anforderungen aus einem bestimmten IP-Adressbereich schützen.

* [Ausblenden von Daten im Ausführungsverlauf mittels Obfuskation.](#obfuscate)

  In vielen Triggern und Aktionen können Sie Eingaben, Ausgaben oder beides im Ausführungsverlauf einer Logik-App ausblenden.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Beschränken des Zugriffs nach IP-Adressbereich

Sie können den Zugriff auf die Ein- und Ausgaben im Ausführungsverlauf Ihrer Logik-App so einschränken, dass diese Daten nur für Anforderungen aus bestimmten IP-Adressbereichen einsehbar sind. Um beispielsweise den Zugriff auf Ein- und Ausgaben zu verhindern, geben Sie einen IP-Adressbereich wie z. B. `0.0.0.0-0.0.0.0` an. Nur Personen mit Administratorberechtigungen können diese Einschränkung entfernen und erhalten damit die Möglichkeit für einen Just-In-Time-Zugriff auf die Daten Ihrer Logik-App. Sie können die einzuschränkenden IP-Bereiche entweder über das Azure-Portal oder in einer Azure Resource Manager-Vorlage angeben, die Sie für die Bereitstellung von Logik-Apps verwenden.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Einschränken von IP-Adressbereichen im Azure-Portal

1. Öffnen Sie die Logik-App im Azure-Portal im Logik-App-Designer.

1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Workfloweinstellungen** aus.

1. Wählen Sie unter **Konfiguration der Zugriffssteuerung** > **Zulässige eingehende IP-Adressen** die Option **Spezifische IP-Bereiche** aus.

1. Geben Sie unter **IP-Bereiche für Inhalte** die IP-Adressbereiche an, die auf Inhalte von Eingaben und Ausgaben zugreifen können. 

   Gültige IP-Adressbereiche verwenden die Formate *x.x.x.x/x* oder *x.x.x.x-x.x.x.x*.

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Einschränken von IP-Adressbereichen in einer Azure Resource Manager-Vorlage

Wenn Sie die Bereitstellung von Logik-Apps mithilfe einer [Azure Resource Manager-Vorlage](../logic-apps/logic-apps-create-deploy-template.md) automatisieren, können Sie die IP-Adressbereiche angeben, indem Sie den Abschnitt `accessControl` mit dem Abschnitt `contents` in der Ressourcendefinition Ihrer Logik-App verwenden. Beispiel:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="hide-data-from-run-history-by-using-obfuscation"></a>Ausblenden von Daten im Ausführungsverlauf mittels Obfuskation

Bei vielen Triggern und Aktionen stehen Einstellungen zur Verfügung, um Eingaben, Ausgaben oder beides im Ausführungsverlauf einer Logik-App auszublenden. Wenn Sie diese Einstellungen verwenden, um entsprechende Daten zu schützen, müssen [einige Aspekte](#obfuscation-considerations) berücksichtigt werden.

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Schützen von Ein- und Ausgaben im Designer

1. Wenn Ihre Logik-App noch nicht im [Azure-Portal](https://portal.azure.com) geöffnet ist, öffnen Sie sie im Logik-App-Designer.

   ![Öffnen einer Beispiel-Logik-App](media/logic-apps-securing-a-logic-app/sample-logic-app.png)

1. Wählen Sie für den Trigger oder die Aktion zum Schützen von Daten die Schaltfläche mit den Auslassungspunkten ( **...** ) und dann **Einstellungen** aus.

   ![Öffnen der Einstellungen](media/logic-apps-securing-a-logic-app/open-settings.png)

1. Aktivieren Sie entweder **Sichere Eingaben**, **Sichere Ausgaben** oder beides. Klicken Sie auf **Fertig**, wenn Sie fertig sind.

   ![Aktivieren sicherer Eingaben oder Ausgaben](media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Die Aktion oder der Trigger zeigt jetzt ein Schlosssymbol in der Titelleiste an.

   ![Schlosssymbol in der Titelleiste](media/logic-apps-securing-a-logic-app/title-bar-lock-icon.png)

   Token, die sichere Ausgaben aus früheren Aktionen darstellen, zeigen ebenfalls Schlosssymbole an. Wenn Sie beispielsweise eine solche Ausgabe aus der Liste mit dynamischen Inhalten für eine Aktion auswählen, zeigt das entsprechende Token ein Schlosssymbol an.

   ![Auswählen der Ausgabe](media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Nachdem die Logik-App ausgeführt wurde, können Sie den Verlauf für diese Ausführung anzeigen.

   1. Wählen Sie im Bereich **Übersicht** der Logik-App die Ausführung aus, die Sie anzeigen möchten.

   1. Erweitern Sie im Bereich **Logik-App-Ausführung** die Aktionen, die Sie überprüfen möchten.

      Wenn Sie sowohl die Ein- als auch die Ausgaben für die Absicherung ausgewählt haben, werden diese Werte jetzt ausgeblendet.

      ![Ausgeblendete Daten im Ausführungsverlauf](media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Schützen von Ein- und Ausgaben in der Codeansicht

Fügen Sie in der zugrunde liegenden Trigger- oder Aktionsdefinition das Array `runtimeConfiguration.secureData.properties` mit einem der folgenden Werte (oder mit beiden Werten) hinzu, oder aktualisieren Sie es entsprechend:

* `"inputs"`: Schützt Eingaben im Ausführungsverlauf.
* `"outputs"`: Schützt Ausgaben im Ausführungsverlauf.

Wenn Sie diese Einstellungen verwenden, um entsprechende Daten zu schützen, müssen [einige Aspekte](#obfuscation-considerations) berücksichtigt werden.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-hiding-inputs-and-outputs"></a>Überlegungen im Zusammenhang mit dem Ausblenden von Ein- und Ausgaben

* Wenn Sie die Ein- oder Ausgaben für einen Trigger oder eine Aktion schützen, sendet Logic Apps die geschützten Daten nicht an Azure Log Analytics. Außerdem können Sie diesem Auslöser oder dieser Aktion keine [nachverfolgten Eigenschaften](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details) zur Überwachung hinzufügen.

* Die [Logic Apps-API zur Verarbeitung des Workflowverlaufs](https://docs.microsoft.com/rest/api/logic/) gibt keine sicheren Ausgaben zurück.

* Wenn eine Aktion, bei der Sie Eingaben schützen oder die explizit sichere Ausgaben verwendet, eine Antwort mit Ausgaben zurückgibt, die diese geschützten Daten enthalten, müssen Sie in dieser Aktion manuell **Sichere Ausgaben** aktivieren, um diese Ausgaben zu schützen.

* Stellen Sie sicher, dass Sie **Sichere Eingaben** oder **Sichere Ausgaben** in nachfolgenden Aktionen aktivieren, bei denen Sie erwarten, dass die Daten im Ausführungsverlauf geschützt werden.

  **Einstellung „Sichere Ausgaben“**

  Wenn Sie **Sichere Ausgaben** in einem Trigger oder einer Aktion manuell aktivieren, schützt Logic Apps diese Ausgaben im Ausführungsverlauf. Wenn eine nachfolgende Aktion diese sicheren Ausgaben explizit als Eingaben verwendet, blendet Logic Apps die Eingaben dieser Aktion im Ausführungsverlauf aus, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** der Aktion.

  ![Sichere Ausgaben als Eingaben und nachgeschaltete Auswirkungen auf die meisten Aktionen](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Die Aktionen „Erstellen“, „JSON analysieren“ und „Antwort“ weisen nur die Einstellung **Sichere Eingaben** auf. Wenn diese aktiviert wird, blendet diese Einstellung auch die Ausgaben dieser Aktionen aus. Wenn diese Aktionen explizit die sicheren Upstreamausgaben als Eingaben verwenden, blendet Logic Apps die Ein- und Ausgaben dieser Aktionen aus, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** dieser Aktionen. Wenn eine nachfolgende Aktion explizit die ausgeblendeten Ausgaben der Aktionen „Erstellen“, „JSON analysieren“ oder „Antwort“ als Eingaben verwendet, *blendet Logic Apps die Ein- oder Ausgaben dieser nachfolgenden Aktion nicht aus*.

  ![Sichere Ausgaben als Eingaben mit nachgeschalteten Auswirkungen auf bestimmte Aktionen](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Einstellung „Sichere Eingaben“**

  Wenn Sie **Sichere Eingaben** in einem Trigger oder einer Aktion manuell aktivieren, schützt Logic Apps diese Eingaben im Ausführungsverlauf. Wenn eine nachfolgende Aktion explizit die sichtbaren Ausgaben dieses Triggers oder dieser Aktion als Eingaben verwendet, blendet Logic Apps die Eingaben dieser nachfolgenden Aktion im Ausführungsverlauf aus, *aktiviert aber nicht*  die Option **Sichere Eingaben** in dieser Aktion und blendet die Ausgaben dieser Aktion nicht aus.

  ![Sichere Eingaben und nachgeschaltete Auswirkungen auf die meisten Aktionen](media/logic-apps-securing-a-logic-app/secure-inputs-flow.png)

  Wenn die Aktionen „Erstellen“, „JSON analysieren“ und „Antwort“ explizit die sichtbaren Ausgaben des Triggers oder der Aktion mit den sicheren Eingaben verwenden, blendet Logic Apps die Ein- und Ausgaben dieser Aktionen, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** der jeweiligen Aktion. Wenn eine nachfolgende Aktion explizit die ausgeblendeten Ausgaben der Aktionen „Erstellen“, „JSON analysieren“ oder „Antwort“ als Eingaben verwendet, *blendet Logic Apps die Ein- oder Ausgaben dieser nachfolgenden Aktion nicht aus*.

  ![Sichere Eingaben und nachgeschaltete Auswirkungen auf bestimmte Aktionen](media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Zugriff auf Parametereingaben

Wenn Sie Bereitstellungen in verschiedenen Umgebungen durchführen, sollten Sie die Werte in Ihrer Workflowdefinition parametrisieren, die je nach Umgebung variieren. Auf diese Weise können Sie eine [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md#parameters) verwenden, um Ihre Logik-App bereitzustellen, sensible Informationen durch die Definition gesicherter Parameter zu schützen und diese Parametereingaben separat über die Parameter der Vorlage unter Verwendung einer [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) bereitzustellen.

Wenn Sie z. B. HTTP-Aktionen mit [Azure Active Directory](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication) authentifizieren, können Sie die Parameter definieren und absichern, die die Client-ID und das Clientgeheimnis für die Authentifizierung akzeptieren. Um diese Parameter für Ihre Logik-App zu definieren, verwenden Sie den Abschnitt `parameters` innerhalb der Workflowdefinition Ihrer Logik-App. Um die Parameterwerte zu schützen, die beim Bearbeiten der Logik-App oder beim Anzeigen des Ausführungsverlaufs nicht angezeigt werden sollen, können Sie Parameter des Typs `securestring` oder `secureobject` definieren und bei Bedarf codieren. Parameter dieses Typs werden nicht mit der Ressourcendefinition zurückgegeben und sind beim Anzeigen der Ressource nach der Bereitstellung nicht zugänglich. Um zur Laufzeit auf diese Parameterwerte zuzugreifen, verwenden Sie den Ausdruck `@parameters('<parameter-name>')` in Ihrer Workflowdefinition. Dieser Ausdruck wird nur zur Laufzeit ausgewertet und durch die [Workflowdefinitionssprache](../logic-apps/logic-apps-workflow-definition-language.md) beschrieben.

> [!NOTE]
> Wenn Sie einen Parameter im HTTP-Anforderungsheader oder -text verwenden, kann dieser Parameter sichtbar sein, wenn Sie den Ausführungsverlauf Ihrer Logik-App und die ausgehende HTTP-Anforderung anzeigen. Achten Sie darauf, Ihre Richtlinien für den Zugriff auf Inhalte entsprechend festzulegen. Autorisierungsheader sind nie über Eingaben oder Ausgaben sichtbar. Wenn hier ein Geheimnis verwendet wird, ist dieses daher nicht abrufbar.

Weitere Informationen finden Sie unter [Sichere Parameter in Workflowdefinitionen](#secure-parameters-workflow) weiter unten in diesem Thema.

Wenn Sie Bereitstellungen mit [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md#parameters) automatisieren, können Sie mithilfe der Typen `securestring` und `secureobject` sichere Vorlagenparameter definieren, die bei der Bereitstellung ausgewertet werden. Um Vorlagenparameter zu definieren, verwenden Sie den Abschnitt `parameters` auf der obersten Ebene Ihrer Vorlage, der vom Abschnitt `parameters` Ihrer Workflowdefinition getrennt ist und anders lautet. Um die Werte für Vorlagenparameter bereitzustellen, verwenden Sie eine separate [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values).

Wenn Sie beispielsweise Geheimnisse verwenden, können Sie sichere Vorlagenparameter definieren und verwenden, die diese Geheimnisse bei der Bereitstellung aus [Azure Key Vault](../key-vault/key-vault-overview.md) abrufen. Anschließend können Sie auf den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei verweisen. Weitere Informationen finden Sie in den folgenden Themen:

* [Verwenden von Azure Key Vault zum Übergeben sicherer Parameterwerte bei der Bereitstellung](../azure-resource-manager/resource-manager-keyvault-parameter.md)
* [Sichere Parameter in Azure Resource Manager-Vorlagen](#secure-parameters-deployment-template) weiter unten in diesem Thema

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Sichere Parameter in Workflowdefinitionen

Zum Schützen sensibler Informationen in der Workflowdefinition der Logik-App verwenden Sie sichere Parameter, damit diese Informationen nach dem Speichern der Logik-App nicht sichtbar sind. Angenommen, Sie verwenden eine HTTP-Aktion, die eine Standardauthentifizierung mit Benutzernamen und Kennwort erfordert. In der Workflowdefinition definiert der Abschnitt `parameters` die Parameter `basicAuthPasswordParam` und `basicAuthUsernameParam` über den Typ `securestring`. Die Aktionsdefinition verweist dann auf diese Parameter im Abschnitt `authentication`.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Sichere Parameter in Azure Resource Manager-Vorlagen

Eine Resource Manager-Vorlage für eine Logik-App enthält mehrere `parameters`-Abschnitte. Um Kennwörter, Schlüssel, Geheimnisse und andere sensible Informationen zu schützen, definieren Sie sichere Parameter auf Vorlagen- und Workflowdefinitionsebene mit dem Typ `securestring` oder `secureobject`. Sie können diese Werte dann in [Azure Key Vault](../key-vault/key-vault-overview.md) speichern und die [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) verwenden, um auf den Schlüsselspeicher und das Geheimnis zu verweisen. Ihre Vorlage ruft diese Informationen dann bei der Bereitstellung ab. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben sicherer Parameterwerte bei der Bereitstellung](../azure-resource-manager/resource-manager-keyvault-parameter.md).

Hier finden Sie weitere Informationen zu diesen `parameters`-Abschnitten:

* Auf der obersten Ebene der Vorlage definiert ein `parameters`-Abschnitt die Parameter für die Werte, die von der Vorlage bei der *Bereitstellung* verwendet werden. Diese Werte können beispielsweise Verbindungszeichenfolgen für eine bestimmte Bereitstellungsumgebung beinhalten. Sie können diese Werte dann in einer separaten [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) speichern, was das Ändern dieser Werte erleichtert.

* Innerhalb der Ressourcendefinition Ihrer Logik-App, aber außerhalb Ihrer Workflowdefinition, gibt der Abschnitt `parameters` die Werte für die Parameter Ihrer Workflowdefinition an. In diesem Abschnitt können Sie diese Werte anhand von Vorlagenausdrücken zuweisen, die auf die Parameter Ihrer Vorlage verweisen. Diese Ausdrücke werden bei der Bereitstellung ausgewertet.

* Innerhalb Ihrer Workflowdefinition definiert der Abschnitt `parameters` die Parameter, die von Ihrer Logik-App zur Runtime verwendet werden. Sie können auf diese Parameter dann innerhalb des Workflows Ihrer Logik-App verweisen, indem Sie Workflowdefinitionsausdrücke verwenden, die zur Laufzeit ausgewertet werden.

Diese Beispielvorlage weist mehrere sichere Parameterdefinitionen auf, die den Typ `securestring` verwenden:

| Parametername | BESCHREIBUNG |
|----------------|-------------|
| `TemplatePasswordParam` | Ein Vorlagenparameter, der ein Kennwort akzeptiert, das anschließend an den `basicAuthPasswordParam`-Parameter der Workflowdefinition übergeben wird |
| `TemplateUsernameParam` | Ein Vorlagenparameter, der einen Benutzernamen akzeptiert, der dann an den `basicAuthUserNameParam`-Parameter der Workflowdefinition übergeben wird |
| `basicAuthPasswordParam` | Ein Workflowdefinitionsparameter, der das Kennwort für die Standardauthentifizierung in einer HTTP-Aktion akzeptiert |
| `basicAuthUserNameParam` | Ein Workflowdefinitionsparameter, der den Benutzernamen für die Standardauthentifizierung in einer HTTP-Aktion akzeptiert |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Zugriff auf Dienste und Systeme, die von Logik-Apps aus aufgerufen werden

Hier finden Sie einige Möglichkeiten zum Schützen von Endpunkten, auf die Ihre Logik-App zugreifen muss, um Anforderungen zu senden:

* Fügen Sie eine Authentifizierung für ausgehende Anforderungen hinzu.

  Wenn Sie eine Aktion vom Typ HTTP, HTTP + Swagger (Open API) oder Webhook verwenden, können Sie der von Ihrer Logik-App gesendeten Anforderung eine Authentifizierung hinzufügen. Beispielsweise können Sie eine grundlegende Authentifizierung, eine Zertifikatauthentifizierung oder eine Azure Active Directory-Authentifizierung verwenden. Weitere Informationen finden Sie unter [Authentifizieren von Triggern oder Aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

* Schränken Sie den Zugriff von IP-Adressen von Logik-Apps ein.

  Alle Aufrufe von Logik-Apps an Endpunkte stammen von bestimmten IP-Adressen, die auf den Regionen Ihrer Logik-App basieren. Sie können Filter hinzufügen, die Anforderungen nur von diesen IP-Adressen akzeptieren. Weitere Informationen zu diesen IP-Adressen finden Sie unter [Grenzwert- und Konfigurationsinformationen für Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

* Authentifizieren Sie Ressourcen als verwaltete Identitäten, und greifen Sie darauf zu.

  Für den Zugriff auf Ressourcen in anderen Azure AD-Mandanten (Azure Active Directory) kann Ihre Logik-App Ihre Identität ohne Anmeldung anhand einer verwalteten Identität (früher als „Verwaltete Dienstidentität“ oder MSI bezeichnet) authentifizieren, statt Anmeldeinformationen oder Geheimnisse zu verwenden. Azure verwaltet diese Identität für Sie und dient als Hilfe beim Schützen Ihrer Anmeldeinformationen, da Sie keine Geheimnisse angeben oder eine Rotation dafür durchführen müssen. Weitere Informationen zum Einrichten und Verwenden einer systemseitig zugewiesenen verwalteten Identität für Ihre Logik-App finden Sie unter [Authentifizieren und Zugreifen auf Ressourcen mit verwalteten Identitäten in Azure Logic Apps](../logic-apps/create-managed-service-identity.md).

* Schützen Sie Ihre Verbindungen mit lokalen Systemen.

  Azure Logic Apps bietet eine Integration mit diesen Diensten, um eine sichere und zuverlässige lokale Kommunikation bereitzustellen.

  * Lokales Datengateway

    Viele verwaltete Connectors in Azure Logic Apps bieten sichere Verbindungen mit lokalen Systemen wie dem Dateisystem, SQL, SharePoint und DB2. Das Gateway sendet Daten von lokalen Quellen durch verschlüsselte Kanäle über Azure Service Bus. Der gesamte Datenverkehr stammt als sicherer ausgehender Datenverkehr vom Gateway-Agent. Erfahren Sie, [wie das lokale Datengateway funktioniert](logic-apps-gateway-install.md#gateway-cloud-service).

  * Verbindung über Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) bietet lokale Konnektivitätsoptionen wie die Integration von Site-to-Site-VPN und ExpressRoute für geschützte Proxys und die Kommunikation mit lokalen Systemen. Im Logik-App-Designer Ihres Logik-App-Workflows können Sie eine API auswählen, die über API Management verfügbar gemacht wird. So wird der Zugriff auf lokale Systeme schneller bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Bereitstellungsvorlagen](logic-apps-create-deploy-template.md)  
* [Überwachen von Logik-Apps](logic-apps-monitor-your-logic-apps.md)  
* [Diagnostizieren von Fehlern und Problemen bei Logik-Apps](logic-apps-diagnosing-failures.md)  
* [Automatisieren der Logik-App-Bereitstellung](logic-apps-azure-resource-manager-templates-overview.md)
