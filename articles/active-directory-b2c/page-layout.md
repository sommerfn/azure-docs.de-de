---
title: Auswählen eines Seitenlayouts – Azure Active Directory B2C
description: Es wird beschrieben, wie Sie in Azure Active Directory B2C ein Seitenlayout auswählen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 108d86e35422e1dc1d10aeb6b2c9488f5067232e
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389688"
---
# <a name="select-a-page-layout-in-azure-active-directory-b2c-using-custom-policies"></a>Auswählen eines Seitenlayouts in Azure Active Directory B2C mit benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Sie können den clientseitigen JavaScript-Code in Ihren Azure Active Directory B2C-Richtlinien (Azure AD B2C) aktivieren, und zwar unabhängig davon, ob Sie Benutzerflows oder benutzerdefinierte Richtlinien verwenden. Sie müssen ein Element zu Ihrer [benutzerdefinierten Richtlinie](active-directory-b2c-overview-custom.md) hinzufügen, ein Seitenlayout auswählen und [b2clogin.com](b2clogin.md) in Ihren Anforderungen verwenden, um JavaScript für Ihre Anwendungen zu aktivieren.

Ein Seitenlayout ist eine Zusammenstellung der von Azure AD B2C bereitgestellten Elemente und des von Ihnen bereitgestellten Inhalts.

In diesem Artikel wird das Auswählen eines Seitenlayouts in Azure AD B2C beschrieben, indem dieses in einer benutzerdefinierten Richtlinie konfiguriert wird.

> [!NOTE]
> Wenn Sie JavaScript für Benutzerflows aktivieren möchten, finden Sie Informationen unter [JavaScript und Seitenlayoutversionen in Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>Ersetzen von DataUri-Werten

In Ihren benutzerdefinierten Richtlinien verfügen Sie ggf. über [ContentDefinitions](contentdefinitions.md), mit denen die HTML-Vorlagen für die User Journey definiert werden. Das **ContentDefinition**-Element enthält einen **DataUri**, mit dem auf die von Azure AD B2C bereitgestellten Seitenelemente verwiesen wird. Der **LoadUri** ist der relative Pfad zum HTML- und CSS-Inhalt, den Sie bereitstellen.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Zum Auswählen eines Seitenlayouts ändern Sie die **DataUri**-Werte in den [ContentDefinitions](contentdefinitions.md)-Elementen Ihrer Richtlinien. Indem Sie von den alten **DataUri**-Werten zu den neuen Werten wechseln, wählen Sie ein unveränderliches Paket aus. Der Vorteil der Verwendung dieses Pakets besteht darin, dass Sie sicher sein können, dass es nicht zu Änderungen kommt und kein unerwartetes Verhalten für Ihre Seite auftritt.

Verwenden Sie zum Festlegen eines Seitenlayouts die folgende Tabelle, um nach **DataUri**-Werten zu suchen.

| Alter DataUri-Wert | Neuer DataUri-Wert |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Versionsänderungsprotokoll

Seitenlayoutpakete werden regelmäßig aktualisiert, um Korrekturen und Verbesserungen in ihre Seitenelemente aufzunehmen. Das folgende Änderungsprotokoll gibt die in den einzelnen Versionen eingeführten Änderungen an.

### <a name="120"></a>1.2.0 
- Alle Seiten
  - Korrekturen zur Barrierefreiheit
  - Sie können nun in den HTML-Tags das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern. Mögliche Szenarien:
      - Verwenden Sie dieses Attribut in Ihrem CSS-Link, um das CSS gleichzeitig mit dem HTML-Code zu laden, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
      - Mit diesem Attribut können Sie die Reihenfolge steuern, in der die „script“-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
  - Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
  - Unterstützung für den Google Chrome-Übersetzer
- Einheitliche und selbstbestätigte Seite
  - Für die Felder „Benutzername/E-Mail“ und „Kennwort“ wird jetzt der HTML-Befehl für Formular („form“) verwendet.  Dadurch können Edge und IE diese Informationen jetzt richtig speichern.
  
### <a name="110"></a>1.1.0

- Seite mit Ausnahmen (globalexception)
  - Korrektur zur Barrierefreiheit
  - Standardmeldung bei fehlendem Kontakt wurde aus der Richtlinie entfernt.
  - Standard-CSS wurde entfernt.
- MFA-Seite (Multi-Factor)
  - Schaltfläche zur Codebestätigung wurde entfernt.
  - Das Codeeingabefeld nimmt jetzt nur noch Eingaben mit bis zu sechs (6) Zeichen entgegen.
  - Die Seite versucht automatisch, den eingegebenen Code zu überprüfen, wenn ein 6-stelliger Code eingegeben wird, ohne dass auf eine Schaltfläche geklickt werden muss.
  - Wenn der Code falsch ist, wird das Eingabefeld automatisch gelöscht.
  - Nach drei (3) Versuchen mit einem falschen Code sendet B2C eine Fehlermeldung an die vertrauende Seite.
  - Korrekturen zur Barrierefreiheit
  - Standard-CSS wurde entfernt.
- Selbstbestätigte Seite (selbstbestätigt)
  - Abbruchwarnung wurde entfernt.
  - CSS-Klasse für Fehlerelemente
  - Anzeigen/Ausblenden der Fehlerlogik wurde verbessert.
  - Standard-CSS wurde entfernt.
- SSP vereinheitlicht (unifiedssp)
  - „Angemeldet bleiben“-Steuerelement (Keep Me Signed In, KMSI) wurde hinzugefügt.

### <a name="100"></a>1.0.0

- Erste Version

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).
