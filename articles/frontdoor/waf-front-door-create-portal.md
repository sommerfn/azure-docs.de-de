---
title: Erstellen einer Web Application Firewall-Richtlinie für Azure Front Door im Azure-Portal
titlesuffix: Azure web application firewall
description: Erfahren Sie, wie Sie im Azure-Portal eine Web Application Firewall-Richtlinie (WAF) erstellen.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: abaef0fb521d848134885a06591b0656c60c67e6
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846391"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Erstellen einer WAF-Richtlinie für Azure Front Door im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie eine einfache Azure Web Application Firewall-Richtlinie (WAF) erstellen und mithilfe von Azure Front Door auf einen Front-End-Host anwenden.

## <a name="prerequisites"></a>Voraussetzungen

Gehen Sie zum Erstellen eines Front Door-Profils gemäß den Anweisungen unter [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](quickstart-create-front-door.md) vor. 

## <a name="create-a-waf-policy"></a>Erstellen einer WAF-Richtlinie

Erstellen Sie zuerst im Portal mithilfe des verwalteten Standardregelsatzes eine einfache WAF-Richtlinie. 

1. Klicken Sie links auf dem Bildschirm auf **Ressource erstellen**. Suchen Sie nach **WAF**, wählen Sie **Web Application Firewall (Vorschau)** aus, und klicken Sie auf **Erstellen**.
2. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription            |Wählen Sie den Namen Ihres Front Door-Abonnements aus.|
    | Resource group          |Wählen Sie den Namen Ihrer Front Door-Ressourcengruppe aus.|
    | Richtlinienname             |Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|

   ![Erstellen einer WAF-Richtlinie](./media/waf-front-door-create-portal/basic.png)

3. Klicken Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Zuordnung** auf **Front-End-Host hinzufügen**. Geben Sie die folgenden Einstellungen ein, und klicken Sie dann auf **Hinzufügen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Front Door              | Wählen Sie Ihren Front Door-Profilnamen aus.|
    | Front-End-Host           | Wählen Sie den Namen des Front Door-Hosts aus, und klicken Sie dann auf **Hinzufügen**.|
    
    > [!NOTE]
    > Wenn der Front-End-Host einer WAF-Richtlinie zugeordnet ist, wird er abgeblendet angezeigt. Sie müssen zuerst den Front-End-Host aus der zugehörigen Richtlinie entfernen und ihn dann wieder einer neuen WAF-Richtlinie zuordnen.
1. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**.

## <a name="configure-waf-rules-optional"></a>Konfigurieren von WAF-Regeln (optional)

### <a name="change-mode"></a>Ändern des Modus

Wenn Sie eine WAF-Richtlinie erstellen, wird diese standardmäßig im Modus **Erkennung** betrieben. Im Modus **Erkennung** blockiert WAF keine Anforderungen. Stattdessen werden Anforderungen, die den WAF-Regeln entsprechen, in WAF-Protokollen aufgezeichnet.
Um WAF in Aktion zu sehen, können Sie den Modus von **Erkennung** in **Prävention** ändern. Im Modus **Prävention** werden Anforderungen, die mit Regeln übereinstimmen, die im Standardregelsatz definiert sind, blockiert und in WAF-Protokollen aufgezeichnet.

 ![Ändern des WAF-Richtlinienmodus](./media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Benutzerdefinierte Regeln

Sie können eine benutzerdefinierte Regel erstellen, indem Sie im Abschnitt **Benutzerdefinierte Regeln** die Option **Benutzerdefinierte Regel hinzufügen** wählen. Die Seite für die Konfiguration einer benutzerdefinierten Regel wird geöffnet. Unten ist ein Beispiel für die Konfiguration einer benutzerdefinierten Regel zum Blockieren einer Anforderung für den Fall angegeben, in dem die Abfragezeichenfolge **blockme** enthält.

![Ändern des WAF-Richtlinienmodus](./media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Standardregelsatz

Der von Azure verwaltete Standardregelsatz ist standardmäßig aktiviert. Um eine einzelne Regel innerhalb einer Regelgruppe zu deaktivieren, erweitern Sie die Regeln innerhalb dieser Regelgruppe. Aktivieren Sie das **Kontrollkästchen** vor der Regelnummer, und klicken Sie oben auf der Registerkarte auf **Deaktivieren**. Um Aktionstypen für einzelne Regeln innerhalb des Regelsatzes zu ändern, aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie dann oben die Registerkarte **Aktion ändern** aus.

 ![Ändern des WAF-Regelsatzes](./media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure Web Application Firewall finden Sie [hier](waf-overview.md).
- Erfahren Sie mehr zu [Azure Front Door](front-door-overview.md).
