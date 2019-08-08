---
title: Problembehandlung von Azure Data Catalog
description: Dieser Artikel beschreibt allgemeine Problembehandlungsüberlegungen für Azure Data Catalog-Ressourcen.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 08/01/2019
ms.openlocfilehash: f37110138ae62272e56b155b7e8a9aac2f80cdf8
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68734584"
---
# <a name="troubleshooting-azure-data-catalog"></a>Problembehandlung von Azure Data Catalog

Dieser Artikel beschreibt allgemeine Problembehandlungsüberlegungen für Azure Data Catalog-Ressourcen. 

## <a name="functionality-limitations"></a>Funktionseinschränkungen

Wenn Sie Azure Data Catalog verwenden, sind die folgenden Funktionen eingeschränkt:

- Konten mit dem Typ **Gastrolle** werden nicht unterstützt. Sie können keine Gastkonten als Benutzer von Azure Data Catalog hinzufügen, und Gastbenutzer können nicht das Portal unter www.azuredatacatalog.com verwenden.

- Das Erstellen von Azure Data Catalog-Ressourcen mithilfe von Azure Resource Manager-Vorlagen oder über Azure PowerShell-Befehle wird nicht unterstützt.

- Die Azure Data Catalog-Ressource kann nicht zwischen Azure-Mandanten verschoben werden.

## <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-Richtlinienkonfiguration

Es kann vorkommen, dass Sie sich beim Azure Data Catalog-Portal anmelden können, jedoch eine Fehlermeldung erhalten, wenn Sie versuchen, sich beim Tool zum Registrieren von Datenquellen anzumelden. Dieser Fehler kann auftreten, wenn Sie sich im Unternehmensnetzwerk befinden oder von außerhalb des Unternehmensnetzwerks eine Verbindung herstellen.

Das Registrierungstool verwendet die *Formularauthentifizierung* , um Benutzeranmeldungen mit Azure Active Directory zu überprüfen. Um die Anmeldung zu ermöglichen, muss der Azure Active Directory-Administrator die Formularauthentifizierung in der *globalen Authentifizierungsrichtlinie*aktivieren.

Wie im folgenden Screenshot gezeigt, kann die Authentifizierung in der globalen Authentifizierungsrichtlinie separat für Intranet- und Extranetverbindungen aktiviert werden. Anmeldefehler können auftreten, wenn die Formularauthentifizierung für das Netzwerk, über das Sie eine Verbindung herstellen, nicht aktiviert ist.

 ![Globale Authentifizierungsrichtlinie für Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Azure Data Catalog-Instanz](data-catalog-get-started.md)
