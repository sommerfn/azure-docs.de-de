---
title: Verwalten von Azure Media Services v3-Konten | Microsoft-Dokumentation
description: Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. In diesem Artikel wird erläutert, wie Azure Media Services v3-Konten verwaltet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: fa9720c2c29af184016d2903e60520e701b4cf79
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670687"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Verwalten von Azure Media Services v3-Konten

Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Weitere Informationen finden Sie unter [Speicherkonten](storage-account-concept.md).

## <a name="moving-a-media-services-account-between-subscriptions"></a>Verschieben eines Media Services-Kontos zwischen Abonnements 

Wenn Sie ein Media Services-Konto in ein neues Abonnement verschieben müssen, müssen Sie zunächst die gesamte Ressourcengruppe, die das Media Services-Konto enthält, in das neue Abonnement verschieben. Sie müssen alle angefügten Ressourcen verschieben: Azure Storage-Konten, Azure CDN-Profile usw. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/resource-group-move-resources.md). Wie bei allen Ressourcen in Azure kann das Verschieben einer Ressourcengruppe einige Zeit in Anspruch nehmen.

> [!NOTE]
> Media Services v3 unterstützt das mehrinstanzenfähige Modell.

### <a name="considerations"></a>Überlegungen

* Erstellen Sie vor der Migration zu einem anderen Abonnement Sicherungen aller Daten in Ihrem Konto.
* Sie müssen die Streamingendpunkte und Livestreamingressourcen beenden. Ihre Benutzer können während der Dauer des Verschiebevorgangs der Ressourcengruppe nicht auf Ihre Inhalte zugreifen. 

> [!IMPORTANT]
> Starten Sie den Streamingendpunkt erst, nachdem der Verschiebevorgang erfolgreich abgeschlossen wurde.

### <a name="troubleshoot"></a>Problembehandlung 

Wenn ein Media Services-Konto oder ein zugehöriges Azure Storage-Konto nach dem Verschieben der Ressourcengruppe „getrennt“ wird, versuchen Sie, die Storage Account-Schlüssel zu rotieren. Wenn das Rotieren der Storage Account-Schlüssel den Status „Getrennt“ des Media Services-Kontos nicht aufhebt, stellen Sie eine neue Supportanfrage über das Menü „Support und Problembehandlung“ im Media Services-Konto.  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Kontos](create-account-cli-quickstart.md)
