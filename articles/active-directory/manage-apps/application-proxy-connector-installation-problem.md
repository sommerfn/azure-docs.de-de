---
title: Problem beim Installieren des Anwendungsproxy-Agent-Connectors | Microsoft-Dokumentation
description: Beheben von Problemen, die beim Installieren des Anwendungsproxy-Agent-Connectors auftreten können
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: a35558b81d064680981bcf403a3584e3a3d00e4f
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72311743"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Problem beim Installieren des Anwendungsproxy-Agent-Connectors

Der Microsoft AAD-Anwendungsproxyconnector ist eine interne Domänenkomponente, die ausgehende Verbindungen verwendet, um die Konnektivität zwischen dem von der Cloud zur Verfügung gestellten Endpunkt und der internen Domäne einzurichten.

## <a name="general-problem-areas-with-connector-installation"></a>Allgemeine Problembereiche bei der Installation des Connectors

Wenn bei der Installation eines Connectors ein Fehler auftritt, entstammt die Hauptursache in der Regel einem der folgenden Bereiche:

1.  **Konnektivität**: Zum Abschluss einer erfolgreichen Installation muss der neue Connector zukünftige Eigenschaften der Vertrauensstellung registrieren und einrichten. Dies erfolgt durch das Herstellen einer Verbindung mit dem Clouddienst des AAD-Anwendungsproxys.

2.  **Einrichtung einer Vertrauensstellung**: Der neue Connector erstellt ein selbstsigniertes Zertifikat und registriert es beim Clouddienst.

3.  **Authentifizierung des Administrators**: Während der Installation muss der Benutzer Anmeldeinformationen für den Administrator bereitstellen, um die Connectorinstallation abzuschließen.

> [!NOTE]
> Die Connectorinstallationsprotokolle befinden sich im Ordner „%TEMP%“ und können zusätzliche Informationen zur Ursache eines Installationsfehlers bieten.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Überprüfen der Konnektivität mit dem Cloudanwendungsproxy-Dienst und der Microsoft-Anmeldeseite

**Ziel**: Überprüfen Sie, ob der Connectorcomputer eine Verbindung mit dem Registrierungsendpunkt des AAD-Anwendungsproxys und der Microsoft-Anmeldeseite herstellen kann.

1.  Führen Sie auf dem Connectorserver einen Porttest aus, indem Sie [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) oder ein anderes Tool zum Testen von Ports verwenden, um zu überprüfen, ob die Ports 443 und 80 geöffnet sind.

2.  Wenn einer dieser Ports nicht verfügbar ist, überprüfen Sie, ob die Firewall oder der Back-End-Proxy Zugriff auf die erforderlichen Domänen und Ports hat. Weitere Informationen hierzu finden Sie unter [Vorbereiten Ihrer lokalen Umgebung](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment).

3.  Öffnen Sie einen Browser (separate Registerkarte), und wechseln Sie zur folgenden Webseite: <https://login.microsoftonline.com>. Stellen Sie sicher, dass Sie sich bei dieser Seite anmelden können.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Überprüfen der Unterstützung des Vertrauensstellungszertifikats des Anwendungsproxys durch Computer und Back-End-Komponenten

**Ziel**: Überprüfen Sie, ob Connectorcomputer, Back-End-Proxy und Firewall das Zertifikat unterstützen können, das vom Connector für die zukünftige Vertrauensstellung erstellt wurde.

>[!NOTE]
>Der Connector versucht ein SHA512-Zertifikat zu erstellen, das von TLS1.2 unterstützt wird. Wenn TLS 1.2 vom Computer oder von der Back-End-Firewall und dem Proxy nicht unterstützt wird, tritt bei der Installation ein Fehler auf.
>
>

**So lösen Sie das Problem:**

1.  Überprüfen Sie, ob der Computer TLS 1.2 unterstützt – Alle Windows-Versionen nach 2012 R2 sollten TLS 1.2 unterstützen. Wenn für Ihren Connectorcomputer eine Version von 2012 R2 oder früher verwendet wird, sollten Sie sicherstellen, dass die folgenden KBs darauf installiert sind: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>.

2.  Wenden Sie sich an Ihren Netzwerkadministrator, damit der Back-End-Proxy und die Firewall überprüft werden, damit sie SHA512 nicht für ausgehenden Datenverkehr blockieren.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Überprüfen, dass der Connector vom Administrator installiert wird

**Ziel**: Überprüfen Sie, ob der Benutzer, der versucht, den Connector zu installieren, ein Administrator mit den richtigen Anmeldeinformationen ist. Derzeit muss der Benutzer mindestens über die Rolle „Anwendungsadministrator“ verfügen, damit die Installation erfolgreich ausgeführt werden kann.

**So überprüfen Sie, ob die Anmeldeinformationen korrekt sind:**

Stellen Sie eine Verbindung mit <https://login.microsoftonline.com> her, und verwenden Sie die gleichen Anmeldeinformationen. Überprüfen Sie, ob die Anmeldung erfolgreich war. Sie können die Benutzerrolle überprüfen, indem Sie zu **Azure Active Directory** -&gt; **Benutzer und Gruppen** -&gt; **Alle Benutzer** wechseln. 

Wählen Sie Ihr Benutzerkonto aus, und klicken Sie dann im daraufhin angezeigten Menü auf „Verzeichnisrolle“. Stellen Sie sicher, dass die Rolle „Anwendungsadministrator“ ausgewählt ist. Wenn Sie in diesen Schritten nicht auf die Seiten zugreifen können, verfügen Sie nicht über die erforderliche Rolle.

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-connectors.md)
