---
title: 'Einrichten der Registrierung und Anmeldung mit OpenID Connect: Azure Active Directory B2C | Microsoft-Dokumentation'
description: Einrichten der Registrierung und Anmeldung mit OpenID Connect mithilfe von Azure Active Directory B2C
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0303f8c7e18a5c229bc5a8c5e9b90d95cdaccbe7
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672919"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit OpenID Connect mithilfe von Azure Active Directory B2C

[OpenID Connect](active-directory-b2c-reference-oidc.md) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, das für die sichere Anmeldung von Benutzern verwendet werden kann. Die meisten Identitätsanbieter, die dieses Protokoll verwenden, werden in Azure AD B2C unterstützt. In diesem Artikel wird erläutert, wie Sie benutzerdefinierte OpenID Connect-Identitätsanbieter zu Ihren Benutzerflows hinzufügen können.

## <a name="add-the-identity-provider"></a>Hinzufügen des Identitätsanbieters

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Neuer OpenID Connect-Anbieter** aus.

## <a name="configure-the-identity-provider"></a>Konfigurieren des Identitätsanbieters

Jeder OpenID Connect-Identitätsanbieter beschreibt ein Metadatendokument, das den Großteil der erforderlichen Informationen für die Anmeldung enthält. Hierzu gehören Informationen wie z.B. die zu verwendenden URLs und der Speicherort der öffentlichen Signaturschlüssel des Dienstes. Das OpenID Connect-Metadatendokument befindet sich immer an einem Endpunkt, der in `.well-known\openid-configuration` endet. Geben Sie für den OpenID Connect-Identitätsanbieter, den Sie hinzufügen möchten, die Metadaten-URL ein.

## <a name="client-id-and-secret"></a>Client-ID und Geheimnis

Damit Benutzer sich anmelden können, setzt der Identitätsanbieter voraus, dass Entwickler eine Anwendung in ihrem Dienst registrieren. Diese Anwendung verfügt über eine ID, die als die **Client-ID** und ein **Clientgeheimnis** bezeichnet wird. Kopieren Sie diese Werte vom Identitätsanbieter aus, und geben Sie sie in die entsprechenden Felder ein.

> [!NOTE]
> Das Clientgeheimnis ist optional. Allerdings müssen Sie ein Clientgeheimnis eingeben, wenn Sie den [Autorisierungscodefluss](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth) verwenden möchten, der das Geheimnis für den Austausch des Codes für das Token verwendet.

## <a name="scope"></a>`Scope`

Der Bereich definiert die Informationen und Berechtigungen, die Sie bei Ihrem benutzerdefinierten Identitätsanbieter sammeln möchten. OpenID Connect-Anforderungen müssen den `openid`-Bereichswert enthalten, um das ID-Token vom Identitätsanbieter zu empfangen. Ohne das ID-Token können Benutzer sich nicht mit dem benutzerdefinierten Identitätsanbieter bei Azure AD B2C anmelden. Andere Bereiche können (durch Leerzeichen getrennt) angefügt werden. Entnehmen Sie der Dokumentation des benutzerdefinierten Identitätsanbieters, welche anderen Bereiche möglicherweise zur Verfügung stehen.

## <a name="response-type"></a>Antworttyp

Der Antworttyp beschreibt, welche Art von Informationen beim ersten Aufruf von `authorization_endpoint` des benutzerdefinierten Identitätsanbieters zurückgesendet werden. Die folgenden Antworttypen können verwendet werden:

* `code`: Gemäß [Autorisierungscodeflow](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth) wird ein Code an Azure AD B2C zurückgegeben. Azure AD B2C fährt damit fort, den `token_endpoint` für den Austausch des Codes für das Token aufzurufen.
* `id_token`: Ein ID-Token wird vom benutzerdefinierten Identitätsanbieter an Azure AD B2C zurückgegeben.

## <a name="response-mode"></a>Antwortmodus

Der Antwortmodus definiert die Methode, die zum Senden der Daten vom benutzerdefinierten Identitätsanbieter zurück zu Azure AD B2C verwendet werden sollte. Die folgenden Modi können für die Antwort verwendet werden:

* `form_post`: Dieser Antwortmodus wird für optimale Sicherheit empfohlen. Die Antwort wird mit der HTTP-`POST`-Methode übertragen, wobei Code oder Token im `application/x-www-form-urlencoded`-Format codiert wird.
* `query`: Der Code oder das Token wird als Abfrageparameter zurückgegeben.

## <a name="domain-hint"></a>Domänenhinweis

Der Domänenhinweis kann zum direkten Wechsel zur Anmeldeseite des angegebenen Identitätsanbieters verwendet werden, anstatt den Benutzer eine Auswahl in der Liste der verfügbaren Identitätsanbieter treffen zu lassen. Um diese Art von Verhalten zu ermöglichen, geben Sie einen Wert für den Domänenhinweis ein. Um direkt zum benutzerdefinierten Identitätsanbieter zu wechseln, fügen Sie den Parameter `domain_hint=<domain hint value>` beim Aufrufen von Azure AD B2C für die Anmeldung am Ende Ihrer Anforderung an.

## <a name="claims-mapping"></a>Anspruchszuordnung

Nachdem der benutzerdefinierte Identitätsanbieter ein ID-Token an Azure AD B2C zurückgesendet hat, muss Azure AD B2C in der Lage sein, die Ansprüche aus dem empfangenen Token den Ansprüchen zuzuordnen, die Azure AD B2C erkennt und verwendet. Ziehen Sie für jede der folgenden Zuordnungen die Dokumentation des benutzerdefinierten Identitätsanbieters zu Rate, um die Ansprüche zu verstehen, die in den Token des Identitätsanbieters zurückgegeben werden:

* **Benutzer-ID**: Geben Sie den Anspruch ein, der den *eindeutigen Bezeichner* für den angemeldeten Benutzer bereitstellt.
* **Anzeigename**: Geben Sie den Anspruch ein, der den *Anzeigenamen* oder den *vollständigen Namen* des Benutzers bereitstellt.
* **Vorname**: Geben Sie den Anspruch ein, der den *Vornamen* des Benutzers bereitstellt.
* **Nachname**: Geben Sie den Anspruch ein, der den *Nachnamen* des Benutzers bereitstellt.
* **E-Mail**: Geben Sie den Anspruch ein, der die *E-Mail-Adresse* des Benutzers bereitstellt.
