---
title: Client-Assertionen in der Microsoft Authentication Library für .NET
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie mehr über signierte Client-Assertionen für vertrauliche Clientanwendungen in der Microsoft Authentication Library für .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcf11ac8dc39dcb1d70b932dbe870687f5446a52
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802850"
---
# <a name="confidential-client-assertions"></a>Assertionen für vertrauliche Clients
Um ihre Identität nachzuweisen, tauschen vertrauliche Clientanwendungen einen geheimen Schlüssel mit Azure AD aus. Folgende geheime Schlüssel sind möglich:
- Ein geheimer Clientschlüssel (Anwendungskennwort)
- Ein Zertifikat, mit dem eine signierte Assertion erstellt wird, die Standardansprüche enthält

Der geheime Schlüssel kann auch einfach eine signierte Assertion sein.

MSAL.NET verfügt über vier Methoden, um für die vertrauliche Client-App Anmeldeinformationen oder Assertionen bereitzustellen:
- `.WithClientSecret()`
- `.WithCertificate()`
- `.WithClientAssertion()`
- `.WithClientClaims()`

### <a name="signed-assertions"></a>Signierte Assertionen

Eine signierte Client Assertion hat die Form eines signierten, Base64-codierten JWT mit der Nutzlast, die die erforderlichen Authentifizierungsansprüche enthält, die von Azure AD vorgeschrieben sind. Gehen Sie zur Verwendung wie folgt vor:

```CSharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Von Azure AD werden folgende Ansprüche erwartet:

Anspruchstyp | Wert | BESCHREIBUNG
---------- | ---------- | ----------
aud | https://login.microsoftonline.com/{tenantId}/v2.0 | Der Anspruch „aud“ (Zielgruppe) identifiziert die Empfänger, für die das JWT vorgesehen ist (hier Azure AD). [RFC 7519, Abschnitt 4.1.3]
exp | Thu Jun 27 2019 15:04:17 GMT+0200 (Romance Daylight Time) | Der Anspruch „exp“ (Ablaufzeit) gibt die Ablaufzeit an, ab oder nach der das JWT NICHT für die Bearbeitung akzeptiert werden darf. [RFC 7519 Abschnitt 4.1.4]
iss | {ClientID} | Der Anspruch „iss“ (Aussteller) identifiziert den Prinzipal, der das JWT ausgestellt hat. Die Verarbeitung dieses Anspruchs ist anwendungsspezifisch. Der Wert von „iss“ ist eine Zeichenfolge mit Beachtung der Groß-/Kleinschreibung, die einen StringOrURI-Wert enthält. [RFC 7519, Abschnitt 4.1.1]
jti | (eine GUID) | Der Anspruch "jti" (JWT-ID) stellt einen eindeutigen Bezeichner für das JWT bereit. Es MUSS ein Bezeichnerwert zugewiesen werden, bei dem die Wahrscheinlichkeit vernachlässig ist, dass derselbe Wert versehentlich einem anderen Datenobjekt zugewiesen wird. Wenn die Anwendung mehrere Aussteller verwendet, MÜSSEN auch Konflikte zwischen von mehreren Ausstellern erstellten Werten verhindert werden. Der Anspruch „jti“ kann verwendet werden, um die erneute Wiedergabe des JWT zu verhindern. Der Wert von „JTI“ ist eine Zeichenfolge mit Beachtung der Groß-/Kleinschreibung. [RFC 7519, Abschnitt 4.1.7]
nbf | Thu Jun 27 2019 14:54:17 GMT+0200 (Romance Daylight Time) | Der Anspruch „nbf“ (nicht vor) gibt die Zeit an, vor der das JWT NICHT für die Bearbeitung akzeptiert werden darf. [RFC 7519, Abschnitt 4.1.5]
sub | {ClientID} | Der Anspruch „sub“ (Antragsteller) identifiziert den Antragsteller des JWT. Die Ansprüche in einem JWT sind normalerweise Angaben zum Antragsteller. Der Antragstellerwert MUSS entweder im Kontext des Ausstellers lokal eindeutig sein oder global eindeutig sein. [RFC 7519, Abschnitt 4.1.2]

Im folgenden Beispiel wird das Erstellen dieser Ansprüche veranschaulicht:

```CSharp
private static IDictionary<string, string> GetClaims()
{
      //aud = https://login.microsoftonline.com/ + Tenant ID + /v2.0
      string aud = "https://login.microsoftonline.com/72f988bf-86f1-41af-hd4m-2d7cd011db47/v2.0";

      string ConfidentialClientID = "61dab2ba-145d-4b1b-8569-bf4b9aed5dhb" //client id
      const uint JwtToAadLifetimeInSeconds = 60 * 10; // Ten minutes
      DateTime validFrom = DateTime.UtcNow;
      var nbf = ConvertToTimeT(validFrom);
      var exp = ConvertToTimeT(validFrom + TimeSpan.FromSeconds(JwtToAadLifetimeInSeconds));

      return new Dictionary<string, string>()
           {
                { "aud", aud },
                { "exp", exp.ToString() },
                { "iss", ConfidentialClientID },
                { "jti", Guid.NewGuid().ToString() },
                { "nbf", nbf.ToString() },
                { "sub", ConfidentialClientID }
            };
}
```

So erstellen Sie eine signierte Client-Assertion:

```CSharp
string Encode(byte[] arg)
{
    char Base64PadCharacter = '=';
    char Base64Character62 = '+';
    char Base64Character63 = '/';
    char Base64UrlCharacter62 = '-';
    char Base64UrlCharacter63 = '_';

    string s = Convert.ToBase64String(arg);
    s = s.Split(Base64PadCharacter)[0]; // RemoveAccount any trailing padding
    s = s.Replace(Base64Character62, Base64UrlCharacter62); // 62nd char of encoding
    s = s.Replace(Base64Character63, Base64UrlCharacter63); // 63rd char of encoding

    return s;
}

