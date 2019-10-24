---
title: Aktivieren und Erstellen großer Dateifreigaben – Azure Files
description: In diesem Artikel erfahren Sie, wie Sie große Dateifreigaben aktivieren und erstellen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 00db5905c008644bc21704179363849756fa7dcc
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518162"
---
# <a name="how-to-enable-and-create-large-file-shares"></a>Aktivieren und Erstellen großer Dateifreigaben

Ursprünglich konnten standardmäßige Dateifreigaben nur auf maximal 5 TiB skaliert werden. Ab jetzt lassen sie sich dank großer Dateifreigaben auf bis zu 100 TiB skalieren. Premium-Dateifreigaben können standardmäßig auf bis zu 100 TiB skaliert werden. 

Um unter Verwendung von Standarddateifreigaben eine Skalierung auf bis zu 100 TiB vorzunehmen, müssen Sie Ihr Speicherkonto für die Verwendung großer Dateifreigaben aktivieren. Sie können entweder ein vorhandenes Konto aktivieren oder ein neues Konto erstellen, um große Dateifreigaben zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="restrictions"></a>Einschränkungen

Konten, die für große Dateifreigaben aktiviert sind, unterstützen LRS oder ZRS. GZRS, GRS oder RA-GRS werden von Konten, die für große Dateifreigaben aktiviert sind, derzeit nicht unterstützt. Die Aktivierung großer Dateifreigaben für ein Konto kann nicht rückgängig gemacht werden. Danach kann Ihr Konto nicht mehr für GZRS, GRS oder RA-GRS konvertiert werden.

## <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Speicherkonten** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Speicherkonten**.
1. Klicken Sie im angezeigten Fenster **Speicherkonten** auf **Hinzufügen**.
1. Wählen Sie das Abonnement aus, in dem das Speicherkonto erstellt werden soll.
1. Wählen Sie unter dem Feld **Ressourcengruppe** die Option **Neu erstellen**. Geben Sie einen Namen für die neue Ressourcengruppe an, wie in der folgenden Abbildung dargestellt.

    ![Screenshot: Erstellen einer Ressourcengruppe im Portal](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Geben Sie als Nächstes einen Namen für Ihr Speicherkonto ein. Der gewählte Name muss innerhalb von Azure eindeutig sein. Der Name muss ebenfalls zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.
1. Wählen Sie einen Speicherort für Ihr Speicherkonto aus, und vergewissern Sie sich, dass es sich um eine der [für LFS unterstützten Regionen](storage-files-planning.md#regional-availability) handelt.
1. Legen Sie die Replikation auf **lokal redundanten Speicher** oder **zonenredundanten Speicher** fest.
1. Behalten Sie die Standardwerte für diese Felder bei:

   |Feld  |Wert  |
   |---------|---------|
   |Bereitstellungsmodell     |Ressourcen-Manager         |
   |Leistung     |Standard         |
   |Kontoart     |StorageV2 (universell v2)         |
   |Zugriffsebene     |Heiß         |

1. Wählen Sie erst **Erweitert** und dann für große **** Dateifreigaben die Option **Aktiviert** aus.
1. Wählen Sie **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.

    ![large-file-shares-advanced-enable.png](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. Klicken Sie auf **Erstellen**.

## <a name="enable-on-existing-account"></a>Aktivieren für ein vorhandenes Konto

Sie können große Dateifreigaben auch für vorhandene Konten aktivieren. In diesem Fall kann für das Konto keine Konvertierung mehr in GZRS, GRS oder RA-GRS durchgeführt werden. Die Auswahl kann für dieses Konto nicht rückgängig gemacht werden.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu dem Speicherkonto, für das Sie große Dateifreigaben aktivieren möchten.
1. Öffnen Sie das Speicherkonto, und wählen Sie **Konfiguration** aus.
1. Wählen Sie für große Dateifreigaben die Option **Aktiviert** aus, und wählen Sie dann **Speichern** aus.
1. Wählen Sie **Übersicht** und dann **Aktualisieren** aus.

![enable-large-file-shares-on-existing.png](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Sie haben jetzt große Dateifreigaben für Ihr Speicherkonto aktiviert.

Falls Sie die Fehlermeldung „Große Dateifreigaben sind für das Konto noch nicht verfügbar“ erhalten, können Sie einige Zeit warten, weil sich Ihre Region wahrscheinlich gerade mitten im Rollout befindet. Falls jedoch dringender Bedarf besteht, wenden Sie sich an den Support.

## <a name="create-a-large-file-share"></a>Erstellen einer großen Dateifreigabe

Das Erstellen einer großen Dateifreigabe ist nahezu identisch mit dem Erstellen einer standardmäßigen Dateifreigabe. Der Hauptunterschied besteht darin, dass Sie ein Kontingent von bis zu 100 TiB festlegen können.

1. Wählen Sie in Ihrem Speicherkonto die Option **Dateifreigaben** aus.
1. Klicken Sie auf **+ Dateifreigabe**.
1. Geben Sie einen Namen für die Dateifreigabe und (optional) die gewünschte Kontingentgröße (bis zu 100 TiB) ein, und wählen Sie dann **Erstellen** aus. 

![large-file-shares-create-share.png](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

## <a name="expand-existing-file-shares"></a>Erweitern vorhandener Dateifreigaben

Sobald Sie große Dateifreigaben in Ihrem Speicherkonto aktiviert haben, können Sie vorhandene Freigaben auf das höhere Kontingent erweitern.

1. Wählen Sie in Ihrem Speicherkonto die Option **Dateifreigaben** aus.
1. Klicken Sie mit der rechten Maustaste auf die Dateifreigabe, und wählen Sie **Kontingent** aus.
1. Geben Sie die gewünschte neue Größe ein, und wählen Sie dann **OK** aus.

![update-large-file-share-quota.png](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

## <a name="next-steps"></a>Nächste Schritte

* [Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows](storage-how-to-use-files-windows.md)
* [Verwenden des Azure-Dateispeichers unter Linux](../storage-how-to-use-files-linux.md)
* [Einbinden einer Azure-Dateifreigabe über SMB mit macOS](storage-how-to-use-files-mac.md)