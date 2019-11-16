---
title: Bereitstellen von OpenShift Container Platform 4.x in Azure
description: Bereitstellen von OpenShift Container Platform 4.x in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 213c02b76f822d134729ebc4c0e6bff40f62089f
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74035435"
---
# <a name="deploy-openshift-container-platform-4x-in-azure"></a>Bereitstellen von OpenShift Container Platform 4.x in Azure

Die Bereitstellung von OpenShift Container Platform (OCP) 4.2 wird jetzt in Azure über das IPI-Modell (Installer-Provisioned Infrastructure, vom Installer bereitgestellte Infrastruktur) unterstützt.  Die Angebotsseite zum Testen von OpenShift 4 ist [try.openshift.com](https://try.openshift.com/). Zum Installieren von OCP 4.2 in Azure besuchen Sie die Seite [Red Hat OpenShift Cluster Manager](https://cloud.redhat.com/openshift/install/azure/installer-provisioned).  Für den Zugriff auf diese Website sind Red Hat-Anmeldeinformationen erforderlich.


## <a name="notes"></a>Notizen 

 - Zum Installieren und Ausführen von OCP 4.x in Azure ist ein Azure Active Directory (AAD)-Dienstprinzipal erforderlich.
     - Dem Dienstprinzipal muss die API-Berechtigung **Application.ReadWrite.OwnedBy** für Azure Active Directory Graph erteilt werden.
     - Ein AAD-Mandantenadministrator muss eine Administratoreinwilligung für diese API-Berechtigung geben, damit sie wirksam wird.
     - Dem Dienstprinzipal müssen die Rollen **Mitwirkender** und **Benutzerzugriffsadministrator** für das Abonnement zugewiesen werden.
 - Das Installationsmodell für OCP 4.x unterscheidet sich von 3.x, und es sind keine Azure Resource Manager-Vorlagen für die Bereitstellung von OCP 4.x in Azure verfügbar.
 - Wenn während des Installationsprozesses Probleme auftreten, wenden Sie sich an das jeweilige Unternehmen (Microsoft oder Red Hat).

| Problembeschreibung | Ansprechpartner |
|-------------------|---------------|
| Azure-spezifische Probleme (AAD, Dienstprinzipal, Azure-Abonnement usw.)                              | Microsoft |
| OpenShift-spezifische Probleme (Installationsfehler, Red Hat-Abonnement usw.) |  Red Hat  |




## <a name="next-steps"></a>Nächste Schritte

- [Getting Started with OpenShift Container Platform](https://docs.openshift.com) (Erste Schritte mit OpenShift Container Platform)
