---
title: Benutzerdefinierte Rollen für Onlinemigrationen von SQL Server zu verwalteten SQL-Datenbank-Instanzen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die benutzerdefinierten Rollen für Onlinemigrationsvorgänge von SQL Server zu verwalteten SQL-Datenbank-Instanzen verwenden.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/25/2019
ms.openlocfilehash: 8148e029bf343613a230b20d0397fa7851c96712
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942151"
---
# <a name="custom-roles-for-sql-server-to-sql-database-managed-instance-online-migrations"></a>Benutzerdefinierte Rollen für Onlinemigrationsvorgänge von SQL Server zu verwalteten SQL-Datenbank-Instanzen

In Azure Database Migration Service wird zur Interaktion mit Azure-Diensten eine App-ID verwendet. Die App-ID erfordert die Rolle „Mitwirkender“ auf Abonnementebene (durch die Sicherheitsabteilungen vieler Unternehmen nicht zugelassen) oder die Erstellung benutzerdefinierter Rollen, die die spezifischen für Azure Database Migration Service erforderlichen Berechtigungen gewähren. Da in Azure Active Directory eine Beschränkung von maximal 2.000 benutzerdefinierten Rollen gilt, sollten Sie alle speziell für die App-ID erforderlichen Berechtigungen in einer oder zwei benutzerdefinierten Rollen kombinieren und dann der App-ID die benutzerdefinierte Rolle für spezifische Objekte oder Ressourcengruppen (im Gegensatz zur Abonnementebene) zuweisen. Wenn die Anzahl der benutzerdefinierten Rollen nicht von Bedeutung ist, können Sie die benutzerdefinierten Rollen nach Ressourcentyp aufteilen und wie nachfolgend beschrieben insgesamt drei benutzerdefinierte Rollen erstellen.

Im Abschnitt „AssignableScopes“ der JSON-Zeichenfolge der Rollendefinition können Sie festlegen, wo die Berechtigungen in der Benutzeroberfläche **Rollenzuweisung hinzufügen** im Portal angezeigt werden. Es empfiehlt sich, die Rollen auf Ressourcengruppen- oder sogar Ressourcenebene zu definieren, um zu vermeiden, dass die Benutzeroberfläche durch zusätzliche Rollen unübersichtlich wird. Beachten Sie, dass dadurch nicht die eigentliche Rollenzuweisung erfolgt.

## <a name="minimum-number-of-roles"></a>Mindestanzahl von Rollen

Derzeit wird empfohlen, mindestens zwei benutzerdefinierte Rollen für die App-ID zu erstellen: eine auf Ressourcenebene und eine auf Abonnementebene.

> [!NOTE]
> Die Anforderung für die letzte benutzerdefinierte Rolle kann zu einem bestimmten Zeitpunkt nicht mehr nötig sein, da neuer Code für die verwaltete SQL-Datenbank-Instanz in Azure bereitgestellt wird.

**Benutzerdefinierte Rolle für die App-ID:** Diese Rolle ist für die Migration von Azure Database Migration Service auf *Ressourcenebene* oder *Ressourcengruppenebene* erforderlich. (Weitere Informationen zur App-ID finden Sie im Artikel [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).)

```json
{
  "Name": "DMS Role - App ID",
  "IsCustom": true,
  "Description": "DMS App ID access to complete MI migrations",
  "Actions": [
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageaccounts/blobservices/read",
        "Microsoft.Storage/storageaccounts/blobservices/write",
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/write",
        "Microsoft.Sql/managedInstances/databases/read",
        "Microsoft.Sql/managedInstances/databases/write",
        "Microsoft.Sql/managedInstances/databases/delete",
        "Microsoft.Sql/managedInstances/metrics/read",
        "Microsoft.DataMigration/locations/*",
        "Microsoft.DataMigration/services/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/ResourceGroups/<StorageAccount_rg_name>",
    "/subscriptions/<subscription_id>/ResourceGroups/<ManagedInstance_rg_name>",
    "/subscriptions/<subscription_id>/ResourceGroups/<DMS_rg_name>",
  ]
}
```

