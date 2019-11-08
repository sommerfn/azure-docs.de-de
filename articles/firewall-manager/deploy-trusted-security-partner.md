---
title: Bereitstellen eines vertrauenswürdigen Sicherheitspartners für Azure Firewall Manager
description: Erfahren Sie, wie Sie über das Azure-Portal einen vertrauenswürdigen Sicherheitspartner für Azure Firewall Manager erstellen.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: fe733b686f2b56beee26a6c33c4d6264d621e627
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511363"
---
# <a name="deploy-a-trusted-security-partner-preview"></a>Bereitstellen eines vertrauenswürdigen Sicherheitspartners (Vorschau)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Mit dem Feature *Vertrauenswürdiger Sicherheitspartner* in Azure Firewall Manager können Sie Ihre vertrauten erstklassigen SECaaS-Angebote (Security-as-a-Service) von Drittanbietern verwenden, um den Internetzugriff für Ihre Benutzer zu schützen.

Weitere Informationen zu unterstützten Szenarien und Best Practices finden Sie unter [Was sind vertrauenswürdige Sicherheitspartner (Vorschau)?](trusted-security-partners.md).

Die für diese Vorschau unterstützten Sicherheitspartner sind **ZScaler** und **iboss**. Die unterstützten Regionen sind „USA, Westen-Mitte“, „USA, Norden-Mitte“, „USA, Westen“, „USA, Westen 2“ und „USA, Osten“.

## <a name="prerequisites"></a>Voraussetzungen

> [!IMPORTANT]
> Azure Firewall Manager (Vorschau) muss explizit mithilfe des PowerShell-Befehls `Register-AzProviderFeature` aktiviert werden.

Führen Sie an einer PowerShell-Eingabeaufforderung die folgenden Befehle aus:

```azure-powershell
connect-azaccount
Register-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network
```
Es dauert bis zu 30 Minuten, bis die Featureregistrierung abgeschlossen ist. Führen Sie den folgenden Befehl aus, um den Registrierungsstatus zu überprüfen:

`Get-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network`

## <a name="deploy-a-third-party-security-provider-in-a-new-hub"></a>Bereitstellen eines Drittanbieters für die Sicherheit in einem neuen Hub

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
2. Geben Sie im Feld **Suche** den Text **Firewall Manager** ein, und wählen Sie diesen Eintrag unter **Dienste** aus.
3. Navigieren Sie zu **Erste Schritte**. Wählen Sie **Geschützten virtuellen Hub erstellen** aus. 
4. Geben Sie Ihr Abonnement und Ihre Ressourcengruppe ein, wählen Sie eine unterstützte Region aus, und fügen Sie Informationen zu Ihrem Hub und Virtual WAN hinzu. 
5. **VPN-Gateway bereitstellen** ist standardmäßig aktiviert. Zum Bereitstellen eines vertrauenswürdigen Sicherheitspartners im Hub ist ein VPN-Gateway erforderlich. 
6. Klicken Sie auf **Weiter: Azure Firewall**.
   > [!NOTE]
   > Vertrauenswürdige Sicherheitspartner stellen über VPN-Gateway-Tunnel eine Verbindung mit Ihrem Hub her. Wenn Sie das VPN-Gateway löschen, gehen die Verbindungen mit Ihren vertrauenswürdigen Sicherheitspartnern verloren.
