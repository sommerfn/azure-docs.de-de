---
title: Erstellen und Verwenden von Kennwortrichtlinien in Azure AD Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie und warum differenzierte Kennwortrichtlinien verwendet werden, um Kontokennwörter in einer verwalteten Azure AD DS-Domäne zu sichern und zu steuern.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 08/08/2019
ms.author: iainfou
ms.openlocfilehash: 45fb2daaeaf9ee788207d43d805e070320372ca0
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617197"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Kennwort- und Kontosperrungsrichtlinien in verwalteten Domänen

Zum Verwalten der Kontosicherheit in Azure Active Directory Domain Services (Azure AD DS) können differenzierte Kennwortrichtlinien definiert werden, mit denen Einstellungen wie die Mindestlänge, die Ablaufzeit oder die Komplexität von Kennwörtern gesteuert werden. Eine Standard-Kennwortrichtlinie gilt für alle Benutzer in einer verwalteten Azure AD DS-Domäne. Um eine differenzierte Steuerung zu gewährleisten und bestimmte Geschäfts- oder Complianceanforderungen zu erfüllen, können zusätzliche Richtlinien erstellt und auf bestimmte Gruppen von Benutzern angewendet werden.

In diesem Artikel wird das Erstellen und Konfigurieren einer differenzierten Kennwortrichtlinie mithilfe des Active Directory-Verwaltungscenters erläutert.

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
  * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
  * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
  * Führen Sie bei Bedarf das Tutorial zum [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][create-azure-ad-ds-instance] aus.
* Eine Windows Server-Verwaltungs-VM, die in die verwaltete Azure AD DS-Domäne eingebunden ist.
  * Führen Sie bei Bedarf das Tutorial zum [Erstellen eines virtuellen Verwaltungscomputers][tutorial-create-management-vm] aus.
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="fine-grained-password-policies-fgpp-overview"></a>Übersicht über differenzierte Kennwortrichtlinien (FGPP)

Mit differenzierten Kennwortrichtlinien (FGPP) können Sie bestimmte Einschränkungen für Kennwort- und Kontosperrungsrichtlinien auf verschiedene Benutzer in einer Domäne anwenden. Zum Sichern privilegierter Konten können Sie beispielsweise strengere Kennworteinstellungen anwenden als für reguläre Konten, die nicht privilegiert sind. Sie können mehrere FGPPs erstellen, um Kennwortrichtlinien innerhalb einer verwalteten Azure AD DS-Domäne anzugeben.

Sie können die folgenden Kennworteinstellungen mit FGPP konfigurieren:

* Minimale Kennwortlänge
* Kennwortverlauf
* Kennwörter müssen den Anforderungen an die Komplexität entsprechen
* Minimales Kennwortalter
* Maximales Kennwortalter
* Kontosperrungsrichtlinie
  * Kontosperrungsdauer
  * Zulässige Anzahl fehlgeschlagener Anmeldeversuche
  * Anzahl fehlgeschlagener Anmeldeversuche zurücksetzen nach

FGPP betrifft nur in Azure AD DS erstellte Benutzer. Cloudbenutzer und Domänenbenutzer, die von Azure AD in der verwalteten Azure AD DS-Domäne synchronisiert werden, sind von den Kennwortrichtlinien nicht betroffen.

Richtlinien werden über die Gruppenzuordnung in der verwalteten Azure AD DS-Domäne verteilt. Von Ihnen vorgenommene Änderungen werden bei der nächsten Benutzeranmeldung angewendet. Wenn Sie die Richtlinie ändern, wird ein bereits gesperrtes Benutzerkonto nicht entsperrt.

## <a name="default-fine-grained-password-policy-settings"></a>Standardmäßige Einstellungen für differenzierte Kennwortrichtlinien

In einer verwalteten Azure AD DS-Domäne werden die folgenden Kennwortrichtlinien standardmäßig konfiguriert und auf alle Benutzer angewendet:

* **Minimale Kennwortlänge (Zeichen):** 7
* **Maximales Kennwortalter (Gültigkeitsdauer):** 90 Tage
* **Kennwörter müssen den Anforderungen an die Komplexität entsprechen**

Die folgenden Kontosperrungsrichtlinien sind standardmäßig konfiguriert:

* **Kontosperrungsdauer:** 30
* **Zulässige Anzahl fehlerhafter Anmeldeversuche:** 5
* **Anzahl fehlerhafter Anmeldeversuche zurücksetzen nach:** 30 Minuten

Bei diesen Standardeinstellungen werden Benutzerkonten für 30 Minuten gesperrt, wenn innerhalb von zwei Minuten fünf ungültige Kennwörter verwendet werden. Nach 30 Minuten werden die Konten automatisch wieder entsperrt.

Sie können die standardmäßige integrierte differenzierte Kennwortrichtlinie nicht ändern oder löschen. Mitglieder der Gruppe *AAD DC-Administratoren* können eine benutzerdefinierte FGPP erstellen und so konfigurieren, dass sie die integrierte Standard-FGPP außer Kraft setzt, wie im nächsten Abschnitt erläutert wird.

