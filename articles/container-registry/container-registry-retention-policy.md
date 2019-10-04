---
title: Richtlinie zum Aufbewahren von nicht markierten Manifesten in Azure Container Registry
description: Hier erfahren Sie, wie Sie eine Aufbewahrungsrichtlinie in Ihrer Azure-Containerregistrierung aktivieren, um nicht markierte Manifeste nach einem definierten Zeitraum automatisch zu löschen.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 09/25/2019
ms.author: danlep
ms.openlocfilehash: 36d27bc6089bbe3f4ada6862a9c1be1fa0bdbae7
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71305921"
---
# <a name="set-a-retention-policy-for-untagged-manifests"></a>Festlegen einer Aufbewahrungsrichtlinie für nicht markierte Manifeste

In Azure Container Registry haben Sie die Möglichkeit, eine *Aufbewahrungsrichtlinie* für gespeicherte Imagemanifeste ohne zugeordnete Markierungen (*nicht markierte Manifeste*) festzulegen. Ist eine Aufbewahrungsrichtlinie aktiviert, werden nicht markierte Manifeste in der Registrierung nach einer von Ihnen festgelegten Anzahl von Tagen automatisch gelöscht. Mit dieser Funktion wird eine Überfüllung der Registrierung mit nicht mehr benötigten Artefakten vermieden, und Speicherkosten werden gespart. Wird das Attribut `delete-enabled` eines nicht markierten Manifests auf `false` festgelegt, kann das Manifest nicht gelöscht werden, und die Aufbewahrungsrichtlinie wird nicht angewendet.

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI verwenden, um die Beispielbefehle in diesem Artikel auszuführen. Für die lokale Verwendung ist Version 2.0.74 oder höher erforderlich. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli].

> [!IMPORTANT]
> Dieses Feature befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

> [!WARNING]
> Legen Sie eine Aufbewahrungsrichtlinie stets mit Bedacht fest, da gelöschte Imagedaten nicht wiederhergestellt werden können. Bei Systemen, die Images nach dem Manifest-Digest pullen (und nicht nach dem Imagenamen), sollten Sie für nicht markierte Manifeste keine Aufbewahrungsrichtlinie festlegen. Wenn Sie Images ohne Tags löschen, hindern Sie diese Systeme daran, die Images aus Ihrer Registrierung zu löschen. Erwägen Sie statt des Pullens nach Manifest die Einführung eines *eindeutigen Tagging*-Schemas. Dies ist eine [bewährte Methode](container-registry-image-tag-version.md).

Informationen zum Löschen einzelner Imagetags oder Manifeste mithilfe von Azure CLI-Befehlen finden Sie unter [Löschen von Containerimages in Azure Container Registry](container-registry-delete.md).

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Nur im Tarif **Premium** kann die Containerregistrierung mit einer Aufbewahrungsrichtlinie konfiguriert werden. Informationen zu den Tarifen des Registrierungsdiensts finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).
* Eine Aufbewahrungsrichtlinie kann nur für nicht markierte Manifeste festgelegt werden.

## <a name="set-a-retention-policy---cli"></a>Festlegen einer Aufbewahrungsrichtlinie über die CLI

Im folgenden Beispiel wird gezeigt, wie Sie mithilfe der Azure CLI eine Aufbewahrungsrichtlinie für nicht markierte Manifeste in einer Registrierung festlegen.

### <a name="enable-a-retention-policy"></a>Aktivieren einer Aufbewahrungsrichtlinie

Standardmäßig ist in einer Containerregistrierung keine Aufbewahrungsrichtlinie festgelegt. Führen Sie zum Festlegen oder Aktualisieren einer Aufbewahrungsrichtlinie den Befehl [az acr config retention update][az-acr-config-retention-update] in der Azure CLI aus. Sie können einen Zeitraum zwischen 0 und 365 Tagen angeben, in dem die nicht markierten Manifeste aufbewahrt werden. Wenn Sie keine Angaben machen, legt der Befehl einen Standardwert von sieben Tagen fest. Nach Ablauf der Aufbewahrungsdauer werden alle nicht markierten Manifeste in der Registrierung automatisch gelöscht.

Im folgenden Beispiel wird eine Aufbewahrungsrichtlinie mit einer Aufbewahrungsdauer von 30 Tagen für nicht markierte Manifeste in der Registrierung *myregistry* festgelegt:

```azurecli
az acr config retention update --name myregistry --status enabled --days 30 --type UntaggedManifests
```

Im folgenden Beispiel wird eine Richtlinie festgelegt, durch die jedes Manifest in der Registrierung gelöscht wird, das keine Markierung aufweist. Legen Sie hierzu die Aufbewahrungsdauer auf 0 Tage fest:

```azurecli
az acr config retention update --name myregistry --status enabled --days 0 --type UntaggedManifests
```

### <a name="disable-a-retention-policy"></a>Deaktivieren einer Aufbewahrungsrichtlinie

Führen Sie den Befehl [az acr config retention show][az-acr-config-retention-show] aus, um die in einer Registrierung festgelegte Aufbewahrungsrichtlinie anzuzeigen:

```azurecli
az acr config retention show --name myregistry
```

Führen Sie den Befehl [az acr config retention update][az-acr-config-retention-update] aus, und legen Sie `--status disabled` fest, um die Aufbewahrungsrichtlinie in einer Registrierung zu deaktivieren:

```azurecli
az acr config retention update --name myregistry --status disabled
```

## <a name="set-a-retention-policy---portal"></a>Festlegen einer Aufbewahrungsrichtlinie über das Portal

Sie können eine Aufbewahrungsrichtlinie in einer Registrierung auch über das [Azure-Portal](https://portal.azure.com) festlegen. Im folgenden Beispiel wird gezeigt, wie Sie über das Portal eine Aufbewahrungsrichtlinie für nicht markierte Manifeste in einer Registrierung festlegen.

### <a name="enable-a-retention-policy"></a>Aktivieren einer Aufbewahrungsrichtlinie

1. Navigieren Sie zu Ihrer Azure-Containerregistrierung. Wählen Sie unter **Richtlinien** die Option **Aufbewahrung (Vorschau)** aus.
1. Wählen Sie **Aktiviert** als **Status** aus.
1. Geben Sie einen Zeitraum zwischen 0 und 365 Tagen an, in dem die nicht markierten Manifeste aufbewahrt werden. Wählen Sie **Speichern** aus.

![Aktivieren einer Aufbewahrungsrichtlinie im Azure-Portal](media/container-registry-retention-policy/container-registry-retention-policy01.png)

### <a name="disable-a-retention-policy"></a>Deaktivieren einer Aufbewahrungsrichtlinie

1. Navigieren Sie zu Ihrer Azure-Containerregistrierung. Wählen Sie unter **Richtlinien** die Option **Aufbewahrung (Vorschau)** aus.
1. Wählen Sie **Deaktiviert** als **Status** aus. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

* Hier erfahren Sie mehr zu den Optionen zum [Löschen von Images und Repositorys](container-registry-delete.md) in Azure Container Registry.

* Hier erfahren Sie, wie Sie ausgewählte Images und Manifeste aus einer Registrierung [automatisch bereinigen](container-registry-auto-purge.md).

* Hier erfahren Sie mehr zu den Optionen zum [Sperren von Images und Manifesten](container-registry-image-lock.md) in einer Registrierung.

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/


<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-config-retention-update]: /cli/azure/acr/config/retention#az-acr-config-retention-update
[az-acr-config-retention-show]: /cli/azure/acr/config/retention#az-acr-config-retention-show
