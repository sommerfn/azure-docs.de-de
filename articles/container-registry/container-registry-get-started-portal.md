---
title: 'Schnellstart: Erstellen einer Registrierung im Portal – Azure Container Registry'
description: Hier erfahren Sie, wie Sie ganz schnell eine private Docker-Registrierung über das Azure-Portal in Azure Container Registry erstellen.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: bcf0874266aa83a4b4b932670557854c4688d400
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931629"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>Schnellstart: Erstellen einer privaten Containerregistrierung im Azure-Portal

Eine Azure Container Registry-Instanz ist eine private Docker-Registrierung in Azure, in der Sie Ihre privaten Docker-Containerimages speichern und verwalten können. In dieser Schnellstartanleitung erstellen Sie eine Containerregistrierung über das Azure-Portal. Übertragen Sie anschließend mithilfe von Docker-Befehlen ein Containerimage per Push in die Registrierung. Rufen Sie abschließend das Image per Pull aus der Registrierung ab, und führen Sie es aus.

Für diese Schnellstartanleitung müssen Sie die Azure CLI (mindestens Version 2.0.55 empfohlen) ausführen, um sich bei der Registrierung anmelden und Containerimages verwenden zu können. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli].

Darüber hinaus muss Docker lokal installiert sein. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac][docker-mac]-, [Windows][docker-windows]- oder [Linux][docker-linux]-System konfiguriert werden kann.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Klicken Sie auf **Ressource erstellen** > **Container** > **Container Registry**.

![Erstellen einer Containerregistrierung über das Azure-Portal][qs-portal-01]

Geben Sie Werte für **Registrierungsname** und **Ressourcengruppe** ein. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Erstellen Sie im Rahmen dieser Schnellstartanleitung eine neue Ressourcengruppe namens `myResourceGroup` am Standort `West US`, und wählen Sie für **SKU** die Option „Basic“ aus. Klicken Sie auf **Erstellen**, um die ACR-Instanz bereitzustellen.

![Erstellen einer Containerregistrierung im Azure-Portal][qs-portal-03]

In dieser Schnellstartanleitung erstellen Sie eine Registrierung vom Typ *Basic*. Dabei handelt es sich um eine kostenoptimierte Option für Entwickler, die sich mit Azure Container Registry vertraut machen. Ausführliche Informationen zu verfügbaren Dienstebenen finden Sie unter [Azure Container Registry-SKUs][container-registry-skus].

Wenn die Meldung **Bereitstellung erfolgreich** erscheint, wählen Sie die Containerregistrierung im Portal aus. 

![Containerregistrierungsübersicht im Azure-Portal][qs-portal-05]

Notieren Sie sich den Wert unter **Anmeldeserver**. Diesen Wert benötigen Sie in den folgenden Schritten, wenn Sie über die Azure CLI und Docker Aktionen für Ihre Registrierung ausführen.

## <a name="log-in-to-registry"></a>Anmelden bei der Registrierung

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der ACR-Instanz anmelden. Öffnen Sie in Ihrem Betriebssystem eine Befehlsshell, und verwenden Sie den Befehl [az acr login][az-acr-login] in der Azure CLI.

```azurecli
az acr login --name <acrName>
```

Der Befehl gibt nach Abschluss des Vorgangs `Login Succeeded` zurück. 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Auflisten von Containerimages

Navigieren Sie zum Auflisten der Images in Ihrer Registrierung im Portal zu Ihrer Registrierung, klicken Sie auf **Repositorys**, und wählen Sie das Repository aus, das Sie mit `docker push` erstellt haben.

In diesem Beispiel wählen wir das Repository **hello-world** aus. Das mit `v1` markierte Image wird unter **TAGS** angezeigt.

![Auflisten von Containerimages im Azure-Portal][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Navigieren Sie zum Bereinigen von Ressourcen im Portal zur Ressourcengruppe **myResourceGroup**. Klicken Sie nach dem Laden der Ressourcengruppe auf **Ressourcengruppe löschen**, um die Ressourcengruppe, die Containerregistrierung und die dort gespeicherten Containerimages zu entfernen.

![Löschen der Ressourcengruppe im Azure-Portal][qs-portal-08]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie im Azure-Portal eine Azure Container Registry-Instanz erstellt, ein Containerimage per Push übertragen und das Image per Pull aus der Registrierung abgerufen und ausgeführt. Fahren Sie mit den Azure Container Registry-Tutorials fort, um eingehendere Informationen zu ACR zu erhalten.

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry][container-registry-tutorial-quick-task]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-login]: /cli/azure/acr#az-acr-login
