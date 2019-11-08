---
title: 'Beheben von Problemen mit einem VPN-Client für die Azure AD-Authentifizierung: Azure VPN Gateway | Microsoft-Dokumentation'
description: Troubleshooting für VPN Gateway-Clients mit Azure AD-Authentifizierung für Point-to-Site-Verbindungen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: e239f283d5065e032e98e87a72a4bfe9fbafbdb1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511651"
---
# <a name="troubleshoot-an-azure-ad-authentication-vpn-client"></a>Beheben von Problemen mit einem VPN-Client für die Azure AD-Authentifizierung

Dieser Artikel unterstützt Sie beim Beheben von Problemen mit einem VPN-Client für die Verbindung mit einem virtuellen Netzwerk über ein Point-to-Site-VPN mit Azure Active Directory-Authentifizierung.

## <a name="status"></a>Anzeigen des Statusprotokolls

Überprüfen Sie das Statusprotokoll auf Fehlermeldungen.

![logs](./media/troubleshoot-ad-vpn-client/1.png)

1. Klicken Sie in der rechten unteren Ecke des Clientfensters auf das Pfeilsymbol, um die **Statusprotokolle** anzuzeigen.
2. Überprüfen Sie die Protokolle auf Fehler, die möglicherweise auf das Problem hinweisen.
3. Fehlermeldungen werden rot dargestellt.

## <a name="clear"></a>Löschen von Anmeldeinformationen

Löschen Sie die Anmeldeinformationen.

![Anmelden](./media/troubleshoot-ad-vpn-client/2.png)

1. Wählen Sie die Auslassungszeichen (…) neben dem Profil aus, für das Sie Probleme beheben möchten. Wählen Sie **Konfigurieren-> Gespeichertes Konto löschen** aus.
2. Wählen Sie **Speichern** aus.
3. Versuchen Sie, eine Verbindung herzustellen.
4. Wenn die Verbindung immer noch fehlerhaft ist, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="diagnostics"></a>Ausführen von Diagnosen

Führen Sie Diagnosen im VPN-Client aus.

![Diagnose](./media/troubleshoot-ad-vpn-client/3.png)

1. Klicken Sie auf **...** . neben dem Profil, für das Sie eine Diagnose ausführen möchten. Wählen Sie **Diagnose-> Diagnose ausführen** aus.
2. Der Client führt eine Reihe von Tests durch und zeigt das Ergebnis des Tests an.

   * Internetzugriff: prüft, ob der Client über eine Internetverbindung verfügt.
   * Clientanmeldeinformationen: prüft, ob der Endpunkt für die Azure Active Directory-Authentifizierung erreichbar ist.
   * Server auflösbar: kontaktiert den DNS-Server, um die IP-Adresse des konfigurierten VPN-Servers aufzulösen.
   * Server erreichbar: prüft, ob der VPN-Server antwortet.
3. Wenn einer der Tests zu einem Fehler führt, wenden Sie sich an den Netzwerkadministrator, um das Problem zu beheben.
4. Im nächsten Abschnitt erfahren Sie, wie Sie bei Bedarf Protokolle sammeln.

## <a name="logfiles"></a>Sammeln von Protokolldateien des Clients

Sammeln Sie die Protokolldateien des VPN-Clients. Die Protokolldateien können über eine Methode Ihrer Wahl an den Support oder Ihren Administrator gesendet werden. Dies kann z. B. per E-Mail erfolgen.

1. Klicken Sie auf das Auslassungszeichen (…) neben dem Profil, für das Sie eine Diagnose ausführen möchten. Wählen Sie **Diagnose-> Protokollverzeichnis anzeigen** aus.

   ![Anzeigen von Protokollen](./media/troubleshoot-ad-vpn-client/4.png)
2. Der Ordner mit den Protokolldateien wird in Windows-Explorer geöffnet.

   ![Anzeigen von Dateien](./media/troubleshoot-ad-vpn-client/5.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory-Mandanten für P2S Open VPN-Verbindungen, die die Azure AD-Authentifizierung verwenden](openvpn-azure-ad-tenant.md).