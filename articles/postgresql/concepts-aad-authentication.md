---
title: 'Azure Active Directory-Authentifizierung mit Azure Database for PostgreSQL: Einzelserver'
description: 'Erfahren Sie mehr über die Konzepte von Azure Active Directory für die Authentifizierung mit Azure Database for PostgreSQL: Einzelserver'
author: lfittl
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 47637396581beeafb0748066cd6a66f011e8eaa1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512099"
---
# <a name="use-azure-active-directory-for-authenticating-with-postgresql"></a>Verwenden von Azure Active Directory für die Authentifizierung mit PostgreSQL

Microsoft Azure AD-Authentifizierung (Azure Active Directory) ist ein Mechanismus zum Herstellen einer Verbindung mit Azure Database for PostgreSQL unter Verwendung der in Azure AD definierten Identitäten.
Mit Azure AD-Authentifizierung können Sie Datenbankbenutzeridentitäten und andere Microsoft-Dienste an einem zentralen Ort verwalten, wodurch die Berechtigungsverwaltung vereinfacht wird.

> [!IMPORTANT]
> Azure AD-Authentifizierung für Azure Database for PostgreSQL befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Beispiele für die Vorteile der Verwendung von Azure AD:

- Einheitliche Authentifizierung von Benutzern über Azure-Dienste hinweg
- Verwaltung von Kennwortrichtlinien und Kennwortrotation an einem zentralen Ort
- Mehrere Formen der Authentifizierung, die von Azure Active Directory unterstützt werden, wodurch das Speichern von Kennwörtern entfällt
- Kunden können Datenbankberechtigungen mithilfe von externen Gruppen (Azure AD) verwalten.
- Azure AD-Authentifizierung verwendet PostgreSQL-Datenbankrollen zum Authentifizieren von Identitäten auf Datenbankebene.
- Unterstützung von tokenbasierter Authentifizierung für Anwendungen, die eine Verbindung mit Azure Database for PostgreSQL herstellen

Um Azure Active Directory-Authentifizierung zu konfigurieren und zu verwenden, verwenden Sie den folgenden Vorgang:

1. Erstellen Sie nach Bedarf Benutzeridentitäten, und füllen Sie Azure Active Directory mit diesen auf.
2. Ordnen Sie optional das Active Directory zu, das derzeit mit Ihrem Azure-Abonnement verknüpft ist, oder ändern Sie es.
3. Erstellen Sie einen Azure AD-Administrator für den Azure Database for PostgreSQL-Server.
4. Erstellen Sie Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind.
5. Stellen Sie eine Verbindung mit der Datenbank her, indem Sie ein Token für eine Azure AD-Identität abrufen und sich anmelden.

> [!NOTE]
> Informationen zum Erstellen und Auffüllen von Azure AD und zum anschließenden Konfigurieren von Azure AD mit Azure Database for PostgreSQL finden Sie unter [Konfigurieren von und Anmelden mit Azure AD für Azure Database for PostgreSQL](howto-configure-sign-in-aad-authentication.md).

## <a name="architecture"></a>Architecture

Die folgende Abbildung bietet eine Übersicht über die Funktionsweise der Authentifizierung bei der Verwendung von Azure AD-Authentifizierung mit Azure Database for PostgreSQL. Die Pfeile zeigen die Kommunikationswege.

![Authentifizierungsablauf][1]

## <a name="administrator-structure"></a>Administratorstruktur

Bei Verwendung von Azure AD-Authentifizierung sind zwei Administratorkonten für den PostgreSQL-Server vorhanden: der ursprüngliche PostgreSQL-Administrator und der Azure AD-Administrator. Nur der auf einem Azure AD-Konto basierende Administrator kann den ersten eigenständigen Azure AD-Datenbankbenutzer in einer Benutzerdatenbank erstellen. Das Konto für die Azure AD-Administratoranmeldung kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn es sich bei dem Administrator um ein Gruppenkonto handelt, kann es von einem beliebigen Gruppenmitglied verwendet werden, sodass mehrere Azure AD-Administratoren den PostgreSQL-Server verwalten können. Die Verwendung eines Gruppenkontos für den Administrator ermöglicht es Ihnen, Gruppenmitglieder in Azure AD zentral hinzuzufügen und zu entfernen, ohne die Benutzer oder Berechtigungen im PostgreSQL-Server zu ändern. Es kann jeweils nur ein Azure AD-Administrator (ein Benutzer oder eine Gruppe) konfiguriert werden.

![Administratorstruktur][2]

## <a name="permissions"></a>Berechtigungen

Zum Erstellen neuer Benutzer, die sich mit Azure AD authentifizieren können, müssen Sie über die Rolle `azure_ad_admin` in der Datenbank verfügen. Diese Rolle wird zugewiesen, indem das Azure AD-Administratorkonto für einen bestimmten Azure Database for PostgreSQL-Server konfiguriert wird.

Um einen neuen Azure AD-Datenbankbenutzer zu erstellen, müssen Sie als Azure AD-Administrator eine Verbindung herstellen. Dies wird unter [Konfigurieren und Anmelden mit Azure AD für Azure Database for PostgreSQL](howto-configure-sign-in-aad-authentication.md) gezeigt.

