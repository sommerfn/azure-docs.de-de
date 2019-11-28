---
title: Was ist Blockchain Data Manager für Azure Blockchain Service?
description: Blockchain Data Manager erfasst, transformiert und stellt Blockchaindaten für Event Grid-Themen bereit.
ms.date: 11/04/2019
ms.topic: overview
ms.reviewer: chroyal
ms.openlocfilehash: 6583a99ef6efaefa732178d0228708cea82365b1
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326202"
---
# <a name="what-is-blockchain-data-manager-for-azure-blockchain-service"></a>Was ist Blockchain Data Manager für Azure Blockchain Service?

Blockchain Data Manager erfasst, transformiert und stellt Azure Blockchain Service-Transaktionsdaten für Azure Event Grid-Themen bereit, die eine zuverlässige und skalierbare Blockchain-Ledger-Integration in Azure-Dienste ermöglichen.

In den meisten Blockchainszenarien für Unternehmen ist ein Blockchain-Ledger ein Teil einer Lösung. Wenn Sie z. B. ein Objekt zwischen zwei Entitäten übertragen möchten, benötigen Sie einen Mechanismus zum Übermitteln der Transaktion. Anschließend benötigen Sie einen Mechanismus zum Lesen von Ledger-Daten, um sicherzustellen, dass die Transaktion erfolgt ist, akzeptiert wurde und die resultierenden Zustandsänderungen in die End-to-End-Lösung integriert wurden. Wenn Sie in diesem Beispiel einen Smart Contract zur Übertragung von Objekten programmieren, können Sie Blockchain Data Manager verwenden, um externe Anwendungen und Datenspeicher zu integrieren. Wenn ein Objekt (wie beim Beispiel für die Objektübertragung) an die Blockchain übertragen wird, werden Änderungen des Ereignis- und Eigenschaftsstatus von Blockchain Data Manager über Event Grid bereitgestellt. Sie können dann mehrere mögliche Ereignishandler für Event Grid verwenden, um die Blockchaindaten extern zu speichern oder auf Zustandsänderungen in Echtzeit zu reagieren.

Blockchain Data Manager führt drei Hauptfunktionen aus: Erfassung, Transformation und Bereitstellung.

![Blockchain Data Manager-Funktionen](./media/data-manager/functions.png)

## <a name="capture"></a>Erfassen

Jede Blockchain Data Manager-Instanz stellt eine Verbindung mit dem Transaktionsknoten eines Azure Blockchain Service-Mitglieds her. Nur Benutzer, die Zugriff auf den Transaktionsknoten haben, können eine Verbindung herstellen. So wird die korrekte Zugriffssteuerung für Kundendaten sichergestellt. Eine Blockchain Data Manager-Instanz erfasst zuverlässig alle Block- und Transaktionsrohdaten aus dem Transaktionsknoten und kann skaliert werden, um Unternehmensworkloads zu unterstützen.

## <a name="transform"></a>Transformieren

Mithilfe von Blockchain Data Manager können Sie den Ereignis-und Eigenschaftsstatus decodieren, indem Sie Smart Contract-Anwendungen innerhalb von Blockchain Data Manager konfigurieren. Um einen Smart Contract hinzuzufügen, stellen Sie Vertrags-ABI und Bytecode bereit. Blockchain Data Manager verwendet die Smart Contract-Artefakte zum Decodieren und Ermitteln von Vertragsadressen. Nachdem die Blockchainanwendung der Instanz hinzugefügt wurde, ermittelt Blockchain Data Manager dynamisch die Smart Contract-Adresse, wenn der Smart Contract im Konsortium bereitgestellt wird, und sendet den decodierten Ereignis- und Eigenschaftsstatus an konfigurierte Ziele.

## <a name="deliver"></a>Bereitstellen

Blockchain Data Manager unterstützt ausgehende Verbindungen mit mehreren Event Grid-Themen für eine beliebige Blockchain Data Manager-Instanz. Sie können Blockchaindaten an ein einzelnes Ziel oder an mehrere Ziele senden. Mithilfe von Blockchain Data Manager können Sie eine skalierbare ereignisbasierte Datenveröffentlichungslösung für jede Blockchainbereitstellung entwickeln.

## <a name="configuration-options"></a>Konfigurationsoptionen

Sie können Blockchain Data Manager für die Anforderungen Ihrer Lösung konfigurieren. Sie können z. B. Folgendes bereitstellen:

* Eine Blockchain Data Manager-Instanz für ein Azure Blockchain Service-Mitglied
* Eine Blockchain Data Manager-Instanz pro Azure Blockchain Service-Transaktionsknoten Private Transaktionsknoten können z. B. über eine eigene Blockchain Data Manager-Instanz verfügen, um Vertraulichkeit zu gewährleisten.
* Eine Blockchain Data Manager-Instanz kann mehrere Ausgabeverbindungen unterstützen. Über eine Blockchain Data Manager-Instanz können alle Integrationspunkte zur Datenveröffentlichung für ein Azure Blockchain Service-Mitglied verwaltet werden.

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, [eine Blockchain Data Manager-Instanz für ein Azure Blockchain Service-Mitglied zu erstellen](data-manager-portal.md).
