---
title: Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps mithilfe eines Azure AD-Anwendungsproxys | Microsoft-Dokumentation
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3fa5c5638da390f4416afc9f4bd9c5d58c34cea8
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825570"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps mithilfe eines Azure AD-Anwendungsproxys

Dieser Artikel erläutert das Konfigurieren einer App, sodass ein Benutzer auf eine benutzerdefinierte Homepage weitergeleitet wird. Dieses kann sich danach unterscheiden, ob es sich um einen internen oder externen Benutzer handelt. Wenn Sie eine App mit dem Anwendungsproxy veröffentlichen, legen Sie eine interne URL fest. Dies ist jedoch in einigen Fällen nicht die Seite, die den Benutzern zuerst angezeigt werden soll. Legen Sie eine benutzerdefinierte Startseite fest, sodass die Benutzer beim Zugreifen auf die App zur richtigen Seite gelangen. Benutzern wird die benutzerdefinierte Startseite angezeigt, die Sie festlegen, egal ob sie über den Azure Active Directory-Zugriffsbereich oder den Office 365-App-Starter auf die App zugreifen.

Wenn ein Benutzer die App startet, wird er standardmäßig zur Stammdomänen-URL der veröffentlichten App weitergeleitet. Die Zielseite wird in der Regel als URL der Startseite festgelegt. Definieren Sie mithilfe des Azure AD-PowerShell-Moduls eine benutzerdefinierte Homepage-URL, wenn App-Benutzer zu einer bestimmten Seite innerhalb der App gelangen sollen.

Hier wird in einem Szenario begründet, warum Ihr Unternehmen eine benutzerdefinierte Homepage festlegt, die je nach den Benutzertyp unterschiedlich ausfällt:

- Da Sie auf oberster Ebene der Ordnerstruktur über andere Ressourcen verfügen, auf die der Anwendungsproxy zugreifen muss (z. B. Images), veröffentlichen Sie die App mit `https://ExpenseApp` als interner URL.
- In Ihrem Unternehmensnetzwerk navigiert ein Benutzer jedoch zu `https://ExpenseApp/login/login.aspx`, um sich anzumelden und auf Ihre App zuzugreifen.
- Die externe Standard-URL ist `https://ExpenseApp-contoso.msappproxy.net`. Über diese gelangen die Benutzer jedoch nicht auf die Anmeldeseite.
- Sie möchten `https://ExpenseApp-contoso.msappproxy.net/login/login.aspx` als URL der externen Homepage festlegen, damit externen Benutzern zuerst die Anmeldeseite angezeigt wird.

>[!NOTE]
>Wenn Sie Benutzern Zugriff auf veröffentlichte Apps erteilen, werden die Apps im [Azure AD-Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md) und im [Office 365-Startfeld](https://www.microsoft.com/microsoft-365/blog/2016/09/27/introducing-the-new-office-365-app-launcher/) angezeigt.

## <a name="before-you-start"></a>Vorbereitung

Bevor Sie die URL der Homepage festlegen, berücksichtigen Sie die folgenden Anforderungen:

- Bei dem angegebenen Pfad muss es sich um einen Unterdomänenpfad der Stammdomänen-URL handeln.

  Wenn die URL der Stammdomäne beispielsweise `https://apps.contoso.com/app1/` lautet, muss die konfigurierte Homepage-URL mit `https://apps.contoso.com/app1/` beginnen.

- Nach einer Änderung der veröffentlichten App wird der Wert der Startseiten-URL unter Umständen zurückgesetzt. Wenn Sie die App in Zukunft aktualisieren, sollten Sie die URL der Startseite erneut überprüfen und bei Bedarf aktualisieren.

Sie können die externe oder interne Homepage über das Azure-Portal oder mithilfe von PowerShell ändern.

## <a name="change-the-home-page-in-the-azure-portal"></a>Ändern der Startseite im Azure-Portal

Um die externe und die interne Startseite Ihrer App über das Azure AD-Portal zu ändern, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure Active Directory-Portal](https://aad.portal.azure.com/) an. Das Dashboard für Azure Active Directory Admin Center wird angezeigt.
2. Wählen Sie auf der Seitenleiste **Azure Active Directory** aus. Die Übersichtsseite für Azure AD wird angezeigt.
3. Wählen Sie auf der Seitenleiste der Übersicht die Option **App-Registrierungen** aus. Die Liste der registrierten Apps wird angezeigt.
4. Wählen Sie Ihre App in der Liste aus. Eine Seite mit den Details der registrierten App wird angezeigt.
5. Wählen Sie den Link unter **Umleitungs-URIs** aus, der die Anzahl der Umleitungs-URIs für Web- und öffentliche Clients anzeigt. Die Authentifizierungsseite für die registrierte App wird angezeigt.
6. Legen Sie in der letzten Zeile der Tabelle **Umleitungs-URIs** die Spalte **TYP** auf **Public client (mobile & desktop)** (Öffentlicher Client (mobil und Desktop)) fest, und geben Sie in der Spalte **UMLEITUNGS-URI** die interne URL ein, die Sie verwenden möchten. Eine neue leere Zeile wird unter der gerade geänderten Zeile angezeigt.
7. Legen Sie in der neuen Zeile die Spalte **TYP** auf **Web** fest, und geben Sie in der Spalte **UMLEITUNGS-URI** die externe URL ein, die Sie verwenden möchten.
8. Wenn Sie vorhandene Zeilen für Umleitungs-URIs löschen möchten, wählen Sie das Symbol **Löschen** (Mülleimer) neben den einzelnen unerwünschten Zeilen aus.
9. Wählen Sie **Speichern** aus.

## <a name="change-the-home-page-with-powershell"></a>Ändern der Startseite mit PowerShell

Um die Homepage einer App über PowerShell zu konfigurieren, gehen Sie folgendermaßen vor:

1. Installieren Sie das Azure AD PowerShell-Modul.
2. Suchen Sie den Wert ObjectId der App.
3. Aktualisieren Sie die URL der Homepage für die App mit PowerShell-Befehlen.

### <a name="install-the-azure-ad-powershell-module"></a>Installieren des Azure AD PowerShell-Moduls

Bevor Sie mithilfe von PowerShell eine benutzerdefinierte Homepage-URL definieren, installieren Sie das Azure AD PowerShell-Modul. Das entsprechende Paket können Sie aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureAD/2.0.2.16) herunterladen. Dieser verwendet den Graph-API-Endpunkt.

