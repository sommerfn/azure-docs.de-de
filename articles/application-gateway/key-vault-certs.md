---
title: SSL-Terminierung mit Azure Key Vault-Zertifikaten
description: Erfahren Sie, wie Sie Azure Application Gateway mit Key Vault für Serverzertifikate integrieren können, die an HTTPS-fähige Listener angefügt sind.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 76807c8ed10e30c554b6aa06ec096c830a86e36e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73571978"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>SSL-Terminierung mit Key Vault-Zertifikaten

[Azure Key Vault](../key-vault/key-vault-overview.md) ist ein als Plattform verwalteter Geheimnisspeicher, mit dem Sie Geheimnisse, Schlüssel und SSL-Zertifikate schützen können. Azure Application Gateway unterstützt die Integration mit Key Vault für Serverzertifikate, die HTTPS-fähigen Listenern zugeordnet sind. Diese Unterstützung ist auf die v2-SKU von Application Gateway beschränkt.

Die Key Vault-Integration bietet zwei Modelle für die SSL-Terminierung:

- Sie können explizit SSL-Zertifikate angeben, die an den Listener angefügt sind. Dies ist das traditionelle Modell für die Übergabe von SSL-Zertifikaten an Application Gateway für die SSL-Terminierung.
- Sie können bei der Erstellung des HTTPS-fähigen Listeners optional einen Verweis auf ein vorhandenes Key Vault-Zertifikat oder -Geheimnis angeben.

Die Application Gateway-Integration mit Key Vault bietet viele Vorteile. Zu diesen gehören:

- Höhere Sicherheit, da SSL-Zertifikate nicht direkt vom Anwendungsentwicklungsteam gehandhabt werden. Die Integration ermöglicht einem getrennten Sicherheitsteam Folgendes:
  * Einrichten von Anwendungsgateways
  * Steuern von Application Gateway-Lebenszyklen
  * Erteilen von Berichtigungen für ausgewählte Gateways, damit diese auf Zertifikate zugreifen können, die in Ihrem Schlüsseltresor gespeichert sind
- Unterstützung des Imports vorhandener Zertifikate in Ihren Schlüsseltresor Verwenden von Key Vault-APIs zum Erstellen und Verwalten neuer Zertifikate bei den vertrauenswürdigen Key Vault-Partnern
- Unterstützung für die automatische Verlängerung von Zertifikaten, die in Ihrem Schlüsseltresor gespeichert sind

Application Gateway unterstützt derzeit nur per Software überprüfte Zertifikate. Durch das Hardwaresicherheitsmodul (HSM) überprüfte Zertifikate werden nicht unterstützt. Nachdem Application Gateway für die Verwendung von Key Vault-Zertifikaten konfiguriert wurde, rufen die Instanzen das Zertifikat von Key Vault ab und installieren sie lokal für die SSL-Terminierung. Die Instanzen rufen außerdem Key Vault in 24-Stunden-Intervallen ab, um ggf. eine erneuerte Version des Zertifikats zu erhalten. Wenn ein aktualisiertes Zertifikat gefunden wird, wird das derzeit dem HTTPS-Listener zugeordnete SSL-Zertifikat automatisch rotiert.

## <a name="how-integration-works"></a>Funktionsweise der Integration

Die Application Gateway-Integration in Key Vault erfolgt über eine Konfiguration in drei Schritten:

1. **Erstellen einer benutzerseitig zugewiesenen verwalteten Identität**

   Sie erstellen eine benutzerseitig zugewiesene verwaltete Identität oder verwenden eine bereits vorhandenen, über die Application Gateway in Ihrem Namen Zertifikate von Key Vault abruft. Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md). In diesem Schritt wird eine neue Identität im Azure Active Directory-Mandanten erstellt. Die Identität wird vom Abonnement, das zum Erstellen der Identität verwendet wird, als vertrauenswürdig eingestuft.

1. **Konfigurieren Ihres Schlüsseltresors**

   Sie importieren dann entweder ein vorhandenes Zertifikat oder erstellen ein neues in Ihrem Schlüsseltresor. Das Zertifikat wird von Anwendungen verwendet, die über das Anwendungsgateway ausgeführt werden. In diesem Schritt kann auch ein Key Vault-Geheimnis verwendet werden, das als Base64-codierte PFX-Datei ohne Kennwort gespeichert ist. Die Verwendung eines Zertifikattyps wird empfohlen, da für Zertifikattypobjekte im Schlüsseltresor Funktionen für die automatische Verlängerung zur Verfügung stehen. Nach dem Erstellen eines Zertifikats oder Geheimnisses definieren Sie Zugriffsrichtlinien im Schlüsseltresor, durch die der Identität der *get*-Zugriff zum Erhalten des Geheimnisses gewährt werden kann.

1. **Konfigurieren der Application Gateway-Instanz**

   Nachdem Sie die beiden vorherigen Schritte abgeschlossen haben, können Sie ein vorhandenes Anwendungsgateway einrichten oder ändern, um die vom Benutzer zugewiesene verwaltete Identität zu verwenden. Sie können auch das SSL-Zertifikat des HTTP-Listeners so konfigurieren, dass es auf den vollständigen URI des Key Vault-Zertifikats oder -Geheimnisses zeigt.

   ![Key Vault-Zertifikate](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von SSL-Terminierung mit Key Vault-Zertifikaten mithilfe von Azure PowerShell](configure-keyvault-ps.md)
