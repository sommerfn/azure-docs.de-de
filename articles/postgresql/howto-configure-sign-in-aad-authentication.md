---
title: 'Konfigurieren und Anmelden mit Azure AD für Azure Database for PostgreSQL: Einzelserver'
description: 'Erfahren Sie, wie Sie Azure Active Directory für die Authentifizierung mit Azure Database for PostgreSQL einrichten: Einzelserver'
author: lfittl
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d5abfe4cc6aa0679d8009343fa24c1059700bb79
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511267"
---
# <a name="use-azure-active-directory-for-authenticating-with-postgresql"></a>Verwenden von Azure Active Directory für die Authentifizierung mit PostgreSQL

Dieser Artikel führt Sie durch die Schritte zum Konfigurieren des Zugriffs auf Azure Active Directory mit Azure Database for PostgreSQL sowie zum Herstellen einer Verbindung mit einem Azure AD-Token.

> [!IMPORTANT]
> Azure AD-Authentifizierung für Azure Database for PostgreSQL befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="setting-the-azure-ad-admin-user"></a>Festlegen des Azure AD-Administratorbenutzers

Nur ein Azure AD-Administratorbenutzer kann Benutzer für Azure AD-basierte Authentifizierung erstellen/aktivieren. Führen Sie die folgenden Schritte aus, um den Azure AD-Administratorbenutzer zu erstellen:

1. Wählen Sie im Azure-Portal die Instanz von Azure Database for PostgreSQL aus, die Sie für Azure AD aktivieren möchten.
2. Wählen Sie unter „Einstellungen“ die Option „Active Directory-Administrator“ aus:

![Festlegen des Azure AD-Administrators][2]

3. Wählen Sie im Kundenmandanten einen gültigen Azure AD-Benutzer aus, der als Azure AD-Administrator fungieren soll.

> [!IMPORTANT]
> Beim Festlegen des Administrators wird dem Azure Database for PostgreSQL-Server ein neuer Benutzer mit vollständigen Administratorberechtigungen hinzugefügt. Der Azure AD Administratorbenutzer in Azure Database for PostgreSQL übt die Rolle `azure_ad_admin` aus.

Es kann nur ein Azure AD-Administrator pro PostgreSQL-Server erstellt werden, und die Auswahl eines anderen Administrators überschreibt den vorhandenen Azure AD-Administrator, der für den Server konfiguriert ist. Sie können eine Azure AD-Gruppe anstelle eines einzelnen Benutzers angeben, damit mehrere Administratoren vorhanden sind. Beachten Sie, dass Sie sich dann zu Verwaltungszwecken mit dem Gruppennamen anmelden.

## <a name="creating-azure-ad-users-in-azure-database-for-postgresql"></a>Erstellen von Azure AD-Benutzern in Azure Database for PostgreSQL

Um Ihrer Azure Database for PostgreSQL-Datenbank einen Azure AD-Benutzer hinzuzufügen, führen Sie nach dem Herstellen einer Verbindung die folgenden Schritte aus (weitere Informationen finden Sie in einem späteren Abschnitt zum Herstellen einer Verbindung):

1. Stellen Sie zunächst sicher, dass der Azure AD-Benutzer `<user>@yourtenant.onmicrosoft.com` ein gültiger Benutzer im Azure AD-Mandanten ist.
2. Melden Sie sich bei Ihrer Azure Database for PostgreSQL-Instanz als Azure AD-Administratorbenutzer an.
3. Erstellen Sie die Rolle `<user>@yourtenant.onmicrosoft.com` in Azure Database for PostgreSQL.
4. Erklären Sie `<user>@yourtenant.onmicrosoft.com` zum Mitglied der Rolle azure_ad_user. Dies darf nur für Azure AD-Benutzer erfolgen.

**Beispiel:**

```sql
CREATE ROLE "user1@yourtenant.onmicrosoft.com" WITH LOGIN IN ROLE azure_ad_user;
```

> [!NOTE]
> Die Authentifizierung eines Benutzers über Azure AD verleiht dem Benutzer keine Berechtigung für den Zugriff auf Objekte innerhalb der Azure Database for PostgreSQL-Datenbank. Sie müssen dem Benutzer die erforderlichen Berechtigungen manuell erteilen.

## <a name="creating-azure-ad-groups-in-azure-database-for-postgresql"></a>Erstellen von Azure AD-Gruppen in Azure Database for PostgreSQL

Um eine Azure AD-Gruppe für den Zugriff auf Ihre Datenbank zu aktivieren, verwenden Sie den gleichen Mechanismus wie für Benutzer, geben aber stattdessen den Gruppennamen an:

