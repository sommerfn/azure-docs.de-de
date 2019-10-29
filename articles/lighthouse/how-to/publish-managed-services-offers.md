---
title: Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace
description: Erfahren Sie, wie Sie ein Angebot für verwaltete Dienste veröffentlichen, das das Onboarding von Kunden in die delegierte Azure-Ressourcenverwaltung durchführt.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 10/17/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 10105d06e48a727e71ea5cb03f2ffceb589df50a
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595273"
---
# <a name="publish-a-managed-services-offer-to-azure-marketplace"></a>Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace

In diesem Artikel erfahren Sie, wie Sie über das [Cloud-Partnerportal](https://cloudpartner.azure.com/) ein öffentliches oder privates Angebot für verwaltete Dienste im [Azure Marketplace](https://azuremarketplace.microsoft.com) veröffentlichen und damit einem Kunden, der das Angebot kauft, das Onboarding für die delegierte Azure-Ressourcenverwaltung ermöglichen.

> [!NOTE]
> Sie benötigen ein gültiges [Konto in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), um diese Angebote zu erstellen und zu veröffentlichen. Wenn Sie noch nicht über ein Konto verfügen, führt Sie der [Registrierungsvorgang](https://aka.ms/joinmarketplace) durch die Schritte zum Erstellen eines Kontos im Partner Center und die Registrierung beim kommerziellen Marketplace-Programm. Ihre MPN-ID (Microsoft Partner Network) den von Ihnen veröffentlichten Angeboten [automatisch zugeordnet](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started), um ihre Wirksamkeit hinsichtlich der Kundenbindung zu verfolgen.
>
> Wenn Sie ein Angebot nicht im Azure Marketplace veröffentlichen möchten, können Sie das Onboarding von Kunden mithilfe von Azure Resource Manager-Vorlagen manuell durchführen. Weitere Informationen finden Sie unter [Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen](onboard-customer.md).

Das Veröffentlichen eines Angebots für verwaltete Dienste ähnelt dem Veröffentlichen eines beliebigen anderen Typs von Angebot im Azure Marketplace. Informationen zu diesem Prozess finden Sie unter [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) und [Verwalten von Angeboten im Azure Marketplace und im AppSource-Marketplace](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers). Sehen Sie sich darüber hinaus die Informationen zu [Zertifizierungsrichtlinien für den kommerziellen Marketplace](https://docs.microsoft.com/legal/marketplace/certification-policies) (insbesondere den Abschnitt [Managed Services](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services) (Verwaltete Dienste)) an.

Wenn ein Kunde Ihr Angebot hinzufügt, kann er bestimmte Abonnements oder Ressourcengruppen delegieren, für die dann [für die delegierte Azure-Ressourcenverwaltung das Onboarding durchgeführt wird](#the-customer-onboarding-process). Beachten Sie Folgendes: Bevor für ein Abonnement (oder Ressourcengruppen innerhalb eines Abonnements) das Onboarding durchgeführt werden kann, muss das Abonnement manuell für das Onboarding autorisiert werden, indem der Ressourcenanbieter **Microsoft.ManagedServices** registriert wird.

> [!IMPORTANT]
> Jeder Plan in einem Angebot für verwaltete Dienste enthält einen Abschnitt mit **Manifestdetails**, in dem Sie die Azure Active Directory-Entitäten (Azure AD) in Ihrem Mandanten definieren, die Zugriff auf die delegierten Ressourcengruppen und/oder Abonnements für Kunden, die diesen Plan kaufen, erhalten sollen. Es ist wichtig, zu bedenken, dass jede Gruppe (bzw. jeder Benutzer oder Dienstprinzipal), die Sie hier einschließen, über dieselben Berechtigungen für jeden Kunden verfügt, der den Plan kauft. Um verschiedene Gruppen für die Arbeit mit den einzelnen Kunden zuzuweisen, müssen Sie einen gesonderten privaten Plan veröffentlichen, der exklusiv für jeden Kunden ist.

## <a name="create-your-offer-in-the-cloud-partner-portal"></a>Erstellen Ihres Angebots im Cloud-Partnerportal

1. Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.
2. Wählen Sie im linken Navigationsmenü **Neues Angebot** aus, und wählen Sie dann **Verwaltete Dienste** aus.
3. Ihnen wird ein Abschnitt **Editor** für Ihr Angebot angezeigt, der vier auszufüllende Teile enthält: **Angebotseinstellungen**, **Pläne**, **Marketplace** und **Support**. Lesen Sie weiter, wenn Sie eine Anleitung zum Ausfüllen dieser Abschnitte wünschen.

## <a name="enter-offer-settings"></a>Eingeben der Angebotseinstellungen

Geben Sie im Abschnitt **Angebotseinstellungen** Folgendes ein:

|Feld  |BESCHREIBUNG  |
|---------|---------|
|**Angebots-ID**     | Ein eindeutiger Bezeichner für Ihr Angebot (innerhalb Ihres Herausgeberprofils). Diese ID kann nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und darf höchstens 50 Zeichen lang sein. Denken Sie daran, dass die Angebots-ID an Stellen wie in Produkt-URLs und Abrechnungsberichten möglicherweise für Kunden sichtbar ist. Sobald Sie das Angebot veröffentlichen, können Sie diesen Wert nicht mehr ändern.        |
|**Herausgeber-ID**     | Die Herausgeber-ID, die dem Angebot zugeordnet wird. Wenn Sie über mehr als eine Herausgeber-ID verfügen, können Sie die für dieses Angebot zu verwendende auswählen.       |
|**Name**     | Der Name (bis zu 50 Zeichen), der Kunden im Azure Marketplace und im Azure-Portal für Ihr Angebot angezeigt wird. Verwenden Sie einen erkennbaren Markennamen, den Kunden verstehen. Wenn Sie dieses Angebot über Ihre eigene Website bewerben, achten Sie darauf, dass Sie hier genau denselben Namen verwenden.        |

Wählen Sie fertig sind, wählen Sie **Speichern** aus. Nun sind Sie bereit, um mit dem Abschnitt **Pläne** fortzufahren.

## <a name="create-plans"></a>Pläne erstellen

Jedes Angebot muss über einen oder mehrere Pläne (manchmal auch als SKUs bezeichnet) verfügen. Sie können mehrere Pläne hinzufügen, um verschiedene Featuregruppen zu unterschiedlichen Preisen zu unterstützen oder um einen bestimmten Plan für eine begrenzte Zielgruppe spezifischer Kunden anzupassen. Kunden können die Pläne, die Ihnen zur Verfügung stehen, unter dem übergeordneten Angebot anzeigen.

Wählen Sie im Abschnitt „Pläne“ für jeden Plan, den Sie erstellen möchten, **Neuer Plan** aus. Geben Sie dann eine **Plan-ID** ein. Diese ID kann nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und darf höchstens 50 Zeichen lang sein. Die Plan-ID kann an Stellen wie in Produkt-URLs und Abrechnungsberichten möglicherweise für Kunden sichtbar ist. Sobald Sie das Angebot veröffentlichen, können Sie diesen Wert nicht mehr ändern.

Füllen Sie als Nächstes die folgenden Abschnitte im Abschnitt **Plandetails** aus:

|Feld  |BESCHREIBUNG  |
|---------|---------|
|**Titel**     | Anzeigename für den anzuzeigenden Plan. Die maximale Länge beträgt 50 Zeichen.        |
|**Zusammenfassung**     | Eine kurze Beschreibung des Plans für die Anzeige unterhalb des Titels. Die maximale Länge beträgt 100 Zeichen.        |
|**Beschreibung**     | Beschreibender Text zur ausführlicheren Erläuterung des Plans.         |
|**Abrechnungsmodell**     | Hier werden 2 Abrechnungsmodelle angezeigt, aber für Angebote für verwaltete Dienste müssen Sie **Bring Your Own License** auswählen. Dies bedeutet, dass Sie Ihren Kunden Kosten im Zusammenhang mit diesem Angebot direkt berechnen, und dass Microsoft Ihnen keine Gebühren berechnet.   |
|**Ist dies ein privater Plan?**     | Gibt an, ob es sich um eine private oder öffentliche SKU handelt. Der Standardwert lautet **Nein** (öffentlich). Wenn Sie diese Auswahl unverändert lassen, wird Ihr Plan nicht auf bestimmte Kunden (oder eine bestimmte Anzahl von Kunden) beschränkt. Nachdem Sie einen öffentlichen Plan veröffentlicht haben, können Sie ihn später nicht mehr in „Privat“ ändern. Um diesen Plan nur für bestimmte Kunden verfügbar zu machen, wählen Sie **Ja** aus. Wenn Sie dies tun, müssen Sie die Kunden identifizieren, indem Sie ihre Abonnement-IDs angeben. Diese können einzeln (für bis zu 10 Abonnements) oder durch Hochladen einer CSV-Datei (für bis zu 20.000 Abonnements) eingegeben werden. Stellen Sie sicher, dass Sie hier Ihre eigenen Abonnements einschließen, damit Sie das Angebot testen und validieren können. Weitere Informationen finden Sie unter [Private SKUs und Pläne](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  |

Füllen Sie schließlich den Abschnitt **Manifestdetails** aus. Dadurch wird ein Manifest mit Autorisierungsinformationen zum Verwalten von Kundenressourcen erstellt. Die Informationen, die Sie hier angeben, sind für das Onboarding Ihrer Kunden für die delegierte Azure-Ressourcenverwaltung notwendig. Wie zuvor erwähnt, gelten diese Berechtigungen für alle Kunden, die den Plan kaufen. Wenn Sie also den Zugriff auf einen bestimmten Kunden beschränken möchten, müssen Sie einen privaten Plan zu dessen exklusiver Verwendung veröffentlichen.

- Geben Sie als Erstes eine **Version** für das Manifest an. Verwenden Sie das Format *n.n.n* (z. B. 1.2.5).
- Geben Sie als Nächstes Ihre **Mandanten-ID** an. Dabei handelt es sich um eine GUID, die der Azure Active Directory-Mandanten-ID Ihrer Organisation zugeordnet ist (d. h. dem Mandanten, in dem Sie zum Verwalten der Ressourcen Ihrer Kunden arbeiten werden). Wenn Sie diese nicht zur Hand haben, können Sie sie anzeigen, indem Sie rechts oben im Azure-Portal auf Ihren Kontonamen zeigen, oder indem Sie **Verzeichnis wechseln** auswählen. 
- Fügen Sie zu guter Letzt Ihrem Plan einen oder mehrere **Autorisierung**seinträge hinzu. Autorisierungen definieren die Entitäten, die auf Ressourcen und Abonnements für Kunden zugreifen können, die den Plan kaufen. Sie müssen diese Informationen bereitstellen, um im Namen des Kunden mithilfe der delegierten Azure-Ressourcenverwaltung auf Ressourcen zuzugreifen.
  Geben Sie für jede Autorisierung Folgendes an. Sie können dann so oft wie nötig **Neue Autorisierung** auswählen, um weitere Benutzer-/Rollendefinitionen hinzuzufügen.
  - **Azure AD-Objekt-ID**: Der Azure AD-Bezeichner eines Benutzers, einer Benutzergruppe oder Anwendung, dem bestimmte Berechtigungen (wie in der Rollendefinition beschrieben) für die Ressourcen Ihrer Kunden gewährt werden.
  - **Azure AD-Objektanzeigename**: Ein Anzeigename, der dem Kunden helfen soll, den Zweck dieser Autorisierung zu verstehen. Dem Kunde wird dieser Name beim Delegieren von Ressourcen angezeigt.
  - **Rollendefinition:** Wählen Sie eine der verfügbaren integrierten Azure AD-Rollen aus der Liste aus. Diese Rolle legt die Berechtigungen fest, über die der Benutzer im Feld **Azure AD-Objekt-ID** für Ressourcen Ihrer Kunden verfügt. Weitere Informationen zu diesen Rollen finden Sie unter [Integrierte Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).
  - **Zuweisbare Rollen**: Dies ist nur erforderlich, wenn Sie in der **Rollendefinition** für diese Autorisierung „Benutzerzugriffsadministrator“ ausgewählt haben. Wenn dies der Fall ist, müssen Sie hier mindestens eine zuweisbare Rollen hinzufügen. Der Benutzer im Feld **Azure AD-Objekt-ID** kann diese **Zuweisbaren Rollen** [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) zuweisen. Beachten Sie, dass für diesen Benutzer keine anderen Berechtigungen gelten, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind. Wenn Sie hier keine Rolle auswählen, besteht Ihre Übermittlung die Zertifizierung nicht. (Wenn Sie für die Rollendefinition dieses Benutzers nicht „Benutzerzugriffsadministrator“ ausgewählt haben, hat dieses Feld keine Auswirkung.)

> [!TIP]
> In den meisten Fällen sollten Sie einer Azure AD-Benutzergruppe oder einem Dienstprinzipal Berechtigungen zuzuweisen, anstatt eine Reihe einzelner Benutzerkonten. Auf diese Weise können Sie den Zugriff für einzelne Benutzer hinzufügen oder entfernen, ohne den Plan aktualisieren und erneut veröffentlichen zu müssen, wenn sich ihre Zugriffsanforderungen ändern.

Wenn Sie mit dem Hinzufügen von Plänen fertig sind, wählen Sie **Speichern** aus, und fahren Sie dann mit dem Abschnitt **Marketplace** fort.

## <a name="provide-marketplace-text-and-images"></a>Bereitstellen von Marketplace-Text und -Bildern

Im Abschnitt **Marketplace** stellen Sie den Text und die Bilder bereit, die Kunden im Azure Marketplace und im Azure-Portal sehen werden.

Geben Sie im Abschnitt **Übersicht** Informationen für die folgenden Felder an:

|Feld  |BESCHREIBUNG  |
|---------|---------|
|**Titel**     |  Der Titel des Angebots, meist der lange, formelle Name. Dieser Titel wird im Marketplace gut sichtbar angezeigt. Die maximale Länge beträgt 50 Zeichen. In den meisten Fällen entspricht dieser Wert dem **Namen**, den Sie im Abschnitt **Angebotseinstellungen** eingegeben haben.       |
|**Zusammenfassung**     | Kurze Beschreibung des Zwecks oder der Funktion Ihres Angebots. Diese wird in der Regel unterhalb des Titels angezeigt. Die maximale Länge beträgt 100 Zeichen.        |
|**Long Summary** (Lange Zusammenfassung)     | Eine längere Zusammenfassung von Zweck und Funktion Ihres Angebots. Die maximale Länge beträgt 256 Zeichen.        |
|**Beschreibung**     | Weitere Informationen zu Ihrem Angebot. Die maximale Länge des Felds beträgt 3000 Zeichen, und es unterstützt einfache HTML-Formatierung. Sie müssen die Begriffe „verwalteter Dienst“ oder „verwaltete Dienste“ in Ihrer Beschreibung verwenden.       |
|**Marketing Identifier** (Marketingbezeichner)     | Ein eindeutiger, aussagekräftiger URL-Bezeichner. Dieser wird in den Marketplace-URLs für dieses Angebot verwendet. Angenommen, Ihre Herausgeber-ID lautet *contoso* und Ihr Marketingbezeichner lautet *sampleApp*, dann ist die URL für Ihr Angebot im Azure Marketplace gleich *https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp* .        |
|**Preview Subscription IDs** (Abonnement-IDs für die Vorschauversion)     | Sie können zwischen 1 und 100 Abonnement-IDs hinzufügen. Die diesem Abonnement zugeordneten Kunden können das Angebot in Azure Marketplace anzeigen, bevor es live geschaltet wird. Wir schlagen vor, dass Sie hier Ihre eigenen Abonnements einschließen, damit Sie eine Vorschau anzeigen können, wie Ihr Angebot im Azure Marketplace angezeigt wird, bevor Sie es für Kunden verfügbar machen.  (Microsoft-Support- und -Technikteams können Ihr Angebot auch während dieses Vorschauzeitraums anzeigen.)   |
|**Nützliche Links**     | URLs im Zusammenhang mit Ihrem Angebot, z. B. Dokumentation, Anmerkungen zu dieser Version, FAQs usw.        |
|**Vorgeschlagene Kategorien (max. 5)**     | Mindestens eine Kategorie (bis zu fünf), die auf Ihr Angebot zutrifft. Diese Kategorien helfen Kunden dabei, Ihr Angebot in Azure Marketplace und im Azure-Portal zu ermitteln.        |

Im Abschnitt **Marketingartefakte** können Sie Logos und andere Ressourcen hochladen, die mit Ihrem Angebot angezeigt werden sollen. Optional können Sie Screenshots oder Links zu Videos hochladen, die Kunden dabei helfen können, Ihr Angebot besser zu verstehen.

Vier Logogrößen sind erforderlich: **Klein (40 x 40)** , **Mittel (90 x 90)** , **Groß (115 x 115)** und **Breit (255 x 155)** . Befolgen Sie die folgenden Richtlinien für Ihre Logos:

- Die Farbpalette des Azure-Designs ist einfach und geradlinig. Beschränken Sie die Anzahl der Primär- und Sekundärfarben auf Ihrem Logo.
- Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihr Logo. Verwenden Sie eine Farbe, die Ihr Logo im Portal ideal zur Geltung bringt. Sie sollten einfache Primärfarben verwenden.
- Wenn Sie einen transparenten Hintergrund verwenden, stellen Sie sicher, dass das Logo und der Text nicht weiß, schwarz oder blau sind.
- Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein. Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
- Platzieren Sie keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen.
- Stellen Sie sicher, dass das Logo nicht verzerrt wird.

Das **Hero (815 x 290)** -Logo ist optional, wird aber empfohlen. Wenn Sie ein Hero-Logo einschließen, befolgen Sie die folgenden Richtlinien:

- Fügen Sie keinen Text in das Hero-Logo ein, und stellen Sie sicher, dass Sie am rechten Rand des Logos 415 Pixel Leerraum lassen. Dies ist erforderlich, um freien Platz für Textelemente zu lassen, die programmgesteuert eingebettet werden: Ihren Anzeigenamen als Herausgeber, den Plantitel, die ausführliche Angebotszusammenfassung.
- Der Hintergrund Ihres Hero-Logos darf weder schwarz noch weiß noch transparent sein. Stellen Sie sicher, dass Ihre Hintergrundfarbe nicht zu hell ist, da der eingebettete Text in Weiß angezeigt wird.
- Nachdem Sie Ihr Angebot mit einem Hero-Symbol veröffentlicht haben, können Sie dieses nicht mehr entfernen (obwohl Sie es bei Bedarf mit einer anderen Version aktualisieren können).

Im Abschnitt **Leadverwaltung** können Sie das CRM-System auswählen, in dem Ihre Leads gespeichert werden sollen. Beachten Sie, dass gemäß den [Zertifizierungsrichtlinien für verwaltete Dienste](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services) eine **Leadzielgruppe** erforderlich ist.

Geben Sie abschließend Ihre **Datenschutzrichtlinien-URL** und die **Nutzungsbedingungen** im Abschnitt **Rechtliche Hinweise** an. Sie können hier auch angeben, ob der [Standardvertrag](https://docs.microsoft.com/azure/marketplace/standard-contract) für dieses Angebot verwendet werden soll oder nicht.

Stellen Sie sicher, dass Sie Ihre Änderungen speichern, bevor Sie mit dem Abschnitt **Support** fortfahren.

## <a name="add-support-info"></a>Hinzufügen von Supportinformationen

Geben Sie im Abschnitt **Support** den Namen, die E-Mail-Adresse und die Telefonnummer eines Engineering-Kontakts ein sowie einen Kundensupportkontakt an. Sie müssen außerdem Support-URLs bereitstellen. Microsoft kann diese Informationen verwenden, wenn wir Sie wegen geschäftlicher und Supportprobleme kontaktieren müssen.

Nachdem Sie diese Informationen hinzugefügt haben, wählen Sie **Speichern** aus.

## <a name="publish-your-offer"></a>Veröffentlichen Ihres Angebots

Wenn alle von Ihnen bereitgestellten Informationen so sind, wie Sie es haben möchten, besteht Ihr nächster Schritt darin, das Angebot in Azure Marketplace zu veröffentlichen. Wählen Sie die Schaltfläche **Veröffentlichen** aus, um den Vorgang zu starten, mit dem Ihr Angebot live geschaltet wird. Weitere Informationen zu diesem Prozess finden Sie unter [Veröffentlichen von Azure Marketplace- und AppSource-Angeboten](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer).

## <a name="the-customer-onboarding-process"></a>Kundenonboarding

Wenn ein Kunde Ihr Angebot hinzufügt, kann er [bestimmte Abonnements oder Ressourcengruppen delegieren](view-manage-service-providers.md#delegate-resources), für die dann für die delegierte Azure-Ressourcenverwaltung das Onboarding durchgeführt wird. Wenn ein Kunde ein Angebot angenommen, aber noch keine Ressourcen delegiert hat, wird im Azure-Portal auf der Seite [**Dienstanbieter**](view-manage-service-providers.md) am oberen Rand des Abschnitts **Anbieterangebote** ein Hinweis angezeigt. Wenn ein Benutzer im Mandanten des Kunden diese Delegierung nicht ausführen kann, liegt dies wahrscheinlich daran, dass er nicht über die Rolle „Besitzer“ für das Abonnement verfügt. Um Benutzer zu suchen, die das Abonnement delegieren können, kann der Benutzer das Abonnement im Azure-Portal auswählen, **Zugriffssteuerung (IAM)** öffnen und [alle Benutzer mit der Rolle „Besitzer“ anzeigen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#view-roles-and-permissions).

Bevor für ein Abonnement (oder Ressourcengruppen innerhalb eines Abonnements) das Onboarding durchgeführt werden kann, muss das Abonnement manuell für das Onboarding autorisiert werden, indem der Ressourcenanbieter **Microsoft.ManagedServices** registriert wird. Ein Benutzer im Kundenmandanten mit der Rolle „Mitwirkender“ oder „Besitzer“ kann diesen Schritt anhand der Anleitung unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/resource-manager-supported-services.md) ausführen.

Der Kunde kann dann mit einer der folgenden Methoden bestätigen, dass das Abonnement für das Onboarding bereit ist:

### <a name="azure-portal"></a>Azure-Portal

1. Wählen Sie im Azure-Portal das Abonnement aus.
1. Wählen Sie **Ressourcenanbieter** aus.
1. Vergewissern Sie sich, dass **Microsoft. ManagedServices** als **Registriert** angezeigt wird.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Set-AzContext -Subscription <subscriptionId>
Get-AzResourceProvider -ProviderNameSpace 'Microsoft.ManagedServices'
```

Daraufhin sollten Ergebnisse ähnlich den folgenden zurückgegeben werden:

```output
ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationDefinitions}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationAssignments}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {}
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set –subscription <subscriptionId>
az provider show --namespace "Microsoft.ManagedServices" --output table
```

Daraufhin sollten Ergebnisse ähnlich den folgenden zurückgegeben werden:

```output
Namespace                  RegistrationState
-------------------------  -------------------
Microsoft.ManagedServices  Registered
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- [Anzeigen und Verwalten von Kunden](view-manage-customers.md), indem sie im Azure-Portal zu **Meine Kunden** navigieren.