7. Wenn Sie zusätzlich zu einer Sicherheitslösung eines Drittanbieters zum Filtern des Internetdatenverkehrs Azure Firewall bereitstellen möchten, um privaten Datenverkehr zu filtern, wählen Sie eine Richtlinie für Azure Firewall aus. Informationen dazu finden Sie in den [unterstützten Szenarien](trusted-security-partners.md#key-scenarios).
8. Wenn Sie nur die Sicherheitslösung eines Drittanbieters im Hub bereitstellen möchten, wählen Sie Folgendes aus: **Azure Firewall: aktiviert/deaktiviert**, und legen Sie die Einstellung auf **Deaktiviert** fest. 
9. Klicken Sie auf **Weiter: Vertrauenswürdige Sicherheitspartner**
10. Wählen Sie **Vertrauenswürdiger Sicherheitspartner** aus, um die Einstellung auf **Aktiviert** festzulegen. Wählen Sie einen Partner aus. 
11. Klicken Sie auf **Weiter**. 
12. Überprüfen Sie den Inhalt, und klicken Sie dann auf **Erstellen**.

Die Bereitstellung des VPN-Gateways kann mehr als 30 Minuten in Anspruch nehmen.

Um zu überprüfen, ob das Hub erstellt wurde, navigieren Sie zu „Azure Firewall Manager“ > „Geschützte Hubs“. Wählen Sie den Hub und darin die Seite „Übersicht“ aus, um den Partnernamen und den Status **Sicherheitsverbindung ausstehend** anzuzeigen.

Wenn der Hub erstellt und der Sicherheitspartner eingerichtet wurde, stellen Sie eine Verbindung zwischen dem Sicherheitsanbieter und dem Hub her.

## <a name="deploy-a-third-party-security-provider-in-an-existing-hub"></a>Bereitstellen eines Drittanbieters für die Sicherheit in einem vorhandenen Hub

Sie können auch einen vorhandenen Hub in einem Virtual WAN auswählen und diesen in einen *geschützten virtuellen Hub* konvertieren.

1. Wählen Sie unter **Erste Schritte** die Option **Vorhandene Hubs konvertieren** aus.
2. Wählen Sie ein Abonnement und einen vorhandenen Hub aus. Führen Sie die restlichen Schritte in der Vorgehensweise zum Bereitstellen eines Drittanbieters in einem neuen Hub aus.

Denken Sie daran, dass ein VPN-Gateway bereitgestellt sein muss, um einen vorhandenen Hub in einen geschützten Hub mit Drittanbietern zu konvertieren.

## <a name="configure-third-party-security-providers-to-connect-to-a-secured-hub"></a>Konfigurieren von Drittanbietern für die Sicherheit zum Herstellen einer Verbindung mit einem geschützten Hub

Um Tunnel zum VPN-Gateway Ihres virtuellen Hubs einzurichten, benötigen Drittanbieter Zugriffsrechte für Ihren Hub. Weisen Sie zu diesem Zweck einen Dienstprinzipal mit Ihrem Abonnement oder Ihrer Ressourcengruppe zu, und gewähren Sie diesem Zugriffsrechte. Diese Anmeldeinformationen müssen Sie dann dem Drittanbieter über dessen Portal mitteilen.

1. Erstellen Sie einen Azure Active Directory-Dienstprinzipal: Sie können die Umleitungs-URL überspringen. 

   [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)
2. Fügen Sie Zugriffsrechte und einen Bereich für den Dienstprinzipal hinzu.
   [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)

   > [!NOTE]
   > Zur genaueren Steuerung können Sie den Zugriff auf Ihre Ressourcengruppe beschränken.
3. Befolgen Sie die Anweisungen im folgenden Link.

   - Melden Sie sich beim Partnerportal an, und fügen Sie Ihre Anmeldeinformationen hinzu, um dem vertrauenswürdigen Partner Zugriff auf Ihren geschützten Hub zu gewähren.
   - Wenn Ihre Azure AD-Authentifizierungsinformationen validiert wurden, führen Sie die folgenden Anweisungen aus, um die virtuellen Hubs im Partnerportal zu synchronisieren und den Tunnel zum virtuellen Hub einzurichten.

   [ZScaler: Configuring a Microsoft Azure Virtual WAN Integration](https://help.zscaler.com/zia/configuring-microsoft-azure-virtual-wan-integration) (Konfigurieren einer Microsoft Azure Virtual WAN-Integration)
4. Im Azure Virtual WAN-Portal in Azure können Sie den Status der Tunnelerstellung anzeigen. Sobald die Tunnel sowohl im Azure-Portal als auch im Partnerportal **verbunden** anzeigen, fahren Sie mit den nächsten Schritten fort, um Routen einzurichten, mit denen ausgewählt wird, welche Zweigstellen und VNETs Internetdatenverkehr an den Partner senden sollen.

## <a name="configure-route-settings"></a>Konfigurieren von Routeneinstellungen

1. Navigieren Sie zu „Azure Firewall Manager“ > „Geschützte Hubs“. 
2. Wählen Sie einen Hub aus. Der Hubstatus sollte jetzt als **Bereitgestellt** angezeigt werden, nicht mehr als **Sicherheitsverbindung ausstehend**.

   Stellen Sie sicher, dass der Drittanbieter eine Verbindung mit dem Hub herstellen kann. Die Tunnel im VPN-Gateway sollten den Status **Verbunden** aufweisen. Dieser Status spiegelt die Verbindungsintegrität zwischen Hub und Drittanbieter besser wider als der vorherige Status.
3. Wählen Sie den Hub aus, und navigieren Sie zu **Routeneinstellungen**.

   Wenn Sie einen Drittanbieter im Hub bereitstellen, wird der Hub in einen *geschützten virtuellen Hub* konvertiert. So ist sichergestellt, dass der Drittanbieter eine (standardmäßige) Route von 0.0.0.0/0 an den Hub sendet. VNET-Verbindungen und mit dem Hub verbundene Standorte erhalten diese Route jedoch nicht, es sei denn, Sie wählen die Verbindungen aus, die diese Standardroute erhalten sollen.
4. Wählen Sie unter **Internetdatenverkehr** die Option **VNET-to-Internet** oder **Branch-to-Internet** oder beide aus, sodass Routen zum Senden über den Drittanbieter konfiguriert sind.

   Hiermit wird nur angegeben, welche Art von Datenverkehr an den Hub geleitet werden soll, es gibt noch keine Auswirkungen auf die Routen in VNETs oder Zweigstellen. Diese Routen werden nicht standardmäßig an alle VNETs/Zweigstellen weitergegeben, die an den Hub angeschlossen sind.
5. Sie müssen auf **sichere Verbindungen** klicken und diejenigen Verbindungen auswählen, für die diese Routen festgelegt werden sollen. Dies gibt an, welche VNETs oder Zweigstellen Internetdatenverkehr an den Drittanbieter senden dürfen.
6. Wählen Sie bei den **Routeneinstellungen** unter „Internetdatenverkehr“ die Option **Sichere Verbindungen** aus, und wählen Sie dann das VNET oder die Zweigstellen (*Standorte* in Virtual WAN) aus, die gesichert werden sollen. Wählen Sie **Sicherer Internetdatenverkehr** aus.
   ![Sicherer Internetdatenverkehr](media/deploy-trusted-security-partner/secure-internet-traffic.png)
7. Kehren Sie zur Seite „Hubs“ zurück. Der Status **Vertrauenswürdiger Sicherheitspartner** im Hub sollte jetzt als **Gesichert** angezeigt werden.

## <a name="branch-or-vnet-internet-traffic-via-third-party-service"></a>Zweigstellen- oder VNET-Internetdatenverkehr über Drittanbieterdienst

Als Nächstes können Sie überprüfen, ob die virtuellen Computer im VNET oder am Zweigstellenstandort auf das Internet zugreifen können und ob der Datenverkehr an den Drittanbieterdienst geleitet wird.

Nachdem die Schritte zur Routenfestlegung ausgeführt wurden, wird sowohl an die virtuellen VNET-Computer als auch an die Zweigstellenstandorte eine 0/0-Route zum Drittanbieterdienst gesendet. Sie können keine RDP- oder SSH-Verbindung mit diesen virtuellen Computern herstellen. Für die Anmeldung können Sie den [Azure Bastion](../bastion/bastion-overview.md)-Dienst in einem Peering-VNET bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager (Vorschauversion) unter Verwendung des Azure-Portals](secure-cloud-network.md)