**Beispiel:**

```sql
CREATE ROLE "Prod DB Readonly" WITH LOGIN IN ROLE azure_ad_user;
```

Wenn sie sich anmelden, verwenden die Mitglieder der Gruppe ihre persönlichen Zugriffstoken, melden sich aber mit dem Gruppennamen an, der als Benutzername angegeben wurde.

## <a name="connecting-to-azure-database-for-postgresql-using-azure-ad"></a>Herstellen einer Verbindung mit Azure Database for PostgreSQL mithilfe von Azure AD

Die folgende Abbildung bietet eine Übersicht über den Workflow bei der Verwendung von Azure AD-Authentifizierung mit Azure Database for PostgreSQL:

![Authentifizierungsablauf][1]

Wir haben die Azure AD-Integration so konzipiert, dass sie mit gängigen PostgreSQL-Tools wie psql funktioniert, die nicht Azure AD-fähig sind und nur die Angabe von Benutzername und Kennwort beim Herstellen einer Verbindung mit PostgreSQL unterstützen. Wir übergeben das Azure AD-Token als Kennwort, wie in der Abbildung oben gezeigt.

Zurzeit haben wir die folgenden Clients getestet:

- psql-Befehlszeile (verwenden Sie die PGPASSWORD-Variable, um das Token zu übergeben, siehe unten)
- Azure Data Studio (mit der PostgreSQL-Erweiterung)
- Andere libpq-basierte Clients (z.B. gängige Anwendungsframeworks und ORMs)

> [!NOTE]
> Beachten Sie, dass die Verwendung des Azure AD-Tokens mit pgAdmin derzeit nicht unterstützt wird, da es eine hartcodierte Einschränkung von 256 Zeichen für Kennwörter aufweist (die das Token überschreitet).

Dies sind die Schritte, die ein Benutzer oder eine Anwendung für die Authentifizierung mit Azure AD ausführen muss, die im Folgenden beschrieben werden:

### <a name="step-1-authenticate-with-azure-ad"></a>Schritt 1: Authentifizierung über Azure AD

Stellen Sie sicher, dass die [Azure CLI](/cli/azure/install-azure-cli) installiert ist.

Rufen Sie das Azure CLI-Tool auf, um sich über Azure AD zu authentifizieren. Sie müssen Ihre Azure AD-Benutzer-ID und das Kennwort angeben.

```
az login
```

Mit diesem Befehl wird ein Browserfenster auf der Seite „Azure AD-Authentifizierung“ gestartet.

> [!NOTE]
> Sie können auch Azure Cloud Shell verwenden, um diese Schritte auszuführen.
> Beachten Sie, dass Sie beim Abrufen von Azure AD-Zugriffstoken in Azure Cloud Shell `az login` explizit aufrufen und sich erneut anmelden müssen (im separaten Fenster mit einem Code). Nach diesem Anmeldevorgang funktioniert der Befehl `get-access-token` wie erwartet.

### <a name="step-2-retrieve-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Rufen Sie das Azure CLI-Tool auf, um ein Zugriffstoken für den über Azure AD authentifizierten Benutzer aus Schritt 1 für den Zugriff auf Azure Database for PostgreSQL abzurufen.

Beispiel (für die öffentliche Cloud):

```shell
az account get-access-token --resource https://ossrdbms-aad.database.windows.net
```

Der oben genannte Ressourcenwert muss genau wie gezeigt angegeben werden. Für andere Clouds kann der Ressourcenwert wie folgt nachgeschlagen werden:

```shell
az cloud show
```

Für Version 2.0.71 oder höher der Azure CLI kann der Befehl in der folgenden, bequemeren Version für alle Clouds angegeben werden:

```shell
az account get-access-token --resource-type oss-rdbms
```

Nachdem die Authentifizierung erfolgreich war, gibt Azure AD ein Zugriffstoken zurück:

```json
{
  "accessToken": "TOKEN",
  "expiresOn": "...",
  "subscription": "...",
  "tenant": "...",
  "tokenType": "Bearer"
}
```

Das Token ist eine Base-64-Zeichenfolge, die alle Informationen zum authentifizierten Benutzer codiert und die für den Azure Database for PostgreSQL-Dienst bestimmt ist.

> [!NOTE]
> Die Gültigkeitsdauer des Zugriffstokens liegt zwischen 5 Minuten und 60 Minuten. Es wird empfohlen, das Zugriffstoken erst kurz vor dem Initiieren der Anmeldung bei Azure Database for PostgreSQL abzurufen.

