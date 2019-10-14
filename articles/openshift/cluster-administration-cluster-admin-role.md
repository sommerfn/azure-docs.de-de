---
title: Administratorrolle für Azure Red Hat OpenShift-Cluster | Microsoft-Dokumentation
description: Zuweisen und Verwenden der Administratorrolle für Azure Red Hat OpenShift-Cluster
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 1c676b2671b73084a2b4ae8908acb83c23a59b7b
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936928"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Administratorrolle für Azure Red Hat OpenShift-Kunden

Sie sind der Clusteradministrator eines Azure Red Hat OpenShift-Clusters. Ihr Konto verfügt über erhöhte Berechtigungen und über Zugriff auf alle vom Benutzer erstellten Projekte.

Wenn Ihrem Konto die Autorisierungsrolle „osa-customer-admins“ zugewiesen ist, kann es ein Projekt automatisch verwalten.

> [!Note] 
> Die Clusterrolle „osa-customer-admin“ ist nicht mit der Clusterrolle „cluster-admin“ identisch.


Beispielsweise können Sie Aktionen ausführen, die mit einer Gruppe von Verben (`create`) verknüpft sind, um einen Satz von Ressourcennamen (`templates`) zu verarbeiten. Führen Sie den folgenden Befehl aus, um Details dieser Rollen und ihren Satz von Verben und Ressourcen anzuzeigen:

`$ oc describe clusterrole/osa-customer-admin`

Die Verbnamen werden nicht unbedingt alle direkt den `oc`-Befehlen zugeordnet. Sie entsprechen genereller den Typen von CLI-Vorgängen, die Sie ausführen können. 

Beispielsweise bedeutet das `list`-Verb, dass Sie eine Liste aller Objekte eines Ressourcennamens anzeigen können (`oc get`). Das `get`-Verb bedeutet, dass Sie die Details eines bestimmten Objekts anzeigen können, wenn Sie dessen Namen kennen (`oc describe`).

## <a name="configure-the-customer-administrator-role"></a>Konfigurieren der Kundenadministratorrolle

Sie können die Kundenadministratorrolle nur während der Clustererstellung konfigurieren, indem Sie das Flag `--customer-admin-group-id` angeben. Informationen zum Konfigurieren von Azure Active Directory und der Gruppe „Administratoren“ finden Sie unter [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren der Rolle „osa-customer-admin“:
> [!div class="nextstepaction"]
> [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).
