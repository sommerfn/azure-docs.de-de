---
title: Verwalten von Einschränkungen des Sicherheitskontexts in Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Administratoren von Azure Red Hat OpenShift-Clustern, die Einschränkungen des Sicherheitskontexts verwalten
services: container-service
author: troy0820
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 8e85ac98683487c6b18be7f502f28cad9a0c2251
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71712841"
---
# <a name="overview"></a>Übersicht 

Einschränkungen des Sicherheitskontexts ermöglichen Administratoren, die Berechtigungen für Pods zu steuern. Weitere Informationen zu diesem API-Typ finden Sie in der Dokumentation der Architektur zu [Einschränkungen beim Sicherheitskontext](https://https://docs.openshift.com/container-platform/3.11/architecture/additional_concepts/authorization.html) (Security Context Constraints, SCCs). Sie können diese Einschränkungen beim Sicherheitskontext in Ihrer Instanz mithilfe der Befehlszeilenschnittstelle wie normale API-Objekte verwalten.

## <a name="listing-security-context-constraints"></a>Auflisten von Einschränkungen beim Sicherheitskontext

So erstellen Sie eine aktuelle Liste der Einschränkungen beim Sicherheitskontext 

```bash
$ oc get scc

NAME               PRIV      CAPS      SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY   READONLYROOTFS   VOLUMES
anyuid             false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    10         false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
hostaccess         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath persistentVolumeClaim secret]
hostmount-anyuid   false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath nfs persistentVolumeClaim secret]
hostnetwork        false     []        MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
nonroot            false     []        MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
privileged         true      [*]       RunAsAny    RunAsAny           RunAsAny    RunAsAny    <none>     false            [*]
restricted         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
```

## <a name="examining-a-security-context-constraints-object"></a>Untersuchen eines Einschränkungsobjekts für den Sicherheitskontext

Verwenden Sie zum Untersuchen einer bestimmten Einschränkung für den Sicherheitskontext `oc get`, `oc describe` oder `oc edit`.  So untersuchen Sie beispielsweise die Einschränkung **restricted** für den Sicherheitskontext
```bash
$ oc describe scc restricted
Name:                   restricted
Priority:               <none>
Access:
  Users:                <none>
  Groups:               system:authenticated
Settings:
  Allow Privileged:         false
  Default Add Capabilities:     <none>
  Required Drop Capabilities:       KILL,MKNOD,SYS_CHROOT,SETUID,SETGID
  Allowed Capabilities:         <none>
  Allowed Seccomp Profiles:     <none>
  Allowed Volume Types:         configMap,downwardAPI,emptyDir,persistentVolumeClaim,projected,secret
  Allow Host Network:           false
  Allow Host Ports:         false
  Allow Host PID:           false
  Allow Host IPC:           false
  Read Only Root Filesystem:        false
  Run As User Strategy: MustRunAsRange
    UID:                <none>
    UID Range Min:          <none>
    UID Range Max:          <none>
  SELinux Context Strategy: MustRunAs
    User:               <none>
    Role:               <none>
    Type:               <none>
    Level:              <none>
  FSGroup Strategy: MustRunAs
    Ranges:             <none>
  Supplemental Groups Strategy: RunAsAny
    Ranges:             <none>
```
## <a name="next-steps"></a>Nächste Schritte
Konfigurieren der Rolle „osa-customer-admin“:
> [!div class="nextstepaction"]
> [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md). 