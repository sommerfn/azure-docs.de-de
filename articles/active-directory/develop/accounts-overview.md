---
title: Konten und Mandantenprofile für Microsoft Identity Platform (Android) | Azure
description: Übersicht über Microsoft Identity Platform-Konten für Android
services: active-directory
documentationcenter: ''
author: shoatman
manager: nadima
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7beab6759524037f86c83429644c1bb1fffe4d07
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71678858"
---
# <a name="accounts--tenant-profiles-android"></a>Konten und Mandantenprofile (Android)

Dieser Artikel bietet einen Überblick darüber, was ein `account` auf der Microsoft Identity-Plattform bedeutet.

Die Microsoft Authentication Library (MSAL) API ersetzt den Begriff *Benutzer* durch den Begriff *Konto*. Ein Grund hierfür ist, dass ein Benutzer (Person oder Software-Agent) mehrere Konten besitzen bzw. nutzen kann. Diese Konten können sich in der benutzereigenen Organisation befinden und/oder in anderen Organisationen, in denen der Benutzer Mitglied ist.

Ein Microsoft Identity Platform-Konto umfasst folgende Komponenten:

  - Einen eindeutigen Bezeichner.
  - Anmeldeinformationen, um den Kontobesitzer bzw. die Befugnis zur Kontrolle zu bestätigen.
  - Ein oder mehrere Profile, die aus Attributen bestehen, z. B.:
    - Bild, Vorname, Familienname, Titel, Büro
- Ein Konto verfügt über eine Autoritätsquelle oder ein bevorzugtes System. Das Konto wird im bevorzugten System erstellt, und die diesem Konto zugeordneten Anmeldeinformationen sind dort gespeichert. In Systemen mit mehreren Mandanten, wie der Microsoft Identity Platform, ist das bevorzugte System der `tenant`, auf dem das Konto erstellt wurde. Dieser Mandant wird auch als „`home tenant`“ bezeichnet.
- Konten auf der Microsoft Identity Platform haben folgende bevorzugte Systeme:
  - Azure Active Directory, einschließlich Azure Active Directory B2C.
  - Microsoft-Konto (Live).
- Auch Konten von bevorzugten Systemen außerhalb der Microsoft Identity Platform sind auf derselben vertreten, einschließlich:
  - Identitäten aus verbundenen lokalen Verzeichnissen (Windows Server Active Directory)
  - externe Identitäten aus LinkedIn, GitHub usw.
  In diesen Fällen hat ein Konto sowohl ein bevorzugtes Ursprungssystem als auch ein bevorzugtes System auf der Microsoft Identity Platform.
- Die Microsoft Identity Platform ermöglicht mit nur einem Konto den Zugriff auf die Ressourcen mehrerer Organisationen (Azure Active Directory-Mandanten).
  - Um die Zugriffsberechtigung eines Kontos aus einem bevorzugten System (AAD-Mandant A) auf eine Ressource in einem anderen bevorzugten System (AAD-Mandant B) zu vermerken, muss das Konto in dem Mandanten dargestellt werden, in dem die Ressource definiert ist. Hierzu wird in System B ein lokaler Datensatz für das Konto aus System A erstellt.
  - Dieser lokale Datensatz stellt das Konto dar und ist an das ursprüngliche Konto gebunden.
  - MSAL macht diesen lokalen Datensatz als `Tenant Profile` verfügbar.
  - Mandantenprofile können über verschiedene, dem lokalen Kontext entsprechende Attribute verfügen, z. B. die Position, das Büro, Kontaktinformationen usw.
 
- Da ein Konto in einem oder mehreren Mandanten vorhanden sein kann, ist auch mehr als ein Profil möglich.

> [!NOTE]
> MSAL behandelt das System mit dem Microsoft-Konto (Live, MSA) als einen weiteren Mandanten innerhalb der Microsoft Identity Platform. Die Mandanten-ID des Mandanten mit Microsoft-Konto lautet: `9188040d-6c67-4c5b-b112-36a304b66dad`

## <a name="account-overview-diagram"></a>Diagramm zur Kontoübersicht

