---
title: Berechtigungen für Repositorys in Azure Container Registry
description: Erstellen eines Tokens mit Berechtigungen, die für bestimmte Repositorys in einer Registrierung gelten, um Images zu pullen oder zu pushen
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2019
ms.author: danlep
ms.openlocfilehash: 7b9d220ac7e507513458eab6b55276b3aa434739
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73742750"
---
# <a name="repository-scoped-permissions-in-azure-container-registry"></a>Repositorybezogene Berechtigungen in Azure Container Registry 

Azure Container Registry unterstützt mehrere [Authentifizierungsoptionen](container-registry-authentication.md) unter Verwendung von Identitäten, die über [rollenbasierten Zugriff](container-registry-roles.md) auf eine vollständige Registrierung verfügen. In bestimmten Szenarien möchten Sie jedoch möglicherweise nur Zugriff auf bestimmte *Repositorys* in einer Registrierung gewähren. 

Dieser Artikel zeigt, wie Sie ein Zugriffstoken erstellen und verwenden, das über Berechtigungen verfügt, Aktionen nur für bestimmte Repositorys in einer Registrierung auszuführen. Mit einem Zugriffstoken können Sie Benutzern oder Diensten einen zeitlich begrenzten Zugriff auf Repositorys gewähren, um Images zu pullen oder zu pushen oder andere Aktionen durchzuführen. 

