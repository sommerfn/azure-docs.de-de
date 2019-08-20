---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/31/2019
ms.openlocfilehash: e571c65e64fad73c646aa05366cab685aa745caa
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968870"
---
1. Melden Sie sich mit den Anmeldeinformationen, die Sie für das Azure-Abonnement verwenden, beim [Azure-Portal](https://portal.azure.com/) an. 

1. Wählen Sie im Azure-Portal oben links die Option **Ressource erstellen** aus.

1. Suchen Sie mithilfe der Suchleiste den **Machine Learning Service-Arbeitsbereich**.

1. Wählen Sie **Machine Learning Service-Arbeitsbereich** aus.

1. Wählen Sie im Bereich **ML-Dienstarbeitsbereich** die Option **Erstellen** aus, um zu beginnen.

1. Konfigurieren Sie Ihren neuen Arbeitsbereich, indem Sie den Arbeitsbereichsnamen, das Abonnement, die Ressourcengruppe und den Standort angeben.

    ![Arbeitsbereich erstellen](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Feld|BESCHREIBUNG 
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert. In diesem Beispiel verwenden wir **docs-ws**. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet.  
   Subscription |Wählen Sie das gewünschte Azure-Abonnement aus.
   Resource group | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe enthält verwandte Ressourcen für eine Azure-Lösung. In diesem Beispiel verwenden wir **docs-aml**. 
   Location | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist, um Ihren Arbeitsbereich zu erstellen.

1. Wählen Sie **Erstellen** aus, nachdem die Konfiguration des Arbeitsbereichs abgeschlossen ist. 

   Es kann einige Augenblicke dauern, bis der Arbeitsbereich erstellt wurde.

   Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. Diese finden Sie auch im Abschnitt „Benachrichtigungen“. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

   ![Status der Arbeitsbereichserstellung](./media/aml-create-in-portal/notifications.png)

1. Falls Sie später eine Option ohne Code aus dem Portal (etwa die grafische Benutzeroberfläche oder automatisierte ML-Experimente) verwenden, sind Sie jetzt fertig. Erstellen Sie einen [virtuellen Notebook-Computer](../articles/machine-learning/service/tutorial-1st-experiment-sdk-setup.md#azure), sind Sie ebenfalls fertig. 

1. Wenn Sie Code in Ihrer lokalen Umgebung verwenden möchten, der auf diesen Arbeitsbereich verweist, wählen Sie im Abschnitt **Übersicht** des Arbeitsbereichs die Option **„config.json“ herunterladen** aus.  

   ![Herunterladen von „config.json“](./media/aml-create-in-portal/configure.png)
   
   Legen Sie die Datei in der Verzeichnisstruktur mit Ihren Python-Skripts oder Jupyter Notebooks ab. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden. Bei der Erstellung eines virtuellen Notebook-Computers wird diese Datei für Sie dem richtigen Verzeichnis auf dem virtuellen Computer hinzugefügt.

    