Eine Azure AD-Authentifizierung ist nur möglich, wenn der Azure AD-Administrator für Azure Database for PostgreSQL erstellt wurde. Falls der Azure Active Directory-Administrator vom Server entfernt wurde, können vorhandene Azure Active Directory-Benutzer, die zuvor erstellt wurden, nicht mehr mithilfe ihrer Azure Active Directory-Anmeldeinformationen auf die Datenbank zugreifen.

## <a name="connecting-using-azure-ad-identities"></a>Herstellen einer Verbindung mit Azure AD-Identitäten

Die Azure Active Directory-Authentifizierung unterstützt die folgenden Methoden der Verbindungsherstellung mit einer Datenbank unter Verwendung von Azure AD-Identitäten:

- Azure Active Directory Password
- Azure Active Directory Integrated
- Azure Active Directory: universell mit MFA
- Verwenden von Active Directory Anwendungszertifikaten oder Clientgeheimnissen

Nachdem Sie sich über Active Directory authentifiziert haben, rufen Sie ein Token ab. Dieses Token ist Ihr Kennwort für die Anmeldung.

> [!NOTE]
> Weitere Informationen zum Herstellen einer Verbindung mit einem Active Directory-Token finden Sie unter [Konfigurieren von und Anmelden mit Azure AD für Azure Database for PostgreSQL](howto-configure-sign-in-aad-authentication.md).

## <a name="additional-considerations"></a>Zusätzliche Überlegungen

- Um die Verwaltungsmöglichkeiten zu verbessern, wird empfohlen, eine dedizierte Azure AD-Gruppe als Administrator bereitzustellen.
- Es kann immer nur ein einzelner Azure AD-Administrator (Benutzer oder Gruppe) für einen Azure Database for PostgreSQL-Server konfiguriert werden.
- Nur ein Azure AD-Administrator für PostgreSQL kann zunächst über ein Azure Active Directory-Konto eine Verbindung mit Azure Database for PostgreSQL herstellen. Der Active Directory-Administrator kann weitere Azure AD-Datenbankbenutzer konfigurieren.
- Wenn ein Benutzer aus Azure AD gelöscht wird, kann dieser Benutzer sich nicht mehr bei Azure AD authentifizieren, und daher ist es nicht mehr möglich, ein Zugriffstoken für diesen Benutzer abzurufen. In diesem Fall ist es nicht möglich, mit dieser Rolle eine Verbindung mit dem Server herzustellen, obwohl die entsprechende Rolle weiterhin in der Datenbank vorhanden ist.
> [!NOTE]
> Die Anmeldung mit dem gelöschten Azure AD-Benutzer kann bis zum Ablauf des Token (bis zu 60 Minuten nach Ausstellung des Token) dennoch erfolgen.  Wenn Sie den Benutzer auch aus Azure Database for PostgreSQL entfernen, wird dieser Zugriff sofort widerrufen.
- Wenn der Azure AD-Administrator vom Server entfernt wird, ist der Server keinem Azure AD-Mandanten mehr zugeordnet, und daher werden alle Azure AD-Anmeldungen für den Server deaktiviert. Wenn Sie einen neuen Azure AD-Administrator aus demselben Mandanten hinzufügen, werden Azure AD Anmeldungen erneut aktiviert.
- Azure Database for PostgreSQL gleicht Zugriffstoken für die Azure Database for PostgreSQL-Rolle mit der eindeutigen Azure AD-Benutzer-ID des Benutzers ab, anstatt den Benutzernamen zu verwenden. Dies bedeutet Folgendes: Wenn ein Azure AD-Benutzer in Azure AD gelöscht und ein neuer Benutzer mit dem gleichen Namen erstellt wird, geht Azure Database for PostgreSQL davon aus, dass dies ein anderer Benutzer ist. Wenn ein Benutzer aus Azure AD gelöscht und dann ein neuer Benutzer mit dem gleichen Namen hinzugefügt wird, kann der neue Benutzer daher keine Verbindung mit der vorhandenen Rolle herstellen. Um diesen Vorgang zuzulassen, muss der Azure Database for PostgreSQL Azure AD-Administrator die Rolle „azure_ad_user“ widerrufen und dann dem Benutzer diese Rolle zuweisen, um die Azure AD-Benutzer-ID zu aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen und Auffüllen von Azure AD und zum anschließenden Konfigurieren von Azure AD mit Azure Database for PostgreSQL finden Sie unter [Konfigurieren von und Anmelden mit Azure AD für Azure Database for PostgreSQL](howto-configure-sign-in-aad-authentication.md).
- Eine Übersicht über Anmeldungen, Benutzer und Datenbankrollen in Azure Database for PostgreSQL finden Sie unter [Erstellen von Benutzern in Azure Database for PostgreSQL: Einzelserver](howto-create-users.md).

<!--Image references-->

[1]: ./media/concepts-aad-authentication/authentication-flow.png
[2]: ./media/concepts-aad-authentication/admin-structure.png
