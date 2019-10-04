---
title: Firewallregeln in Azure Database for PostgreSQL – Hyperscale (Citus)
description: In diesem Artikel werden die Firewallregeln für Azure Database for PostgreSQL – Hyperscale (Citus) beschrieben.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: 567fb27ed942a24ab7d031d791e18fa487956fad
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275304"
---
# <a name="firewall-rules-in-azure-database-for-postgresql---hyperscale-citus"></a>Firewallregeln in Azure Database for PostgreSQL – Hyperscale (Citus)
Die Azure Database for PostgreSQL-Serverfirewall verhindert jeglichen Zugriff auf Ihren Hyperscale (Citus)-Koordinatorknoten, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Serverzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
Zum Konfigurieren der Firewall erstellen Sie Firewallregeln, die Bereiche zulässiger IP-Adressen festlegen. Sie können Firewallregeln auf Serverebene erstellen.

**Firewallregeln**: Diese Regeln ermöglichen es Clients, auf Ihren Hyperscale (Citus)-Koordinatorknoten zuzugreifen (also auf alle Datenbanken innerhalb desselben logischen Servers). Firewallregeln auf Serverebene können über das Azure-Portal konfiguriert werden. Zum Erstellen von Firewallregeln auf Serverebene müssen Sie der Besitzer oder ein Mitwirkender des Abonnements sein.

## <a name="firewall-overview"></a>Firewallübersicht
Jeglicher Datenbankzugriff auf den Koordinatorknoten wird standardmäßig durch die Firewall blockiert. Damit Sie Ihren Server über einen anderen Computer verwenden können, müssen Sie eine oder mehrere Firewallregeln auf Serverebene angeben, die Zugriff auf Ihren Server ermöglichen. Legen Sie mithilfe der Firewallregeln fest, welche IP-Adressbereiche aus dem Internet zugelassen werden sollen. Der Zugriff auf die Website des Azure-Portals selbst wird durch die Firewallregeln nicht beeinträchtigt.
Verbindungsversuche über das Internet und über Azure müssen zunächst die Firewall passieren, bevor sie Ihre PostgreSQL-Datenbank erreichen (wie im folgenden Diagramm dargestellt):

![Beispielfluss zur Funktionsweise der Firewall](media/concepts-hyperscale-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet-and-from-azure"></a>Herstellen einer Verbindung über das Internet und über Azure

Die Firewall einer Hyperscale (Citus)-Servergruppe steuert, wer eine Verbindung mit dem Koordinatorknoten der Gruppe herstellen darf. Die Firewall bestimmt den Zugriff anhand einer konfigurierbaren Regelliste. Jede Regel entspricht einer einzelnen oder einem Bereich von IP-Adressen, über die der Zugriff zulässig ist.

Wenn die Firewall Verbindungen blockiert, können Anwendungsfehler auftreten. Die Verwendung des PostgreSQL-JDBC-Treibers kann einen mit folgendem vergleichbaren Fehler auslösen:

> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: KRITISCH: kein pg\_hba.conf-Eintrag für Host „123.45.67.890“, Benutzer „citus“, Datenbank „citus“, SSL

Unter [Erstellen und Verwalten von Firewallregeln](howto-hyperscale-manage-firewall-using-portal.md) erfahren Sie, wie die Regeln definiert werden.

## <a name="troubleshooting-the-database-server-firewall"></a>Problembehandlung der Datenbankserverfirewall
Wenn der Zugriff auf den Microsoft Azure Database for PostgreSQL – Hyperscale (Citus)-Dienst nicht das erwartete Verhalten aufweist, sollten Sie Folgendes beachten:

* **Änderungen an der Zulassungsliste sind noch nicht wirksam**: Änderungen der Firewallkonfiguration für Hyperscale (Citus) werden möglicherweise erst nach fünf Minuten wirksam.

* **Der Benutzer ist nicht autorisiert, oder es wurde ein falsches Kennwort verwendet**: Wenn ein Benutzer keine Berechtigungen für den Server hat oder das verwendete Kennwort falsch ist, wird die Verbindung mit dem Server verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, eine Verbindung mit dem Server herzustellen. Jeder Client muss weiterhin die erforderlichen Sicherheitsanmeldeinformationen bereitstellen.

Beispielsweise kann bei der Verwendung eines JDBC-Clients folgende Fehlermeldung angezeigt werden.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: KRITISCH: Fehler bei der Kennwortauthentifizierung für Benutzer „IhrBenutzername“

* **Dynamische IP-Adresse**: Wenn Sie über eine Internetverbindung mit dynamischer IP-Adressierung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

* Fragen Sie Ihren Internetdienstanbieter (ISP) nach dem IP-Adressbereich, der den Clientcomputern zugewiesen ist, mit denen auf den Hyperscale (Citus)-Koordinatorknoten zugegriffen wird, und fügen Sie dann den IP-Adressbereich als eine Firewallregel hinzu.

* Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die statische IP-Adresse als Firewallregel hinzu.

## <a name="next-steps"></a>Nächste Schritte
Artikel zum Erstellen von Firewallregeln auf Serverebene und auf Datenbankebene finden Sie hier:
* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](howto-hyperscale-manage-firewall-using-portal.md)
