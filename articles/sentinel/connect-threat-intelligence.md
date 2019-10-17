---
title: Verknüpfen von Threat Intelligence-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Threat Intelligence-Daten mit Azure Sentinel verknüpfen.
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: rkarlin
ms.openlocfilehash: 44b3830465bf2b5aa06612aa868b086b120f1ece
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72372272"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Verknüpfen von Daten von Threat Intelligence-Anbietern

> [!IMPORTANT]
> Der Threat Intelligence Platforms-Datenconnector in Azure Sentinel ist derzeit als Public Preview verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit Azure Sentinel können Sie die von Ihrer Organisation verwendeten Bedrohungsindikatoren importieren. Dies kann die Fähigkeit Ihrer Sicherheitsanalysten verbessern, bekannte Bedrohungen zu erkennen und zu priorisieren. Einige Features von Azure Sentinel werden dann verfügbar oder wurden verbessert:

- **Analysen** enthalten eine Reihe von geplanten Regelvorlagen, die Sie aktivieren können, um Warnungen und Incidents zu generieren, die auf Übereinstimmungen von Protokollereignissen aus Ihren Bedrohungsindikatoren basieren.

- **Arbeitsmappen** bieten zusammengefasste Informationen zu den in Azure Sentinel importierten Bedrohungsindikatoren und allen Warnungen, die von Analyseregeln generiert werden, die Ihren Bedrohungsindikatoren entsprechen.

- **Hunting**-Abfragen ermöglichen es Sicherheitsbeauftragten, Bedrohungsindikatoren im Rahmen allgemeiner Hunting-Szenarien zu verwenden.

- **Notebooks** können Bedrohungsindikatoren verwenden, wenn Sie Anomalien untersuchen und nach schädlichen Verhaltensweisen suchen.

Sie können Bedrohungsindikatoren zu Azure Sentinel streamen, indem Sie eines der im nächsten Abschnitt aufgeführten Produkte der integrierten Threat Intelligence Platform (TIP) verwenden, oder indem Sie die direkte Integration mit der [tiIndicators-API von Microsoft Graph-Sicherheit](https://aka.ms/graphsecuritytiindicators) verwenden.

## <a name="integrated-threat-intelligence-platform-products"></a>Produkte der integrierten Threat Intelligence Platform

- [MISP-Open-Source-Threat Intelligence Platform](https://www.misp-project.org/)
    
    Ein Beispielskript, das Clients mit MISP-Instanzen zur Migration von Bedrohungsindikatoren in die Microsoft Graph-Sicherheits-API bereitstellt, finden Sie unter [MISP zu Microsoft Graph-Sicherheitsskript](https://github.com/microsoftgraph/security-api-solutions/tree/master/Samples/MISP).

- [Palo Alto Networks MineMeld](https://www.paloaltonetworks.com/products/secure-the-network/subscriptions/minemeld)
    
    Anleitungen finden Sie unter [Senden von IOCs an die Microsoft Graph-Sicherheits-API mit MineMeld](https://live.paloaltonetworks.com/t5/MineMeld-Articles/Sending-IOCs-to-the-Microsoft-Graph-Security-API-using-MineMeld/ta-p/258540).

- [ThreatConnect-Plattform](https://threatconnect.com/solution/)


## <a name="prerequisites"></a>Voraussetzungen  

- Azure AD-Rolle des Unternehmensadministrators oder Sicherheitsadministrators, um Berechtigungen für Ihr TIP-Produkt oder Ihre benutzerdefinierte Anwendung zu erteilen, die die direkte Integration mit der tiIndicators-API von Microsoft Graph-Sicherheit verwendet.

- Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich, um Ihre Bedrohungsindikatoren zu speichern.

## <a name="connect-azure-sentinel-to-your-threat-intelligence-provider"></a>Verbinden von Azure Sentinel mit Ihrem Threat Intelligence-Anbieter

1. [Registrieren Sie eine Anwendung](/graph/auth-v2-service#1-register-your-app) in Azure Active Directory, um eine Anwendungs-ID, ein Anwendungsgeheimnis und eine Azure Active Directory-Mandanten-ID zu erhalten. Sie benötigen diese Werte, wenn Sie Ihr integriertes TIP-Produkt oder Ihre integrierte Anwendung konfigurieren, die eine direkte Integration mit der tiIndicators-API der Microsoft Graph-Sicherheit verwendet.

2. [Konfigurieren Sie API-Berechtigungen](/graph/auth-v2-service#2-configure-permissions-for-microsoft-graph) für die registrierte Anwendung: Fügen Sie Ihrer registrierten Anwendung die Microsoft Graph-Anwendungsberechtigung **ThreatIndicators.ReadWrite.OwnedBy** hinzu.

3. Bitten Sie Ihren Azure Active Directory-Mandantenadministrator, der registrierten Anwendung die Administratoreinwilligung für Ihre Organisation zu erteilen. Gehen Sie im Azure-Portal so vor: **Azure Active Directory** > **App-Registrierungen** >  **\<_App-Name_>**  > **API-Berechtigungen anzeigen** > **Administratoreinwilligung erteilen für \<_Mandantenname_>** .

4. Konfigurieren Sie das TIP-Produkt oder die TIP-App, das bzw. die die direkte Integration mit der tiIndicators-API der Microsoft Graph-Sicherheit verwendet, um Indikatoren an Azure Sentinel zu senden, indem Sie Folgendes angeben:
    
    a. Die Werte für die ID, das Geheimnis und die Mandanten-ID der registrierten Anwendung.
    
    b. Geben Sie für das Zielprodukt „Azure Sentinel“ an.
    
    c. Geben Sie für die Aktion „Warnung“ an.

5. Navigieren Sie im Azure-Portal zu **Azure Sentinel** > **Datenconnectors**, und wählen Sie dann den Connector **Threat Intelligence Platforms (Vorschauversion)** aus.

6. Wählen Sie **Connectorseite öffnen** und dann **Verbinden** aus.

7. Um die Bedrohungsindikatoren anzuzeigen, die in Azure Sentinel importiert werden, navigieren Sie zu **Azure Sentinel – Protokolle** > **SecurityInsights**, und erweitern Sie dann **ThreatIntelligenceIndicator**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Ihren Threat Intelligence-Anbieter mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln.

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