![Diagramm zur Kontoübersicht](./media/accounts-overview/accounts-overview.png)

Für dieses Diagramm gilt:

- Das Konto `bob@contoso.com` wird im lokalen Windows Server Active Directory (dem ursprünglichen lokalen bevorzugten System) erstellt.
- Das Konto `tom@live.com` wird im Microsoft-Konto-Mandanten erstellt.
- `bob@contoso.com` hat auf mindestens eine Ressource in den folgenden Azure Active Directory-Mandanten Zugriff:
  - contoso.com (bevorzugtes Cloudsystem – mit dem bevorzugten lokalen System verknüpft)
  - fabrikam.com
  - woodgrovebank.com
  - Ein Mandantenprofil für `bob@contoso.com` ist in jedem dieser Mandanten vorhanden.
- `tom@live.com` hat Zugriff auf Ressourcen in den folgenden Microsoft-Mandanten:
  - contoso.com
  - fabrikam.com
  - Ein Mandantenprofil für `tom@live.com` ist in jedem dieser Mandanten vorhanden.
- Informationen zu Tom und Bob in anderen Mandanten können von denen im bevorzugten System abweichen. Sie können sich durch Attribute wie z. B. Position, Büro usw. unterscheiden. Sie könnten in beiden Organisationen Gruppenmitglieder sein bzw. Rollen innehaben (Azure Active Directory-Mandant). Diese Informationen werden als bob@contoso.com-Mandantenprofil bezeichnet.

Im Diagramm haben bob@contoso.com und tom@live.com Zugriff auf Ressourcen in verschiedenen Azure Active Directory-Mandanten. Weitere Informationen finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

## <a name="accounts-and-single-sign-on-sso"></a>Konten und Einmaliges Anmelden (SSO)

Der MSAL-Cache speichert ein *einziges Aktualisierungstoken* pro Konto. Mit diesem Aktualisierungstoken können Zugriffstokens automatisch von mehreren Microsoft Identity Platform-Mandanten angefordert werden. Wenn ein auf einem Gerät ein Broker installiert ist, wird das Konto vom Broker verwaltet, und Einmaliges Anmelden ist geräteweit möglich.

