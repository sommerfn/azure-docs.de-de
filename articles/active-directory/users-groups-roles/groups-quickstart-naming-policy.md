---
title: 'Schnellstart: Hinzufügen einer Benennungsrichtlinie für Office 365-Gruppen: Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie in Azure Active Directory neue Benutzer hinzugefügt oder bestehende gelöscht werden.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4105fa17041c7cefd1387d1ee50c177b8c55fc9
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651285"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Schnellstart: Benennungsrichtlinie für Gruppen in Azure Active Directory

In dieser Schnellstartanleitung richten Sie eine Benennungsrichtlinie in Ihrem Azure Active Directory-Mandanten (Azure AD) für von Benutzern erstellte Office 365-Gruppen ein, um das Sortieren und Durchsuchen der Gruppen in Ihrem Mandanten zu erleichtern. Sie können die Benennungsrichtlinie beispielsweise für Folgendes verwenden:

* Kommunizieren der Funktion einer Gruppe, die Mitgliedschaft, die geografische Region oder den Ersteller der Gruppe
* Kategorisieren von Gruppen im Adressbuch
* Blockieren bestimmter Wörter in Gruppennamen und Aliasen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="install-powershell-cmdlets"></a>Installieren von PowerShell-Cmdlets

Achten Sie darauf, dass Sie alle älteren Versionen von Azure Active Directory-PowerShell für das Graph-Modul für Windows PowerShell deinstallieren und [Azure Active Directory-PowerShell für Graph – öffentliche Vorschauversion 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) vor dem Ausführen der PowerShell-Befehle installieren. 

1. Führen Sie die Windows PowerShell-App als Administrator aus.
2. Deinstallieren Sie alle vorherigen Versionen von AzureADPreview.
  

   ```powershell
   Uninstall-Module AzureADPreview
   ```

3. Installieren Sie die neueste Version von AzureADPreview.
  

   ```powershell
   Install-Module AzureADPreview
   ```

   Wenn Sie über den Zugriff auf ein nicht vertrauenswürdiges Repository informiert werden, geben Sie **Y** ein. Es dauert möglicherweise einige Minuten, bis das neue Modul installiert wird.

## <a name="set-up-naming-policy"></a>Einrichten der Benennungsrichtlinie

### <a name="step-1-sign-in-using-powershell-cmdlets"></a>Schritt 1: Anmelden mit PowerShell-Cmdlets

1. Öffnen Sie die Windows PowerShell-App. Sie benötigen keine erhöhten Rechte.

2. Führen Sie die folgenden Befehle aus, um die Ausführung der Cmdlets vorzubereiten.
  

   ```powershell
   Import-Module AzureADPreview
   Connect-AzureAD
   ```

   Geben Sie auf dem angezeigten Bildschirm **Bei Ihrem Konto anmelden** Ihr Administratorkonto und das zugehörige Kennwort ein, um eine Verbindung mit dem Dienst herzustellen, und wählen Sie **Anmelden** aus.

3. Um Gruppeneinstellungen für diesen Mandanten zu erstellen, führen Sie die Schritte in [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](groups-settings-cmdlets.md) aus.

### <a name="step-2-view-the-current-settings"></a>Schritt 2: Anzeigen der aktuellen Einstellungen

1. Zeigen Sie die aktuellen Einstellungen der Benennungsrichtlinie an.
  

   ```powershell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id
   ```

  
2. Zeigen Sie die aktuellen Gruppeneinstellungen an.
  

   ```powershell
   $Setting.Values
   ```

  

### <a name="step-3-set-the-naming-policy-and-any-custom-blocked-words"></a>Schritt 3: Festlegen von Benennungsrichtlinie und benutzerdefinierten blockierten Wörtern

1. Legen Sie die Präfixe und Suffixe für Gruppennamen in Azure AD PowerShell fest. Für die ordnungsgemäße Funktion des Features muss [GroupName] in der Einstellung enthalten sein.
  

   ```powershell
   $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
   ```

  
2. Legen Sie benutzerdefinierte blockierte Wörter fest, deren Verwendung Sie einschränken möchten. Im folgenden Beispiel wird veranschaulicht, wie Sie Ihre eigenen benutzerdefinierten Wörter hinzufügen können.
  

   ```powershell
   $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
   ```

  
3. Speichern Sie die Einstellungen wie im folgenden Beispiel, damit die neue Richtlinie angewendet wird.
  

   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

  
Das ist alles. Sie haben Ihre Benennungsrichtlinie festgelegt und benutzerdefinierte blockierte Wörter hinzugefügt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

1. Leeren Sie die Präfixe und Suffixe für Gruppennamen in Azure AD PowerShell.
  

   ```powershell
   $Setting["PrefixSuffixNamingRequirement"] =""
   ```

  
2. Leeren Sie die benutzerdefinierten blockierten Wörter.
  

   ```powershell
   $Setting["CustomBlockedWordsList"]=""
   ```

  
3. Speichern Sie die Einstellungen.
  

   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mithilfe von PowerShell-Cmdlets die Benennungsrichtlinie für Ihren Azure AD-Mandanten festlegen.

Weitere Informationen zu technischen Einschränkungen, zum Hinzufügen einer Liste mit benutzerdefinierten blockierten Wörtern oder zu den Benutzeroberflächen von Endbenutzern in Office 365-Apps finden Sie im nächsten Artikel, der zudem weitere Einzelheiten zur Benennungsrichtlinie enthält.
> [!div class="nextstepaction"]
> [Erzwingen einer Benennungsrichtlinie für Office 365-Gruppen in Azure Active Directory (Vorschau)](groups-naming-policy.md)