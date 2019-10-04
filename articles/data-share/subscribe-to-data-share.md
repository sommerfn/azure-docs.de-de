---
title: 'Tutorial: Akzeptieren und Empfangen von Daten per Azure Data Share (Vorschauversion)'
description: 'Tutorial: Akzeptieren und Empfangen von Daten per Azure Data Share (Vorschauversion)'
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: fc63c1a0b3b496de8e5ecea58f79f1db9d872e80
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838439"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Tutorial: Akzeptieren und Empfangen von Daten per Azure Data Share (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie mit Azure Data Share (Vorschauversion) eine Einladung für eine Datenfreigabe annehmen. Sie erfahren, wie Sie die für Sie freigegebenen Daten empfangen und ein Intervall für die regelmäßige Aktualisierung einrichten. So können Sie sicherstellen, dass Sie immer über die aktuellste Momentaufnahme der Daten verfügen, die für Sie freigegeben werden. 

> [!div class="checklist"]
> * Annehmen einer Azure Data Share-Einladung (Vorschauversion)
> * Erstellen eines Azure Data Share-Kontos (Vorschauversion)
> * Angeben eines Ziels für Ihre Daten
> * Erstellen eines Abonnements Ihrer Datenfreigabe für die geplante Aktualisierung

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Einladung zu einer Datenfreigabe annehmen können, müssen Sie einige Azure-Ressourcen bereitstellen. Diese sind unten aufgeführt. 

Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind, bevor Sie die Einladung zu einer Datenfreigabe annehmen. 

