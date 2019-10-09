---
title: Administratorrolle für Azure Red Hat OpenShift-Cluster | Microsoft-Dokumentation
description: Zuweisen und Verwenden der Administratorrolle für Azure Red Hat OpenShift-Cluster
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 6d7aacea5e34e21513452880448227a1ca5f9b67
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71712837"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Administratorrolle für Azure Red Hat OpenShift-Kunden

Als ARO-Clusteradministrator (Azure Red Hat OpenShift) eines OpenShift-Clusters verfügt Ihr Konto über erweiterte Berechtigungen und Zugriff auf alle von Benutzern erstellte Projekte.

Wenn Ihrem Konto die Autorisierungsrolle „osa-customer-admins“ zugewiesen ist, kann es ein Projekt automatisch verwalten.

> [!Note] 
> Die Clusterrolle „osa-customer-admin“ ist nicht mit der Clusterrole „cluster-admin“ identisch.


Beispielsweise können Sie Aktionen ausführen, die mit einer Gruppe von Verben (`create`) verknüpft sind, um einen Satz von Ressourcennamen (`templates`) zu verarbeiten. Führen Sie den folgenden Befehl aus, um Details dieser Rollen und ihren Satz von Verben und Ressourcen anzuzeigen:

`$ oc describe clusterrole/osa-customer-admin`

Die Verbnamen werden nicht unbedingt direkt den oc-Befehlen zugeordnet, sondern entsprechen eher den Typen von CLI-Vorgängen, die Sie ausführen können. Beispielsweise bedeutet das Verb `list`, dass Sie eine Liste aller Objekte eines bestimmten Ressourcennamens (`oc get`) anzeigen können, während das Verb `get` bedeutet, dass Sie Details eines bestimmten Objekts anzeigen können, wenn Sie dessen Namen (`oc describe`) kennen.

## <a name="how-to-configure-customer-administrator-role"></a>Konfigurieren der Kundenadministratorrolle

Die Kundenadministratorrolle kann nur bei der Clustererstellung konfiguriert werden, indem das Flag `--customer-admin-group-id` angegeben wird. Eine Anleitung zum Konfigurieren von Azure Active Directory und der Gruppe „Administratoren“ finden Sie unter: [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren der Rolle „osa-customer-admin“:
> [!div class="nextstepaction"]
> [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).
