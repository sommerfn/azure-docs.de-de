---
title: Grundlegendes zur Android-Konfigurationsdatei für die Microsoft Authentication Library (MSAL)
titleSuffix: Microsoft identity platform
description: Eine Übersicht über die Android-Konfigurationsdatei für die Microsoft Authentication Library (MSAL), die die Konfiguration einer Anwendung in Azure Active Directory darstellt.
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
ms.date: 09/12/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.collection: M365-identity-device-management
ms.openlocfilehash: c06bc1ebf3b87f622029e9e875fe478eae7e6a30
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803151"
---
# <a name="android-microsoft-authentication-library-msal-configuration-file"></a>Android-Konfigurationsdatei für die Microsoft Authentication Library (MSAL)

Die MSAL wird mit einer [JSON-Standardkonfigurationsdatei](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json) ausgeliefert, die Sie anpassen, um das Verhalten Ihrer öffentlichen Client-App zu definieren, z.B. als Standardautorität, die von Ihnen verwendeten Autoritäten usw.

Dieser Artikel soll Ihnen helfen, die verschiedenen Einstellungen in der Konfigurationsdatei zu verstehen, und es wird darin erläutert, wie Sie die Konfigurationsdatei zur Verwendung in Ihrer MSAL-basierten App angeben.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

### <a name="general-settings"></a>Allgemeine Einstellungen

| Eigenschaft | Datentyp | Erforderlich | Notizen |
|-----------|------------|-------------|-------|
| `client_id` | Zeichenfolge | Ja | Die Client-ID Ihrer App von der [Seite für die Anwendungsregistrierung](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) |
| `redirect_uri`   | Zeichenfolge | Ja | Der Umleitungs-URI Ihrer App von der [Seite für die Anwendungsregistrierung](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) |
| `authorities` | List\<Authority> | Nein | Die Liste der von Ihrer App benötigten Autoritäten |
| `authorization_user_agent` | AuthorizationAgent (enum) | Nein | Mögliche Werte: `DEFAULT`, `BROWSER`, `WEBVIEW` |
| `http` | HttpConfiguration | Nein | Konfigurieren Sie `HttpUrlConnection` `connect_timeout` und `read_timeout`. |
| `logging` | LoggingConfiguration | Nein | Gibt die Stufe des Protokollierungsdetails an. Optionale Konfigurationen umfassen: `pii_enabled`, die einen booleschen Wert annimmt, und `log_level`, die `ERROR`, `WARNING`, `INFO` oder `VERBOSE` annimmt. |

### <a name="client_id"></a>client_id

Die Client-ID oder App-ID, die beim Registrieren Ihrer Anwendung erstellt wurde.

### <a name="redirect_uri"></a>redirect_uri

