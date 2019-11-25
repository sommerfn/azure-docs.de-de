---
title: Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace
description: Erfahren Sie, wie Sie ein Angebot für verwaltete Dienste veröffentlichen, das das Onboarding von Kunden in die delegierte Azure-Ressourcenverwaltung durchführt.
ms.date: 11/15/2019
ms.topic: overview
ms.openlocfilehash: 29f17e6227d3c50a4d9fe13f7525ac71f7550632
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131296"
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

Wählen Sie im Abschnitt für Pläne **Neuer Plan** aus. Geben Sie dann eine **Plan-ID** ein. Diese ID kann nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und darf höchstens 50 Zeichen lang sein. Die Plan-ID kann an Stellen wie in Produkt-URLs und Abrechnungsberichten möglicherweise für Kunden sichtbar ist. Sobald Sie das Angebot veröffentlichen, können Sie diesen Wert nicht mehr ändern.

### <a name="plan-details"></a>Plandetails

Füllen Sie als die folgenden Abschnitte im Abschnitt **Plandetails** aus:

|Feld  |BESCHREIBUNG  |
|---------|---------|
|**Titel**     | Anzeigename für den anzuzeigenden Plan. Die maximale Länge beträgt 50 Zeichen.        |
|**Zusammenfassung**     | Eine kurze Beschreibung des Plans für die Anzeige unterhalb des Titels. Die maximale Länge beträgt 100 Zeichen.        |
|**Beschreibung**     | Beschreibender Text zur ausführlicheren Erläuterung des Plans.         |
|**Abrechnungsmodell**     | Hier werden 2 Abrechnungsmodelle angezeigt, aber für Angebote für verwaltete Dienste müssen Sie **Bring Your Own License** auswählen. Dies bedeutet, dass Sie Ihren Kunden Kosten im Zusammenhang mit diesem Angebot direkt berechnen, und dass Microsoft Ihnen keine Gebühren berechnet.   |
|**Ist dies ein privater Plan?**     | Gibt an, ob es sich um eine private oder öffentliche SKU handelt. Der Standardwert lautet **Nein** (öffentlich). Wenn Sie diese Auswahl unverändert lassen, wird Ihr Plan nicht auf bestimmte Kunden (oder eine bestimmte Anzahl von Kunden) beschränkt. Nachdem Sie einen öffentlichen Plan veröffentlicht haben, können Sie ihn später nicht mehr in „Privat“ ändern. Um diesen Plan nur für bestimmte Kunden verfügbar zu machen, wählen Sie **Ja** aus. Wenn Sie dies tun, müssen Sie die Kunden identifizieren, indem Sie ihre Abonnement-IDs angeben. Diese können einzeln (für bis zu 10 Abonnements) oder durch Hochladen einer CSV-Datei (für bis zu 20.000 Abonnements) eingegeben werden. Stellen Sie sicher, dass Sie hier Ihre eigenen Abonnements einschließen, damit Sie das Angebot testen und validieren können. Weitere Informationen finden Sie unter [Private SKUs und Pläne](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  |

### <a name="manifest-details"></a>Manifestdetails

Füllen Sie den Abschnitt **Manifestdetails** für Ihren Plan aus. Dadurch wird ein Manifest mit Autorisierungsinformationen zum Verwalten von Kundenressourcen erstellt. Diese Informationen werden benötigt, um die delegierte Azure-Ressourcenverwaltung zu aktivieren.

> [!NOTE]
> Wie oben erwähnt, werden die Benutzer und Rollen in Ihren **Autorisierungseinträgen** auf jeden Kunden angewendet, der den Plan erwirbt. Wenn Sie den Zugriff auf einen bestimmten Kunden eingrenzen möchten, müssen Sie einen privaten Plan für dessen ausschließliche Nutzung veröffentlichen.

Geben Sie als Erstes eine **Version** für das Manifest an. Verwenden Sie das Format *n.n.n* (z. B. 1.2.5).

Geben Sie als Nächstes Ihre **Mandanten-ID** an. Dabei handelt es sich um eine GUID, die der Azure Active Directory-Mandanten-ID Ihrer Organisation zugeordnet ist (d. h. dem Mandanten, in dem Sie zum Verwalten der Ressourcen Ihrer Kunden arbeiten werden). Wenn Sie diese nicht zur Hand haben, können Sie sie anzeigen, indem Sie rechts oben im Azure-Portal auf Ihren Kontonamen zeigen, oder indem Sie **Verzeichnis wechseln** auswählen.

Fügen Sie zu guter Letzt Ihrem Plan einen oder mehrere **Autorisierung**seinträge hinzu. Autorisierungen definieren die Entitäten, die auf Ressourcen und Abonnements für Kunden zugreifen können, die den Plan erwerben. Außerdem werden Rollen zur Erteilung bestimmter Zugriffsebenen zugewiesen. Ausführliche Informationen zu den unterstützten Rollen finden Sie unter [Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien](../concepts/tenants-users-roles.md).

Geben Sie für jede **Autorisierung** die folgenden Informationen an. Sie können dann so oft wie nötig **Neue Autorisierung** auswählen, um weitere Benutzer- und Rollendefinitionen hinzuzufügen.

- **Azure AD-Objekt-ID**: Der Azure AD-Bezeichner eines Benutzers, einer Benutzergruppe oder Anwendung, dem bestimmte Berechtigungen (wie in der Rollendefinition beschrieben) für die Ressourcen Ihrer Kunden gewährt werden.
- **Azure AD-Objektanzeigename**: Ein Anzeigename, der dem Kunden helfen soll, den Zweck dieser Autorisierung zu verstehen. Dem Kunde wird dieser Name beim Delegieren von Ressourcen angezeigt.
- **Rollendefinition:** Wählen Sie eine der verfügbaren integrierten Azure AD-Rollen aus der Liste aus. Diese Rolle legt die Berechtigungen fest, über die der Benutzer im Feld **Azure AD-Objekt-ID** für Ressourcen Ihrer Kunden verfügt. Beschreibungen dieser Rollen finden Sie unter [Integrierte Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) und [Rollenunterstützung für die delegierte Azure-Ressourcenverwaltung](../concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management).
- **Zuweisbare Rollen**: Dies ist nur erforderlich, wenn Sie in der **Rollendefinition** für diese Autorisierung „Benutzerzugriffsadministrator“ ausgewählt haben. Wenn dies der Fall ist, müssen Sie hier mindestens eine zuweisbare Rollen hinzufügen. Der Benutzer im Feld **Azure AD-Objekt-ID** kann diese **Zuweisbaren Rollen** [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) zuweisen. Dies ist erforderlich, um [Richtlinien bereitzustellen, die gewartet werden können](deploy-policy-remediation.md). Beachten Sie, dass für diesen Benutzer keine anderen Berechtigungen gelten, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind. Wenn Sie hier keine Rolle auswählen, besteht Ihre Übermittlung die Zertifizierung nicht. (Wenn Sie für die Rollendefinition dieses Benutzers nicht „Benutzerzugriffsadministrator“ ausgewählt haben, hat dieses Feld keine Auswirkung.)

> [!TIP]
> In den meisten Fällen sollten Sie einer Azure AD-Benutzergruppe oder einem Dienstprinzipal Berechtigungen zuzuweisen, anstatt eine Reihe einzelner Benutzerkonten. Auf diese Weise können Sie den Zugriff für einzelne Benutzer hinzufügen oder entfernen, ohne den Plan aktualisieren und erneut veröffentlichen zu müssen, wenn sich ihre Zugriffsanforderungen ändern. Zusätzliche Empfehlungen finden Sie unter [Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien](../concepts/tenants-users-roles.md).

Nachdem Sie die Informationen eingegeben haben, können Sie so häufig wie nötig **Neuer Plan** auswählen, um zusätzliche Pläne zu erstellen. Wenn Sie die Bearbeitung abgeschlossen haben, wählen Sie **Speichern** aus, und fahren Sie dann mit dem Abschnitt **Marketplace** fort.

## <a name="provide-marketplace-text-and-images"></a>Bereitstellen von Marketplace-Text und -Bildern

Im Abschnitt **Marketplace** stellen Sie den Text und die Bilder bereit, die Kunden im Azure Marketplace und im Azure-Portal sehen werden.

Vervollständigen Sie im Abschnitt **Übersicht** die folgenden Felder:

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

Geben Sie im Abschnitt **Support** den Namen, die E-Mail-Adresse und die Telefonnummer eines Engineering-Kontakts ein sowie einen Kundensupportkontakt an. Sie müssen außerdem Support-URLs bereitstellen. Microsoft kann diese Informationen verwenden, um Sie bei Geschäfts- und Supportfragen zu kontaktieren.

Nachdem Sie diese Informationen hinzugefügt haben, wählen Sie **Speichern** aus.

## <a name="publish-your-offer"></a>Veröffentlichen Ihres Angebots

Nach der Vervollständigung aller Abschnitte besteht Ihr nächster Schritt darin, das Angebot im Azure Marketplace zu veröffentlichen. Wählen Sie die Schaltfläche **Veröffentlichen** aus, um den Vorgang zu starten, mit dem Ihr Angebot live geschaltet wird. Weitere Informationen zu diesem Vorgang finden Sie unter [Veröffentlichen von Azure Marketplace- und AppSource-Angeboten](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer).

Sie können jederzeit [eine aktualisierte Version Ihres Angebots veröffentlichen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-update-offer). Beispielsweise können Sie einem zuvor veröffentlichten Angebot eine neue Rollendefinition hinzufügen. Kunden, die das Angebot bereits hinzugefügt haben, wird dann auf der Seite [**Dienstanbieter**](view-manage-service-providers.md) im Azure-Portal ein Symbol angezeigt, das darauf hinweist, dass eine Aktualisierung verfügbar ist. Jeder Kunde kann die [Änderungen überprüfen](view-manage-service-providers.md#update-service-provider-offers) und entscheiden, ob er auf die neue Version aktualisieren möchte. 

## <a name="the-customer-onboarding-process"></a>Kundenonboarding

Nachdem ein Kunde Ihr Angebot hinzugefügt hat, kann er [bestimmte Abonnements oder Ressourcengruppen delegieren](view-manage-service-providers.md#delegate-resources), für die dann ein Onboarding für die delegierte Azure-Ressourcenverwaltung durchgeführt wird. Wenn ein Kunde ein Angebot angenommen, aber noch keine Ressourcen delegiert hat, wird im Azure-Portal auf der Seite [**Dienstanbieter**](view-manage-service-providers.md) am oberen Rand des Abschnitts **Anbieterangebote** ein Hinweis angezeigt.

> [!IMPORTANT]
> Die Delegierung muss von einem Nicht-Gastkonto im Mandanten des Kunden durchgeführt werden, das über die [integrierte Rolle „Besitzer“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) für das Abonnement verfügt, das integriert wird (oder das die Ressourcengruppen enthält, die integriert werden). Um alle Benutzer anzuzeigen, die das Abonnement delegieren können, kann ein Benutzer im Mandanten des Kunden das Abonnement im Azure-Portal auswählen, **Zugriffssteuerung (IAM)** öffnen und [alle Benutzer mit der Rolle „Besitzer“ anzeigen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#view-roles-and-permissions).

Nachdem der Kunde ein Abonnement (oder eine oder mehrere Ressourcengruppen innerhalb eines Abonnements) delegiert hat, wird der Ressourcenanbieter **Microsoft.ManagedServices** für dieses Abonnement registriert, und Benutzer in Ihrem Mandanten können gemäß den Autorisierungen in Ihrem Angebot auf die delegierten Ressourcen zugreifen.

> [!NOTE]
> Derzeit können Abonnements (oder Ressourcengruppen innerhalb eines Abonnements) nicht delegiert werden, wenn das Abonnement Azure Databricks verwendet. Ebenso ist es nicht möglich, Databricks-Arbeitsbereiche in diesem Abonnement zu erstellen, wenn ein Abonnement (oder eine Ressourcengruppe innerhalb eines Abonnements) bereits delegiert wurde.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- [Anzeigen und Verwalten von Kunden](view-manage-customers.md), indem sie im Azure-Portal zu **Meine Kunden** navigieren.
