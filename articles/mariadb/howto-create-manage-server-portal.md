---
title: Erstellen und Verwalten eines Azure Database for MariaDB-Servers mit dem Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal schnell einen neuen Azure Database for MariaDB-Server erstellen und verwalten können.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 08/09/2019
ms.openlocfilehash: 5aae3eb0582956ccb45cc41d8400f489f1077bad
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70143201"
---
# <a name="create-and-manage-azure-database-for-mariadb-server-using-azure-portal"></a>Erstellen und Verwalten eines Azure Database for MariaDB-Servers mit dem Azure-Portal
In diesem Artikel wird beschrieben, wie Sie schnell einen neuen Azure Database for MariaDB-Server erstellen können. Außerdem erhalten Sie Informationen zum Verwalten des Servers mithilfe des Azure-Portals. Die Serververwaltung umfasst das Anzeigen von Serverdetails und Datenbanken, das Zurücksetzen des Kennworts, das Skalieren von Ressourcen und das Löschen des Servers.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-mariadb-server"></a>Erstellen eines Azure Database for MariaDB-Servers
Führen Sie die folgenden Schritte aus, um einen Azure Database for MariaDB-Server mit dem Namen „mydemoserver“ zu erstellen.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Ressource erstellen**.

2. Wählen Sie auf der Seite „Neu“ die Option **Datenbanken** und anschließend auf der Seite „Datenbanken“ die Option **Azure Database for MariaDB** aus.

    > Ein Azure Database for MariaDB-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-pricing-tiers.md) erstellt. Die Datenbank wird in einer Azure-Ressourcengruppe und auf einem Azure Database for MariaDB-Server erstellt.

   ![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Tragen Sie folgende Informationen in das Formular „Azure Database for MariaDB“ ein:

    | **Formularfeld** | **Feldbeschreibung** |
    |----------------|-----------------------|
    | *Servername* | mydemoserver (Servername ist global eindeutig) |
    | *Abonnement* | mysubscription (im Dropdownmenü auswählen) |
    | *Ressourcengruppe* | myresourcegroup (neue Ressourcengruppe erstellen oder eine bereits vorhandene verwenden) |
    | *Quelle auswählen* | Leer (leeren MariaDB-Server erstellen) |
    | *Serveradministratoranmeldung* | myadmin (Administratorkontoname einrichten) |
    | *Kennwort* | Kennwort des Administratorkontos festlegen |
    | *Kennwort bestätigen* | Kennwort des Administratorkontos bestätigen |
    | *Location* | Asien, Südosten (zwischen „Europa, Norden“ und „USA, Westen“ auswählen) |
    | *Version* | 10.3 (Azure Database for MariaDB-Serverversion auswählen) |

   ![create-new-server](./media/howto-create-manage-server-portal/form-field.png)

4. Klicken Sie auf **Server konfigurieren**, um die Dienstebene und die Leistungsstufe für Ihren neuen Server anzugeben. Wählen Sie die Registerkarte **Allgemein** aus. *Gen 5*, *4 virtuelle Kerne*, *100 GB* und *7 Tage* sind die Standardwerte für **Computegeneration**, **Virtuelle Kerne**, **Speicher** und **Aufbewahrungszeit für Sicherung**. Sie können diese Schieberegler unverändert lassen. Wählen Sie zum Aktivieren der Serversicherungen in georedundantem Speicher unter **Optionen für Sicherungsredundanz** die Option **Georedundant** aus.

   ![Servertarif erstellen](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klicken Sie auf **Überprüfen + erstellen**, um zum Überprüfungsbildschirm zu wechseln und alle Details zu überprüfen. Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.

    > Wählen Sie die Option **An das Dashboard anheften** aus, um das leichte Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.

## <a name="update-an-azure-database-for-mariadb-server"></a>Aktualisieren eines Azure Database for MariaDB-Servers
Nach der Bereitstellung des neuen Servers stehen dem Benutzer mehrere Optionen zum Konfigurieren des vorhandenen Servers zur Verfügung. Hierzu zählen beispielsweise das Zurücksetzen des Administratorkennworts, das Ändern des Tarifs und das zentrale Hoch- oder Herunterskalieren des Servers durch eine Änderung der virtuellen Kerne oder des Speichers.

### <a name="change-the-administrator-user-password"></a>Ändern des Administratorbenutzerkennworts
1. Klicken Sie auf dem Serverblatt **Übersicht** auf **Kennwort zurücksetzen**, um das Fenster für die Kennwortzurücksetzung anzuzeigen.

   ![Übersicht](./media/howto-create-manage-server-portal/overview.png)

2. Geben Sie ein neues Kennwort ein, und bestätigen Sie das Kennwort wie unten gezeigt im Fenster:

   ![Kennwort zurücksetzen](./media/howto-create-manage-server-portal/reset-password.png)

3. Klicken Sie auf **OK**, um das neue Kennwort zu speichern.

### <a name="change-the-pricing-tier"></a>Ändern des Tarifs
> [!NOTE]
> Es wird nur die Skalierung von „Universell“ auf „Arbeitsspeicheroptimiert“ (und umgekehrt) unterstützt. Beachten Sie, dass der Wechsel in den und aus dem Basic-Tarif nach der Servererstellung in Azure Database for MariaDB nicht unterstützt wird.
> 
1. Klicken Sie unter **Einstellungen** auf **Tarif**.
2. Wählen Sie den **Tarif** aus, zu dem Sie wechseln möchten.

    ![change-pricing-tier](./media/howto-create-manage-server-portal/change-pricing-tier.png)

4. Klicken Sie zum Speichern der Änderungen auf **OK**. 

### <a name="scale-vcores-updown"></a>Zentrales Hoch-/Herunterskalieren von virtuellen Kernen

1. Klicken Sie unter **Einstellungen** auf **Tarif**.

2. Ändern Sie die Einstellung **Virtuelle Kerne**, indem Sie den Schieberegler auf den gewünschten Wert ziehen.

    ![Skalieren von Computeressourcen](./media/howto-create-manage-server-portal/scale-compute.png)

3. Klicken Sie zum Speichern der Änderungen auf **OK**.

### <a name="scale-storage-up"></a>Zentrales Hochskalieren des Speichers

1. Klicken Sie unter **Einstellungen** auf **Tarif**.

2. Ändern Sie die Einstellung **Speicher**, indem Sie den Schieberegler auf den gewünschten Wert ziehen.

    ![Skalieren des Speichers](./media/howto-create-manage-server-portal/scale-storage.png)

3. Klicken Sie zum Speichern der Änderungen auf **OK**.

## <a name="delete-an-azure-database-for-mariadb-server"></a>Löschen eines Azure Database for MariaDB-Servers

1. Klicken Sie auf dem Serverblatt **Übersicht** auf die Schaltfläche **Löschen**, um die Bestätigungsaufforderung zum Löschen zu öffnen.

    ![delete](./media/howto-create-manage-server-portal/delete.png)

2. Geben Sie den Namen des Servers in das Eingabefeld für die doppelte Bestätigung ein.

    ![Löschen bestätigen](./media/howto-create-manage-server-portal/confirm.png)

3. Klicken Sie auf die Schaltfläche **Löschen**, um das Löschen des Servers zu bestätigen. Warten Sie, bis auf der Benachrichtigungsleiste die Popupmeldung „MariaDB-Server erfolgreich gelöscht“ angezeigt wird.

## <a name="list-the-azure-database-for-mariadb-databases"></a>Auflisten der Azure Database for MariaDB-Datenbanken
Scrollen Sie auf dem Serverblatt **Übersicht** nach unten, bis die Datenbankkachel im unteren Bereich angezeigt wird. In der Tabelle werden alle auf dem Server vorhandenen Datenbanken aufgeführt.

   ![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mariadb-server"></a>Anzeigen von Details zu einem Azure Database for MariaDB-Server
Klicken Sie unter **Einstellungen** auf **Eigenschaften**, um detaillierte Informationen zum Server anzuzeigen.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Azure Database for MariaDB-Servers mithilfe des Azure-Portals](./quickstart-create-mariadb-server-database-using-azure-portal.md)