**Benutzerdefinierte Rolle für die App-ID – Abonnement:** Diese Rolle ist für die Migration von Azure Database Migration Service auf *Abonnementebene* erforderlich.

```json
{
  "Name": "DMS Role - App ID - Sub",
  "IsCustom": true,
  "Description": "DMS App ID access at subscription level to complete MI migrations",
  "Actions": [
        "Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>"
  ]
}
```

Der JSON-Code oben muss in drei Textdateien gespeichert werden. Sie können die Rollen mithilfe der PowerShell-Cmdlets AzureRM oder Az oder über die Azure-Befehlszeilenschnittstelle unter Verwendung von **New-AzureRmRoleDefinition (AzureRM)** bzw. **New-AzRoleDefinition (Az)** erstellen.

Weitere Informationen finden Sie im Artikel [Benutzerdefinierte Rollen für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).

Nach dem Erstellen dieser benutzerdefinierten Rollen müssen Sie den Benutzern und App-IDs für die entsprechenden Ressourcen oder Ressourcengruppen Rollenzuweisungen hinzufügen:

* Die Rolle „DMS Role – App ID“ muss der App-ID, die für die Migrationsvorgänge verwendet wird, sowie auf den Ressourcenebenen des Speicherkontos, der Azure Database Migration Service-Instanz und der verwalteten SQL-Datenbank-Instanz zugewiesen werden.
* Die Rolle „DMS Role – App ID - Sub“ muss der App-ID auf Abonnementebene zugewiesen werden. (Bei der Zuweisung auf Ressourcen- oder Ressourcengruppenebene treten Fehler auf.) Diese Anforderung gilt, bis ein Codeupdate bereitgestellt wurde.

## <a name="expanded-number-of-roles"></a>Erweiterte Anzahl von Rollen

Wenn die Anzahl der benutzerdefinierten Rollen in Ihrer Azure Active Directory-Instanz nicht von Bedeutung ist, empfiehlt sich die Erstellung von insgesamt drei Rollen. Sie benötigen weiterhin die Rolle „DMS Role – App ID – Sub“, die Rolle „DMS Role – App ID“ von oben wird jedoch nach Ressourcentyp in zwei verschiedene Rollen aufgeteilt.

**Benutzerdefinierte Rolle für die App-ID für die verwaltete SQL-Datenbank-Instanz**

```json
{
  "Name": "DMS Role - App ID - SQL MI",
  "IsCustom": true,
  "Description": "DMS App ID access to complete MI migrations",
  "Actions": [
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/write",
        "Microsoft.Sql/managedInstances/databases/read",
        "Microsoft.Sql/managedInstances/databases/write",
        "Microsoft.Sql/managedInstances/databases/delete",
        "Microsoft.Sql/managedInstances/metrics/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/resourceGroups/<ManagedInstance_rg_name>"
  ]
}
```

**Benutzerdefinierte Rolle für die App-ID für den Speicher**

```json
{
  "Name": "DMS Role - App ID - Storage",
  "IsCustom": true,
  "Description": "DMS App ID storage access to complete MI migrations",
  "Actions": [
"Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageaccounts/blobservices/read",
        "Microsoft.Storage/storageaccounts/blobservices/write"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/resourceGroups/<StorageAccount_rg_name>"
  ]
}
```

## <a name="role-assignment"></a>Rollenzuweisung

Um Benutzern oder einer App-ID eine Rolle zuzuweisen, öffnen Sie das Azure-Portal, und führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu der entsprechenden Ressourcengruppe oder Ressource (mit Ausnahme der Rolle, die für das Abonnement zugewiesen werden muss), wechseln Sie zu **Zugriffssteuerung**, und scrollen Sie zu den soeben erstellten Rollen.

2. Wählen Sie die entsprechende Rolle und dann die App-ID aus, und speichern Sie die Änderungen.

  Die App-IDs werden nun auf der Registerkarte **Rollenzuweisungen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der Migrationsanleitung für Ihr Szenario im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)
