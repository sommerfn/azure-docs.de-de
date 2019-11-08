---
title: Arbeiten mit Sicherheitsrichtlinien | Microsoft-Dokumentation
description: In diesem Artikel ist beschrieben, wie Sie mit Sicherheitsrichtlinien in Azure Security Center arbeiten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 4ac6ac52d6d950d814a37e94ea2801c2ba8e4170
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521202"
---
# <a name="working-with-security-policies"></a>Arbeiten mit Sicherheitsrichtlinien

In diesem Artikel wird beschrieben, wie Sicherheitsrichtlinien konfiguriert werden und wie Sie sie in Security Center anzeigen. 

## <a name="introduction-to-security-policies"></a>Einführung in Sicherheitsrichtlinien

Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei.

Azure Security Center erstellt die Sicherheitsempfehlungen auf der Grundlage der ausgewählten Richtlinien. Security Center-Richtlinien basieren auf Richtlinieninitiativen, die in Azure Policy erstellt wurden. Sie können Ihre Richtlinien in [Azure Policy](../governance/policy/overview.md) konfigurieren und übergreifend für Verwaltungsgruppen und mehrere Abonnements festlegen.

Security Center bietet die folgenden Optionen für die Arbeit mit Sicherheitsrichtlinien:

* **Anzeigen und Bearbeiten der integrierten Standardrichtlinie:** Wenn Sie Security Center aktivieren, wird eine integrierte Initiative mit dem Namen „ASC default“ (ASC Standard) automatisch allen in Security Center registrierten Abonnements (Tarife „Free“ oder „Standard“) zugewiesen. Zum Anpassen dieser Initiative können Sie die einzelnen enthaltenen Richtlinien aktivieren bzw. deaktivieren. Sehen Sie sich die Liste der [integrierten Sicherheitsrichtlinien](security-center-policy-definitions.md) an, um die standardmäßig verfügbaren Optionen zu verstehen.

* **Hinzufügen eigener benutzerdefinierter Richtlinien:** Wenn Sie die auf Ihr Abonnement angewandten Sicherheitsinitiativen anpassen möchten, können Sie dies in Security Center durchführen. Sie erhalten dann Empfehlungen, wenn Ihre Computer Ihre erstellten Richtlinien nicht einhalten. Anweisungen zum Erstellen und Zuweisen von benutzerdefinierten Richtlinien finden Sie unter [Verwenden von benutzerdefinierten Sicherheitsrichtlinien](custom-security-policies.md).

* **Hinzufügen von Richtlinien für die Einhaltung gesetzlicher Bestimmungen:** Das Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen zeigt den Status aller Bewertungen in Ihrer Umgebung im Zusammenhang mit einem bestimmten Standard oder einer Verordnung (z. B. Azure CIS, NIST SP 800-53 R4, SWIFT CSP CSCF-v2020). Weitere Informationen finden Sie unter [Verbessern der Einhaltung gesetzlicher Vorschriften](security-center-compliance-dashboard.md).


## <a name="managing-your-security-policies"></a>Verwalten Ihrer Sicherheitsrichtlinien

Zeigen Sie Ihre Sicherheitsrichtlinien in Security Center wie folgt an:

1. Wählen Sie im **Security Center**-Dashboard die Option **Sicherheitsrichtlinie**.

    ![Bereich „Richtlinienverwaltung“](./media/security-center-policies/security-center-policy-mgt.png)

   Auf dem Bildschirm **Richtlinienverwaltung** können Sie die Anzahl von Verwaltungsgruppen, Abonnements und Arbeitsbereichen sowie Ihre Verwaltungsgruppenstruktur anzeigen.

1. Wählen Sie das Abonnement oder die Verwaltungsgruppe aus, zu dem bzw. der Sie die Richtlinien anzeigen möchten.

