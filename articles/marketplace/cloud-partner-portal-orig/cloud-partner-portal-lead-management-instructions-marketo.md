---
title: Konfigurieren der Leadverwaltung in Marketo | Azure Marketplace
description: Konfigurieren der Leadverwaltung in Marketo für Azure Marketplace-Kunden.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: 7949507c8c7ef57cded25cde8579c1945aa93a81
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825215"
---
# <a name="configure-lead-management-in-marketo"></a>Konfigurieren der Leadverwaltung in Marketo

Dieser Artikel beschreibt, wie Sie Marketo zum Verwenden von Microsoft Sales Leads einrichten.

1. Melden Sie sich bei Marketo an.
2. Wählen Sie **Design Studio** (Designstudio) aus.
    ![„Design Studio“ in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Wählen Sie **New Form** (Neues Formular) aus.
    ![Neues Formular in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Füllen Sie im Fenster „New Form“ (Neues Formular) die Pflichtfelder aus, und wählen Sie **Erstellen** aus.
    ![Erstellen eines neuen Formulars in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Wählen Sie im Abschnitt „Field Details“ (Felddetails) **Fertig stellen** aus.
    ![Fertigstellen eines Formulars in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Bestätigen Sie die Eingaben, und schließen Sie sie.

6.  Wählen Sie auf der Registerkarte „MarketplaceLeadBacked“ **Embed Code** (Code einbetten) aus.
    ![Option zum Einbetten von Code in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Der Marketo-Einbettungscode wird etwa folgendermaßen angezeigt:

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Kopieren Sie die angezeigten Werte, um im Cloud-Partnerportal die **Server-ID**, **Munchkin-ID** und **Formular-ID** in den Marketo-Feldern zu konfigurieren.

Das nächste Beispiel dient als Leitfaden zum Abrufen der benötigten IDs aus dem Marketo-Einbettungscode.

- Server-ID: **ys12**
- Munchkin-ID: **123-PQR-789**
- Formular-ID: **1179**\
