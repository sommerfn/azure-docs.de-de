---
title: Verwalten von Speicherkontoeinstellungen im Azure-Portal – Azure Storage | Microsoft-Dokumentation
description: Sie erfahren, wie Sie Speicherkontoeinstellungen im Azure-Portal verwalten, einschließlich der Konfiguration von Einstellungen für die Zugriffssteuerung, der erneuten Generierung von Kontozugriffsschlüsseln, dem Ändern der Zugriffsebene oder dem Ändern des von dem Konto verwendeten Typs der Replikation. Ferner erfahren Sie, wie Sie ein Speicherkonto im Portal löschen.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: 3c01cc870b20c8256b215eb700548e6cd69ad0d5
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748983"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Verwalten von Speicherkontoeinstellungen im Azure-Portal

Eine Reihe verschiedener Einstellungen für Ihr Speicherkonto ist im [Azure-Portal](https://portal.azure.com) verfügbar. In diesem Artikel werden einige dieser Einstellungen und deren Verwendung beschrieben.

## <a name="access-control"></a>Zugriffssteuerung

Azure Storage unterstützt die Autorisierung mit Azure Active Directory für Blob Storage und Queue Storage mittels der rollenbasierten Zugriffssteuerung (RBAC). Weitere Informationen zur Autorisierung mit Azure AD finden Sie unter [Authorize access to Azure blobs and queues using Azure Active Directory (Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory)](storage-auth-aad.md).

Die Einstellungen für die **Zugriffssteuerung** im Azure-Portal bieten eine einfache Möglichkeit zum Zuweisen von RBAC-Rollen zu Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten. Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie unter [Verwalten der Zugriffsrechte für Blob- und Warteschlangendaten mit RBAC](storage-auth-aad-rbac.md).

## <a name="tags"></a>`Tags`

Azure Storage unterstützt Azure Resource Manager-Tags zum Organisieren von Azure-Ressourcen mittels einer benutzerdefinierten Taxonomie. Sie können Tags auf Ihre Speicherkonten anwenden, sodass Sie sie in Ihrem Abonnement logisch gruppieren können.

Bei Speicherkonten ist die Länge eines Tagnamens auf 128 Zeichen beschränkt, und ein Tagwert auf maximal 256 Zeichen.

Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Zugriffsschlüssel

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicherkonto-Zugriffsschlüssel. Mit diesen Schlüsseln können Sie den Zugriff auf Ihr Speicherkonto per gemeinsam verwendetem Schlüssel autorisieren. Sie können die Schlüssel ohne Unterbrechung Ihrer Anwendungen rotieren und erneut generieren, und Microsoft empfiehlt, dies auch regelmäßig zu tun.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

### <a name="view-account-keys-and-connection-string"></a>Anzeigen von Kontoschlüsseln und Verbindungszeichenfolgen

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

Microsoft empfiehlt, dass Sie Ihre Zugriffsschlüssel regelmäßig neu generieren, um dafür zu sorgen, dass Ihr Speicherkonto sicher ist. Es werden zwei Zugriffsschlüssel zugewiesen, sodass Sie Ihre Schlüssel rotieren können. Wenn Sie Ihre Schlüssel rotieren, stellen Sie sicher, dass der Zugriff Ihrer Anwendung auf Azure Storage während des Prozesses erhalten bleibt. 

> [!WARNING]
> Das erneute Generieren Ihrer Zugriffsschlüssel kann sich auf Anwendungen oder Azure-Dienste auswirken, die von dem Speicherkontoschlüssel abhängig sind. Alle Clients, die den Kontoschlüssel verwenden, um auf das Speicherkonto zuzugreifen, müssen aktualisiert werden, damit der neue Schlüssel verwendet wird, einschließlich Media Services, Cloud-, Desktop- und mobiler Anwendungen sowie grafischer Benutzeroberflächenanwendungen für Azure Storage wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

Befolgen Sie diesen Prozess, um Ihre Speicherkontoschlüssel zu rotieren:

1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Anwendungscode, um den sekundären Schlüssel zu verwenden.
2. Generieren Sie den primären Zugriffsschlüssel für das Speicherkonto neu. Klicken Sie im Azure-Portal auf dem Blatt **Zugriffsschlüssel** auf **Schlüssel 1 erneut generieren** und dann auf **Ja**, um das Generieren eines neuen Schlüssels zu bestätigen.
3. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Code, um auf den neuen primären Zugriffsschlüssel zu verweisen.
4. Generieren Sie den sekundären Zugriffsschlüssel auf die gleiche Weise neu.

## <a name="account-configuration"></a>Kontokonfiguration

Nachdem Sie ein Speicherkonto erstellt haben, können Sie dessen Konfiguration ändern. Beispielsweise können Sie ändern, wie Ihre Daten repliziert werden, oder die Zugriffsebene des Kontos von „Heiß“ in „Kalt“ ändern. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto, suchen und klicken Sie dann unter **Einstellungen** auf **Konfiguration**, um die Kontokonfiguration anzuzeigen oder zu ändern.

Das Ändern der Speicherkontokonfiguration kann zusätzliche Kosten verursachen. Weitere Informationen finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="delete-a-storage-account"></a>Löschen von Speicherkonten

Um ein Speicherkonto zu entfernen, das Sie nicht mehr verwenden, wechseln Sie im [Azure-Portal](https://portal.azure.com)zum entsprechenden Speicherkonto, und klicken Sie auf **Löschen**. Durch das Löschen eines Speicherkontos wird das gesamte Konto gelöscht, einschließlich aller Daten im Konto.

> [!WARNING]
> Es ist nicht möglich, ein gelöschtes Speicherkonto wiederherzustellen oder Inhalte abzurufen, die das Konto vor dem Löschen enthielt. Sichern Sie alle Inhalte, die Sie speichern möchten, bevor Sie das Konto löschen. Dies gilt auch für alle Ressourcen im Konto – gelöschte Blobs, Tabellen, Warteschlangen oder Dateien können nicht wiederhergestellt werden.
> 

Wenn Sie versuchen, ein Speicherkonto zu löschen, das einem virtuellen Azure-Computer zugewiesen ist, wird unter Umständen ein Fehler mit dem Hinweis angezeigt, dass das Speicherkonto noch verwendet wird. Hilfe zum Beheben dieses Fehlers finden Sie unter [Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md) (Beheben von Fehlern beim Löschen von Speicherkonten, -containern oder -VHDs in Azure).

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Storage-Konten](storage-account-overview.md)
- [Erstellen eines Speicherkontos](storage-quickstart-create-account.md)
