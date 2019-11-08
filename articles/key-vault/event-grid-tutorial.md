---
title: Empfangen von und Reagieren auf Key Vault-Benachrichtigungen mit Azure Event Grid
description: Hier erfahren Sie, wie Sie Key Vault in Azure Event Grid integrieren.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b24da4d988554da240baba2984df44ff4744aaf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464102"
---
# <a name="how-to-receive-and-respond-to-key-vault-notifications-with-azure-event-grid-preview"></a>Gewusst wie: Empfangen von und Reagieren auf Key Vault-Benachrichtigungen mit Azure Event Grid (Vorschau)

Die Key Vault-Integration in Azure Event Grid befindet sich derzeit in der Vorschauphase und ermöglicht es Benutzern, sich benachrichtigen zu lassen, wenn sich der Status eines im Schlüsseltresor gespeicherten Geheimnisses geändert hat. Eine Übersicht über dieses Feature finden Sie unter [Überwachen von Key Vault mit Azure Event Grid](event-grid-overview.md).

In dieser Anleitung erfahren Sie, wie Sie Key Vault-Benachrichtigungen über Azure Event Grid erhalten und mithilfe von Azure Automation auf Statusänderungen reagieren.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Ein Schlüsseltresor in Ihrem Azure-Abonnement. Sie können ganz schnell einen neuen Schlüsseltresor erstellen. Eine entsprechende Anleitung finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe der Azure CLI](quick-create-cli.md).

## <a name="concepts"></a>Konzepte

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Leitfaden abonnieren Sie Ereignisse für einen Schlüsseltresor und leiten Ereignisse an Azure Automation weiter. Wenn eines der Geheimnisse im Schlüsseltresor in Kürze abläuft, wird Event Grid über die Statusänderung informiert und sendet eine HTTP POST-Anforderung an den Endpunkt. Daraufhin löst ein Webhook die Ausführung eines PowerShell-Skripts durch Azure Automation aus.

![image](media/image1.png)

## <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Erstellen Sie ein Azure Automation-Konto über das [Azure-Portal](https://portal.azure.com).

1.  Navigieren Sie zu „portal.azure.com“, und melden Sie sich bei Ihrem Abonnement an.

1.  Geben Sie „Automation-Konten“ in das Suchfeld ein.

1.  Wählen Sie auf der Suchleiste im Abschnitt „Dienste“ des Dropdownmenüs die Option „Automation-Konten“ aus.

1.  Klicken Sie auf "Hinzufügen".

    ![](media/image2.png)

1.  Geben Sie auf dem Blatt „Automation-Konto hinzufügen“ die erforderlichen Informationen an, und wählen Sie anschließend „Erstellen“ aus.

## <a name="create-a-runbook"></a>Erstellen eines Runbooks

Wenn Ihr Azure Automation Konto bereit ist, erstellen Sie ein Runbook.

![](media/image3.png)

1.  Wählen Sie das soeben erstellte Automation-Konto aus.

1.  Wählen Sie im Abschnitt „Prozessautomatisierung“ die Option „Runbooks“ aus.

1.  Klicken Sie auf „Runbook erstellen“.

1.  Benennen Sie Ihr Runbook, und wählen Sie „PowerShell“ als Runbooktyp aus.

1.  Klicken Sie auf das erstellte Runbook, und wählen Sie die Schaltfläche „Bearbeiten“ aus.

1.  Geben Sie zu Testzwecken den folgenden Code ein, und klicken Sie anschließend auf die Schaltfläche „Veröffentlichen“. Dadurch wird das Ergebnis der empfangenen POST-Anforderung ausgegeben.

```azurepowershell
param
(
[Parameter (Mandatory = $false)]
[object] $WebhookData
)

#If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

#rotate secret:
#generate new secret version in key vault
#update db/service with generated secret

#Write-Output "WebhookData <$WebhookData>"
Write-Output $WebhookData.RequestBody
}
else
{
# Error
write-Error "No input data found." 
}
```

![](media/image4.png)

## <a name="create-a-webhook"></a>Erstellen eines Webhooks

Erstellen Sie nun einen Webhook, um Ihr neu erstelltes Runbook auszulösen.

1.  Wählen Sie im Ressourcenabschnitt des soeben veröffentlichten Runbooks die Option „Webhooks“ aus.

1.  Klicken Sie auf „Webhook hinzufügen“.

    ![](media/image5.png)

1.  Wählen Sie „Neuen Webhook erstellen“ aus.

1. Benennen Sie den Webhook, legen Sie ein Ablaufdatum fest, und **kopieren Sie die URL**.

    > [!IMPORTANT] 
    > Nach der Erstellung kann die URL nicht mehr angezeigt werden. Speichern Sie sie an einem sicheren Ort, auf den Sie im weiteren Verlauf dieses Leitfadens problemlos zugreifen können.

1. Klicken Sie auf „Parameter und Ausführungseinstellungen konfigurieren“, und wählen Sie „OK“ aus. Geben Sie keine Parameter ein. Dadurch wird die Schaltfläche „Erstellen“ aktiviert.

1. Wählen Sie „OK“ und anschließend „Erstellen“ aus.

    ![](media/image6.png)

## <a name="create-an-event-grid-subscription"></a>Erstellen eines Event Grid-Abonnements

