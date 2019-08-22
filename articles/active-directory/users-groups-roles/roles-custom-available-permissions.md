---
title: Benutzerdefinierte Administratorrollenberechtigungen zur App-Registrierungsverwaltung – Azure Active Directory | Microsoft-Dokumentation
description: Benutzerdefinierte Administratorrollenberechtigungen zur Delegierung der Identitätsverwaltung.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99f31c5928273973a9089ae9ef1fd184cdb78bbb
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69033318"
---
# <a name="application-registration-subtypes-and-permissions-in-azure-active-directory"></a>Anwendungsregistrierungsuntertypen und -berechtigungen in Azure Active Directory

Dieser Artikel enthält die derzeit verfügbaren Anwendungsregistrierungsberechtigungen für benutzerdefinierte Rollendefinitionen in Azure Active Directory (Azure AD).

## <a name="single-tenant-v-multi-tenant-permissions"></a>Berechtigungen für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen

Benutzerdefinierte Rollenberechtigungen unterscheiden sich für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Einzelmandantenanwendungen sind nur für Benutzer in der Azure AD-Organisation verfügbar, in der die Anwendung registriert ist. Mehrinstanzenfähige Anwendungen sind für alle Azure AD-Organisationen verfügbar. Per Definition ist bei Einzelmandantenanwendungen **Unterstützte Kontotypen** auf „Nur Konten in diesem Organisationsverzeichnis“ festgelegt. In der Graph-API ist für Einzelmandantenanwendungen die signInAudience-Eigenschaft auf „AzureADMyOrg“ festgelegt.

## <a name="application-registration-subtypes-and-permissions"></a>Anwendungsregistrierungsuntertypen und -berechtigungen

Eine Erläuterung der allgemeinen Begriffe „Untertyp“, „Berechtigung“ und „Eigenschaftensatz“ finden Sie in der [Übersicht über benutzerdefinierte Rollen](roles-custom-overview.md). Die folgenden Informationen gelten speziell für Anwendungsregistrierungen.

### <a name="subtypes"></a>Untertypen

Es ist nur ein Anwendungsregistrierungsuntertyp vorhanden: applications.myOrganization. Beispiel: microsoft.directory/applications.myOrganization/basic/update. Dieser Untertyp wird auf der Seite **Authentifizierung** für eine bestimmte Anwendungsregistrierung festgelegt und entspricht der Festlegung der signInAudience-Eigenschaft auf „AzureADMyOrg“ in der Graph-API oder mit PowerShell. Der Untertyp beschränkt die Berechtigung auf Anwendungsregistrierungen, die als nur für Konten in Ihrer Organisation zugänglich markiert sind (Einzelmandantenanwendungen).

Sie können die eingeschränkte Berechtigung verwenden, um Lese- oder Verwaltungsberechtigungen nur für interne Anwendungen zu erteilen, ohne Lese- oder Verwaltungsberechtigungen für Anwendungen zu erteilen, die über Konten in anderen Organisationen zugänglich sind.

Es gibt Versionen von applications.myOrganization für alle Lese- und Aktualisierungsberechtigungen sowie für die Löschberechtigung. Derzeit ist keine applications.myOrganization-Version für die Berechtigung zum Erstellen verfügbar. Standardberechtigungen (z. B. microsoft.directory/applications/basic/update) erteilen Lese- oder Verwaltungsberechtigungen für alle App-Registrierungstypen.

![Deklarieren einer Einzelmandantenanwendung oder mehrinstanzenfähigen Anwendung](./media/roles-custom-available-permissions/supported-account-types.png)

Details zu den folgenden Berechtigungen für die benutzerdefinierten Rollen (Vorschau) sind unter [Verfügbare benutzerdefinierte Rollenberechtigungen in Azure Active Directory](roles-custom-available-permissions.md) aufgeführt.

### <a name="create-and-delete"></a>Erstellen und Löschen

Für die Erstellung von Anwendungsregistrierungen stehen zwei Berechtigungen zur Verfügung, die jeweils ein anderes Verhalten aufweisen:

- **microsoft.directory/applications/createAsOwner**: Wird diese Berechtigung zugewiesen, wird der Ersteller als erster Besitzer der erstellten App-Registrierung hinzugefügt, und die erstellte App-Registrierung zählt zum Objekterstellungskontingent des Erstellers (250 Objekte).
- **microsoft.directory/applicationPolicies/create**: Wird diese Berechtigung zugewiesen, wird der Ersteller nicht als erster Besitzer der erstellten App-Registrierung hinzugefügt, und die erstellte App-Registrierung zählt nicht zum Kontingent von 250 erstellten Objekten des Erstellers. Verwenden Sie diese Berechtigung mit Bedacht, da der zugewiesene Benutzer in diesem Fall so viele App-Registrierungen erstellen kann, bis das Kontingent auf der Verzeichnisebene erreicht ist. Sind beide Berechtigungen zugewiesen, hat diese Berechtigung Vorrang.

