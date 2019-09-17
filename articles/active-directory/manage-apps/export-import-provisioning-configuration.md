---
title: Exportieren oder Importieren Ihrer Bereitstellungskonfiguration mithilfe von Graph-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Bereitstellungskonfiguration mithilfe von Graph-API exportieren und importieren.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c3e92ee5ffd97174331703b703e811bd1ce5f43
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70815861"
---
# <a name="export-or-import-your-provisioning-configuration-by-using-graph-api"></a>Exportieren oder Importieren Ihrer Bereitstellungskonfiguration mithilfe von Graph-API

Mit Microsoft Graph-API und Graph-Tester können Sie die Attributzuordnungen und das Schema Ihrer Benutzerbereitstellung in eine JSON-Datei exportieren und wieder in Azure AD importieren. Sie können die hier aufgeführten Schritte auch verwenden, um eine Sicherung Ihrer Bereitstellungskonfiguration zu erstellen. 

## <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>Schritt 1: Abrufen der Dienstprinzipal-ID der Bereitstellungs-App (Objekt-ID)

1. Starten Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zum Abschnitt „Eigenschaften“ Ihrer Bereitstellungsanwendung. Wenn Sie z. B. die Zuordnung Ihrer *Anwendung für die Benutzerbereitstellung von Workday in AD* exportieren möchten, navigieren Sie zum Abschnitt „Eigenschaften“ dieser App. 
1. Kopieren Sie im Abschnitt „Eigenschaften“ Ihrer Bereitstellungs-App den GUID-Wert, der dem Feld *Objekt-ID* zugeordnet ist. Dieser Wert wird auch als die **ServicePrincipalId** Ihrer App bezeichnet, und wird in Graph-Tester-Vorgängen verwendet.

   ![Dienstprinzipal-ID der Workday-App](./media/export-import-provisioning-mappings/wd_export_01.png)

## <a name="step-2-sign-into-microsoft-graph-explorer"></a>Schritt 2: Anmelden bei Microsoft Graph-Tester

1. Starten Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).
1. Klicken Sie auf die Schaltfläche „Mit Microsoft anmelden“, und melden Sie sich mit den globalen Azure AD-Anmeldeinformationen oder mit den Anmeldeinformationen für den App-Administrator an.

    ![Graph-Anmeldung](./media/export-import-provisioning-mappings/wd_export_02.png)

1. Nach der erfolgreichen Anmeldung sehen Sie die Details des Benutzerkontos im linken Bereich.

## <a name="step-3-retrieve-the-provisioning-job-id-of-the-provisioning-app"></a>Schritt 3: Abrufen der Bereitstellungsauftrags-ID der Bereitstellungs-App

Führen Sie im Microsoft Graph-Tester die folgende GET-Abfrage durch und ersetzen Sie [servicePrincipalId] durch die in [Schritt 1](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id) extrahierte **ServicePrincipalId**.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

Sie erhalten eine Antwort, wie unten dargestellt. Kopieren Sie das „id attribut“ aus der Antwort. Dieser Wert ist die **ProvisioningJobId** und wird zum Abrufen der zugrundeliegenden Schemametadaten verwendet.

   [![ID des Bereitstellungsauftrags](./media/export-import-provisioning-mappings/wd_export_03.png)](./media/export-import-provisioning-mappings/wd_export_03.png#lightbox)

## <a name="step-4-download-the-provisioning-schema"></a>Schritt 4: Herunterladen des Bereitstellungsschemas

Führen Sie im Microsoft Graph-Tester die folgende GET-Abfrage durch und ersetzen Sie [servicePrincipalId] und [ProvisioningJobId] durch die in den vorherigen Schritten abgerufen Werten für „ServicePrincipalId“ und „ProvisioningJobId“.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Kopieren Sie das JSON-Objekt aus der Antwort, und speichern Sie sie in einer Datei, um eine Sicherung des Schemas zu erstellen.

## <a name="step-5-import-the-provisioning-schema"></a>Schritt 5: Importieren des Bereitstellungsschemas

> [!CAUTION]
> Führen Sie diesen Schritt nur aus, wenn Sie das Schema für eine Konfiguration ändern müssen, die nicht über das Azure-Portal geändert werden kann, oder wenn Sie die Konfiguration aus einer zuvor gesicherten Datei mit einem gültigen und funktionierenden Schema wiederherstellen müssen.

Konfigurieren Sie im Microsoft Graph-Tester die folgende PUT-Abfrage durch und ersetzen Sie [servicePrincipalId] und [ProvisioningJobId] durch die in den vorherigen Schritten abgerufen Werten für „ServicePrincipalId“ und „ProvisioningJobId“.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Kopieren Sie auf der Registerkarte „Anforderungstext“ den Inhalt der JSON-Schemadatei.

   [![Anforderungstext](./media/export-import-provisioning-mappings/wd_export_04.png)](./media/export-import-provisioning-mappings/wd_export_04.png#lightbox)

Fügen Sie auf der Registerkarte „Anforderungsheader“ das Content-Type-Header-Attribut mit dem Wert „application/json“ ein.

   [![Anforderungsheader](./media/export-import-provisioning-mappings/wd_export_05.png)](./media/export-import-provisioning-mappings/wd_export_05.png#lightbox)

Klicken Sie auf die Schaltfläche „Abfrage ausführen“, um das neue Schema zu importieren.
