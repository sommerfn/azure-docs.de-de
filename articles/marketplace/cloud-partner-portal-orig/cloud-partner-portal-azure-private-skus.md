---
title: Private-SKUs und -Pläne | Azure Marketplace
description: Erfahren Sie, wie Sie private SKUs verwenden, um die Angebotsverfügbarkeit zu verwalten.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: pabutler
ms.openlocfilehash: eb6eac5eafaeea239bfaf9cf2aface3db659dd57
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818834"
---
<a name="private-skus-and-plans"></a>Private SKUs und Pläne
============

Private SKUs ermöglichen das Beschränken der Verfügbarkeit von SKUs auf bestimmte Kunden. Wird eine SKU als privat gekennzeichnet, ist sie nicht in jedem öffentlichen Katalog verfügbar. Dies gilt auch für den [Azure Marketplace](https://azuremarketplace.microsoft.com) und das [Azure-Portal](https://portal.azure.com). Im Azure-Portal wird die SKU nur Kunden mit Zugriff auf die SKU angezeigt. Darüber hinaus werden sie auch darüber informiert, dass sie Zugriff auf private Angebote haben.

>[!NOTE]
>Private SKUs benötigen neue eindeutige SKU-/Tarif-IDs, um mögliche Konflikte mit Ihren öffentlichen SKUs zu vermeiden.

Sie können private SKUs für die folgenden Szenarien verwenden:

1.  Veröffentlichen von Software, die nur für bestimmte Kunden und nicht öffentlich verfügbar sein soll
2.  Veröffentlichen von Varianten öffentlicher Software zu einem benutzerdefinierten Preis für bestimmte Kunden
3.  Veröffentlichen von Varianten öffentlicher Software mit einer benutzerdefinierten Beschreibung und angepassten Nutzungsbedingungen (über ein neues Angebot)

Wenn Sie nur den Preis ändern möchten, können Sie die Datenträger einer anderen SKU in demselben Angebot wiederverwenden. Sie müssen bei privaten SKUs keine Datenträger zwischen den SKUs erneut übermitteln.

<a name="mark-a-sku-private"></a>Kennzeichnen einer SKU als privat
---------------------

Um eine SKU als privat zu markieren, schalten Sie die entsprechende Option um:

![Kennzeichnen einer SKU als privat](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

Sie können die Datenträger aus einer anderen SKU wiederverwenden und den Preis oder die Beschreibung anpassen. Wenn Sie die Datenträger wiederverwenden möchten, beantworten Sie die Frage „Does this SKU re-use images from a public SKU“ (Werden bei dieser SKU Images einer öffentlichen SKU wiederverwendet?) mit **Ja**.

Wenn die SKU als privat gekennzeichnet wurde und das Angebot andere SKUs mit wiederverwendbaren Datenträgern enthält, müssen Sie angeben, dass die SKU Datenträger aus einer anderen SKU wiederverwendet. Sie müssen auch die Zielgruppe für die private SKU angeben.

>[!NOTE]
>Nachdem eine öffentliche SKU veröffentlicht wurde, kann sie nicht mehr in eine private umgewandelt werden.

<a name="select-an-image"></a>Auswählen eines Images
------------------

Sie können neue Datenträger für die private SKU bereitstellen oder Datenträger, die bereits in einer anderen SKU bereitgestellt wurden, erneut verwenden und nur die Preise oder die Beschreibung ändern. Wenn Sie die Datenträger wiederverwenden möchten, beantworten Sie die Frage „Does this SKU re-use images from a public SKU“ (Werden bei dieser SKU Images einer öffentlichen SKU wiederverwendet?) mit **Ja**.

![Angeben der Wiederverwendung von Images](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

Nachdem Sie bestätigt haben, dass die SKU Images erneut verwendet, wählen Sie die Quell-SKU (oder *Basis-SKU*) für die Images aus:

![Auswählen eines Images](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Wenn Sie das Angebot veröffentlichen, werden die Images der ausgewählten SKU unter der privaten SKU-ID mit den benutzerdefinierten Preisen/Nutzungsbedingungen verfügbar gemacht. Die private SKU wird nur für die Zielgruppe sichtbar gemacht.

Bei Imageupdates müssten Sie lediglich das Image der zugrunde liegenden SKU aktualisieren. Das Image der privaten SKU wird im Hintergrund ebenfalls automatisch aktualisiert. Entsprechend wird ein Image, wenn es aus der zugrunde liegenden SKU gelöscht wird, auch aus der privaten SKU entfernt.

<a name="restricting-the-audience"></a>Einschränken der Zielgruppe
------------------------

Private Angebote können nur von Benutzern aus der Zielgruppe gefunden und bereitgestellt werden.
Derzeit wird das Einschränken der Benutzer anhand von Abonnement-IDs unterstützt.

Diese Abonnements können manuell über ein Formular **für bis zu 10 Abonnements** eingegeben oder durch Hochladen einer CSV-Datei **mit bis zu 20.000 Abonnements** angegeben werden.

Manuelles Eingeben der eingeschränkten Zielgruppe:

![Manuelles Einschränken der Zielgruppe](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

Hochladen einer CSV-Datei zum Einschränken der Zielgruppe:

![Verwenden einer CSV-Datei zum Einschränken der Zielgruppe](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Beispielinhalt der CSV-Datei:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Beim Wechsel von der manuellen Eingabe zum Hochladen einer CSV-Datei oder von einer CSV-Datei zur manuellen Eingabe wird die alte Liste mit den Abonnement-IDs mit Zugriff auf die SKU nicht beibehalten. Es wird eine Warnung angezeigt, und die Liste wird erst nach dem Speichern des Angebots überschrieben.

<a name="managing-private-audiences"></a>Verwalten privater Zielgruppen
-------------------------

**Wenn Sie die Zielgruppe aktualisieren möchten, ohne das gesamte Angebot erneut zu veröffentlichen, nehmen Sie die gewünschten Zielgruppenänderungen vor (entweder über die Benutzeroberfläche oder über die API), und initiieren Sie dann die Aktion zum Synchronisieren privater Zielgruppen.**

Wenn Ihre Zielgruppe maximal zehn Abonnements umfasst, können Sie sie vollständig über die CPP-Benutzeroberfläche verwalten.

Umfasst Ihre Zielgruppe mehr als zehn Abonnements, können Sie sie mithilfe einer CSV-Datei verwalten, die Sie entweder über die CPP-Benutzeroberfläche oder über die API hochladen.

Wenn Sie die API verwenden, aber keine CSV-Datei verwalten möchten, können Sie die Zielgruppe wie weiter unten beschrieben auch direkt über die API verwalten.

> [!NOTE]
> Verwenden Sie die Azure-Abonnement-ID (Pläne und SKUs) oder die Mandanten-ID (nur Pläne), um Ihrem privaten Angebot eine Zielgruppe hinzuzufügen.

###  <a name="managing-subscriptions-with-the-api"></a>Verwalten von Abonnements über die API

Über die API können Sie entweder eine CSV-Datei hochladen oder die Zielgruppe direkt (also ohne Verwendung einer CSV-Datei) verwalten. Ganz allgemein müssen Sie lediglich Ihr Angebot abrufen, das Objekt `restrictedAudience` aktualisieren und die Änderungen anschließend wieder an Ihr Angebot übermitteln, um Zielgruppenmitglieder hinzuzufügen oder zu entfernen.

Im Anschluss wird die Vorgehensweise zum programmgesteuerten Aktualisieren Ihrer Zielgruppenliste erläutert:

1. [Rufen Sie Ihre Angebotsdaten ab:](cloud-partner-portal-api-retrieve-specific-offer.md)

    ```
    GET https://cloudpartner.azure.com/api/publishers//offers/?api-version=2017-10-31&includeAllPricing=true
    ```

2. Suchen Sie in jeder SKU des Angebots mithilfe der folgenden JPath-Abfrage nach Objekten vom Typ „restrictedAudience“:

    ```
    $.definition.plans[*].restrictedAudience
    ```
3. Aktualisieren Sie die Objekte vom Typ „restrictedAudience“ für Ihr Angebot.

    **Falls Sie die Abonnementliste für Ihr privates Angebot ursprünglich mithilfe einer CSV-Datei hochgeladen haben:**

    In diesem Fall sehen Ihre Objekte vom Typ *restrictedAudience* wie folgt aus:
    ```
    "restrictedAudience": {
                  "uploadedCsvUri": "{SasUrl}"
    }
    ```

    Gehen Sie für jedes Objekt vom Typ „restrictedAudience“ wie folgt vor:

    a. Laden Sie den Inhalt von `restrictedAudience.uploadedCsvUri` herunter. Dabei handelt es sich um eine CSV-Datei mit Kopfzeilen. Beispiel:

        type,id,description
        subscriptionId,541a269f-3df2-486e-8fe3-c8f9dcf28205,sub1
        subscriptionId,c0da499c-25ec-4e4b-a42a-6e75635253b9,sub2

    b. Fügen Sie in der heruntergeladenen CSV-Datei nach Bedarf Abonnements hinzu, oder löschen Sie sie.

    c. Laden Sie die aktualisierte CSV-Datei in einen Speicherort wie [Azure Blob Storage](../../storage/blobs/storage-blobs-overview.md) oder [OneDrive](https://onedrive.live.com) hoch, und erstellen Sie einen schreibgeschützten Link zu Ihrer Datei. Dies ist Ihre neue *SasUrl*.

    d. Aktualisieren Sie den Schlüssel `restrictedAudience.uploadedCsvUri` mit Ihrer neuen *SasUrl*.

    **Falls Sie die ursprüngliche Abonnementliste für Ihr privates Angebot manuell über das Cloud-Partnerportal eingegeben haben:**

    In diesem Fall sehen Ihre Objekte vom Typ *restrictedAudience* in etwa wie folgt aus:

    ```
    "restrictedAudience": {
        "manualEntries": [{
            "type": "subscriptionId",
            "id": "541a269f-3df2-486e-8fe3-c8f9dcf28205",
            "description": "sub1"
            }, {
            "type": "subscriptionId",
            "id": "c0da499c-25ec-4e4b-a42a-6e75635253b9",
            "description": "sub2"
            }
        ]}
    ```

    a. Fügen Sie für jedes Objekt vom Typ „restrictedAudience“ in der Liste `restrictedAudience.manualEntries` nach Bedarf Einträge hinzu, oder löschen Sie sie.

4. Nachdem Sie alle Objekte vom Typ *restrictedAudience* für jede SKU Ihres privaten Angebots aktualisiert haben, [aktualisieren Sie das Angebot](cloud-partner-portal-api-creating-offer.md):

    ```
    PUT https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31
    ```
    Ihre aktualisierte Zielgruppenliste ist nun wirksam.

<a name="previewing-private-offers"></a>Anzeigen einer Vorschau für private Angebote
-------------------------

Während des Vorschau-/Stagingschritts können nur Abonnements mit einer Berechtigung für die Vorschauversion auf die SKU zugreifen. In dieser Testphase können Sie eine Vorschau des Angebots anzeigen, um zu sehen, wie das Angebot für Ihre Zielkunden dargestellt wird.

Gewähren des Zugriffs auf Angebote im Stagingprozess für Abonnements mit Zugriff auf die Vorschauversion:

![Preview-Abonnement-IDs](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Nachdem das Angebot live geschaltet wurde, können nur Abonnements der eingeschränkten Zielgruppe (die manuell oder per CSV-Datei angegeben wurden) die private SKU anzeigen und bereitstellen. Es wird zu Validierungszwecken für private SKUs empfohlen, stets **Ihre eigenen Abonnements in die eingeschränkte Zielgruppe einzuschließen**.

>[!NOTE]
>Zum Debuggen benötigen auch die Support- und Technikerteams von Microsoft Zugriff auf diese privaten Angebote.