Führen Sie diese Schritte aus, um das Paket zu installieren:

1. Öffnen Sie ein standardmäßiges PowerShell-Fenster, und führen Sie dann den folgenden Befehl aus:

   ```powershell
   Install-Module -Name AzureAD
   ```

    Wenn Sie den Befehl als Benutzer ohne Administratorrechte ausführen, verwenden Sie die Option `-scope currentuser`.

2. Wählen Sie während der Installation **J** aus, um zwei Pakete von „Nuget.org“ zu installieren. Beide Pakete sind erforderlich.

### <a name="find-the-objectid-of-the-app"></a>Ermitteln der Objekt-ID der App

Sie erhalten die Objekt-ID der App, indem Sie nach dem Anzeigenamen oder der Homepage der App suchen.

1. Importieren Sie in demselben PowerShell-Fenster das Azure AD-Modul.

   ```powershell
   Import-Module AzureAD
   ```

2. Melden Sie sich beim Azure AD-Modul als Mandantenadministrator an.

   ```powershell
   Connect-AzureAD
   ```

3. Suchen Sie die App. In diesem Beispiel wird die Objekt-ID mithilfe von PowerShell gesucht, indem nach der App mit dem Anzeigenamen `SharePoint` gesucht wird.

   ```powershell
   Get-AzureADApplication | Where-Object { $_.DisplayName -eq "SharePoint" } | Format-List DisplayName, Homepage, ObjectId
   ```

   Das Ergebnis sieht etwa wie das hier gezeigte aus. Kopieren Sie die GUID für die Objekt-ID zur Verwendung im nächsten Abschnitt.

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

   Alternativ können Sie auch einfach die Liste aller Apps abrufen, die Liste nach der App mit einem bestimmten Anzeigenamen oder einer bestimmten Homepage durchsuchen und die Objekt-ID der App anschließend kopieren.

   ```powershell
   Get-AzureADApplication | Format-List DisplayName, Homepage, ObjectId
   ```

### <a name="update-the-home-page-url"></a>Schritt 2: Aktualisieren der URL der Startseite

Erstellen Sie die URL der Startseite, und aktualisieren Sie die App mit diesem Wert. Fahren Sie im selben PowerShell-Fenster fort, oder wenn Sie stattdessen ein neues PowerShell-Fenster verwenden, melden Sie sich mit `Connect-AzureAD` erneut beim Azure AD-Modul an. Führen Sie dann die folgenden Schritte durch:

1. Erstellen Sie eine Variable für den Objekt-ID-Wert, den Sie im vorherigen Abschnitt kopiert haben. (Ersetzen Sie den Objekt-ID-Wert in diesem SharePoint-Beispiel durch die Objekt-ID Ihrer App.)

   ```powershell
   $objguid = "8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4"
   ```

2. Vergewissern Sie sich, dass die richtige App vorliegt, indem Sie folgenden Befehl ausführen. Die Ausgabe sollte mit der im vorherigen Abschnitt angezeigten Ausgabe ([Ermitteln der Objekt-ID der App](#find-the-objectid-of-the-app)) übereinstimmen.

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

3. Erstellen Sie ein leeres Anwendungsobjekt, um die gewünschten Änderungen zu speichern.

   ```powershell
   $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
   ```

4. Legen Sie die URL der Startseite auf den gewünschten Wert fest. Der Wert muss ein Unterdomänenpfad der veröffentlichten App sein. Wenn Sie die URL der Startseite also beispielsweise von `https://sharepoint-iddemo.msappproxy.net/` in `https://sharepoint-iddemo.msappproxy.net/hybrid/` ändern, werden App-Benutzer direkt zur benutzerdefinierten Startseite weitergeleitet.

   ```powershell
   $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
   ```

5. Nehmen Sie das Update der Homepage vor.

   ```powershell
   Set-AzureADApplication -ObjectId $objguid -Homepage $homepage
   ```

6. Um zu bestätigen, dass die Änderung erfolgreich war, führen Sie den folgenden Befehl aus Schritt 2 erneut aus.

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

   In unserem Beispiel sollte die Ausgabe der folgenden gleichen:

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/hybrid/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

7. Starten Sie die App neu, um zu bestätigen, dass die Homepage wie erwartet als erster Bildschirm angezeigt wird.

>[!NOTE]
>Sämtliche Änderungen, die Sie an der App vornehmen, können die URL der Startseite zurücksetzen. Wenn die URL der Startseite zurückgesetzt wird, wiederholen Sie die Schritte in diesem Abschnitt, um sie wiederherzustellen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-integrate-with-sharepoint-server.md)
- [Tutorial: Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure Active Directory](application-proxy-add-on-premises-application.md)