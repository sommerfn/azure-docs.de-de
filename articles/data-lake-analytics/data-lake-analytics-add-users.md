---
title: Hinzufügen von Benutzern zu einem Azure Data Lake Analytics-Konto
description: Erfahren Sie, wie Sie ordnungsgemäß Benutzer zu Ihrem Data Lake Analytics-Konto hinzufügen.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 8323c4e1b236444f55dab826d2567491f5f0f736
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60629321"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Hinzufügen eines Benutzers im Azure-Portal

## <a name="start-the-add-user-wizard"></a>Starten des Assistenten für das Hinzufügen von Benutzern
1. Öffnen Sie Azure Data Lake Analytics über https://portal.azure.com.
2. Klicken Sie auf **Assistent für das Hinzufügen von Benutzern**.
3. Suchen Sie im Schritt **Benutzer auswählen** nach dem Benutzer, den Sie hinzufügen möchten. Klicken Sie auf **Auswählen**.
4. Wählen Sie im Schritt **Rolle auswählen** die Option **Data Lake Analytics-Entwickler** aus. Diese Rolle verfügt über die mindestens erforderlichen Berechtigungen zum Übermitteln/Überwachen/Verwalten von U-SQL-Aufträgen. Führen Sie eine Zuweisung zu dieser Rolle durch, wenn die Gruppe nicht zum Verwalten von Azure-Diensten vorgesehen ist.
5. Wählen Sie im Schritt **Katalogberechtigungen auswählen** alle zusätzlichen Datenbanken aus, auf die der Benutzer Zugriff benötigt. Für das Übermitteln von Aufträgen ist Lese- und Schreibzugriff auf die Masterdatenbank erforderlich. Klicken Sie anschließend auf **OK**.
6. Überprüfen Sie im letzten Schritt **Ausgewählte Berechtigungen zuweisen** die Änderungen, die der Assistent vornehmen wird. Klicken Sie auf **OK**.


## <a name="configure-acls-for-data-folders"></a>Konfigurieren von Zugriffssteuerungslisten (ACLs) für Datenordner
Erteilen Sie je nach Bedarf „R-X“ oder „RWX“ für Ordner mit Eingabe- und Ausgabedaten.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>Fügen Sie den Benutzer optional zur Azure Data Lake Storage Gen1-Rolle **Leser** hinzu.
1.  Suchen Sie Ihr Azure Data Lake Storage Gen1-Konto.
2.  Klicken Sie auf **Benutzer**.
3. Klicken Sie auf **Hinzufügen**.
4.  Wählen Sie eine Azure RBAC-Rolle aus, die dieser Gruppe zugewiesen werden soll.
5.  Weisen Sie die Rolle „Leser“ zu. Diese Rolle verfügt über die mindestens erforderlichen Berechtigungen zum Durchsuchen/Verwalten von Daten, die in ADLSGen1 gespeichert sind. Führen Sie eine Zuweisung zu dieser Rolle durch, wenn die Gruppe nicht zum Verwalten von Azure-Diensten vorgesehen ist.
6.  Geben Sie den Namen der Gruppe ein.
7.  Klicken Sie auf **OK**.

## <a name="adding-a-user-using-powershell"></a>Hinzufügen eines Benutzers mit PowerShell

1. Befolgen Sie die Anweisungen in diesem Leitfaden: [Installieren und Konfigurieren von Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Laden Sie das PowerShell-Skript [Add-AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) herunter.
3. Führen Sie das PowerShell-Skript aus. 

Der Beispielbefehl zum Erteilen von Benutzerzugriff für das Übermitteln von Aufträgen, das Anzeigen von Metadaten für neue Aufträge und das Anzeigen von alten Metadaten lautet wie folgt:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Nächste Schritte

* [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
* [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

