---
title: Aktivieren der Azure-VMware-Lösung von CloudSimple-Dienst
description: Beschreibt, wie Sie den CloudSimple-Dienst in einem Azure-Abonnement aktivieren und anschließend den CloudSimple-Ressourcenanbieter registrieren
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2553aa95d5028c510b4e1a1b7f51a9f410bcea51
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154857"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registrieren des Microsoft.VMwareCloudSimple-Ressourcenanbieters in Ihrem Azure-Abonnement

Der CloudSimple-Dienst ermöglicht es Ihnen, die Azure-VMware-Lösung von CloudSimple zu nutzen. Sie können den Microsoft.VMwareCloudSimple-Dienst als Ihren Ressourcenanbieter registrieren.

## <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Alle Dienste** aus.

3. Suchen Sie nach **Abonnements**, und wählen Sie diese Option aus.

    ![Auswählen von Abonnements](media/cloudsimple-service-select-subscriptions.png)

4. Wählen Sie das Abonnement aus, für das Sie den CloudSimple-Dienst aktivieren möchten.

5. Klicken Sie auf **Ressourcenanbieter** für das Abonnement.

6. Verwenden Sie **Microsoft.VMwareCloudSimple**, um den Ressourcenanbieter zu filtern.

7. Wählen Sie den Ressourcenanbieter aus, und klicken Sie auf **Registrieren**.

    ![Registrieren des Ressourcenanbieters](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Erstellen eines CloudSimple-Diensts](create-cloudsimple-service.md).
* Erfahren Sie mehr über [Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md).
