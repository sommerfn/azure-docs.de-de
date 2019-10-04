---
title: Es werden keine Benutzer für eine Azure AD-Kataloganwendung bereitgestellt | Microsoft-Dokumentation
description: Informationen zur Problembehandlung bei häufigen Problemen, die auftreten, wenn Benutzer nicht in einer Azure AD-Kataloganwendung angezeigt werden, die Sie für die Benutzerbereitstellung mit Azure AD konfiguriert haben
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0562027bb599b596b640e0c1a669447e3fed9680
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71034177"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Es werden keine Benutzer für eine Azure AD-Kataloganwendung bereitgestellt
Nachdem die automatische Bereitstellung für eine Anwendung konfiguriert wurde (einschließlich der Überprüfung, ob die Anmeldeinformationen für die App gültig sind, die Azure AD bereitgestellt wurden, um die Verbindung mit der Anwendung herzustellen), werden Benutzer bzw. Gruppen für die App bereitgestellt. Die Bereitstellung wird durch die folgenden Faktoren bestimmt:

-   Welche Benutzer und Gruppen wurden der Anwendung **zugewiesen**. Beachten Sie, dass die Bereitstellung von geschachtelten Gruppen oder Office 365-Gruppen nicht unterstützt wird. Weitere Informationen zur Zuweisung finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](assign-user-or-group-access-portal.md).
-   Ob **Attributzuordnungen** aktiviert und so konfiguriert sind, dass gültige Attribute von Azure AD mit der App synchronisiert werden. Weitere Informationen zu Attributzuordnungen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](customize-application-attributes.md).
-   Ob ein **Bereichsfilter** vorhanden ist, der Benutzer auf Basis bestimmter Attributwerte filtert. Weitere Informationen zu Bereichsfiltern finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](define-conditional-rules-for-provisioning-user-accounts.md).
  
Falls Sie bemerken, dass Benutzer nicht bereitgestellt werden, können Sie in den [Bereitstellungsprotokollen (Vorschau)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) in Azure AD nachsehen. Suchen Sie nach Protokolleinträgen für einen bestimmten Benutzer.

Sie können im Azure-Portal auf die Bereitstellungsprotokolle zugreifen. Wählen Sie dazu **Azure Active Directory** &gt; **Unternehmensanwendungen** &gt; **Bereitstellungsprotokolle (Vorschau)** (im Abschnitt **Aktivität**) aus. Sie können die Bereitstellungsdaten anhand des Benutzernamens oder des Bezeichners entweder im Quell- oder im Zielsystem durchsuchen. Einzelheiten hierzu finden Sie unter [Bereitstellungsprotokolle (Vorschau)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context). 

Die Bereitstellungsprotokolle zeichnen alle vom Bereitstellungsdienst durchgeführten Vorgänge auf, einschließlich der Abfragen von Azure AD für zugewiesene Benutzer, die sich im Bereitstellungsbereich befinden. Zudem fragen sie die Ziel-App in Bezug auf das Vorkommen dieser Benutzer ab, wobei die Benutzerobjekte zwischen dem System verglichen werden. Fügen Sie dann auf Basis des Vergleichs das Benutzerkonto zum Zielsystem hinzu oder aktualisieren bzw. deaktivieren Sie es.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Zu berücksichtigende allgemeine Problembereiche bei der Bereitstellung
Nachfolgend finden Sie eine Liste der allgemeinen Problembereiche, für die Sie Detailinformationen anzeigen können, wenn Sie wissen, wo Sie starten müssen.

