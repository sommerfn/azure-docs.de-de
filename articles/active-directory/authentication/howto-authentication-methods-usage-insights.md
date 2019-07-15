---
title: Authentifizierungsmethoden Nutzung & Einblicke Berichterstattung (Vorschau) – Azure Active Directory
description: Bericht über die Verwendung der Azure AD Self-Service-Passwort-Rücksetzung und der Multi-Faktor-Authentifizierungsverfahren
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0f6a74308f1bc4a7b77576fb9f39f965de0a4f8
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561021"
---
# <a name="authentication-methods-usage--insights-preview"></a>Verwendung und Erkenntnisse von Authentifizierungsmethoden (Vorschau)

Nutzung & Einsichten ermöglichen es Ihnen zu verstehen, wie Authentifizierungsmethoden für Funktionen wie Azure Multi-Factor Authentication und Self-Service-Passwort-Reset in Ihrem Unternehmen funktionieren. Diese Berichtsfunktion gibt Ihrem Unternehmen die Möglichkeit zu verstehen, welche Methoden registriert werden und wie sie verwendet werden.

## <a name="permissions-and-licenses"></a>Berechtigungen und Lizenzen

Die folgenden Rollen können auf Nutzung und Einsichten zugreifen:

- Globaler Administrator
- Sicherheitsleseberechtigter
- Sicherheitsadministrator
- Meldet Reader

Für den Zugriff auf Nutzung und Erkenntnisse ist keine zusätzliche Lizenz erforderlich. Informationen zur Lizenzierung von Azure Multi-Factor Authentication und Self-Service Password Reset (SSPR) finden Sie auf der [Azure Active Directory Preisliste](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="how-it-works"></a>So funktioniert's

Um auf die Verwendung von Authentifizierungsmethoden und Erkenntnisse zuzugreifen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu **Azure Active Directory** >  **Kennwort zurücksetzen** >  **Verwendung & Einsichten**.
1. In den Übersichten **Registrierung** oder **Nutzung** können Sie die vorgefilterten Berichte öffnen, um nach Ihren Bedürfnissen zu filtern.

![Nutzung & Einsichten Übersicht](./media/howto-authentication-methods-usage-insights/usage-insights-overview.png)

Um direkt auf Nutzung und Erkenntnisse zuzugreifen, gehen Sie zu [https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade). Über diesen Link gelangen Sie zur Registrierungsübersicht.

Die registrierten Benutzer, die aktivierten Benutzer und die benutzerfähigen Kacheln zeigen die folgenden Registrierungsdaten für Ihre Benutzer:

- Registriert: Ein Benutzer gilt als registriert, wenn er (oder ein Admin) genügend Authentifizierungsmethoden registriert hat, um die SSPR- oder Multi-Faktor-Authentifizierungsrichtlinie Ihres Unternehmens zu erfüllen.
- Aktiviert: Ein Benutzer gilt als aktiviert, wenn er in den Geltungsbereich der SSPR-Richtlinie fällt. Wenn SSPR für eine Gruppe aktiviert ist, gilt der Benutzer als aktiviert, wenn er sich in dieser Gruppe befindet. Wenn SSPR für alle Benutzer aktiviert ist, gelten alle Benutzer im Mieter (außer Gäste) als aktiviert.
- Fähig: Ein Benutzer gilt als fähig, wenn er sowohl registriert als auch aktiviert ist. Dieser Status bedeutet, dass sie bei Bedarf jederzeit SSPR durchführen können.

Wenn Sie auf eine dieser Kacheln oder die darin angezeigten Einblicke klicken, gelangen Sie zu einer vorgefilterten Liste von Registrierungsdetails.

Das Diagramm **Registrierungen** auf der Registerkarte **Registrierung** zeigt die Anzahl der erfolgreichen und fehlgeschlagenen Registrierungen von Authentifizierungsmethoden nach Authentifizierungsmethoden. Das **Rücksetz**diagramm auf der Registerkarte **Verwendung** zeigt die Anzahl der erfolgreichen und fehlgeschlagenen Authentifizierungen während des Passwort-Reset-Ablaufs nach Authentifizierungsmethode.

Wenn Sie auf eines der Diagramme klicken, gelangen Sie zu einer vorgefilterten Liste von Registrierungs- oder Reset-Ereignissen.

Mit der Steuerung in der oberen, rechten Ecke können Sie den Datumsbereich für die in den Diagrammen Registrierungen und Zurücksetzen angezeigten Auditdaten auf 24 Stunden, 7 Tage oder 30 Tage ändern.

Registrierungsdaten aus dem 

### <a name="registration-details"></a>Registrierungsdetails

Wenn Sie auf die Option **Registrierte Benutzer**, **Aktivierte Benutzer** oder **Benutzer-fähige** Kacheln oder Einsichten klicken, gelangen Sie zu den Registrierungsdetails.

Der Bericht mit den Registrierungsdetails zeigt für jeden Benutzer die folgenden Informationen an:

- NAME
- Benutzername
- Registrierungsstatus (alle, registriert, nicht registriert)
- Status aktiviert (alle, aktiviert, nicht aktiviert)
- Fähigkeitsstatus (alle, fähig, nicht fähig)
- Methoden (App-Benachrichtigung, App-Code, Telefonanruf, SMS, E-Mail, Sicherheitsfragen)

Mit den Steuerelementen oben in der Liste können Sie nach einem Benutzer suchen und die Liste der Benutzer anhand der angezeigten Spalten filtern.

### <a name="reset-details"></a>Zurücksetzen von Details

Wenn Sie auf die Diagramme Registrierungen oder Zurücksetzen klicken, gelangen Sie zu den Rücksetzdetails.

Der Rücksetzdetailbericht zeigt Registrierungs- und Rücksetzereignisse der letzten 30 Tage einschließlich:

- NAME
- Benutzername
- Funktion (Alle, Registrierung, Zurücksetzen)
- Authentifizierungsmethode (App-Benachrichtigung, App-Code, Telefonat, Büroanruf, SMS, E-Mail, Sicherheitsfragen)
- Status (alle, Erfolg, Fehler)

Mit den Steuerelementen oben in der Liste können Sie nach einem Benutzer suchen und die Liste der Benutzer anhand der angezeigten Spalten filtern.

## <a name="limitations"></a>Einschränkungen

Die in diesen Berichten angezeigten Daten werden sich um bis zu 60 Minuten verzögern. Im Azure-Portal existiert ein Feld "Zuletzt aktualisiert", um festzustellen, wie aktuell Ihre Daten sind.

Nutzungs- und Einsichtsdaten sind kein Ersatz für die Aktivitätsberichte der Azure Multi-Factor Authentication oder Informationen, die im Azure AD Anmeldebericht enthalten sind.

## <a name="next-steps"></a>Nächste Schritte

- [Arbeiten mit den Authentifizierungsmethoden Verwendung Bericht API ](https://docs.microsoft.com/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Auswahl von Authentifizierungsmethoden für Ihr Unternehmen](concept-authentication-methods.md)
- [Kombinierte Registrierungserfahrung](concept-registration-mfa-sspr-combined.md)