* Azure-Abonnement: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Ein Azure Storage-Konto: Falls Sie noch nicht über ein Konto verfügen, können Sie [hier ein Azure Storage-Konto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Eine Data Share-Einladung: Eine Einladung von Microsoft Azure mit dem Betreff „Azure Data Share-Einladung von **<yourdataprovider@domain.com>** “.
* Berechtigung zum Hinzufügen einer Rollenzuweisung zum Speicherkonto (in der Berechtigung *Microsoft.Authorization/role assignments/write* enthalten). Diese Berechtigung ist in der Rolle „Besitzer“ vorhanden. 
* Ressourcenanbieterregistrierung für Microsoft.DataShare Informationen zur Vorgehensweise finden Sie in der Dokumentation zu [Azure-Ressourcenanbietern](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

> [!IMPORTANT]
> Zum Annehmen und Empfangen einer Azure Data Share-Einladung müssen Sie zuerst den Microsoft.DataShare-Ressourcenanbieter registrieren, und Sie müssen ein Besitzer des Speicherkontos sein, für das Sie die Daten akzeptieren. Befolgen Sie die Anleitung unter [Troubleshoot common issues in Azure Data Share Preview](data-share-troubleshoot.md) (Behandeln von allgemeinen Problemen in Azure Data Share (Vorschauversion)), um den Ressourcenanbieter der Datenfreigabe zu registrieren und sich selbst als Besitzer des Speicherkontos hinzuzufügen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="open-invitation"></a>Öffnen der Einladung

Überprüfen Sie Ihren Posteingang auf eine Einladung von Ihrem Datenanbieter. Die Einladung stammt von Microsoft Azure und hat die Bezeichnung **Azure Data Share-Einladung von <yourdataprovider@domain.com>** . Notieren Sie sich den Namen der Freigabe. So können Sie sicherstellen, dass Sie die richtige Freigabe akzeptieren, falls mehrere Einladungen vorhanden sind. 

Wählen Sie **Einladung anzeigen**, um Ihre Einladung in Azure anzuzeigen. Sie gelangen zur Ansicht „Empfangene Freigaben“.

![Einladungen](./media/invitations.png "Liste mit Einladungen") 

Wählen Sie die Freigabe aus, die Sie anzeigen möchten. 

## <a name="accept-invitation"></a>Annehmen der Einladung
Stellen Sie sicher, dass Sie alle Felder überprüfen – auch die **Nutzungsbedingungen**. Wenn Sie den Nutzungsbedingungen zustimmen, müssen Sie das entsprechende Kontrollkästchen aktivieren, um dies anzugeben. 

![Nutzungsbedingungen](./media/terms-of-use.png "Nutzungsbedingungen") 

Wählen Sie unter *Target Data Share Account* (Data Share-Zielkonto) das Abonnement und die Ressourcengruppe für die Bereitstellung Ihrer Data Share-Instanz aus. 

Wählen Sie für das Feld **Data Share Account** (Data Share-Konto) die Option **Neue erstellen**, falls Sie nicht über ein vorhandenes Data Share-Konto verfügen. Wählen Sie andernfalls ein vorhandenes Data Share-Konto für Ihre Datenfreigabe aus. 

Für das Feld *Received Share Name* (Name der empfangenen Freigabe) können Sie den vom Datenanbieter angegebenen Standardnamen übernehmen oder einen neuen Namen für die empfangene Freigabe angeben. 

![Data Share-Zielkonto](./media/target-data-share.png "Data Share-Zielkonto") 

Nachdem Sie den Nutzungsbedingungen zugestimmt und einen Speicherort für Ihre Freigabe angegeben haben, wählen Sie *Accept and Configure* (Akzeptieren und konfigurieren). Wenn Sie diese Option wählen, wird ein Freigabeabonnement erstellt. Auf dem nächsten Bildschirm werden Sie aufgefordert, ein Zielspeicherkonto auszuwählen, in das Ihre Daten kopiert werden sollen. 

![Optionen für „Akzeptieren“](./media/accept-options.png "Optionen für „Akzeptieren“") 

Wenn Sie die Einladung jetzt annehmen, den Speicher aber erst später konfigurieren möchten, wählen Sie *Accept and Configure later* (Akzeptieren und später konfigurieren). Bei dieser Option können Sie Ihr Zielspeicherkonto später konfigurieren. Informationen zum Konfigurieren Ihres Speichers zu einem späteren Zeitpunkt finden Sie auf der Seite zum Thema [Konfigurieren Ihres Speicherkontos](how-to-configure-mapping.md). Sie enthält die ausführlichen Schritte zur Konfiguration der Datenfreigabe. 

Wählen Sie *Ablehnen*, falls Sie die Einladung nicht annehmen möchten. 

## <a name="configure-storage"></a>Konfigurieren des Speichers
Wählen Sie unter *Target Storage Settings* (Einstellungen für Zielspeicher) das Abonnement, die Ressourcengruppe und das Speicherkonto für den Empfang der Daten aus. 

![Einstellungen für Zielspeicher](./media/target-storage-settings.png "Zielspeicher") 

Stellen Sie sicher, dass Sie die Einstellungen für Momentaufnahmen aktivieren, um regelmäßige Aktualisierungen Ihrer Daten zu erhalten. Beachten Sie, dass nur dann ein Zeitplan für die Momentaufnahmeneinstellungen angezeigt wird, wenn Ihr Datenanbieter diesen in die Datenfreigabe einbezogen hat. 

![Einstellungen für Momentaufnahmen](./media/snapshot-settings.png "Einstellungen für Momentaufnahmen") 

Wählen Sie *Speichern* aus. 

## <a name="trigger-a-snapshot"></a>Auslösen einer Momentaufnahme

Sie können eine Momentaufnahme auf der Registerkarte „Empfangene Freigaben“ > „Details“ auslösen, indem Sie **Trigger snapshot** (Momentaufnahme auslösen) wählen. Hier können Sie eine vollständige oder inkrementelle Momentaufnahme Ihrer Daten auslösen. Wählen Sie die Option für das vollständige Kopieren, falls Sie zum ersten Mal Daten von Ihrem Datenanbieter erhalten. 

![Momentaufnahme auslösen](./media/trigger-snapshot.png "Momentaufnahme auslösen") 

Wenn der Status der letzten Ausführung *Erfolgreich* lautet, können Sie das Speicherkonto öffnen, um die empfangenen Daten anzuzeigen. 

Wählen Sie **Datasets**, um zu überprüfen, welches Speicherkonto Sie verwendet haben. 

![Consumerdataset](./media/consumer-datasets.png "Consumerdataset-Zuordnung") 

## <a name="view-history"></a>Anzeigen des Verlaufs
Navigieren Sie zum Anzeigen eines Verlaufs Ihrer Momentaufnahmen zu „Empfangene Freigaben“ > „Verlauf“. Hier ist ein Verlauf aller Momentaufnahmen angegeben, die in den letzten 60 Tagen generiert wurden. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie erfahren, wie Sie eine Azure Data Share-Instanz akzeptieren und empfangen. Weitere Informationen zu den Konzepten von Azure Data Share finden Sie unter [Konzepte: Azure Data Share-Terminologie](terminology.md).