Lesen Sie [Informationen zu repositorybezogenen Berechtigungen](#about-repository-scoped-permissions) weiter unten in diesem Artikel, um Hintergrundinformationen zu Tokenkonzepten und Szenarien zu erhalten.

> [!IMPORTANT]
> Dieses Feature befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Dieses Feature ist nur in einer Containerregistrierung des Typs **Premium** verfügbar. Weitere Informationen zu den Tarifen des Registrierungsdiensts und zu den Einschränkungen finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).
* Zurzeit können Sie einem Azure Active Directory-Objekt (beispielsweise einem Dienstprinzipal oder einer verwalteten Identität) keine repositorybezogenen Berechtigungen zuweisen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure Cli**: Für diesen Artikel wird eine lokale Installation der Azure CLI (Version 2.0.76 oder höher) benötigt. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).
* **Docker-** : Um sich bei der Registrierung zu authentifizieren, benötigen Sie auch eine lokale Docker-Installation. Docker-Installationsanleitungen stehen für Systeme unter [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung.
* **Containerregistrierung mit Repositorys**: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung, wenn noch keine Registrierung vorhanden ist. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder die [Azure CLI](container-registry-get-started-azure-cli.md). 

  [Pushen](container-registry-get-started-docker-cli.md) oder [importieren](container-registry-import-images.md) Sie zu Testzwecken mindestens ein Beispielimage in die Registrierung. Die Beispiele in diesem Artikel beziehen sich auf die folgenden Images in zwei Repositorys: `samples/hello-world:v1` und `samples/nginx:v1`. 

## <a name="create-an-access-token"></a>Erstellen eines Zugriffstokens

Erstellen Sie mit dem Befehl [az acr token create][az-acr-token-create] ein Token. Wenn Sie ein Token erstellen, geben Sie mindestens ein Repository und zugehörige Aktionen für jedes Repository an, oder geben Sie eine vorhandene Gültigkeitsbereichszuordnung mit diesen Einstellungen an.

### <a name="create-access-token-and-specify-repositories"></a>Erstellen eines Zugriffstokens und Angeben von Repositorys

Im folgenden Beispiel wird ein Zugriffstoken mit Berechtigungen zum Ausführen von `content/write`- und `content/read`-Aktionen im `samples/hello-world`-Repository und der `content/read`-Aktion im `samples/nginx`-Repository erstellt. Standardmäßig generiert der Befehl zwei Kennwörter. 

In diesem Beispiel wird der Tokenstatus auf `enabled` (die Standardeinstellung) festgelegt. Sie können das Token jedoch jederzeit aktualisieren und den Status auf `disabled` festlegen.

```azurecli
az acr token create --name MyToken --registry myregistry \
  --repository samples/hello-world content/write content/read \
  --repository samples/nginx content/read --status enabled
```

In der Ausgabe werden Details zum Token angezeigt, einschließlich der generierten Kennwörter und der Gültigkeitsbereichszuordnung. Es wird empfohlen, die Kennwörter an einem sicheren Ort zu speichern, um sie später mit `docker login` zu verwenden. Die Kennwörter können nicht erneut abgerufen werden, aber es können neue Kennwörter generiert werden.

Die Ausgabe zeigt auch, dass automatisch eine Gültigkeitsbereichszuordnung mit dem Namen `MyToken-scope-map` erstellt wird. Mit der Gültigkeitsbereichszuordnung können Sie dieselben Repositoryaktionen auf andere Token anwenden. Oder aktualisieren Sie die Gültigkeitsbereichszuordnung später, um die Tokenberechtigungen zu ändern.

```console
{
  "creationDate": "2019-10-22T00:15:34.066221+00:00",
  "credentials": {
    "certificates": [],
    "passwords": [
      {
        "creationTime": "2019-10-22T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password1",
        "value": "uH54BxxxxK7KOxxxxRbr26dAs8JXxxxx"
      },
      {
        "creationTime": "2019-10-22T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password2",
        "value": "kPX6Or/xxxxLXpqowxxxxkA0idwLtmxxxx"
      }
    ],
    "username": "MyToken"
  },
  "id": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/tokens/MyToken",
  "name": "MyToken",
  "objectId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapId": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyToken-scope-map",
  "status": "enabled",
  "type": "Microsoft.ContainerRegistry/registries/tokens"
```

### <a name="create-a-scope-map-and-associated-token"></a>Erstellen einer Gültigkeitsbereichszuordnung und eines zugehörigen Tokens

Sie können alternativ eine Gültigkeitsbereichszuordnung mit Repositorys und zugeordneten Aktionen angeben, wenn Sie ein Token erstellen. Um eine Gültigkeitsbereichszuordnung zu erstellen, verwenden Sie den Befehl [az acr scope-map create][az-acr-scope-map-create].

Mit dem folgenden Beispielbefehl wird eine Gültigkeitsbereichszuordnung mit den gleichen Berechtigungen erstellt, die im vorherigen Beispiel verwendet wurden. Sie ermöglicht `content/write`- und `content/read`-Aktionen für das `samples/hello-world` Repository und die `content/read`-Aktion für das `samples/nginx`-Repository:

```azurecli
az acr scope-map create --name MyScopeMap --registry myregistry \
  --repository samples/hello-world content/write content/read \
  --repository samples/nginx content/read \
  --description "Sample scope map"
```

Die Ausgabe sieht in etwa wie folgt aus:

```console
{
  "actions": [
    "repositories/samples/hello-world/content/write",
    "repositories/samples/nginx/content/read"
  ],
  "creationDate": "2019-10-22T05:07:35.194413+00:00",
  "description": "Sample scope map.",
  "id": "/subscriptions/fxxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyScopeMap",
  "name": "MyScopeMap",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapType": "UserDefined",
  "type": "Microsoft.ContainerRegistry/registries/scopeMaps"
```

Führen Sie [az acr token create][az-acr-token-create] aus, um ein Token zu erstellen, das der Gültigkeitsbereichszuordnung *MyScopeMap* zugeordnet ist. Standardmäßig generiert der Befehl zwei Kennwörter. In diesem Beispiel wird der Tokenstatus auf `enabled` (die Standardeinstellung) festgelegt. Sie können das Token jedoch jederzeit aktualisieren und den Status auf `disabled` festlegen.

```azurecli
az acr token create --name MyToken --registry myregistry --scope-map MyScopeMap --status enabled
```

In der Ausgabe werden Details zum Token angezeigt, einschließlich der generierten Kennwörter und der Gültigkeitsbereichszuordnung, die Sie angewendet haben. Es wird empfohlen, die Kennwörter an einem sicheren Ort zu speichern, um sie später mit `docker login` zu verwenden. Die Kennwörter können nicht erneut abgerufen werden, aber es können neue Kennwörter generiert werden.

## <a name="generate-passwords-for-token"></a>Generieren von Kennwörtern für das Token

Wenn beim Erstellen des Tokens Kennwörter erstellt wurden, fahren Sie mit [Authentifizieren mit dem Registrierung](#authenticate-using-token) fort.

Wenn Sie nicht über ein Tokenkennwort verfügen oder neue Kennwörter generieren möchten, führen Sie den Befehl [az acr token credential generate][az-acr-token-credential-generate] aus.

Im folgenden Beispiel wird ein neues Kennwort mit einem Ablaufzeitraum von 30 Tagen für das Token generiert, das Sie erstellt haben. Das Kennwort wird in der Umgebungsvariablen TOKEN_PWD gespeichert. Dieses Beispiel ist für die Bash-Shell formatiert.

```azurecli
TOKEN_PWD=$(az acr token credential generate \
  --name MyToken --registry myregistry --days 30 \
  --password1 --query 'passwords[0].value' --output tsv)
```

## <a name="authenticate-using-token"></a>Authentifizierung mithilfe des Tokens

Führen Sie `docker login` aus, um sich mithilfe der Tokenanmeldeinformationen bei der Registrierung zu authentifizieren. Geben Sie den Tokennamen als Benutzernamen ein, und geben Sie eines seiner Kennwörter an. Das folgende Beispiel ist für die Bash-Shell formatiert und stellt die Werte mithilfe von Umgebungsvariablen bereit.

```bash
TOKEN_NAME=MyToken
TOKEN_PWD=<token password>

echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin myregistry.azurecr.io
```

Die Ausgabe sollte die erfolgreiche Authentifizierung anzeigen:

```console
Login Succeeded
```

## <a name="verify-scoped-access"></a>Überprüfen des bereichsbezogenen Zugriffs

Sie können überprüfen, ob das Token über bereichsbezogene Berechtigungen für die Repositorys in der Registrierung verfügt. In diesem Beispiel werden die folgenden `docker pull`-Befehle erfolgreich ausgeführt, um Images zu pullen, die in den `samples/hello-world`- und `samples/nginx`-Repositorys verfügbar sind:

```console
docker pull myregistry.azurecr.io/samples/hello-world:v1
docker pull myregistry.azurecr.io/samples/nginx:v1
```

Da das Beispieltoken nur die `content/write`-Aktion für das `samples/hello-world`-Repository zulässt, ist `docker push` für dieses Repository erfolgreich, schlägt jedoch für `samples/nginx` fehl:

```console
# docker push succeeds
docker pull myregistry.azurecr.io/samples/hello-world:v1

# docker push fails
docker pull myregistry.azurecr.io/samples/nginx:v1
```

## <a name="update-scope-map-and-token"></a>Aktualisieren der Gültigkeitsbereichszuordnung und des Tokens

Um Tokenberechtigungen zu aktualisieren, aktualisieren Sie die Berechtigungen in der zugehörigen Gültigkeitsbereichszuordnung mit [az acr scope-map update][az-acr-scope-map-update]. Um beispielsweise *MyScopeMap* zu aktualisieren, entfernen Sie die Aktion `content/write` für das Repository `samples/hello-world`:

```azurecli
az acr scope-map update --name MyScopeMap --registry myregistry \
  --remove samples/hello-world content/write
```

Wenn die Gültigkeitsbereichszuordnung mehreren Token zugeordnet ist, aktualisiert der Befehl die Berechtigung aller zugehörigen Token.

Wenn Sie ein Token mit einer anderen Gültigkeitsbereichszuordnung aktualisieren möchten, führen Sie [az acr token update][az-acr-token-update] aus. Beispiel:

```azurecli
az acr token update --name MyToken --registry myregistry \
  --scope-map MyNewScopeMap
```

Nach dem Aktualisieren eines Tokens oder einer Gültigkeitsbereichszuordnung, die einem Token zugeordnet ist, werden die Berechtigungsänderungen beim nächsten `docker login`-Vorgang oder bei einer anderen Authentifizierung mit dem Token wirksam.

Nach dem Aktualisieren eines Tokens möchten Sie möglicherweise neue Kennwörter generieren, um auf die Registrierung zuzugreifen. Führen Sie [az acr token credential generate][az-acr-token-credential-generate] aus. Beispiel:

```azurecli
az acr token credential generate \
  --name MyToken --registry myregistry --days 30
```

## <a name="about-repository-scoped-permissions"></a>Informationen zu repositorybezogenen Berechtigungen

### <a name="concepts"></a>Konzepte

Um repositorybezogene Berechtigungen zu konfigurieren, erstellen Sie ein *Zugriffstoken* und eine zugehörige *Gültigkeitsbereichszuordnung* mithilfe von Befehlen in der Azure CLI.

* Bei einem **Zugriffstoken** handelt es sich um Anmeldeinformationen, die mit einem Kennwort für die Authentifizierung bei der Registrierung verwendet werden. Jedem Token sind zulässige *Aktionen* mit einem Gültigkeitsbereich für mindestens ein Repository zugeordnet. Sie können für jedes Token eine Ablaufzeit festlegen. 

* Zu den **Aktionen** für jedes angegebene Repository gehören die folgenden Aktionen.

  |Aktion  |BESCHREIBUNG  |
  |---------|---------|
  |`content/read`     |  Lesen von Daten aus dem Repository. Beispielsweise das Pullen eines Artefakts.  |
  |`metadata/read`    | Lesen von Metadaten aus dem Repository. Beispielsweise das Auflisten von Tags oder Anzeigen von Manifestmetadaten.   |
  |`content/write`     |  Schreiben von Daten in das Repository. Verwenden Sie die Aktion zusammen mit `content/read` zum Pushen eines Artefakts.    |
  |`metadata/write`     |  Schreiben von Metadaten in das Repository. Beispielsweise Aktualisieren der Manifestattribute.  |
  |`content/delete`    | Entfernen von Daten aus dem Repository. Beispielsweise Löschen eines Repositorys oder eines Manifests. |

* Eine **Gültigkeitsbereichszuordnung** ist ein Registrierungsobjekt, das die für ein Token geltenden Repositoryberechtigungen gruppiert oder auf andere Token erneut anwendet. Wenn Sie beim Erstellen eines Tokens keine Gültigkeitsbereichszuordnung anwenden, wird automatisch eine Gültigkeitsbereichszuordnung erstellt, um die Berechtigungseinstellungen zu speichern. 

  Eine Gültigkeitsbereichszuordnung unterstützt Sie bei der Konfiguration mehrerer Benutzer mit identischem Zugriff auf eine Sammlung von Repositorys. Azure Container Registry bietet auch vom System definierte Gültigkeitsbereichszuordnungen, die Sie beim Erstellen von Zugriffstoken anwenden können.

In der folgenden Abbildung wird die Beziehung zwischen Token und Gültigkeitsbereichszuordnungen zusammengefasst. 

![Gültigkeitsbereichszuordnungen der Registrierung und Token](media/container-registry-repository-scoped-permissions/token-scope-map-concepts.png)

### <a name="scenarios"></a>Szenarien

Zu den Szenarien für die Verwendung eines Zugriffstokens gehören:

* Bereitstellen von IoT-Geräten mit individuellen Token zum Pullen eines Images aus einem Repository
* Bereitstellen einer externen Organisation mit Berechtigungen für ein bestimmtes Repository 
* Einschränken des Zugriffs auf bestimmte Benutzergruppen in Ihrer Organisation. Beispielsweise Bereitstellen von Schreib- und Lesezugriff für Entwickler, die Images für bestimmte Repositorys erstellen, und von Lesezugriff für Teams, die aus diesen Repositorys Bereitstellungen durchführen.

### <a name="authentication-using-token"></a>Authentifizierung mithilfe von Token

Verwenden Sie einen Tokennamen als Benutzernamen und eines der zugehörigen Kennwörter, um sich bei der Zielregistrierung zu authentifizieren. Welche Authentifizierungsmethode verwendet wird, hängt von den konfigurierten Aktionen ab.

### <a name="contentread-or-contentwrite"></a>content/read oder content/write

Wenn das Token nur `content/read`- oder `content/write`-Aktionen zulässt, stellen Sie Tokenanmeldeinformationen für einen der folgenden Authentifizierungsabläufe bereit:

* Authentifizieren mit Docker mithilfe von `docker login`
* Authentifizieren bei der Registrierung mit dem Befehl [az acr login][az-acr-login] in der Azure CLI

Nach der Authentifizierung ermöglicht das Token die konfigurierten Aktionen in den bereichsbezogenen Repositorys. Wenn das Token beispielsweise die `content/read`-Aktion in einem Repository zulässt, sind `docker pull`-Vorgänge für Images in diesem Repository zulässig.

#### <a name="metadataread-metadatawrite-or-contentdelete"></a>metadata/read, metadata/write oder content/delete

Wenn das Token `metadata/read`-, `metadata/write`- oder `content/delete`-Aktionen für ein Repository zulässt, müssen Tokenanmeldeinformationen als Parameter mit den zugehörigen [az acr repository][az-acr-repository]-Befehlen in der Azure CLI bereitgestellt werden.

Wenn beispielsweise `metadata/read`-Aktionen für ein Repository zulässig sind, übergeben Sie die Tokenanmeldeinformationen, wenn Sie den Befehl [az acr repository show-tags][az-acr-repository-show-tags] ausführen, um Tags aufzulisten.

## <a name="next-steps"></a>Nächste Schritte

* Um Gültigkeitsbereichszuordnungen und Zugriffstoken zu verwalten, verwenden Sie zusätzliche Befehle in den [az acr scope-map][az-acr-scope-map]- und [az acr token][az-acr-token]-Befehlsgruppen.
* In der [Authentifizierungsübersicht](container-registry-authentication.md) finden Sie Szenarien zur Authentifizierung mit einer Azure-Containerregistrierung über ein Administratorkonto oder eine Azure Active Directory-Identität.


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository]: /cli/azure/acr/repository/
[az-acr-repository-show-tags]: /cli/azure/acr/repository/#az-acr-repository-show-tags
[az-acr-scope-map]: /cli/azure/acr/scope-map/
[az-acr-scope-map-create]: /cli/azure/acr/scope-map/#az-acr-scope-map-create
[az-acr-scope-map-update]: /cli/azure/acr/scope-map/#az-acr-scope-map-update
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-token]: /cli/azure/acr/token/
[az-acr-token-show]: /cli/azure/acr/token/#az-acr-token-show
[az-acr-token-create]: /cli/azure/acr/token/#az-acr-token-create
[az-acr-token-update]: /cli/azure/acr/token/#az-acr-token-update
[az-acr-token-credential-generate]: /cli/azure/acr/token/credential/#az-acr-token-credential-generate