1. Die Seite „Sicherheitsrichtlinie“ für dieses Abonnement bzw. diese Verwaltungsgruppe wird angezeigt. Sie enthält die verfügbaren und die zugewiesenen Richtlinien.

   ![Bildschirm mit Richtlinien](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Wenn neben Ihrer Standardrichtlinie die Bezeichnung „Verwaltungsgruppe geerbt“ steht, bedeutet dies, dass die Richtlinie einer Verwaltungsgruppe zugewiesen und vom angezeigten Abonnement geerbt wurde.


1. Wählen Sie in den verfügbaren Optionen auf dieser Seite aus:

    1. Wenn Sie mit Branchenrichtlinien arbeiten möchten, klicken Sie auf **Weitere Standards hinzufügen**. Weitere Informationen finden Sie unter [Aktualisieren von dynamischen Compliancepaketen](update-regulatory-compliance-packages.md).

    1. Um benutzerdefinierte Initiativen zuzuweisen und zu verwalten, klicken Sie auf **Benutzerdefinierte Initiativen hinzufügen**. Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Sicherheitsrichtlinien](custom-security-policies.md).

    1. Zum Anzeigen und Bearbeiten der Standardrichtlinie klicken Sie auf **Effektive Richtlinie anzeigen** und fahren wie unten beschrieben fort. 

       ![Bildschirm mit Richtlinien](./media/security-center-policies/policy-screen.png)
       
       Auf diesem Bildschirm **Sicherheitsrichtlinie** wird die Aktion der Richtlinien widergespiegelt, die dem von Ihnen ausgewählten Abonnement oder der Verwaltungsgruppe zugewiesen sind.
       
       * Verwenden Sie die oben angegebenen Links, um eine **Richtlinienzuweisung** zu öffnen, die für das Abonnement bzw. die Verwaltungsgruppe gilt. Mit diesen Links können Sie auf die Zuweisung zuzugreifen und die Richtlinie bearbeiten oder deaktivieren. Wenn Sie beispielsweise feststellen, dass eine bestimmte Richtlinienzuweisung den Endpunktschutz verhindert, können Sie die Richtlinie über den Link bearbeiten oder deaktivieren.
       
       * In der Liste mit den Richtlinien können Sie die aktive Anwendung der Richtlinie auf Ihr Abonnement oder die Verwaltungsgruppe anzeigen. Die Einstellungen der einzelnen Richtlinien, die für den Bereich gelten, werden berücksichtigt, und das kumulierte Ergebnis der von der Richtlinie durchgeführten Aktionen wird angezeigt. Wenn die Richtlinie beispielsweise in einer Zuweisung deaktiviert wird, aber in einer anderen auf „AuditIfNotExist“ festgelegt ist, wird aufgrund der Kumulation „AuditIfNotExist“ angewandt. Die aktivere Auswirkung hat immer Vorrang.
       
       * Für die Richtlinien können sich die folgenden Auswirkungen ergeben: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists, Disabled. Weitere Informationen zur Anwendung von Auswirkungen finden Sie unter [Grundlegendes zu Richtlinienauswirkungen](../governance/policy/concepts/effects.md).

       > [!NOTE]
       > Beim Anzeigen von zugewiesenen Richtlinien können Sie mehrere Zuweisungen sehen und die jeweilige Konfiguration der Zuweisungen auch einzeln prüfen.


## <a name="who-can-edit-security-policies"></a>Wer kann Sicherheitsrichtlinien bearbeiten?

Sie können Sicherheitsrichtlinien über das Azure Policy-Portal, über die REST-API oder über Windows PowerShell bearbeiten.

Security Center verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Dabei werden integrierte Rollen bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. In Security Center werden den Benutzern nur Informationen zu den Ressourcen angezeigt, auf die sie Zugriff haben. Das bedeutet, dass Benutzern die Rolle *Besitzer*, *Mitwirkender* oder *Leser* für das Abonnement der Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- **Sicherheitsleseberechtigter**: Kann Informationen in Security Center (wie etwa Empfehlungen, Warnungen, Richtlinien und die Integrität) anzeigen, aber keine Änderungen vornehmen.
- **Sicherheitsadministrator**: Verfügt über dieselben Anzeigeberechtigungen wie ein *Sicherheitsleseberechtigter*, ist zusätzlich aber auch zum Aktualisieren der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen berechtigt.


## <a name="disable-security-policies"></a>Deaktivieren von Sicherheitsrichtlinien
Wenn die Standardsicherheitsrichtlinie eine Empfehlung generiert, die für Ihre Umgebung nicht relevant ist, können Sie dies beenden, indem Sie die Richtliniendefinition deaktivieren, die diese Empfehlung sendet.
Weitere Informationen zu Empfehlungen finden Sie unter [Verwalten von Sicherheitsempfehlungen](security-center-recommendations.md).

1. Klicken Sie im Security Center im Abschnitt **Richtlinie und Konformität** auf **Sicherheitsrichtlinie**.

   ![Richtlinienverwaltung](./media/tutorial-security-policy/policy-management.png)

2. Klicken Sie auf das Abonnement, für das Sie die Empfehlung deaktivieren möchten.

   > [!NOTE]
   > Denken Sie daran, dass eine Verwaltungsgruppe ihre Richtlinien auf ihre Abonnements anwendet. Aus diesem Grund erhalten Sie weiterhin Richtlinienempfehlungen, wenn Sie eine Abonnementrichtlinie deaktivieren und das Abonnement zu einer Verwaltungsgruppe gehört, die immer noch dieselbe Richtlinie verwendet. Die Richtlinie wird immer noch von der Verwaltungsebene aus angewandt, und die Empfehlungen werden immer noch generiert.

1. Klicken Sie auf die zugewiesene Richtlinie.

   ![Richtlinie deaktivieren](./media/tutorial-security-policy/security-policy.png)

1. Suchen Sie im Abschnitt **PARAMETER** die Richtlinie, die die zu deaktivierende Empfehlung aufruft, und wählen Sie in der Dropdownliste die Option **Deaktiviert** aus.

   ![Richtlinie deaktivieren](./media/tutorial-security-policy/disable-policy.png)

1. Klicken Sie auf **Speichern**.

   > [!NOTE]
   > Es kann bis zu 12 Stunden dauern, bis die Änderungen zum Deaktivieren der Richtlinie wirksam werden.



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie sich über Sicherheitsrichtlinien informiert. Weitere Informationen finden Sie in den folgenden Artikeln:

* Anweisungen dazu, wie Richtlinien über PowerShell festgelegt werden, finden Sie unter [Schnellstart: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen mithilfe des Azure PowerShell-Moduls](../governance/policy/assign-policy-powershell.md).

* Anweisungen zum Bearbeiten einer Sicherheitsrichtlinie in Azure Policy finden Sie unter [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md).

* Anweisungen zum Festlegen einer Richtlinie für Abonnements oder Verwaltungsgruppen mithilfe von Azure Policy finden Sie unter [Was ist Azure Policy?](../governance/policy/overview.md).