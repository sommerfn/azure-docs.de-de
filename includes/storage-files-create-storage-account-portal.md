---
title: storage-files-create-storage-account-portal
description: Erstellen Sie ein Speicherkonto für Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: a09e9206b3ecd23a6208572a88c16b149033f123
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "68699442"
---
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, in dem Sie eine Azure-Dateifreigabe oder andere Speicherressourcen wie Blobs oder Warteschlangen bereitstellen können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Freigaben enthalten. Auf einer Freigabe kann eine unbegrenzte Anzahl von Dateien gespeichert werden, bis die Kapazitätsgrenzen des Speicherkontos erreicht sind.

So erstellen Sie ein Speicherkonto:

1. Klicken Sie im Menü auf der linken Seite auf **+** , um eine Ressource zu erstellen.
2. Geben Sie im Suchfeld den Suchbegriff **Speicherkonto** ein, klicken Sie auf **Speicherkonto – Blob, Datei, Tabelle, Warteschlange**, und klicken Sie anschließend auf **Erstellen**.
    ![Screenshot: Speicherkontoeintrag im Dialogfeld für die Ressourcensuche](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. Geben Sie unter **Name** den Namen *mystorageacct* sowie einige zufällig gewählte Zahlen ein, bis durch ein grünes Häkchen signalisiert wird, dass der Name eindeutig ist. Der Name des Speicherkontos darf nur Kleinbuchstaben enthalten und muss global eindeutig sein. Notieren Sie sich den Speicherkontonamen. Er wird später noch benötigt. 
4. Übernehmen Sie unter **Bereitstellungsmodell** den Standardwert **Resource Manager**. Weitere Informationen zu den Unterschieden zwischen der Azure Resource Manager-Bereitstellung und dem klassischen Bereitstellungsmodell finden Sie unter [Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. Wählen Sie unter **Kontoart** die Option **StorageV2**. Weitere Informationen zu den unterschiedlichen Arten von Speicherkonten finden Sie unter [Optionen für Azure Storage-Konten](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. Übernehmen Sie unter **Leistung** den Standardwert **Standardspeicher**. Für Azure Files wird derzeit nur Standardspeicher unterstützt. Ihre Dateifreigabe wird also auch dann im Standardspeicher gespeichert, wenn Sie Azure Storage Premium auswählen.
7. Wählen Sie unter **Replikation** die Option **Lokal redundanter Speicher (LRS)** aus. 
8. Es empfiehlt sich, unter **Sichere Übertragung erforderlich** immer die Option **Aktiviert** auszuwählen. Weitere Informationen zu dieser Option finden Sie unter [Vorschreiben einer sicheren Übertragung in Azure Storage](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. Wählen Sie unter **Abonnement** das Abonnement aus, mit dem das Speicherkonto erstellt wurde. Falls Sie nur über ein einzelnes Abonnement verfügen, sollte es das Standardabonnement sein.
10. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus. Geben Sie als Name *myResourceGroup* ein.
11. Wählen Sie unter **Standort** die Option **USA, Osten**.
12. Übernehmen Sie unter **Virtuelle Netzwerke** die Standardoption **Deaktiviert**. 
13. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, um das Speicherkonto leichter zu finden.
14. Klicken Sie abschließend auf **Erstellen**, um die Bereitstellung zu starten.