Wenn beide Berechtigungen zugewiesen sind, hat die Berechtigung zum Erstellen („/create“) Vorrang. Obwohl mit der Berechtigung „/createAsOwner“ der Ersteller nicht automatisch als erster Besitzer hinzufügt wird, können Besitzer bei der Erstellung der App-Registrierung angegeben werden, wenn Graph-APIs oder PowerShell-Cmdlets verwendet werden.

Berechtigungen zum Erstellen gewähren Zugriff auf den Befehl **Neue Registrierung**.

[Diese Berechtigungen gewähren Zugriff auf den Befehl „Neue Registrierung“ im Portal.](./media/roles-create-custom/new-custom-role.png)

Für die Erteilung der Möglichkeit zum Löschen von Anwendungsregistrierungen stehen zwei Berechtigungen zur Verfügung:

#### <a name="microsoftdirectoryapplicationsdelete"></a>microsoft.directory/applications/delete

Erteilt die Möglichkeit zum Löschen von Anwendungsregistrierungen unabhängig vom Untertyp, d. h. für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen.

#### <a name="microsoftdirectoryapplicationsmyorganizationdelete"></a>microsoft.directory/applications.myOrganization/delete

Erteilt die Möglichkeit zum Löschen von Anwendungsregistrierungen mit Beschränkung auf die Registrierungen, die nur für Konten in Ihrer Organisation oder Einzelmandantenanwendungen zugänglich sind (Untertyp myOrganization).

![Diese Berechtigungen gewähren Zugriff auf den Befehl „App-Registrierung löschen“.](./media/roles-custom-available-permissions/delete-app-registration.png)

> [!NOTE]
> Beim Zuweisen einer Rolle, die Berechtigungen zum Erstellen enthält, muss die Rollenzuweisung im Verzeichnisbereich vorgenommen werden. Eine Berechtigung zum Erstellen, die in einem Ressourcenbereich zugewiesen wird, erteilt nicht die Möglichkeit zum Erstellen von Anwendungsregistrierungen.

### <a name="read"></a>Lesen

Alle Mitgliedsbenutzer in der Organisation können standardmäßig App-Registrierungsinformationen lesen. Gastbenutzer und Anwendungsdienstprinzipale können dies dagegen nicht. Wenn Sie einem Gastbenutzer oder einer Anwendung eine Rolle zuweisen möchten, müssen Sie die entsprechenden Leseberechtigungen einfügen.

#### <a name="microsoftdirectoryapplicationsallpropertiesread"></a>microsoft.directory/applications/allProperties/read

Berechtigung zum Lesen aller Eigenschaften von Einzelmandantenanwendungen und mehrinstanzenfähigen Anwendungen, mit Ausnahme von vertraulichen Eigenschaften wie z. B. Anmeldeinformationen.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesread"></a>microsoft.directory/applications.myOrganization/allProperties/read

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/allProperties/read, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsstandardread-grants-access-to-all-fields-on-the-application-registration-branding-page"></a>microsoft.directory/applications/standard/read: Gewährt Zugriff auf alle Felder auf der Seite „Branding“ der Anwendungsregistrierung.

