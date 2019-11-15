---
title: Kontomigration vom Cloud-Partnerportal zu Partner Center – Kommerzieller Marketplace für Azure
description: Hier erfahren Sie, wie Sie Ihr Konto von CPP zu Partner Center migrieren. – Kommerzieller Marketplace für Azure
author: ChJenk
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: f8644a2aa989b7013cbbd64e8a8194bc08f40251
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73813103"
---
# <a name="account-migration-from-cloud-partner-portal-to-partner-center"></a>Kontomigration vom Cloud-Partnerportal zu Partner Center

Wenn Sie ein vorhandenes CPP für das Cloud-Partnerportal (CPP) verwenden, müssen Ihre Kontoeinstellungen zu Partner Center migriert werden.

## <a name="account-migration-process"></a>Kontomigrationsprozess

Wenn Sie ein Benutzer mit der Rolle „Besitzer“ in CPP für ein bestimmtes Konto sind, wird ein gelbes Banner auf der Seite mit Ihrem Herausgeberprofil angezeigt. Für Sie gilt möglicherweise einer der beiden folgenden Fälle:

- Ihr Konto wurde bereits migriert, und Sie sind bereit für die Verwaltung Ihrer Kontoeinstellungen in Partner Center.
- Ihr Konto wurde nicht zu Partner Center migriert, und Sie müssen weitere Aktionen durchführen.

### <a name="your-account-has-been-migrated-to-partner-center"></a>Ihr Konto wurde zu Partner Center migriert.

Für alle Konten, die die Migration von CPP zu Partner Center abgeschlossen haben, erfolgt die Kontoverwaltung in Partner Center. Änderungen wie z. B. das Hinzufügen oder Löschen von Benutzern, werden mit CPP synchronisiert.

### <a name="you-have-not-yet-migrated-your-account-to-partner-center"></a>Sie haben Ihr Konto noch nicht zu Partner Center migriert.

Klicken Sie auf das Banner, um den Prozess der Kontomigration zu starten. Es werden folgende Eingaben von Ihnen erwartet:

