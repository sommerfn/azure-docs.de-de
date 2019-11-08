---
title: SaaS Fulfillment APIs | Azure Marketplace
description: Vorgestellt werden die Versionen der Fulfillment-APIs, mit denen Sie Ihre SaaS-Angebote in den Azure Marketplace integrieren können.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: ebfc278d09c244970df5807df1505295fe7016c4
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819118"
---
# <a name="saas-fulfillment-apis"></a>SaaS-Fulfillment-APIs

Mit den SaaS-Fulfillment-APIs können unabhängige Softwarehersteller (ISVs) ihre SaaS-Anwendungen in den Azure Marketplace integrieren. Mit diesen APIs können ISVs für ihre Anwendungen alle Handelskanäle nutzen: direkt, über Partner (Reseller) und über den Vertrieb.  Sie sind eine Voraussetzung für die Auflistung transaktionsfähiger SaaS-Angebote im Azure Marketplace.

> [!WARNING]
> Die aktuelle Version dieser API ist Version 2, die für alle neuen SaaS-Angebote verwendet werden sollte.  Version 1 der API ist veraltet und wird zur Unterstützung vorhandener Angebote noch gepflegt.


## <a name="business-model-support"></a>Geschäftsmodellunterstützung

Diese API unterstützt die folgenden Geschäftsmodellfunktionen für folgende Aufgaben:

* Definieren mehrerer Pläne für ein Angebot. Diese Pläne haben unterschiedliche Funktionen und werden möglicherweise unterschiedlich berechnet.
* Bereitstellen eines Angebots auf Website- oder Benutzer-Abrechnungsbasis.
* Bereitstellen monatlicher und jährlicher (Vorauszahlung) Abrechnungsoptionen.
* Bereitstellen eines privaten Preismodells für einen Kunden auf der Grundlage einer ausgehandelten Geschäftsvereinbarung.


## <a name="next-steps"></a>Nächste Schritte

Wenn nicht bereits geschehen, registrieren Sie Ihre SaaS-Anwendung im [Azure-Portal](https://ms.portal.azure.com) wie unter [Registrieren einer Azure AD-Anwendung](./pc-saas-registration.md) erklärt.  Verwenden Sie anschließend für die Entwicklung die aktuelle Version dieser Schnittstelle: [SaaS Fulfillment API Version 2](./pc-saas-fulfillment-api-v2.md).