![Diese Berechtigung gewährt Zugriff auf die Seite „Branding“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-branding.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationstandardread"></a>microsoft.directory/applications.myOrganization/standard/read

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/standard/read, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsownersread"></a>microsoft.directory/applications/owners/read

Erteilt die Berechtigung zum Lesen der Eigenschaft „Besitzer“ für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Gewährt Zugriff auf alle Felder auf der Seite „Besitzer“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „Besitzer“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-owners.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsentität:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppCreatedDateTime
- AppData
- AppId
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- WebApp
- WwwHomepage

### <a name="update"></a>Aktualisieren

#### <a name="microsoftdirectoryapplicationsallpropertiesupdate"></a>microsoft.directory/applications/allProperties/update

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesupdate"></a>microsoft.directory/applications.myOrganization/allProperties/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/allProperties/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsaudienceupdate"></a>microsoft.directory/applications/audience/update

Gewährt Zugriff auf alle Felder auf der Seite „Authentifizierung“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „Authentifizierung“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/supported-account-types.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:

- AvailableToOtherTenants
- SignInAudience

#### <a name="microsoftdirectoryapplicationsmyorganizationaudienceupdate"></a>microsoft.directory/applications.myOrganization/audience/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/audience/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsauthenticationupdate"></a>microsoft.directory/applications/authentication/update

Berechtigung zum Aktualisieren der Eigenschaften der Antwort-URL, der Abmelde-URL, des impliziten Ablaufs und der Herausgeberdomäne für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Gewährt Zugriff auf alle Felder auf der Seite „Authentifizierung“ der Anwendungsregistrierung, außer auf „Unterstützte Kontotypen“:

![Gewährt Zugriff auf die Authentifizierung der Anwendungsregistrierung, jedoch nicht auf unterstützte Kontotypen.](./media/roles-custom-available-permissions/supported-account-types.png)

 Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:

- AcceptMappedClaims
- AccessTokenAcceptedVersion
- AddIns
- GroupMembershipClaims
- IsDeviceOnlyAuthSupported
- OAuth2LegacyUrlPathMatching
- OauthOidcResponsePolicyBitmap
- OptionalClaims
- OrgRestrictions
- PublicClient
- UseCustomTokenSigningKey

#### <a name="microsoftdirectoryapplicationsmyorganizationauthenticationupdate"></a>microsoft.directory/applications.myOrganization/authentication/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/authentication/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsbasicupdate"></a>microsoft.directory/applications/basic/update

Berechtigung zum Aktualisieren der Eigenschaften des Namens, des Logos, der URL der Startseite, der URL zu den Vertragsbedingungen und der URL zur Datenschutzerklärung für Einzelmandantenanwendungen oder mehrinstanzenfähige Anwendungen. Gewährt Zugriff auf alle Felder auf der Seite „Branding“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „Branding“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-branding.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppData
- AppId
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- WebApp
- WwwHomepage

#### <a name="microsoftdirectoryapplicationsmyorganizationbasicupdate"></a>microsoft.directory/applications.myOrganization/basic/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/basic/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationscredentialsupdate"></a>microsoft.directory/applications/credentials/update

Berechtigung zum Aktualisieren der Eigenschaften der Zertifikate und der geheimen Clientschlüssel für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Gewährt Zugriff auf alle Felder auf der Seite „Zertifikate & Geheimnisse“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „Zertifikate & Geheimnisse“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-secrets.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:
- AsymmetricKey
- EncryptedSecretKey
- KeyDescription
- SharedKeyReference
- TokenEncryptionKeyId

#### <a name="microsoftdirectoryapplicationsmyorganizationcredentialsupdate"></a>microsoft.directory/applications.myOrganization/credentials/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/credentials/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationsownersupdate"></a>microsoft.directory/applications/owners/update

Berechtigung zum Aktualisieren der Eigenschaft „Besitzer“ für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Gewährt Zugriff auf alle Felder auf der Seite „Besitzer“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „Besitzer“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-owners.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:
- Besitzer

#### <a name="microsoftdirectoryapplicationsmyorganizationownersupdate"></a>microsoft.directory/applications.myOrganization/owners/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/owners/update, jedoch nur für Einzelmandantenanwendungen.

#### <a name="microsoftdirectoryapplicationspermissionsupdate"></a>microsoft.directory/applications/permissions/update

Berechtigungen zum Aktualisieren der Eigenschaften der delegierten Berechtigungen, Anwendungsberechtigungen, autorisierten Clientanwendungen, erforderlichen Berechtigungen und der Erteilung der Einwilligung für Einzelmandantenanwendungen und mehrinstanzenfähige Anwendungen. Erteilt nicht die Berechtigung zum Durchführen der Einwilligung. Gewährt Zugriff auf alle Felder auf den Seiten „API-Berechtigungen“ und „Eine API verfügbar machen“ der Anwendungsregistrierung:

![Diese Berechtigung gewährt Zugriff auf die Seite „API-Berechtigungen“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-api-permissions.png)

![Diese Berechtigung gewährt Zugriff auf die Seite „Eine API verfügbar machen“ der Anwendungsregistrierung.](./media/roles-custom-available-permissions/app-registration-expose-api.png)

Gewährt Zugriff auf die folgenden Eigenschaften für die Anwendungsressource:

- AppIdentifierUri
- Entitlement
- PreAuthorizedApplications
- RecordConsentConditions
- RequiredResourceAccess

#### <a name="microsoftdirectoryapplicationsmyorganizationpermissionsupdate"></a>microsoft.directory/applications.myOrganization/permissions/update

Erteilt die gleichen Berechtigungen wie microsoft.directory/applications/permissions/update, jedoch nur für Einzelmandantenanwendungen.

## <a name="required-license-plan"></a>Erforderlicher Lizenzplan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erstellen von benutzerdefinierten Rollen über das [Azure-Portal, Azure AD PowerShell und die Graph-API](roles-create-custom.md)
- [Anzeigen der Zuweisungen für eine benutzerdefinierte Rolle](roles-view-assignments.md)