1. E-Mail-Adresse (geschäftlich): <br> <br> In den meisten Fällen melden Sie sich mit der E-Mail-Adresse an, die Sie zum Anmelden bei CPP verwenden. In bestimmten Fällen muss eine andere E-Mail-Adresse verwendet werden:

    * Microsoft-Konto: Wenn das CPP-Konto ein Microsoft-Konto ist, geben Sie eine gültige geschäftliche E-Mail-Adresse ein, die dem Mandanten zugeordnet ist, für den die Microsoft Partner Network-ID (MPN) registriert ist. Weitere Informationen finden Sie unter [Registrieren für das Microsoft Partner Network-Programm](#sign-up-for-microsoft-partner-network-program).

    * Mandantenkonflikt: Wenn Ihre geschäftliche E-Mail-Adresse nicht zu dem Mandanten gehört, dem die Microsoft Partner Network-ID in Ihrem CPP-Konto zugeordnet ist, wird ein Fehler angezeigt. Um diesen Fehler zu umgehen, geben Sie eine E-Mail-Adresse ein, die dem Mandanten zugeordnet ist. In einer Fehlermeldung ist der Name des Mandanten angegeben.

2. Registrieren für das Microsoft Partner Network-Programm

    Falls Ihr CPP-Konto keine Microsoft Partner Network-ID aufweist oder die ID ungültig ist, müssen Sie sich im Rahmen des Aktivierungsprozesses für das Microsoft Partner Network-Programm registrieren.

## <a name="sign-up-for-microsoft-partner-network-program"></a>Registrieren für das Microsoft Partner Network-Programm

Unternehmen, die mit Microsoft zusammenarbeiten möchten, müssen Mitglied im Microsoft Partner Network (MPN) werden und eine MPN-ID erhalten. Wenn Sie bereits Mitglied des Microsoft Partner Network sind und über eine MPN-ID verfügen, halten Sie die Informationen bereit, da Sie sie während der Kontoaktivierung benötigen.  

Wenn Sie noch kein Mitglied des Microsoft Partner Network sind, können Sie [hier beitreten](https://signup.microsoft.com/signup?sku=StoreForBusinessIW&origin=partnerdashboard&culture=en-us&ru=https://partner.microsoft.com/dashboard/account/v3/xpu/onboard?ru=/en-us/dashboard/account/v3/enrollment/companyprofile/basicpartnernetwork/new), um eine MPN-ID zu erhalten. Notieren Sie sich Ihre MPN-ID, da Sie sie während der Kontoaktivierung eingeben müssen.

Weitere Informationen zum Microsoft Partner Network finden Sie unter [Mitglied werden im Microsoft Partner Network](https://partner.microsoft.com/en-US/membership) auf der Partnerwebsite. Weitere Informationen zu den ISV-Vorteilen des Microsoft Partner Network finden Sie unter [ISV-Ressourcenhub](https://partner.microsoft.com/isv-resource-hub).  

## <a name="move-dynamics-365-and-powerapps-offers-to-partner-center"></a>Verschieben von Dynamics 365- und PowerApps-Angeboten in Partner Center

Um die Konto- und Angebotsverwaltung für Dynamics 365 Customer Engagement, PowerApps und Dynamics 365 Operations zu optimieren, wurden Angebote in [Partner Center](https://partner.microsoft.com/) verschoben. Durch die Verschiebung wird sichergestellt, dass derselbe Inhalt sowohl für öffentliche als auch für Verkäuferkataloge verfügbar ist.

Spezifische Informationen dazu, was Sie bis zum **15. Oktober 2019** für Ihre Dynamics 365 Customer Engagement-, PowerApps- und Dynamics 365 Operations-Angebote tun müssen, finden Sie in den folgenden Anweisungen.

> [!NOTE]
> Dies gilt nicht für Dynamics 365 Business Central-Angebote.  

1. Wenn Ihr MPN-Mitgliedschaftskonto ursprünglich in Partner Membership Center (PMC) erstellt wurde, melden Sie sich bei [Partner Center](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) an, um zu bestätigen, dass Ihr Konto migriert wurde. Wenn ein Profilbildschirm mit Ihrer MPN-ID angezeigt wird, können Sie fortfahren. Wenn das nicht der Fall ist, müssen Sie mit der Kontomigration beginnen, indem Sie den Anweisungen im [Partner Membership Center](https://partners.microsoft.com/partnerprogram/Welcome.aspx) folgen. Wenn Sie Hilfe benötigen, besuchen Sie den [Support](https://partner.microsoft.com/support?issueid=100-0077).
2. Rufen Sie die [Seite mit dem Überblick über den kommerziellen Marketplace in Partner Center](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) auf. Wenn im linken Navigationsbereich „Kommerzieller Marketplace“ angezeigt wird, sind Sie registriert und sollten mit dem nächsten Schritt fortfahren. Wenn nicht, [registrieren Sie sich jetzt beim kommerziellen Marketplace](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv).
3. Vergewissern Sie sich, dass Ihre Angebote in AppSource vorhanden sind, indem Sie [nach Ihren Angeboten suchen](https://appsource.microsoft.com/). Sind Ihre Angebote bereits in AppSource vorhanden, fahren Sie mit dem nächsten Schritt fort. Für jedes Angebot, das nicht in AppSource enthalten ist, erstellen Sie ein [neues Dynamics 365 Customer Engagement-Angebot](create-new-customer-engagement-offer.md) oder ein [neues Dynamics 365 Operations-Angebot](create-new-operations-offer.md).
4. Stellen Sie sicher, dass Sie auf der [Seite „Vereinbarungen“](https://partner.microsoft.com/dashboard/account/agreements) in Partner Center das **Business Applications ISV Addendum** gelesen und akzeptiert haben.
5. Stellen Sie sicher, dass die Abrechnungsinformationen in den [Kontoeinstellungen](https://partner.microsoft.com/dashboard/account/v3/accountsettings/billingprofile) von Partner Center vollständig sind.
6. Übermitteln Sie jedes neue und bestehende Angebot zur Zertifizierung und Veröffentlichung, auch wenn Ihre Angebote bereits zuvor zertifiziert wurden.
    * Vervollständigen Sie die Informationen auf den Bildschirmen, einschließlich der Bereitstellung Ihrer App für die Zertifizierung, sowie Marketinginformationen. Wählen Sie **Senden** (obere rechte Ecke des Bildschirms) bis zum  **15. Oktober 2019** aus. Diese Schritte müssen ausgeführt werden, um eine Beeinträchtigung der Verfügbarkeit des Angebots zu vermeiden.
    * Wenn Sie dazu berechtigt sind, können Sie während dieses Prozesses die Teilnahme am Premium-Tarif beantragen.
    * Die Zertifizierung oder erneute Zertifizierung erfordert, dass Ihre App die neueste Version unserer Business Applications-Plattform unterstützt.
    * Nachdem Ihre App genehmigt wurde, erhalten Sie eine E-Mail. Kehren Sie darüber zum Angebot zurück, und wählen Sie „Live schalten“ aus, um das Angebot auf Microsoft AppSource live zu schalten.

## <a name="additional-resources"></a>Weitere Ressourcen

Nehmen Sie am wöchentlichen [Dynamics ISV-Community-Call](https://aka.ms/DynamicsISV-CommunityCall) teil, um Support und Updates zu erhalten.

Wenn Sie Hilfe bei der Veröffentlichung, Zertifizierung oder Verwaltung Ihrer Marketplace-Angebote benötigen, [senden Sie ein Supportticket](https://aka.ms/MarketplacePublisherSupport).

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten Ihres Kontos im kommerziellen Marketplace in Partner Center](./manage-account.md)
