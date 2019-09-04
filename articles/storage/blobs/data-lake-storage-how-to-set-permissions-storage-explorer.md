---
title: Festlegen von Berechtigungen für Data Lake Storage Gen2 mit Azure Storage-Explorer
description: In dieser Anleitung erfahren Sie, wie Sie mit dem Azure Storage-Explorer Berechtigungen für Dateien und Verzeichnisse in Ihrem Azure Data Lake Storage Gen2-fähigen Speicherkonto festlegen.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 2cc93ad9ae1d9d6d21064a2a80c3d62bf661dabc
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69992277"
---
# <a name="set-file-and-directory-level-permissions-using-azure-storage-explorer-with-azure-data-lake-storage-gen2"></a>Festlegen von Berechtigungen auf Datei- und Verzeichnisebene mithilfe des Azure Storage-Explorers mit Azure Data Lake Storage Gen2

Dateien, die in Azure Data Lake Storage Gen2 gespeichert sind, unterstützen differenzierte Berechtigungen und die Verwaltung von Zugriffssteuerungslisten (Access Control Lists, ACLs). In Kombination ermöglichen Ihnen differenzierte Berechtigungen und die ACL-Verwaltung den Zugriff auf Ihre Daten auf sehr präzise Weise.

In diesem Artikel erfahren Sie, wie Sie Azure Storage-Explorer für folgende Aufgaben verwenden:

> [!div class="checklist"]
> * Festlegen von Berechtigungen auf Dateiebene
> * Festlegen von Berechtigungen auf Verzeichnisebene
> * Hinzufügen von Benutzern oder Gruppen zu einer Zugriffssteuerungsliste

## <a name="prerequisites"></a>Voraussetzungen

Um den Prozess bestmöglich abbilden zu können, müssen Sie den [Schnellstart für Azure Storage-Explorer](data-lake-storage-Explorer.md) abgeschlossen haben. Dadurch wird sichergestellt, dass sich Ihr Speicherkonto im am besten geeigneten Zustand befindet (Container erstellt und Daten hochgeladen).

## <a name="managing-access"></a>Verwalten des Zugriffs

Sie können Berechtigungen im Stammverzeichnis Ihres Containers festlegen. Hierzu müssen Sie mit Ihrem individuellen Konto mit entsprechenden Rechten beim Azure Storage-Explorer angemeldet sein (anstatt über eine Verbindungszeichenfolge). Klicken Sie mit der rechten Maustaste auf Ihren Container, und wählen Sie **Berechtigungen verwalten** aus, um das Dialogfeld **Berechtigung verwalten** zu öffnen.

![Microsoft Azure Storage-Explorer – Verzeichniszugriff verwalten](media/storage-quickstart-blobs-storage-Explorer/manageperms.png)

Das Dialogfeld **Berechtigung verwalten** ermöglicht es Ihnen, Berechtigungen für den Besitzer und die Besitzergruppe zu verwalten. Sie können auch neue Benutzer und Gruppen zur Zugriffssteuerungsliste hinzufügen, für die Sie dann die Berechtigungen verwalten können.

Um einen neuen Benutzer oder eine Gruppe zur Zugriffssteuerungsliste hinzuzufügen, wählen Sie das Feld **Benutzer oder Gruppe hinzufügen** aus.

Geben Sie den entsprechenden Azure Active Directory (AAD)-Eintrag ein, den Sie zur Liste hinzufügen möchten, und wählen Sie dann **Hinzufügen**.

Der Benutzer oder die Gruppe erscheint nun im Feld **Benutzer und Gruppen:** , sodass Sie mit der Verwaltung ihrer Berechtigungen beginnen können.

> [!NOTE]
> Es ist eine Best Practice und es wird empfohlen, eine Sicherheitsgruppe in AAD zu erstellen und die Berechtigungen für die Gruppe und nicht für einzelne Benutzer zu verwalten. Einzelheiten zu dieser Empfehlung sowie zu anderen Best Practices finden Sie unter [Best Practices für Data Lake Storage Gen2](data-lake-storage-best-practices.md).

Es gibt zwei Kategorien von Berechtigungen, die Sie zuweisen können: Zugriffs-ACLs und Standard-ACLs.

* **Zugriff**: Zugriffs-ACLs steuern den Zugriff auf ein Objekt. Dateien und Verzeichnisse verfügen jeweils über Zugriffs-ACLs.

* **Standard**: Eine Vorlage von ACLs, die einem Verzeichnis zugeordnet ist, das die Zugriffs-ACLs für alle untergeordneten Elemente bestimmt, die unter diesem Verzeichnis erstellt werden. Dateien verfügen über keine Standard-ACLs.

Innerhalb dieser beiden Kategorien gibt es drei Berechtigungen, die Sie dann Dateien oder Verzeichnissen zuweisen können: **Lesen**, **Schreiben** und **Ausführen**.

>[!NOTE]
> Wenn Sie hier eine Auswahl treffen, werden keine Berechtigungen für ein aktuell vorhandenes Element im Verzeichnis festgelegt. Sie müssen jedes einzelne Element aufrufen und die Berechtigungen manuell festlegen, wenn die Datei bereits existiert.

Sie können Berechtigungen für einzelne Verzeichnisse sowie einzelne Dateien verwalten, was Ihnen eine differenzierte Zugriffssteuerung ermöglicht. Der Prozess zur Verwaltung von Berechtigungen für Verzeichnisse und Dateien ist derselbe wie oben beschrieben. Klicken Sie mit der rechten Maustaste auf die Datei oder das Verzeichnis, für das Sie die Berechtigungen verwalten möchten, und folgen Sie dem gleichen Prozess.

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie gelernt, wie Sie mit **Azure Storage-Explorer** Berechtigungen für Dateien und Verzeichnisse festlegen. Um mehr über ACLs, einschließlich Standard-ACLs, Zugriffs-ACLs, ihr Verhalten und die entsprechenden Berechtigungen zu erfahren, lesen Sie unseren konzeptionellen Artikel zu diesem Thema.

> [!div class="nextstepaction"]
> [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md)
