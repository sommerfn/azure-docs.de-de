---
title: Multi-Factor Authentication in Azure Active Directory B2C | Microsoft-Dokumentation
description: Aktivieren der Multi-Factor Authentication in kundenorientierten Anwendungen, die mit Azure Active Directory B2C geschützt werden
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0b872fcfbc2ead2cebdd32ff718b582c13af314e
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065582"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Aktivieren der Multi-Factor Authentication in Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) bietet eine direkte Integration von [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md), damit Sie eine zweite Sicherheitsebene zu Registrierungs- und Anmeldeoberflächen in Anwendungen hinzufügen können. Sie können die Multi-Factor Authentication aktivieren, ohne eine einzige Codezeile schreiben zu müssen. Wenn Sie bereits Benutzerflows für Registrierung und Anmeldung erstellt haben, können Sie dennoch die mehrstufige Authentifizierung aktivieren.

Mithilfe dieses Features können Anwendungen z. B. folgende Szenarios bewältigen:

- Für den Zugriff auf eine Anwendung ist keine Multi-Factor Authentication erforderlich, jedoch für den Zugriff auf eine andere Anwendung. Beispielsweise kann sich der Kunde bei der Anwendung einer Kfz-Versicherung mit einem lokalen Konto oder dem Konto eines sozialen Netzwerks anmelden. Er muss jedoch seine Telefonnummer bestätigen, bevor er auf die Anwendung für die Hausversicherung zugreifen kann, die im selben Verzeichnis registriert ist.
- Die Multi-Factor Authentication ist allgemein für den Zugriff auf eine Anwendung nicht erforderlich, jedoch für vertrauliche Unterbereiche dieser Anwendung. Beispielsweise kann sich der Kunde bei einer Onlinebanking-Anwendung mit einem lokalen Konto oder dem Konto eines sozialen Netzwerks anmelden, um den Kontostand abzufragen. Um eine Überweisung zu tätigen, ist jedoch die Bestätigung der Telefonnummer erforderlich.

## <a name="set-multi-factor-authentication"></a>Festlegen der Multi-Factor Authentication

Wenn Sie einen Benutzerflow erstellen, können Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) aktivieren.

![Festlegen der Multi-Factor Authentication](./media/active-directory-b2c-reference-mfa/add-policy.png)

Legen Sie **Mehrstufige Authentifizierung** auf **Aktiviert** fest.

Sie können die Benutzeroberflächenfunktion mit **Benutzerflow ausführen** überprüfen. Bestätigen Sie das folgende Szenario:

In Ihrem Mandanten wird ein Kundenkonto erstellt, bevor der Schritt für die Multi-Factor Authentication ausgeführt wird. Während dieses Schritts wird der Kunde aufgefordert, seine Telefonnummer anzugeben und zu bestätigen. Wenn die Überprüfung erfolgreich ist, wird die Telefonnummer zur späteren Verwendung an das Kundenkonto angefügt. Auch wenn der Kunde den Vorgang abbricht, kann er bei der nächsten Anmeldung aufgefordert werden, eine Telefonnummer erneut zu bestätigen (bei aktivierter Multi-Factor Authentication).

## <a name="add-multi-factor-authentication"></a>Hinzufügen der Multi-Factor Authentication

Es ist möglich, die mehrstufige Authentifizierung für einen Benutzerflow zu aktivieren, den Sie zuvor erstellt haben.

So aktivieren Sie die Multi-Factor Authentication:

1. Öffnen Sie den Benutzerflow, und wählen Sie **Eigenschaften** aus.
2. Klicken Sie neben **Mehrstufige Authentifizierung** auf **Aktiviert**.
3. Klicken Sie oben auf der Seite auf **Speichern**.


