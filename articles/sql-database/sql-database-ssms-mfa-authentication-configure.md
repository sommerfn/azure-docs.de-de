---
title: Konfigurieren der mehrstufigen Authentifizierung
description: Erfahren Sie, wie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse verwendet wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/27/2019
ms.openlocfilehash: 7849f6d391cad7e973babfa97f2859b9df40f23e
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820897"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Konfigurieren der mehrstufigen Authentifizierung für SQL Server Management Studio und Azure AD

In diesem Thema wird beschrieben, wie Sie die mehrstufige Authentifizierung von Azure Active Directory mit SQL Server Management Studio verwenden. Die mehrstufige Azure AD-Authentifizierung (MFA) kann bei Verbindungen von SSMS oder SqlPackage.exe mit Azure [SQL-Datenbank](sql-database-technical-overview.md) und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) verwendet werden. Einen Überblick über Multi-Factor Authentication in Azure SQL-Datenbank finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Dieses Thema gilt für Azure SQL-Server sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Server erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

## <a name="configuration-steps"></a>Konfigurationsschritte

1. **Konfigurieren eines Azure Active Directory-Verzeichnisses:** Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [Hinzufügen eigener Domänennamen zu Azure AD](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure now supports federation with Windows Server Active Directory (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory)](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/) und [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurieren der mehrstufigen Authentifizierung:** Eine ausführliche Anleitung finden Sie unter [Was ist mehrstufige Azure-Authentifizierung?](../active-directory/authentication/multi-factor-authentication.md) und [Bedingter Zugriff (mehrstufige Authentifizierung) mit Azure SQL-Datenbank und Data Warehouse](sql-database-conditional-access.md). (Für eine vollständige Zugangsberechtigung ist Azure Active Directory (Azure AD) im Tarif Premium erforderlich. Begrenzte mehrstufige Authentifizierung ist mit Azure AD im Tarif Standard verfügbar.)
3. **Konfigurieren von SQL-Datenbank oder SQL Data Warehouse für die Azure AD-Authentifizierung**: Eine ausführliche Anleitung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
4. **Herunterladen von SSMS:** Laden Sie die neueste SSMS-Version von [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) auf den Clientcomputer herunter. Verwenden Sie für alle Features in diesem Thema mindestens die Version von Juli 2017, Version 17.2.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Herstellen einer Verbindung mit SSMS mithilfe der universellen Authentifizierung

Die folgenden Schritte zeigen, wie Sie unter Verwendung der neuesten Version von SSMS eine Verbindung mit SQL-Datenbank oder SQL Data Warehouse herstellen.

1. Um eine Verbindung mithilfe der universellen Authentifizierung herzustellen, wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** die Option **Active Directory: universell mit MFA-Unterstützung** aus. (Wenn **Universelle Active Directory-Authentifizierung** angezeigt wird, verwenden Sie nicht die aktuelle Version von SSMS.)  
   ![1mfa-universal-connect][1]  
2. Geben Sie im Feld **Benutzername** die Azure Active Directory-Anmeldeinformationen im Format `user_name@domain.com` ein.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Wenn Sie als Gastbenutzer eine Verbindung herstellen, müssen Sie das Feld „AD-Domänenname“ oder „Mandanten-ID“ für Gastbenutzer nicht mehr ausfüllen, da es von SSMS 18. x oder höher automatisch erkannt wird. Weitere Informationen finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).
   ![mfa-no-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-no-tenant-ssms.png)

   Wenn Sie jedoch eine Verbindung als Gastbenutzer mit SSMS 17.x oder früher herstellen, müssen Sie auf **Optionen** klicken und dann im Dialogfeld **Verbindungseigenschaften** das Feld **AD-Domänenname oder Mandanten-ID** ausfüllen.
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)

4. Wie bei SQL-Datenbank und SQL Data Warehouse üblich, müssen Sie auf **Optionen** klicken und im Dialogfeld **Optionen** die Datenbank angeben. (Wenn der verbundene Benutzer ein Gastbenutzer ist (d.h. joe@outlook.com), müssen Sie das Kontrollkästchen aktivieren und den aktuellen AD-Domänennamen oder die Mandanten-ID als Teil der Optionen hinzufügen. Siehe [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md). Klicken Sie auf **Verbinden**.  
5. Wenn das Dialogfeld **Bei Ihrem Konto anmelden** angezeigt wird, geben Sie den Kontonamen und das Kennwort Ihrer Azure Active Directory-Identität ein. Es ist kein Kennwort erforderlich, wenn ein Benutzer Mitglied einer Domäne im Verbund mit Azure AD ist.  
   ![2mfa-sign-in][2]  

   > [!NOTE]
   > Bei der universellen Authentifizierung mit einem Konto, das keine MFA erfordert, stellen Sie an diesem Punkt die Verbindung her. Fahren Sie für Benutzer, für die MFA erforderlich ist, mit den folgenden Schritten fort:
   >  
   
6. Es werden möglicherweise zwei Dialogfelder für die Einrichtung von MFA angezeigt. Dieser einmalige Vorgang richtet sich nach der Administratoreinstellung für MFA und ist daher möglicherweise optional. In einer Domäne mit aktivierter MFA ist dieser Schritt manchmal vordefiniert (beispielsweise muss ein Benutzer sich in der Domäne möglicherweise mit einer Smartcard und einer PIN authentifizieren).  
   ![3mfa-setup][3]  
7. Das zweite möglicherweise einmalig angezeigte Dialogfeld ermöglicht es Ihnen, die Details Ihrer Authentifizierungsmethode auszuwählen. Die möglichen Optionen werden von Ihrem Administrator konfiguriert.  
   ![4mfa-verify-1][4]  
8. Das Azure Active Directory-Verzeichnis sendet die Bestätigungsinformationen an Sie. Wenn Sie den Prüfcode erhalten haben, geben Sie ihn in das Feld **Prüfcode eingeben** ein, und klicken Sie auf **Anmelden**.  
   ![5mfa-verify-2][5]  

Wenn die Überprüfung abgeschlossen ist, stellt SSMS normalerweise eine Verbindung her, vorausgesetzt, die Anmeldeinformationen sind gültig und der Firewallzugriff ist möglich.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die mehrstufige Authentifizierung in Azure SQL-Datenbank finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).  
- Gewähren Sie anderen Benutzern Zugriff auf Ihre Datenbank: [SQL-Datenbankauthentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md)  
- Stellen Sie sicher, dass andere Benutzer über die Firewall eine Verbindung herstellen können: [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)  
- Bei Verwendung der Authentifizierung **Active Directory: universell mit MFA-Unterstützung** ist ab [SSMS 17.3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) die ADAL-Ablaufverfolgung verfügbar. Die ADAL-Ablaufverfolgung ist standardmäßig deaktiviert. Sie können sie aktivieren, indem Sie unter **Azure-Dienste** > **Azure-Cloud** > **Ablaufverfolgungsebene für ADAL-Ausgabefenster** im Menü **Optionen** die **Tools** verwenden und im Menü **Ansicht** die Option **Ausgabe** aktivieren. Die Ablaufverfolgungen im Ausgabefenster sind verfügbar, wenn Sie die Option **Azure Active Directory** aktivieren.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

