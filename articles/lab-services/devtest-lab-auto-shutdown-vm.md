---
title: Konfigurieren von Einstellungen für das automatische Herunterfahren eines virtuellen Computers in Azure DevTest Labs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Einstellungen für das automatische Herunterfahren eines virtuellen Computers (virtual machine, VM) konfigurieren, sodass der virtuelle Computer automatisch heruntergefahren wird, wenn er nicht verwendet wird.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: spelluru
ms.openlocfilehash: 934e8fd71c901c89f328c777103a8cb39bf21ac4
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361274"
---
# <a name="configure-autoshutdown-settings-for-a-vm-in-azure-devtest-labs"></a>Konfigurieren von Einstellungen für das automatische Herunterfahren eines virtuellen Computers in Azure DevTest Labs
Mit Azure DevTest Labs können Sie Kosten und unnötigen Aufwand in Ihren Labs minimieren, indem Sie Richtlinien (Einstellungen) für jedes Lab verwalten. In diesem Artikel wird das Konfigurieren einer Richtlinie zum automatischen Herunterfahren für ein Labkonto und der Einstellungen für das automatische Herunterfahren für ein Lab im Labkonto veranschaulicht. Informationen zum Einrichten der einzelnen Labrichtlinien finden Sie unter [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="autoshutdown-policy-for-a-lab"></a>Richtlinie zum automatischen Herunterfahren für ein Lab
Als Labbesitzer können Sie einen Zeitplan für das Herunterfahren für alle virtuellen Computer in Ihrem Lab konfigurieren. Auf diese Weise können Sie Kosten für aktive Computer sparen, die nicht verwendet werden (Leerlauf). Sie können eine Richtlinie für das Herunterfahren für alle virtuellen Computer Ihres Labs zentral erzwingen und Ihren Labbenutzern die Arbeit abnehmen, einen Zeitplan für ihre Computer festzulegen. Mit diesem Feature können Sie die Richtlinie für den Labzeitplan festlegen und den Labbenutzern keine, eine teilweise oder die volle Kontrolle anbieten. Als Labbesitzer können Sie diese Richtlinie mit den folgenden Schritten konfigurieren:

1. Wählen Sie auf der Startseite Ihres Labs **Konfiguration und Richtlinien** aus.
2. Wählen Sie im linken Menü im Abschnitt **Zeitpläne** die Option **Richtlinien zum automatischen Herunterfahren** aus.
3. Wählen Sie eine der Optionen aus. Die folgenden Abschnitte enthalten weitere Informationen zu diesen Optionen: Die festgelegte Richtlinie gilt nur für neu im Lab erstellte virtuelle Computer und nicht für bereits vorhandene virtuelle Computer. 

    ![Optionen für die Richtlinie zum automatischen Herunterfahren](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-autoshutdown-settings-for-a-lab"></a>Konfigurieren von Einstellungen für das automatische Herunterfahren für ein Lab
Die Richtlinie zum automatischen Herunterfahren hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die Uhrzeit anzugeben, zu der die virtuellen Computer für dieses Lab heruntergefahren werden.

Um die Richtlinien für ein Lab anzuzeigen (und zu ändern), gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
4. Wählen Sie **Konfiguration und Richtlinien** aus.

    ![Bereich „Richtlinieneinstellungen“](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. Wählen Sie im Bereich **Konfiguration und Richtlinien** des Labs unter **Zeitpläne** die Option **Automatisch herunterfahren** aus.
   
    ![Automatisches Herunterfahren](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
7. Wenn Sie diese Richtlinie aktivieren, geben Sie die Uhrzeit (und die Zeitzone) an, zu der alle virtuellen Computer im aktuellen Lab heruntergefahren werden sollen.
8. Geben Sie für die Option, mit der 15 Minuten vor der angegebenen Uhrzeit des automatischen Herunterfahrens eine Benachrichtigung gesendet wird, entweder **Ja** oder **Nein** an. Wenn Sie **Ja** auswählen, geben Sie einen Webhook-URL-Endpunkt oder eine E-Mail-Adresse ein, der bzw. die angibt, wo die Benachrichtigung veröffentlicht bzw. an wen diese gesendet wird. Der Benutzer erhält eine Benachrichtigung und hat die Möglichkeit, das Herunterfahren zu verzögern. Weitere Informationen finden Sie im Abschnitt [Benachrichtigungen](#notifications). 
9. Wählen Sie **Speichern** aus.

    Standardmäßig gilt diese Richtlinie nach der Aktivierung für alle virtuellen Computer im aktuellen Lab. Um diese Einstellung von einem bestimmten virtuellen Computer zu entfernen, öffnen Sie den Verwaltungsbereich des virtuellen Computers, und ändern Sie die Einstellung **Automatisch herunterfahren**.

## <a name="configure-autoshutdown-settings-for-a-vm"></a>Konfigurieren von Einstellungen für das automatische Herunterfahren eines virtuellen Computers

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Benutzer legt Zeitplan fest und kann diesen deaktivieren
Wenn diese Richtlinienoption für das Lab festgelegt ist, können Benutzer den Labzeitplan außer Kraft setzen oder deaktivieren. Diese Option gewährt Labbenutzern die vollständige Kontrolle über den Zeitplan für das automatische Herunterfahren ihrer virtuellen Computer. Labbenutzer sehen keine Änderungen auf der Seite mit dem Zeitplan zum automatischen Herunterfahren ihrer virtuellen Computer.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Benutzer legt Zeitplan fest und kann diesen nicht deaktivieren
Wenn diese Richtlinienoption für das Lab festgelegt ist, können Benutzer den Labzeitplan außer Kraft setzen. Sie können die Richtlinie zum automatischen Herunterfahren allerdings nicht deaktivieren. Diese Option stellt sicher, dass es für jeden Computer in Ihrem Lab einen Zeitplan für das automatische Herunterfahren gibt. Labbenutzer können den Zeitplan für das automatische Herunterfahren ihrer virtuellen Computer aktualisieren und Benachrichtigungen über das Herunterfahren einrichten.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Benutzer kann den vom Labadministrator festgelegten Zeitplan nicht steuern
Wenn diese Richtlinienoption für das Lab festgelegt ist, können Benutzer den Labzeitplan nicht außer Kraft setzen oder deaktivieren. Diese Option bietet dem Labadministrator die vollständige Kontrolle über den Zeitplan für jeden Computer im Lab. Labbenutzer können nur Benachrichtigungen zum automatischen Herunterfahren für ihre virtuellen Computer einrichten.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Benachrichtigungen
Nachdem das automatische Herunterfahren eingerichtet wurde, werden 15 Minuten vor dem automatischen Herunterfahren Benachrichtigungen an die Labbenutzer gesendet, sofern deren virtuelle Computer betroffen sind. Mit dieser Option erhalten die Labbenutzer die Möglichkeit, ihre Arbeit vor dem Herunterfahren zu speichern. Die Benachrichtigung enthält auch Links für jeden virtuellen Computer für die folgenden Aktionen:

- Überspringen des automatischen Herunterfahrens für dieses Mal
- Das automatische Herunterfahren wird um eine oder zwei Stunden verschoben, damit sie auf dem virtuellen Computer weiterarbeiten können.

Die Benachrichtigung wird über den konfigurierten Webhook-Endpunkt oder eine E-Mail-Adresse gesendet, die von den Labbesitzern in den Einstellungen für das automatische Herunterfahren angegeben wurde. Webhooks ermöglichen Ihnen das Erstellen oder Einrichten von Integrationen, die bestimmte Ereignisse abonnieren. Wenn ein solches Ereignis ausgelöst wird, sendet DevTest Labs eine HTTP POST-Nutzlast an die für den Webhook konfigurierte URL. Weitere Informationen zu Webhooks finden Sie unter [Erstellen eines Webhooks oder einer API-Azure-Funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Es wird empfohlen, Webhooks zu verwenden, da sie von verschiedenen Apps (z. B. Slack, Azure Logic Apps) umfassend unterstützt werden und Ihnen ermöglichen, eigene Methoden zum Senden von Benachrichtigungen zu implementieren. Ein Beispiel für den Empfang von E-Mail-Benachrichtigungen zum automatischen Herunterfahren unter Verwendung von Azure Logic Apps finden Sie unter [Erstellen einer Logik-App, die E-Mail-Benachrichtigungen empfängt](devtest-lab-auto-shutdown.md#create-a-logic-app-that-receives-email-notifications). 


## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Richtlinien zum automatischen Herunterfahren für ein Lab in Azure DevTest Labs](devtest-lab-auto-shutdown.md)