string GetAssertion()
{
    //Signing with SHA-256
    string rsaSha256Signature = "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256";
    X509Certificate2 certificate = ReadCertificate(config.CertificateName);

    //Create RSACryptoServiceProvider
    var x509Key = new X509AsymmetricSecurityKey(certificate);
    var privateKeyXmlParams = certificate.PrivateKey.ToXmlString(true);
    var rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(privateKeyXmlParams);

    //alg represents the desired signing algorithm, which is SHA-256 in this case
    //kid represents the certificate thumbprint
    var header = new Dictionary<string, string>()
         {
              { "alg", "RS256"},
              { "kid", Encode(Certificate.GetCertHash()) }
         };

    //Please see the previous code snippet on how to craft claims for the GetClaims() method
    string token = Encode(Encoding.UTF8.GetBytes(JObject.FromObject(header).ToString())) + "." + Encode(Encoding.UTF8.GetBytes(JObject.FromObject(GetClaims())));

    string signature = Encode(rsa.SignData(Encoding.UTF8.GetBytes(token), new SHA256Cng()));
    string SignedAssertion = string.Concat(token, ".", signature);
    return SignedAssertion;
}
```

### <a name="withclientclaims"></a>WithClientClaims

`WithClientClaims(X509Certificate2 certificate, IDictionary<string, string> claimsToSign, bool mergeWithDefaultClaims = true)` erzeugt standardmäßig eine signierte Assertion, die die von Azure AD erwarteten Ansprüche sowie zusätzliche Clientansprüche enthält, die Sie senden möchten. Im folgenden Codeausschnitt wird die Vorgehensweise gezeigt.

```CSharp
string ipAddress = "192.168.1.2";
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, 
                                                                      new Dictionary<string, string> { { "client_ip", ipAddress } })
                                          .Build();

```

Wenn einer der Ansprüche im Wörterbuch, das Sie übergeben, mit einem der obligatorischen Ansprüche identisch ist, wird der Wert des zusätzlichen Anspruchs berücksichtigt. Er überschreibt die von MSAL.NET berechneten Ansprüche.

Wenn Sie eigene Ansprüche, einschließlich der obligatorischen Ansprüche, die von Azure AD erwartet werden, angeben möchten, übergeben Sie als `mergeWithDefaultClaims`-Parameter `false`.