> [!IMPORTANT]
> B2C (Business to Consumer)-Konten und Aktualisierungstokens verhalten sich anders als sonst auf der Microsoft Identity Platform. Weitere Informationen finden Sie unter [B2C-Richtlinien und -Konten](#b2c-policies--accounts).

## <a name="account-identifiers"></a>Kontokennungen

Die MSAL-Konto-ID ist keine Objekt-ID für das Konto. Sie dient ausschließlich dazu, innerhalb der Microsoft Identity Platform Eindeutigkeit zu vermitteln.

Um Kompatibilität mit der Azure AD Authentication Library (ADAL) zu gewährleisten und die Migration von ADAL zu MSAL zu erleichtern, kann MSAL Konten mithilfe eines beliebigen gültigen Bezeichners suchen, der im MSAL-Cache verfügbar ist.  Im folgenden Beispiel wird immer das gleiche Konto-Objekt für tom@live.com abgerufen, da jeder der Bezeichner gültig ist:

```java
// The following would always retrieve the same account object for tom@live.com because each identifier is valid

IAccount account = app.getAccount("<tome@live.com msal account id>");
IAccount account = app.getAccount("<tom@live.com contoso user object id>");
IAccount account = app.getAccount("<tom@live.com woodgrovebank user object id>");
```

## <a name="accessing-claims-about-an-account"></a>Zugriff auf mit dem Konto verbundene Ansprüche

Neben dem Zugriffstoken fordert MSAL auch immer ein ID-Token von jedem Mandanten an. Dies geschieht jeweils durch Anforderung folgender Bereiche:

- openid
- profile

Das ID-Token enthält eine Liste der Ansprüche. `Claims` sind Name-/Wertpaare zum Konto, die für die Anforderung genutzt werden.

Wie bereits erwähnt, kann jeder Mandant, in dem ein Konto vorhanden ist, unterschiedliche Informationen zum Konto speichern, z. B. Attribute wie Position, Büro usw.

Obwohl ein Konto in mehreren Organisationen Mitglied oder Gast sein kann, fragt MSAL beim jeweiligen Dienst keine Liste der Mandanten ab, bei denen das Konto Mitglied ist. Stattdessen erstellt MSAL anhand der eingegangenen Tokenanforderungen eine Liste der Mandanten, bei denen das Konto geführt wird.

Die für das Konto-Objekt verfügbar gemachten Ansprüche sind immer die Ansprüche der „Home-Mandant“/{Autorität} für ein Konto. Wenn dieses Konto nicht zum Anfordern eines Tokens für seinen Home-Mandanten verwendet wurde, kann MSAL keine Ansprüche über das Konto-Objekt bereitstellen.  Beispiel:

```java
// Psuedo Code
IAccount account = getAccount("accountid");

String username = account.getClaims().get("preferred_username");
String tenantId = account.getClaims().get("tid"); // tenant id
String objectId = account.getClaims().get("oid"); // object id
String issuer = account.getClaims().get("iss"); // The tenant specific authority that issued the id_token
```

> [!TIP]
> Eine Liste der Ansprüche, die im Konto-Objekt verfügbar sind, finden Sie unter [Ansprüche in einem id_token](https://docs.microsoft.com/azure/active-directory/develop/id-tokens#claims-in-an-id_token)

> [!TIP]
> Wenn Sie weitere Ansprüche in Ihr id_token einschließen möchten, lesen Sie die Dokumentation zu optionalen Ansprüchen in [So geht’s: Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)

### <a name="access-tenant-profile-claims"></a>Auf Ansprüche eines Mandantenprofils zugreifen

Um Ansprüche bezüglich ein Kontos so anzuzeigen wie in anderen Mandanten, müssen Sie zuerst Ihr Konto-Objekt in `IMultiTenantAccount` umwandeln. Alle Konten können mehrinstanzenfähig sein; die Anzahl der über MSAL verfügbaren Mandantenprofile richtet sich jedoch danach, von welchen Mandanten Sie von dem aktuellen Konto aus Tokens angefordert haben.  Beispiel:

```java
// Psuedo Code
IAccount account = getAccount("accountid");
IMultiTenantAccount multiTenantAccount = (IMultiTenantAccount)account;

multiTenantAccount.getTenantProfiles().get("tenantid for fabrikam").getClaims().get("family_name");
multiTenantAccount.getTenantProfiles().get("tenantid for contoso").getClaims().get("family_name");
```

## <a name="b2c-policies--accounts"></a>B2C-Richtlinien und -Konten

Aktualisierungstokens für ein Konto werden nicht zwischen verschiedenen B2C-Richtlinien geteilt. Daher ist das Einmalige Anmelden mit Tokens nicht möglich. Das heißt aber nicht, dass Einmaliges Anmelden nicht möglich ist. Vielmehr muss das Einmalige Anmelden interaktiv erfolgen, wobei ein Cookie zur Verfügung steht, um das Einmalige Anmelden zu aktivieren.

Für MSAL bedeutet dies weiterhin: Wenn Sie Token mit unterschiedlichen B2C-Richtlinien abrufen, werden diese als separate Konten behandelt, die jeweils über einen eigenen Bezeichner verfügen. Wenn Sie ein Konto verwenden möchten, um ein Token mithilfe von `acquireTokenSilent` anzufordern, müssen Sie das Konto aus der Liste der Konten auswählen, die der Richtlinie entspricht, die Sie für die Tokenanforderung verwenden. Beispiel:

```java
// Get Account For Policy

String policyId = "SignIn";
IAccount signInPolicyAccount = getAccountForPolicyId(app, policyId);

private IAccount getAccountForPolicy(IPublicClientApplication app, String policyId)
{
    List<IAccount> accounts = app.getAccounts();

    foreach(IAccount account : accounts)
   {
        if (account.getClaims().get("tfp").equals(policyId))
        {
            return account;
        }
    }

    return null;
}
```
