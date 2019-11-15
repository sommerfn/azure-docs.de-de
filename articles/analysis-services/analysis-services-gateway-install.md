---
title: Installieren eines lokalen Datengateways für Azure Analysis Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein lokales Datengateway installieren und konfigurieren, um eine Verbindung zu lokalen Datenquellen von einem Azure Analysis Services-Server herzustellen.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 37658ea6b94d6369d282c49f79947a1f183b61c7
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73572853"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Installieren und Konfigurieren eines lokalen Datengateways

Ein lokales Datengateway ist erforderlich, wenn sich mindestens ein Azure Analysis Services-Server in der gleichen Region mit lokalen Datenquellen verbindet.  Auch wenn das installierte Gateway für alle weiteren Dienste – z.B. Power BI, Power Apps und Logic Apps – identisch ist, sind für Azure Analysis Services und Logic Apps einige zusätzliche Schritte erforderlich. Die Informationen in diesem Artikel gelten speziell für **Azure Analysis Services**.

Weitere Informationen zum Gateway und dessen Verwendung durch Azure Analysis Services finden Sie unter [Herstellen einer Verbindung mit lokalen Datenquellen über ein lokales Datengateway](analysis-services-gateway.md).

## <a name="prerequisites"></a>Voraussetzungen

**Mindestanforderungen.**

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7/Windows Server 2008 R2 (oder höher)

**Empfohlen.**

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

**Wichtige Hinweise:**

* Wenn sich Ihr Gateway während des Setups bei Azure registriert, wird die Standardregion für Ihr Abonnement ausgewählt. Sie können eine andere Region auswählen. Wenn Sie Server in mehreren Regionen haben, müssen Sie für jede Region ein Gateway installieren. 
* Das Gateway darf nicht auf einem Domänencontroller installiert werden.
* Auf einem einzelnen Computer kann nur ein Gateway installiert werden.
* Installieren Sie das Gateway auf einem Computer, der eingeschaltet bleibt und nicht in den Ruhezustand versetzt wird.
* Installieren Sie das Gateway nicht auf einem Computer, der über eine Drahtlosverbindung mit dem Netzwerk verfügt. Die Leistung kann beeinträchtigt werden.
* Wenn Sie das Gateway installieren, muss das Benutzerkonto, unter dem Sie auf Ihrem Computer angemeldet sind, über die Berechtigungen „Anmelden als Dienst“ verfügen. Wenn die Installation abgeschlossen ist, verwendet der lokale Datengatewaydienst das Konto „NT SERVICE\PBIEgwService“, um sich als Dienst anzumelden. Ein anderes Konto kann während des Setups oder nach dem Setup in „Dienste“ angegeben werden. Stellen Sie sicher, dass die Gruppenrichtlinieneinstellungen sowohl das Konto, mit dem Sie sich beim Installieren angemeldet haben, als auch das Dienstkonto, das Sie für die Berechtigungen „Anmelden als Dienst“ ausgewählt haben, zulassen.
* Melden Sie sich bei Azure an. Verwenden Sie dazu ein Konto in Azure AD mit dem gleichen [Mandanten](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant), der auch für das Abonnement verwendet wird, unter dem Sie das Gateway registrieren. Für das Installieren und Registrieren eines Gateways werden keine Azure B2B (Gast)-Konten unterstützt.
* Wenn sich die Datenquellen in einem Azure Virtual Network (VNET) befinden, müssen Sie die [AlwaysUseGateway](analysis-services-vnet-gateway.md)-Servereigenschaft konfigurieren.
* Das hier beschriebene (einheitliche) Gateway wird für Azure Deutschland-Regionen nicht unterstützt. Verwenden Sie das **dedizierte lokale Gateway für Azure Analysis Services**, das vom **Schnellstart** Ihres Servers installiert wurde, im Portal. 


