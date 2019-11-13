---
title: Azure Container Registry-SKUs
description: Erfahren Sie mehr über die Features und Beschränkungen der Diensttarife (SKUs) Basic, Standard und Premium von Azure Container Registry.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 11/05/2019
ms.author: danlep
ms.openlocfilehash: 19b1fb78413f82d422779b12227b4a5e2361d813
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681813"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry-SKUs

Azure Container Registry (ACR) steht in verschiedenen Dienstebenen, sogenannten SKUs, zur Verfügung. Durch diese SKUs können Preise im Voraus eingeschätzt werden. Darüber hinaus bieten sie verschiedene Optionen zur Anpassung an die Kapazität und das Nutzungsverhalten Ihrer privaten Docker-Registrierung in Azure.

| SKU | BESCHREIBUNG |
| --- | ----------- |
| **Basic** | Ein kostenoptimierter Einstiegspunkt für Entwickler, die sich mit Azure Container Registry vertraut machen. Basic-Registrierungen verfügen über die gleichen Programmfunktionen wie Standard- und Premium-Registrierungen (z.B. Azure Active Directory-[Authentifizierungsintegration](container-registry-authentication.md#individual-login-with-azure-ad), [Löschen von Images][container-registry-delete] und [Webhooks][container-registry-webhook]). Der gebotene Speicher- und Imagedurchsatz ist jedoch für Szenarien mit geringerer Nutzung am besten geeignet. |
| **Standard** | Standard-Registrierungen bieten die gleichen Funktionen wie Basic, jedoch höhere Speichergrenzwerte und einen höheren Imagedurchsatz. Standard-Registrierungen erfüllen üblicherweise die Bedürfnisse der meisten Produktionsszenarios. |
| **Premium** | Premium-Registrierungen bieten den größten Umfang an Speicher und gleichzeitigen Vorgängen, sodass Szenarien mit großen Volumen möglich sind. Neben einem höheren Imagedurchsatz bietet Premium Funktionen wie [Georeplikation][container-registry-geo-replication] zum regionsübergreifenden Verwalten einer einzelnen Registrierung, [Inhaltsvertrauensstellung](container-registry-content-trust.md) für das Signieren von Imagetags sowie [Firewalls und virtuelle Netzwerke (Vorschauversion)](container-registry-vnet.md) zum Einschränken des Zugriffs auf die Registrierung. |

Die SKUs Basic, Standard und Premium bieten alle dieselben programmgesteuerten Funktionen. Sie alle profitieren auch von [Imagespeicher][container-registry-storage], der vollständig von Azure verwaltet wird. Wenn Sie sich für eine SKU auf höherer Ebene entscheiden, können Sie von einer höheren Leistung und Skalierung profitieren. Wenn mehrere Dienstebenen vorhanden sind, können Sie mit „Basic“ beginnen und dann bei zunehmender Registrierungsnutzung in „Standard“ und „Premium“ konvertieren.

## <a name="sku-features-and-limits"></a>SKU-Features und -Beschränkungen

In der folgenden Tabelle werden die Features und Grenzwerte der Dienstebenen „Basic“, „Standard“ und „Premium“ dargestellt.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Wechseln von SKUs

Sie können die SKU einer Registrierung mit der Azure CLI oder im Azure-Portal wechseln. Zwischen den SKUs können Sie sich frei bewegen, solange die SKU, zu der Sie wechseln, über die erforderliche maximale Speicherkapazität verfügt. 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Zwischen SKUs kann mithilfe des Azure CLI-Befehls [az acr update][az-acr-update] gewechselt werden. Gehen Sie beispielsweise wie folgt vor, um zu Premium zu wechseln:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure-Portal

Klicken Sie im Azure-Portal in der Containerregistrierung **Übersicht** auf **Update**, und wählen Sie anschließend im Dropdownmenü die neue **SKU** aus.

![Aktualisieren der Containerregistrierung-SKU im Azure-Portal][update-registry-sku]

## <a name="pricing"></a>Preise

Informationen zu Preisen für jede SKU von Azure Container Registry finden Sie unter [Preise von Azure Container Registry][container-registry-pricing].

Weitere Informationen zu den Preisen für Datenübertragungen finden Sie unter [Bandbreite: Preisdetails](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Nächste Schritte

**Azure Container Registry – Roadmap**

Besuchen Sie [ACR Roadmap][acr-roadmap] auf GitHub, wo Sie Informationen zu zukünftigen Features des Diensts finden.

**Azure Container Registry – UserVoice**

Unter [ACR UserVoice][container-registry-uservoice] können Sie neue Featurevorschläge einsenden oder für diese abstimmen.

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