## <a name="create-a-custom-fine-grained-password-policy"></a>Erstellen einer benutzerdefinierten differenzierten Kennwortrichtlinie

Wenn Sie Anwendungen in Azure erstellen, sollten Sie eine benutzerdefinierte FGPP konfigurieren. Beispielsweise kann es erforderlich sein, eine benutzerdefinierte FGPP zu erstellen, damit eine andere Kontosperrungsrichtlinie festgelegt werden kann oder um für die verwaltete Domäne eine Standardeinstellung für die Kennwortgültigkeitsdauer zu konfigurieren.

Sie können eine benutzerdefinierte FGPP erstellen und auf bestimmte Gruppen in der verwalteten Azure AD DS-Domäne anwenden. Diese Konfiguration überschreibt effektiv die Standard-FGPP. Sie können auch benutzerdefinierte differenzierte Kennwortrichtlinien erstellen und auf benutzerdefinierte Organisationseinheiten anwenden, die Sie in der verwalteten Azure AD DS-Domäne erstellen.

Um eine differenzierte Kennwortrichtlinie zu erstellen, verwenden Sie die Active Directory-Verwaltungstools auf einer in die Domäne eingebundenen VM. Mit dem Active Directory-Verwaltungscenter können Sie Ressourcen in einer verwalteten Azure AD DS-Domäne anzeigen, bearbeiten und erstellen, einschließlich Organisationseinheiten.

> [!NOTE]
> Zum Erstellen einer differenzierten Kennwortrichtlinie in einer verwalteten Azure AD DS-Domäne müssen Sie bei einem Benutzerkonto angemeldet sein, das Mitglied der Gruppe *AAD DC-Administrators* ist.

1. Klicken Sie auf dem Startbildschirm auf **Verwaltung**. Es wird eine Liste der verfügbaren Verwaltungstools angezeigt, die im Tutorial zum [Erstellen eines virtuellen Verwaltungscomputers][tutorial-create-management-vm] installiert wurden.
1. Wählen Sie zum Erstellen und Verwalten von Organisationseinheiten **Active Directory-Verwaltungscenter** aus der Liste der Verwaltungstools aus.
1. Wählen Sie im linken Bereich die verwaltete Azure AD DS-Domäne aus, z. B. *contoso.com*.
1. Klicken Sie im Bereich **Tasks** auf der rechten Seite auf **Neu > Kennworteinstellungen**.
1. Geben Sie im Dialogfeld **Kennworteinstellungen erstellen** einen Namen für die Richtlinie ein, z. B. *MyCustomFGPP*. Legen Sie die Rangfolge entsprechend fest, um die Standard-FGPP *200* außer Kraft zu setzen, z. B. *1*.

    Bearbeiten Sie ggf. andere Kennwortrichtlinien-Einstellungen wie **Kennwortchronik erzwingen**, damit der Benutzer ein Kennwort erstellen muss, das sich von den letzten *24* Kennwörtern unterscheidet.

    ![Erstellen einer benutzerdefinierten differenzierten Kennwortrichtlinie](./media/how-to/custom-fgpp.png)

1. Deaktivieren Sie **Vor versehentlichem Löschen schützen**. Wenn diese Option aktiviert ist, kann die FGPP nicht gespeichert werden.
1. Wählen Sie im Abschnitt **Direkt anwendbar auf** die Schaltfläche **Hinzufügen** aus. Klicken Sie im Dialogfeld **Benutzer oder Gruppen auswählen** auf die Schaltfläche **Speicherorte**.

    ![Auswählen von Benutzern und Gruppen, auf die die Kennwortrichtlinie angewendet werden soll](./media/how-to/fgpp-applies-to.png)

1. Differenzierte Kennwortrichtlinien (FGPP) können nur auf Gruppen angewendet werden. Erweitern Sie im Dialogfeld **Speicherorte** den Domänennamen, z. B. *contoso.com*, und wählen Sie dann eine Organisationseinheit aus, z. B. **AADDC-Benutzer**. Wenn Sie über eine benutzerdefinierte Organisationseinheit verfügen, die eine Gruppe von Benutzern enthält, die Sie anwenden möchten, wählen Sie diese Organisationseinheit aus.

    ![Auswählen der Organisationseinheit, zu der die Gruppe gehört](./media/how-to/fgpp-container.png)

1. Geben Sie den Namen der Gruppe ein, auf die die Richtlinie angewendet werden soll, und klicken Sie dann auf **Namen überprüfen**, um sicherzustellen, dass die Gruppe vorhanden ist.

    ![Suchen und Auswählen der Gruppe zum Anwenden der FGPP](./media/how-to/fgpp-apply-group.png)

1. Der Name der ausgewählten Gruppe wird jetzt im Abschnitt **Direkt anwendbar auf** angezeigt. Wählen Sie **OK** aus, um die benutzerdefinierte Kennwortrichtlinie zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu differenzierten Kennwortrichtlinien und zum Verwenden des Active Directory-Verwaltungscenters finden Sie in den folgenden Artikeln:

* [Informationen über differenzierte Kennwortrichtlinien](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [Konfigurieren differenzierter Kennwortrichtlinien mit dem Active Directory-Verwaltungscenter](/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