## <a name="download"></a>Herunterladen

 [Gateway herunterladen](https://go.microsoft.com/fwlink/?LinkId=820925&clcid=0x409)

## <a name="install"></a>Installieren

1. Führen Sie das Setup aus.

2. Wählen Sie **Lokales Datengateway** aus.

   ![Select](media/analysis-services-gateway-install/aas-gateway-installer-select.png)

2. Wählen Sie einen Speicherort, akzeptieren Sie die Lizenzbedingungen, und klicken Sie dann auf **Installieren**.

   ![Installationsspeicherort und Lizenzbedingungen](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Melden Sie sich bei Azure an. Das Konto muss sich in Ihrem Azure Active Directory-Mandanten befinden. Dieses Konto wird vom Gatewayadministrator verwendet. Für das Installieren und Registrieren des Gateways werden keine Azure B2B (Gast)-Konten unterstützt.

   ![Anmelden bei Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Wenn Sie sich mit einem Domänenkonto anmelden, wird es Ihrem Unternehmenskonto in Azure AD zugeordnet. Ihr Unternehmenskonto wird vom Gatewayadministrator verwendet.

## <a name="register"></a>Registrieren

Um eine Gatewayressource in Azure zu erstellen, müssen Sie die lokale Instanz registrieren, die Sie mit dem Gateway-Clouddienst installiert haben. 

1.  Wählen Sie **Ein neues Gateway auf diesem Computer registrieren** aus.

    ![Register](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Geben Sie einen Namen und Wiederherstellungsschlüssel für Ihr Gateway ein. Standardmäßig verwendet das Gateway die Standardregion Ihres Abonnements. Wenn Sie eine andere Region auswählen müssen, wählen Sie **Region ändern** aus.

    > [!IMPORTANT]
    > Bewahren Sie Ihren Wiederherstellungsschlüssel an einem sicheren Ort auf. Der Wiederherstellungsschlüssel ist erforderlich, um ein Gateway zu übernehmen, zu migrieren oder wiederherzustellen. 

   ![Register](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Erstellen einer Azure-Gatewayressource

Nachdem Sie Ihr Gateway registriert und installiert haben, müssen Sie in Ihrem Azure-Abonnement eine Gatewayressource erstellen. Melden Sie sich bei Azure mit dem Konto an, das Sie beim Registrieren des Gateways verwendet haben.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**, suchen Sie nach **Lokales Datengateway**, und klicken Sie dann auf **Erstellen**.

   ![Erstellen einer Gatewayressource](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. Geben Sie in **Verbindungsgateway erstellen** diese Einstellungen ein:

   * **Name**: Geben Sie einen Namen für die Gatewayressource ein. 

   * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das Sie der Gatewayressource zuordnen möchten. 
   
     Das standardmäßige Abonnement basiert auf dem Azure-Konto, das Sie zum Anmelden verwendet haben.

   * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus.

   * **Standort**: Wählen Sie die Region aus, in der Sie Ihr Gateway registriert haben.

   * **Installationsname:** Wenn Ihre Gatewayinstallation nicht bereits ausgewählt ist, wählen Sie das Gateway aus, das Sie auf Ihrem Computer installiert und registriert haben. 

     Klicken Sie auf **Erstellen**, wenn Sie fertig sind.

## <a name="connect-servers"></a>Verbinden von Servern mit der Gatewayressource

1. Klicken Sie in der Übersicht über Ihre Azure Analysis Services-Server auf **Lokales Datengateway**.

   ![Verbinden des Servers mit dem Gateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. Wählen Sie in **Lokales Datengateway für die Verbindung auswählen** Ihre Gatewayressource aus, und klicken Sie dann auf **Ausgewähltes Gateway verbinden**.

   ![Verbinden des Servers mit der Gatewayressource](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Wenn Ihr Gateway nicht in der Liste nicht angezeigt wird, befindet sich Ihr Server wahrscheinlich nicht in der Region, die Sie beim Registrieren des Gateways angegeben haben.

    Wenn die Verbindung zwischen Ihrem Server und der Gatewayressource erfolgreich ist, wird der Status als **Verbunden** angezeigt.


    ![Erfolgreiche Verbindung des Servers mit der Gatewayressource](media/analysis-services-gateway-install/aas-gateway-connect-success.png)

Das ist alles. Wenn Sie Ports öffnen oder eine Problembehandlung durchführen müssen, lesen Sie den Artikel zum [lokalen Datengateway](analysis-services-gateway.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Analysis Services](analysis-services-manage.md)   
* [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md)   
* [Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz](analysis-services-vnet-gateway.md)