- [Bereitstellungsdienst wird anscheinend nicht gestartet](#provisioning-service-does-not-appear-to-start)
- [Bereitstellungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Bereitstellungsdienst wird anscheinend nicht gestartet
Wenn Sie für **Bereitstellungsstatus** im Abschnitt **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt;Bereitstellung** des Azure-Portals die Option **Ein** festlegen. Auf dieser Seite werden jedoch nach anschließendem erneuten Laden keine anderen Statusdetails angezeigt. Es ist wahrscheinlich, dass der Dienst ausgeführt wird, aber noch keinen Startzyklus abgeschlossen hat. Überprüfen Sie die oben beschriebenen **Bereitstellungsprotokolle (Vorschau)** , um zu ermitteln, welche Vorgänge der Dienst durchführt und ob Fehler aufgetreten sind.

>[!NOTE]
>Ein Startzyklus kann zwischen 20 Minuten und mehreren Stunden dauern. Dies hängt von der Größe des Azure AD-Verzeichnisses und der Anzahl der Benutzer im Bereitstellungsbereich ab. Nachfolgende Synchronisierungen nach dem Startzyklus werden schneller durchgeführt, da der Bereitstellungsdienst Wasserzeichen speichert, die den Status beider Systeme nach dem Startzyklus darstellen. Mit dem Startzyklus wird die Leistung nachfolgender Synchronisierungen verbessert.
>


## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Bereitstellungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind

Wenn ein Benutzer in den Bereitstellungsprotokollen als „übersprungen“ angezeigt wird, ist es wichtig, anhand der Registerkarte **Schritte** des Protokolls die Ursache zu ermitteln. Nachfolgend sind häufige Ursachen und Lösungen aufgeführt:

- **Es wurde ein Bereichsfilter konfiguriert** **, der einen Benutzer basierend auf einem Attributwert herausfiltert**. Weitere Informationen zu Bereichsfiltern finden Sie unter [Bereichsfilter](define-conditional-rules-for-provisioning-user-accounts.md).
- **Der Benutzer ist „nicht wirklich berechtigt“.** Wenn diese bestimmte Fehlermeldung angezeigt wird, liegt ein Problem mit dem Benutzerzuweisungseintrag vor, der in Azure AD gespeichert wird. Heben Sie die Zuweisung des Benutzers (oder der Gruppe) zur App auf, und weisen Sie ihn bzw. sie erneut zu, um dieses Problem zu beheben. Weitere Informationen zur Zuweisung finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](assign-user-or-group-access-portal.md).
- **Ein erforderliches Attribut fehlt oder wurde für einen Benutzer nicht ausgefüllt.** Beim Einrichten der Bereitstellung sollte unbedingt beachtet werden, dass die Attributzuordnungen und Workflows überprüft und konfiguriert werden, mit denen festgelegt wird, welche Benutzereigenschaften (oder Gruppeneigenschaften) von Azure AD zur Anwendung gelangen. Diese Konfiguration umfasst auch das Festlegen der „übereinstimmenden Eigenschaft“, mit der Benutzer/Gruppen zwischen den beiden Systemen eindeutig identifiziert und abgeglichen werden. Weitere Informationen zu diesem wichtigen Vorgang finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](customize-application-attributes.md).
- **Attributzuordnungen für Gruppen:** Die Bereitstellung von Gruppennamen und Gruppendetails zusätzlich zu den Mitgliedern wird für einige Anwendungen unterstützt. Sie können diese Funktionen aktivieren oder deaktivieren, indem Sie die **Zuordnung** für Gruppenobjekte aktivieren oder deaktivieren, die auf der Registerkarte **Bereitstellung** angezeigt wird. Wenn die Bereitstellung von Gruppen aktiviert ist, überprüfen Sie auf jeden Fall die Attributzuordnungen, um sicherzustellen, dass ein entsprechendes Feld für die „übereinstimmende ID“ verwendet wird. Die übereinstimmende ID kann der Anzeigename oder der E-Mail-Alias sein. Die Gruppe und ihre Mitglieder werden nicht bereitgestellt, wenn die übereinstimmende Eigenschaft leer ist oder für eine Gruppe in Azure AD nicht aufgefüllt wurde.

## <a name="next-steps"></a>Nächste Schritte

[Azure AD Connect-Synchronisierung: Grundlegendes zur deklarativen Bereitstellung](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