Erstellen Sie ein Event Grid-Abonnement über das [Azure-Portal](https://portal.azure.com).

1.  Verwenden Sie den folgenden Link, um das Azure-Portal zu öffnen: https://portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true

1.  Navigieren Sie zu Ihrem Schlüsseltresor, und wählen Sie die Registerkarte „Ereignisse“ aus. Sollte die Registerkarte „Ereignisse“ nicht angezeigt werden, vergewissern Sie sich, dass Sie die [Vorschauversion des Portals](https://ms.portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true) verwenden.

    ![](media/image7.png)

1.  Klicken Sie auf die Schaltfläche „+ Ereignisabonnement“ aus.

1.  Erstellen Sie einen aussagekräftigen Namen für das Abonnement.

1.  Wählen Sie „Event Grid-Schema“ aus.

1.  „Themenressource“ muss der Schlüsseltresor sein, den Sie auf Statusänderungen überwachen möchten.

1.  Lassen Sie unter „Nach Ereignistypen filtern“ alle aktiviert („9 ausgewählt“).

1.  Wählen Sie unter „Endpunkttyp“ die Option „Webhook“ aus.

1.  Wählen Sie „Endpunkt auswählen“ aus. Fügen Sie im neuen Kontextbereich die Webhook-URL aus dem Schritt [Erstellen eines Webhooks](#create-a-webhook) in das Feld „Abonnentenendpunkt“ ein.

1.  Wählen Sie im Kontextbereich die Option „Auswahl bestätigen“ aus.

1.  Wählen Sie „Erstellen“.

    ![](media/image8.png)

## <a name="test-and-verify"></a>Testen und Überprüfen

Vergewissern Sie sich, dass Ihr Event Grid-Abonnement ordnungsgemäß konfiguriert ist.  Bei diesem Test wird davon ausgegangen, dass Sie im Schritt [Erstellen eines Event Grid-Abonnements](#create-an-event-grid-subscription) die Benachrichtigung für die Erstellung einer neuen Geheimnisversion abonniert haben und über die nötigen Berechtigungen verfügen, um in einem Schlüsseltresor eine neue Version eines Geheimnisses zu erstellen.

![](media/image9.png)

![](media/image10.png)

1.  Navigieren Sie im Azure-Portal zu Ihrem Schlüsseltresor.

1.  Erstellen Sie ein neues Geheimnis. Legen Sie zu Testzwecken das Ablaufdatum auf den nächsten Tag fest.

1.  Navigieren Sie in Ihrem Schlüsseltresor zur Registerkarte „Ereignisse“.

1.  Wählen Sie das erstellte Event Grid-Abonnement aus.

1.  Überprüfen Sie unter „Metriken“, ob ein Ereignis erfasst wurde. Zwei Ereignisse werden erwartet: „SecretNewVersion“ und „SecretNearExpiry“. Dadurch wird überprüft, ob die Statusänderung des Geheimnisses in Ihrem Schlüsseltresor von Event Grid erfolgreich erfasst wurde.

    ![](media/image11.png)

1.  Wechseln Sie zu Ihrem Azure Automation-Konto.

1.  Wählen Sie die Registerkarte „Runbooks“ und anschließend das erstellte Runbook aus.

1.  Wählen Sie die Registerkarte „Webhooks“ aus, und vergewissern Sie sich, dass der Zeitstempel der letzten Auslösung innerhalb von 60 Sekunden nach der Erstellung des neuen Geheimnisses liegt.  Ist dies der Fall, hat Event Grid eine POST-Anforderung mit den Ereignisdetails der Statusänderung in Ihrem Schlüsseltresor an den Webhook gesendet, und der Webhook wurde ausgelöst.

    ![](media/image12.png)

1. Kehren Sie zu Ihrem Runbook zurück, und wählen Sie die Registerkarte „Übersicht“ aus.

1. Sehen Sie sich die Liste mit den aktuellen Aufträgen an. Darin sollte ein erstellter und abgeschlossener Auftrag enthalten sein.  Ist dies der Fall, hat der Webhook das Runbook ausgelöst, um die Skriptausführung zu starten.

    ![](media/image13.png)

1. Wählen Sie den aktuellen Auftrag aus, und sehen Sie sich die POST-Anforderung an, die von Event Grid an den Webhook gesendet wurde. Überprüfen Sie den JSON-Code, und vergewissern Sie sich, dass die Parameter für den Schlüsseltresor und den Ereignistyp korrekt sind. Wenn der Parameter „event type“ (Ereignistyp) im JSON-Objekt dem Ereignis entspricht, das im Schlüsseltresor aufgetreten ist (in diesem Beispiel: Microsoft.KeyVault.SecretNearExpiry), war der Test erfolgreich.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="unable-to-create-event-subscription"></a>Erstellen des Ereignisabonnements nicht möglich

Wiederholen Sie die Registrierung des Event Grid- und Key Vault-Anbieters in den Ressourcenanbietern Ihres Azure-Abonnements. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Nächste Schritte

Glückwunsch! Wenn Sie alle oben beschriebenen Schritte ausgeführt haben, können Sie nun programmgesteuert auf Statusänderungen von in Ihrem Schlüsseltresor gespeicherten Geheimnissen reagieren.

Falls Sie bislang ein abrufbasiertes System verwendet haben, um Statusänderungen von Geheimnissen in Ihrem Schlüsseltresor zu ermitteln, migrieren Sie zur Verwendung dieses Benachrichtigungsfeatures. Das Testskript in Ihrem Runbook kann auch durch Code zur programmgesteuerten Erneuerung Ihrer in Kürze ablaufenden Geheimnisse ersetzt werden.

Weitere Informationen:

- [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)
- [Was ist Azure Event Grid?](../event-grid/overview.md)
- [Überwachen von Key Vault mit Azure Event Grid (Vorschau)](event-grid-overview.md)
- [Azure Event Grid-Ereignisschema für Azure Key Vault (Vorschau)](../event-grid/event-schema-key-vault.md)
- [Azure Automation – Übersicht](../automation/index.yml)
