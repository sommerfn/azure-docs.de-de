---
title: 'Schnellstart: Verbinden von Computern mit Azure mithilfe von Azure Arc für Server – Portal'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Computer mithilfe von Azure Arc für Server über das Portal mit Azure verbinden.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: bobbytreed
ms.author: robreed
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Updateverwaltung, Änderungsnachverfolgung, Bestand, Runbooks, Python, grafisch, Hybrid, Onboarding
ms.date: 08/25/2019
ms.custom: mvc
ms.topic: quickstart
ms.openlocfilehash: b014f6015b3e13a603cf3893062bd0463eb110ee
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488196"
---
# <a name="quickstart-connect-machines-to-azure-using-azure-arc-for-servers---portal"></a>Schnellstart: Verbinden von Computern mit Azure mithilfe von Azure Arc für Server – Portal

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Machen Sie sich in der [Übersicht über Azure Arc für Server](overview.md) mit den unterstützten Clients und der erforderlichen Netzwerkkonfiguration vertraut.

## <a name="generate-the-agent-install-script-using-the-azure-portal"></a>Generieren des Skripts für die Agent-Installation mit dem Azure-Portal

1. Starten Sie [https://aka.ms/hybridmachineportal ][aka_hybridmachineportal]
1. Klicken Sie auf **+ Hinzufügen**.
1. Befolgen Sie die Anweisungen im Assistenten bis zum Ende.
1. Auf der letzten Seite wird ein Skript generiert, das Sie kopieren (oder herunterladen) können.

Das Skript muss auf dem Zielcomputer ausgeführt werden, für den Sie eine Verbindung herstellen möchten. Mit dem Skript wird in einem einzelnen Vorgang der Agent heruntergeladen und installiert und der Computer verbunden.

Gehen Sie auf den nicht zu Azure gehörenden Servern, die Sie verwalten möchten, wie folgt vor:

1. Melden Sie sich beim Server an (per SSH, RDP oder PowerShell-Remoting).
1. Starten Sie eine Shell: Bash unter Linux, PowerShell als Administrator unter Windows.
1. Fügen Sie das Skript aus dem Portal ein, und führen Sie es auf dem Server aus, für den eine Verbindung mit Azure hergestellt werden soll.
1. Die Standardauthentifizierung für das Onboarding eines einzelnen Servers wird *interaktiv* über die „Geräteanmeldung“ von Azure durchgeführt. Wenn Sie das Skript ausführen, wird in etwa folgende Meldung angezeigt:

  ```none
  To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code B3V3NLWRF to authenticate.
  ```
  
   Öffnen Sie einen Browser, und geben Sie den Code für die Authentifizierung ein. Der Browser muss nicht unbedingt auf dem Server, für den Sie das Onboarding durchführen, ausgeführt werden. Dies kann auch auf einem anderen Computer, z. B. Ihrem Laptop, erfolgen.

1. Falls Sie die Authentifizierung nicht interaktiv vornehmen möchten, müssen Sie die Schritte unter [Erstellen eines Dienstprinzipals](quickstart-onboard-powershell.md#create-a-service-principal-for-onboarding-at-scale) ausführen und das im Portal generierte Skript ändern.

> [!NOTE]
> Falls Sie Internet Explorer auf dem Server zum ersten Mal für die Anmeldung nutzen, wird der Vorgang mit einem Fehler beendet. Sie können den Browser einfach erneut öffnen und den Vorgang wiederholen.

## <a name="execute-the-script-on-target-nodes"></a>Ausführen des Skripts auf Zielknoten

Melden Sie sich bei jedem Knoten an, und führen Sie das im Portal generierte Skript aus. Nachdem das Skript erfolgreich abgeschlossen wurde, können Sie zum Azure-Portal wechseln und dort überprüfen, ob die Verbindungsherstellung für den Server erfolgreich war.

![Erfolgreiches Onboarding](./media/quickstart-onboard/arc-for-servers-successful-onboard.png)

## <a name="clean-up"></a>Bereinigen

Wenn Sie die Verbindung zwischen einem Computer und Azure Arc für Server trennen möchten, müssen Sie zwei Schritte ausführen:

1. Wählen Sie im [Portal](https://aka.ms/hybridmachineportal) den Computer aus, klicken Sie auf die Auslassungspunkte (`...`), und wählen Sie **Löschen** aus.
1. Deinstallieren Sie den Agent auf dem Computer.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen](../../governance/policy/assign-policy-portal.md)