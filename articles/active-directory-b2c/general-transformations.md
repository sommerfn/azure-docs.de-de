---
title: Beispiele für die Transformation allgemeiner Ansprüche für das Identity Experience Framework-Schema von Azure Active Directory B2C
description: Hier finden Sie Beispiele für die Transformation allgemeiner Ansprüche für das Identity Experience Framework-Schema von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/27/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7cea33cb61f8f8d0fe305a757f11c80bc5da24ca
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70032896"
---
# <a name="general-claims-transformations"></a>Transformationen allgemeiner Ansprüche

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel enthält Beispiele für die Verwendung von Transformationen allgemeiner Ansprüche für das Identity Experience Framework-Schema in Azure Active Directory B2C (Azure AD B2C). Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Überprüft, ob **inputClaim** vorhanden ist oder nicht, und legt **outputClaim** auf TRUE bzw. FALSE fest.

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |Any | Der Eingabeanspruch, dessen Vorhandensein überprüft werden muss. |
| OutputClaim | outputClaim | boolean | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Mithilfe dieser Anspruchstransformation können Sie überprüfen, ob ein Anspruch vorhanden ist oder einen beliebigen Wert enthält. Der Rückgabewert ist ein boolescher Wert, der angibt, ob der Anspruch vorhanden ist. Im folgenden Beispiel wird geprüft, ob die E-Mail-Adresse vorhanden ist.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **inputClaim**: someone@contoso.com
- Ausgabeansprüche:
  - **outputClaim**: true

## <a name="hash"></a>Hash

Hash des angegebenen Nur-Texts, der Salt und einen geheimen Schlüssel verwendet. Der verwendete Hashalgorithmus lautet SHA-256.

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | plaintext | Zeichenfolge | Der zu verschlüsselnde Eingabeanspruch |
| InputClaim | Salt | Zeichenfolge | Der Salt-Parameter. Sie können mit der `CreateRandomString`-Anspruchstransformation einen Zufallswert erstellen. |
| InputParameter | randomizerSecret | Zeichenfolge | Verweist auf einen vorhandenen Azure AD B2C-**Richtlinienschlüssel**. Gehen Sie wie folgt vor, um einen neuen Richtlinienschlüssel zu erstellen: Wählen Sie in Ihrem Azure AD B2C-Mandanten unter **Verwalten** die Option **Identity Experience Framework** aus. Wählen Sie **Richtlinienschlüssel** aus, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen. Wählen Sie **Hinzufügen**. Wählen Sie unter **Optionen** den Eintrag **Manuell** aus. Geben Sie einen Namen an (das Präfix *B2C_1A_* wird möglicherweise automatisch hinzugefügt). Geben Sie im Textfeld **Geheimnis** einen geheimen Schlüssel ein, der verwendet werden soll, z. B. „1234567890“. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus. Klicken Sie auf **Erstellen**. |
| OutputClaim | hash | Zeichenfolge | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. Der im Eingabeanspruch `plaintext` konfigurierte Anspruch. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **plaintext**: MyPass@word1
  - **salt**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Ausgabeansprüche:
  - **outputClaim:** CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=
