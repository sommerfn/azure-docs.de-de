---
title: 'Tutorial: Erstellen eines Aliaseintrags zum Verweisen auf einen Ressourceneintrag in einer Zone'
titleSuffix: Azure DNS
description: In diesem Tutorial erfahren Sie, wie Sie einen Azure DNS-Aliaseintrag konfigurieren, um auf einen Ressourceneintrag in der Zone zu verweisen.
services: dns
author: asudbring
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: allensu
ms.openlocfilehash: 59ffe9781d97880044da5eedbdf84181bf1b2fa1
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082891"
---
# <a name="tutorial-create-an-alias-record-to-refer-to-a-zone-resource-record"></a>Tutorial: Erstellen eines Aliaseintrags, um auf einen Zonenressourceneintrag zu verweisen

Aliaseinträge können auf andere Eintragssätze desselben Typs verweisen. So kann beispielsweise ein DNS-CNAME-Eintragssatz ein Alias für einen anderen CNAME-Eintragsatz desselben Typs sein. Dies ist hilfreich, wenn Sie möchten, dass einige Eintragssätze in Bezug auf ihr Verhalten Aliase und andere keine Aliase sind.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Aliaseintrags für einen Ressourceneintrag in der Zone
> * Testen des Aliaseintrags


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über einen Domänennamen verfügen, den Sie zum Testen in Azure DNS hosten können. Sie müssen uneingeschränkte Kontrolle über diese Domäne haben. Das bedeutet, Sie müssen unter anderem die Namenservereinträge für die Domäne festlegen können.

Eine Anleitung zum Hosten Ihrer Domäne in Azure DNS finden Sie unter [Tutorial: Hosten Ihrer Domäne in Azure DNS](dns-delegate-domain-azure-dns.md).


## <a name="create-an-alias-record"></a>Erstellen eines Aliaseintrags

Erstellen eines Aliaseintrags, der auf einen Ressourceneintrag in der Zone verweist

### <a name="create-the-target-resource-record"></a>Erstellen des Zielressourceneintrags
1. Wählen Sie Ihre Azure DNS-Zone aus, um sie zu öffnen.
2. Wählen Sie **Ressourceneintragssatz**.
3. Geben Sie im Textfeld **Name** den Namen **Server** ein.
4. Wählen Sie unter **Typ** die Option**A** aus.
5. Geben Sie im Textfeld **IP-ADRESSE** die Adresse **10.10.10.10** ein.
6. Klicken Sie auf **OK**.

### <a name="create-the-alias-record"></a>Erstellen des Aliaseintrags
1. Wählen Sie Ihre Azure DNS-Zone aus, um sie zu öffnen.
2. Wählen Sie **Ressourceneintragssatz**.
3. Geben Sie im Textfeld **Name** den Namen **Test** ein.
4. Wählen Sie unter **Typ** die Option**A** aus.
5. Wählen Sie für das Kontrollkästchen **Alias Record Set** (Aliaseintragssatz) die Option **Ja**. Aktivieren Sie anschließend die Option **Zone record set** (Zoneneintragssatz).
6. Wählen Sie unter **Zone record set** (Zoneneintragssatz) den Eintrag **Server** aus.
7. Klicken Sie auf **OK**.

## <a name="test-the-alias-record"></a>Testen des Aliaseintrags

1. Starten Sie Ihr bevorzugtes nslookup-Tool. Eine Möglichkeit besteht darin, zu [https://network-tools.com/nslook](https://network-tools.com/nslook) zu navigieren.
2. Legen Sie den Abfragetyp für A-Einträge fest, und suchen Sie **test.\<Ihr Domänenname\>** . Die Antwort ist **10.10.10.10**.
3. Ändern Sie im Azure-Portal den A-Eintrag **Server** in **10.11.11.11**.
4. Warten Sie einige Minuten, und verwenden Sie dann nslookup für den Eintrag **Test**. Die Antwort ist **10.11.11.11**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die für dieses Tutorial erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourceneinträge **Server** und **Test** in Ihrer Zone.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Aliaseintrag zum Verweisen auf einen Ressourceneintrag in der Zone erstellt. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