### <a name="step-3-use-token-as-password-for-logging-in-with-postgresql"></a>Schritt 3: Verwenden des Tokens als Kennwort für die Anmeldung bei PostgreSQL

Beim Herstellen einer Verbindung müssen Sie das Zugriffstoken als PostgreSQL-Benutzerkennwort verwenden.

Wenn der `psql`-Befehlszeilenclient verwendet wird, muss das Zugriffstoken über die Umgebungsvariable `PGPASSWORD` übergeben werden, da das Zugriffstoken die Kennwortlänge überschreitet, die `psql` direkt annehmen kann:

Windows-Beispiel:

```shell
set PGPASSWORD=<copy/pasted TOKEN value from step 2>
```

Beispiel für Linux/macOS:

```shell
export PGPASSWORD=<copy/pasted TOKEN value from step 2>
```

Nun können Sie eine Verbindung mit Azure Database for PostgreSQL wie gewohnt initiieren:

```shell
psql "host=mydb.postgres... user=user@tenant.onmicrosoft.com@mydb dbname=postgres"
```

Sie sind jetzt bei Ihrem PostgreSQL-Server über Azure AD-Authentifizierung authentifiziert.

## <a name="token-validation"></a>Tokenüberprüfung

Azure AD-Authentifizierung in Azure Database for PostgreSQL stellt sicher, dass der Benutzer auf dem PostgreSQL-Server vorhanden ist, und überprüft die Gültigkeit des Tokens, indem der Inhalt des Tokens überprüft wird. Die folgenden Tokenvalidierungsschritte werden ausgeführt:

-   Das Token ist von Azure AD signiert und wurde nicht manipuliert.
-   Das Token wurde von Azure AD für den dem Server zugeordneten Mandanten ausgestellt.
-   Das Token ist nicht abgelaufen.
-   Das Token gilt für die Azure Database for PostgreSQL-Ressource (und nicht für eine andere Azure-Ressource).

## <a name="migrating-existing-postgresql-users-to-azure-ad-based-authentication"></a>Migrieren vorhandener PostgreSQL-Benutzer zu Azure AD-basierter Authentifizierung

Sie können Azure AD-Authentifizierung für vorhandene Benutzer aktivieren. Es müssen zwei Fälle berücksichtigt werden:

### <a name="case-1-postgresql-username-matches-the-azure-ad-user-principal-name"></a>Fall 1: Der PostgreSQL-Benutzername entspricht dem Azure AD-Benutzerprinzipalnamen

In dem unwahrscheinlichen Fall, dass Ihre vorhandenen Benutzer bereits den Azure AD-Benutzernamen entsprechen, können Sie ihnen die Rolle `azure_ad_user` zuweisen, um sie für die Azure AD-Authentifizierung zu aktivieren:

```sql
GRANT azure_ad_user TO "existinguser@yourtenant.onmicrosoft.com";
```

Sie sind jetzt in der Lage, sich mit Azure AD-Anmeldeinformationen anzumelden, anstatt ihr zuvor konfiguriertes PostgreSQL-Benutzerkennwort zu verwenden.

### <a name="case-2-postgresql-username-is-different-than-the-azure-ad-user-principal-name"></a>Fall 2: Der PostgreSQL-Benutzername entspricht nicht dem Azure AD-Benutzerprinzipalnamen

Wenn ein PostgreSQL-Benutzer entweder nicht in Azure AD vorhanden ist oder einen anderen Benutzernamen aufweist, können Sie Azure AD-Gruppen verwenden, um sich als dieser PostgreSQL-Benutzer zu authentifizieren. Sie können vorhandene Azure Database for PostgreSQL-Benutzer zu Azure AD migrieren, indem Sie eine Azure AD-Gruppe mit einem Namen erstellen, der mit dem PostgreSQL-Benutzer übereinstimmt, und dem vorhandenen PostgreSQL-Benutzer dann die Rolle azure_ad_user zuweisen:

```sql
GRANT azure_ad_user TO "DBReadUser";
```

Dabei wird davon ausgegangen, dass Sie eine Gruppe „DBReadUser“ in Azure AD erstellt haben. Benutzer, die zu dieser Gruppe gehören, können sich nun bei der Datenbank als dieser Benutzer anmelden.

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der Gesamtkonzepte für die [Azure Active Directory-Authentifizierung mit Azure Database for PostgreSQL: Einzelserver](concepts-aad-authentication.md)

<!--Image references-->

[1]: ./media/concepts-aad-authentication/authentication-flow.png
[2]: ./media/concepts-aad-authentication/set-aad-admin.png
