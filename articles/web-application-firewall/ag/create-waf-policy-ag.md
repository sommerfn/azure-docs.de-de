---
title: Erstellen von Web Application Firewall-Richtlinien (WAF) für Application Gateway
description: Erfahren Sie, wie Sie Web Application Firewall-Richtlinien für Application Gateway erstellen.
services: web-application-firewall
ms.topic: conceptual
author: vhorne
ms.service: web-application-firewall
ms.date: 10/26/2019
ms.author: victorh
ms.openlocfilehash: 31a5ad92942b40e42b66e404df2d09cd8158f7a2
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73606485"
---
# <a name="create-web-application-firewall-policies-for-application-gateway"></a>Erstellen von Web Application Firewall-Richtlinien für Application Gateway

Das Zuordnen einer WAF-Richtlinie zu Listenern ermöglicht den Schutz mehrerer Websites hinter einer einzelnen WAF durch unterschiedliche Richtlinien. Wenn sich beispielsweise hinter Ihrer WAF fünf Websites befinden, können Sie fünf einzelne WAF-Richtlinien (eine für jeden Listener) verwenden, um Ausschlüsse, benutzerdefinierte Regeln und verwaltete Regelsätze für jeweils eine Website anzupassen, ohne dass die anderen vier Websites davon beeinträchtigt werden. Wenn Sie eine einzelne Richtlinie auf alle Standorte anwenden möchten, weisen Sie diese Richtlinie einfach Application Gateway anstelle der einzelnen Listener zu, um sie global anzuwenden. Richtlinien können auch auf eine pfadbasierte Routingregel angewandt werden. 

   > [!NOTE]
   > Die WAF-Richtlinie für einzelne Websites ist in den Regionen „USA, Süden-Mitte“ und „Europa, Norden“ verfügbar. Wenn Sie im Portal darauf zugreifen möchten, verwenden Sie bis zur Liveschaltung für alle Benutzer [diesen Link](https://aka.ms/AppgwwafWithAllFeatureFlags).  

Sie können beliebig viele Richtlinien erstellen. Nachdem Sie eine Richtlinie erstellt haben, muss sie einer Application Gateway-Instanz zugeordnet werden, damit sie wirksam wird. Sie kann jedoch auch einer beliebigen Kombination aus Application Gateway-Instanzen und Listenern zugeordnet werden. 

Wenn auf Ihre Application Gateway-Instanz eine Richtlinie angewandt wird und Sie dann eine andere Richtlinie auf einen Listener in dieser Application Gateway-Instanz anwenden, gilt die Richtlinie des Listeners – aber nur für die Listener, denen sie zugewiesen ist. Die Application Gateway-Richtlinie gilt weiterhin für alle anderen Listener, denen keine spezielle Richtlinie zugewiesen ist. 

   > [!NOTE]
   > WAF-Richtlinien für einzelne Websites und einzelne URIs sind als Public Preview verfügbar. Das bedeutet, dass diese Funktion den zusätzlichen Nutzungsbedingungen von Microsoft unterliegt. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Alle neuen WAF-Einstellungen (benutzerdefinierte Regeln, verwaltete Regelsatzkonfigurationen, Ausschlüsse usw.) sind in einer WAF-Richtlinie enthalten. Wenn Sie bereits über eine WAF verfügen, sind diese Einstellungen möglicherweise weiterhin in Ihrer WAF-Konfiguration vorhanden. Schritte für den Umstieg auf die neue WAF-Richtlinie finden Sie unter [Migrieren Ihrer WAF-Konfiguration zu einer WAF-Richtlinie](#migrate) weiter unten in diesem Artikel. 

## <a name="create-a-policy"></a>Erstellen einer Richtlinie

Erstellen Sie zuerst im Azure-Portal mithilfe des verwalteten Standardregelsatzes eine einfache WAF-Richtlinie.

1. Wählen Sie links oben im Portal die Option **Ressource erstellen** aus. Suchen Sie nach **WAF**, wählen Sie **Web Application Firewall** aus, und wählen Sie dann **Erstellen** aus.
2. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und wählen Sie **Überprüfen + erstellen** aus:

   |Einstellung  |Wert  |
   |---------|---------|
   |Richtlinie für     |Regionale WAF (Application Gateway)|
   |Subscription     |Wählen Sie Ihren Abonnementnamen aus.|
   |Resource group     |Ressourcengruppe auswählen|
   |Richtlinienname     |Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|
3. Geben Sie auf der Registerkarte **Zuordnung** eine der folgenden Einstellungen ein, und wählen Sie dann**Hinzufügen** aus:

   |Einstellung  |Wert  |
   |---------|---------|
   |Application Gateway zuordnen     |Wählen Sie Ihren Application Gateway-Profilnamen aus.|
   |Listener zuordnen     |Wählen Sie den Namen Ihres Application Gateway-Listeners und dann **Hinzufügen** aus.|

   > [!NOTE]
   > Wenn Sie Ihrer Application Gateway-Instanz (oder Ihrem Listener) eine Richtlinie zuweisen, jedoch gilt dafür bereits eine Richtlinie, wird die ursprüngliche Richtlinie überschrieben und durch die neue Richtlinie ersetzt.
4. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**.

   ![Grundlegendes zu WAF-Richtlinien](../media/create-waf-policy-ag/waf-policy-basics.png)

## <a name="configure-waf-rules-optional"></a>Konfigurieren von WAF-Regeln (optional)

Wenn Sie eine WAF-Richtlinie erstellen, erfolgt dies standardmäßig im Modus *Erkennung*. Im Erkennungsmodus blockiert WAF keine Anforderungen. Stattdessen werden die übereinstimmenden WAF-Regeln in den WAF-Protokollen aufgezeichnet. Um WAF in Aktion zu sehen, können Sie die Einstellung für den Modus in *Prävention* ändern. Im Präventionsmodus erfolgt bei Übereinstimmung mit Regeln, die in dem ausgewählten CRS-Regelsatz definiert sind, eine Blockierung oder Aufzeichnung in den WAF-Protokollen.

## <a name="managed-rules"></a>Verwaltete Regeln

Von Azure verwaltete OWASP-Regeln sind standardmäßig aktiviert. Um eine einzelne Regel innerhalb einer Regelgruppe zu deaktivieren, erweitern Sie die Regeln innerhalb dieser Regelgruppe. Aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie oben auf der Registerkarte **Deaktivieren** aus.

![Verwaltete Regeln](../media/create-waf-policy-ag/managed-rules.png)

## <a name="custom-rules"></a>Benutzerdefinierte Regeln

Sie können eine benutzerdefinierte Regel erstellen, indem Sie auf der Registerkarte **Benutzerdefinierte Regeln** die Option **Benutzerdefinierte Regel hinzufügen** auswählen. Die Seite für die Konfiguration einer benutzerdefinierten Regel wird geöffnet. Der folgende Screenshot zeigt ein Beispiel für eine benutzerdefinierte Regel, nach deren Konfiguration eine Anforderung blockiert wird, wenn die Abfragezeichenfolge den Text *blockme* enthält.

![Bearbeiten benutzerdefinierter Regeln](../media/create-waf-policy-ag/edit-custom-rule.png)

## <a name="migrate"></a>Migrieren Ihrer WAF-Konfiguration zu einer WAF-Richtlinie

Wenn Sie bereits über eine WAF verfügen, sind Ihnen möglicherweise einige Änderungen im Portal aufgefallen. Zunächst müssen Sie ermitteln, welche Art von Richtlinie Sie in der WAF aktiviert haben. Es gibt drei mögliche Zustände:

- Keine WAF-Richtlinie
- Richtlinie nur mit benutzerdefinierten Regeln
- WAF-Richtlinie

Sie können den Zustand der WAF im Portal erkennen. Wenn die WAF-Einstellungen sichtbar sind und in der Application Gateway-Ansicht geändert werden können, befindet sich die WAF im Zustand 1.

![WAF-Konfiguration](../media/create-waf-policy-ag/waf-configure.png)

Wenn Sie **Web Application Firewall** auswählen und eine zugeordnete Richtlinie angezeigt wird, befindet sich die WAF in einem der Zustände 2 oder 3. Wenn nach dem Navigieren zur Richtlinie **nur** benutzerdefinierte Regeln und zugeordnete Application Gateway-Instanzen angezeigt werden, handelt es sich um eine Richtlinie mit ausschließlich benutzerdefinierten Regeln.

![Benutzerdefinierte WAF-Regeln](../media/create-waf-policy-ag/waf-custom-rules.png)

Wenn außerdem Richtlinieneinstellungen und verwaltete Regeln angezeigt werden, handelt es sich um eine vollwertige Web Application Firewall-Richtlinie. 

![WAF-Richtlinieneinstellungen](../media/create-waf-policy-ag/waf-policy-settings.png)

## <a name="migrate-to-waf-policy"></a>Migrieren zu WAF-Richtlinien

Wenn Sie über eine WAF-Richtlinie nur mit benutzerdefinierten Regeln verfügen, sollten Sie erwägen, zur neuen WAF-Richtlinie zu wechseln. In Zukunft unterstützt die Firewallrichtlinie WAF-Richtlinieneinstellungen, verwaltete Regelsätze, Ausschlüsse und deaktivierte Regelgruppen. Im Wesentlichen werden alle WAF-Konfigurationen, die zuvor in Application Gateway vorgenommen wurden, jetzt über die WAF-Richtlinie durchgeführt. 

Bearbeitungen der WAF-Richtlinie nur mit benutzerdefinierten Regeln sind deaktiviert. Um WAF-Einstellungen wie das Deaktivieren von Regeln, das Hinzufügen von Ausschlüssen usw. zu bearbeiten, müssen Sie zu einer neuen Firewallrichtlinienressource der obersten Ebene migrieren.

Erstellen Sie dazu eine *Web Application Firewall-Richtlinie*, und ordnen Sie sie Ihren gewünschten Application Gateway-Instanzen und Listenern zu. Diese neue Richtlinie **muss** identisch mit der aktuellen WAF-Konfiguration sein. Das bedeutet, dass Sie jede benutzerdefinierte Regel, jeden Ausschluss, jede deaktivierte Regel usw. in die neu zu erstellende Richtlinie kopieren müssen. Wenn Sie Ihrer Application Gateway-Instanz eine Richtlinie zugeordnet haben, können Sie weiterhin Änderungen an den WAF-Regeln und -Einstellungen vornehmen. 

Sie können zu diesem Zweck auch Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verknüpfen einer WAF-Richtlinie mit einer vorhandenen Application Gateway-Instanz](associate-waf-policy-existing-gateway.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [CRS-Regelgruppen und -Regeln von Web Application Firewall](application-gateway-crs-rulegroups-rules.md).