Der Umleitungs-URI, den Sie beim Registrieren Ihrer Anwendung registriert haben. Wenn der Umleitungs-URI zu einer Broker-App gehört, lesen Sie [Umleitungs-URI für öffentliche Client-Apps](msal-client-application-configuration.md#redirect-uri-for-public-client-apps), um sicherzustellen, dass Sie das richtige Umleitungs-URI-Format für Ihre Broker-App verwenden.

### <a name="authorities"></a>authorities

Die Liste von Autoritäten, die Ihnen bekannt sind und von Ihnen als vertrauenswürdig eingestuft werden. Zusätzlich zu den hier aufgelisteten Autoritäten fragt die MSAL auch Microsoft ab, um eine Liste der Clouds und Autoritäten zu erhalten, die Microsoft bekannt sind. Geben Sie in dieser Liste von Autoritäten den Typ der Autorität und alle zusätzlichen optionalen Parameter an, z.B. `"audience"`, die auf der Grundlage der Registrierung Ihrer App mit der Zielgruppe Ihrer App übereinstimmen sollten. Im Folgenden finden Sie eine Beispielliste von Autoritäten:

```javascript
// Example AzureAD and Personal Microsoft Account
{
    "type": "AAD",
    "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
    },
    "default": true // Indicates that this is the default to use if not provided as part of the acquireToken or acquireTokenSilent call
},
// Example AzureAD My Organization
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMyOrg",
        "tenantId": "contoso.com" // Provide your specific tenant ID here
    }
},
// Example AzureAD Multiple Organizations
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMultipleOrgs"
    }
},
//Example PersonalMicrosoftAccount
{
    "type": "AAD",
    "audience": {
        "type": "PersonalMicrosoftAccount"
    }
}
```

#### <a name="map-aad-authority--audience-to-microsoft-identity-platform-endpoints"></a>Zuordnen der AAD-Autorität und -Zielgruppe zu Microsoft Identity Platform-Endpunkten

| type | Zielgruppe | Mandanten-ID | Authority_Url | Resultierender Endpunkt | Notizen |
|------|------------|------------|----------------|----------------------|---------|
| AAD | AzureADandPersonalMicrosoftAccount | | | https://login.microsoftonline.com/common | `common` ist ein Mandantenalias für den Speicherort des Kontos. Dies kann beispielsweise ein bestimmter Azure Active Directory-Mandant oder das Microsoft-Kontosystem sein. |
| AAD | AzureADMyOrg | contoso.com | | https://login.microsoftonline.com/contoso.com | Nur Konten, die in „contoso.com“ vorhanden sind, können ein Token abrufen. Jede beliebige überprüfte Domäne oder die Mandanten-GUID kann als Mandanten-ID verwendet werden. |
| AAD | AzureADMultipleOrgs | | | https://login.microsoftonline.com/organizations | Nur Azure Active Directory-Konten können bei diesem Endpunkt verwendet werden. Microsoft-Konten können Mitglieder von Organisationen sein. Wenn Sie ein Token über ein Microsoft-Konto für eine Ressource in einer Organisation abrufen möchten, geben Sie den Organisationsmandanten an, dessen Token Sie verwenden möchten. |
| AAD | PersonalMicrosoftAccount | | | https://login.microsoftonline.com/consumers | Dieser Endpunkt kann nur von Microsoft-Konten verwendet werden. |
| B2C | | | Anzeigen des resultierenden Endpunkts | https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/ | Nur Konten, die im Mandanten „contoso.onmicrosoft.com“ vorhanden sind, können ein Token abrufen. In diesem Beispiel ist die B2C-Richtlinie Teil des Autoritäts-URL-Pfads. |

> [!NOTE]
> Die Autoritätsüberprüfung kann in der MSAL nicht aktiviert und deaktiviert werden.
> Autoritäten sind Ihnen entweder als Entwickler bekannt, wie über die Konfiguration festgelegt wurde, oder sie sind Microsoft über Metadaten bekannt.
> Wenn die MSAL eine Anforderung für ein Token an eine unbekannte Autorität erhält, wird eine `MsalClientException` vom Typ `UnknownAuthority` ausgegeben.

#### <a name="authority-properties"></a>Eigenschaften von Autoritäten

| Eigenschaft | Datentyp  | Erforderlich | Notizen |
|-----------|-------------|-----------|--------|
| `type` | Zeichenfolge | Ja | Entspricht der Zielgruppe oder dem Kontotyp Ihrer App-Ziele. Mögliche Werte: `AAD`, `B2C` |
| `audience` | Object | Nein | Gilt nur, wenn „type“=`AAD` ist. Gibt die Identität ihrer App-Ziele an. Verwenden Sie den Wert aus Ihrer App-Registrierung. |
| `authority_url` | Zeichenfolge | Ja | Nur erforderlich, wenn „type“=`B2C` ist. Gibt die Autoritäts-URL oder Richtlinie an, die Ihre App verwenden sollte.  |
| `default` | boolean | Ja | Wenn eine oder mehrere Autoritäten angegeben werden, ist ein einzelnes `"default":true` erforderlich. |

#### <a name="audience-properties"></a>Eigenschaften der Zielgruppe

| Eigenschaft | Datentyp  | Erforderlich | Notizen |
|-----------|-------------|------------|-------|
| `type` | Zeichenfolge | Ja | Gibt die Zielgruppe an, auf die Ihre App abzielen möchte. Mögliche Werte: `AzureADandPersonalMicrosoftAccount`, `PersonalMicrosoftAccount`, `AzureADMultipleOrgs`, `AzureADMyOrg` |
| `tenant_id` | Zeichenfolge | Ja | Nur erforderlich, wenn `"type":"AzureADMyOrg"` ist. Optional bei anderen `type`-Werten. Dies kann eine Mandantendomäne, z.B. `contoso.com`, oder eine Mandanten-ID sein, z.B. `72f988bf-86f1-41af-91ab-2d7cd011db46`. |

### <a name="authorization_user_agent"></a>authorization_user_agent

Gibt an, ob eine eingebettete Webansicht oder der Standardbrowser auf dem Gerät verwendet werden soll, wenn ein Konto angemeldet oder der Zugriff auf eine Ressource autorisiert wird.

Mögliche Werte:
- `DEFAULT`: Bevorzugt den Systembrowser. Verwendet die eingebettete Webansicht, wenn kein Browser auf dem Gerät verfügbar ist.
- `WEBVIEW`: Verwendet die eingebettete Webansicht.
- `BROWSER`: Verwendet den Standardbrowser auf dem Gerät.

### <a name="multiple_clouds_supported"></a>multiple_clouds_supported

Geben Sie bei Clients, die mehrere nationale Clouds unterstützen, `true` an. Die Microsoft Identity Platform leitet dann während der Autorisierung und der Einlösung von Token automatisch zur richtigen nationalen Cloud um. Sie können die nationale Cloud des angemeldeten Kontos ermitteln, indem Sie die dem `AuthenticationResult` zugeordnete Autorität untersuchen. Beachten Sie, dass das `AuthenticationResult` nicht die für die nationale Cloud spezifische Endpunktadresse der Ressource liefert, für die Sie ein Token anfordern.

### <a name="broker_redirect_uri_registered"></a>broker_redirect_uri_registered

Ein boolescher Wert, der angibt, ob Sie einen mit Microsoft-Identitätsbroker kompatiblen Umleitungs-URI für den Broker verwenden. Legen Sie den Wert auf `false` fest, wenn Sie den Broker nicht innerhalb Ihrer App verwenden möchten.

Wenn Sie die AAD-Autorität verwenden, bei der die Zielgruppe auf `"MicrosoftPersonalAccount"` festgelegt wurde, wird der Broker nicht verwendet.

### <a name="http"></a>http

Konfigurieren Sie globale Einstellungen für HTTP-Zeitlimits, z.B.:

| Eigenschaft | Datentyp | Erforderlich | Notizen |
| ---------|-----------|------------|--------|
| `connect_timeout` | int | Nein | Zeit in Millisekunden |
| `read_timeout` | int | Nein | Zeit in Millisekunden |

### <a name="logging"></a>logging

Die folgenden globalen Einstellungen gelten für die Protokollierung:

| Eigenschaft | Datentyp  | Erforderlich | Notizen |
| ----------|-------------|-----------|---------|
| `pii_enabled`  | boolean | Nein | Gibt an, ob persönliche Daten ausgegeben werden sollen. |
| `log_level`   | boolean | Nein | Gibt an, welche Protokollmeldungen ausgegeben werden sollen. |
| `logcat_enabled` | boolean | Nein | Gibt an, ob die Ausgabe an die Protokollkategorie zusätzlich zur Protokollierungsschnittstelle ausgegeben werden soll. |

### <a name="account_mode"></a>account_mode

Gibt an, wie viele Konten in Ihrer App gleichzeitig verwendet werden können. Mögliche Werte:

- `MULTIPLE` (Standard)
- `SINGLE`

Das Erstellen einer `PublicClientApplication` unter Verwendung eines Kontomodus, der dieser Einstellung nicht entspricht, führt zu einer Ausnahme.

Weitere Informationen zu den Unterschieden zwischen einzelnen und mehreren Konten finden Sie unter [Apps für einzelne und mehrere Konten](single-multi-account.md).

### <a name="browser_safelist"></a>browser_safelist

Eine Zulassungsliste der mit der MSAL kompatiblen Browser. Diese Browser verarbeiten ordnungsgemäß Umleitungen an benutzerdefinierte Absichten. Sie können sie dieser Liste hinzufügen. Der Standardwert wird in der unten gezeigten Standardkonfiguration bereitgestellt.
``
## <a name="the-default-msal-configuration-file"></a>Die MSAL-Standardkonfigurationsdatei

Die MSAL-Standardkonfiguration, die mit MSAL ausgeliefert wird, ist unten dargestellt. Die neueste Version können Sie auf [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json) sehen.

Diese Konfiguration wird durch von Ihnen bereitgestellte Werte ergänzt. Die von Ihnen bereitgestellten Werte setzen die Standardwerte außer Kraft.

```javascript
{
  "authorities": [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      },
      "default": true
    }
  ],
  "authorization_user_agent": "DEFAULT",
  "multiple_clouds_supported": false,
  "broker_redirect_uri_registered": false,
  "http": {
    "connect_timeout": 10000,
    "read_timeout": 30000
  },
  "logging": {
    "pii_enabled": false,
    "log_level": "WARNING",
    "logcat_enabled": false
  },
  "shared_device_mode_supported": false,
  "account_mode": "MULTIPLE",
  "browser_safelist": [
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "45"
    },
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "57"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "4.0"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.cloudmosa.puffinFree",
      "browser_signature_hashes": [
        "1WqG8SoK2WvE4NTYgr2550TRhjhxT-7DWxu6C_o6GrOLK6xzG67Hq7GCGDjkAFRCOChlo2XUUglLRAYu3Mn8Ag=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.duckduckgo.mobile.android",
      "browser_signature_hashes": [
        "S5Av4cfEycCvIvKPpKGjyCuAE5gZ8y60-knFfGkAEIZWPr9lU5kA7iOAlSZxaJei08s0ruDvuEzFYlmH-jAi4Q=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.explore.web.browser",
      "browser_signature_hashes": [
        "BzDzBVSAwah8f_A0MYJCPOkt0eb7WcIEw6Udn7VLcizjoU3wxAzVisCm6bW7uTs4WpMfBEJYf0nDgzTYvYHCag=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.ksmobile.cb",
      "browser_signature_hashes": [
        "lFDYx1Rwc7_XUn4KlfQk2klXLufRyuGHLa3a7rNjqQMkMaxZueQfxukVTvA7yKKp3Md3XUeeDSWGIZcRy7nouw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.microsoft.emmx",
      "browser_signature_hashes": [
        "Ivy-Rk6ztai_IudfbyUrSHugzRqAtHWslFvHT0PTvLMsEKLUIgv7ZZbVxygWy_M5mOPpfjZrd3vOx3t-cA6fVQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.browser",
      "browser_signature_hashes": [
        "FIJ3IIeqB7V0qHpRNEpYNkhEGA_eJaf7ntca-Oa_6Feev3UkgnpguTNV31JdAmpEFPGNPo0RHqdlU0k-3jWJWw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.mini.native",
      "browser_signature_hashes": [
        "TOTyHs086iGIEdxrX_24aAewTZxV7Wbi6niS2ZrpPhLkjuZPAh1c3NQ_U4Lx1KdgyhQE4BiS36MIfP6LbmmUYQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "mobi.mgeek.TunnyBrowser",
      "browser_signature_hashes": [
        "RMVoXuK1sfJZuGZ8onG1yhMc-sKiAV2NiB_GZfdNlN8XJ78XEE2wPM6LnQiyltF25GkHiPN2iKQiGwaO2bkyyQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "org.mozilla.focus",
      "browser_signature_hashes": [
        "L72dT-stFqomSY7sYySrgBJ3VYKbipMZapmUXfTZNqOzN_dekT5wdBACJkpz0C6P0yx5EmZ5IciI93Q0hq0oYA=="
      ],
      "browser_use_customTab" : false
    }
  ]
}
```
## <a name="example-basic-configuration"></a>Beispiel für eine Standardkonfiguration

Das folgende Beispiel veranschaulicht eine Standardkonfiguration, die die Client-ID, den Umleitungs-URI, ob eine Brokerumleitung registriert ist, und eine Liste von Autoritäten angibt.

```javascript
{
  "client_id" : "4b0db8c2-9f26-4417-8bde-3f0e3656f8e0",
  "redirect_uri" : "msauth://com.microsoft.identity.client.sample.local/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      }
      "default": true
    }
  ]
}
```

## <a name="how-to-use-a-configuration-file"></a>Verwenden einer Konfigurationsdatei

1. Erstellen Sie eine Konfigurationsdatei. Es wird empfohlen, dass Sie Ihre benutzerdefinierte Konfigurationsdatei in `res/raw/auth_config.json` erstellen. Sie können sie aber an einem beliebigen Ort speichern.
2. Informieren Sie die MSAL, wo sie nach ihrer Konfiguration suchen soll, wenn Sie die `PublicClientApplication` erstellen. Beispiel:

   ```java
   //On Worker Thread
   IMultipleAccountPublicClientApplication sampleApp = null; 
   sampleApp = new PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.auth_config);
   ```
