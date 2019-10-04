---
title: 'Schnellstart: Erstellen eines Azure Database for PostgreSQL-Einzelservers mit dem CLI-Befehl „az postgres up“'
description: Schnellstartanleitung zum Erstellen eines Azure Database for PostgreSQL-Einzelservers mit dem „up“-Befehl der Azure CLI (Befehlszeilenschnittstelle).
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 49f71c199a2832d763bb3c19d878fade47dfb8e4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069077"
---
# <a name="quickstart-use-an-azure-cli-command-az-postgres-up-preview-to-create-an-azure-database-for-postgresql---single-server"></a>Schnellstart: Verwenden des Azure CLI-Befehls „az postgres up“ (Vorschau) zum Erstellen eines Azure Database for PostgreSQL-Einzelservers

> [!IMPORTANT]
> Der Befehl [az postgres up](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) der Azure-Befehlszeilenschnittstelle befindet sich in der Vorschauphase.

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In diesem Schnellstart wird veranschaulicht, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle und des Befehls [az postgres up](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) einen Azure Database for PostgreSQL-Server erstellen. Zusätzlich zum Server erstellt der Befehl `az postgres up` auch eine Beispieldatenbank und einen Root-Benutzer in der Datenbank, er öffnet die Firewall für Azure-Dienste und erstellt Firewallregeln für den Clientcomputer. Diese Standardeinstellungen tragen zur Beschleunigung des Entwicklungsprozesses bei.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Für den Artikel müssen Sie mindestens Version 2.0 der Azure-Befehlszeilenschnittstelle lokal ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Sie müssen sich mit dem Befehl [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) bei Ihrem Konto anmelden. Beachten Sie die Eigenschaft **ID** aus der Befehlsausgabe für den entsprechenden Abonnementnamen.

```azurecli
az login
```

Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Wählen Sie mithilfe des Befehls [az account set](/cli/azure/account) die Abonnement-ID unter Ihrem Konto aus. Ersetzen Sie den Platzhalter für die Abonnement-ID durch die **subscription ID**-Eigenschaft der Ausgabe von **az login** für Ihr Abonnement.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Damit Sie die Befehle verwenden können, installieren Sie die Erweiterung [db-up](/cli/azure/ext/db-up). Falls ein Fehler auftritt, vergewissern Sie sich, dass die neueste Version der Azure-Befehlszeilenschnittstelle installiert ist. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Erstellen Sie mit dem folgenden Befehl einen Azure Database for PostgreSQL-Server:

```azurecli
az postgres up
```

Der Server wird mit den folgenden Standardwerten erstellt (sofern Sie diese nicht manuell überschreiben):

**Einstellung** | **Standardwert** | **Beschreibung**
---|---|---
Servername | Systemgeneriert | Ein eindeutiger Name, der Ihren Azure-Datenbank für PostgreSQL-Server identifiziert.
resource-group | Systemgeneriert | Eine neue Azure-Ressourcengruppe
sku-name | GP_Gen5_2 | Der Name der SKU. Folgt der Konvention „{Tarif}\_{Computegeneration}\_{virtuelle Kerne}“ in Kurzform. Der Standardwert ist ein Gen5-Server vom Typ „Universell“ mit zwei virtuellen Kernen. Informationen zu den Tarifen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/postgresql/).
backup-retention | 7 | Gibt an, wie lange eine Sicherung aufbewahrt wird. Die Einheit ist Tage.
geo-redundant-backup | Deaktiviert | Gibt an, ob georedundante Sicherungen für diesen Server aktiviert werden sollen.
location | westus2 | Der Azure-Standort für den Server.
ssl-enforcement | Deaktiviert | Gibt an, ob SSL für diesen Server aktiviert werden soll.
storage-size | 5120 | Die Speicherkapazität des Servers (Einheit: MB).
Version | 10 | Die PostgreSQL-Hauptversion.
admin-user | Systemgeneriert | Der Benutzername für den Administrator.
admin-password | Systemgeneriert | Das Kennwort des Administratorbenutzers.

> [!NOTE]
> Weitere Informationen zum Befehl `az postgres up` und den zugehörigen zusätzlichen Parametern finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up).

Nach der Erstellung weist Ihr Server die folgenden Einstellungen auf:

- Es wird eine Firewallregel namens „devbox“ erstellt. Die Azure-Befehlszeilenschnittstelle versucht, die IP-Adresse des Computers zu ermitteln, auf dem der Befehl `az postgres up` ausgeführt wird, und fügt diese IP-Adresse der Whitelist hinzu.
- „Zugriff auf Azure-Dienste zulassen“ ist aktiviert. Mit dieser Einstellung wird die Firewall des Servers so konfiguriert, dass sie Verbindungen von allen Azure-Ressourcen akzeptiert – auch von Ressourcen, die nicht in Ihrem Abonnement enthalten sind.
- Es wird eine leere Datenbank mit der Bezeichnung „sampledb“ erstellt.
- Es wird ein neuer Benutzer mit dem Namen „root“ und Berechtigungen für „sampledb“ erstellt.

> [!NOTE]
> Azure Database for PostgreSQL kommuniziert über Port 5432. Wenn Sie eine Verbindung aus einem Unternehmensnetzwerk heraus herstellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. Ihre IT-Abteilung muss Port 5432 öffnen, damit Sie eine Verbindung mit Ihrem Server herstellen können.

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Nach Abschluss des Befehls `az postgres up` wird eine Liste von Verbindungszeichenfolgen für verbreitete Programmiersprachen an Sie zurückgegeben. Diese Verbindungszeichenfolgen sind mit bestimmten Attributen Ihres neu erstellten Azure Database for PostgreSQL-Servers vorkonfiguriert.

Sie können den Befehl [az postgres show-connection-string](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-show-connection-string) verwenden, um diese Verbindungszeichenfolgen erneut aufzulisten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie alle im Schnellstart erstellten Ressourcen mit dem folgenden Befehl. Dieser Befehl löscht den Azure Database for PostgreSQL-Server und die Ressourcengruppe.

```azurecli
az postgres down --delete-group
```

Wenn Sie nur den neu erstellten Server löschen möchten, können Sie den Befehl [az postgres down](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-down) ausführen.

```azurecli
az postgres down
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
