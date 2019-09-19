---
title: 'Azure Active Directory Domain Services: Problembehandlung bei Netzwerksicherheitsgruppen | Microsoft-Dokumentation'
description: Problembehandlung bei der Konfiguration von Netzwerksicherheitsgruppen für Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 450ee5635b378ed7c4d4e4bedc1c4245f6b52d70
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "70743419"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Beheben von Problemen bei einer ungültigen Netzwerkkonfiguration für verwaltete Domänen
Dieser Artikel hilft Ihnen bei der Problembehandlung und Lösung von Konfigurationsfehlern im Zusammenhang mit dem Netzwerk, die zu der folgenden Fehlermeldung führen:

## <a name="alert-aadds104-network-error"></a>Warnung AADDS104: Netzwerkfehler
**Warnmeldung:** *Microsoft kann nicht auf die Domänencontroller für diese verwaltete Domäne zugreifen. Dies kann geschehen, wenn eine für Ihr virtuelles Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) den Zugriff auf ihre verwaltete Domäne blockiert. Eine weitere mögliche Ursache ist eine benutzerdefinierte Route, die eingehenden Datenverkehr aus dem Internet blockiert.*

Die häufigste Ursache von Netzwerkfehlern für Azure AD Domain Services sind ungültige NSG-Konfigurationen. Die für das virtuelle Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) muss den Zugriff auf [bestimmte Ports](network-considerations.md#network-security-groups-and-required-ports) zulassen. Wenn diese Ports blockiert sind, kann Microsoft die verwaltete Domäne nicht überwachen oder aktualisieren. Darüber hinaus wird die Synchronisierung zwischen Ihrem Azure AD-Verzeichnis und der verwalteten Domäne beeinträchtigt. Halten Sie diese Ports während der Erstellung Ihrer NSG offen, um Dienstunterbrechungen zu vermeiden.

### <a name="checking-your-nsg-for-compliance"></a>Überprüfen der NSG auf Konformität

1. Navigieren Sie zur Seite [Netzwerksicherheitsgruppen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) im Azure-Portal.
2. Wählen Sie in der Tabelle die NSG aus, die dem Subnetz zugeordnet ist, in dem Ihre verwaltete Domäne aktiviert ist.
3. Klicken Sie im linken Bereich unter **Einstellungen** auf **Eingangssicherheitsregeln**.
4. Überprüfen Sie die bestehenden Regeln, und finden Sie heraus, welche Regeln den Zugriff auf [diese Ports](network-considerations.md#network-security-groups-and-required-ports) blockieren.
5. Bearbeiten Sie die NSG, um Konformität sicherzustellen, indem Sie entweder die Regel löschen, eine Regel hinzufügen oder eine komplett neue NSG erstellen. Schritte zum [Hinzufügen einer Regel](#add-a-rule-to-a-network-security-group-using-the-azure-portal) und zum Erstellen einer neuen, konformen NSG finden Sie weiter unten.

## <a name="sample-nsg"></a>NSG-Beispiel
Die folgende Tabelle zeigt ein NSG-Beispiel, das den Schutz Ihrer verwalteten Domäne sicherstellt und es Microsoft zugleich ermöglicht, Informationen zu überwachen, zu verwalten und zu aktualisieren.

![NSG-Beispiel](./media/active-directory-domain-services-alerts/default-nsg.png)

>[!NOTE]
> Für Azure AD Domain Services ist uneingeschränkter ausgehender Zugriff vom virtuellen Netzwerk erforderlich. Es wird empfohlen, keine zusätzliche NSG-Regel zu erstellen, die den ausgehenden Zugriff für das virtuelle Netzwerk beschränkt.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Hinzufügen einer Regel zu einer Netzwerksicherheitsgruppe mithilfe des Azure-Portals
Wenn Sie PowerShell nicht verwenden möchten, können Sie den NSGs mithilfe des Azure-Portals manuell einzelne Regeln hinzufügen. Führen Sie die folgenden Schritte aus, um Regeln in Ihrer Netzwerksicherheitsgruppe zu erstellen:

1. Navigieren Sie zur Seite [Netzwerksicherheitsgruppen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) im Azure-Portal.
2. Wählen Sie in der Tabelle die NSG aus, die dem Subnetz zugeordnet ist, in dem Ihre verwaltete Domäne aktiviert ist.
3. Klicken Sie im linken Bereich unter **Einstellungen** entweder auf **Eingangssicherheitsregeln** oder auf **Ausgangssicherheitsregeln**.
4. Erstellen Sie die Regel, indem Sie auf **Hinzufügen** klicken und die erforderlichen Informationen einsetzen. Klicken Sie auf **OK**.
5. Überprüfen Sie, ob Ihre Regel erstellt wurde, indem Sie sie in der Tabelle der Regeln suchen.


## <a name="need-help"></a>Sie brauchen Hilfe?
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](contact-us.md).
